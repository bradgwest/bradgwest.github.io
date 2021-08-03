---
title: "A Functional REST API Client in Python"
---

The other day I came across Moseley and Marks' 2006 essay on software
complexity,
[_Out of the Tar Pit_](http://curtclifton.net/papers/MoseleyMarks06a.pdf). It's a
great read, and easily digestible[^short]. Moseley and Marks are direct, wasting
no time in stating why software is so complex:

> We believe that the major contributor to this complexity in many systems is
  the handling of _state_ and the burden that this adds when trying to analyse
  and reason about the system.

[^short]: Although a bit long at 66 pages. [The Morning Paper](https://blog.acolyer.org/2015/03/20/out-of-the-tar-pit/)
covered it if you're looking for a slightly shorter version.

Having read _Out of the Tar Pit_ on Thursday evening, I came to work Friday
morning with stateless systems fresh in the mind. As fate had it, I needed to
hit a [Redash](https://github.com/getredash/redash) endpoint to smoke test our
self-hosted solution. And so, in the quiet of the Friday afternoon[^work] I
decided to mess around and implement a stateless REST client.

[^work]: Honestly, one of the best times of the week to get anything done.

It's pretty common for a REST client to look like this:

```py
import requests

class Client:
    def __init__(self, host: str, token: str):
        self._host = host
        self._session = requests.Session()
        self._session.headers.update(
            {"Authorization": f"Bearer {token}"}
        )

    def _request(
        self, method: str, endpoint: str, **kwargs
    ) -> request.Response:
        url = self._host + endpoint
        response = self._session.request(method, url, **kwargs)
        response.raise_for_status()
        return response

    # endpoints
    def post_something(self, something_id, **kwargs) -> dict:
        return self._request(
            "POST", f"/api/somethings/{something_id}", **kwargs
        )

    def get_something(self, something_id, **kwargs) -> dict:
        return self._request(
            "POST", f"/api/somethings/{something_id}", **kwargs
        )
```

I'm sure there are ways to make it better, but overall that's fairly simple. The
client, however, is an object, which means it needs
to be passed around throughout the application, and that object contains a
session object. That's not very functional.

I work as a data engineer and have limited experience with Flask. But I know it
offers some convenience `@app.route` decorator to define which url should tigger
which logic:

```py
from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

I figured we could do a similar thing for an API client, and use a decorator
to specify functions that should process results returned from a given REST
endpoint. Here's a minimal API:

```py
# client.py
from collections import NamedTuple

from utils import endpoint

class ConnInfo(NamedTuple):
    host: str
    token: str

@endpoint("GET", "/api/somethings/{something_id}")
def get_something(response, *args, **kwargs) -> dict:
    return response.json()  # or any processing logic you like
```

A user can leverage this API like so:

```py
# main.py
import os

import client

conn_info = client.ConnInfo(
    os.environ["API_HOST"],
    os.environ["API_TOKEN"]
)
my_something = get_something(conn_info, something_id=1)
print(my_something)
# {"id": 1}
```

The nice this about this API is that it's purely functional. You pass around
an immutable `ConnInfo` tuple, that's all. No need for a long existing client
with a session with a state that can be unintentionally, or intentionally,
modified. It's also extremely simple to add a new endpoint method, you simply
need to write a function with the `f(response, *args, **kwargs)` signature,
and decorate it with the `endpoint` decorator, passing the HTTP request method
and endpoint to target.

`endpoint` itself is a little bit trickier:

```py
# utils.py
import re
from functools import partial, wraps

import requests


_ROUTE_PARAM = re.compile(r"((\w+))")


def _default_request(method, route, conn_info, **kwargs):
    url = conn_info.host + route
    headers = {"Authorization": f"Bearer {conn_info.token}"}
    headers.update(kwargs.pop("headers", {}))
    response = requests.request(method, url, headers=headers, **kwargs)
    response.raise_for_status()
    return response


def _parse_route_params(route: str):
    return frozenset(re.findall(_ROUTE_PARAM), route)


def _validate_args(required_params, **kwargs):
    missing = required_params - set(**kwargs.keys())
    if missing:
        raise ValueError(
            f"Missing required arguments in request function: {missing}"
        )


def endpoint(method: str, route: str):
    def _endpoint(func):
        @wraps
        def inner(*args, **kwargs):
            required_params = _parse_route_params(route)
            validate_args(required_params, **kwargs)
            ep = route.format(**kwargs)

            def request(conn_info, *args, **kwargs):
                requester = kwargs.get(
                    "requester", partial(_default_request, method, ep)
                )
                request_kwargs = kwargs.get("request_kwargs", {})
                result = requester(conn_info, **request_kwargs)
                return func(result, *args, **kwargs)

            return request(*args, **kwargs)

        return inner

    return _endpoint
```

That's a lot, and it's fairly complicated. In order for the `endpoint` decorator
to take parameters we wrap the actual decorator (`_endpoint`) in a function
with those two parameters. `_endpoint`, the real decorator, wraps the `inner`
function, which first validates that the user passed in necessary keyword
arguments, like `something_id=1` when calling a function decorated with
`@endpoint("GET", "/api/somethings/{something_id}")`. `inner` then builds
the actual requesting function, `request`. If the user has specified a
`requester` keyword argument, then that is the function that will be used
to fetch data. This is extremely useful for testing. Rather than invoking a
network call, a user can simply pass in a dummy function, say
`requester=lambda _: {"id": 1}`. No mocking required. Sure, there's
indirection (the user needs to know that `requester` is the keyword), but
:shrug:. Finally, `request` passes kwargs to the data fetching function, calls
it, and passes the result to the processing function, which is the function
the user defined, like `get_something()`. `inner` is what actually gets
returned at import time, post decoration, and is what gets run.

I think there are some good things about this design: testing is simple,
new api definitions are simple, and there's no state at all. However, the
code needed to implement this pattern is not simple. Too many nested functions
as it stands right now, but more importantly, it's not immediately clear to
the code reader what is happening with their decorator. I don't like this.
Code readers should know exactly what's happening when they decorate something
and this method relies a bit too much on the processing function having a
certain interface.

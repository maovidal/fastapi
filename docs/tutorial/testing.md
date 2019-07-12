Thanks to <a href="https://www.starlette.io/testclient/" target="_blank">Starlette's TestClient</a>, testing **FastAPI** applications is easy and enjoyable.

It is based on <a href="http://docs.python-requests.org" target="_blank">Requests</a>, so it's very familiar and intuitive.

With it, you can use <a href="https://docs.pytest.org/" target="_blank">pytest</a> directly with **FastAPI**.

## Using `TestClient`

Import `TestClient` from `starlette.testclient`.

Create a `TestClient` passing to it your **FastAPI**.

Create functions with a name that starts with `test_` (this is standard `pytest` conventions).

Use the `TestClient` object the same way as you do with `requests`.

Write simple `assert` statements with the standard Python expressions that you need to check (again, standard `pytest`).

```Python hl_lines="2 12 15 16 17 18"
{!./src/app_testing/tutorial001.py!}
```

!!! tip
    Notice that the testing functions are normal `def`, not `async def`. 

    And the calls to the client are also normal calls, not using `await`.

    This allows you to use `pytest` directly without complications.

## Separating tests

In a real application, you probably would have your tests in a different file.

And your **FastAPI** application might also be composed of several files/modules, etc.

### **FastAPI** app file

Let's say you have a file `main.py` with your **FastAPI** app:

```Python
{!./src/app_testing/main.py!}
```

### Testing file

Then you could have a file `test_main.py` with your tests, and import your `app` from the `main` module (`main.py`):

```Python
{!./src/app_testing/test_main.py!}
```

## Testing: extended example

Now let's extend this example and add more details to see how to test different parts.

### Extended **FastAPI** app file

Let's say you have a file `main_b.py` with your **FastAPI** app.

It has a `GET` operation that could return an error.

It has a `POST` operation that could return several errors.

Both *path operations* require an `X-Token` header.

```Python
{!./src/app_testing/main_b.py!}
```

### Extended testing file

You could then have a `test_main_b.py`, the same as before, with the extended tests:

```Python
{!./src/app_testing/test_main_b.py!}
```

Whenever you need the client to pass information in the request and you don't know how to, you can search (Google) how to do it in `requests`.

Then you just do the same in your tests.

E.g.:

* To pass a *path* or *query* parameter, add it to the URL itself.
* To pass a JSON body, pass a Python object (e.g. a `dict`) to the parameter `json`.
* If you need to send *Form Data* instead of JSON, use the `data` parameter instead.
* To pass *headers*, use a `dict` in the `headers` parameter.
* For *cookies*, a `dict` in the `cookies` parameter.

For more information about how to pass data to the backend (using `requests` or the `TestClient`) check the <a href="http://docs.python-requests.org" target="_blank">Requests documentation</a>.

!!! info
    Note that the `TestClient` receives data that can be converted to JSON, not Pydantic models.

    If you have a Pydantic model in your test and you want to send its data to the application during testing, you can use the <a href="https://fastapi.tiangolo.com/tutorial/encoder/" target="_blank">JSON compatible encoder: `jsonable_encoder`</a>.

## Testing WebSockets

You can use the same `TestClient` to test WebSockets.

For this, you use the `TestClient` in a `with` statement, connecting to the WebSocket:

```Python hl_lines="27 28 29 30 31"
{!./src/app_testing/tutorial002.py!}
```

## Testing Events, `startup` and `shutdown`

When you need your event handlers (`startup` and `shutdown`) to run in your tests, you can use the `TestClient` with a `with` statement:

```Python hl_lines="9 10 11 12 20 21 22 23 24"
{!./src/app_testing/tutorial003.py!}
```

## Run it

After that, you just need to install `pytest`:

```bash
pip install pytest
```

It will detect the files and tests automatically, execute them, and report the results back to you.

Run the tests with:

```bash
pytest
```
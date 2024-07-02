---
title: Getting Started with the Validate Call Decorator
---
The <SwmToken path="/pydantic/validate_call_decorator.py" pos="19:2:2" line-data="def validate_call(">`validate_call`</SwmToken> decorator is a feature of Pydantic that provides runtime validation for function calls. It can be used as a plain decorator `@validate_call` or with arguments `@validate_call(...)`. The decorator wraps around a function and validates the arguments and, optionally, the return value. It takes two optional parameters: <SwmToken path="/pydantic/validate_call_decorator.py" pos="20:3:3" line-data="    *, config: ConfigDict | None = None, validate_return: bool = False">`config`</SwmToken>, which is a configuration dictionary, and <SwmToken path="/pydantic/validate_call_decorator.py" pos="20:17:17" line-data="    *, config: ConfigDict | None = None, validate_return: bool = False">`validate_return`</SwmToken>, a boolean that determines whether to validate the return value.

<SwmSnippet path="/pydantic/validate_call_decorator.py" line="18">

---

## Usage of Validate Call Decorator

Here is an example of how the <SwmToken path="/pydantic/validate_call_decorator.py" pos="19:2:2" line-data="def validate_call(">`validate_call`</SwmToken> decorator is defined. The <SwmToken path="/pydantic/validate_call_decorator.py" pos="20:3:3" line-data="    *, config: ConfigDict | None = None, validate_return: bool = False">`config`</SwmToken> argument is used to pass a configuration dictionary, and the <SwmToken path="/pydantic/validate_call_decorator.py" pos="20:17:17" line-data="    *, config: ConfigDict | None = None, validate_return: bool = False">`validate_return`</SwmToken> argument is used to specify whether the return value of the function should be validated.

```python
@overload
def validate_call(
    *, config: ConfigDict | None = None, validate_return: bool = False
) -> Callable[[AnyCallableT], AnyCallableT]: ...
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/validate_call_decorator.py" line="50">

---

## Validate Call Decorator in Action

This is the <SwmToken path="/pydantic/validate_call_decorator.py" pos="50:3:3" line-data="    def validate(function: AnyCallableT) -&gt; AnyCallableT:">`validate`</SwmToken> function which is used inside the <SwmToken path="/pydantic/validate_call_decorator.py" pos="53:26:26" line-data="            raise TypeError(f&#39;The `@{name}` decorator should be applied after `@validate_call` (put `@{name}` on top)&#39;)">`validate_call`</SwmToken> decorator. It checks if the function is a classmethod or staticmethod, and if so, raises a <SwmToken path="/pydantic/validate_call_decorator.py" pos="53:3:3" line-data="            raise TypeError(f&#39;The `@{name}` decorator should be applied after `@validate_call` (put `@{name}` on top)&#39;)">`TypeError`</SwmToken>. Otherwise, it creates a <SwmToken path="/pydantic/validate_call_decorator.py" pos="54:7:7" line-data="        validate_call_wrapper = _validate_call.ValidateCallWrapper(function, config, validate_return)">`ValidateCallWrapper`</SwmToken> for the function and returns a wrapper function that calls the <SwmToken path="/pydantic/validate_call_decorator.py" pos="54:7:7" line-data="        validate_call_wrapper = _validate_call.ValidateCallWrapper(function, config, validate_return)">`ValidateCallWrapper`</SwmToken> when invoked.

```python
    def validate(function: AnyCallableT) -> AnyCallableT:
        if isinstance(function, (classmethod, staticmethod)):
            name = type(function).__name__
            raise TypeError(f'The `@{name}` decorator should be applied after `@validate_call` (put `@{name}` on top)')
        validate_call_wrapper = _validate_call.ValidateCallWrapper(function, config, validate_return)

        @functools.wraps(function)
        def wrapper_function(*args, **kwargs):
            return validate_call_wrapper(*args, **kwargs)

        wrapper_function.raw_function = function  # type: ignore

        return wrapper_function  # type: ignore
```

---

</SwmSnippet>

## Validate Call Decorator with Async Methods

The <SwmToken path="/pydantic/validate_call_decorator.py" pos="19:2:2" line-data="def validate_call(">`validate_call`</SwmToken> decorator can also be used with async methods, as mentioned in the history of the project (see [#7046](https://github.com/pydantic/pydantic/pull/7046)).

# Validate Call Decorator

Understanding Validate Call Decorator

<SwmSnippet path="/pydantic/validate_call_decorator.py" line="18">

---

## Validate Call Decorator

The <SwmToken path="/pydantic/validate_call_decorator.py" pos="19:2:2" line-data="def validate_call(">`validate_call`</SwmToken> function is a decorator used for validating function calls. It can be used either as a plain decorator `@validate_call` or with arguments `@validate_call(...)`. The function to be decorated is passed as an argument, along with an optional configuration dictionary and a boolean flag to indicate whether to validate the return value. The decorator returns a wrapper around the function that validates the arguments and, optionally, the return value.

```python
@overload
def validate_call(
    *, config: ConfigDict | None = None, validate_return: bool = False
) -> Callable[[AnyCallableT], AnyCallableT]: ...


@overload
def validate_call(func: AnyCallableT, /) -> AnyCallableT: ...


def validate_call(
    func: AnyCallableT | None = None,
    /,
    *,
    config: ConfigDict | None = None,
    validate_return: bool = False,
) -> AnyCallableT | Callable[[AnyCallableT], AnyCallableT]:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/validation_decorator/

    Returns a decorated wrapper around the function that validates the arguments and, optionally, the return value.

```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

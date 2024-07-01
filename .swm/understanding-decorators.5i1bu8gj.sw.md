---
title: Understanding Decorators
---
Decorators in Python are a powerful tool that allows us to wrap a function or method in order to extend or modify its behavior. In the context of the DEMO-pydantic repository, decorators are used extensively for various purposes such as validation of function arguments and deprecation warnings. For instance, the `validate_arguments` decorator is used to validate the arguments passed to a function. It's defined with the `@overload` decorator to allow different configurations of arguments. Another example is the `deprecated` decorator, which is used to mark certain methods as deprecated and issue a warning when they are used.

<SwmSnippet path="/pydantic/deprecated/decorator.py" line="38">

---

# Using the `@deprecated` Decorator

Here, the `@deprecated` decorator is used to indicate that the `validate_arguments` function is deprecated. When this function is called, a warning message is displayed, suggesting to use the `validate_call` function instead.

```python
@deprecated(
    'The `validate_arguments` method is deprecated; use `validate_call` instead.',
    category=None,
)
def validate_arguments(func: Optional['AnyCallableT'] = None, *, config: 'ConfigType' = None) -> Any:
    """Decorator to validate the arguments passed to a function."""
    warnings.warn(
        'The `validate_arguments` method is deprecated; use `validate_call` instead.',
        PydanticDeprecatedSince20,
        stacklevel=2,
    )
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/decorator.py" line="53">

---

# Using the `@wraps` Decorator

In this snippet, the `@wraps` decorator is used in the `wrapper_function`. This decorator preserves the metadata of the original function (`_func`), such as its name and docstring, when it's being wrapped by another function (`wrapper_function`).

```python
        @wraps(_func)
        def wrapper_function(*args: Any, **kwargs: Any) -> Any:
            return vd.call(*args, **kwargs)

        wrapper_function.vd = vd  # type: ignore
        wrapper_function.validate = vd.init_model_instance  # type: ignore
        wrapper_function.raw_function = vd.raw_function  # type: ignore
        wrapper_function.model = vd.model  # type: ignore
        return wrapper_function
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/decorator.py" line="28">

---

# Using the `@overload` Decorator

The `@overload` decorator is used to provide multiple ways to call the `validate_arguments` function with different types of arguments. This helps improve the readability of the code and provides better type checking.

```python
@overload
def validate_arguments(
    func: None = None, *, config: 'ConfigType' = None
) -> Callable[['AnyCallableT'], 'AnyCallableT']: ...
```

---

</SwmSnippet>

# Decorator Functions

This section will cover the main functions in the decorator module of the Pydantic library.

<SwmSnippet path="/pydantic/deprecated/decorator.py" line="42">

---

## validate_arguments

The `validate_arguments` function is a decorator that validates the arguments passed to a function. It is overloaded to handle different types of input. If the function is deprecated, a warning is issued. The function being decorated is passed to the `validate` function, which creates a `ValidatedFunction` instance for it. The `validate` function then returns a wrapper function that calls the `ValidatedFunction` instance's `call` method.

```python
def validate_arguments(func: Optional['AnyCallableT'] = None, *, config: 'ConfigType' = None) -> Any:
    """Decorator to validate the arguments passed to a function."""
    warnings.warn(
        'The `validate_arguments` method is deprecated; use `validate_call` instead.',
        PydanticDeprecatedSince20,
        stacklevel=2,
    )

    def validate(_func: 'AnyCallable') -> 'AnyCallable':
        vd = ValidatedFunction(_func, config)

        @wraps(_func)
        def wrapper_function(*args: Any, **kwargs: Any) -> Any:
            return vd.call(*args, **kwargs)

        wrapper_function.vd = vd  # type: ignore
        wrapper_function.validate = vd.init_model_instance  # type: ignore
        wrapper_function.raw_function = vd.raw_function  # type: ignore
        wrapper_function.model = vd.model  # type: ignore
        return wrapper_function

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/decorator.py" line="75">

---

## ValidatedFunction

The `ValidatedFunction` class is used to create a model for the function being decorated. It includes methods for building values, executing the function, and creating the model. The `__init__` method initializes the instance with the function and its configuration. The `build_values` method builds a dictionary of values from the arguments and keyword arguments passed to the function. The `execute` method executes the function with the built values. The `create_model` method creates a Pydantic model for the function.

```python
class ValidatedFunction:
    def __init__(self, function: 'AnyCallable', config: 'ConfigType'):
        from inspect import Parameter, signature

        parameters: Mapping[str, Parameter] = signature(function).parameters

        if parameters.keys() & {ALT_V_ARGS, ALT_V_KWARGS, V_POSITIONAL_ONLY_NAME, V_DUPLICATE_KWARGS}:
            raise PydanticUserError(
                f'"{ALT_V_ARGS}", "{ALT_V_KWARGS}", "{V_POSITIONAL_ONLY_NAME}" and "{V_DUPLICATE_KWARGS}" '
                f'are not permitted as argument names when using the "{validate_arguments.__name__}" decorator',
                code=None,
            )

        self.raw_function = function
        self.arg_mapping: Dict[int, str] = {}
        self.positional_only_args: set[str] = set()
        self.v_args_name = 'args'
        self.v_kwargs_name = 'kwargs'

        type_hints = _typing_extra.get_type_hints(function, include_extras=True)
        takes_args = False
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

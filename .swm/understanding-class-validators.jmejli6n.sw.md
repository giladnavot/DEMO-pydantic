---
title: Understanding Class Validators
---
Class Validators in Pydantic are methods decorated with `@validator` or `@root_validator` that are used to validate and perhaps modify data either before or after standard model parsing/validation is performed. They are defined within the model class and can be used to validate fields or the entire model. The `@validator` decorator is used to validate individual fields, while the `@root_validator` is used to validate the entire model. These validators can be configured to run before or after the standard validators, validate individual elements of complex objects, and even run when the value is missing.

<SwmSnippet path="/pydantic/deprecated/class_validators.py" line="82">

---

# Validator Function

The `validator` function is a decorator used to indicate that a method should be used to validate fields. It takes as arguments the name of the field to be validated, and optionally other fields, and several flags that control its behavior. The decorated method should take as arguments the class and the value of the field, and return the validated value.

```python
def validator(
    __field: str,
    *fields: str,
    pre: bool = False,
    each_item: bool = False,
    always: bool = False,
    check_fields: bool | None = None,
    allow_reuse: bool = False,
) -> Callable[[_V1ValidatorType], _V1ValidatorType]:
    """Decorate methods on the class indicating that they should be used to validate fields.

    Args:
        __field (str): The first field the validator should be called on; this is separate
            from `fields` to ensure an error is raised if you don't pass at least one.
        *fields (str): Additional field(s) the validator should be called on.
        pre (bool, optional): Whether this validator should be called before the standard
            validators (else after). Defaults to False.
        each_item (bool, optional): For complex objects (sets, lists etc.) whether to validate
            individual elements rather than the whole object. Defaults to False.
        always (bool, optional): Whether this method and other validators should be called even if
            the value is missing. Defaults to False.
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/class_validators.py" line="159">

---

# Root Validator Function

The `root_validator` function is a decorator used to indicate that a method should be used to validate the entire model. It takes as arguments several flags that control its behavior. The decorated method should take as arguments the class and a dictionary of field values, and return a dictionary of validated field values.

```python
def root_validator(
    *,
    # if you don't specify `pre` the default is `pre=False`
    # which means you need to specify `skip_on_failure=True`
    skip_on_failure: Literal[True],
    allow_reuse: bool = ...,
) -> Callable[
    [_V1RootValidatorFunctionType],
    _V1RootValidatorFunctionType,
]: ...


@overload
def root_validator(
    *,
    # if you specify `pre=True` then you don't need to specify
    # `skip_on_failure`, in fact it is not allowed as an argument!
    pre: Literal[True],
    allow_reuse: bool = ...,
) -> Callable[
    [_V1RootValidatorFunctionType],
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/class_validators.py" line="19">

---

# Validator Protocols

These protocols define the expected signatures of validator methods. They are used in the type hints of the `validator` and `root_validator` functions to ensure that the decorated methods have the correct signatures.

```python
if TYPE_CHECKING:

    class _OnlyValueValidatorClsMethod(Protocol):
        def __call__(self, __cls: Any, __value: Any) -> Any: ...

    class _V1ValidatorWithValuesClsMethod(Protocol):
        def __call__(self, __cls: Any, __value: Any, values: dict[str, Any]) -> Any: ...

    class _V1ValidatorWithValuesKwOnlyClsMethod(Protocol):
        def __call__(self, __cls: Any, __value: Any, *, values: dict[str, Any]) -> Any: ...

    class _V1ValidatorWithKwargsClsMethod(Protocol):
        def __call__(self, __cls: Any, **kwargs: Any) -> Any: ...

    class _V1ValidatorWithValuesAndKwargsClsMethod(Protocol):
        def __call__(self, __cls: Any, values: dict[str, Any], **kwargs: Any) -> Any: ...

    class _V1RootValidatorClsMethod(Protocol):
        def __call__(
            self, __cls: Any, __values: _decorators_v1.RootValidatorValues
        ) -> _decorators_v1.RootValidatorValues: ...
```

---

</SwmSnippet>

# Class Validators Functions

This section covers the main functions of class validators in Pydantic.

<SwmSnippet path="/pydantic/deprecated/class_validators.py" line="82">

---

## Validator Function

The `validator` function is a decorator used to indicate that the decorated methods should be used to validate fields. It takes several parameters such as the field to be validated, whether the validation should be performed before or after the standard validators, whether to validate individual elements of complex objects, and whether to call the method even if the value is missing. It returns a decorator that can be used to decorate a function to be used as a validator.

```python
def validator(
    __field: str,
    *fields: str,
    pre: bool = False,
    each_item: bool = False,
    always: bool = False,
    check_fields: bool | None = None,
    allow_reuse: bool = False,
) -> Callable[[_V1ValidatorType], _V1ValidatorType]:
    """Decorate methods on the class indicating that they should be used to validate fields.

    Args:
        __field (str): The first field the validator should be called on; this is separate
            from `fields` to ensure an error is raised if you don't pass at least one.
        *fields (str): Additional field(s) the validator should be called on.
        pre (bool, optional): Whether this validator should be called before the standard
            validators (else after). Defaults to False.
        each_item (bool, optional): For complex objects (sets, lists etc.) whether to validate
            individual elements rather than the whole object. Defaults to False.
        always (bool, optional): Whether this method and other validators should be called even if
            the value is missing. Defaults to False.
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/class_validators.py" line="204">

---

## Root Validator Function

The `root_validator` function is a decorator used to indicate that the decorated methods should be used to validate and possibly modify data either before or after standard model parsing/validation is performed. It takes several parameters such as whether the validation should be performed before the standard validators, whether to stop validation and return as soon as a failure is encountered, and whether to track and raise an error if another validator refers to the decorated function. It returns a decorator that can be used to decorate a function to be used as a root validator.

```python
def root_validator(
    *__args,
    pre: bool = False,
    skip_on_failure: bool = False,
    allow_reuse: bool = False,
) -> Any:
    """Decorate methods on a model indicating that they should be used to validate (and perhaps
    modify) data either before or after standard model parsing/validation is performed.

    Args:
        pre (bool, optional): Whether this validator should be called before the standard
            validators (else after). Defaults to False.
        skip_on_failure (bool, optional): Whether to stop validation and return as soon as a
            failure is encountered. Defaults to False.
        allow_reuse (bool, optional): Whether to track and raise an error if another validator
            refers to the decorated function. Defaults to False.

    Returns:
        Any: A decorator that can be used to decorate a function to be used as a root_validator.
    """
    warn(
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

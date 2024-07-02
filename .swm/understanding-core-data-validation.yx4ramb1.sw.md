---
title: Understanding Core Data Validation
---
Data Validation - Core in the DEMO-pydantic repository refers to the core functionality of the Pydantic library, which is data validation. Pydantic uses Python type hints to validate data. This is done through various functions and methods in the codebase, such as the <SwmToken path="/pydantic/main.py" pos="182:3:3" line-data="    def __init__(self, /, **data: Any) -&gt; None:  # type: ignore">`__init__`</SwmToken> method in the <SwmToken path="/pydantic/main.py" pos="54:14:14" line-data="ModelT = TypeVar(&#39;ModelT&#39;, bound=&#39;BaseModel&#39;)">`BaseModel`</SwmToken> class, which parses and validates input data to create a new model instance. Other functions like <SwmToken path="/pydantic/main.py" pos="1269:7:7" line-data="        copied = self.model_validate(data)">`model_validate`</SwmToken>, <SwmToken path="/pydantic/main.py" pos="599:3:3" line-data="    def model_validate_strings(">`model_validate_strings`</SwmToken>, and <SwmToken path="/pydantic/main.py" pos="192:5:5" line-data="        self.__pydantic_validator__.validate_python(data, self_instance=self)">`validate_python`</SwmToken> are also part of this core data validation process. These functions validate Python objects, string data, and Pydantic model instances respectively against the defined model. The <SwmToken path="/pydantic/main.py" pos="1459:2:2" line-data="def create_model(  # noqa: C901">`create_model`</SwmToken> function dynamically creates a new Pydantic model with validated data. The <SwmToken path="/pydantic/main.py" pos="1251:3:3" line-data="    def copy(">`copy`</SwmToken> function returns a copy of the model with included, excluded, and updated fields as specified.

<SwmSnippet path="/pydantic/main.py" line="51">

---

# <SwmToken path="/pydantic/main.py" pos="54:14:14" line-data="ModelT = TypeVar(&#39;ModelT&#39;, bound=&#39;BaseModel&#39;)">`BaseModel`</SwmToken> Class

The <SwmToken path="/pydantic/main.py" pos="54:14:14" line-data="ModelT = TypeVar(&#39;ModelT&#39;, bound=&#39;BaseModel&#39;)">`BaseModel`</SwmToken> class is defined here. It is the main class that all Pydantic models will inherit from. It provides the core functionality for data validation.

```python

# Always define certain types that are needed to resolve method type hints/annotations
# (even when not type checking) via typing.get_type_hints.
ModelT = TypeVar('ModelT', bound='BaseModel')
TupleGenerator = Generator[Tuple[str, Any], None, None]
# should be `set[int] | set[str] | dict[int, IncEx] | dict[str, IncEx] | None`, but mypy can't cope
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/main.py" line="182">

---

# **init** Method

The **init** method is where the input data is parsed and validated against the model's schema. If the data is invalid, a <SwmToken path="/pydantic/main.py" pos="185:5:5" line-data="        Raises [`ValidationError`][pydantic_core.ValidationError] if the input data cannot be">`ValidationError`</SwmToken> is raised.

```python
    def __init__(self, /, **data: Any) -> None:  # type: ignore
        """Create a new model by parsing and validating input data from keyword arguments.

        Raises [`ValidationError`][pydantic_core.ValidationError] if the input data cannot be
        validated to form a valid model.

        `self` is explicitly positional-only to allow `self` as a field name.
        """
        # `__tracebackhide__` tells pytest and some other tools to omit this function from tracebacks
        __tracebackhide__ = True
        self.__pydantic_validator__.validate_python(data, self_instance=self)
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/main.py" line="1459">

---

# <SwmToken path="/pydantic/main.py" pos="1459:2:2" line-data="def create_model(  # noqa: C901">`create_model`</SwmToken> Function

The <SwmToken path="/pydantic/main.py" pos="1459:2:2" line-data="def create_model(  # noqa: C901">`create_model`</SwmToken> function is a factory function that dynamically creates a new Pydantic model. This can be useful when the model's schema needs to be defined at runtime.

```python
def create_model(  # noqa: C901
    model_name: str,
    /,
    *,
    __config__: ConfigDict | None = None,
    __doc__: str | None = None,
    __base__: type[ModelT] | tuple[type[ModelT], ...] | None = None,
    __module__: str | None = None,
    __validators__: dict[str, Callable[..., Any]] | None = None,
    __cls_kwargs__: dict[str, Any] | None = None,
    __slots__: tuple[str, ...] | None = None,
    **field_definitions: Any,
) -> type[ModelT]:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/models/#dynamic-model-creation

    Dynamically creates and returns a new Pydantic model, in other words, `create_model` dynamically creates a
    subclass of [`BaseModel`][pydantic.BaseModel].

    Args:
        model_name: The name of the newly created model.
        __config__: The configuration of the new model.
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/main.py" line="599">

---

# <SwmToken path="/pydantic/main.py" pos="599:3:3" line-data="    def model_validate_strings(">`model_validate_strings`</SwmToken> Method

The <SwmToken path="/pydantic/main.py" pos="599:3:3" line-data="    def model_validate_strings(">`model_validate_strings`</SwmToken> method validates a Python object against the model, where the object contains string data. This can be useful when dealing with data that comes from a source that only supports strings, such as environment variables.

```python
    def model_validate_strings(
        cls,
        obj: Any,
        *,
        strict: bool | None = None,
        context: Any | None = None,
    ) -> Self:
        """Validate the given object with string data against the Pydantic model.

        Args:
            obj: The object containing string data to validate.
            strict: Whether to enforce types strictly.
            context: Extra variables to pass to the validator.

        Returns:
            The validated Pydantic model.
        """
        # `__tracebackhide__` tells pytest and some other tools to omit this function from tracebacks
        __tracebackhide__ = True
        return cls.__pydantic_validator__.validate_strings(obj, strict=strict, context=context)
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/main.py" line="1251">

---

# copy Method

The copy method creates a copy of the model. It allows you to specify which fields to include or exclude in the copied model, and whether to perform a deep copy.

````python
    def copy(
        self,
        *,
        include: AbstractSetIntStr | MappingIntStrAny | None = None,
        exclude: AbstractSetIntStr | MappingIntStrAny | None = None,
        update: Dict[str, Any] | None = None,  # noqa UP006
        deep: bool = False,
    ) -> Self:  # pragma: no cover
        """Returns a copy of the model.

        !!! warning "Deprecated"
            This method is now deprecated; use `model_copy` instead.

        If you need `include` or `exclude`, use:

        ```py
        data = self.model_dump(include=include, exclude=exclude, round_trip=True)
        data = {**data, **(update or {})}
        copied = self.model_validate(data)
        ```

````

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

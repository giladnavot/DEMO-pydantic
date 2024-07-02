---
title: _FromFieldInfoInputs Overview
---
This document will cover the following topics related to the `_FromFieldInfoInputs` class in the `pydantic/fields.py` file of the DEMO-pydantic repo:

1. What is `_FromFieldInfoInputs`.
2. Variables and functions defined in `_FromFieldInfoInputs`.
3. An example of how to use `_FromFieldInfoInputs`.

```mermaid
graph TD;
  _FromFieldInfoInputs:::currentBaseStyle
_FromFieldInfoInputs --> _FieldInfoInputs

 classDef currentBaseStyle color:#000000,fill:#7CB9F4
```

# What is \_FromFieldInfoInputs

`_FromFieldInfoInputs` is a class that exists solely to add type checking for the `**kwargs` in `FieldInfo.from_field`. It is a TypedDict from the `typing_extensions` module, which means it is a dictionary where keys are strings and values are type hints. The `total=False` parameter indicates that keys that are not explicitly defined may be present with any value.

<SwmSnippet path="/pydantic/fields.py" line="50">

---

# Variables in \_FromFieldInfoInputs

These are the variables defined in the `_FromFieldInfoInputs` class. Each variable represents a key in the dictionary and its type hint. The variables are used to enforce type checking for the `**kwargs` in `FieldInfo.from_field`.

```python
    annotation: type[Any] | None
    default_factory: typing.Callable[[], Any] | None
    alias: str | None
    alias_priority: int | None
    validation_alias: str | AliasPath | AliasChoices | None
    serialization_alias: str | None
    title: str | None
    field_title_generator: typing_extensions.Callable[[str, FieldInfo], str] | None
    description: str | None
    examples: list[Any] | None
    exclude: bool | None
    gt: annotated_types.SupportsGt | None
    ge: annotated_types.SupportsGe | None
    lt: annotated_types.SupportsLt | None
    le: annotated_types.SupportsLe | None
    multiple_of: float | None
    strict: bool | None
    min_length: int | None
    max_length: int | None
    pattern: str | typing.Pattern[str] | None
    allow_inf_nan: bool | None
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/fields.py" line="244">

---

# Usage example

Here is an example of how `_FromFieldInfoInputs` is used in the `from_field` static method of the `FieldInfo` class. The `**kwargs` in the method signature is expected to be of type `_FromFieldInfoInputs`. This means that the keys in `kwargs` should match the variables defined in `_FromFieldInfoInputs` and their values should match the corresponding type hints.

````python
    def from_field(default: Any = PydanticUndefined, **kwargs: Unpack[_FromFieldInfoInputs]) -> FieldInfo:
        """Create a new `FieldInfo` object with the `Field` function.

        Args:
            default: The default value for the field. Defaults to Undefined.
            **kwargs: Additional arguments dictionary.

        Raises:
            TypeError: If 'annotation' is passed as a keyword argument.

        Returns:
            A new FieldInfo object with the given parameters.

        Example:
            This is how you can create a field with default value like this:

            ```python
            import pydantic

            class MyModel(pydantic.BaseModel):
                foo: int = pydantic.Field(4)
````

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="class"><sup>Powered by [Swimm](/)</sup></SwmMeta>

---
title: Defining the role of class validators in Pydantics data validation
---
This document will cover the role of class validators in the Pydantic data validation process. We'll cover:

1. The purpose of class validators
2. How class validators are used in the codebase
3. The functionality and purpose of class validators

# Purpose of Class Validators

Class validators in Pydantic play a crucial role in the data validation process. They are used to apply validation rules to the fields of a model. These rules can be custom or built-in and are applied whenever a new model instance is created.

<SwmSnippet path="/pydantic/functional_validators.py" line="20">

---

# Class Validators in the Codebase

This file contains the definitions of various class validators such as <SwmToken path="/pydantic/functional_validators.py" pos="29:2:2" line-data="class AfterValidator:">`AfterValidator`</SwmToken>, <SwmToken path="/pydantic/functional_validators.py" pos="85:2:2" line-data="class BeforeValidator:">`BeforeValidator`</SwmToken>, <SwmToken path="/pydantic/functional_validators.py" pos="129:2:2" line-data="class PlainValidator:">`PlainValidator`</SwmToken>, and <SwmToken path="/pydantic/functional_validators.py" pos="181:2:2" line-data="class WrapValidator:">`WrapValidator`</SwmToken>. Each of these validators serves a specific purpose in the validation process.

````python
if sys.version_info < (3, 11):
    from typing_extensions import Protocol
else:
    from typing import Protocol

_inspect_validator = _decorators.inspect_validator


@dataclasses.dataclass(frozen=True, **_internal_dataclass.slots_true)
class AfterValidator:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/validators/#annotated-validators

    A metadata class that indicates that a validation should be applied **after** the inner validation logic.

    Attributes:
        func: The validator function.

    Example:
        ```py
        from typing_extensions import Annotated

````

---

</SwmSnippet>

<SwmSnippet path="/pydantic/_internal/_dataclasses.py" line="42">

---

# Functionality and Purpose of Class Validators

The <SwmToken path="/pydantic/_internal/_dataclasses.py" pos="42:3:3" line-data="    class PydanticDataclass(StandardDataclass, typing.Protocol):">`PydanticDataclass`</SwmToken> class contains a <SwmToken path="/pydantic/_internal/_dataclasses.py" pos="48:20:20" line-data="            __pydantic_core_schema__: The pydantic-core schema used to build the SchemaValidator and SchemaSerializer.">`SchemaValidator`</SwmToken> attribute, which is used to validate instances of the dataclass. This is an example of how class validators are used in the Pydantic data validation process.

```python
    class PydanticDataclass(StandardDataclass, typing.Protocol):
        """A protocol containing attributes only available once a class has been decorated as a Pydantic dataclass.

        Attributes:
            __pydantic_config__: Pydantic-specific configuration settings for the dataclass.
            __pydantic_complete__: Whether dataclass building is completed, or if there are still undefined fields.
            __pydantic_core_schema__: The pydantic-core schema used to build the SchemaValidator and SchemaSerializer.
            __pydantic_decorators__: Metadata containing the decorators defined on the dataclass.
            __pydantic_fields__: Metadata about the fields defined on the dataclass.
            __pydantic_serializer__: The pydantic-core SchemaSerializer used to dump instances of the dataclass.
            __pydantic_validator__: The pydantic-core SchemaValidator used to validate instances of the dataclass.
        """

        __pydantic_config__: ClassVar[ConfigDict]
        __pydantic_complete__: ClassVar[bool]
        __pydantic_core_schema__: ClassVar[core_schema.CoreSchema]
        __pydantic_decorators__: ClassVar[_decorators.DecoratorInfos]
        __pydantic_fields__: ClassVar[dict[str, FieldInfo]]
        __pydantic_serializer__: ClassVar[SchemaSerializer]
        __pydantic_validator__: ClassVar[SchemaValidator]
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/type_adapter.py" line="314">

---

The <SwmToken path="/pydantic/type_adapter.py" pos="314:3:3" line-data="    def validator(self) -&gt; SchemaValidator:">`validator`</SwmToken> function returns a <SwmToken path="/pydantic/type_adapter.py" pos="314:10:10" line-data="    def validator(self) -&gt; SchemaValidator:">`SchemaValidator`</SwmToken> used to validate instances of the model. This is another example of how class validators are used in the Pydantic data validation process.

```python
    def validator(self) -> SchemaValidator:
        """The pydantic-core SchemaValidator used to validate instances of the model."""
        if not isinstance(self._validator, SchemaValidator):
            self._init_core_attrs(rebuild_mocks=True)  # Do not expose MockValSer from public function
        assert isinstance(self._validator, SchemaValidator)
        return self._validator
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

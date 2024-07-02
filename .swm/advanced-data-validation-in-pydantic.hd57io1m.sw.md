---
title: Advanced Data Validation in Pydantic
---
Advanced Data Validation in Pydantic involves the use of various classes and functions that allow for more complex validation logic. This includes the use of classes like <SwmToken path="/pydantic/functional_validators.py" pos="29:2:2" line-data="class AfterValidator:">`AfterValidator`</SwmToken>, <SwmToken path="/pydantic/functional_validators.py" pos="85:2:2" line-data="class BeforeValidator:">`BeforeValidator`</SwmToken>, <SwmToken path="/pydantic/functional_validators.py" pos="129:2:2" line-data="class PlainValidator:">`PlainValidator`</SwmToken>, and <SwmToken path="/pydantic/functional_validators.py" pos="181:2:2" line-data="class WrapValidator:">`WrapValidator`</SwmToken>. These classes indicate when a validation should be applied in the data validation process. For instance, <SwmToken path="/pydantic/functional_validators.py" pos="29:2:2" line-data="class AfterValidator:">`AfterValidator`</SwmToken> applies validation after the inner validation logic, <SwmToken path="/pydantic/functional_validators.py" pos="85:2:2" line-data="class BeforeValidator:">`BeforeValidator`</SwmToken> applies it before, <SwmToken path="/pydantic/functional_validators.py" pos="129:2:2" line-data="class PlainValidator:">`PlainValidator`</SwmToken> applies it instead of the inner validation logic, and <SwmToken path="/pydantic/functional_validators.py" pos="181:2:2" line-data="class WrapValidator:">`WrapValidator`</SwmToken> applies it around the inner validation logic. Additionally, functions like <SwmToken path="/pydantic/functional_validators.py" pos="294:2:2" line-data="def field_validator(">`field_validator`</SwmToken> and <SwmToken path="/pydantic/functional_validators.py" pos="539:2:2" line-data="def model_validator(">`model_validator`</SwmToken> are used to decorate methods on the class indicating that they should be used to validate fields or models respectively.

<SwmSnippet path="/pydantic/functional_validators.py" line="29">

---

# <SwmToken path="/pydantic/functional_validators.py" pos="29:2:2" line-data="class AfterValidator:">`AfterValidator`</SwmToken>

The <SwmToken path="/pydantic/functional_validators.py" pos="29:2:2" line-data="class AfterValidator:">`AfterValidator`</SwmToken> class is used to indicate that a validation should be applied after the inner validation logic. It takes a validator function as an attribute, which is applied after the standard validation process.

````python
class AfterValidator:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/validators/#annotated-validators

    A metadata class that indicates that a validation should be applied **after** the inner validation logic.

    Attributes:
        func: The validator function.

    Example:
        ```py
        from typing_extensions import Annotated

        from pydantic import AfterValidator, BaseModel, ValidationError

        MyInt = Annotated[int, AfterValidator(lambda v: v + 1)]

        class Model(BaseModel):
            a: MyInt

        print(Model(a=1).a)
        #> 2
````

---

</SwmSnippet>

<SwmSnippet path="/pydantic/functional_validators.py" line="85">

---

# <SwmToken path="/pydantic/functional_validators.py" pos="85:2:2" line-data="class BeforeValidator:">`BeforeValidator`</SwmToken>

The <SwmToken path="/pydantic/functional_validators.py" pos="85:2:2" line-data="class BeforeValidator:">`BeforeValidator`</SwmToken> class is used to indicate that a validation should be applied before the inner validation logic. It also takes a validator function as an attribute, which is applied before the standard validation process.

````python
class BeforeValidator:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/validators/#annotated-validators

    A metadata class that indicates that a validation should be applied **before** the inner validation logic.

    Attributes:
        func: The validator function.

    Example:
        ```py
        from typing_extensions import Annotated

        from pydantic import BaseModel, BeforeValidator

        MyInt = Annotated[int, BeforeValidator(lambda v: v + 1)]

        class Model(BaseModel):
            a: MyInt

        print(Model(a=1).a)
        #> 2
````

---

</SwmSnippet>

<SwmSnippet path="/pydantic/functional_validators.py" line="129">

---

# <SwmToken path="/pydantic/functional_validators.py" pos="129:2:2" line-data="class PlainValidator:">`PlainValidator`</SwmToken>

The <SwmToken path="/pydantic/functional_validators.py" pos="129:2:2" line-data="class PlainValidator:">`PlainValidator`</SwmToken> class is used to indicate that a validation should be applied instead of the inner validation logic. This allows developers to completely override the standard validation process with their own custom logic.

````python
class PlainValidator:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/validators/#annotated-validators

    A metadata class that indicates that a validation should be applied **instead** of the inner validation logic.

    Attributes:
        func: The validator function.

    Example:
        ```py
        from typing_extensions import Annotated

        from pydantic import BaseModel, PlainValidator

        MyInt = Annotated[int, PlainValidator(lambda v: int(v) + 1)]

        class Model(BaseModel):
            a: MyInt

        print(Model(a='1').a)
        #> 2
````

---

</SwmSnippet>

<SwmSnippet path="/pydantic/functional_validators.py" line="181">

---

# <SwmToken path="/pydantic/functional_validators.py" pos="181:2:2" line-data="class WrapValidator:">`WrapValidator`</SwmToken>

The <SwmToken path="/pydantic/functional_validators.py" pos="181:2:2" line-data="class WrapValidator:">`WrapValidator`</SwmToken> class is used to indicate that a validation should be applied around the inner validation logic. This allows developers to apply additional validation logic both before and after the standard validation process.

````python
class WrapValidator:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/validators/#annotated-validators

    A metadata class that indicates that a validation should be applied **around** the inner validation logic.

    Attributes:
        func: The validator function.

    ```py
    from datetime import datetime

    from typing_extensions import Annotated

    from pydantic import BaseModel, ValidationError, WrapValidator

    def validate_timestamp(v, handler):
        if v == 'now':
            # we don't want to bother with further validation, just return the new value
            return datetime.now()
        try:
            return handler(v)
````

---

</SwmSnippet>

<SwmSnippet path="/pydantic/functional_validators.py" line="294">

---

# <SwmToken path="/pydantic/functional_validators.py" pos="294:2:2" line-data="def field_validator(">`field_validator`</SwmToken>

The <SwmToken path="/pydantic/functional_validators.py" pos="294:2:2" line-data="def field_validator(">`field_validator`</SwmToken> function is used to decorate methods on the class indicating that they should be used to validate fields. It takes a list of fields and a mode ('before', 'after', 'wrap', 'plain') as arguments.

````python
def field_validator(
    field: str,
    /,
    *fields: str,
    mode: FieldValidatorModes = 'after',
    check_fields: bool | None = None,
) -> Callable[[Any], Any]:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/validators/#field-validators

    Decorate methods on the class indicating that they should be used to validate fields.

    Example usage:
    ```py
    from typing import Any

    from pydantic import (
        BaseModel,
        ValidationError,
        field_validator,
    )

````

---

</SwmSnippet>

<SwmSnippet path="/pydantic/functional_validators.py" line="539">

---

# <SwmToken path="/pydantic/functional_validators.py" pos="539:2:2" line-data="def model_validator(">`model_validator`</SwmToken>

The <SwmToken path="/pydantic/functional_validators.py" pos="539:2:2" line-data="def model_validator(">`model_validator`</SwmToken> function is used to decorate model methods for validation purposes. It takes a mode ('wrap', 'before', 'after') as an argument, specifying when the validation should occur.

````python
def model_validator(
    *,
    mode: Literal['wrap', 'before', 'after'],
) -> Any:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/validators/#model-validators

    Decorate model methods for validation purposes.

    Example usage:
    ```py
    from typing_extensions import Self

    from pydantic import BaseModel, ValidationError, model_validator

    class Square(BaseModel):
        width: float
        height: float

        @model_validator(mode='after')
        def verify_square(self) -> Self:
            if self.width != self.height:
````

---

</SwmSnippet>

<SwmSnippet path="/pydantic/functional_validators.py" line="671">

---

# <SwmToken path="/pydantic/functional_validators.py" pos="671:3:3" line-data="    class SkipValidation:">`SkipValidation`</SwmToken>

The <SwmToken path="/pydantic/functional_validators.py" pos="671:3:3" line-data="    class SkipValidation:">`SkipValidation`</SwmToken> class is used to skip validation for certain fields. This can be useful for performance reasons, or when the validation logic is not necessary for certain fields.

```python
    class SkipValidation:
        """If this is applied as an annotation (e.g., via `x: Annotated[int, SkipValidation]`), validation will be
            skipped. You can also use `SkipValidation[int]` as a shorthand for `Annotated[int, SkipValidation]`.

        This can be useful if you want to use a type annotation for documentation/IDE/type-checking purposes,
        and know that it is safe to skip validation for one or more of the fields.

        Because this converts the validation schema to `any_schema`, subsequent annotation-applied transformations
        may not have the expected effects. Therefore, when used, this annotation should generally be the final
        annotation applied to a type.
        """

        def __class_getitem__(cls, item: Any) -> Any:
            return Annotated[item, SkipValidation()]

        @classmethod
        def __get_pydantic_core_schema__(cls, source: Any, handler: GetCoreSchemaHandler) -> core_schema.CoreSchema:
            original_schema = handler(source)
            metadata = _core_metadata.build_metadata_dict(js_annotation_functions=[lambda _c, h: h(original_schema)])
            return core_schema.any_schema(
                metadata=metadata,
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

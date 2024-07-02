---
title: Getting started with Complex Type Definitions
---
Complex Type Definitions in Pydantic are used to define more intricate data structures beyond simple integers, strings, and boolean values. They include classes, such as <SwmToken path="/pydantic/types.py" pos="2583:4:4" line-data="class Model(BaseModel):">`BaseModel`</SwmToken>, which allow for the creation of custom data models with type hinting and automatic validation. Pydantic also provides a variety of complex types out of the box, such as UUID, <SwmToken path="/pydantic/v1/types.py" pos="781:1:1" line-data="    FilePath = Path">`FilePath`</SwmToken>, Json, etc. These complex types come with <SwmToken path="/pydantic/types.py" pos="970:5:7" line-data="        #&gt; obj=&lt;built-in function cos&gt;">`built-in`</SwmToken> validation and serialization methods, making it easier to work with complex data. Additionally, Pydantic allows for the creation of custom complex types through the use of Annotated, a special class that can be used to add metadata to type hints.

<SwmSnippet path="/pydantic/types.py" line="2583">

---

# Defining a Complex Type

Here we see a complex type <SwmToken path="/pydantic/types.py" pos="2583:2:2" line-data="class Model(BaseModel):">`Model`</SwmToken> being defined. It has an attribute <SwmToken path="/pydantic/types.py" pos="2584:1:1" line-data="    base64url_str: Base64UrlStr">`base64url_str`</SwmToken> of type <SwmToken path="/pydantic/types.py" pos="2584:4:4" line-data="    base64url_str: Base64UrlStr">`Base64UrlStr`</SwmToken>. An instance of <SwmToken path="/pydantic/types.py" pos="2583:2:2" line-data="class Model(BaseModel):">`Model`</SwmToken> is then created with some <SwmToken path="/pydantic/types.py" pos="2586:10:10" line-data="# Initialize the model with base64 data">`base64`</SwmToken> data, which is automatically validated by Pydantic.

````python
class Model(BaseModel):
    base64url_str: Base64UrlStr

# Initialize the model with base64 data
m = Model(base64url_str='SHc_dHc-TXc==')
print(m)
#> base64url_str='Hw?tw>Mw'
```
````

---

</SwmSnippet>

<SwmSnippet path="/pydantic/types.py" line="2597">

---

# Using <SwmToken path="/pydantic/types.py" pos="2598:2:2" line-data="class GetPydanticSchema:">`GetPydanticSchema`</SwmToken>

The <SwmToken path="/pydantic/types.py" pos="2598:2:2" line-data="class GetPydanticSchema:">`GetPydanticSchema`</SwmToken> class is a convenience class for creating annotations that provide Pydantic custom type hooks. It eliminates the need to create a custom 'marker' which defines the <SwmToken path="/pydantic/types.py" pos="2604:2:2" line-data="     `__get_pydantic_core_schema__` and `__get_pydantic_json_schema__` custom hook methods.">`__get_pydantic_core_schema__`</SwmToken> and <SwmToken path="/pydantic/types.py" pos="2604:8:8" line-data="     `__get_pydantic_core_schema__` and `__get_pydantic_json_schema__` custom hook methods.">`__get_pydantic_json_schema__`</SwmToken> custom hook methods. In this example, a field is treated by type checkers as `int`, but by Pydantic as <SwmToken path="/pydantic/types.py" pos="2606:37:37" line-data="    For example, to have a field treated by type checkers as `int`, but by pydantic as `Any`, you can do:">`Any`</SwmToken>.

````python
@_dataclasses.dataclass(**_internal_dataclass.slots_true)
class GetPydanticSchema:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/types/#using-getpydanticschema-to-reduce-boilerplate

    A convenience class for creating an annotation that provides pydantic custom type hooks.

    This class is intended to eliminate the need to create a custom "marker" which defines the
     `__get_pydantic_core_schema__` and `__get_pydantic_json_schema__` custom hook methods.

    For example, to have a field treated by type checkers as `int`, but by pydantic as `Any`, you can do:
    ```python
    from typing import Any

    from typing_extensions import Annotated

    from pydantic import BaseModel, GetPydanticSchema

    HandleAsAny = GetPydanticSchema(lambda _s, h: h(Any))

    class Model(BaseModel):
        x: Annotated[int, HandleAsAny]  # pydantic sees `x: Any`
````

---

</SwmSnippet>

<SwmSnippet path="/pydantic/types.py" line="2645">

---

# Using Tag

The <SwmToken path="/pydantic/types.py" pos="2646:2:2" line-data="class Tag:">`Tag`</SwmToken> class provides a way to specify the expected tag to use for a case of a (callable) discriminated union. It also provides a way to label a union case in error messages. In this example, the <SwmToken path="/pydantic/types.py" pos="2646:2:2" line-data="class Tag:">`Tag`</SwmToken> is used to specify that if <SwmToken path="/pydantic/types.py" pos="2653:4:4" line-data="    if `get_discriminator_value` returns `&#39;apple&#39;`, the input should be validated as an `ApplePie`, and if it">`get_discriminator_value`</SwmToken> returns <SwmToken path="/pydantic/types.py" pos="2653:11:11" line-data="    if `get_discriminator_value` returns `&#39;apple&#39;`, the input should be validated as an `ApplePie`, and if it">`apple`</SwmToken>, the input should be validated as an <SwmToken path="/pydantic/types.py" pos="2653:30:30" line-data="    if `get_discriminator_value` returns `&#39;apple&#39;`, the input should be validated as an `ApplePie`, and if it">`ApplePie`</SwmToken>, and if it returns <SwmToken path="/pydantic/types.py" pos="2654:5:5" line-data="    returns `&#39;pumpkin&#39;`, the input should be validated as a `PumpkinPie`.">`pumpkin`</SwmToken>, the input should be validated as a <SwmToken path="/pydantic/types.py" pos="2654:24:24" line-data="    returns `&#39;pumpkin&#39;`, the input should be validated as a `PumpkinPie`.">`PumpkinPie`</SwmToken>.

````python
@_dataclasses.dataclass(**_internal_dataclass.slots_true, frozen=True)
class Tag:
    """Provides a way to specify the expected tag to use for a case of a (callable) discriminated union.

    Also provides a way to label a union case in error messages.

    When using a callable `Discriminator`, attach a `Tag` to each case in the `Union` to specify the tag that
    should be used to identify that case. For example, in the below example, the `Tag` is used to specify that
    if `get_discriminator_value` returns `'apple'`, the input should be validated as an `ApplePie`, and if it
    returns `'pumpkin'`, the input should be validated as a `PumpkinPie`.

    The primary role of the `Tag` here is to map the return value from the callable `Discriminator` function to
    the appropriate member of the `Union` in question.

    ```py
    from typing import Any, Union

    from typing_extensions import Annotated, Literal

    from pydantic import BaseModel, Discriminator, Tag

````

---

</SwmSnippet>

<SwmSnippet path="/pydantic/types.py" line="2734">

---

# Using Discriminator

The <SwmToken path="/pydantic/types.py" pos="2734:2:2" line-data="class Discriminator:">`Discriminator`</SwmToken> class provides a way to use a custom callable as the way to extract the value of a union discriminator. This allows you to get validation behavior like you'd get from <SwmToken path="/pydantic/types.py" pos="2739:26:32" line-data="    This allows you to get validation behavior like you&#39;d get from `Field(discriminator=&lt;field_name&gt;)`,">`Field(discriminator=<field_name>)`</SwmToken>, but without needing to have a single shared field across all the union choices. In this example, a custom callable is used as the way to identify which member of a union a value belongs to.

````python
class Discriminator:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/unions/#discriminated-unions-with-callable-discriminator

    Provides a way to use a custom callable as the way to extract the value of a union discriminator.

    This allows you to get validation behavior like you'd get from `Field(discriminator=<field_name>)`,
    but without needing to have a single shared field across all the union choices. This also makes it
    possible to handle unions of models and primitive types with discriminated-union-style validation errors.
    Finally, this allows you to use a custom callable as the way to identify which member of a union a value
    belongs to, while still seeing all the performance benefits of a discriminated union.

    Consider this example, which is much more performant with the use of `Discriminator` and thus a `TaggedUnion`
    than it would be as a normal `Union`.

    ```py
    from typing import Any, Union

    from typing_extensions import Annotated, Literal

    from pydantic import BaseModel, Discriminator, Tag

````

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

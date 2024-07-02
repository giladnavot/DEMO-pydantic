---
title: Understanding Basic Type Definitions
---
Basic Type Definitions in Pydantic are a set of predefined types that can be used to validate, serialize, and document your data models. They include common types such as integers, strings, and booleans, as well as more complex types like URLs, emails, and UUIDs. These types are used to define the data model's fields, ensuring that the data adheres to the defined structure and rules. For example, if a field is defined as an <SwmToken path="/pydantic/networks.py" pos="412:1:1" line-data="    EmailStr = Annotated[str, ...]">`EmailStr`</SwmToken>, Pydantic will ensure that this field contains a valid email address.

<SwmSnippet path="/pydantic/types.py" line="2583">

---

# Model Class

The <SwmToken path="/pydantic/types.py" pos="2583:2:2" line-data="class Model(BaseModel):">`Model`</SwmToken> class is a basic type definition used to create data models. Here, a model is defined with a <SwmToken path="/pydantic/types.py" pos="2584:1:1" line-data="    base64url_str: Base64UrlStr">`base64url_str`</SwmToken> field of type <SwmToken path="/pydantic/types.py" pos="2584:4:4" line-data="    base64url_str: Base64UrlStr">`Base64UrlStr`</SwmToken>. The model is then initialized with <SwmToken path="/pydantic/types.py" pos="2586:10:10" line-data="# Initialize the model with base64 data">`base64`</SwmToken> data.

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

<SwmSnippet path="/pydantic/types.py" line="811">

---

# conset Function

The <SwmToken path="/pydantic/types.py" pos="811:2:2" line-data="def conset(">`conset`</SwmToken> function is used to create a set with constraints. It takes the item type and optional parameters for minimum and maximum length of the set.

```python
def conset(
    item_type: type[HashableItemType], *, min_length: int | None = None, max_length: int | None = None
) -> type[set[HashableItemType]]:
    """A wrapper around `typing.Set` that allows for additional constraints.

    Args:
        item_type: The type of the items in the set.
        min_length: The minimum length of the set.
        max_length: The maximum length of the set.

    Returns:
        The wrapped set type.
    """
    return Annotated[Set[item_type], annotated_types.Len(min_length or 0, max_length)]  # pyright: ignore[reportReturnType]
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/types.py" line="1599">

---

# <SwmToken path="/pydantic/types.py" pos="2624:1:1" line-data="    get_pydantic_core_schema: Callable[[Any, GetCoreSchemaHandler], CoreSchema] | None = None">`get_pydantic_core_schema`</SwmToken> Method

The <SwmToken path="/pydantic/types.py" pos="1599:3:3" line-data="    def __get_pydantic_core_schema__(cls, source: type[Any], handler: GetCoreSchemaHandler) -&gt; core_schema.CoreSchema:">`__get_pydantic_core_schema__`</SwmToken> method is used to get the core schema of a Pydantic model. It is used internally by Pydantic to handle serialization and schema generation.

```python
    def __get_pydantic_core_schema__(cls, source: type[Any], handler: GetCoreSchemaHandler) -> core_schema.CoreSchema:
        def serialize(
            value: _SecretField[SecretType], info: core_schema.SerializationInfo
        ) -> str | _SecretField[SecretType]:
            if info.mode == 'json':
                # we want the output to always be string without the `b'` prefix for bytes,
                # hence we just use `secret_display`
                return _secret_display(value.get_secret_value())
            else:
                return value

        def get_json_schema(_core_schema: core_schema.CoreSchema, handler: GetJsonSchemaHandler) -> JsonSchemaValue:
            json_schema = handler(cls._inner_schema)
            _utils.update_not_none(
                json_schema,
                type='string',
                writeOnly=True,
                format='password',
            )
            return json_schema

```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

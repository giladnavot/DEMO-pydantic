---
title: Getting Started with Type Handling
---
Type Handling in Pydantic is about validating and parsing data types using Python type hints. It provides a way to ensure that the data adheres to the specified types. This is done using various classes and functions in the Pydantic library. For example, Pydantic provides classes like <SwmToken path="/tests/mypy/outputs/1.0.1/mypy-default_ini/plugin_success.py" pos="10:2:2" line-data="class Model(BaseModel):">`Model`</SwmToken> for defining data models with type annotations, and functions like <SwmToken path="/pydantic/types.py" pos="846:2:2" line-data="def conlist(">`conlist`</SwmToken> for constraining the length and type of list items. Pydantic also provides support for custom data types and complex structures like discriminated unions using classes like <SwmToken path="/pydantic/types.py" pos="109:2:2" line-data="    &#39;Tag&#39;,">`Tag`</SwmToken> and <SwmToken path="/pydantic/types.py" pos="110:2:2" line-data="    &#39;Discriminator&#39;,">`Discriminator`</SwmToken>.

<SwmSnippet path="/pydantic/types.py" line="846">

---

# Defining Data Types

Here is an example of how to define a constrained list using the <SwmToken path="/pydantic/types.py" pos="846:2:2" line-data="def conlist(">`conlist`</SwmToken> function. The <SwmToken path="/pydantic/types.py" pos="847:1:1" line-data="    item_type: type[AnyItemType],">`item_type`</SwmToken> parameter specifies the type of the items in the list, while the <SwmToken path="/pydantic/types.py" pos="849:1:1" line-data="    min_length: int | None = None,">`min_length`</SwmToken> and <SwmToken path="/pydantic/types.py" pos="850:1:1" line-data="    max_length: int | None = None,">`max_length`</SwmToken> parameters specify the minimum and maximum length of the list respectively.

```python
def conlist(
    item_type: type[AnyItemType],
    *,
    min_length: int | None = None,
    max_length: int | None = None,
    unique_items: bool | None = None,
) -> type[list[AnyItemType]]:
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/types.py" line="2890">

---

# Type Validation

The <SwmToken path="/pydantic/types.py" pos="2890:2:2" line-data="def _get_type_name(x: Any) -&gt; str:">`_get_type_name`</SwmToken> function is used to get the name of the type of a given value. This is used in the process of validating the types of the input data.

```python
def _get_type_name(x: Any) -> str:
    type_ = type(x)
    if type_ in _JSON_TYPES:
        return type_.__name__

    # Handle proper subclasses; note we don't need to handle None or bool here
    if isinstance(x, int):
        return 'int'
    if isinstance(x, float):
        return 'float'
    if isinstance(x, str):
        return 'str'
    if isinstance(x, list):
        return 'list'
    if isinstance(x, dict):
        return 'dict'

    # Fail by returning the type's actual name
    return getattr(type_, '__name__', '<no type name>')
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/types.py" line="811">

---

# Using Custom Types

You can also define your own custom types using the <SwmToken path="/pydantic/types.py" pos="824:3:3" line-data="    return Annotated[Set[item_type], annotated_types.Len(min_length or 0, max_length)]  # pyright: ignore[reportReturnType]">`Annotated`</SwmToken> class. In this example, the <SwmToken path="/pydantic/types.py" pos="811:2:2" line-data="def conset(">`conset`</SwmToken> function is used to create a constrained set type.

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

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

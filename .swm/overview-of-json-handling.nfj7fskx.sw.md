---
title: Overview of JSON Handling
---
JSON Handling in the DEMO-pydantic repository refers to the process of encoding and decoding data into JSON format. This is achieved through the use of Python's built-in json module and the Pydantic library. The Pydantic library provides a way to validate data and convert it into Python data types, which can then be easily converted into JSON. The repository contains a file named `json.py` in the `pydantic/deprecated` directory, which includes functions and methods for handling JSON data. These include `pydantic_encoder`, `custom_pydantic_encoder`, and `timedelta_isoformat`. The `ENCODERS_BY_TYPE` dictionary in the same file maps Python data types to their corresponding JSON encoders.

<SwmSnippet path="/pydantic/deprecated/json.py" line="53">

---

# JSON Encoders

This is a dictionary of encoders for various Python data types. Each encoder is a function that takes a Python object of a certain type and returns its JSON equivalent.

```python
ENCODERS_BY_TYPE: Dict[Type[Any], Callable[[Any], Any]] = {
    bytes: lambda o: o.decode(),
    Color: str,
    datetime.date: isoformat,
    datetime.datetime: isoformat,
    datetime.time: isoformat,
    datetime.timedelta: lambda td: td.total_seconds(),
    Decimal: decimal_encoder,
    Enum: lambda o: o.value,
    frozenset: list,
    deque: list,
    GeneratorType: list,
    IPv4Address: str,
    IPv4Interface: str,
    IPv4Network: str,
    IPv6Address: str,
    IPv6Interface: str,
    IPv6Network: str,
    NameEmail: str,
    Path: str,
    Pattern: lambda o: o.pattern,
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/json.py" line="85">

---

# Pydantic Encoder

This function takes a Python object and tries to find a suitable encoder for it in the `ENCODERS_BY_TYPE` dictionary. If it finds one, it uses it to convert the object into its JSON equivalent. If it doesn't find one, it raises a TypeError.

```python
def pydantic_encoder(obj: Any) -> Any:
    warnings.warn(
        '`pydantic_encoder` is deprecated, use `pydantic_core.to_jsonable_python` instead.',
        category=PydanticDeprecatedSince20,
        stacklevel=2,
    )
    from dataclasses import asdict, is_dataclass

    from ..main import BaseModel

    if isinstance(obj, BaseModel):
        return obj.model_dump()
    elif is_dataclass(obj):
        return asdict(obj)  # type: ignore

    # Check the class type and its superclasses for a matching encoder
    for base in obj.__class__.__mro__[:-1]:
        try:
            encoder = ENCODERS_BY_TYPE[base]
        except KeyError:
            continue
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/json.py" line="116">

---

# Custom Pydantic Encoder

This function is similar to the `pydantic_encoder`, but it allows you to pass in your own dictionary of encoders. This can be useful if you have custom Python types that you need to serialize.

```python
def custom_pydantic_encoder(type_encoders: Dict[Any, Callable[[Type[Any]], Any]], obj: Any) -> Any:
    warnings.warn(
        '`custom_pydantic_encoder` is deprecated, use `BaseModel.model_dump` instead.',
        category=PydanticDeprecatedSince20,
        stacklevel=2,
    )
    # Check the class type and its superclasses for a matching encoder
    for base in obj.__class__.__mro__[:-1]:
        try:
            encoder = type_encoders[base]
        except KeyError:
            continue

        return encoder(obj)
    else:  # We have exited the for loop without finding a suitable encoder
        return pydantic_encoder(obj)
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

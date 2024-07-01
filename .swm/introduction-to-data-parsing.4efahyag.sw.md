---
title: Introduction to Data Parsing
---
Data parsing in the context of this repository refers to the process of interpreting input data into a format more suitable for further processing. It's a crucial step in data preprocessing, especially when the input data comes in various formats. In this repository, data parsing is handled by functions like `load_file` and `load_str_bytes`, which interpret data from files or strings respectively. These functions support different protocols such as JSON and pickle, and they convert the input data into a Python data structure for further processing.

<SwmSnippet path="/pydantic/deprecated/parse.py" line="60">

---

# Data Parsing with load_file

The `load_file` function is used to parse data from a file. It takes in a file path and optional parameters such as `content_type`, `encoding`, `proto`, `allow_pickle`, and `json_loads`. The function reads the bytes from the file, determines the protocol (json or pickle) based on the file suffix, and then calls the `load_str_bytes` function to parse the data.

```python
def load_file(
    path: str | Path,
    *,
    content_type: str | None = None,
    encoding: str = 'utf8',
    proto: Protocol | None = None,
    allow_pickle: bool = False,
    json_loads: Callable[[str], Any] = json.loads,
) -> Any:
    warnings.warn('`load_file` is deprecated.', category=PydanticDeprecatedSince20, stacklevel=2)
    path = Path(path)
    b = path.read_bytes()
    if content_type is None:
        if path.suffix in ('.js', '.json'):
            proto = Protocol.json
        elif path.suffix == '.pkl':
            proto = Protocol.pickle

    return load_str_bytes(
        b, proto=proto, content_type=content_type, encoding=encoding, allow_pickle=allow_pickle, json_loads=json_loads
    )
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/parse.py" line="26">

---

# Data Parsing with load_str_bytes

The `load_str_bytes` function is used to parse data from a string or bytes. It takes in a string or bytes and optional parameters such as `content_type`, `encoding`, `proto`, `allow_pickle`, and `json_loads`. The function determines the protocol (json or pickle) based on the content type, decodes the data if necessary, and then uses the appropriate method (json.loads or pickle.loads) to parse the data.

```python
def load_str_bytes(
    b: str | bytes,
    *,
    content_type: str | None = None,
    encoding: str = 'utf8',
    proto: Protocol | None = None,
    allow_pickle: bool = False,
    json_loads: Callable[[str], Any] = json.loads,
) -> Any:
    warnings.warn('`load_str_bytes` is deprecated.', category=PydanticDeprecatedSince20, stacklevel=2)
    if proto is None and content_type:
        if content_type.endswith(('json', 'javascript')):
            pass
        elif allow_pickle and content_type.endswith('pickle'):
            proto = Protocol.pickle
        else:
            raise TypeError(f'Unknown content-type: {content_type}')

    proto = proto or Protocol.json

    if proto == Protocol.json:
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

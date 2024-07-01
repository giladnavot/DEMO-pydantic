---
title: Exploring the Core of Pydantic
---
Pydantic Core, as referred to in this repository, is a fundamental part of the Pydantic library. It contains core functionalities and classes such as <SwmToken path="/pydantic/main.py" pos="29:6:6" line-data="from pydantic_core import PydanticUndefined">`PydanticUndefined`</SwmToken> and <SwmToken path="/pydantic/_internal/_core_utils.py" pos="13:6:6" line-data="from pydantic_core import CoreSchema, core_schema">`CoreSchema`</SwmToken>. These are used across different modules for data validation and schema definition. The <SwmToken path="/pydantic/main.py" pos="29:6:6" line-data="from pydantic_core import PydanticUndefined">`PydanticUndefined`</SwmToken> is a special class used to represent undefined values, while <SwmToken path="/pydantic/_internal/_core_utils.py" pos="13:6:6" line-data="from pydantic_core import CoreSchema, core_schema">`CoreSchema`</SwmToken> is used for defining and validating data schemas.

<SwmSnippet path="/pydantic/main.py" line="29">

---

## <SwmToken path="/pydantic/main.py" pos="29:6:6" line-data="from pydantic_core import PydanticUndefined">`PydanticUndefined`</SwmToken>

<SwmToken path="/pydantic/main.py" pos="29:6:6" line-data="from pydantic_core import PydanticUndefined">`PydanticUndefined`</SwmToken> is a class from the Pydantic Core that represents undefined values in Pydantic models.

```python
from pydantic_core import PydanticUndefined
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/_internal/_core_utils.py" line="13">

---

## <SwmToken path="/pydantic/_internal/_core_utils.py" pos="13:6:6" line-data="from pydantic_core import CoreSchema, core_schema">`CoreSchema`</SwmToken>

<SwmToken path="/pydantic/_internal/_core_utils.py" pos="13:6:6" line-data="from pydantic_core import CoreSchema, core_schema">`CoreSchema`</SwmToken> is another class from the Pydantic Core that is used to define the schema of a Pydantic model.

```python
from pydantic_core import CoreSchema, core_schema
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/_internal/_config.py" line="12">

---

## <SwmToken path="/pydantic/_internal/_config.py" pos="12:6:6" line-data="from pydantic_core import core_schema">`core_schema`</SwmToken>

<SwmToken path="/pydantic/_internal/_config.py" pos="12:6:6" line-data="from pydantic_core import core_schema">`core_schema`</SwmToken> is a function from the Pydantic Core that is used to validate the schema of a Pydantic model.

```python
from pydantic_core import core_schema
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

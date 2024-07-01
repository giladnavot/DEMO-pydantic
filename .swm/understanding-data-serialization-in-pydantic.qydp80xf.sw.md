---
title: Understanding Data Serialization in Pydantic
---
Data Serialization is the process of converting data objects or structures into a format that can be stored, transmitted, and reconstructed later. In the context of Pydantic, it's used to transform complex Python data types into a format that can be easily stored or transmitted. The `json` module in Pydantic is an example of a tool used for data serialization, allowing for the conversion of Python objects into a JSON formatted string.

<SwmSnippet path="/pydantic/json.py" line="1">

---

## Usage of Data Serialization in Pydantic

This code snippet shows the import of the `json` module in Pydantic. The `json` module is used for data serialization in Pydantic. The `__getattr__` function is a Python built-in function that is used to handle attributes that are not found; here it's used for backward compatibility with version 1 of the library.

```python
"""The `json` module is a backport module from V1."""

from ._migration import getattr_migration

__getattr__ = getattr_migration(__name__)
```

---

</SwmSnippet>

# Data Serialization Function

This section will explain the **getattr** function in the `json` module.

<SwmSnippet path="/pydantic/functional_serializers.py" line="5">

---

## **getattr** Function

The `__getattr__` function is a special Python method used to customize attribute access. In this context, it's used for attribute migration, which is a part of data serialization process. When an attribute is not found in the usual places (i.e., it is not an instance attribute nor is it found in the class tree for self), Python calls this method to get the attribute.

```python
import dataclasses
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

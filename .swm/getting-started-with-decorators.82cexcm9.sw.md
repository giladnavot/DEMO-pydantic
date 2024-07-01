---
title: Getting Started with Decorators
---
Decorators in Python are a powerful tool that allows developers to modify the behavior of a function or class. They provide a simple syntax for calling higher-order functions. In the context of the DEMO-pydantic repository, decorators are used for various purposes such as backporting functionalities from previous versions. For instance, the `decorator` module is a backport module from V1, which means it contains functionalities that were present in the first version of the library and are still needed in the current version.

<SwmSnippet path="/pydantic/decorator.py" line="1">

---

# Decorator Usage in [decorator.py](http://decorator.py)

Here, the `getattr_migration` function is imported from the `_migration` module and used as a decorator for the `__getattr__` function. This modifies the behavior of `__getattr__` to include migration-related logic.

```python
"""The `decorator` module is a backport module from V1."""

from ._migration import getattr_migration

__getattr__ = getattr_migration(__name__)
```

---

</SwmSnippet>

# Functions of Decorators

Understanding Decorators

<SwmSnippet path="/pydantic/decorator.py" line="1">

---

## Decorators in Pydantic

The `decorator` module in Pydantic is a backport from V1. It includes a function `getattr_migration` which is used for attribute migration. This function is assigned to the `__getattr__` attribute of the module, which is a special method in Python that's called when the dot operator is used to access an undefined attribute.

```python
"""The `decorator` module is a backport module from V1."""

from ._migration import getattr_migration

__getattr__ = getattr_migration(__name__)
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

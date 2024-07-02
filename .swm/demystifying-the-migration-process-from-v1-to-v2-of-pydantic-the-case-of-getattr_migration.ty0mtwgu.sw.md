---
title: >-
  Demystifying the migration process from V1 to V2 of Pydantic - the case of
  getattr_migration
---
This document will cover the rationale and mechanics behind the migration from <SwmToken path="/pydantic/_migration.py" pos="32:17:17" line-data="    f&#39;pydantic.utils:{obj}&#39;: f&#39;pydantic.v1.utils:{obj}&#39;">`v1`</SwmToken>, particularly regarding <SwmToken path="/pydantic/_migration.py" pos="249:2:2" line-data="def getattr_migration(module: str) -&gt; Callable[[str], Any]:">`getattr_migration`</SwmToken>. We'll cover:

1. What is <SwmToken path="/pydantic/_migration.py" pos="249:2:2" line-data="def getattr_migration(module: str) -&gt; Callable[[str], Any]:">`getattr_migration`</SwmToken>?
2. How is <SwmToken path="/pydantic/_migration.py" pos="249:2:2" line-data="def getattr_migration(module: str) -&gt; Callable[[str], Any]:">`getattr_migration`</SwmToken> used in the codebase?
3. The functionality and purpose of <SwmToken path="/pydantic/_migration.py" pos="249:2:2" line-data="def getattr_migration(module: str) -&gt; Callable[[str], Any]:">`getattr_migration`</SwmToken>.

<SwmSnippet path="/pydantic/_migration.py" line="249">

---

# What is <SwmToken path="/pydantic/_migration.py" pos="249:2:2" line-data="def getattr_migration(module: str) -&gt; Callable[[str], Any]:">`getattr_migration`</SwmToken>?

<SwmToken path="/pydantic/_migration.py" pos="249:2:2" line-data="def getattr_migration(module: str) -&gt; Callable[[str], Any]:">`getattr_migration`</SwmToken> is a function that implements PEP 562 for objects that were either moved or removed during the migration to <SwmToken path="/pydantic/_migration.py" pos="251:3:3" line-data="    to V2.">`V2`</SwmToken>. It takes a module name as an argument and returns a callable that will raise an error if the object is not found.

```python
def getattr_migration(module: str) -> Callable[[str], Any]:
    """Implement PEP 562 for objects that were either moved or removed on the migration
    to V2.

    Args:
        module: The module name.

    Returns:
        A callable that will raise an error if the object is not found.
    """
    # This avoids circular import with errors.py.
    from .errors import PydanticImportError

    def wrapper(name: str) -> object:
        """Raise an error if the object is not found, or warn if it was moved.

        In case it was moved, it still returns the object.

        Args:
            name: The object name.

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/datetime_parse.py" line="3">

---

# How is <SwmToken path="/pydantic/datetime_parse.py" pos="3:7:7" line-data="from ._migration import getattr_migration">`getattr_migration`</SwmToken> used in the codebase?

<SwmToken path="/pydantic/datetime_parse.py" pos="3:7:7" line-data="from ._migration import getattr_migration">`getattr_migration`</SwmToken> is imported from the <SwmToken path="/pydantic/datetime_parse.py" pos="3:3:3" line-data="from ._migration import getattr_migration">`_migration`</SwmToken> module and is used to define the <SwmToken path="/pydantic/datetime_parse.py" pos="5:0:0" line-data="__getattr__ = getattr_migration(__name__)">`__getattr__`</SwmToken> function for the module. This pattern is repeated across multiple modules in the codebase.

```python
from ._migration import getattr_migration

__getattr__ = getattr_migration(__name__)
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/_migration.py" line="249">

---

# The functionality and purpose of <SwmToken path="/pydantic/_migration.py" pos="249:2:2" line-data="def getattr_migration(module: str) -&gt; Callable[[str], Any]:">`getattr_migration`</SwmToken>

The <SwmToken path="/pydantic/_migration.py" pos="249:2:2" line-data="def getattr_migration(module: str) -&gt; Callable[[str], Any]:">`getattr_migration`</SwmToken> function serves to handle the migration of objects from <SwmToken path="/pydantic/_migration.py" pos="32:17:17" line-data="    f&#39;pydantic.utils:{obj}&#39;: f&#39;pydantic.v1.utils:{obj}&#39;">`v1`</SwmToken> to <SwmToken path="/pydantic/_migration.py" pos="251:3:3" line-data="    to V2.">`V2`</SwmToken>. If an object was moved, it issues a warning and still returns the object. If an object was removed, it raises an error. This function is crucial for maintaining backward compatibility and guiding developers through the migration process.

```python
def getattr_migration(module: str) -> Callable[[str], Any]:
    """Implement PEP 562 for objects that were either moved or removed on the migration
    to V2.

    Args:
        module: The module name.

    Returns:
        A callable that will raise an error if the object is not found.
    """
    # This avoids circular import with errors.py.
    from .errors import PydanticImportError

    def wrapper(name: str) -> object:
        """Raise an error if the object is not found, or warn if it was moved.

        In case it was moved, it still returns the object.

        Args:
            name: The object name.

```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

---
title: Validators Backport
---
Validators in Pydantic are used for data validation. They ensure that the data conforms to the defined schema. Validators are functions that take a field's value and return a modified value, which is then used in the model instance. If the validation fails, validators raise an error.

The <SwmToken path="/pydantic/validators.py" pos="1:6:6" line-data="&quot;&quot;&quot;The `validators` module is a backport module from V1.&quot;&quot;&quot;">`validators`</SwmToken> module in this repository is a backport from <SwmToken path="/pydantic/validators.py" pos="1:21:21" line-data="&quot;&quot;&quot;The `validators` module is a backport module from V1.&quot;&quot;&quot;">`V1`</SwmToken>. This means it contains functions that were originally in <SwmToken path="/pydantic/validators.py" pos="1:21:21" line-data="&quot;&quot;&quot;The `validators` module is a backport module from V1.&quot;&quot;&quot;">`V1`</SwmToken> of Pydantic, but have been brought back into this version for compatibility reasons.

The <SwmToken path="/pydantic/validators.py" pos="3:7:7" line-data="from ._migration import getattr_migration">`getattr_migration`</SwmToken> function is used in the <SwmToken path="/pydantic/validators.py" pos="1:6:6" line-data="&quot;&quot;&quot;The `validators` module is a backport module from V1.&quot;&quot;&quot;">`validators`</SwmToken> module to handle attribute access in a way that's compatible with both the current version and <SwmToken path="/pydantic/validators.py" pos="1:21:21" line-data="&quot;&quot;&quot;The `validators` module is a backport module from V1.&quot;&quot;&quot;">`V1`</SwmToken>.

<SwmSnippet path="/pydantic/validators.py" line="1">

---

# Validators Module

This is the validators module in Pydantic. It is a backport from <SwmToken path="/pydantic/validators.py" pos="1:21:21" line-data="&quot;&quot;&quot;The `validators` module is a backport module from V1.&quot;&quot;&quot;">`V1`</SwmToken>, meaning it was brought from <SwmToken path="/pydantic/validators.py" pos="1:21:21" line-data="&quot;&quot;&quot;The `validators` module is a backport module from V1.&quot;&quot;&quot;">`V1`</SwmToken> to maintain compatibility. The <SwmToken path="/pydantic/validators.py" pos="3:7:7" line-data="from ._migration import getattr_migration">`getattr_migration`</SwmToken> function is imported from the <SwmToken path="/pydantic/validators.py" pos="3:3:3" line-data="from ._migration import getattr_migration">`_migration`</SwmToken> module and is used to get attributes from the module.

```python
"""The `validators` module is a backport module from V1."""

from ._migration import getattr_migration

__getattr__ = getattr_migration(__name__)
```

---

</SwmSnippet>

# Validators Functions

Validators Functions

<SwmSnippet path="/pydantic/validators.py" line="1">

---

## <SwmToken path="/pydantic/validators.py" pos="3:7:7" line-data="from ._migration import getattr_migration">`getattr_migration`</SwmToken>

The <SwmToken path="/pydantic/validators.py" pos="3:7:7" line-data="from ._migration import getattr_migration">`getattr_migration`</SwmToken> function is used to handle attribute migration in the <SwmToken path="/pydantic/validators.py" pos="1:6:6" line-data="&quot;&quot;&quot;The `validators` module is a backport module from V1.&quot;&quot;&quot;">`validators`</SwmToken> module. It's a part of the backport process from <SwmToken path="/pydantic/validators.py" pos="1:21:21" line-data="&quot;&quot;&quot;The `validators` module is a backport module from V1.&quot;&quot;&quot;">`V1`</SwmToken>.

```python
"""The `validators` module is a backport module from V1."""

from ._migration import getattr_migration

__getattr__ = getattr_migration(__name__)
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

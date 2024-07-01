---
title: Getting Started with Experimental Features
---
Experimental Features in Pydantic are potential new functionalities that are still under development and testing. They are not stable and are subject to change or deprecation. These features are housed in the 'experimental' module of Pydantic. Developers are cautioned to use these features carefully due to their volatile nature.

<SwmSnippet path="/pydantic/experimental/__init__.py" line="1">

---

# Using Experimental Features

This is where the 'experimental' module is defined. A warning is issued to inform users that the module is experimental and its contents are subject to change and deprecation.

```python
"""The "experimental" module of pydantic contains potential new features that are subject to change."""

import warnings

from pydantic.warnings import PydanticExperimentalWarning

warnings.warn(
    'This module is experimental, its contents are subject to change and deprecation.',
    category=PydanticExperimentalWarning,
)
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/experimental/pipeline.py" line="1">

---

# Experimental Pipeline API

The '[pipeline.py](http://pipeline.py)' file contains the experimental pipeline API functionality. This is an example of an experimental feature that is being tested.

```python
"""Experimental pipeline API functionality. Be careful with this API, it's subject to change."""
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

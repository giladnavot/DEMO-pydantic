---
title: >-
  Major Differences between Pydantic V1.10 and V2: The Need for Backward
  Compatibility
---
This document will cover the major differences between Pydantic V1.10 and V2, focusing on the changes that necessitate the backward compatibility layer. The topics covered include:

1. Versioning in Pydantic
2. The role of the backward compatibility layer
3. Changes in Pydantic V2

<SwmSnippet path="/pydantic/version.py" line="7">

---

# Versioning in Pydantic

The current version of Pydantic is defined here as a global constant. This is used throughout the codebase to handle version-specific features and compatibility.

```python
VERSION = '2.8.0a1'
```

---

</SwmSnippet>

<SwmSnippet path="/pyproject.toml" line="211">

---

# The role of the backward compatibility layer

The <SwmToken path="/pyproject.toml" pos="211:0:0" line-data="omit = [&#39;pydantic/deprecated/*&#39;, &#39;pydantic/v1/*&#39;]">`omit`</SwmToken> configuration in <SwmPath>[pyproject.toml](/pyproject.toml)</SwmPath> is used to exclude certain directories from coverage. This includes the <SwmPath>[pydantic/deprecated/](/pydantic/deprecated/)</SwmPath> and <SwmPath>[pydantic/v1/](/pydantic/v1/)</SwmPath> directories, which likely contain code that is only relevant for Pydantic V1.10 and is not compatible with V2.

```toml
omit = ['pydantic/deprecated/*', 'pydantic/v1/*']
```

---

</SwmSnippet>

# Changes in Pydantic V2

The <SwmPath>[HISTORY.md](/HISTORY.md)</SwmPath> file contains a record of changes made in each version of Pydantic. For example, it mentions the addition of <SwmToken path="/pydantic/version.py" pos="55:2:4" line-data="        &#39;pydantic-core version&#39;: pdc.__version__,">`pydantic-core`</SwmToken>` `<SwmToken path="/pydantic/_internal/_decorators.py" pos="224:3:3" line-data="    def build(">`build`</SwmToken>` `<SwmToken path="/pydantic/version.py" pos="53:1:1" line-data="    info = {">`info`</SwmToken> to <SwmToken path="/pydantic/v1/version.py" pos="16:2:4" line-data="def version_info() -&gt; str:">`version_info()`</SwmToken> and the fix for Pydantic dataclasses that use slots with default values.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

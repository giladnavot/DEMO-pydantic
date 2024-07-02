---
title: Getting started with Python 3.8+
---
Python 3.8+ refers to Python version 3.8 and any subsequent versions. It's the minimum Python version required to run the code in this repository. Python 3.8 introduced several new features and optimizations including assignment expressions (walrus operator :=), positional-only parameters, and more precise types. The '+' indicates that the code is compatible with Python 3.8 and all newer versions of Python.

<SwmSnippet path="/pydantic/plugin/_loader.py" line="17">

---

# Python 3.8+ in Pydantic

In this section of the code, we can see that Pydantic is designed to handle plugins. This is a feature that requires Python 3.8+ due to the use of advanced typing annotations. The use of the 'Final' type hint, for example, was introduced in Python 3.8.

```python
# this means that if plugins use pydantic
_loading_plugins: bool = False


def get_plugins() -> Iterable[PydanticPluginProtocol]:
    """Load plugins for Pydantic.

    Inspired by: https://github.com/pytest-dev/pluggy/blob/1.3.0/src/pluggy/_manager.py#L376-L402
    """
    disabled_plugins = os.getenv('PYDANTIC_DISABLE_PLUGINS')
    global _plugins, _loading_plugins
    if _loading_plugins:
        # this happens when plugins themselves use pydantic, we return no plugins
        return ()
    elif disabled_plugins in ('__all__', '1', 'true'):
        return ()
    elif _plugins is None:
        _plugins = {}
        # set _loading_plugins so any plugins that use pydantic don't themselves use plugins
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

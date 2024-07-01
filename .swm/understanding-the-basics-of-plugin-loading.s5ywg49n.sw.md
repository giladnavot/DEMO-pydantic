---
title: Understanding the Basics of Plugin Loading
---
Plugin Loading in Pydantic refers to the process of dynamically loading and managing plugins for the Pydantic library. This is done using Python's importlib_metadata library to discover and load entry points. A global variable `_loading_plugins` is used to avoid recursion and errors during the plugin loading process. If `_loading_plugins` is True, no plugins are returned to avoid plugins that use Pydantic from using plugins themselves. The loaded plugins are stored in a global dictionary `_plugins` for caching and easy access.

<SwmSnippet path="/pydantic/plugin/_loader.py" line="21">

---

## Plugin Loading Mechanism

The `get_plugins` function is responsible for loading all registered Pydantic plugins. It first checks if plugins are currently being loaded or if plugin loading is disabled. If not, it proceeds to load each registered plugin, handling any errors that occur during the loading process.

```python
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
        _loading_plugins = True
        try:
            for dist in importlib_metadata.distributions():
                for entry_point in dist.entry_points:
                    if entry_point.group != PYDANTIC_ENTRY_POINT_GROUP:
                        continue
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/plugin/_loader.py" line="12">

---

## Plugin Registration

Plugins are registered using the entry point group 'pydantic'. The `_plugins` dictionary is used to cache loaded plugins.

```python
PYDANTIC_ENTRY_POINT_GROUP: Final[str] = 'pydantic'

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/plugin/_loader.py" line="16">

---

## Plugin Loading Flag

The `_loading_plugins` flag is used to prevent recursion and errors when plugins themselves use Pydantic. It is set to `True` when plugins are being loaded and reset to `False` once all plugins have been loaded.

```python
# return no plugins while loading plugins to avoid recursion and errors while import plugins
# this means that if plugins use pydantic
_loading_plugins: bool = False
```

---

</SwmSnippet>

# Plugin Loading Function

This section will cover the main function of plugin loading in Pydantic.

<SwmSnippet path="/pydantic/plugin/_loader.py" line="21">

---

## get_plugins

The `get_plugins` function is responsible for loading plugins for Pydantic. It first checks if plugins are currently being loaded, if so, it returns an empty list to avoid recursion. If plugins are disabled, it also returns an empty list. If the plugins have not been loaded yet, it initializes the `_plugins` dictionary and sets `_loading_plugins` to True. It then iterates over all distributions and their entry points, skipping those that are not part of the Pydantic entry point group or are already in the `_plugins` dictionary. If a plugin is not disabled, it attempts to load it and add it to the `_plugins` dictionary. If an error occurs during loading, a warning is issued and the plugin is not installed. After all plugins have been processed, `_loading_plugins` is set back to False and the function returns the values of the `_plugins` dictionary.

```python
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
        _loading_plugins = True
        try:
            for dist in importlib_metadata.distributions():
                for entry_point in dist.entry_points:
                    if entry_point.group != PYDANTIC_ENTRY_POINT_GROUP:
                        continue
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

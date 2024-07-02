---
title: Getting Started with the Loader Mechanism
---
The Loader in this repository refers to the mechanism used to load Pydantic plugins. It uses a global variable <SwmToken path="/pydantic/plugin/_loader.py" pos="18:0:0" line-data="_loading_plugins: bool = False">`_loading_plugins`</SwmToken> to avoid recursion and errors while importing plugins. This is particularly useful when plugins themselves use Pydantic. The Loader also maintains a cache of plugins in the <SwmToken path="/pydantic/plugin/_loader.py" pos="15:0:0" line-data="_plugins: dict[str, PydanticPluginProtocol] | None = None">`_plugins`</SwmToken> dictionary. This cache is populated when the plugins are loaded, and it's used to avoid loading the same plugin multiple times.

<SwmSnippet path="/pydantic/plugin/_loader.py" line="15">

---

# Loader Implementation

The Loader is implemented using a global variable <SwmToken path="/pydantic/plugin/_loader.py" pos="18:0:0" line-data="_loading_plugins: bool = False">`_loading_plugins`</SwmToken> which is used to track whether the loading process is currently ongoing. This is used to prevent recursion and errors during the import of plugins.

```python
_plugins: dict[str, PydanticPluginProtocol] | None = None
# return no plugins while loading plugins to avoid recursion and errors while import plugins
# this means that if plugins use pydantic
_loading_plugins: bool = False
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/plugin/_loader.py" line="26">

---

# Loading Process

The loading process is initiated by setting <SwmToken path="/pydantic/plugin/_loader.py" pos="27:6:6" line-data="    global _plugins, _loading_plugins">`_loading_plugins`</SwmToken> to True. The Loader then iterates over all available distributions, checking their entry points for plugins. If a plugin is found and it is not already loaded, it is loaded and added to the <SwmToken path="/pydantic/plugin/_loader.py" pos="27:3:3" line-data="    global _plugins, _loading_plugins">`_plugins`</SwmToken> dictionary.

```python
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
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

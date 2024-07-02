---
title: Introduction to Data Loading in Pydantic
---
Data Loading in Pydantic refers to the process of loading plugins for Pydantic. This is done by the function <SwmToken path="/pydantic/plugin/_loader.py" pos="21:2:4" line-data="def get_plugins() -&gt; Iterable[PydanticPluginProtocol]:">`get_plugins()`</SwmToken>. The function checks if plugins are currently being loaded, indicated by the <SwmToken path="/pydantic/plugin/_loader.py" pos="18:0:0" line-data="_loading_plugins: bool = False">`_loading_plugins`</SwmToken> variable. If plugins are being loaded, the function returns an empty list to avoid recursion and errors. If not, it sets <SwmToken path="/pydantic/plugin/_loader.py" pos="18:0:0" line-data="_loading_plugins: bool = False">`_loading_plugins`</SwmToken> to True and begins loading the plugins. Once all plugins are loaded, <SwmToken path="/pydantic/plugin/_loader.py" pos="18:0:0" line-data="_loading_plugins: bool = False">`_loading_plugins`</SwmToken> is set back to False.

<SwmSnippet path="/pydantic/plugin/_loader.py" line="21">

---

## Pydantic Plugin Loader

This is the <SwmToken path="/pydantic/plugin/_loader.py" pos="21:2:2" line-data="def get_plugins() -&gt; Iterable[PydanticPluginProtocol]:">`get_plugins`</SwmToken> function, which is responsible for loading Pydantic plugins. It first checks if plugins are currently being loaded to avoid recursion. If not, it checks if plugins are disabled. If plugins are not disabled and not currently being loaded, it proceeds to load the plugins. The loaded plugins are stored in the <SwmToken path="/pydantic/plugin/_loader.py" pos="27:3:3" line-data="    global _plugins, _loading_plugins">`_plugins`</SwmToken> dictionary.

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

<SwmSnippet path="/pydantic/plugin/_loader.py" line="18">

---

## Loading Flag

This is the <SwmToken path="/pydantic/plugin/_loader.py" pos="18:0:0" line-data="_loading_plugins: bool = False">`_loading_plugins`</SwmToken> flag. It is used to prevent recursion and errors during the plugin loading process. If this flag is `True`, the <SwmToken path="/pydantic/plugin/_loader.py" pos="21:2:2" line-data="def get_plugins() -&gt; Iterable[PydanticPluginProtocol]:">`get_plugins`</SwmToken> function will return an empty list, indicating that no plugins are currently being loaded.

```python
_loading_plugins: bool = False
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

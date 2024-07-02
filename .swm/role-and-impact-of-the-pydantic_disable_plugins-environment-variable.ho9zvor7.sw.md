---
title: Role and Impact of the PYDANTIC_DISABLE_PLUGINS Environment Variable
---
This document will cover the role and impact of the <SwmToken path="/pydantic/plugin/_loader.py" pos="26:10:10" line-data="    disabled_plugins = os.getenv(&#39;PYDANTIC_DISABLE_PLUGINS&#39;)">`PYDANTIC_DISABLE_PLUGINS`</SwmToken> environment variable in the DEMO-pydantic repository. We'll cover:

1. What is the <SwmToken path="/pydantic/plugin/_loader.py" pos="26:10:10" line-data="    disabled_plugins = os.getenv(&#39;PYDANTIC_DISABLE_PLUGINS&#39;)">`PYDANTIC_DISABLE_PLUGINS`</SwmToken> environment variable?
2. How is it used in the codebase?
3. What is its functionality and purpose?

# What is the <SwmToken path="/pydantic/plugin/_loader.py" pos="26:10:10" line-data="    disabled_plugins = os.getenv(&#39;PYDANTIC_DISABLE_PLUGINS&#39;)">`PYDANTIC_DISABLE_PLUGINS`</SwmToken> environment variable?

The <SwmToken path="/pydantic/plugin/_loader.py" pos="26:10:10" line-data="    disabled_plugins = os.getenv(&#39;PYDANTIC_DISABLE_PLUGINS&#39;)">`PYDANTIC_DISABLE_PLUGINS`</SwmToken> environment variable is used in the Pydantic library to control the loading of plugins. When this environment variable is set, the loading of plugins is disabled. This can be useful in scenarios where there is a need to avoid recursion and errors while importing plugins.

<SwmSnippet path="/pydantic/plugin/_loader.py" line="26">

---

# How is it used in the codebase?

The <SwmToken path="/pydantic/plugin/_loader.py" pos="26:10:10" line-data="    disabled_plugins = os.getenv(&#39;PYDANTIC_DISABLE_PLUGINS&#39;)">`PYDANTIC_DISABLE_PLUGINS`</SwmToken> environment variable is checked in the <SwmToken path="/pydantic/plugin/_loader.py" pos="21:2:2" line-data="def get_plugins() -&gt; Iterable[PydanticPluginProtocol]:">`get_plugins`</SwmToken> function. If the variable is set (i.e., its value is '**all**', '1', or 'true'), the function returns an empty list, effectively disabling the loading of plugins.

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
```

---

</SwmSnippet>

# What is its functionality and purpose?

The <SwmToken path="/pydantic/plugin/_loader.py" pos="26:10:10" line-data="    disabled_plugins = os.getenv(&#39;PYDANTIC_DISABLE_PLUGINS&#39;)">`PYDANTIC_DISABLE_PLUGINS`</SwmToken> environment variable serves as a switch to control the loading of plugins in the Pydantic library. By setting this environment variable, developers can disable the loading of plugins, which can be useful in avoiding recursion and errors during the import of plugins. This provides a level of control over the plugin system and can help in debugging and testing scenarios.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

---
title: Understanding Pydantic Plugin System
---
The <SwmPath>[pydantic/plugin/](/pydantic/plugin/)</SwmPath> refers to a directory in the Pydantic library that contains code related to the Pydantic plugin system. This system allows developers to extend or modify the functionality of Pydantic by writing their own plugins. The <SwmPath>[pydantic/plugin/](/pydantic/plugin/)</SwmPath> directory contains several Python files that define the plugin system's architecture and functionality. For example, the <SwmPath>[pydantic/plugin/\_loader.py](/pydantic/plugin/_loader.py)</SwmPath> file is responsible for loading and managing plugins, while the <SwmPath>[pydantic/plugin/\_schema_validator.py](/pydantic/plugin/_schema_validator.py)</SwmPath> file defines a protocol for creating new schema validators. The Pydantic plugin system is a powerful tool for developers who want to customize the behavior of Pydantic to suit their specific needs.

<SwmSnippet path="/pydantic/plugin/__init__.py" line="37">

---

# <SwmToken path="/pydantic/plugin/__init__.py" pos="37:2:2" line-data="class PydanticPluginProtocol(Protocol):">`PydanticPluginProtocol`</SwmToken>

The <SwmToken path="/pydantic/plugin/__init__.py" pos="37:2:2" line-data="class PydanticPluginProtocol(Protocol):">`PydanticPluginProtocol`</SwmToken> is an interface that defines the methods that a Pydantic plugin should implement. The most important method is <SwmToken path="/pydantic/plugin/__init__.py" pos="40:3:3" line-data="    def new_schema_validator(">`new_schema_validator`</SwmToken>, which is called every time a new <SwmToken path="/pydantic/plugin/__init__.py" pos="51:28:28" line-data="        &quot;&quot;&quot;This method is called for each plugin every time a new [`SchemaValidator`][pydantic_core.SchemaValidator]">`SchemaValidator`</SwmToken> is created. This method should return an event handler for each of the three validation methods, or `None` if the plugin does not implement that method.

```python
class PydanticPluginProtocol(Protocol):
    """Protocol defining the interface for Pydantic plugins."""

    def new_schema_validator(
        self,
        schema: CoreSchema,
        schema_type: Any,
        schema_type_path: SchemaTypePath,
        schema_kind: SchemaKind,
        config: CoreConfig | None,
        plugin_settings: dict[str, object],
    ) -> tuple[
        ValidatePythonHandlerProtocol | None, ValidateJsonHandlerProtocol | None, ValidateStringsHandlerProtocol | None
    ]:
        """This method is called for each plugin every time a new [`SchemaValidator`][pydantic_core.SchemaValidator]
        is created.

        It should return an event handler for each of the three validation methods, or `None` if the plugin does not
        implement that method.

        Args:
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/plugin/_loader.py" line="21">

---

# <SwmToken path="/pydantic/plugin/_loader.py" pos="21:2:2" line-data="def get_plugins() -&gt; Iterable[PydanticPluginProtocol]:">`get_plugins`</SwmToken> function

The <SwmToken path="/pydantic/plugin/_loader.py" pos="21:2:2" line-data="def get_plugins() -&gt; Iterable[PydanticPluginProtocol]:">`get_plugins`</SwmToken> function is responsible for loading all Pydantic plugins. It checks the environment variable <SwmToken path="/pydantic/plugin/_loader.py" pos="26:10:10" line-data="    disabled_plugins = os.getenv(&#39;PYDANTIC_DISABLE_PLUGINS&#39;)">`PYDANTIC_DISABLE_PLUGINS`</SwmToken> to determine if any plugins should be disabled. If a plugin is not disabled, it is loaded and added to the <SwmToken path="/pydantic/plugin/_loader.py" pos="27:3:3" line-data="    global _plugins, _loading_plugins">`_plugins`</SwmToken> dictionary.

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

<SwmSnippet path="/pydantic/mypy.py" line="130">

---

# <SwmToken path="/pydantic/mypy.py" pos="130:2:2" line-data="class PydanticPlugin(Plugin):">`PydanticPlugin`</SwmToken> class

The <SwmToken path="/pydantic/mypy.py" pos="130:2:2" line-data="class PydanticPlugin(Plugin):">`PydanticPlugin`</SwmToken> class is a concrete implementation of a Pydantic plugin. It provides several hooks that allow it to modify the behavior of Pydantic, such as <SwmToken path="/pydantic/mypy.py" pos="138:3:3" line-data="    def get_base_class_hook(self, fullname: str) -&gt; Callable[[ClassDefContext], bool] | None:">`get_base_class_hook`</SwmToken>, <SwmToken path="/pydantic/mypy.py" pos="147:3:3" line-data="    def get_metaclass_hook(self, fullname: str) -&gt; Callable[[ClassDefContext], None] | None:">`get_metaclass_hook`</SwmToken>, <SwmToken path="/pydantic/v1/mypy.py" pos="127:3:3" line-data="    def get_function_hook(self, fullname: str) -&gt; &#39;Optional[Callable[[FunctionContext], Type]]&#39;:">`get_function_hook`</SwmToken>, and <SwmToken path="/pydantic/v1/mypy.py" pos="133:3:3" line-data="    def get_method_hook(self, fullname: str) -&gt; Optional[Callable[[MethodContext], Type]]:">`get_method_hook`</SwmToken>. These hooks are used to update Pydantic model classes, adjust the return type of certain functions, and more.

```python
class PydanticPlugin(Plugin):
    """The Pydantic mypy plugin."""

    def __init__(self, options: Options) -> None:
        self.plugin_config = PydanticPluginConfig(options)
        self._plugin_data = self.plugin_config.to_data()
        super().__init__(options)

    def get_base_class_hook(self, fullname: str) -> Callable[[ClassDefContext], bool] | None:
        """Update Pydantic model class."""
        sym = self.lookup_fully_qualified(fullname)
        if sym and isinstance(sym.node, TypeInfo):  # pragma: no branch
            # No branching may occur if the mypy cache has not been cleared
            if any(base.fullname == BASEMODEL_FULLNAME for base in sym.node.mro):
                return self._pydantic_model_class_maker_callback
        return None

    def get_metaclass_hook(self, fullname: str) -> Callable[[ClassDefContext], None] | None:
        """Update Pydantic `ModelMetaclass` definition."""
        if fullname == MODEL_METACLASS_FULLNAME:
            return self._pydantic_model_metaclass_marker_callback
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

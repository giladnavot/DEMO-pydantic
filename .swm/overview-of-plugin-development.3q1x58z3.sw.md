---
title: Overview of Plugin Development
---
Plugin Development in the context of the DEMO-pydantic repository refers to the process of creating plugins that can extend or modify the functionality of the Pydantic library. This is achieved through the use of the <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="30:15:15" line-data="    &quot;&quot;&quot;Create a `SchemaValidator` or `PluggableSchemaValidator` if plugins are installed.">`PluggableSchemaValidator`</SwmToken> class, which allows for the creation of custom schema validators. These validators can be used to validate data in different ways, depending on the specific requirements of the plugin. The <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="30:15:15" line-data="    &quot;&quot;&quot;Create a `SchemaValidator` or `PluggableSchemaValidator` if plugins are installed.">`PluggableSchemaValidator`</SwmToken> class uses event handlers for different types of validation (Python, JSON, strings), which are defined by the plugins. This allows for a high degree of flexibility and customization in the validation process.

<SwmSnippet path="/pydantic/plugin/_schema_validator.py" line="21">

---

## Creating a Pluggable Schema Validator

The <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="21:2:2" line-data="def create_schema_validator(">`create_schema_validator`</SwmToken> function is used to create a <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="29:4:4" line-data=") -&gt; SchemaValidator:">`SchemaValidator`</SwmToken> or <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="30:15:15" line-data="    &quot;&quot;&quot;Create a `SchemaValidator` or `PluggableSchemaValidator` if plugins are installed.">`PluggableSchemaValidator`</SwmToken> depending on whether plugins are installed. If plugins are installed, a <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="30:15:15" line-data="    &quot;&quot;&quot;Create a `SchemaValidator` or `PluggableSchemaValidator` if plugins are installed.">`PluggableSchemaValidator`</SwmToken> is created with the provided schema, schema type, and plugin settings.

```python
def create_schema_validator(
    schema: CoreSchema,
    schema_type: Any,
    schema_type_module: str,
    schema_type_name: str,
    schema_kind: SchemaKind,
    config: CoreConfig | None = None,
    plugin_settings: dict[str, Any] | None = None,
) -> SchemaValidator:
    """Create a `SchemaValidator` or `PluggableSchemaValidator` if plugins are installed.

    Returns:
        If plugins are installed then return `PluggableSchemaValidator`, otherwise return `SchemaValidator`.
    """
    from . import SchemaTypePath
    from ._loader import get_plugins

    plugins = get_plugins()
    if plugins:
        return PluggableSchemaValidator(
            schema,
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/plugin/_schema_validator.py" line="53">

---

## Implementing a Plugin

The <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="53:2:2" line-data="class PluggableSchemaValidator:">`PluggableSchemaValidator`</SwmToken> class is a pluggable schema validator that uses plugins to validate data. It initializes with a list of plugins and creates event handlers for each plugin. These handlers are then used to validate Python, JSON, and string data.

```python
class PluggableSchemaValidator:
    """Pluggable schema validator."""

    __slots__ = '_schema_validator', 'validate_json', 'validate_python', 'validate_strings'

    def __init__(
        self,
        schema: CoreSchema,
        schema_type: Any,
        schema_type_path: SchemaTypePath,
        schema_kind: SchemaKind,
        config: CoreConfig | None,
        plugins: Iterable[PydanticPluginProtocol],
        plugin_settings: dict[str, Any],
    ) -> None:
        self._schema_validator = SchemaValidator(schema, config)

        python_event_handlers: list[BaseValidateHandlerProtocol] = []
        json_event_handlers: list[BaseValidateHandlerProtocol] = []
        strings_event_handlers: list[BaseValidateHandlerProtocol] = []
        for plugin in plugins:
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/plugin/_schema_validator.py" line="127">

---

## Filtering Handlers

The <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="127:2:2" line-data="def filter_handlers(handler_cls: BaseValidateHandlerProtocol, method_name: str) -&gt; bool:">`filter_handlers`</SwmToken> function is used to filter out handler methods which are not implemented by the plugin directly. This ensures that only the methods defined in the plugin are used for validation.

```python
def filter_handlers(handler_cls: BaseValidateHandlerProtocol, method_name: str) -> bool:
    """Filter out handler methods which are not implemented by the plugin directly - e.g. are missing
    or are inherited from the protocol.
    """
    handler = getattr(handler_cls, method_name, None)
    if handler is None:
        return False
    elif handler.__module__ == 'pydantic.plugin':
        # this is the original handler, from the protocol due to runtime inheritance
        # we don't want to call it
        return False
    else:
        return True
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

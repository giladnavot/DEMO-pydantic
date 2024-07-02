---
title: Understanding Pydantic Plugin Functionality
---
Plugin functionality in Pydantic refers to the ability to extend or modify the library's behavior using plugins. This is achieved through the PydanticPluginProtocol, which defines the interface for Pydantic plugins. The protocol includes a method called new_schema_validator, which is called for each plugin every time a new SchemaValidator is created. It should return an event handler for each of the three validation methods, or None if the plugin does not implement that method.

The get_plugins function is used to load plugins for Pydantic. It checks if plugins are currently being loaded or if plugins are disabled, and in such cases, it returns no plugins. If plugins are not currently loaded, it loads them from the available distributions and stores them in a global \_plugins dictionary for future use.

The create_schema_validator function creates a SchemaValidator or PluggableSchemaValidator if plugins are installed. It uses the get_plugins function to check if plugins are available. If plugins are available, it creates a PluggableSchemaValidator, otherwise, it creates a SchemaValidator.

<SwmSnippet path="/pydantic/plugin/_loader.py" line="21">

---

# get_plugins function

The `get_plugins` function is used to load the plugins for Pydantic. It checks if plugins are disabled and if not, it loads the plugins from the entry points defined in the installed packages.

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

<SwmSnippet path="/pydantic/plugin/__init__.py" line="40">

---

# new_schema_validator method

The `new_schema_validator` method is part of the PydanticPluginProtocol interface. It is called every time a new SchemaValidator is created. It should return an event handler for each of the three validation methods, or `None` if the plugin does not implement that method.

```python
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
            schema: The schema to validate against.
            schema_type: The original type which the schema was created from, e.g. the model class.
            schema_type_path: Path defining where `schema_type` was defined, or where `TypeAdapter` was called.
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/plugin/_schema_validator.py" line="127">

---

# filter_handlers function

The `filter_handlers` function is used to filter out handler methods which are not implemented by the plugin directly. It checks if the handler method is missing or inherited from the protocol, and if so, it does not call it.

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

<SwmSnippet path="/pydantic/plugin/__init__.py" line="72">

---

# BaseValidateHandlerProtocol class

The `BaseValidateHandlerProtocol` class is a base class for plugin callbacks protocols. It defines the `on_enter`, `on_success`, `on_error`, and `on_exception` methods that can be overridden by the plugins to add custom behavior during the validation process.

```python
class BaseValidateHandlerProtocol(Protocol):
    """Base class for plugin callbacks protocols.

    You shouldn't implement this protocol directly, instead use one of the subclasses with adds the correctly
    typed `on_error` method.
    """

    on_enter: Callable[..., None]
    """`on_enter` is changed to be more specific on all subclasses"""

    def on_success(self, result: Any) -> None:
        """Callback to be notified of successful validation.

        Args:
            result: The result of the validation.
        """
        return

    def on_error(self, error: ValidationError) -> None:
        """Callback to be notified of validation errors.

```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

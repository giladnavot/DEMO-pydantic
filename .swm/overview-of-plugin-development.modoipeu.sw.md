---
title: Overview of Plugin Development
---
Plugin Development in Pydantic involves creating extensions that adhere to the PydanticPluginProtocol. This protocol defines the interface for Pydantic plugins, specifying a method called `new_schema_validator`. This method is called for each plugin every time a new SchemaValidator is created. It should return an event handler for each of the three validation methods - `validate_python`, `validate_json`, `validate_strings`, or `None` if the plugin does not implement that method.

Plugins are loaded using the `get_plugins` function in the `_loader.py` file. This function loads plugins for Pydantic, inspired by the plugin manager in pytest. It checks for disabled plugins and avoids recursion and errors while importing plugins. The loaded plugins are cached in a global `_plugins` dictionary for future use.

Each plugin can define its own validation handlers for Python, JSON, and string data. These handlers are defined by implementing the `ValidatePythonHandlerProtocol`, `ValidateJsonHandlerProtocol`, and `ValidateStringsHandlerProtocol` protocols respectively. Each protocol defines an `on_enter` method that is called at the start of validation, and can also define `on_success`, `on_error`, and `on_exception` methods to handle different validation outcomes.

<SwmSnippet path="/pydantic/plugin/__init__.py" line="37">

---

## PydanticPluginProtocol

The `PydanticPluginProtocol` is the main interface for Pydantic plugins. It defines a method `new_schema_validator` that is called every time a new `SchemaValidator` is created. This method should return an event handler for each of the three validation methods, or `None` if the plugin does not implement that method.

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

<SwmSnippet path="/pydantic/plugin/__init__.py" line="72">

---

## BaseValidateHandlerProtocol

The `BaseValidateHandlerProtocol` is the base class for plugin callbacks protocols. It defines methods like `on_enter`, `on_success`, `on_error`, and `on_exception` that are called during the validation process. Subclasses of this protocol provide more specific implementations of these methods.

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

<SwmSnippet path="/pydantic/plugin/__init__.py" line="107">

---

## ValidatePythonHandlerProtocol, ValidateJsonHandlerProtocol, ValidateStringsHandlerProtocol

These are event handlers for different validation methods. They inherit from `BaseValidateHandlerProtocol` and provide specific implementations of the `on_enter` method. These handlers are notified at the start of validation, upon successful validation, upon validation errors, and upon validation exceptions.

```python
class ValidatePythonHandlerProtocol(BaseValidateHandlerProtocol, Protocol):
    """Event handler for `SchemaValidator.validate_python`."""

    def on_enter(
        self,
        input: Any,
        *,
        strict: bool | None = None,
        from_attributes: bool | None = None,
        context: dict[str, Any] | None = None,
        self_instance: Any | None = None,
    ) -> None:
        """Callback to be notified of validation start, and create an instance of the event handler.

        Args:
            input: The input to be validated.
            strict: Whether to validate the object in strict mode.
            from_attributes: Whether to validate objects as inputs by extracting attributes.
            context: The context to use for validation, this is passed to functional validators.
            self_instance: An instance of a model to set attributes on from validation, this is used when running
                validation from the `__init__` method of a model.
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/plugin/_loader.py" line="21">

---

## get_plugins function

The `get_plugins` function is used to load plugins for Pydantic. It checks for disabled plugins and loads the enabled ones. This function is inspired by the plugin manager in pytest.

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

---
title: Overview of Schema Validation
---
Schema Validation in Pydantic is a process that ensures the data matches the predefined schema. It checks the data type, format, and range of values. This is done using the `SchemaValidator` class, which validates the data against the schema. If plugins are installed, a `PluggableSchemaValidator` is used instead, which allows for additional validation logic provided by the plugins. The validation process can handle different types of data, such as JSON, Python objects, and strings, each having its own validation method.

<SwmSnippet path="/pydantic/plugin/_schema_validator.py" line="21">

---

# SchemaValidator and PluggableSchemaValidator

The `create_schema_validator` function is used to create a `SchemaValidator` or `PluggableSchemaValidator` depending on whether plugins are installed. The `SchemaValidator` is used for basic schema validation, while the `PluggableSchemaValidator` allows for customization of the validation process through plugins.

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

<SwmSnippet path="/pydantic/plugin/_schema_validator.py" line="58">

---

# Validation Process

The `PluggableSchemaValidator` class defines the validation process. It uses event handlers provided by plugins to customize the validation of Python data, JSON data, and string data. These event handlers are invoked at different stages of the validation process.

```python
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
            try:
                p, j, s = plugin.new_schema_validator(
                    schema, schema_type, schema_type_path, schema_kind, config, plugin_settings
                )
            except TypeError as e:  # pragma: no cover
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/plugin/_schema_validator.py" line="95">

---

# Event Handlers

The `build_wrapper` function is used to build a wrapper function for the validation function. This wrapper function invokes the event handlers at the appropriate stages of the validation process.

```python
def build_wrapper(func: Callable[P, R], event_handlers: list[BaseValidateHandlerProtocol]) -> Callable[P, R]:
    if not event_handlers:
        return func
    else:
        on_enters = tuple(h.on_enter for h in event_handlers if filter_handlers(h, 'on_enter'))
        on_successes = tuple(h.on_success for h in event_handlers if filter_handlers(h, 'on_success'))
        on_errors = tuple(h.on_error for h in event_handlers if filter_handlers(h, 'on_error'))
        on_exceptions = tuple(h.on_exception for h in event_handlers if filter_handlers(h, 'on_exception'))

        @functools.wraps(func)
        def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
            for on_enter_handler in on_enters:
                on_enter_handler(*args, **kwargs)

            try:
                result = func(*args, **kwargs)
            except ValidationError as error:
                for on_error_handler in on_errors:
                    on_error_handler(error)
                raise
            except Exception as exception:
```

---

</SwmSnippet>

# Schema Validation Functions

This section discusses the main functions involved in schema validation in Pydantic.

<SwmSnippet path="/pydantic/plugin/_schema_validator.py" line="21">

---

## create_schema_validator

The `create_schema_validator` function is responsible for creating a SchemaValidator. If plugins are installed, it creates a PluggableSchemaValidator instead. This function takes in several parameters including the schema, schema type, and configuration, and returns a SchemaValidator object.

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

## PluggableSchemaValidator

The `PluggableSchemaValidator` class is a schema validator that can be extended with plugins. It has methods for validating Python, JSON, and string data. The class is initialized with a schema, schema type, and configuration among other parameters. It uses the `build_wrapper` function to build the validation methods.

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

<SwmSnippet path="/pydantic/plugin/_schema_validator.py" line="95">

---

## build_wrapper

The `build_wrapper` function is used to build the validation methods for the PluggableSchemaValidator. It takes a function and a list of event handlers as parameters. The function wraps the original function with additional functionality provided by the event handlers. This includes handling events on entering the function, on success, on error, and on exception.

```python
def build_wrapper(func: Callable[P, R], event_handlers: list[BaseValidateHandlerProtocol]) -> Callable[P, R]:
    if not event_handlers:
        return func
    else:
        on_enters = tuple(h.on_enter for h in event_handlers if filter_handlers(h, 'on_enter'))
        on_successes = tuple(h.on_success for h in event_handlers if filter_handlers(h, 'on_success'))
        on_errors = tuple(h.on_error for h in event_handlers if filter_handlers(h, 'on_error'))
        on_exceptions = tuple(h.on_exception for h in event_handlers if filter_handlers(h, 'on_exception'))

        @functools.wraps(func)
        def wrapper(*args: P.args, **kwargs: P.kwargs) -> R:
            for on_enter_handler in on_enters:
                on_enter_handler(*args, **kwargs)

            try:
                result = func(*args, **kwargs)
            except ValidationError as error:
                for on_error_handler in on_errors:
                    on_error_handler(error)
                raise
            except Exception as exception:
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

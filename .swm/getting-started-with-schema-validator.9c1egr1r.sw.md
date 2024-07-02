---
title: Getting Started with Schema Validator
---
A Schema Validator in Pydantic is a tool that validates data against a defined schema. It checks if the data matches the expected format and types defined in the schema. The Schema Validator in this repository is pluggable, meaning it can be extended with plugins to add additional validation logic. It is used to ensure the consistency and correctness of data in the application.

<SwmSnippet path="/pydantic/plugin/_schema_validator.py" line="21">

---

# <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="29:4:4" line-data=") -&gt; SchemaValidator:">`SchemaValidator`</SwmToken> Creation

The <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="21:2:2" line-data="def create_schema_validator(">`create_schema_validator`</SwmToken> function is used to create a <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="29:4:4" line-data=") -&gt; SchemaValidator:">`SchemaValidator`</SwmToken> or <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="30:15:15" line-data="    &quot;&quot;&quot;Create a `SchemaValidator` or `PluggableSchemaValidator` if plugins are installed.">`PluggableSchemaValidator`</SwmToken> if plugins are installed. It takes a schema, schema type, schema type module, schema type name, schema kind, config, and plugin settings as parameters. If plugins are installed, a <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="30:15:15" line-data="    &quot;&quot;&quot;Create a `SchemaValidator` or `PluggableSchemaValidator` if plugins are installed.">`PluggableSchemaValidator`</SwmToken> is returned, otherwise, a <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="29:4:4" line-data=") -&gt; SchemaValidator:">`SchemaValidator`</SwmToken> is returned.

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

# <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="53:2:2" line-data="class PluggableSchemaValidator:">`PluggableSchemaValidator`</SwmToken>

The <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="53:2:2" line-data="class PluggableSchemaValidator:">`PluggableSchemaValidator`</SwmToken> class is a customizable schema validator that allows for plugin integration. It has methods to validate Python data, JSON data, and strings. These methods are built using the <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="95:2:2" line-data="def build_wrapper(func: Callable[P, R], event_handlers: list[BaseValidateHandlerProtocol]) -&gt; Callable[P, R]:">`build_wrapper`</SwmToken> function, which wraps the validation function with event handlers for entering, success, error, and exception events.

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

# Event Handlers

The <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="95:2:2" line-data="def build_wrapper(func: Callable[P, R], event_handlers: list[BaseValidateHandlerProtocol]) -&gt; Callable[P, R]:">`build_wrapper`</SwmToken> function is used to build the validation methods of the <SwmToken path="/pydantic/plugin/_schema_validator.py" pos="30:15:15" line-data="    &quot;&quot;&quot;Create a `SchemaValidator` or `PluggableSchemaValidator` if plugins are installed.">`PluggableSchemaValidator`</SwmToken>. It wraps the validation function with event handlers for entering, success, error, and exception events. These event handlers are provided by the plugins and can be used to customize the validation process.

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

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

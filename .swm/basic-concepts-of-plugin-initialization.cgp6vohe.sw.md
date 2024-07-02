---
title: Basic Concepts of Plugin Initialization
---
Plugin Initialization in Pydantic refers to the process of setting up a new plugin to extend or modify the functionality of the Pydantic library. This is achieved through the implementation of the <SwmToken path="/pydantic/plugin/__init__.py" pos="37:2:2" line-data="class PydanticPluginProtocol(Protocol):">`PydanticPluginProtocol`</SwmToken> interface, which defines the method <SwmToken path="/pydantic/plugin/__init__.py" pos="40:3:3" line-data="    def new_schema_validator(">`new_schema_validator`</SwmToken>. This method is called for each plugin every time a new <SwmToken path="/pydantic/plugin/__init__.py" pos="51:28:28" line-data="        &quot;&quot;&quot;This method is called for each plugin every time a new [`SchemaValidator`][pydantic_core.SchemaValidator]">`SchemaValidator`</SwmToken> is created. It should return an event handler for each of the three validation methods (<SwmToken path="/pydantic/type_adapter.py" pos="349:3:3" line-data="    def validate_python(">`validate_python`</SwmToken>, <SwmToken path="/pydantic/v1/validators.py" pos="534:2:2" line-data="def validate_json(v: Any, config: &#39;BaseConfig&#39;) -&gt; Any:">`validate_json`</SwmToken>, <SwmToken path="/pydantic/type_adapter.py" pos="394:3:3" line-data="    def validate_strings(self, obj: Any, /, *, strict: bool | None = None, context: dict[str, Any] | None = None) -&gt; T:">`validate_strings`</SwmToken>), or `None` if the plugin does not implement that method. This allows the plugin to customize the validation process according to its specific needs.

<SwmSnippet path="/pydantic/plugin/__init__.py" line="37">

---

# <SwmToken path="/pydantic/plugin/__init__.py" pos="37:2:2" line-data="class PydanticPluginProtocol(Protocol):">`PydanticPluginProtocol`</SwmToken>

The <SwmToken path="/pydantic/plugin/__init__.py" pos="37:2:2" line-data="class PydanticPluginProtocol(Protocol):">`PydanticPluginProtocol`</SwmToken> is a protocol that defines the interface for Pydantic plugins. It requires the implementation of the <SwmToken path="/pydantic/plugin/__init__.py" pos="40:3:3" line-data="    def new_schema_validator(">`new_schema_validator`</SwmToken> method, which is called every time a new <SwmToken path="/pydantic/plugin/__init__.py" pos="51:28:28" line-data="        &quot;&quot;&quot;This method is called for each plugin every time a new [`SchemaValidator`][pydantic_core.SchemaValidator]">`SchemaValidator`</SwmToken> is created. This method should return an event handler for each of the three validation methods, or `None` if the plugin does not implement that method.

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

# ValidateHandlerProtocols

The `ValidateHandlerProtocols` are protocols that define the event handlers for the validation methods. They require the implementation of the <SwmToken path="/pydantic/plugin/__init__.py" pos="79:1:1" line-data="    on_enter: Callable[..., None]">`on_enter`</SwmToken> method, which is called at the start of the validation process. There are three different protocols for Python, JSON, and string data validation.

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

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

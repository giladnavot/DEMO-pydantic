---
title: Understanding JSON Processing
---
JSON Processing refers to the operations performed on JSON (JavaScript Object Notation) data. These operations can include parsing, generating, transforming, and querying JSON data. In the context of the DEMO-pydantic repository, JSON processing is primarily used for data validation and serialization. The Pydantic library, which this repository is based on, uses Python type hints to validate data. This data can be in the form of JSON objects, which are parsed and validated against predefined data models. The validated data can then be serialized back into JSON format. The repository also provides functionality for generating JSON schemas for the defined data models.

<SwmSnippet path="/pydantic/json_schema.py" line="69">

---

# JSON Processing in Pydantic

The <SwmToken path="/pydantic/json_schema.py" pos="76:0:0" line-data="JsonSchemaValue = Dict[str, Any]">`JsonSchemaValue`</SwmToken> and <SwmToken path="/pydantic/json_schema.py" pos="81:0:0" line-data="JsonSchemaMode = Literal[&#39;validation&#39;, &#39;serialization&#39;]">`JsonSchemaMode`</SwmToken> type aliases are used to represent a JSON schema value and the mode of a JSON schema respectively. The <SwmToken path="/pydantic/json_schema.py" pos="91:0:0" line-data="_MODE_TITLE_MAPPING: dict[JsonSchemaMode, str] = {&#39;validation&#39;: &#39;Input&#39;, &#39;serialization&#39;: &#39;Output&#39;}">`_MODE_TITLE_MAPPING`</SwmToken> dictionary maps the modes to their corresponding titles.

```python
CoreSchemaOrFieldType = Literal[core_schema.CoreSchemaType, core_schema.CoreSchemaFieldType]
"""
A type alias for defined schema types that represents a union of
`core_schema.CoreSchemaType` and
`core_schema.CoreSchemaFieldType`.
"""

JsonSchemaValue = Dict[str, Any]
"""
A type alias for a JSON schema value. This is a dictionary of string keys to arbitrary JSON values.
"""

JsonSchemaMode = Literal['validation', 'serialization']
"""
A type alias that represents the mode of a JSON schema; either 'validation' or 'serialization'.

For some types, the inputs to validation differ from the outputs of serialization. For example,
computed fields will only be present when serializing, and should not be provided when
validating. This flag provides a way to indicate whether you want the JSON schema required
for validation inputs, or that will be matched by serialization outputs.
"""
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/json_schema.py" line="98">

---

The <SwmToken path="/pydantic/json_schema.py" pos="98:2:2" line-data="def update_json_schema(schema: JsonSchemaValue, updates: dict[str, Any]) -&gt; JsonSchemaValue:">`update_json_schema`</SwmToken> function is used to update a JSON schema <SwmToken path="/pydantic/json_schema.py" pos="99:12:14" line-data="    &quot;&quot;&quot;Update a JSON schema in-place by providing a dictionary of updates.">`in-place`</SwmToken> by providing a dictionary of updates. It sets the provided <SwmToken path="/pydantic/json_schema.py" pos="101:11:13" line-data="    This function sets the provided key-value pairs in the schema and returns the updated schema.">`key-value`</SwmToken> pairs in the schema and returns the updated schema.

```python
def update_json_schema(schema: JsonSchemaValue, updates: dict[str, Any]) -> JsonSchemaValue:
    """Update a JSON schema in-place by providing a dictionary of updates.

    This function sets the provided key-value pairs in the schema and returns the updated schema.

    Args:
        schema: The JSON schema to update.
        updates: A dictionary of key-value pairs to set in the schema.

    Returns:
        The updated JSON schema.
    """
    schema.update(updates)
    return schema
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/json_schema.py" line="232">

---

The <SwmToken path="/pydantic/json_schema.py" pos="232:2:2" line-data="class GenerateJsonSchema:">`GenerateJsonSchema`</SwmToken> class is used to generate JSON schemas based on configured parameters. It provides methods to generate schemas for different data types and handle schema generation warnings.

```python
class GenerateJsonSchema:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/json_schema/#customizing-the-json-schema-generation-process

    A class for generating JSON schemas.

    This class generates JSON schemas based on configured parameters. The default schema dialect
    is [https://json-schema.org/draft/2020-12/schema](https://json-schema.org/draft/2020-12/schema).
    The class uses `by_alias` to configure how fields with
    multiple names are handled and `ref_template` to format reference names.

    Attributes:
        schema_dialect: The JSON schema dialect used to generate the schema. See
            [Declaring a Dialect](https://json-schema.org/understanding-json-schema/reference/schema.html#id4)
            in the JSON Schema documentation for more information about dialects.
        ignored_warning_kinds: Warnings to ignore when generating the schema. `self.render_warning_message` will
            do nothing if its argument `kind` is in `ignored_warning_kinds`;
            this value can be modified on subclasses to easily control which warnings are emitted.
        by_alias: Whether to use field aliases when generating the schema.
        ref_template: The format string used when generating reference names.
        core_to_json_refs: A mapping of core refs to JSON refs.
        core_to_defs_refs: A mapping of core refs to definition refs.
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/json_schema.py" line="2231">

---

The <SwmToken path="/tests/test_main.py" pos="981:1:1" line-data="            model_json_schema = (">`model_json_schema`</SwmToken> function is a utility function to generate a JSON Schema for a model.

```python
    cls: type[BaseModel] | type[PydanticDataclass],
    by_alias: bool = True,
    ref_template: str = DEFAULT_REF_TEMPLATE,
    schema_generator: type[GenerateJsonSchema] = GenerateJsonSchema,
    mode: JsonSchemaMode = 'validation',
) -> dict[str, Any]:
    """Utility function to generate a JSON Schema for a model.
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/json_schema.py" line="2268">

---

The <SwmToken path="/pydantic/json_schema.py" pos="2267:2:2" line-data="def models_json_schema(">`models_json_schema`</SwmToken> function is a utility function to generate a JSON Schema for multiple models.

```python
    models: Sequence[tuple[type[BaseModel] | type[PydanticDataclass], JsonSchemaMode]],
    *,
    by_alias: bool = True,
    title: str | None = None,
    description: str | None = None,
    ref_template: str = DEFAULT_REF_TEMPLATE,
    schema_generator: type[GenerateJsonSchema] = GenerateJsonSchema,
) -> tuple[dict[tuple[type[BaseModel] | type[PydanticDataclass], JsonSchemaMode], JsonSchemaValue], JsonSchemaValue]:
    """Utility function to generate a JSON Schema for multiple models.
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

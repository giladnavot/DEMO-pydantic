---
title: Understanding JSON Schema
---
JSON Schema is a vocabulary that allows you to annotate and validate JSON documents. It provides a contract for the JSON data required by a given application and how that data can be modified. In the context of Pydantic, JSON Schema is used for data validation and serialization. It helps in defining the structure and type of data in a Pydantic model, ensuring that the data adheres to the predefined schema. Pydantic's <SwmToken path="/pydantic/json_schema.py" pos="232:2:2" line-data="class GenerateJsonSchema:">`GenerateJsonSchema`</SwmToken> class is used to generate these JSON schemas based on configured parameters.

The <SwmToken path="/pydantic/json_schema.py" pos="232:2:2" line-data="class GenerateJsonSchema:">`GenerateJsonSchema`</SwmToken> class in Pydantic uses the JSON Schema standard to generate schemas for Pydantic models. It uses the <SwmToken path="/pydantic/json_schema.py" pos="239:8:8" line-data="    The class uses `by_alias` to configure how fields with">`by_alias`</SwmToken> parameter to configure how fields with multiple names are handled and <SwmToken path="/pydantic/json_schema.py" pos="240:12:12" line-data="    multiple names are handled and `ref_template` to format reference names.">`ref_template`</SwmToken> to format reference names. The class also provides methods to generate schemas for various data types like string, integer, boolean, etc.

The <SwmToken path="/pydantic/json_schema.py" pos="2230:2:2" line-data="def model_json_schema(">`model_json_schema`</SwmToken> function is a utility function that generates a JSON Schema for a Pydantic model. It takes in parameters like the model class, whether to use field aliases, the template for generating JSON Schema references, and the mode of schema generation (validation or serialization).

The <SwmToken path="/pydantic/json_schema.py" pos="2267:2:2" line-data="def models_json_schema(">`models_json_schema`</SwmToken> function is another utility function that generates a JSON Schema for multiple Pydantic models. It returns a tuple where the first element is a dictionary mapping the input keys to the JSON schema and the second element is a JSON schema containing all definitions referenced in the first element.

<SwmSnippet path="/pydantic/json_schema.py" line="232">

---

# <SwmToken path="/pydantic/json_schema.py" pos="232:2:2" line-data="class GenerateJsonSchema:">`GenerateJsonSchema`</SwmToken> Class

The <SwmToken path="/pydantic/json_schema.py" pos="232:2:2" line-data="class GenerateJsonSchema:">`GenerateJsonSchema`</SwmToken> class is used to generate JSON schemas based on configured parameters. It has various methods such as <SwmToken path="/pydantic/json_schema.py" pos="445:3:3" line-data="    def generate_inner(self, schema: CoreSchemaOrField) -&gt; JsonSchemaValue:  # noqa: C901">`generate_inner`</SwmToken>, <SwmToken path="/pydantic/json_schema.py" pos="293:9:9" line-data="        self._schema_type_to_method = self.build_schema_type_to_method()">`build_schema_type_to_method`</SwmToken>, and <SwmToken path="/pydantic/json_schema.py" pos="344:3:3" line-data="    def generate_definitions(">`generate_definitions`</SwmToken> to assist in the generation of JSON schemas.

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

<SwmSnippet path="/pydantic/json_schema.py" line="2230">

---

# <SwmToken path="/pydantic/json_schema.py" pos="2230:2:2" line-data="def model_json_schema(">`model_json_schema`</SwmToken> Function

The <SwmToken path="/pydantic/json_schema.py" pos="2230:2:2" line-data="def model_json_schema(">`model_json_schema`</SwmToken> function is a utility function to generate a JSON Schema for a model. It takes in parameters such as the model class, whether to use field aliases, the reference template, the schema generator, and the mode for generating the JSON Schema.

```python
def model_json_schema(
    cls: type[BaseModel] | type[PydanticDataclass],
    by_alias: bool = True,
    ref_template: str = DEFAULT_REF_TEMPLATE,
    schema_generator: type[GenerateJsonSchema] = GenerateJsonSchema,
    mode: JsonSchemaMode = 'validation',
) -> dict[str, Any]:
    """Utility function to generate a JSON Schema for a model.

    Args:
        cls: The model class to generate a JSON Schema for.
        by_alias: If `True` (the default), fields will be serialized according to their alias.
            If `False`, fields will be serialized according to their attribute name.
        ref_template: The template to use for generating JSON Schema references.
        schema_generator: The class to use for generating the JSON Schema.
        mode: The mode to use for generating the JSON Schema. It can be one of the following:

            - 'validation': Generate a JSON Schema for validating data.
            - 'serialization': Generate a JSON Schema for serializing data.

    Returns:
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/json_schema.py" line="1134">

---

# <SwmToken path="/pydantic/json_schema.py" pos="1134:3:3" line-data="    def tagged_union_schema(self, schema: core_schema.TaggedUnionSchema) -&gt; JsonSchemaValue:">`tagged_union_schema`</SwmToken> Function

The <SwmToken path="/pydantic/json_schema.py" pos="1134:3:3" line-data="    def tagged_union_schema(self, schema: core_schema.TaggedUnionSchema) -&gt; JsonSchemaValue:">`tagged_union_schema`</SwmToken> function generates a JSON schema that matches a schema that allows values matching any of the given schemas, where the schemas are tagged with a discriminator field that indicates which schema should be used to validate the value.

```python
    def tagged_union_schema(self, schema: core_schema.TaggedUnionSchema) -> JsonSchemaValue:
        """Generates a JSON schema that matches a schema that allows values matching any of the given schemas, where
        the schemas are tagged with a discriminator field that indicates which schema should be used to validate
        the value.

        Args:
            schema: The core schema.

        Returns:
            The generated JSON schema.
        """
        generated: dict[str, JsonSchemaValue] = {}
        for k, v in schema['choices'].items():
            if isinstance(k, Enum):
                k = k.value
            try:
                # Use str(k) since keys must be strings for json; while not technically correct,
                # it's the closest that can be represented in valid JSON
                generated[str(k)] = self.generate_inner(v).copy()
            except PydanticOmit:
                continue
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/json_schema.py" line="76">

---

# <SwmToken path="/pydantic/json_schema.py" pos="76:0:0" line-data="JsonSchemaValue = Dict[str, Any]">`JsonSchemaValue`</SwmToken> Constant

The <SwmToken path="/pydantic/json_schema.py" pos="76:0:0" line-data="JsonSchemaValue = Dict[str, Any]">`JsonSchemaValue`</SwmToken> constant is a type alias for a JSON schema value. It represents a dictionary of string keys to arbitrary JSON values.

```python
JsonSchemaValue = Dict[str, Any]
"""
```

---

</SwmSnippet>

# JSON Schema Generation Functions

This section will cover the main functions used in Pydantic for generating JSON schemas.

<SwmSnippet path="/pydantic/json_schema.py" line="445">

---

## Generate Inner

The <SwmToken path="/pydantic/json_schema.py" pos="445:3:3" line-data="    def generate_inner(self, schema: CoreSchemaOrField) -&gt; JsonSchemaValue:  # noqa: C901">`generate_inner`</SwmToken> function is used to generate a JSON schema for a given core schema. It checks if a schema with the same <SwmToken path="/pydantic/json_schema.py" pos="454:15:15" line-data="        # If a schema with the same CoreRef has been handled, just return a reference to it">`CoreRef`</SwmToken> has been handled, and if so, it returns a reference to it. Otherwise, it generates the JSON schema, accounting for the <SwmToken path="/pydantic/json_schema.py" pos="463:18:18" line-data="        # Generate the JSON schema, accounting for the json_schema_override and core_schema_override">`json_schema_override`</SwmToken> and <SwmToken path="/pydantic/json_schema.py" pos="463:22:22" line-data="        # Generate the JSON schema, accounting for the json_schema_override and core_schema_override">`core_schema_override`</SwmToken>.

```python
    def generate_inner(self, schema: CoreSchemaOrField) -> JsonSchemaValue:  # noqa: C901
        """Generates a JSON schema for a given core schema.

        Args:
            schema: The given core schema.

        Returns:
            The generated JSON schema.
        """
        # If a schema with the same CoreRef has been handled, just return a reference to it
        # Note that this assumes that it will _never_ be the case that the same CoreRef is used
        # on types that should have different JSON schemas
        if 'ref' in schema:
            core_ref = CoreRef(schema['ref'])  # type: ignore[typeddict-item]
            core_mode_ref = (core_ref, self.mode)
            if core_mode_ref in self.core_to_defs_refs and self.core_to_defs_refs[core_mode_ref] in self.definitions:
                return {'$ref': self.core_to_json_refs[core_mode_ref]}

        # Generate the JSON schema, accounting for the json_schema_override and core_schema_override
        metadata_handler = _core_metadata.CoreMetadataHandler(schema)

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/json_schema.py" line="344">

---

## Generate Definitions

The <SwmToken path="/pydantic/json_schema.py" pos="344:3:3" line-data="    def generate_definitions(">`generate_definitions`</SwmToken> function generates JSON schema definitions from a list of core schemas. It pairs the generated definitions with a mapping that links the input keys to the definition references. It also checks if the JSON schema generator has already been used to generate a JSON schema.

```python
    def generate_definitions(
        self, inputs: Sequence[tuple[JsonSchemaKeyT, JsonSchemaMode, core_schema.CoreSchema]]
    ) -> tuple[dict[tuple[JsonSchemaKeyT, JsonSchemaMode], JsonSchemaValue], dict[DefsRef, JsonSchemaValue]]:
        """Generates JSON schema definitions from a list of core schemas, pairing the generated definitions with a
        mapping that links the input keys to the definition references.

        Args:
            inputs: A sequence of tuples, where:

                - The first element is a JSON schema key type.
                - The second element is the JSON mode: either 'validation' or 'serialization'.
                - The third element is a core schema.

        Returns:
            A tuple where:

                - The first element is a dictionary whose keys are tuples of JSON schema key type and JSON mode, and
                    whose values are the JSON schema corresponding to that pair of inputs. (These schemas may have
                    JsonRef references to definitions that are defined in the second returned element.)
                - The second element is a dictionary whose keys are definition references for the JSON schemas
                    from the first returned element, and whose values are the actual JSON schema definitions.
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/json_schema.py" line="2230">

---

## Model JSON Schema

The <SwmToken path="/pydantic/json_schema.py" pos="2230:2:2" line-data="def model_json_schema(">`model_json_schema`</SwmToken> function is a utility function to generate a JSON Schema for a model. It takes in a model class and generates a JSON schema for it. It also checks if the JSON schema generator has already been used to generate a JSON schema.

```python
def model_json_schema(
    cls: type[BaseModel] | type[PydanticDataclass],
    by_alias: bool = True,
    ref_template: str = DEFAULT_REF_TEMPLATE,
    schema_generator: type[GenerateJsonSchema] = GenerateJsonSchema,
    mode: JsonSchemaMode = 'validation',
) -> dict[str, Any]:
    """Utility function to generate a JSON Schema for a model.

    Args:
        cls: The model class to generate a JSON Schema for.
        by_alias: If `True` (the default), fields will be serialized according to their alias.
            If `False`, fields will be serialized according to their attribute name.
        ref_template: The template to use for generating JSON Schema references.
        schema_generator: The class to use for generating the JSON Schema.
        mode: The mode to use for generating the JSON Schema. It can be one of the following:

            - 'validation': Generate a JSON Schema for validating data.
            - 'serialization': Generate a JSON Schema for serializing data.

    Returns:
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/json_schema.py" line="2267">

---

## Models JSON Schema

The <SwmToken path="/pydantic/json_schema.py" pos="2267:2:2" line-data="def models_json_schema(">`models_json_schema`</SwmToken> function is a utility function to generate a JSON Schema for multiple models. It takes in a sequence of tuples of the form (model, mode) and generates a JSON schema for each model in the sequence.

```python
def models_json_schema(
    models: Sequence[tuple[type[BaseModel] | type[PydanticDataclass], JsonSchemaMode]],
    *,
    by_alias: bool = True,
    title: str | None = None,
    description: str | None = None,
    ref_template: str = DEFAULT_REF_TEMPLATE,
    schema_generator: type[GenerateJsonSchema] = GenerateJsonSchema,
) -> tuple[dict[tuple[type[BaseModel] | type[PydanticDataclass], JsonSchemaMode], JsonSchemaValue], JsonSchemaValue]:
    """Utility function to generate a JSON Schema for multiple models.

    Args:
        models: A sequence of tuples of the form (model, mode).
        by_alias: Whether field aliases should be used as keys in the generated JSON Schema.
        title: The title of the generated JSON Schema.
        description: The description of the generated JSON Schema.
        ref_template: The reference template to use for generating JSON Schema references.
        schema_generator: The schema generator to use for generating the JSON Schema.

    Returns:
        A tuple where:
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

---
title: Pydantics Data Validation Flow
---
This document will cover the process of Pydantic's data validation flow, which includes:

1. Initiating the validator
2. Initializing core attributes
3. Generating and cleaning the schema
4. Applying discriminators to the schema
5. Converting the schema
6. Generating the final schema.

```mermaid
graph TD;
  _wrapper2:::mainFlowStyle --> validator
  validator:::mainFlowStyle --> _init_core_attrs
  _init_core_attrs:::mainFlowStyle --> _get_schema
subgraph pydantic/_internal
  _get_schema:::mainFlowStyle --> clean_schema
end
subgraph pydantic/_internal
  clean_schema:::mainFlowStyle --> simplify_schema_references
end
subgraph pydantic/_internal
  clean_schema:::mainFlowStyle --> collect_definitions
end
subgraph pydantic/_internal
  clean_schema:::mainFlowStyle --> apply_discriminators
end
  simplify_schema_references --> definitions_schema
  definitions_schema --> generate_inner
  collect_definitions --> definitions_schema
subgraph pydantic/_internal
  apply_discriminators:::mainFlowStyle --> collect_definitions
end
subgraph pydantic/_internal
  apply_discriminators:::mainFlowStyle --> apply_discriminator
end
  apply_discriminator:::mainFlowStyle --> _convert_schema
subgraph pydantic/_internal
  apply_discriminator:::mainFlowStyle --> apply
end
  _convert_schema --> union_schema
  union_schema --> generate_inner
  apply:::mainFlowStyle --> nullable_schema
subgraph pydantic/_internal
  apply:::mainFlowStyle --> _apply_to_root
end
  nullable_schema --> generate_inner
  generate_inner --> convert_to_all_of
  _apply_to_root:::mainFlowStyle --> union_schema
  _apply_to_root:::mainFlowStyle --> tagged_union_schema
  tagged_union_schema:::mainFlowStyle --> ...

 classDef mainFlowStyle color:#000000,fill:#7CB9F4
  classDef rootsStyle color:#000000,fill:#00FFF4
```

<SwmSnippet path="/pydantic/type_adapter.py" line="314">

---

# Initiating the validator

The function `validator` is used to initiate the Pydantic-core SchemaValidator used to validate instances of the model.

```python
    def validator(self) -> SchemaValidator:
        """The pydantic-core SchemaValidator used to validate instances of the model."""
        if not isinstance(self._validator, SchemaValidator):
            self._init_core_attrs(rebuild_mocks=True)  # Do not expose MockValSer from public function
        assert isinstance(self._validator, SchemaValidator)
        return self._validator
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/type_adapter.py" line="275">

---

# Initializing core attributes

The function `_init_core_attrs` is used to initialize core attributes of the model. If the attributes are not present, they are created.

```python
    def _init_core_attrs(self, rebuild_mocks: bool) -> None:
        try:
            self._core_schema = _getattr_no_parents(self._type, '__pydantic_core_schema__')
            self._validator = _getattr_no_parents(self._type, '__pydantic_validator__')
            self._serializer = _getattr_no_parents(self._type, '__pydantic_serializer__')
        except AttributeError:
            config_wrapper = _config.ConfigWrapper(self._config)
            core_config = config_wrapper.core_config(None)

            self._core_schema = _get_schema(self._type, config_wrapper, parent_depth=self._parent_depth)
            self._validator = create_schema_validator(
                schema=self._core_schema,
                schema_type=self._type,
                schema_type_module=self._module_name,
                schema_type_name=str(self._type),
                schema_kind='TypeAdapter',
                config=core_config,
                plugin_settings=config_wrapper.plugin_settings,
            )
            self._serializer = SchemaSerializer(self._core_schema, core_config)

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/type_adapter.py" line="54">

---

# Generating and cleaning the schema

The function `_get_schema` is used to generate the schema for the model. The schema is then cleaned using the `clean_schema` function.

````python
def _get_schema(type_: Any, config_wrapper: _config.ConfigWrapper, parent_depth: int) -> CoreSchema:
    """`BaseModel` uses its own `__module__` to find out where it was defined
    and then looks for symbols to resolve forward references in those globals.
    On the other hand this function can be called with arbitrary objects,
    including type aliases, where `__module__` (always `typing.py`) is not useful.
    So instead we look at the globals in our parent stack frame.

    This works for the case where this function is called in a module that
    has the target of forward references in its scope, but
    does not always work for more complex cases.

    For example, take the following:

    a.py
    ```python
    from typing import Dict, List

    IntList = List[int]
    OuterDict = Dict[str, 'IntList']
    ```

````

---

</SwmSnippet>

<SwmSnippet path="/pydantic/_internal/_generate_schema.py" line="446">

---

# Applying discriminators to the schema

The function `clean_schema` is used to apply discriminators to the schema. This is done by calling the `apply_discriminators` function.

```python
    def clean_schema(self, schema: CoreSchema) -> CoreSchema:
        schema = self.collect_definitions(schema)
        schema = simplify_schema_references(schema)
        if collect_invalid_schemas(schema):
            raise self.CollectedInvalid()
        schema = _discriminated_union.apply_discriminators(schema)
        schema = validate_core_schema(schema)
        return schema
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/types.py" line="2835">

---

# Converting the schema

The function `_convert_schema` is used to convert the schema into a TaggedUnionSchema.

```python
    def _convert_schema(self, original_schema: core_schema.CoreSchema) -> core_schema.TaggedUnionSchema:
        if original_schema['type'] != 'union':
            # This likely indicates that the schema was a single-item union that was simplified.
            # In this case, we do the same thing we do in
            # `pydantic._internal._discriminated_union._ApplyInferredDiscriminator._apply_to_root`, namely,
            # package the generated schema back into a single-item union.
            original_schema = core_schema.union_schema([original_schema])

        tagged_union_choices = {}
        for i, choice in enumerate(original_schema['choices']):
            tag = None
            if isinstance(choice, tuple):
                choice, tag = choice
            metadata = choice.get('metadata')
            if metadata is not None:
                metadata_tag = metadata.get(_core_utils.TAGGED_UNION_TAG_KEY)
                if metadata_tag is not None:
                    tag = metadata_tag
            if tag is None:
                raise PydanticUserError(
                    f'`Tag` not provided for choice {choice} used with `Discriminator`',
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/json_schema.py" line="445">

---

# Generating the final schema

The function `generate_inner` is used to generate the final JSON schema based on the input schema.

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

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="flows"><sup>Powered by [Swimm](/)</sup></SwmMeta>

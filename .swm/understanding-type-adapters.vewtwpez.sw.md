---
title: Understanding Type Adapters
---
Type Adapters in Pydantic are a flexible way to perform validation and serialization based on a Python type. They expose some of the functionality from <SwmToken path="/pydantic/type_adapter.py" pos="55:5:5" line-data="    &quot;&quot;&quot;`BaseModel` uses its own `__module__` to find out where it was defined">`BaseModel`</SwmToken> instance methods for types that do not have such methods, such as dataclasses, primitive types, and more. However, <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> instances are not types themselves and cannot be used as type annotations for fields. They are initialized with a type, a configuration, a parent depth, and a module. The type is the type associated with the <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken>, the configuration should be a dictionary conforming to <SwmToken path="/pydantic/type_adapter.py" pos="161:13:13" line-data="    [`defer_build=True`][pydantic.config.ConfigDict.defer_build] setting in the">`ConfigDict`</SwmToken>, the parent depth is the depth at which to search the parent namespace to construct the local namespace, and the module is the module that passes to the plugin if provided.

<SwmSnippet path="/pydantic/type_adapter.py" line="150">

---

# <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> Class

The <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> class is defined here. It is initialized with a type and optionally a configuration dictionary and a parent depth. The class has methods for validation (<SwmToken path="/pydantic/type_adapter.py" pos="349:3:3" line-data="    def validate_python(">`validate_python`</SwmToken>, <SwmToken path="/pydantic/type_adapter.py" pos="376:3:3" line-data="    def validate_json(">`validate_json`</SwmToken>, <SwmToken path="/pydantic/type_adapter.py" pos="394:3:3" line-data="    def validate_strings(self, obj: Any, /, *, strict: bool | None = None, context: dict[str, Any] | None = None) -&gt; T:">`validate_strings`</SwmToken>), serialization (<SwmToken path="/pydantic/type_adapter.py" pos="421:3:3" line-data="    def dump_python(">`dump_python`</SwmToken>, <SwmToken path="/pydantic/type_adapter.py" pos="474:3:3" line-data="    def dump_json(">`dump_json`</SwmToken>), and schema generation (<SwmToken path="/pydantic/type_adapter.py" pos="529:3:3" line-data="    def json_schema(">`json_schema`</SwmToken>).

```python
class TypeAdapter(Generic[T]):
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/type_adapter/

    Type adapters provide a flexible way to perform validation and serialization based on a Python type.

    A `TypeAdapter` instance exposes some of the functionality from `BaseModel` instance methods
    for types that do not have such methods (such as dataclasses, primitive types, and more).

    **Note:** `TypeAdapter` instances are not types, and cannot be used as type annotations for fields.

    **Note:** By default, `TypeAdapter` does not respect the
    [`defer_build=True`][pydantic.config.ConfigDict.defer_build] setting in the
    [`model_config`][pydantic.BaseModel.model_config] or in the `TypeAdapter` constructor `config`. You need to also
    explicitly set [`experimental_defer_build_mode=('model', 'type_adapter')`][pydantic.config.ConfigDict.experimental_defer_build_mode] of the
    config to defer the model validator and serializer construction. Thus, this feature is opt-in to ensure backwards
    compatibility.

    Attributes:
        core_schema: The core schema for the type.
        validator (SchemaValidator): The schema validator for the type.
        serializer: The schema serializer for the type.
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/type_adapter.py" line="196">

---

# <SwmToken path="/pydantic/type_adapter.py" pos="204:8:8" line-data="        &quot;&quot;&quot;Initializes the TypeAdapter object.">`TypeAdapter`</SwmToken> Initialization

The <SwmToken path="/pydantic/type_adapter.py" pos="196:3:3" line-data="    def __init__(">`__init__`</SwmToken> method of the <SwmToken path="/pydantic/type_adapter.py" pos="204:8:8" line-data="        &quot;&quot;&quot;Initializes the TypeAdapter object.">`TypeAdapter`</SwmToken> class is defined here. It sets up the <SwmToken path="/pydantic/type_adapter.py" pos="204:8:8" line-data="        &quot;&quot;&quot;Initializes the TypeAdapter object.">`TypeAdapter`</SwmToken> with the provided type and configuration. It also checks if the type has a config and raises an error if a config is provided in such a case.

```python
    def __init__(
        self,
        type: Any,
        *,
        config: ConfigDict | None = None,
        _parent_depth: int = 2,
        module: str | None = None,
    ) -> None:
        """Initializes the TypeAdapter object.

        Args:
            type: The type associated with the `TypeAdapter`.
            config: Configuration for the `TypeAdapter`, should be a dictionary conforming to [`ConfigDict`][pydantic.config.ConfigDict].
            _parent_depth: depth at which to search the parent namespace to construct the local namespace.
            module: The module that passes to plugin if provided.

        !!! note
            You cannot use the `config` argument when instantiating a `TypeAdapter` if the type you're using has its own
            config that cannot be overridden (ex: `BaseModel`, `TypedDict`, and `dataclass`). A
            [`type-adapter-config-unused`](../errors/usage_errors.md#type-adapter-config-unused) error will be raised in this case.

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/type_adapter.py" line="349">

---

# <SwmToken path="/pydantic/type_adapter.py" pos="367:6:6" line-data="            When using `TypeAdapter` with a Pydantic `dataclass`, the use of the `from_attributes`">`TypeAdapter`</SwmToken> Validation

The <SwmToken path="/pydantic/type_adapter.py" pos="349:3:3" line-data="    def validate_python(">`validate_python`</SwmToken> method of the <SwmToken path="/pydantic/type_adapter.py" pos="367:6:6" line-data="            When using `TypeAdapter` with a Pydantic `dataclass`, the use of the `from_attributes`">`TypeAdapter`</SwmToken> class is defined here. It validates a Python object against the model. Similar methods exist for validating JSON (<SwmToken path="/pydantic/type_adapter.py" pos="376:3:3" line-data="    def validate_json(">`validate_json`</SwmToken>) and strings (<SwmToken path="/pydantic/type_adapter.py" pos="394:3:3" line-data="    def validate_strings(self, obj: Any, /, *, strict: bool | None = None, context: dict[str, Any] | None = None) -&gt; T:">`validate_strings`</SwmToken>).

```python
    def validate_python(
        self,
        object: Any,
        /,
        *,
        strict: bool | None = None,
        from_attributes: bool | None = None,
        context: dict[str, Any] | None = None,
    ) -> T:
        """Validate a Python object against the model.

        Args:
            object: The Python object to validate against the model.
            strict: Whether to strictly check types.
            from_attributes: Whether to extract data from object attributes.
            context: Additional context to pass to the validator.

        !!! note
            When using `TypeAdapter` with a Pydantic `dataclass`, the use of the `from_attributes`
            argument is not supported.

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/type_adapter.py" line="421">

---

# <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> Serialization

The <SwmToken path="/pydantic/type_adapter.py" pos="421:3:3" line-data="    def dump_python(">`dump_python`</SwmToken> method of the <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> class is defined here. It dumps an instance of the adapted type to a Python object. A similar method exists for dumping to JSON (<SwmToken path="/pydantic/type_adapter.py" pos="474:3:3" line-data="    def dump_json(">`dump_json`</SwmToken>).

```python
    def dump_python(
        self,
        instance: T,
        /,
        *,
        mode: Literal['json', 'python'] = 'python',
        include: IncEx | None = None,
        exclude: IncEx | None = None,
        by_alias: bool = False,
        exclude_unset: bool = False,
        exclude_defaults: bool = False,
        exclude_none: bool = False,
        round_trip: bool = False,
        warnings: bool | Literal['none', 'warn', 'error'] = True,
        serialize_as_any: bool = False,
        context: dict[str, Any] | None = None,
    ) -> Any:
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/type_adapter.py" line="529">

---

# <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> Schema Generation

The <SwmToken path="/pydantic/type_adapter.py" pos="529:3:3" line-data="    def json_schema(">`json_schema`</SwmToken> method of the <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> class is defined here. It generates a JSON schema for the adapted type.

```python
    def json_schema(
        self,
        *,
        by_alias: bool = True,
        ref_template: str = DEFAULT_REF_TEMPLATE,
        schema_generator: type[GenerateJsonSchema] = GenerateJsonSchema,
        mode: JsonSchemaMode = 'validation',
    ) -> dict[str, Any]:
```

---

</SwmSnippet>

# <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> Functions

This section covers the main functions of the <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> class in the Pydantic library.

<SwmSnippet path="/pydantic/type_adapter.py" line="54">

---

## <SwmToken path="/pydantic/type_adapter.py" pos="54:2:2" line-data="def _get_schema(type_: Any, config_wrapper: _config.ConfigWrapper, parent_depth: int) -&gt; CoreSchema:">`_get_schema`</SwmToken>

The <SwmToken path="/pydantic/type_adapter.py" pos="54:2:2" line-data="def _get_schema(type_: Any, config_wrapper: _config.ConfigWrapper, parent_depth: int) -&gt; CoreSchema:">`_get_schema`</SwmToken> function is used to generate a schema for a given type. It takes into account the configuration wrapper and the parent depth. This function is used internally by the <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> class to generate the schema for the type it is adapting.

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

<SwmSnippet path="/pydantic/type_adapter.py" line="275">

---

## <SwmToken path="/pydantic/type_adapter.py" pos="275:3:3" line-data="    def _init_core_attrs(self, rebuild_mocks: bool) -&gt; None:">`_init_core_attrs`</SwmToken>

The <SwmToken path="/pydantic/type_adapter.py" pos="275:3:3" line-data="    def _init_core_attrs(self, rebuild_mocks: bool) -&gt; None:">`_init_core_attrs`</SwmToken> function is used to initialize the core attributes of the <SwmToken path="/pydantic/type_adapter.py" pos="290:4:4" line-data="                schema_kind=&#39;TypeAdapter&#39;,">`TypeAdapter`</SwmToken> instance. These include the core schema, the validator, and the serializer. If these attributes are not already set, they are generated using the <SwmToken path="/pydantic/type_adapter.py" pos="284:7:7" line-data="            self._core_schema = _get_schema(self._type, config_wrapper, parent_depth=self._parent_depth)">`_get_schema`</SwmToken> function and the <SwmToken path="/pydantic/type_adapter.py" pos="285:7:7" line-data="            self._validator = create_schema_validator(">`create_schema_validator`</SwmToken> and <SwmToken path="/pydantic/type_adapter.py" pos="294:7:7" line-data="            self._serializer = SchemaSerializer(self._core_schema, core_config)">`SchemaSerializer`</SwmToken> classes.

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

<SwmSnippet path="/pydantic/type_adapter.py" line="349">

---

## <SwmToken path="/pydantic/type_adapter.py" pos="349:3:3" line-data="    def validate_python(">`validate_python`</SwmToken>

The <SwmToken path="/pydantic/type_adapter.py" pos="349:3:3" line-data="    def validate_python(">`validate_python`</SwmToken> function is used to validate a Python object against the model. It uses the <SwmToken path="/pydantic/type_adapter.py" pos="169:4:4" line-data="        validator (SchemaValidator): The schema validator for the type.">`SchemaValidator`</SwmToken> instance of the <SwmToken path="/pydantic/type_adapter.py" pos="367:6:6" line-data="            When using `TypeAdapter` with a Pydantic `dataclass`, the use of the `from_attributes`">`TypeAdapter`</SwmToken> to perform the validation.

```python
    def validate_python(
        self,
        object: Any,
        /,
        *,
        strict: bool | None = None,
        from_attributes: bool | None = None,
        context: dict[str, Any] | None = None,
    ) -> T:
        """Validate a Python object against the model.

        Args:
            object: The Python object to validate against the model.
            strict: Whether to strictly check types.
            from_attributes: Whether to extract data from object attributes.
            context: Additional context to pass to the validator.

        !!! note
            When using `TypeAdapter` with a Pydantic `dataclass`, the use of the `from_attributes`
            argument is not supported.

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/type_adapter.py" line="376">

---

## <SwmToken path="/pydantic/type_adapter.py" pos="376:3:3" line-data="    def validate_json(">`validate_json`</SwmToken>

The <SwmToken path="/pydantic/type_adapter.py" pos="376:3:3" line-data="    def validate_json(">`validate_json`</SwmToken> function is used to validate a JSON string or bytes against the model. It uses the <SwmToken path="/pydantic/type_adapter.py" pos="169:4:4" line-data="        validator (SchemaValidator): The schema validator for the type.">`SchemaValidator`</SwmToken> instance of the <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> to perform the validation.

```python
    def validate_json(
        self, data: str | bytes, /, *, strict: bool | None = None, context: dict[str, Any] | None = None
    ) -> T:
        """Usage docs: https://docs.pydantic.dev/2.8/concepts/json/#json-parsing

        Validate a JSON string or bytes against the model.

        Args:
            data: The JSON data to validate against the model.
            strict: Whether to strictly check types.
            context: Additional context to use during validation.

        Returns:
            The validated object.
        """
        return self.validator.validate_json(data, strict=strict, context=context)
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/type_adapter.py" line="421">

---

## <SwmToken path="/pydantic/type_adapter.py" pos="421:3:3" line-data="    def dump_python(">`dump_python`</SwmToken>

The <SwmToken path="/pydantic/type_adapter.py" pos="421:3:3" line-data="    def dump_python(">`dump_python`</SwmToken> function is used to serialize an instance of the adapted type to a Python object. It uses the <SwmToken path="/pydantic/type_adapter.py" pos="294:7:7" line-data="            self._serializer = SchemaSerializer(self._core_schema, core_config)">`SchemaSerializer`</SwmToken> instance of the <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> to perform the serialization.

```python
    def dump_python(
        self,
        instance: T,
        /,
        *,
        mode: Literal['json', 'python'] = 'python',
        include: IncEx | None = None,
        exclude: IncEx | None = None,
        by_alias: bool = False,
        exclude_unset: bool = False,
        exclude_defaults: bool = False,
        exclude_none: bool = False,
        round_trip: bool = False,
        warnings: bool | Literal['none', 'warn', 'error'] = True,
        serialize_as_any: bool = False,
        context: dict[str, Any] | None = None,
    ) -> Any:
        """Dump an instance of the adapted type to a Python object.

        Args:
            instance: The Python object to serialize.
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/type_adapter.py" line="474">

---

## <SwmToken path="/pydantic/type_adapter.py" pos="474:3:3" line-data="    def dump_json(">`dump_json`</SwmToken>

The <SwmToken path="/pydantic/type_adapter.py" pos="474:3:3" line-data="    def dump_json(">`dump_json`</SwmToken> function is used to serialize an instance of the adapted type to JSON. It uses the <SwmToken path="/pydantic/type_adapter.py" pos="294:7:7" line-data="            self._serializer = SchemaSerializer(self._core_schema, core_config)">`SchemaSerializer`</SwmToken> instance of the <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> to perform the serialization.

```python
    def dump_json(
        self,
        instance: T,
        /,
        *,
        indent: int | None = None,
        include: IncEx | None = None,
        exclude: IncEx | None = None,
        by_alias: bool = False,
        exclude_unset: bool = False,
        exclude_defaults: bool = False,
        exclude_none: bool = False,
        round_trip: bool = False,
        warnings: bool | Literal['none', 'warn', 'error'] = True,
        serialize_as_any: bool = False,
        context: dict[str, Any] | None = None,
    ) -> bytes:
        """Usage docs: https://docs.pydantic.dev/2.8/concepts/json/#json-serialization

        Serialize an instance of the adapted type to JSON.

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/type_adapter.py" line="529">

---

## <SwmToken path="/pydantic/type_adapter.py" pos="529:3:3" line-data="    def json_schema(">`json_schema`</SwmToken>

The <SwmToken path="/pydantic/type_adapter.py" pos="529:3:3" line-data="    def json_schema(">`json_schema`</SwmToken> function is used to generate a JSON schema for the adapted type. It uses the core schema of the <SwmToken path="/pydantic/type_adapter.py" pos="150:2:2" line-data="class TypeAdapter(Generic[T]):">`TypeAdapter`</SwmToken> and a schema generator to create the JSON schema.

```python
    def json_schema(
        self,
        *,
        by_alias: bool = True,
        ref_template: str = DEFAULT_REF_TEMPLATE,
        schema_generator: type[GenerateJsonSchema] = GenerateJsonSchema,
        mode: JsonSchemaMode = 'validation',
    ) -> dict[str, Any]:
        """Generate a JSON schema for the adapted type.
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

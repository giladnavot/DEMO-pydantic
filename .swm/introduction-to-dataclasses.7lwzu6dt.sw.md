---
title: Introduction to Dataclasses
---
Dataclasses in this repository are a way of creating classes which are primarily used to store data. They are similar to the standard Python `dataclass`, but with added validation. This is achieved by using the `dataclass` decorator provided by Pydantic. The decorator accepts a class as its argument and returns a Pydantic-enhanced dataclass. The decorator also takes several parameters to customize the behavior of the dataclass, such as `init`, `repr`, `eq`, `order`, `unsafe_hash`, `frozen`, `config`, `validate_on_init`, `kw_only`, and `slots`. These parameters control various aspects of the dataclass, such as whether it should be hashable, whether it should be immutable, and whether its attributes should be validated upon initialization.

<SwmSnippet path="/pydantic/dataclasses.py" line="96">

---

Here is the definition of the `dataclass` decorator. It takes a class and several optional parameters to customize the behavior of the dataclass. The decorator asserts that `init` is `False` and `validate_on_init` is not `False`, meaning that Pydantic inserts its own `__init__` function and all Pydantic dataclasses are validated on init.

```python
def dataclass(  # noqa: C901
    _cls: type[_T] | None = None,
    *,
    init: Literal[False] = False,
    repr: bool = True,
    eq: bool = True,
    order: bool = False,
    unsafe_hash: bool = False,
    frozen: bool = False,
    config: ConfigDict | type[object] | None = None,
    validate_on_init: bool | None = None,
    kw_only: bool = False,
    slots: bool = False,
) -> Callable[[type[_T]], type[PydanticDataclass]] | type[PydanticDataclass]:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/dataclasses/

    A decorator used to create a Pydantic-enhanced dataclass, similar to the standard Python `dataclass`,
    but with added validation.

    This function should be used similarly to `dataclasses.dataclass`.

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/dataclasses.py" line="149">

---

The `make_pydantic_fields_compatible` function is used to ensure that standard Python `dataclasses` understand `Field` kwargs like `kw_only`. It modifies the class's fields to be compatible with `dataclasses.field`.

```python
    def make_pydantic_fields_compatible(cls: type[Any]) -> None:
        """Make sure that stdlib `dataclasses` understands `Field` kwargs like `kw_only`
        To do that, we simply change
          `x: int = pydantic.Field(..., kw_only=True)`
        into
          `x: int = dataclasses.field(default=pydantic.Field(..., kw_only=True), kw_only=True)`
        """
        for annotation_cls in cls.__mro__:
            # In Python < 3.9, `__annotations__` might not be present if there are no fields.
            # we therefore need to use `getattr` to avoid an `AttributeError`.
            annotations = getattr(annotation_cls, '__annotations__', [])
            for field_name in annotations:
                field_value = getattr(cls, field_name, None)
                # Process only if this is an instance of `FieldInfo`.
                if not isinstance(field_value, FieldInfo):
                    continue

                # Initialize arguments for the standard `dataclasses.field`.
                field_args: dict = {'default': field_value}

                # Handle `kw_only` for Python 3.10+
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/dataclasses.py" line="184">

---

The `create_dataclass` function is used to create a Pydantic dataclass from a regular dataclass. It first checks if the class is already a Pydantic model, then it modifies the class to be compatible with `dataclasses.dataclass` and adds Pydantic-specific attributes.

```python
    def create_dataclass(cls: type[Any]) -> type[PydanticDataclass]:
        """Create a Pydantic dataclass from a regular dataclass.

        Args:
            cls: The class to create the Pydantic dataclass from.

        Returns:
            A Pydantic dataclass.
        """
        from ._internal._utils import is_model_class

        if is_model_class(cls):
            raise PydanticUserError(
                f'Cannot create a Pydantic dataclass from {cls.__name__} as it is already a Pydantic model',
                code='dataclass-on-model',
            )

        original_cls = cls

        config_dict = config
        if config_dict is None:
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/dataclasses.py" line="96">

---

# Dataclass Definition

This is the definition of the `dataclass` decorator. It accepts a class as its argument and returns a Pydantic-enhanced dataclass. The decorator adds validation to the class based on the type annotations of the class attributes. It also supports additional parameters such as `init`, `repr`, `eq`, `order`, `unsafe_hash`, `frozen`, `config`, `validate_on_init`, `kw_only`, and `slots`.

```python
def dataclass(  # noqa: C901
    _cls: type[_T] | None = None,
    *,
    init: Literal[False] = False,
    repr: bool = True,
    eq: bool = True,
    order: bool = False,
    unsafe_hash: bool = False,
    frozen: bool = False,
    config: ConfigDict | type[object] | None = None,
    validate_on_init: bool | None = None,
    kw_only: bool = False,
    slots: bool = False,
) -> Callable[[type[_T]], type[PydanticDataclass]] | type[PydanticDataclass]:
    """Usage docs: https://docs.pydantic.dev/2.8/concepts/dataclasses/

    A decorator used to create a Pydantic-enhanced dataclass, similar to the standard Python `dataclass`,
    but with added validation.

    This function should be used similarly to `dataclasses.dataclass`.

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/functional_validators.py" line="28">

---

# Dataclass Usage

This is an example of how the `dataclass` decorator is used in the repository. Here, it is used to define the `AfterValidator` class. The decorator adds special methods to the class based on the type annotations of the class attributes.

```python
@dataclasses.dataclass(frozen=True, **_internal_dataclass.slots_true)
class AfterValidator:
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/dataclasses.py" line="277">

---

# Dataclass Rebuilding

This is the `rebuild_dataclass` function, which is used to rebuild the Pydantic schema for a dataclass. This may be necessary when one of the annotations is a ForwardRef which could not be resolved during the initial attempt to build the schema, and automatic rebuilding fails.

```python
def rebuild_dataclass(
    cls: type[PydanticDataclass],
    *,
    force: bool = False,
    raise_errors: bool = True,
    _parent_namespace_depth: int = 2,
    _types_namespace: dict[str, Any] | None = None,
) -> bool | None:
    """Try to rebuild the pydantic-core schema for the dataclass.

    This may be necessary when one of the annotations is a ForwardRef which could not be resolved during
    the initial attempt to build the schema, and automatic rebuilding fails.

    This is analogous to `BaseModel.model_rebuild`.

    Args:
        cls: The class to rebuild the pydantic-core schema for.
        force: Whether to force the rebuilding of the schema, defaults to `False`.
        raise_errors: Whether to raise errors, defaults to `True`.
        _parent_namespace_depth: The depth level of the parent namespace, defaults to 2.
        _types_namespace: The types namespace, defaults to `None`.
```

---

</SwmSnippet>

# Dataclasses in Pydantic

This section covers the main functions in the dataclasses module of the Pydantic library.

<SwmSnippet path="/pydantic/dataclasses.py" line="30">

---

## The `dataclass` decorator

The `dataclass` decorator is used to create a Pydantic-enhanced dataclass, similar to the standard Python `dataclass`, but with added validation. This function should be used similarly to `dataclasses.dataclass`. It takes several parameters, including `init`, `repr`, `eq`, `order`, `unsafe_hash`, `frozen`, `config`, `validate_on_init`, `kw_only`, and `slots`. These parameters control various aspects of the dataclass, such as whether it includes an `__init__` method, whether it includes a `__repr__` method, whether it is hashable, whether it is frozen (immutable), and more.

```python
    def dataclass(
        *,
        init: Literal[False] = False,
        repr: bool = True,
        eq: bool = True,
        order: bool = False,
        unsafe_hash: bool = False,
        frozen: bool = False,
        config: ConfigDict | type[object] | None = None,
        validate_on_init: bool | None = None,
        kw_only: bool = ...,
        slots: bool = ...,
    ) -> Callable[[type[_T]], type[PydanticDataclass]]:  # type: ignore
        ...

    @dataclass_transform(field_specifiers=(dataclasses.field, Field, PrivateAttr))
    @overload
    def dataclass(
        _cls: type[_T],  # type: ignore
        *,
        init: Literal[False] = False,
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/dataclasses.py" line="277">

---

## The `rebuild_dataclass` function

The `rebuild_dataclass` function is used to rebuild the pydantic-core schema for the dataclass. This may be necessary when one of the annotations is a ForwardRef which could not be resolved during the initial attempt to build the schema, and automatic rebuilding fails. This function takes several parameters, including `cls`, `force`, `raise_errors`, `_parent_namespace_depth`, and `_types_namespace`. These parameters control various aspects of the rebuilding process, such as whether to force the rebuilding of the schema, whether to raise errors, and more.

```python
def rebuild_dataclass(
    cls: type[PydanticDataclass],
    *,
    force: bool = False,
    raise_errors: bool = True,
    _parent_namespace_depth: int = 2,
    _types_namespace: dict[str, Any] | None = None,
) -> bool | None:
    """Try to rebuild the pydantic-core schema for the dataclass.

    This may be necessary when one of the annotations is a ForwardRef which could not be resolved during
    the initial attempt to build the schema, and automatic rebuilding fails.

    This is analogous to `BaseModel.model_rebuild`.

    Args:
        cls: The class to rebuild the pydantic-core schema for.
        force: Whether to force the rebuilding of the schema, defaults to `False`.
        raise_errors: Whether to raise errors, defaults to `True`.
        _parent_namespace_depth: The depth level of the parent namespace, defaults to 2.
        _types_namespace: The types namespace, defaults to `None`.
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

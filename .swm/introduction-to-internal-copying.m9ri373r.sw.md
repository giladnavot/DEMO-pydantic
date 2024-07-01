---
title: Introduction to Internal Copying
---
Internal Copying in Pydantic refers to the process of creating a new instance of a model by copying the data from an existing instance. This is achieved through the `_copy_and_set_values` function, which takes a model instance and a dictionary of new values, and returns a new instance of the model with the updated values. If the `deep` parameter is set to True, the function will perform a deep copy of the values, meaning that changes to the original data will not affect the copied data. This function is used in various parts of the codebase to ensure data consistency and isolation.

<SwmSnippet path="/pydantic/deprecated/copy_internals.py" line="98">

---

# Internal Copying in Pydantic

This is the `_copy_and_set_values` function, which performs the actual copying. It first performs a deep copy of the data if the `deep` parameter is set to `True`. Then it creates a new instance of the model and sets its data to the copied data.

```python
def _copy_and_set_values(
    self: Model,
    values: dict[str, Any],
    fields_set: set[str],
    extra: dict[str, Any] | None = None,
    private: dict[str, Any] | None = None,
    *,
    deep: bool,  # UP006
) -> Model:
    if deep:
        # chances of having empty dict here are quite low for using smart_deepcopy
        values = deepcopy(values)
        extra = deepcopy(extra)
        private = deepcopy(private)

    cls = self.__class__
    m = cls.__new__(cls)
    _object_setattr(m, '__dict__', values)
    _object_setattr(m, '__pydantic_extra__', extra)
    _object_setattr(m, '__pydantic_fields_set__', fields_set)
    _object_setattr(m, '__pydantic_private__', private)
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/copy_internals.py" line="29">

---

The `_iter` function is used to iterate over the fields of the model. It takes into account various parameters such as `include`, `exclude`, `exclude_unset`, `exclude_defaults`, and `exclude_none` to determine which fields to include in the iteration. This function is used by `_copy_and_set_values` to determine which fields to copy.

```python
def _iter(
    self: BaseModel,
    to_dict: bool = False,
    by_alias: bool = False,
    include: AbstractSetIntStr | MappingIntStrAny | None = None,
    exclude: AbstractSetIntStr | MappingIntStrAny | None = None,
    exclude_unset: bool = False,
    exclude_defaults: bool = False,
    exclude_none: bool = False,
) -> TupleGenerator:
    # Merge field set excludes with explicit exclude parameter with explicit overriding field set options.
    # The extra "is not None" guards are not logically necessary but optimizes performance for the simple case.
    if exclude is not None:
        exclude = _utils.ValueItems.merge(
            {k: v.exclude for k, v in self.model_fields.items() if v.exclude is not None}, exclude
        )

    if include is not None:
        include = _utils.ValueItems.merge({k: True for k in self.model_fields}, include, intersect=True)

    allowed_keys = _calculate_keys(self, include=include, exclude=exclude, exclude_unset=exclude_unset)  # type: ignore
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/copy_internals.py" line="124">

---

The `_get_value` function is used to get the value of a field for copying. It takes into account various parameters such as `to_dict`, `by_alias`, `include`, `exclude`, `exclude_unset`, `exclude_defaults`, and `exclude_none` to determine how to get the value. This function is used by `_iter` to get the values of the fields to copy.

```python
def _get_value(
    cls: type[BaseModel],
    v: Any,
    to_dict: bool,
    by_alias: bool,
    include: AbstractSetIntStr | MappingIntStrAny | None,
    exclude: AbstractSetIntStr | MappingIntStrAny | None,
    exclude_unset: bool,
    exclude_defaults: bool,
    exclude_none: bool,
) -> Any:
    from .. import BaseModel

    if isinstance(v, BaseModel):
        if to_dict:
            return v.model_dump(
                by_alias=by_alias,
                exclude_unset=exclude_unset,
                exclude_defaults=exclude_defaults,
                include=include,  # type: ignore
                exclude=exclude,  # type: ignore
```

---

</SwmSnippet>

# Internal Copying Functions

This section discusses the main functions related to internal copying in Pydantic.

<SwmSnippet path="/pydantic/deprecated/copy_internals.py" line="29">

---

## \_iter Function

The `_iter` function is used to iterate over the data model. It takes several parameters to control the iteration process, such as whether to convert the output to a dictionary, whether to use aliases for field names, and which fields to include or exclude. It yields key-value pairs from the data model.

```python
def _iter(
    self: BaseModel,
    to_dict: bool = False,
    by_alias: bool = False,
    include: AbstractSetIntStr | MappingIntStrAny | None = None,
    exclude: AbstractSetIntStr | MappingIntStrAny | None = None,
    exclude_unset: bool = False,
    exclude_defaults: bool = False,
    exclude_none: bool = False,
) -> TupleGenerator:
    # Merge field set excludes with explicit exclude parameter with explicit overriding field set options.
    # The extra "is not None" guards are not logically necessary but optimizes performance for the simple case.
    if exclude is not None:
        exclude = _utils.ValueItems.merge(
            {k: v.exclude for k, v in self.model_fields.items() if v.exclude is not None}, exclude
        )

    if include is not None:
        include = _utils.ValueItems.merge({k: True for k in self.model_fields}, include, intersect=True)

    allowed_keys = _calculate_keys(self, include=include, exclude=exclude, exclude_unset=exclude_unset)  # type: ignore
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/copy_internals.py" line="98">

---

## \_copy_and_set_values Function

The `_copy_and_set_values` function is used to create a deep copy of the data model and set new values. It takes a dictionary of values, a set of fields, and optional dictionaries for extra and private data. If the `deep` parameter is set to True, it creates a deep copy of the values, extra, and private data.

```python
def _copy_and_set_values(
    self: Model,
    values: dict[str, Any],
    fields_set: set[str],
    extra: dict[str, Any] | None = None,
    private: dict[str, Any] | None = None,
    *,
    deep: bool,  # UP006
) -> Model:
    if deep:
        # chances of having empty dict here are quite low for using smart_deepcopy
        values = deepcopy(values)
        extra = deepcopy(extra)
        private = deepcopy(private)

    cls = self.__class__
    m = cls.__new__(cls)
    _object_setattr(m, '__dict__', values)
    _object_setattr(m, '__pydantic_extra__', extra)
    _object_setattr(m, '__pydantic_fields_set__', fields_set)
    _object_setattr(m, '__pydantic_private__', private)
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/copy_internals.py" line="124">

---

## \_get_value Function

The `_get_value` function is used to get a value from the data model. It takes several parameters to control how the value is retrieved, such as whether to convert the output to a dictionary, whether to use aliases for field names, and which fields to include or exclude. It returns the retrieved value.

```python
def _get_value(
    cls: type[BaseModel],
    v: Any,
    to_dict: bool,
    by_alias: bool,
    include: AbstractSetIntStr | MappingIntStrAny | None,
    exclude: AbstractSetIntStr | MappingIntStrAny | None,
    exclude_unset: bool,
    exclude_defaults: bool,
    exclude_none: bool,
) -> Any:
    from .. import BaseModel

    if isinstance(v, BaseModel):
        if to_dict:
            return v.model_dump(
                by_alias=by_alias,
                exclude_unset=exclude_unset,
                exclude_defaults=exclude_defaults,
                include=include,  # type: ignore
                exclude=exclude,  # type: ignore
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/copy_internals.py" line="198">

---

## \_calculate_keys Function

The `_calculate_keys` function is used to calculate the keys to be included in the output based on the include and exclude parameters, and whether unset values should be excluded. It returns a set of keys.

```python
def _calculate_keys(
    self: BaseModel,
    include: MappingIntStrAny | None,
    exclude: MappingIntStrAny | None,
    exclude_unset: bool,
    update: typing.Dict[str, Any] | None = None,  # noqa UP006
) -> typing.AbstractSet[str] | None:
    if include is None and exclude is None and exclude_unset is False:
        return None

    keys: typing.AbstractSet[str]
    if exclude_unset:
        keys = self.__pydantic_fields_set__.copy()
    else:
        keys = set(self.__dict__.keys())
        keys = keys | (self.__pydantic_extra__ or {}).keys()

    if include is not None:
        keys &= include.keys()

    if update:
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

---
title: Introduction to Deprecated Features
---
Deprecated features in the DEMO-pydantic repository refer to parts of the codebase that are no longer recommended for use and are likely to be removed in future updates. These are typically functions, methods, or classes that have been superseded by newer, more efficient, or more secure alternatives. The Pydantic library, which this repository is based on, uses the `DeprecationWarning` variable to flag these deprecated features. This variable is defined in multiple files within the `pydantic/deprecated/` directory, and it is set to `PydanticDeprecatedSince20`, indicating that these features have been deprecated since the 2.0 version of Pydantic.

<SwmSnippet path="/pydantic/deprecated/parse.py" line="17">

---

# DeprecationWarning

The `DeprecationWarning` is a variable that is used to mark features as deprecated. It is defined as `PydanticDeprecatedSince20`, indicating that the feature has been deprecated since version 2.0 of Pydantic.

```python
    DeprecationWarning = PydanticDeprecatedSince20
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/config.py" line="30">

---

# BaseConfig

The `BaseConfig` class is an example of a deprecated feature. It is marked as deprecated with a warning message that suggests using `pydantic.ConfigDict` instead. The `DeprecationWarning` is used within the `__getattr__` and `__init_subclass__` methods to warn developers when they are using this deprecated feature.

```python
class BaseConfig(metaclass=_ConfigMetaclass):
    """This class is only retained for backwards compatibility.

    !!! Warning "Deprecated"
        BaseConfig is deprecated. Use the [`pydantic.ConfigDict`][pydantic.ConfigDict] instead.
    """

    def __getattr__(self, item: str) -> Any:
        try:
            obj = super().__getattribute__(item)
            warnings.warn(_config.DEPRECATION_MESSAGE, DeprecationWarning)
            return obj
        except AttributeError as exc:
            try:
                return getattr(type(self), item)
            except AttributeError:
                # re-raising changes the displayed text to reflect that `self` is not a type
                raise AttributeError(str(exc)) from exc

    def __init_subclass__(cls, **kwargs: Any) -> None:
        warnings.warn(_config.DEPRECATION_MESSAGE, DeprecationWarning)
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/config.py" line="55">

---

# **getattribute**

The `__getattribute__` method is used to emit a `DeprecationWarning` when certain attributes are accessed. This is another way to notify developers about deprecated features.

```python
    def __getattribute__(self, __name: str) -> Any:
        # The @deprecated decorator accesses other attributes, so we only emit a warning for the expected ones
        if __name in {'allow', 'ignore', 'forbid'}:
            warnings.warn(
                "`pydantic.config.Extra` is deprecated, use literal values instead (e.g. `extra='allow'`)",
                DeprecationWarning,
                stacklevel=2,
            )
        return super().__getattribute__(__name)
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/deprecated/class_validators.py" line="16">

---

# \_ALLOW_REUSE_WARNING_MESSAGE

The `_ALLOW_REUSE_WARNING_MESSAGE` is a constant that holds a deprecation warning message. This message is used to warn developers when they use the `allow_reuse` feature, which is deprecated.

```python
_ALLOW_REUSE_WARNING_MESSAGE = '`allow_reuse` is deprecated and will be ignored; it should no longer be necessary'
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

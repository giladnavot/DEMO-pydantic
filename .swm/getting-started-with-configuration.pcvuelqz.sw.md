---
title: Getting Started with Configuration
---
Configuration in the DEMO-pydantic repository refers to the setup and customization of the Pydantic library. It is primarily handled through the `BaseConfig` class and the `_ConfigMetaclass` metaclass, which are defined in the `pydantic/deprecated/config.py` file. These classes provide a way to define default configuration values and handle attribute access. However, it's important to note that the `BaseConfig` class is deprecated and it's recommended to use the `pydantic.ConfigDict` instead. The `Extra` class, also defined in the same file, is used to specify how to handle additional, unexpected fields in the data models. It too is deprecated and literal values are recommended instead.

<SwmSnippet path="/pydantic/deprecated/config.py" line="30">

---

# BaseConfig Class

The BaseConfig class was used for configuration management. It's now deprecated and replaced by `pydantic.ConfigDict`. The `__getattr__` method was used to access the configuration attributes, and a warning was issued if deprecated attributes were accessed.

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

<SwmSnippet path="/pydantic/deprecated/config.py" line="19">

---

# \_ConfigMetaclass

The \_ConfigMetaclass is the metaclass for the BaseConfig class. It handles the attribute access for the class, issuing a warning if deprecated attributes are accessed.

```python
class _ConfigMetaclass(type):
    def __getattr__(self, item: str) -> Any:
        try:
            obj = _config.config_defaults[item]
            warnings.warn(_config.DEPRECATION_MESSAGE, DeprecationWarning)
            return obj
        except KeyError as exc:
            raise AttributeError(f"type object '{self.__name__}' has no attribute {exc}") from exc
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

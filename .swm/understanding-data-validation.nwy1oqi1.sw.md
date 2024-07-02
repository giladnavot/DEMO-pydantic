---
title: Understanding Data Validation
---
Data Validation refers to the process of ensuring that the data being input or processed adheres to certain rules or constraints. It is a critical aspect of maintaining data integrity and preventing errors in data processing. In the context of Pydantic, data validation is performed using Python type hints, which allows developers to define and validate data in a straightforward and Pythonic manner.

<SwmSnippet path="/pydantic/plugin/_loader.py" line="21">

---

## Using Pydantic for Data Validation

This code snippet shows the <SwmToken path="/pydantic/plugin/_loader.py" pos="21:2:2" line-data="def get_plugins() -&gt; Iterable[PydanticPluginProtocol]:">`get_plugins`</SwmToken> function in Pydantic. This function is used to load plugins for Pydantic, which can be used to extend its functionality. While this is not directly related to data validation, it shows how Pydantic can be extended and customized to suit the needs of different applications.

```python
def get_plugins() -> Iterable[PydanticPluginProtocol]:
    """Load plugins for Pydantic.

    Inspired by: https://github.com/pytest-dev/pluggy/blob/1.3.0/src/pluggy/_manager.py#L376-L402
    """
    disabled_plugins = os.getenv('PYDANTIC_DISABLE_PLUGINS')
    global _plugins, _loading_plugins
    if _loading_plugins:
        # this happens when plugins themselves use pydantic, we return no plugins
        return ()
    elif disabled_plugins in ('__all__', '1', 'true'):
        return ()
    elif _plugins is None:
        _plugins = {}
        # set _loading_plugins so any plugins that use pydantic don't themselves use plugins
        _loading_plugins = True
        try:
            for dist in importlib_metadata.distributions():
                for entry_point in dist.entry_points:
                    if entry_point.group != PYDANTIC_ENTRY_POINT_GROUP:
                        continue
```

---

</SwmSnippet>

## Handling Invalid Data

When Pydantic validates data and encounters a value that does not match the expected type, it raises a <SwmToken path="/pydantic/v1/error_wrappers.py" pos="50:2:2" line-data="class ValidationError(Representation, ValueError):">`ValidationError`</SwmToken>. This error can be caught and handled in your code, allowing you to decide how to proceed when invalid data is encountered. This could involve returning an error message to the user, logging the error for debugging purposes, or even ignoring the error in certain cases.

## Custom Validators

In addition to the built-in data types, Pydantic allows you to define custom validators. These are functions that take a value and return a modified value or raise an error. This allows you to implement complex validation logic, such as checking if a string matches a certain pattern, or if a number is within a certain range.

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

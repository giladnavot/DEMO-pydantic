---
title: Getting Started with Error Handling
---
Error handling in Pydantic is managed through a set of custom exceptions, defined in the `errors.py` file. These exceptions are designed to provide detailed error messages and codes to help developers identify and fix issues. The `PydanticErrorMixin` class is a base class that provides common functionality for all Pydantic-specific errors, including a message and an optional error code. Specific error classes like `PydanticUserError`, `PydanticUndefinedAnnotation`, `PydanticImportError`, and others inherit from this mixin and provide more specific error handling functionality.

<SwmSnippet path="/pydantic/errors.py" line="71">

---

# Pydantic Error Classes

The `PydanticErrorMixin` class is a base class for all Pydantic-specific errors. It provides common functionality such as storing an error message and an optional error code, and converting the error to a string.

```python
class PydanticErrorMixin:
    """A mixin class for common functionality shared by all Pydantic-specific errors.

    Attributes:
        message: A message describing the error.
        code: An optional error code from PydanticErrorCodes enum.
    """

    def __init__(self, message: str, *, code: PydanticErrorCodes | None) -> None:
        self.message = message
        self.code = code

    def __str__(self) -> str:
        if self.code is None:
            return self.message
        else:
            return f'{self.message}\n\nFor further information visit {DEV_ERROR_DOCS_URL}{self.code}'
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/errors.py" line="90">

---

The `PydanticUserError` class is a specific type of Pydantic error that is raised due to incorrect use of Pydantic.

```python
class PydanticUserError(PydanticErrorMixin, TypeError):
    """An error raised due to incorrect use of Pydantic."""


```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/errors.py" line="94">

---

The `PydanticUndefinedAnnotation` class is a specific type of Pydantic error that is raised when handling undefined annotations during `CoreSchema` generation. It provides a method to create an instance of the error from a `NameError`.

```python
class PydanticUndefinedAnnotation(PydanticErrorMixin, NameError):
    """A subclass of `NameError` raised when handling undefined annotations during `CoreSchema` generation.

    Attributes:
        name: Name of the error.
        message: Description of the error.
    """

    def __init__(self, name: str, message: str) -> None:
        self.name = name
        super().__init__(message=message, code='undefined-annotation')

    @classmethod
    def from_name_error(cls, name_error: NameError) -> Self:
        """Convert a `NameError` to a `PydanticUndefinedAnnotation` error.

        Args:
            name_error: `NameError` to be converted.

        Returns:
            Converted `PydanticUndefinedAnnotation` error.
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic" doc-type="overview"><sup>Powered by [Swimm](/)</sup></SwmMeta>

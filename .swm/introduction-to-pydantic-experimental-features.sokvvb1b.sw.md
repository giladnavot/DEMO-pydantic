---
title: Introduction to Pydantic Experimental Features
---
The <SwmPath>[pydantic/experimental/](/pydantic/experimental/)</SwmPath> directory in the DEMO-pydantic repository contains experimental features of the Pydantic library. These features are still under development and may not be stable or fully documented. The directory includes Python files like '[pipeline.py](http://pipeline.py)' and '**init**.py'. The experimental features are intended for testing and feedback purposes, and developers are warned about their experimental nature through the <SwmToken path="/pydantic/experimental/__init__.py" pos="5:8:8" line-data="from pydantic.warnings import PydanticExperimentalWarning">`PydanticExperimentalWarning`</SwmToken> warning.

<SwmSnippet path="/pydantic/experimental/pipeline.py" line="20">

---

# Experimental Features

This file contains the implementation of the experimental pipeline feature. It imports the <SwmToken path="/pydantic/experimental/pipeline.py" pos="22:7:7" line-data="    from pydantic import GetCoreSchemaHandler">`GetCoreSchemaHandler`</SwmToken> from the Pydantic library, which is used to handle core schemas.

```python
    from pydantic_core import core_schema as cs

    from pydantic import GetCoreSchemaHandler

from pydantic._internal._internal_dataclass import slots_true as _slots_true
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/experimental/__init__.py" line="3">

---

# Experimental Warnings

This file contains a warning that is raised when using experimental features. The <SwmToken path="/pydantic/experimental/__init__.py" pos="5:8:8" line-data="from pydantic.warnings import PydanticExperimentalWarning">`PydanticExperimentalWarning`</SwmToken> is used to alert users that they are using features that are still under development and may change.

```python
import warnings

from pydantic.warnings import PydanticExperimentalWarning

warnings.warn(
```

---

</SwmSnippet>

# Functions in <SwmPath>[pydantic/experimental/](/pydantic/experimental/)</SwmPath>

This section will provide an overview of the functions in the <SwmPath>[pydantic/experimental/](/pydantic/experimental/)</SwmPath> directory, focusing on the [pipeline.py](http://pipeline.py) file.

<SwmSnippet path="/pydantic/experimental/pipeline.py" line="113">

---

## The \_Pipeline Function

The \_Pipeline function is a generic function that represents a sequence of validation and transformation steps. It is used as the base for the <SwmToken path="/pydantic/experimental/pipeline.py" pos="356:0:0" line-data="validate_as = _Pipeline[Any, Any](()).validate_as">`validate_as`</SwmToken> and transform functions.

```python
_Step = Union[_ValidateAs, _ValidateAsDefer, _Transform, _PipelineOr, _PipelineAnd, _Constraint]

_InT = TypeVar('_InT')
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/experimental/pipeline.py" line="356">

---

## The <SwmToken path="/pydantic/experimental/pipeline.py" pos="356:0:0" line-data="validate_as = _Pipeline[Any, Any](()).validate_as">`validate_as`</SwmToken> Function

The <SwmToken path="/pydantic/experimental/pipeline.py" pos="356:0:0" line-data="validate_as = _Pipeline[Any, Any](()).validate_as">`validate_as`</SwmToken> function is a method of the \_Pipeline class that adds a validation step to the pipeline. It takes a type as an argument and validates the input data against this type.

```python
validate_as = _Pipeline[Any, Any](()).validate_as
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/experimental/pipeline.py" line="358">

---

## The transform Function

The transform function is a method of the \_Pipeline class that adds a transformation step to the pipeline. It takes a function as an argument and applies this function to the input data.

```python
transform = _Pipeline[Any, Any]((_ValidateAs(_FieldTypeMarker),)).transform
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

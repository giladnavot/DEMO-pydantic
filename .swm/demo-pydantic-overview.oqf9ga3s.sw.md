---
title: DEMO-pydantic overview
---
DEMO-pydantic is a Python library that provides data validation using Python type hints. It allows developers to define and validate data in pure, canonical Python 3.8+. The library is designed to work well with any editor or IDE out of the box, and it supports additional editor features when used with Visual Studio Code or <SwmToken path="/pydantic/deprecated/tools.py" pos="14:5:5" line-data="    # See PyCharm issues https://youtrack.jetbrains.com/issue/PY-21915">`PyCharm`</SwmToken>. It's used by hundreds of organizations and packages worldwide.

## Modules

### Pydantic Core

Pydantic Core is a module in the Pydantic library that provides essential functionalities and types such as PydanticUndefined and <SwmToken path="/pydantic/type_adapter.py" pos="26:6:6" line-data="from pydantic_core import CoreSchema, SchemaSerializer, SchemaValidator, Some">`CoreSchema`</SwmToken>. It is imported and used across various files in the library, indicating its central role in the functioning of Pydantic. PydanticUndefined is a type used to represent undefined values, while <SwmToken path="/pydantic/type_adapter.py" pos="26:6:6" line-data="from pydantic_core import CoreSchema, SchemaSerializer, SchemaValidator, Some">`CoreSchema`</SwmToken> is used to define the core schema for Pydantic models. The <SwmToken path="/pydantic/type_adapter.py" pos="305:3:3" line-data="    def core_schema(self) -&gt; CoreSchema:">`core_schema`</SwmToken> function is also part of Pydantic Core, and it is used to generate the core schema for Pydantic models.

- <SwmLink doc-title="Exploring the Core of Pydantic">[Exploring the Core of Pydantic](/.swm/exploring-the-core-of-pydantic.wqkv0hqh.sw.md)</SwmLink>
- <SwmLink doc-title="Data Validation and Manipulation in Pydantic">[Data Validation and Manipulation in Pydantic](/.swm/data-validation-and-manipulation-in-pydantic.yczmkex6.sw.md)</SwmLink>
- <SwmLink doc-title="Dataclass Creation in Pydantic">[Dataclass Creation in Pydantic](/.swm/dataclass-creation-in-pydantic.3cjjn3re.sw.md)</SwmLink>
- <SwmLink doc-title="Rebuilding a Dataclass">[Rebuilding a Dataclass](/.swm/rebuilding-a-dataclass.ju7876as.sw.md)</SwmLink>
- <SwmLink doc-title="Data Validation in Pydantic">[Data Validation in Pydantic](/.swm/data-validation-in-pydantic.w0xam561.sw.md)</SwmLink>
- <SwmLink doc-title="Schema Generation in Pydantic">[Schema Generation in Pydantic](/.swm/schema-generation-in-pydantic.d5dldiny.sw.md)</SwmLink>
- <SwmLink doc-title="Schema Generation in Pydantic">[Schema Generation in Pydantic](/.swm/schema-generation-in-pydantic.72geczrq.sw.md)</SwmLink>
- <SwmLink doc-title="JSON Data Validation and Manipulation">[JSON Data Validation and Manipulation](/.swm/json-data-validation-and-manipulation.gtv5849p.sw.md)</SwmLink>
- <SwmLink doc-title="Model Rebuilding and Schema Generation">[Model Rebuilding and Schema Generation](/.swm/model-rebuilding-and-schema-generation.9cgpmqkm.sw.md)</SwmLink>
- <SwmLink doc-title="Updating Forward References Process">[Updating Forward References Process](/.swm/updating-forward-references-process.ew0ou6vz.sw.md)</SwmLink>
- <SwmLink doc-title="Data Validation in Pydantic">[Data Validation in Pydantic](/.swm/data-validation-in-pydantic.2lxi7kyw.sw.md)</SwmLink>
- <SwmLink doc-title="Data Validation in Pydantic">[Data Validation in Pydantic](/.swm/data-validation-in-pydantic.mxqs1t0d.sw.md)</SwmLink>
- <SwmLink doc-title="Schema Validation and Generation in Pydantic">[Schema Validation and Generation in Pydantic](/.swm/schema-validation-and-generation-in-pydantic.sylj00cr.sw.md)</SwmLink>
- <SwmLink doc-title="Data Validation in Pydantic">[Data Validation in Pydantic](/.swm/data-validation-in-pydantic.oifi28h1.sw.md)</SwmLink>
- <SwmLink doc-title="Pydantics Core Schema Generation and Validation">[Pydantics Core Schema Generation and Validation](/.swm/pydantics-core-schema-generation-and-validation.zkou251x.sw.md)</SwmLink>
- <SwmLink doc-title="Data Serialization Process">[Data Serialization Process](/.swm/data-serialization-process.aoqi0qfb.sw.md)</SwmLink>
- <SwmLink doc-title="Pydantic Model Creation and Validation">[Pydantic Model Creation and Validation](/.swm/pydantic-model-creation-and-validation.a9o3r32d.sw.md)</SwmLink>
- <SwmLink doc-title="The Protocol Class in Pydantic">[The Protocol Class in Pydantic](/.swm/the-protocol-class-in-pydantic.r64cl.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of the BaseModel Class">[Overview of the BaseModel Class](/.swm/overview-of-the-basemodel-class.fvfsy.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of the Representation Class">[Overview of the Representation Class](/.swm/overview-of-the-representation-class.0l4ky.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of BaseModel Class">[Overview of BaseModel Class](/.swm/overview-of-basemodel-class.nlcxq.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of the AnyUrl Class">[Overview of the AnyUrl Class](/.swm/overview-of-the-anyurl-class.1qbuo.sw.md)</SwmLink>
- <SwmLink doc-title="_SecretField Class Overview">[_SecretField Class Overview](/.swm/_secretfield-class-overview.spfum.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of the ConstrainedFloat Class">[Overview of the ConstrainedFloat Class](/.swm/overview-of-the-constrainedfloat-class.l29dq.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of the ConstrainedInt Class">[Overview of the ConstrainedInt Class](/.swm/overview-of-the-constrainedint-class.r7czm.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of the BaseConfig Class">[Overview of the BaseConfig Class](/.swm/overview-of-the-baseconfig-class.i1u4j.sw.md)</SwmLink>
- <SwmLink doc-title="PydanticDeprecationWarning Overview">[PydanticDeprecationWarning Overview](/.swm/pydanticdeprecationwarning-overview.dym9o.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of the EncodedBytes Class">[Overview of the EncodedBytes Class](/.swm/overview-of-the-encodedbytes-class.zw9iy.sw.md)</SwmLink>
- <SwmLink doc-title="_FromFieldInfoInputs Overview">[_FromFieldInfoInputs Overview](/.swm/_fromfieldinfoinputs-overview.kq5c8.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of the UUID1 Class">[Overview of the UUID1 Class](/.swm/overview-of-the-uuid1-class.8hbxt.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of PydanticMetadata">[Overview of PydanticMetadata](/.swm/overview-of-pydanticmetadata.azkqv.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of the SecretField Class">[Overview of the SecretField Class](/.swm/overview-of-the-secretfield-class.c6bub.sw.md)</SwmLink>
- <SwmLink doc-title="_ErrorDictRequired Overview">[_ErrorDictRequired Overview](/.swm/_errordictrequired-overview.95mkz.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of StandardDataclass">[Overview of StandardDataclass](/.swm/overview-of-standarddataclass.lt7ul.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of the ConstrainedBytes Class">[Overview of the ConstrainedBytes Class](/.swm/overview-of-the-constrainedbytes-class.761ko.sw.md)</SwmLink>
- <SwmLink doc-title="ConstrainedStr Class Overview">[ConstrainedStr Class Overview](/.swm/constrainedstr-class-overview.9llax.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of Schema Validation">[Overview of Schema Validation](/.swm/overview-of-schema-validation.zao89x5e.sw.md)</SwmLink>
- <SwmLink doc-title="Exploring Data Parsing in Pydantic">[Exploring Data Parsing in Pydantic](/.swm/exploring-data-parsing-in-pydantic.td8sxyhd.sw.md)</SwmLink>
- <SwmLink doc-title="Getting Started with Error Handling">[Getting Started with Error Handling](/.swm/getting-started-with-error-handling.t7oyjwa8.sw.md)</SwmLink>
- <SwmLink doc-title="Understanding Data Serialization in Pydantic">[Understanding Data Serialization in Pydantic](/.swm/understanding-data-serialization-in-pydantic.qydp80xf.sw.md)</SwmLink>
- <SwmLink doc-title="Understanding Type Adapters">[Understanding Type Adapters](/.swm/understanding-type-adapters.vewtwpez.sw.md)</SwmLink>
- <SwmLink doc-title="Introduction to Dataclasses">[Introduction to Dataclasses](/.swm/introduction-to-dataclasses.7lwzu6dt.sw.md)</SwmLink>
- <SwmLink doc-title="Getting Started with Decorators">[Getting Started with Decorators](/.swm/getting-started-with-decorators.82cexcm9.sw.md)</SwmLink>
- <SwmLink doc-title="Basic Concepts of Validators">[Basic Concepts of Validators](/.swm/basic-concepts-of-validators.9s5kyplz.sw.md)</SwmLink>

### Experimental Features

Experimental features in Pydantic are potential new functionalities that are currently being tested and refined. They are not stable and may undergo significant changes or even removal. These features are housed in the 'experimental' module of Pydantic. This module is explicitly marked as experimental to warn users about the potential instability and changes in its features. The code in this module, such as the pipeline API functionality, is subject to change and should be used with caution.

- <SwmLink doc-title="Getting Started with Experimental Features">[Getting Started with Experimental Features](/.swm/getting-started-with-experimental-features.2urdcgk3.sw.md)</SwmLink>

### Plugin Development

Plugin development in Pydantic involves creating classes that adhere to specific protocols defined in the <SwmPath>[pydantic/plugin/\__init_\_.py](/pydantic/plugin/__init__.py)</SwmPath> file. These protocols define the interface for Pydantic plugins and include methods that plugins should implement. The plugins are loaded and managed by the functions and constants defined in the <SwmPath>[pydantic/plugin/\_loader.py](/pydantic/plugin/_loader.py)</SwmPath> file.

- <SwmLink doc-title="Overview of Plugin Development">[Overview of Plugin Development](/.swm/overview-of-plugin-development.modoipeu.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of Schema Validation">[Overview of Schema Validation](/.swm/overview-of-schema-validation.ra85w7jw.sw.md)</SwmLink>
- <SwmLink doc-title="Understanding the Basics of Plugin Loading">[Understanding the Basics of Plugin Loading](/.swm/understanding-the-basics-of-plugin-loading.s5ywg49n.sw.md)</SwmLink>

### Deprecated Features

Deprecated features are parts of the codebase that are no longer recommended for use and are likely to be removed in future versions. In Pydantic, these features are marked with a special warning called <SwmToken path="/pydantic/deprecated/tools.py" pos="16:1:1" line-data="    DeprecationWarning = PydanticDeprecatedSince20">`DeprecationWarning`</SwmToken>, which is an alias for <SwmToken path="/pydantic/warnings.py" pos="49:2:2" line-data="class PydanticDeprecatedSince20(PydanticDeprecationWarning):">`PydanticDeprecatedSince20`</SwmToken>. This warning is used across different modules in the <SwmPath>[pydantic/deprecated//](/pydantic/deprecated//)</SwmPath> directory to indicate deprecated features. For instance, the <SwmToken path="/pydantic/v1/config.py" pos="80:2:2" line-data="class BaseConfig:">`BaseConfig`</SwmToken> class in <SwmPath>[pydantic/deprecated/config.py](/pydantic/deprecated/config.py)</SwmPath> is marked as deprecated and any attempt to use it will trigger a <SwmToken path="/pydantic/deprecated/tools.py" pos="16:1:1" line-data="    DeprecationWarning = PydanticDeprecatedSince20">`DeprecationWarning`</SwmToken>. Developers are advised to avoid using deprecated features and to migrate their code to use the recommended alternatives.

- <SwmLink doc-title="Introduction to Deprecated Features">[Introduction to Deprecated Features](/.swm/introduction-to-deprecated-features.36f108xw.sw.md)</SwmLink>
- <SwmLink doc-title="Overview of JSON Handling">[Overview of JSON Handling](/.swm/overview-of-json-handling.nfj7fskx.sw.md)</SwmLink>
- <SwmLink doc-title="Understanding Class Validators">[Understanding Class Validators](/.swm/understanding-class-validators.jmejli6n.sw.md)</SwmLink>
- <SwmLink doc-title="Introduction to Internal Copying">[Introduction to Internal Copying](/.swm/introduction-to-internal-copying.m9ri373r.sw.md)</SwmLink>
- <SwmLink doc-title="Understanding Decorators">[Understanding Decorators](/.swm/understanding-decorators.5i1bu8gj.sw.md)</SwmLink>
- <SwmLink doc-title="Getting Started with Configuration">[Getting Started with Configuration](/.swm/getting-started-with-configuration.pcvuelqz.sw.md)</SwmLink>
- **Data Parsing**
  - <SwmLink doc-title="Introduction to Data Parsing">[Introduction to Data Parsing](/.swm/introduction-to-data-parsing.4efahyag.sw.md)</SwmLink>
  - <SwmLink doc-title="Protocol Class Overview">[Protocol Class Overview](/.swm/protocol-class-overview.lr9lw.sw.md)</SwmLink>

## Build Tools

- <SwmLink doc-title="Usage of Makefile in the Project">[Usage of Makefile in the Project](/.swm/usage-of-makefile-in-the-project.wflufswn.sw.md)</SwmLink>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

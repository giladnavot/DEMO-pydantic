---
title: BaseModel Subclass Configuration
---
This document will cover the process of configuring a Pydantic <SwmToken path="/pydantic/mypy.py" pos="486:8:8" line-data="        &quot;&quot;&quot;Configures the BaseModel subclass according to the plugin settings.">`BaseModel`</SwmToken> subclass, which includes:

1. Determining the model config and fields
2. Adding a <SwmToken path="/pydantic/mypy.py" pos="491:7:9" line-data="        * adds a fields-aware signature for the initializer and construct methods">`fields-aware`</SwmToken> signature for the initializer and construct methods
3. Freezing the class if frozen = True
4. Storing the fields, config, and if the class is settings in the mypy metadata for access by subclasses.

```mermaid
graph TD;
subgraph pydantic/mypy.py
  _pydantic_model_class_maker_callback:::mainFlowStyle --> transform
end
subgraph pydantic/mypy.py
  transform:::mainFlowStyle --> collect_fields_and_class_vars
end
subgraph pydantic/mypy.py
  collect_fields_and_class_vars:::mainFlowStyle --> collect_field_or_class_var_from_stmt
end
subgraph pydantic/mypy.py
  collect_field_or_class_var_from_stmt:::mainFlowStyle --> _infer_dataclass_attr_init_type
end
  _infer_dataclass_attr_init_type:::mainFlowStyle --> ...

 ClassDef mainFlowStyle color:#000000,fill:#7CB9F4
  ClassDef rootsStyle color:#000000,fill:#00FFF4

%% Swimm:
%% graph TD;
%% subgraph <SwmPath>[pydantic/mypy.py](/pydantic/mypy.py)</SwmPath>
%%   _pydantic_model_class_maker_callback:::mainFlowStyle --> transform
%% end
%% subgraph <SwmPath>[pydantic/mypy.py](/pydantic/mypy.py)</SwmPath>
%%   transform:::mainFlowStyle --> <SwmToken path="/pydantic/mypy.py" pos="498:10:10" line-data="        fields, class_vars = self.collect_fields_and_class_vars(config, is_root_model)">`collect_fields_and_class_vars`</SwmToken>
%% end
%% subgraph <SwmPath>[pydantic/mypy.py](/pydantic/mypy.py)</SwmPath>
%%   collect_fields_and_class_vars:::mainFlowStyle --> <SwmToken path="/pydantic/mypy.py" pos="704:3:3" line-data="    def collect_field_or_class_var_from_stmt(  # noqa C901">`collect_field_or_class_var_from_stmt`</SwmToken>
%% end
%% subgraph <SwmPath>[pydantic/mypy.py](/pydantic/mypy.py)</SwmPath>
%%   collect_field_or_class_var_from_stmt:::mainFlowStyle --> <SwmToken path="/pydantic/mypy.py" pos="829:3:3" line-data="    def _infer_dataclass_attr_init_type(self, sym: SymbolTableNode, name: str, context: Context) -&gt; Type | None:">`_infer_dataclass_attr_init_type`</SwmToken>
%% end
%%   _infer_dataclass_attr_init_type:::mainFlowStyle --> ...
%% 
%%  <SwmToken path="/pydantic/mypy.py" pos="23:1:1" line-data="    ClassDef,">`ClassDef`</SwmToken> mainFlowStyle color:#000000,fill:#7CB9F4
%%   <SwmToken path="/pydantic/mypy.py" pos="23:1:1" line-data="    ClassDef,">`ClassDef`</SwmToken> rootsStyle color:#000000,fill:#00FFF4
```

<SwmSnippet path="/pydantic/mypy.py" line="485">

---

# Determining the model config and fields

The function <SwmToken path="/pydantic/mypy.py" pos="485:3:3" line-data="    def transform(self) -&gt; bool:">`transform`</SwmToken> is used to configure the <SwmToken path="/pydantic/mypy.py" pos="486:8:8" line-data="        &quot;&quot;&quot;Configures the BaseModel subclass according to the plugin settings.">`BaseModel`</SwmToken> subclass according to the plugin settings. It determines the model config and fields, adds a <SwmToken path="/pydantic/mypy.py" pos="491:7:9" line-data="        * adds a fields-aware signature for the initializer and construct methods">`fields-aware`</SwmToken> signature for the initializer and construct methods, freezes the class if frozen = True, and stores the fields, config, and if the class is settings in the mypy metadata for access by subclasses.

```python
    def transform(self) -> bool:
        """Configures the BaseModel subclass according to the plugin settings.

        In particular:

        * determines the model config and fields,
        * adds a fields-aware signature for the initializer and construct methods
        * freezes the class if frozen = True
        * stores the fields, config, and if the class is settings in the mypy metadata for access by subclasses
        """
        info = self._cls.info
        is_root_model = any(ROOT_MODEL_FULLNAME in base.fullname for base in info.mro[:-1])
        config = self.collect_config()
        fields, class_vars = self.collect_fields_and_class_vars(config, is_root_model)
        if fields is None or class_vars is None:
            # Some definitions are not ready. We need another pass.
            return False
        for field in fields:
            if field.type is None:
                return False

```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/mypy.py" line="625">

---

# Collecting fields for the model

The function <SwmToken path="/pydantic/mypy.py" pos="625:3:3" line-data="    def collect_fields_and_class_vars(">`collect_fields_and_class_vars`</SwmToken> is used to collect the fields for the model, accounting for parent classes. It first collects fields and <SwmToken path="/pydantic/mypy.py" pos="631:12:12" line-data="        # First, collect fields and ClassVars belonging to any class in the MRO, ignoring duplicates.">`ClassVars`</SwmToken> belonging to any class in the MRO, ignoring duplicates. Then, it collects fields and <SwmToken path="/pydantic/mypy.py" pos="631:12:12" line-data="        # First, collect fields and ClassVars belonging to any class in the MRO, ignoring duplicates.">`ClassVars`</SwmToken> belonging to the current class.

```python
    def collect_fields_and_class_vars(
        self, model_config: ModelConfigData, is_root_model: bool
    ) -> tuple[list[PydanticModelField] | None, list[PydanticModelClassVar] | None]:
        """Collects the fields for the model, accounting for parent classes."""
        cls = self._cls

        # First, collect fields and ClassVars belonging to any class in the MRO, ignoring duplicates.
        #
        # We iterate through the MRO in reverse because attrs defined in the parent must appear
        # earlier in the attributes list than attrs defined in the child. See:
        # https://docs.python.org/3/library/dataclasses.html#inheritance
        #
        # However, we also want fields defined in the subtype to override ones defined
        # in the parent. We can implement this via a dict without disrupting the attr order
        # because dicts preserve insertion order in Python 3.7+.
        found_fields: dict[str, PydanticModelField] = {}
        found_class_vars: dict[str, PydanticModelClassVar] = {}
        for info in reversed(cls.info.mro[1:-1]):  # 0 is the current class, -2 is BaseModel, -1 is object
            # if BASEMODEL_METADATA_TAG_KEY in info.metadata and BASEMODEL_METADATA_KEY not in info.metadata:
            #     # We haven't processed the base class yet. Need another pass.
            #     return None, None
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/mypy.py" line="704">

---

# Getting pydantic model field from statement

The function <SwmToken path="/pydantic/mypy.py" pos="704:3:3" line-data="    def collect_field_or_class_var_from_stmt(  # noqa C901">`collect_field_or_class_var_from_stmt`</SwmToken> is used to get a pydantic model field from a statement. It checks if the statement is a valid field name and if it is not a class var or <SwmToken path="/pydantic/mypy.py" pos="785:6:6" line-data="        # x: InitVar[int] is not supported in BaseModel">`InitVar`</SwmToken>. If it is a valid field, it infers the **init** argument type for the attribute.

```python
    def collect_field_or_class_var_from_stmt(  # noqa C901
        self, stmt: AssignmentStmt, model_config: ModelConfigData, class_vars: dict[str, PydanticModelClassVar]
    ) -> PydanticModelField | PydanticModelClassVar | None:
        """Get pydantic model field from statement.

        Args:
            stmt: The statement.
            model_config: Configuration settings for the model.
            class_vars: ClassVars already known to be defined on the model.

        Returns:
            A pydantic model field if it could find the field in statement. Otherwise, `None`.
        """
        cls = self._cls

        lhs = stmt.lvalues[0]
        if not isinstance(lhs, NameExpr) or not _fields.is_valid_field_name(lhs.name) or lhs.name == 'model_config':
            return None

        if not stmt.new_syntax:
            if (
```

---

</SwmSnippet>

<SwmSnippet path="/pydantic/mypy.py" line="829">

---

# Inferring **init** argument type for an attribute

The function <SwmToken path="/pydantic/mypy.py" pos="829:3:3" line-data="    def _infer_dataclass_attr_init_type(self, sym: SymbolTableNode, name: str, context: Context) -&gt; Type | None:">`_infer_dataclass_attr_init_type`</SwmToken> is used to infer **init** argument type for an attribute. It performs a <SwmToken path="/pydantic/mypy.py" pos="839:7:9" line-data="        # Perform a simple-minded inference from the signature of __set__, if present.">`simple-minded`</SwmToken> inference from the signature of **set**, if present.

```python
    def _infer_dataclass_attr_init_type(self, sym: SymbolTableNode, name: str, context: Context) -> Type | None:
        """Infer __init__ argument type for an attribute.

        In particular, possibly use the signature of __set__.
        """
        default = sym.type
        if sym.implicit:
            return default
        t = get_proper_type(sym.type)

        # Perform a simple-minded inference from the signature of __set__, if present.
        # We can't use mypy.checkmember here, since this plugin runs before type checking.
        # We only support some basic scanerios here, which is hopefully sufficient for
        # the vast majority of use cases.
        if not isinstance(t, Instance):
            return default
        setter = t.type.get('__set__')
        if setter:
            if isinstance(setter.node, FuncDef):
                super_info = t.type.get_containing_type_info('__set__')
                assert super_info
```

---

</SwmSnippet>

&nbsp;

*This is an auto-generated document by Swimm AI 🌊 and has not yet been verified by a human*

<SwmMeta version="3.0.0" repo-id="Z2l0aHViJTNBJTNBREVNTy1weWRhbnRpYyUzQSUzQWdpbGFkbmF2b3Q=" repo-name="DEMO-pydantic"><sup>Powered by [Swimm](https://app.swimm.io/)</sup></SwmMeta>

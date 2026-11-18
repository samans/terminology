---
title: "YANG and UML Terminology: Comparison and Analysis"
date: 2026-11-18
---

# YANG and UML Terminology: Comparison and Analysis

**Sources checked**

- RFC 7950, *The YANG 1.1 Data Modeling Language*, §3 "Terminology" (`rfc7950.txt`)
- OMG *Unified Modeling Language* v2.5.1, formal/17-12-05 (`formal-17-12-05.pdf`). The file is 2.5.1, not 2.5. The clauses cited below are the same in both versions.

**Question:** Given the YANG terminology (model, module, submodule) in IETF RFCs, including RFC 7950, how does it compare with UML 2.5 terminology? Is there a consistent set of terminology that can describe both?

**Short answer:** No. A single consistent vocabulary does not exist. The two languages share a few close pairs (module/Package, leaf/Property, rpc/Operation, notification/Signal, must/Constraint). They differ in several structural ways, and some YANG mechanisms have no UML equivalent. The practical solution is an explicit mapping layer with a fixed glossary, not a shared vocabulary.

---

## 1. YANG terminology (RFC 7950 §3)

Verbatim definitions of the terms that matter most:

| Term | RFC 7950 definition |
|---|---|
| module | "A YANG module defines hierarchies of schema nodes. With its definitions and the definitions it imports or includes from elsewhere, a module is self-contained and 'compilable'." |
| submodule | "A partial module definition that contributes derived types, groupings, data nodes, RPCs, actions, and notifications to a module. A YANG module can be constructed from a number of submodules." |
| data model | "A data model describes how data is represented and accessed." |
| schema tree | "The definition hierarchy specified within a module." |
| data tree | "An instantiated tree of any data modeled with YANG, e.g., configuration data, state data, ... RPC or action input, RPC or action output, or notification." |
| schema node | "A node in the schema tree. One of action, container, leaf, leaf-list, list, choice, case, rpc, input, output, notification, anydata, and anyxml." |
| data node | "A node in the schema tree that can be instantiated in a data tree. One of container, leaf, leaf-list, list, anydata, and anyxml." |
| augment | "Adds new schema nodes to a previously defined schema node." |
| grouping | "A reusable set of schema nodes, which may be used locally in the module and by other modules that import from it. The 'grouping' statement is not a data definition statement and, as such, does not define any nodes in the schema tree." |
| uses | "...used to instantiate the set of schema nodes defined in a 'grouping' statement. The instantiated nodes may be refined and augmented..." |
| identity | "A globally unique, abstract, and untyped name." |
| feature | "A mechanism for marking a portion of the model as optional. Definitions can be tagged with a feature name and are only valid on servers that support that feature." |
| server deviation | "A failure of the server to implement a module faithfully." |
| container | "An interior data node that exists in at most one instance in the data tree. A container has no value, but rather a set of child nodes." |
| list | "An interior data node that may exist in multiple instances in the data tree." |
| leaf / leaf-list | A data node with a value and no child nodes. A leaf-list is a set of such nodes. |

**Notes on "model" in YANG**

- RFC 7950 does not define "model" on its own. It defines "data model".
- "Model" is used as a verb and adjective ("modeled with YANG").
- "YANG model" is informal usage. It normally means a module or a set of modules.

Related RFCs (not re-read for this analysis; cited from background knowledge):

- RFC 8199: YANG module classification (element, network service, network element models).
- RFC 8309: service models.
- RFC 8342: NMDA datastores.

## 2. UML 2.5.1 terminology

| Term | UML 2.5.1 definition (section) |
|---|---|
| Model | "A model captures a view of a physical system. It is an abstraction of the physical system, with a certain purpose..." (§12.4.4). It has a `viewpoint : String [0..1]` attribute. |
| Package | "A Package is a namespace for its members, which comprise those elements associated via packagedElement (which are said to be owned or contained), and those imported." (§12.2.3.1). It can have a URI as a unique identifier. The URI need not be dereferenceable. |
| PackageMerge | "A directed relationship between two Packages that indicates that the contents of the target mergedPackage are combined into the source receivingPackage..." It is "very similar to Generalization" (§12.2.3.2). |
| Profile | "A profile defines limited extensions to a reference metamodel with the purpose of adapting the metamodel to a specific platform or domain." (§12.4.7) |
| Stereotype | "...defines how an existing metaclass may be extended..." (§12.3.3, §12.4.9) |
| Component | "A modular part of a system that encapsulates its contents and whose manifestation is replaceable within its environment." (§11) |
| Class | "A Class classifies a set of objects and specifies the features that characterize the structure and behavior of those objects." |
| DataType | "A type whose instances are identified only by their value." |
| Property | A StructuralFeature (attribute of a Classifier, or end of an Association). |
| Signal | "A specification of a kind of communication between objects in which a reaction is asynchronously triggered in the receiver without a reply." |
| Constraint | "A condition or restriction expressed in natural language text or in a machine readable language..." |
| InstanceSpecification | "A model element that represents an instance in a modeled system." |

## 3. Mapping between the two

Fit ratings: **Good**, **Approximate**, **Weak**, **None**.

| YANG | Closest UML 2.5.1 concept | Fit | Comment |
|---|---|---|---|
| module | Package | Approximate | Both are namespaces. A YANG module is also "self-contained and compilable" and is identified by name and revision. A UML Package has none of those properties. The optional Package URI is the nearest counterpart to a YANG namespace. |
| submodule | Package nested in or merged into another Package | Weak | UML has no "partial package" concept. |
| data model (set of modules) | Model | Approximate | UML `Model` is a purpose-driven view with a `viewpoint`. YANG "data model" is only "how data is represented and accessed". |
| schema tree | Class model (Classes, Properties, Associations) | Approximate | |
| data tree | InstanceSpecification graph (object model) | Good | |
| container | Class plus a composite Property | Approximate | A YANG container is a position in a tree, not a reusable type. |
| list | Class plus a multivalued composite Property with key identifiers | Good | |
| leaf, leaf-list | Property typed by a DataType, with multiplicity 0..1 or 0..* | Good | |
| typedef, built-in type | DataType, PrimitiveType, Enumeration | Good | |
| grouping / uses | None. The nearest are an abstract Class with Generalization, or a template. | Weak | `uses` instantiates a copy and allows refinement. It is not inheritance. |
| identity | Stereotype, or a Class hierarchy used as a marker | Weak | An identity is "abstract and untyped". UML Enumeration literals are values, so Enumeration is a poor fit. |
| augment | PackageMerge, or Profile/Stereotype | Weak | `augment` adds nodes to a single node from outside. `PackageMerge` merges whole packages by rule. |
| deviation | None | None | |
| feature / if-feature | Profile, or an OCL Constraint | Weak | |
| must, when | Constraint (OCL) | Good | |
| leafref, instance-identifier | Association or reference Property | Approximate | |
| rpc, action | Operation | Good | |
| notification | Signal | Good | |
| extension | Stereotype / Profile | Approximate | |

## 4. Is there a consistent common terminology?

No. The reasons, each checked against the source text where possible:

1. **"Model" is not the same concept.** In UML, `Model` is a specific metaclass with a purpose and a viewpoint. In YANG, only "data model" is defined, and it is a general description of how data is represented and accessed.
2. **A module is compilable, and a package is not.** The RFC 7950 module definition includes self-containment and compilability. A UML Package is only a namespace.
3. **YANG schema nodes are not types.** A `container` is both the definition and its single position in the tree. UML separates classifiers from their usages.
4. **UML has Generalization, and YANG has no inheritance.** YANG reuses definitions by instantiating copies (`grouping`/`uses`) and can then refine them.
5. **Some YANG mechanisms have no UML equivalent.** These are `deviation`, `feature` and, in strict form, `augment`. `PackageMerge` only approximates `augment`. The UML spec itself describes merge as "very similar to Generalization".
6. **Submodule behaviour is only partly verified.** The §3 definition says a submodule contributes to a module. The "belongs-to" and namespace rules are in §7.2, which was not re-read for this analysis.

## 5. Corrections made during verification

- The PDF is UML **2.5.1**, not 2.5.
- The mapping of `augment` to `PackageMerge` was initially rated better than it deserves. The definitions show it is weak.
- `identity` maps better to a Stereotype or marker hierarchy than to an Enumeration, because an identity is an untyped name and not a value.
- RFC 7950 does not define "model" on its own, only "data model".

## 6. Recommendations

1. **Use a mapping layer, not a shared vocabulary.** Fix a glossary for any joint document:
   - "YANG module" corresponds to "UML Package".
   - Use "UML Model" only for the metaclass. Use "data model" or "set of YANG modules" on the YANG side.
   - Use "schema node" for the YANG definition and "data node" or "instance" for the instantiated node.
   - Use "type" for YANG typedefs and UML DataTypes.
2. **State the gaps explicitly.** Document that `grouping`, `augment`, `deviation`, `feature` and `identity` have no clean UML equivalent.
3. **Use a UML Profile for YANG constructs.** For example, `«yang-module»` on Package, `«yang-container»` and `«yang-list»` on Class, `«yang-identity»` on Class, and `«yang-feature»` on Property or Package.
4. **Accept lossy round trips.** UML to YANG loses Generalization. YANG to UML loses reuse structure, augmentation and deviation.

## 7. What was not verified

- RFC 7950 §7.1 and §7.2 (module and submodule statements, including `belongs-to`) were not re-read.
- UML clause numbers for Stereotype and Component were taken from search results and not read in context. Check them before quoting.
- RFC 8199, RFC 8309, RFC 8342 and any IETF or ONF YANG-to-UML mapping work were cited from background knowledge, not from the files in this workspace.

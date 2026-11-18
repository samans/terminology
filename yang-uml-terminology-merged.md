---
title: "YANG and UML Terminology: Merged Comparison and Recommendations"
date: 2026-11-18
---

# YANG and UML Terminology: Merged Comparison and Recommendations

**Sources verified against the workspace files**

- RFC 7950, *The YANG 1.1 Data Modeling Language*: §3 Terminology, §7.1.5 `import`, §7.1.6 `include`, §7.2 `submodule`, §7.2.2 `belongs-to` (`rfc7950.txt`).
- RFC 8199, *YANG Module Classification*: §1, §1.1, §2, §3 (`rfc8199.txt`).
- RFC 8309, *Service Models Explained*: §1, §2, §6.1 (`rfc8309.txt`).
- RFC 8342, *NMDA (Network Management Datastore Architecture)*: §3 Terminology (`rfc8342.txt`).
- OMG *Unified Modeling Language* v2.5.1, formal/17-12-05 (`formal-17-12-05.pdf`). The file is 2.5.1, not 2.5. The terminology in the clauses cited is the same in both versions.

**Other inputs**

- A first analysis (`yang-uml-terminology.md`): verified definitions, mapping table with fit ratings.
- An analysis from another AI engine (`YANG_and_UML_Terminology_Comparison-co-pilot.docx`): neutral cross-language vocabulary, information-model / data-model levels.

This document keeps the verified definitions from the first analysis and adds the neutral vocabulary and information-model/data-model material from the second. Where the two disagreed, the rationale is given in §8.

**Question:** How does YANG terminology (model, module, submodule) compare with UML 2.5 terminology, and can one consistent set of terms describe both?

**Short answer:**

- **No set of terms is equivalent across the two languages.** A YANG module is close to a UML Package, not to a UML Model. Several YANG mechanisms (`grouping`, `deviation`, `feature`, and strictly `augment`) have no clean UML counterpart.
- **A neutral umbrella vocabulary above both languages is possible.** Use *modeling language, model, model artifact, model element* (plus *information model* and *data model*) as shared terms. Keep the language-specific terms (module, submodule, package, class, container, and so on) below them. Treat "YANG module = UML Package" as an analogy only.

---

## 1. YANG terminology (RFC 7950)

### 1.1 Definitions from §3

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
| uses | "...used to instantiate the set of schema nodes defined in a 'grouping' statement. The instantiated nodes may be refined and augmented to tailor them to any specific needs." |
| identity | "A globally unique, abstract, and untyped name." |
| feature | "A mechanism for marking a portion of the model as optional. Definitions can be tagged with a feature name and are only valid on servers that support that feature." |
| server deviation | "A failure of the server to implement a module faithfully." |
| container | "An interior data node that exists in at most one instance in the data tree. A container has no value, but rather a set of child nodes." |
| list | "An interior data node that may exist in multiple instances in the data tree." |
| leaf / leaf-list | A data node that has a value and no child nodes. A leaf-list defines a set of such nodes. |

### 1.2 Module and submodule relationships (§7.1.5, §7.1.6, §7.2, §7.2.2)

| Statement | RFC 7950 text |
|---|---|
| `import` (§7.1.5) | "...makes definitions from one module available inside another module or submodule." The importing module may use groupings, typedefs, extensions, features and identities from the imported module, and may use its nodes in `must`, `path` and `when` statements and as targets of `augment` and `deviation`. A mandatory `prefix` is scoped to the importing module or submodule. |
| `include` (§7.1.6) | "...used to make content from a submodule available to that submodule's parent module." "When a module includes a submodule, it incorporates the contents of the submodule into the node hierarchy of the module." A module may only include submodules that belong to it. |
| `submodule` (§7.2) | "While the primary unit in YANG is a module, a YANG module can itself be constructed out of several submodules. Submodules allow a module designer to split a complex model into several pieces where all the submodules contribute to a single namespace, which is defined by the module that includes the submodules." |
| `belongs-to` (§7.2.2) | Names the module to which the submodule belongs. "A submodule MUST only be included by either the module to which it belongs or another submodule that belongs to that module." |

### 1.3 What "model" means in YANG

- RFC 7950 does not define "model" on its own. It defines **data model**, which is general: "how data is represented and accessed".
- "Model" is used as a verb and adjective ("modeled with YANG").
- "YANG model" is informal and does not map one-to-one to a `.yang` file. One data model can be realized by several modules, and one module can be split into submodules. So **model ≠ module ≠ file**.

### 1.4 RFC 8199: YANG module classification

RFC 8199 is a taxonomy "to simplify human communication around YANG modules". It reuses the RFC 7950 definitions of *data model* and *module* (§1.1) and classifies **modules** (not models) in two dimensions:

| Dimension | Classes | RFC 8199 text |
|---|---|---|
| Abstraction layer (§2) | Network Element YANG Module | "...describe the configuration, state data, operations, and notifications of specific device-centric technologies or features." |
| | Network Service YANG Module | "...describe the configuration, state data, operations, and notifications of abstract representations of services implemented on one or multiple network elements." |
| Origin type (§3) | Standard YANG Modules | Published by SDOs, lifecycle driven by the specification editing cycle. |
| | Vendor-Specific YANG Modules and Extensions | Developed to support a specific set of implementations under one organization's control. |
| | User-Specific YANG Modules and Extensions | Developed by organizations that operate YANG-based infrastructure. |

Points that matter for terminology:

- **The IETF's own usage treats the module as the unit of classification.** A "Network Service YANG Module" is a module, so "module" is the working artifact term.
- **`augment` is the extension mechanism for the origin dimension.** §3.2: vendors extend standard modules "using the 'augment' statement that allows locally defined data trees to be added into locations in externally defined data trees". Figure 2 shows user extensions augmenting vendor extensions, which augment standard modules.
- **Classification is by intent.** §1: "it is the module developer that classifies the module according to the initial intent of the module content." The same L2VPN module might be either a service or an element module.
- RFC 8199 is Informational, and says its boundaries are "at times blurry".

### 1.5 RFC 8309: service models

RFC 8309 builds on RFC 8199 and defines how "service model" is used. Terms from §1 and §2:

| Term | RFC 8309 text |
|---|---|
| Data Model | Uses the RFC 7950 "data model" and "YANG module". It also takes the information model (IM) and data model (DM) concepts from RFC 3444 (see §3.2 below). |
| Service Model | "A service model is a specific type of data model. It describes a service and the parameters of the service in a portable way that can be used uniformly and independent of the equipment and operating environment." |
| Customer Service Model | Describes a service "as offered or delivered to a customer by a network operator". It is "technology agnostic" and is not sent to network devices. |
| Service Delivery Model | "...used by a network operator to define and manage how a service is engineered in the network." Its YANG modules "are sometimes referred to as 'network service YANG modules' [RFC8199]". |

Points that matter for terminology:

- **RFC 8309 separates the model from the language.** §3: "The language in which a customer service model is described is a choice for whoever specifies the model. The IETF uses the YANG data modeling language." This supports the neutral hierarchy in §3, which separates *model* from *modeling language*.
- **RFC 8309 states the model/module distinction directly.** §1: RFC 7950 "makes a distinction between a 'data model'... and a 'YANG module'... YANG structures data models into modules for ease of use." This matches the model ≠ module ≠ file point in §1.3.
- **The two RFCs overlap in terminology.** RFC 8309 §6.1 compares RFC 8199's "Network Service YANG Module" with its own "service model". It states that network service YANG modules "are used to construct the service delivery models" and are used between the service orchestrator or OSS/BSS and the network orchestrator. The term "service module" therefore means different things in the two RFCs unless the context is stated.
- **Extension is again by augmentation.** Both customer service models and service delivery models are described as a core set of parameters, with operator- or vendor-specific features "defined in extensions or augmentations of the module".

### 1.6 RFC 8342: datastores and NMDA

RFC 8342 §3 revises definitions from RFC 6241 and RFC 7950. It states that the revised definitions are "semantically equivalent" and "expected to replace" the originals when those RFCs are revised. Key terms:

| Term | RFC 8342 definition |
|---|---|
| datastore | "A conceptual place to store and access information... A datastore maps to an instantiated YANG data tree." |
| datastore schema | "The combined set of schema nodes for all modules supported by a particular datastore, taking into consideration any deviations and enabled features for that datastore." |
| configuration | "Data that is required to get a device from its initial default state into a desired operational state. This data is modeled in YANG using 'config true' nodes." |
| system state | Data that is not configuration, "modeled in YANG using 'config false' nodes". |
| operational state | "The combination of applied configuration and system state." |
| running, candidate, startup, intended, operational | Named datastores, written `<running>`, `<candidate>`, `<startup>`, `<intended>` and `<operational>`. |
| conventional configuration datastore | One of `<running>`, `<startup>`, `<candidate>`, `<intended>`. They "share a common datastore schema". |

Points that matter for terminology:

- **A datastore is not a data tree and not a module.** It is a conceptual place whose content maps to an instantiated data tree. It sits at the instance level, together with the data tree defined in RFC 7950.
- **Deviations and features act at the datastore schema level.** The schema a datastore exposes depends on which modules it supports, which features are enabled and which deviations apply. This is the clearest statement in the files of what `feature` and `deviation` do at run time.
- **Configuration and state are distinguished by `config true` / `config false`**, not by separate node kinds.

## 2. UML 2.5.1 terminology

| Term | UML 2.5.1 definition (section) |
|---|---|
| Model | "A model captures a view of a physical system. It is an abstraction of the physical system, with a certain purpose..." (§12.4.4). `Model` is a specialization of `Package` (§12.4.4.3). It has a `viewpoint : String [0..1]` attribute. |
| Package | "A Package is a namespace for its members, which comprise those elements associated via packagedElement (which are said to be owned or contained), and those imported." (§12.2.3.1). It may have a URI as a unique identifier, which need not be dereferenceable. |
| PackageMerge | "A directed relationship between two Packages that indicates that the contents of the target mergedPackage are combined into the source receivingPackage..." "Very similar to Generalization" (§12.2.3.2). |
| PackageImport | "A Relationship that imports all the non-private members of a Package into the Namespace owning the PackageImport." |
| ElementImport | Identifies a NamedElement in a Namespace other than the one that owns it, and imports it into the importing Namespace. |
| Profile | "A profile defines limited extensions to a reference metamodel with the purpose of adapting the metamodel to a specific platform or domain." (§12.4.7) |
| Stereotype | "...defines how an existing metaclass may be extended..." (§12.3.3, §12.4.9) |
| Component | "A modular part of a system that encapsulates its contents and whose manifestation is replaceable within its environment." |
| Class | "A Class classifies a set of objects and specifies the features that characterize the structure and behavior of those objects." |
| DataType | "A type whose instances are identified only by their value." |
| Property | A StructuralFeature (attribute of a Classifier, or end of an Association). |
| Signal | "A specification of a kind of communication between objects in which a reaction is asynchronously triggered in the receiver without a reply." |
| Constraint | "A condition or restriction expressed in natural language text or in a machine readable language..." |
| InstanceSpecification | "A model element that represents an instance in a modeled system." |

**Ambiguity in "model".** UML uses "model" in the ordinary sense (an abstraction of a subject) and in the specific sense of the `UML::Model` metaclass. Specifications that cross languages should say which sense is meant. UML also has an XMI serialization (Annex E), which gives UML a file-level artifact that is not itself the `Model` metaclass.

## 3. Neutral vocabulary above both languages

Both the first analysis and the second agree that no direct term equivalence exists. The second analysis adds a layered neutral vocabulary that makes joint specifications easier to write. I adopt it, with the caveat in §3.3.

### 3.1 Neutral hierarchy

**Modeling language → Model → Model artifact → Model element**

| Conceptual level | Neutral term | YANG | UML 2.5.1 |
|---|---|---|---|
| Language | Modeling language | YANG | UML |
| Complete conceptual description | Model | YANG data model, possibly realized by several modules | UML model (ordinary sense), or the `Model` metaclass |
| Individually managed definition | Model artifact | Module or submodule (a `.yang` file) | `Model`, `Package`, or an XMI serialization |
| Namespace or organizational unit | Model unit | Module | Package |
| Subordinate definition unit | Subordinate model artifact | Submodule | Nested Package (approximate) |
| Element within the model | Model element | container, list, leaf, typedef, grouping, rpc, action, notification | Class, Property, Association, DataType, Operation, Signal |
| Reference to another artifact | Dependency / reference | `import` | `PackageImport`, `ElementImport`, Dependency |
| Incorporation from another artifact | Incorporation | `include` (submodule into its module) | Package nesting, or `PackageMerge` depending on semantics |
| Extension or customization | Extension | `augment`, `extension` | Profile and Stereotype, generalization, `PackageMerge` (see §4) |

### 3.2 Information model and data model

The second analysis added a useful distinction that standards work relies on, for example when a UML information model is transformed into a YANG data model. RFC 8309 §2 gives an IETF source for it: it takes the information model (IM) and data model (DM) concepts from RFC 3444 and quotes RFC 3444:

- "The main purpose of an IM is to model managed objects at a conceptual level, independent of any specific implementations or protocols used to transport the data... an IM should hide all protocol and implementation details... it defines relationships between managed objects."
- "DMs, conversely, are defined at a lower level of abstraction and include many details. They are intended for implementors and include protocol-specific constructs."

| Term | Source | Typical language |
|---|---|---|
| Information model | RFC 3444, as quoted in RFC 8309 §2. Conceptual, implementation-independent. | UML |
| Data model | RFC 7950 §3: "how data is represented and accessed". RFC 3444, as quoted in RFC 8309: lower level of abstraction, for implementors. | YANG |
| Service model | RFC 8309 §2: "a specific type of data model", portable and independent of equipment. | YANG, UML or other |

In many telecom standards processes, a UML information model is transformed into a YANG data model, which is implemented through one or more YANG modules and optionally submodules. UML and YANG therefore do not always occupy equivalent positions in an architecture. UML is general-purpose, and YANG is specialized for network-management data and operations.

RFC 3444 itself is not in the workspace. Its text here is only what RFC 8309 quotes.

### 3.3 Caveats

- The neutral terms *modeling language*, *model artifact* and *model element* are **not defined by RFC 7950, RFC 8199, RFC 8309, RFC 8342 or UML 2.5.1**. Define them in your own document, and cite them as your definitions.
- *Information model* and *data model* do have IETF sources: RFC 3444 (as quoted in RFC 8309 §2) and RFC 7950 §3.
- RFC 8199 and RFC 8309 use "module" as the working unit for classification, so "module" is acceptable inside YANG-specific text even when the neutral term is "model artifact".
- "Model artifact" mixes two meanings in the UML column: a **file** (an XMI serialization) and a **namespace/metaclass** (`Model`, `Package`). Keep those separate when writing normative text.
- Do not use "module" as the universal neutral term. "A model consists of one or more modules" is natural for YANG but does not reflect UML terminology.

## 4. Detailed mapping of YANG constructs to UML

Fit ratings: **Good**, **Approximate**, **Weak**, **None**.

| YANG | Closest UML 2.5.1 concept | Fit | Comment |
|---|---|---|---|
| module | Package | Approximate | Both are namespaces. A YANG module is also "self-contained and compilable" and has a name, prefix and revision. A UML Package has none of those. The optional Package URI is the nearest counterpart to a YANG namespace. |
| submodule | Package nested in or merged into another Package | Weak | A submodule "contributes to a single namespace" defined by its module (§7.2) and has no namespace of its own. A UML Package is always a namespace. |
| data model (set of modules) | Model | Approximate | UML `Model` is a purpose-driven view with a `viewpoint`. YANG "data model" describes how data is represented and accessed. |
| schema tree | Class model | Approximate | |
| data tree | InstanceSpecification graph | Good | |
| container | Class plus a composite Property | Approximate | A YANG container is a position in a tree, not a reusable type. |
| list | Class plus a multivalued composite Property with key | Good | |
| leaf, leaf-list | Property typed by DataType, multiplicity 0..1 or 0..* | Good | |
| typedef, built-in type | DataType, PrimitiveType, Enumeration | Good | |
| `import` | PackageImport / ElementImport | Good | Both make definitions from another namespace available. YANG `import` also requires a prefix. |
| `include` | Package nesting, or PackageMerge | Approximate | `include` incorporates a submodule's content into the module's node hierarchy. The UML mapping depends on how the submodule split is modeled. |
| grouping / uses | None. Nearest: abstract Class with Generalization, or a template | Weak | `uses` instantiates a copy and allows refinement. It is not inheritance. |
| identity | Stereotype, or Class hierarchy used as a marker | Weak | An identity is "abstract and untyped". Enumeration literals are values, so Enumeration is a poor fit. |
| augment | PackageMerge, or Profile/Stereotype | Weak | `augment` adds nodes to one existing node from outside. `PackageMerge` merges whole packages by rule. Generalization changes a type's hierarchy and is a different mechanism. |
| deviation | None | None | |
| feature / if-feature | Profile, or an OCL Constraint | Weak | |
| must, when | Constraint (OCL) | Good | |
| leafref, instance-identifier | Association or reference Property | Approximate | |
| rpc, action | Operation | Good | |
| notification | Signal | Good | |
| extension | Stereotype / Profile | Approximate | |
| config true / config false (RFC 8342) | Property `isReadOnly`, or a Stereotype | Weak | UML `isReadOnly` means the value "may not be updated once it has been assigned an initial value". YANG `config false` marks server-supplied system state, which clients cannot write at all. The meanings overlap only partly. |
| datastore (RFC 8342) | No direct equivalent. Nearest: a top-level InstanceSpecification holding an object graph. | Weak | A datastore is "a conceptual place to store and access information" that "maps to an instantiated YANG data tree". UML has no notion of named, protocol-visible stores such as `<running>`. |
| datastore schema (RFC 8342) | A Package or Profile application, after merges and constraints | Weak | It is the combined schema of all modules a datastore supports, after deviations and enabled features. UML has no run-time conformance concept. |
| Network Element / Network Service YANG Module (RFC 8199) | Package classified by Stereotype | Approximate | This is a classification of modules, not a language construct. A Stereotype such as `«network-element»` or `«network-service»` would express it. |
| Standard / Vendor / User module origin (RFC 8199) | Package classified by Stereotype or tagged value | Approximate | Origin is metadata. In YANG, vendor and user extensions are layered with `augment`. |
| Customer service model, service delivery model (RFC 8309) | Model (ordinary sense) classified by purpose, or `Model::viewpoint` | Approximate | RFC 8309 says a service model is "a specific type of data model". UML `Model` has a purpose-driven `viewpoint` attribute, which is the closest counterpart. |

## 5. Why no single equivalent terminology exists

1. **"Model" is not the same concept.** In UML it is a metaclass tied to a purpose and viewpoint. In YANG only "data model" is defined, and it is general.
2. **A module is compilable, and a package is not.** The RFC definition includes self-containment and compilability.
3. **YANG schema nodes are not types.** A `container` is both the definition and its single position in the tree.
4. **UML has Generalization, and YANG has no inheritance.** YANG reuses definitions by instantiating copies through `uses`.
5. **Some YANG mechanisms have no UML equivalent.** These are `deviation`, `feature` and, in strict form, `augment`.
6. **A submodule is not a namespace.** It contributes to its module's namespace (§7.2), while a UML Package is always a namespace.
7. **The two languages sit at different architectural levels.** UML is general-purpose and often used for information models. YANG is specific to management data models.
8. **IETF classification terms have no UML counterpart.** RFC 8199 (network element / network service, standard / vendor / user) and RFC 8309 (customer service model / service delivery model) classify modules and models by role and origin. UML has no built-in vocabulary for this. A Profile or Stereotype scheme would be needed.
9. **Datastores are an instance-level concept with no UML counterpart.** RFC 8342 defines named datastores (`<running>`, `<operational>` and so on) and a datastore schema that depends on supported modules, enabled features and deviations. UML models neither run-time stores nor conformance of this kind.

## 6. Recommendations

1. **Use the neutral hierarchy in §3.1** in joint specifications, and define each term in your own text.
2. **Keep language-specific terms below it.** Use "YANG module" and "UML Package" as an analogy, never as a formal equivalence.
3. **Use these fixed pairings:**
   - Use "UML Model" only for the metaclass.
   - Use "data model" or "set of YANG modules" on the YANG side.
   - Use "schema node" for the YANG definition and "data node" or "instance" for the instantiated node.
   - Use "type" for YANG typedefs and UML DataTypes.
4. **State the gaps explicitly.** `grouping`, `augment`, `deviation`, `feature` and `identity` have no clean UML equivalent. Document them as a UML Profile (for example `«yang-module»` on Package, `«yang-container»` and `«yang-list»` on Class, `«yang-identity»` on Class).
5. **Distinguish information model from data model**, and say which direction a transformation goes (UML information model to YANG data model). Cite RFC 3444 as quoted in RFC 8309 §2 for the distinction.
6. **Accept lossy round trips.** UML to YANG loses Generalization. YANG to UML loses grouping reuse, augmentation and deviation.
7. **Use the IETF classification terms when describing YANG artifacts.** Say "Network Element YANG Module" or "Network Service YANG Module" (RFC 8199), and "customer service model" or "service delivery model" (RFC 8309). When "service module" is used, say which RFC's meaning applies (RFC 8309 §6.1).
8. **Use the RFC 8342 terms for instance-level discussions.** Use "datastore" for the store, "data tree" for its content and "datastore schema" for the schema it exposes. Do not treat a datastore as a UML Model or a module.
9. **Describe feature and deviation at the datastore schema level.** RFC 8342 places their run-time effect there, which is the clearest way to explain why UML has no equivalent.

## 7. Corrections and improvements made during review

- The PDF is UML **2.5.1**, not 2.5.
- The mapping of `augment` to `PackageMerge` is weak. The definitions show it is not a direct counterpart.
- `identity` fits a Stereotype or marker hierarchy better than an Enumeration.
- RFC 7950 does not define "model" on its own, only "data model".
- The submodule namespace claim is now verified against §7.2: all submodules "contribute to a single namespace, which is defined by the module that includes the submodules". The first analysis had left this unverified.
- `import` and `include` mappings were missing from the first analysis. They are now included and checked against §7.1.5 and §7.1.6.
- UML `Model` is a specialization of `Package` (§12.4.4.3). This is confirmed.
- The second analysis's "Model artifact" row mixes file and namespace meanings. §3.3 splits them.
- RFC 8199, RFC 8309 and RFC 8342 are now read and cited from the workspace files. They were previously cited from background knowledge.
- The earlier statement that no IETF or UML source defines "information model" was too broad. RFC 8309 §2 quotes RFC 3444 for it, and §3.2 now says so.
- RFC 8199 and RFC 8309 both name `augment` as the mechanism for vendor and operator extension, which supports the existing weak rating for `augment` to `PackageMerge` and the reasons given for it.
- RFC 8342 shows that `feature` and `deviation` take effect at the datastore schema level. This sharpens the reason UML has no equivalent.

## 8. Differences between the two source analyses and how they were resolved

| Topic | First analysis | Second analysis (other AI engine) | Resolution in this document |
|---|---|---|---|
| Overall answer | No consistent terminology exists. | A consistent vocabulary can be used via a neutral hierarchy. | Both hold. Terms are not equivalent, but a neutral umbrella layer is workable. See the short answer. |
| Method | Verbatim definitions with clause numbers. | Conceptual structure and recommended definitions, few clause references. | Kept the verified quotes and added the neutral definitions with the caveat that they are not standardized. |
| Coverage of YANG constructs | Broad (grouping, augment, deviation, feature, identity, must, leafref). | High-level terms only, plus import/include and extension. | Kept the broad table and added import and include. |
| `augment` mapping | Weak fit to PackageMerge. | Listed under "extension" with several UML options. | Kept the weak rating and explained why. |
| `identity` mapping | Not a reusable definition like a typedef. Stereotype or marker hierarchy. | Listed as a reusable definition alongside typedef and grouping. | Kept the first analysis, because RFC 7950 defines identity as an untyped name. |
| Information vs data model | Not covered. | Covered, with a five-level table and the UML-to-YANG pipeline. | Added in §3.2. The five-level table is replaced by a simpler two-term definition, because the table mixed layers. |
| Serialization | Not covered. | XMI named as a model artifact. | Included and kept separate from the `Model` metaclass. |
| Submodule | Weak fit, namespace behavior unverified. | Does not establish an independent namespace. | Confirmed by §7.2. |
| IETF classification (RFC 8199, 8309) | Not covered. | Not covered. | Added from the RFC files in §1.4 and §1.5, with mapping rows in §4. |
| Datastores (RFC 8342) | Not covered. | Not covered. | Added from the RFC file in §1.6, with mapping rows in §4. |

## 9. What was not verified

- UML clause numbers for Stereotype (§12.3.3, §12.4.9) and Component (§11) came from search results and were not read in context. Check them before quoting.
- RFC 3444 (information model versus data model) is quoted only as RFC 8309 §2 quotes it. It is not in the workspace. RFC 8309 sections other than §1, §2 and §6.1 and RFC 8199 §4 onward were not read in detail.
- The RFC 8342 sections after §3 (for example the datastore architecture and the `<operational>` semantics) were not read in detail.
- The UML mappings for `config false`, datastore, datastore schema and the RFC 8199 / RFC 8309 classifications are my own judgments. The RFCs do not mention UML.
- IETF or ONF YANG-to-UML mapping work is cited from background knowledge, not from the workspace files.
- The claims about telecom practice (UML information model transformed into a YANG data model) come from the second analysis. They were not checked against the workspace files.
- The URLs in the second analysis's reference list were not opened.

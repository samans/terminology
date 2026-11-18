# YANG and UML Terminology

Comparison of YANG terminology (model, module, submodule and related terms) with UML 2.5.1 terminology, based on IETF RFCs and the OMG UML specification.

## Question

Is there a consistent set of terminology that can describe both YANG and UML?

## Short answer

- The terms are not equivalent across the two languages. A YANG module is close to a UML Package, not to a UML Model. `grouping`, `deviation`, `feature` and, strictly, `augment` have no clean UML counterpart.
- A neutral umbrella vocabulary above both languages is workable: modeling language, model, model artifact, model element, plus information model and data model. Keep the language-specific terms below it, and treat "YANG module = UML Package" as an analogy only.

## Files

### Analysis (start with the merged document)

| File | Description |
|---|---|
| `yang-uml-terminology-merged.md` / `.pdf` | Merged analysis: verified definitions from RFC 7950, 8199, 8309, 8342 and UML 2.5.1, the neutral vocabulary, the detailed mapping table, recommendations and a list of what was not verified. |
| `yang-uml-terminology.md` / `.pdf` | First analysis: verified definitions and a mapping table with fit ratings (RFC 7950 and UML 2.5.1 only). Superseded by the merged document. |
| `YANG_and_UML_Terminology_Comparison-co-pilot.docx` | Analysis from another AI engine (Copilot). Source of the neutral vocabulary and the information-model / data-model discussion. |

### Source documents

| File | Description |
|---|---|
| `rfc7950.txt` | RFC 7950, The YANG 1.1 Data Modeling Language |
| `rfc8199.txt` | RFC 8199, YANG Module Classification |
| `rfc8309.txt` | RFC 8309, Service Models Explained |
| `rfc8342.txt` | RFC 8342, Network Management Datastore Architecture (NMDA) |
| `formal-17-12-05.pdf` | OMG Unified Modeling Language v2.5.1 (formal/17-12-05) |

The `*:Zone.Identifier` files are Windows download markers and can be ignored.

## Key findings

- **Model:** UML `Model` is a metaclass (a specialization of `Package`, §12.4.4) tied to a purpose and viewpoint. RFC 7950 defines only "data model" ("how data is represented and accessed"). Model is not the same as module, and neither is the same as a `.yang` file.
- **Module and submodule:** A module is "self-contained and compilable". A submodule contributes to a single namespace defined by its module (RFC 7950 §7.2), so it is not a namespace in its own right.
- **Extension:** RFC 8199 and RFC 8309 use `augment` for vendor and operator extension. UML `PackageMerge` is only a weak match.
- **Information and data models:** RFC 8309 §2 takes these from RFC 3444. UML is typically used for the information model and YANG for the data model.
- **Datastores:** RFC 8342 defines datastores and datastore schemas. They are instance-level concepts with no UML counterpart.

## Limits

- The UML file is version 2.5.1, not 2.5. The terminology cited is the same.
- Not verified: UML clause numbers for Stereotype and Component, RFC 3444 (quoted only through RFC 8309), later sections of RFC 8309 and RFC 8342, and IETF or ONF YANG-to-UML mapping work.
- The UML mappings for `config false`, datastores and the RFC 8199 / 8309 classifications are the author's judgment. The RFCs do not mention UML.
- The neutral terms (modeling language, model artifact, model element) are not defined by any of the cited standards.

See section 9 of the merged document for the full list.

## Regenerating the PDFs

```bash
pandoc yang-uml-terminology-merged.md -s --metadata pagetitle="YANG and UML Terminology" \
  -c style.css --embed-resources -o out.html
weasyprint out.html yang-uml-terminology-merged.pdf
```

The stylesheet used A4 landscape with 1.5 cm margins, 10 pt sans-serif text and bordered tables. It is not stored in this folder.

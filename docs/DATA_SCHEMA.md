# Data Schema

## 1. Status and Authority

This document is the authoritative human-readable specification for dataset records.

Current draft schema version: `0.4.0-draft`

Project-specific identity, workflow, provenance, privacy, and optional LabelMe fields wrap the core annotation without changing its field names or internal meaning.

## 2. Record Relationships

```text
Building Case
└── one or more Image Records
    └── one Image Annotation File
        ├── one or more Defect Instances
        │   ├── structured visible description
        │   ├── structured cause assessment
        │   ├── structured remedy recommendation
        │   └── optional LabelMe link
        └── annotation history and graphical-annotation status
```

- `case_id` identifies one dwelling or defined residential building group.
- `image_id` identifies one unchanged source image.
- `instance_id` identifies one defect instance within an image.
- All defect instances for one image are stored in the same `_annotation.json` file.
- A LabelMe file is optional and may be added at any annotation stage.

## 3. General Conventions

- Text uses UTF-8.
- Dates use ISO 8601: `YYYY-MM-DD`.
- Use `null` for an optional scalar value that is not yet available.
- Use `[]` for an optional list with no confirmed entries.
- Optional reference fields may be omitted in an export. Importers may also accept an empty spreadsheet cell or empty string and normalize it to `null`.
- Do not guess values merely to complete a field.
- Stable IDs must not be renamed or reused.
- Corrections preserve annotation history or controlled file versions.

## 4. IDs and Filenames

### Building case ID

```text
case-{six-digit number}
```

Example: `case-000001`

### Image ID

```text
{case_id}-{city}-{component}-{four-digit number}
```

Example: `case-000001-meizhou-wall-0001`

The city is a lowercase ASCII name approved for public use during intake. The component is a controlled short code.

### Defect instance ID

```text
{image_id}-d{two-digit number}
```

Example: `case-000001-meizhou-wall-0001-d01`

### Working files

```text
case-000001-meizhou-wall-0001.jpg
case-000001-meizhou-wall-0001_annotation.json
case-000001-meizhou-wall-0001_labelme.json  (optional)
```

## 5. Building Case Record

| Field | Type | Requirement | Meaning |
| --- | --- | --- | --- |
| `schema_version` | string | required | Schema version used by the record. |
| `case_id` | string | required | Stable building-case identifier. |
| `country_or_region` | string or null | required key | Country or broad region. |
| `locality_public` | string or null | required key | Publicly safe locality description. |
| `location_precision` | string | required | Approved public location precision. |
| `building_type` | string or `unknown` | required | Controlled building type. |
| `approximate_date` | string or null | required key | Known or estimated construction period. |
| `current_use` | string or null | required key | Current use when safely known. |
| `case_context` | string or null | optional | Architectural, historical, family, or community context. |
| `consent_status` | string | required | Controlled consent-review status. |
| `privacy_status` | string | required | Controlled privacy-review status. |
| `record_state` | string | required | Current workflow state. |

Exact addresses, precise coordinates, identities, consent evidence, and unredacted metadata remain outside the public record when disclosure creates risk.

## 6. Image Record

| Field | Type | Requirement | Meaning |
| --- | --- | --- | --- |
| `schema_version` | string | required | Schema version used by the record. |
| `image_id` | string | required | Stable ID containing case, public-safe city, component, and sequence. |
| `case_id` | string | required | Parent building case. |
| `file_name` | string | required | Controlled image filename derived from `image_id`. |
| `original_file_name` | string or null | restricted | Original source filename retained for traceability. |
| `capture_date` | string or null | required key | Capture date when safely known. |
| `view_type` | string or `unknown` | required | Controlled view type. |
| `interior_exterior` | string or `unknown` | required | `interior`, `exterior`, or `unknown`. |
| `component` | string or `unknown` | required | Principal visible building component. |
| `material` | string or `unknown` | required | Principal visible material or material combination. |
| `pixel_width` | integer | required | Original image width in pixels. |
| `pixel_height` | integer | required | Original image height in pixels. |
| `rights_status` | string | required | Controlled permission and reuse status. |
| `redaction_status` | string | required | Controlled redaction-review status. |
| `quality_status` | string | required | Controlled image-quality result. |
| `record_state` | string | required | Current workflow state. |

## 7. Image Annotation File

The working filename is `{image_id}_annotation.json`. The same file is updated through coarse annotation, fine annotation, and professional review; it is not renamed at each stage.

### 7.1 Top-level fields

| Field | Type | Requirement | Meaning |
| --- | --- | --- | --- |
| `schema_version` | string | required | Schema version used by the file. |
| `image_id` | string | required | Source image ID. |
| `record_state` | string | required | Current text-annotation workflow state. |
| `annotation_history` | array[object] | required | Ordered annotation and review events. |
| `graphical_annotation` | object | required | Independent status of optional LabelMe annotation. |
| `instances` | array[object] | required | All proposed or confirmed defect instances in the image. |

### 7.2 Annotation history event

Each event contains:

| Field | Type | Requirement |
| --- | --- | --- |
| `stage` | string | required: `coarse_annotation`, `fine_annotation`, or `professional_review` |
| `source` | string | required: `human` or `ai` |
| `tool_or_model` | string or null | required key |
| `annotator_role` | string | required |
| `date` | string or null | required key |
| `result` | string | required, such as `created`, `confirmed`, `corrected`, `reviewed`, or `not_assessable` |

### 7.3 Optional graphical annotation

`graphical_annotation` contains:

| Field | Type | Requirement |
| --- | --- | --- |
| `status` | string | required: `not_created`, `annotated`, or `reviewed` |
| `file` | string or null | required key: `{image_id}_labelme.json` or `null` |
| `tool` | string or null | required key |
| `annotator_role` | string or null | required key |
| `date` | string or null | required key |

Graphical status is independent of `record_state`. A coarse, fine, expert-reviewed, accepted, or published annotation may exist without a LabelMe file.

## 8. Defect Instance Object

Each object in `instances` contains project identity/provenance fields plus the complete structured text annotation.

### 8.1 Project fields

| Field | Type | Requirement | Meaning |
| --- | --- | --- | --- |
| `instance_id` | string | required | Stable defect-instance ID. |
| `coarse_confidence` | number or null | required key | AI coarse-detection confidence, or `null`. This is distinct from professional cause confidence. |
| `geometry` | object | required | Link to optional LabelMe shapes. |

### 8.2 Shared and description fields

The following field names and meanings follow the reference field structure exactly.

| JSON path | Type | Requirement | Meaning |
| --- | --- | --- | --- |
| `label` | string | required | Defect name selected from the approved 40-class controlled vocabulary. |
| `loc` | string | required | Location/construction: elevation or floor plus construction detail or specific position. |
| `mat` | string | optional | Main visible material or material combination; may be omitted when unavailable. |
| `description.morph` | string | required | Visible morphology: area, line, or point form plus observable symptom terms. |
| `description.extent_module` | string | optional | Extent or scale described using building or component modules. |
| `description.context` | string | required | Associated visible signs, such as moisture marks, algae, salt deposits, corrosion, hollowing evidence, or a water path. |
| `description.severity` | string | optional | Severity: `light`, `moderate`, or `severe`. |

`label`, `loc`, and `mat` are shared by the description, cause, and remedy views and must not be duplicated with different values inside those objects.

### 8.3 Cause object

The JSON object is named `cause`.

| JSON path | Type | Requirement | Meaning |
| --- | --- | --- | --- |
| `cause.main_cause` | string | required at professional review | Main cause expressed as a concise mechanism and trigger. |
| `cause.evidence` | string | required at professional review | Supporting rationale connecting visual signs, characteristic patterns, and relevant test evidence. |
| `cause.manifestations` | array[string] | optional | Relevant manifestations of the defect. |
| `cause.confidence` | string | required at professional review | `high`, `medium`, or `low`. |
| `cause.checks` | array[string] | optional | Recommended verification checks, normally one to three items. |
| `cause.risk` | array[string] | optional | Possible risks, normally one to three items. |

Before professional review, required cause fields use `null` and optional arrays use `[]`. A completed professional review uses only `high`, `medium`, or `low` for `cause.confidence`.

### 8.4 Remedy object

The JSON object is named `remedy`.

| JSON path | Type | Requirement | Meaning |
| --- | --- | --- | --- |
| `remedy.target` | string | required at professional review | Conservation or repair objective. |
| `remedy.principles` | array[string] | required at professional review | Applicable conservation or repair principles. |
| `remedy.preconditions` | array[string] | optional | Prerequisites and source-control measures. |
| `remedy.steps` | array[object] | required at professional review | Ordered intervention steps. |
| `remedy.acceptance` | array[string] | optional | Acceptance criteria. |
| `remedy.donts` | array[string] | optional | Prohibited actions or limitations. |
| `remedy.maintenance` | array[string] | optional | Follow-up monitoring and maintenance recommendations. |

Every object in `remedy.steps` uses the exact required keys:

| Key | Type | Requirement | Meaning |
| --- | --- | --- | --- |
| `action` | string | required | Action to perform. |
| `object` | string | required | Material, component, or defect acted upon. |
| `method` | string | required | Method or material used. |
| `params` | string or null | required key | Key parameters; may be `null`. |

Before professional review, `target` uses `null` and all remedy arrays use `[]`.

### 8.5 Optional geometry link

`geometry` contains:

| Field | Type | Requirement |
| --- | --- | --- |
| `labelme_group_id` | integer or null | required key |
| `shape_types` | array[string] | required |

When no LabelMe file exists, use `null` and `[]`. When it exists:

- every shape label exactly matches the current `label`;
- every instance with geometry has a unique integer `group_id` within the image;
- all shapes belonging to the instance use that same `group_id`;
- LabelMe dimensions match the original image.

## 9. Requirements by Workflow State

| Record state | Minimum requirements |
| --- | --- |
| `submitted` | Source image and contribution information available for intake. |
| `privacy_check` | Stable case and city-inclusive image IDs assigned; intake checks recorded. |
| `coarse_annotated` | One annotation file contains proposed instances, visible fields supported by the image, coarse provenance, and AI confidence where applicable. Cause and remedy remain empty. |
| `fine_annotated` | A person has verified the visible fields and added a fine-annotation history event. LabelMe remains optional. |
| `expert_reviewed` | Required cause fields and required remedy fields are completed, or the review is explicitly recorded as `not_assessable`. Optional fields remain empty when unsupported. |
| `accepted` | Schema, privacy, rights, provenance, and quality checks passed. Optional geometry is validated when present. |
| `rejected` | Rejection reason and decision provenance recorded. |
| `published` | Accepted data frozen into a versioned public release. |
| `withdrawn` | Withdrawal information recorded where appropriate. |

## 10. Spreadsheet Export

The English spreadsheet export contains 20 columns corresponding one-to-one with the reference workbook. Export one row per defect instance in this order:

| Column | Spreadsheet header | Source in this schema |
| ---: | --- | --- |
| 1 | `sequence_number` | Export row sequence. |
| 2 | `file_path` | Path to the image named from `image_id`. |
| 3 | `label` | `instances[].label` |
| 4 | `loc (location/construction)` | `instances[].loc` |
| 5 | `mat (material)` | `instances[].mat` |
| 6 | `morph (visible morphology)` | `instances[].description.morph` |
| 7 | `extent_module` | `instances[].description.extent_module` |
| 8 | `context (associated signs)` | `instances[].description.context` |
| 9 | `main_cause (required)` | `instances[].cause.main_cause` |
| 10 | `evidence (required)` | `instances[].cause.evidence` |
| 11 | `confidence (required)` | `instances[].cause.confidence` |
| 12 | `checks (verification recommendations)` | `instances[].cause.checks` |
| 13 | `risk` | `instances[].cause.risk` |
| 14 | `target (repair objective)` | `instances[].remedy.target` |
| 15 | `principles (required)` | `instances[].remedy.principles` |
| 16 | `preconditions (prerequisites/source control)` | `instances[].remedy.preconditions` |
| 17 | `steps (ordered action + object + method/material + parameters)` | `instances[].remedy.steps` |
| 18 | `acceptance (acceptance criteria)` | `instances[].remedy.acceptance` |
| 19 | `donts (prohibited actions/limitations)` | `instances[].remedy.donts` |
| 20 | `maintenance (maintenance recommendations)` | `instances[].remedy.maintenance` |

Arrays and step objects must preserve their ordering when serialized into spreadsheet cells.

The reference field specification additionally defines `description.severity` and `cause.manifestations`. These fields remain in the canonical JSON but are not columns in the current spreadsheet. They must not be silently discarded from the JSON when a spreadsheet export is produced.

## 11. Derived Text Views

Human-readable description, cause, and remedy sentences may be generated from the structured fields. They are derived views, not replacements for the structured JSON. Generated text must not introduce information absent from the stored fields.

## 12. Validation Checklist

Before an annotation is declared schema-compatible, confirm that:

- `label`, `loc`, `mat`, `description`, `cause`, and `remedy` retain the required field names and nesting;
- `description` contains `morph`, optional `extent_module`, `context`, and optional `severity` using `light`, `moderate`, or `severe`;
- `cause` contains `main_cause`, `evidence`, optional `manifestations`, `confidence` using `high`, `medium`, or `low`, `checks`, and `risk`;
- `remedy` contains `target`, `principles`, `preconditions`, ordered `steps`, `acceptance`, `donts`, and `maintenance`;
- every remedy step contains `action`, `object`, `method`, and string-or-null `params`;
- required professional fields are complete at `expert_reviewed` unless explicitly not assessable;
- spreadsheet exports use the exact 20 English headers and order listed above;
- city-inclusive filenames and IDs follow Section 4;
- optional LabelMe data does not alter or replace the structured text annotation.

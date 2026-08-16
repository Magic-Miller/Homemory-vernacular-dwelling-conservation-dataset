# Naming Rules

## Responsibility

Field contributors upload original image files and do **not** need to rename them or create dataset identifiers.

After intake review, the dataset maintainer assigns the stable IDs and working filenames. The original source filename is retained in the controlled intake record.

## Stable IDs

### Building case

```text
case-{six-digit number}
```

Example: `case-000001`

### Image

```text
{case_id}-{city}-{component}-{four-digit number}
```

Example: `case-000001-meizhou-wall-0001`

The city segment uses a short lowercase ASCII name, such as `meizhou`. It is assigned by the dataset maintainer after intake and must be safe for public use. If disclosing the city creates a privacy or conservation risk, the maintainer uses the project's approved restricted-location code instead.

### Defect instance

```text
{image_id}-d{two-digit number}
```

Examples: `case-000001-meizhou-wall-0001-d01` and `case-000001-meizhou-wall-0001-d02`

## Files for One Image

```text
case-000001-meizhou-wall-0001/
├── case-000001-meizhou-wall-0001.jpg
├── case-000001-meizhou-wall-0001_annotation.json
└── case-000001-meizhou-wall-0001_labelme.json  (optional)
```

- `{image_id}_annotation.json` is the single text-annotation file for the image. It contains all defect instances and is updated through coarse annotation, fine annotation, and professional review without being renamed.
- `{image_id}_labelme.json` is optional. It contains graphical annotations created in LabelMe and may be added at any annotation stage.
- A separate file is not created for every defect instance.

## General Rules

- Use lowercase ASCII letters, numbers, and hyphens in IDs.
- Write the city as a consistent lowercase ASCII slug without spaces, for example `meizhou`.
- Use an underscore only before the fixed suffixes `_annotation` and `_labelme`.
- Do not include names, addresses, or other private information.
- Once assigned, an ID must not be renamed or reused.
- Corrections keep the same ID and are recorded through annotation history or controlled file versioning.
- The LabelMe file must use the current labels and instance links in `_annotation.json`.

The current example image follows this rule: [`case-000001-meizhou-wall-0001.jpg`](../examples/annotation_example/case-000001-meizhou-wall-0001.jpg).

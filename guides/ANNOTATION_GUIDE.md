# Annotation Guide

## Quick Start

Your task package should contain:

```text
case-000001-meizhou-wall-0001.jpg
case-000001-meizhou-wall-0001_annotation.json
```

It may also contain or later receive:

```text
case-000001-meizhou-wall-0001_labelme.json
```

### Starting a new annotation

Only a new coarse annotation starts from the [blank annotation template](../templates/ANNOTATION_TEMPLATE.json).

1. Copy `templates/ANNOTATION_TEMPLATE.json` into the assigned task package.
2. Rename the copy as `{image_id}_annotation.json`.
3. Fill in the assigned `image_id` and create the required `instance_id` values.
4. Record whether the coarse annotation was created by a person or AI. For AI annotation, change `source` to `ai` and record the model name and version in `tool_or_model`.

Fine annotators must continue editing the existing `_annotation.json` file. Do not copy the blank template again or create a separate fine-annotation file.

To complete an annotation task:

1. Open the assigned image and `_annotation.json` file.
2. Review the whole image, not only the proposed defect areas.
3. Confirm, correct, remove, or add defect instances in the same annotation file.
4. Describe only what the image visibly supports.
5. Leave cause and remedy fields empty unless you are completing an assigned professional review.
6. If LabelMe work is assigned, draw geometry using the current instance IDs and labels in `_annotation.json`.
7. Complete the checks in Section 9, "Final Check," and return the assigned files.

Do not rename the image or annotation file. If the image ID, annotation file, or approved label list is missing, ask the dataset maintainer rather than creating your own naming system.

## 1. Purpose

This guide tells annotation volunteers how to work with one image and its annotation file. The exact data structure and field requirements are defined in the [Data Schema](../docs/DATA_SCHEMA.md), and filenames follow the [Naming Rules](../docs/TASK_PACKAGE_NAMING.md).

The main principles are:

- one image has one continuing `_annotation.json` file;
- all defects in the image are stored in its `instances` array;
- coarse, fine, and professional stages update the same file without renaming it;
- LabelMe is optional and may be added at any annotation stage;
- visible evidence, professional interpretation, and repair recommendations remain separate.

## 2. Who Completes What

| Field group           | Coarse annotator: human or AI                                      | Fine annotator: human                                       | Professional reviewer                      |
| --------------------- | ------------------------------------------------------------------ | ----------------------------------------------------------- | ------------------------------------------ |
| Image and file IDs    | Use the IDs assigned by the maintainer                             | Confirm; do not rename                                      | Retain                                     |
| Defect `instance_id`  | Create or propose                                                  | Confirm, correct, add, or mark rejected without reusing IDs | Retain                                     |
| `label`, `loc`, `mat` | Propose from visible evidence                                      | Confirm or correct                                          | Correct only when necessary                |
| `description`         | Propose visible morphology, extent, context, and optional severity | Confirm and complete what the image supports                | Review when necessary                      |
| `coarse_confidence`   | Record an AI detection score when available                        | Retain as provenance                                        | Retain                                     |
| LabelMe geometry      | Optional                                                           | Optional                                                    | Optional                                   |
| `cause`               | Leave empty                                                        | Leave empty                                                 | Complete or explicitly mark not assessable |
| `remedy`              | Leave empty                                                        | Leave empty                                                 | Complete or explicitly mark not assessable |
| `annotation_history`  | Add coarse event                                                   | Add fine event                                              | Add professional-review event              |

`fine_annotated` means that a person has verified the visible text annotation. It does not mean that a LabelMe file must exist.

## 3. Before You Start

Confirm that:

- the image opens correctly and has not been resized, rotated, cropped, or edited;
- the image and annotation filenames use the same `image_id`;
- the ID follows `{case_id}-{city}-{component}-{sequence}`;
- the current annotation file opens as valid JSON;
- you have the approved defect-label vocabulary for the task;
- you know whether the assignment is coarse annotation, fine annotation, LabelMe annotation, or professional review.

Do not change `schema_version`, `image_id`, or previously assigned IDs unless the dataset maintainer explicitly instructs you to correct an intake error.

## 4. Working with Defect Instances

One image may show no defect, one defect, or several defects. Every distinct defect instance is one object in `instances`.

Use the image ID plus a sequential suffix:

```text
case-000001-meizhou-wall-0001-d01
case-000001-meizhou-wall-0001-d02
```

Rules:

- use a different `instance_id` for different defect types;
- use different IDs for clearly separate defect occurrences when they need independent description or review;
- do not renumber later instances when an earlier proposal is rejected;
- never reuse a rejected ID for another defect;
- add a new instance to `_annotation.json` before drawing a newly discovered defect in LabelMe.

## 5. Visible Annotation Fields

These fields describe what is visible in the image. Coarse annotators propose them; fine annotators verify them.

| Field | How to complete it | Required for a verified visible annotation |
| --- | --- | --- |
| `label` | Select the exact disease/defect name from the approved 40-class vocabulary. Do not invent synonyms. | Yes |
| `loc` | Describe elevation or floor plus component, construction detail, or specific position. | Yes |
| `mat` | Record the main visible material or material combination. | Optional |
| `description.morph` | Describe point, line, or area form plus visible symptoms such as peeling, loss, cracking, white deposits, or corrosion. | Yes |
| `description.extent_module` | Describe extent using building modules where possible, such as brick courses, brick lengths, or component width. | Optional |
| `description.context` | Record accompanying visible signs such as moisture marks, algae, salt deposits, corrosion, hollowing evidence, or a water path. | Yes |
| `description.severity` | Use only `light`, `moderate`, or `severe` when the image supports a severity judgment. | Optional |

Example of visible fields:

```json
{
  "label": "efflorescence",
  "loc": "east elevation, second floor, window-sill flashing",
  "mat": "plaster/historic coating",
  "description": {
    "morph": "patchy white crystalline deposits following joints and the visible water path",
    "extent_module": "approximately 3–4 brick courses high and 5–6 bricks wide",
    "context": "accompanied by moisture marks and coating loss",
    "severity": "moderate"
  }
}
```

Use `null` for an optional single value that is not available and `[]` for an optional list with no confirmed entries. Do not replace missing information with a guess.

Because `description.context` is required, record a neutral visible observation such as `no clear accompanying signs visible` when the image clearly shows no accompanying sign. Do not use that phrase when the relevant area is obscured or cannot be assessed.

## 6. Coarse and Fine Annotation

Every new entry in `annotation_history` contains:

```json
{
  "stage": "coarse_annotation",
  "source": "human",
  "tool_or_model": null,
  "annotator_role": "annotation volunteer",
  "date": "2026-08-09",
  "result": "created"
}
```

Use an ISO date in `YYYY-MM-DD` format. For AI annotation, change `source` to `ai` and record the model name and version in `tool_or_model`.

### Coarse annotation

A coarse annotation may be created by a person or AI. It is an initial proposal, not verified evidence.

1. Review the entire image.
2. Propose every visible defect instance.
3. Complete the visible fields supported by the image.
4. If AI produced the proposal, record its numeric detection score in `coarse_confidence`; otherwise use `null` when no score exists.
5. Add a `coarse_annotation` event to `annotation_history`.
6. Set `record_state` to `coarse_annotated`.
7. Leave all cause and remedy values empty.

### Fine annotation

Fine annotation must be completed by a person.

1. Compare every coarse proposal with the original image.
2. Confirm correct instances.
3. Correct unsupported labels or descriptions.
4. Add defects that were missed.
5. Mark incorrect proposals through the assigned review method; do not silently reuse their IDs.
6. Confirm all required visible fields and complete optional fields only when supported.
7. Preserve the coarse history and `coarse_confidence`.
8. Add a `fine_annotation` event to `annotation_history` with `source: "human"` and a result such as `confirmed` or `corrected`.
9. Set `record_state` to `fine_annotated` in the same file.

Do not create `_coarse.json`, `_fine.json`, or one file per defect. Continue using `{image_id}_annotation.json`.

## 7. Optional LabelMe Annotation

LabelMe may be created from a coarse, fine, or expert-reviewed annotation. Always read the current `_annotation.json` before drawing.

For each instance that needs geometry:

1. copy the exact `label` from the annotation;
2. assign a unique integer LabelMe `group_id` within the image;
3. record the same integer in `geometry.labelme_group_id` for that instance;
4. record the used shape type in `geometry.shape_types`;
5. use the same `group_id` for multiple shapes belonging to one instance.

Recommended geometry:

- use `linestrip` for a narrow linear crack;
- use `polygon` for an area defect such as staining, efflorescence, material loss, biological growth, or peeling;
- draw only the supported visible area;
- use separate instances for different or overlapping defect types;
- use multiple shapes with the same `group_id` for disconnected areas belonging to one instance.

After saving LabelMe:

- name the file `{image_id}_labelme.json`;
- set `graphical_annotation.status` to `annotated`;
- record the filename, tool, annotator role, and date in `graphical_annotation`;
- confirm that LabelMe `imageWidth` and `imageHeight` match the original image.

Creating geometry alone does not change `record_state` to `fine_annotated`. Only human verification of the visible text fields does that.

When no LabelMe file exists, keep `graphical_annotation.status` as `not_created`, use `null` for its file, tool, role, and date, and keep each instance's geometry as `labelme_group_id: null` and `shape_types: []`.

## 8. Professional Review Boundary

Ordinary coarse, fine, and LabelMe annotators must leave `cause` and `remedy` empty. Do not infer causes or recommend repairs from the image alone.

Before professional review, use:

- `null` for `cause.main_cause`, `cause.evidence`, `cause.confidence`, and `remedy.target`;
- `[]` for `cause.manifestations`, `cause.checks`, `cause.risk`, and every remedy list.

A qualified professional reviewer completes:

- `cause.main_cause` and `cause.evidence`;
- `cause.confidence` using only `high`, `medium`, or `low`;
- optional `cause.manifestations`, `cause.checks`, and `cause.risk`;
- `remedy.target`, `remedy.principles`, and ordered `remedy.steps`;
- optional `remedy.preconditions`, `remedy.acceptance`, `remedy.donts`, and `remedy.maintenance`.

Every remedy step contains:

```json
{
  "action": "action to perform",
  "object": "material, component, or defect acted upon",
  "method": "method or material",
  "params": "key parameters or null"
}
```

The professional cause confidence is different from the numeric AI `coarse_confidence`.

After review, add a `professional_review` event to `annotation_history`. Use `result: "not_assessable"` when the available evidence does not support a professional conclusion. When the required review is complete, set `record_state` to `expert_reviewed` in the same annotation file.

## 9. Final Check

For every annotation task, confirm that:

- the image and annotation use the same city-inclusive `image_id`;
- the annotation filename is `{image_id}_annotation.json`;
- all defects are stored in the same `instances` array;
- every instance has a unique and stable `instance_id`;
- every `label` uses the approved vocabulary;
- required `loc`, `description.morph`, and `description.context` values are complete;
- `mat`, `extent_module`, and `severity` are completed only when supported;
- `severity` uses only `light`, `moderate`, or `severe`;
- missing information has not been guessed;
- coarse and fine history has been preserved;
- non-experts have left cause and remedy empty;
- the JSON opens correctly;
- the file's `record_state` matches the work actually completed.

If a LabelMe file exists, also confirm that:

- its filename is `{image_id}_labelme.json`;
- it opens with the original image;
- each shape label exactly matches the current annotation label;
- each `group_id` links to the correct `instance_id`;
- each used shape type is recorded in `geometry.shape_types`;
- shapes follow the visible defect rather than unrelated background;
- image dimensions and `graphical_annotation` information are correct.

The annotation volunteer does not need to create the 20-column spreadsheet export. Dataset maintainers generate that export from the completed annotation files.

# Dataset Usage

## Intended Uses

Reviewed releases are intended to support:

- documentation of historic vernacular dwellings;
- education and conservation research;
- image classification using reviewed labels and, where graphical annotations are available, detection and segmentation of visible defects;
- evaluation of annotation and quality-control methods;
- training and evaluation of specialized AI models;
- carefully designed decision-support tools that encourage appropriate professional review.

## Unsuitable Uses

The dataset must not be used as the sole basis for:

- structural-safety decisions;
- confirmation of defect causes;
- conservation approval or statutory assessment;
- repair specifications, budgets, or construction instructions;
- actions that expose, target, or intrude upon private occupants or sensitive sites.

## Interpretation

Image annotations primarily describe visible evidence. Possible causes, risks, and conservation knowledge are separate interpretive fields and may be incomplete or contested. Absence of an annotation does not prove absence of a defect.

## Data Splits

Future training, validation, and test splits will be separated at the building-case level rather than by individual image. This is intended to reduce leakage caused by near-duplicate views of the same building appearing in different splits.

## Access and Loading

The reviewed dataset will be released on Hugging Face. Download and loading instructions will be added with the first release. GitHub will remain the source for project documentation, contribution guidance, and issue tracking.

## Known Limitations

Expected limitations include uneven geographic coverage, material and defect imbalance, variations in camera quality, volunteer-selection effects, incomplete historical information, and differing levels of professional review. Each public release should report its known coverage and review status.

## Versioning

Users should cite and record the exact dataset release used. Labels, fields, and case availability may change between versions, while released versions should remain reproducible.

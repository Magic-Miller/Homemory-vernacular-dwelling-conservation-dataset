# Quality Control

## Purpose

Quality control protects the usefulness, traceability, privacy, and credibility of released data. Release criteria will be refined through pilot cases.

## Intake Checks

- Building appears to be within project scope.
- The completed [Contribution and Consent Form](../CONTRIBUTION_AND_CONSENT.txt) and all emailed image parts are stored with the private intake record.
- Required contributor declarations, public-release confirmations, attribution choice, location precision, and any additional property, occupant, custodian, or community permissions are complete.
- Signatures, contact details, and supporting permission evidence remain in the controlled system and are not present in public files.
- Image files open correctly and retain necessary quality.
- Privacy and location risks have been reviewed.
- Duplicate and near-duplicate submissions have been checked.

## Image Checks

Images should be sufficiently clear for their intended purpose. Blur, extreme compression, poor exposure, obstruction, or missing context should be recorded. A technically imperfect image may still be retained when it has unique documentary value, but its limitation must be explicit.

## Annotation Checks

- Labels use the current controlled vocabulary.
- Where graphical annotation exists, geometry matches the visible region or feature and links to the correct defect instance.
- Observable evidence is separated from interpretation.
- Uncertainty and ambiguous cases are recorded.
- AI-generated annotations are identified and human-reviewed.
- Required provenance and status fields are complete.

## Professional Review

Reviewers should record whether an annotation is accepted, revised, disputed, or not assessable. Possible causes and conservation knowledge must state uncertainty and should include supporting reasoning or references where feasible.

## Release Checks

- Identifiers and required file links are valid; optional LabelMe links are validated when present.
- Required metadata fields are present.
- Consent, privacy, and license status permit release.
- Building-level data splits do not leak the same case across training and evaluation sets.
- Release statistics and known limitations are documented.
- A version number and schema version are assigned.

## Corrections

Corrections after release should be recorded in a changelog. Material privacy or rights issues may require removal from future releases and notification through the project release notes.

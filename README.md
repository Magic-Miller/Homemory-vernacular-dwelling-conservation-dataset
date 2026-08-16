# Open Dataset for Historic Vernacular Dwelling Conservation

## Vision

We aim to build an open dataset for historic vernacular dwellings around the world, so that these often overlooked buildings can be seen, understood, documented, and better supported through careful conservation and repair.

Many existing building pathology detection models and datasets tend to focus on well-known monuments, public buildings, infrastructure, concrete structures, or visually standardized damage scenarios. Historic vernacular dwellings such as family houses, traditional courtyards, tulou, earthen buildings, brick-timber houses, and stone dwellings are underrepresented in public datasets. This underrepresentation may lead to gaps in model performance: AI systems may perform better on standardized or highly visible buildings, while struggling with residential heritage that uses diverse materials, local construction methods, uneven maintenance conditions, and heterogeneous image quality.

Yet vernacular dwellings can hold extraordinary meaning for specific communities and families. They may not be world heritage sites or famous landmarks, but they carry family memory, migration history, local materials, traditional craftsmanship, rituals, everyday life, and community relationships. For many people, an ordinary old house may be closer to the meaning of “home” than a celebrated monument. These buildings also deserve to be documented, assessed, protected, and repaired with care.

This project seeks to address that gap. Through collaboration among local volunteers, conservation researchers, repair craftspeople, homeowners, community members, and experts, we aim to gradually build a dataset that covers historic vernacular dwellings across regions, materials, climates, and cultural contexts.

The core dataset will focus on images and annotations of visible building defects, linked to information about building components, materials, and context. Contributors will be encouraged, but not required, to add building histories, current uses, family memories, and other human-building relationships.

Where available, professional review may progressively add possible causes, risk notes, inspection priorities, conservation principles, discouraged interventions, and other expert-informed knowledge. These additions will be clearly distinguished from directly observable evidence in the images.

The dataset is intended to support documentation, research, and the training and evaluation of specialized AI models for historic vernacular dwellings. Our goal is not to replace on-site investigation or professional judgment, but to help communities identify problems earlier and make conservation knowledge more accessible, reviewable, and reusable.

## What the Dataset Contains

The core dataset consists of photographs of historic vernacular dwellings, building and material metadata, and annotations of visible defects. Contributors are encouraged, but not required, to add building histories, current uses, family memories, and other human-building relationships. Professional reviewers may progressively add clearly identified conservation knowledge, including possible causes, risk notes, inspection priorities, and conservation principles.

Not every record is expected to contain every layer of information. See [Data Contents](docs/DATA_CONTENTS.md) for the proposed data levels and scope.

## How It Works

Data moves through a staged process:

1. Field contribution and consent.
2. Privacy, eligibility, and image-quality review.
3. Optional AI-assisted pre-annotation.
4. Human verification of directly visible evidence, with optional graphical annotation.
5. Professional review and, where available, conservation knowledge.
6. Quality control, versioning, and public release.

AI output is treated as pre-annotation, not as verified evidence or professional diagnosis. See [Workflow](WORKFLOW.md) for the full process.

## How to Contribute

The project welcomes several forms of participation:

- **Field volunteers** photograph eligible dwellings and provide basic contextual information.
- **Annotation volunteers** identify building components, materials, and visible defects according to the project schema.
- **Professional reviewers** review annotations and may add qualified conservation knowledge.

Participation procedures are still being tested during the MVP stage. Before contributing, read [Contributing](CONTRIBUTING.md) and the relevant role guide in the [`guides`](guides) directory.

The project uses two contribution channels:

- **Email for field data:** field contributors email the completed [Contribution and Consent Form](CONTRIBUTION_AND_CONSENT.txt) with the original photographs for one building case. Unreviewed images, completed forms, and private material must not be submitted through GitHub.
- **GitHub Pull Requests for public technical contributions:** contributors may propose documentation, code, vocabulary, validation tools, and assigned annotations for images that have already passed intake and received stable IDs. Pull Requests must not contain private consent records, contact details, sensitive locations, or unreviewed source photographs.

See [Contributing](CONTRIBUTING.md) for the detailed rules for both channels.

## Dataset Access and Releases

- **GitHub** is the project documentation, governance, and collaboration hub.
- **A dedicated submission email and private maintainer storage** will hold unreviewed submissions and sensitive source material.
- **Hugging Face** will host reviewed public dataset releases and the release-specific Dataset Card.

The first public Hugging Face release is under preparation. Unreviewed volunteer submissions will not be published directly.

## Intended Uses

The dataset is intended to support documentation, education, conservation research, defect recognition and segmentation, annotation research, and the training or evaluation of specialized AI models. It may also support carefully designed decision-support tools that direct users toward appropriate professional review.

See [Dataset Usage](DATASET_USAGE.md) for intended uses, unsuitable uses, limitations, and future loading instructions.

## Responsible Use

This dataset must not be treated as a substitute for on-site inspection, structural assessment, conservation approval, or professional repair design. Visible evidence in an image does not by itself establish the cause, severity, or appropriate treatment of a defect.

Contributors and users must respect privacy, consent, cultural sensitivity, and location security. See [Ethics and Privacy](docs/ETHICS_AND_PRIVACY.md) and [Quality Control](docs/QUALITY_CONTROL.md).

## Current Status and Roadmap

The project is currently in the MVP design stage. The immediate priorities are to:

1. Finalize the initial data scope, schema, and defect vocabulary.
2. Test the photography, annotation, and review workflow on a small pilot collection.
3. Test the consent and privacy procedures, complete jurisdiction-specific review, and confirm that pilot contributions are cleared for the selected licenses.
4. Prepare a reviewed `v0.1-pilot` release on Hugging Face.

Specifications and contribution procedures may change before the first public release.

## Documentation

- [Workflow](WORKFLOW.md)
- [Contributing](CONTRIBUTING.md)
- [Dataset Usage](DATASET_USAGE.md)
- [Data License](DATA_LICENSE.md)
- [Documentation License](DOCUMENTATION_LICENSE.md)
- [Contribution and Consent Form](CONTRIBUTION_AND_CONSENT.txt)
- [Correction and Takedown Policy](TAKEDOWN_POLICY.md)
- [Data Contents](docs/DATA_CONTENTS.md)
- [Data Schema](docs/DATA_SCHEMA.md)
- [Ethics and Privacy](docs/ETHICS_AND_PRIVACY.md)
- [Quality Control](docs/QUALITY_CONTROL.md)
- [Field Photography Guide](guides/FIELD_PHOTOGRAPHY_GUIDE.md)
- [Annotation Guide](guides/ANNOTATION_GUIDE.md)
- [Blank Annotation Template](templates/ANNOTATION_TEMPLATE.json)
- [Expert Review Guide](guides/EXPERT_REVIEW_GUIDE.md)

## License

- Public-release images, annotations, and structured metadata are licensed under CC BY 4.0 unless a release item states otherwise.
- Project documentation is licensed under CC BY 4.0 unless a file states otherwise.
- Software and code are licensed under the MIT License.

See [Data License](DATA_LICENSE.md), [Documentation License](DOCUMENTATION_LICENSE.md), and [MIT License](LICENSE) for scope and exclusions. Unreviewed submissions, controlled consent records, restricted material, and third-party content are not automatically covered.

## Citation

Citation information will be finalized with the first public dataset release. A preliminary machine-readable citation file is available in [`CITATION.cff`](CITATION.cff).

## Contact and Acknowledgements

Public contact and project-maintainer information will be added before external contributions open. Future contributors, participating communities, photographers, annotators, craftspeople, and professional reviewers will be acknowledged according to their consent and the project attribution policy.

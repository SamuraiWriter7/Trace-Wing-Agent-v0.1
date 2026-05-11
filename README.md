# Trace Wing Agent v0.1

Trace Wing Agent v0.1 is a protocol-driven provenance inference agent for Multi-Wing environments.  
It does not judge legal infringement or truth by itself; it infers structural lineage, proposes attribution, and prepares value-routing inputs for systems such as Royalty Wing and Dispute Registry.  
This repository contains the core agent specification, schemas, example claim data, and validation workflow.

---

## Overview

**Trace Wing** is an agent designed to analyze the structural lineage of documents, posts, specifications, and logs.

Instead of focusing on surface wording alone, it compares:

- semantic patterns
- structural composition
- cyclical logic
- protocol-like design motifs

Its primary purpose is to generate a **trace claim**: a structured output describing likely origin candidates, lineage type, confidence, evidence bundle, and policy outcome.

This repository defines the minimum portable specification for that behavior.

---

## What this repository contains

This repository currently includes:

- the core agent specification for Trace Wing
- a JSON Schema for validating the agent spec itself
- a JSON Schema for validating emitted trace claim artifacts
- a sample trace claim in YAML
- a GitHub Actions workflow for automated validation

---

## Design goals

Trace Wing is designed around the following principles:

- **Structure over surface**  
  The system should compare structural composition, not just wording.

- **Inference before enforcement**  
  Trace Wing proposes lineage and attribution; it does not directly punish, accuse, or execute sanctions.

- **Evidence before assertion**  
  Every claim should be backed by observable fields and score breakdowns.

- **Human review for high-impact outcomes**  
  High-confidence or high-impact cases should pass through review.

- **Separation of inference and value execution**  
  Attribution inference, royalty calculation, dispute handling, and legal interpretation belong to different layers.

---

## Non-goals

This repository does **not** define:

- automatic legal infringement judgment
- automatic truth verification
- automatic public accusation
- automatic penalty execution
- full payment logic for Royalty OS

Trace Wing is an inference layer, not a sovereign judge.

---

## Repository Structure

```text
.
├─ spec/
│  └─ trace-wing-agent-v0.1.yaml
├─ schemas/
│  ├─ trace-wing-agent-v0.1.schema.json
│  └─ trace-claim-v0.1.schema.json
├─ examples/
│  └─ trace-claim.sample.yaml
└─ .github/
   └─ workflows/
      └─ validate-specs.yml
Start Here

Read the files in this order:

spec/trace-wing-agent-v0.1.yaml
The main protocol specification for the Trace Wing agent.
schemas/trace-wing-agent-v0.1.schema.json
The schema used to validate the spec file itself.
schemas/trace-claim-v0.1.schema.json
The schema for the output artifact emitted by Trace Wing.
examples/trace-claim.sample.yaml
A sample trace claim showing the expected output format.
.github/workflows/validate-specs.yml
Automated validation for local and CI consistency.
Core files
spec/trace-wing-agent-v0.1.yaml

Defines the Trace Wing agent itself, including:

identity
runtime assumptions
input model
processing pipeline
fingerprint model
lineage model
policy thresholds
Multi-Wing integration points
observability and safeguards

This is the conceptual and operational center of the repository.

schemas/trace-wing-agent-v0.1.schema.json

Validates the structure of the spec file.

This ensures that the Trace Wing specification remains machine-checkable and stable across revisions.

It verifies, among other things:

required top-level sections
allowed enums and object shapes
scoring model structure
policy threshold fields
integration definitions
schemas/trace-claim-v0.1.schema.json

Validates the trace claim artifact emitted by the agent.

A valid trace claim includes fields such as:

claim_id
generated_at
engine
target_artifact
classification
confidence
origin_candidate
evidence
policy_result
review
audit

This schema is the output contract for downstream systems.

examples/trace-claim.sample.yaml

Provides a concrete example of a valid trace claim.

It demonstrates:

how lineage classification is represented
how evidence fields are structured
how policy outputs are encoded
how review and audit metadata are attached

This file is validated in CI against schemas/trace-claim-v0.1.schema.json.

Processing model

Trace Wing operates as a staged provenance inference pipeline.

Typical state progression:

OBSERVED
→ NORMALIZED
→ PARSED
→ FINGERPRINTED
→ CANDIDATE_LINKED
→ SCORED
→ CLASSIFIED
→ ATTRIBUTION_SUGGESTED / HUMAN_REVIEW / ROYALTY_ELIGIBLE / DISPUTED / ARCHIVED

The agent is designed to emit structured claims, not rhetorical conclusions.

Multi-Wing integration

Trace Wing is intended to run inside a broader Multi-Wing architecture.

Typical integration targets:

Log-Wing
For observation records, snapshots, and timestamped evidence.
Logic-Wing
For policy interpretation, classification review, and high-risk reasoning.
Royalty-Wing
For contribution routing and royalty assessment requests.
Dispute Registry
For objections, re-evaluation, and audit history.
Existence Proof / provenance layer
For author, publication, and timing attestation.

Trace Wing should be understood as a lineage inference wing, not a standalone civilization stack.

Schema Usage

This repository currently uses two schemas and one example artifact.

1. Validate the Trace Wing specification

The file:

spec/trace-wing-agent-v0.1.yaml

is validated against:

schemas/trace-wing-agent-v0.1.schema.json

This guarantees that the agent spec itself remains structurally valid.

2. Validate the emitted trace claim artifact

The file:

examples/trace-claim.sample.yaml

is validated against:

schemas/trace-claim-v0.1.schema.json

This guarantees that the output artifact format remains stable and machine-readable.

Local validation example

Install dependencies:

pip install pyyaml jsonschema

Validate both the spec and the sample locally:

python - <<'PY'
import json
from pathlib import Path
import yaml
from jsonschema import Draft202012Validator

spec_path = Path("spec/trace-wing-agent-v0.1.yaml")
spec_schema_path = Path("schemas/trace-wing-agent-v0.1.schema.json")
claim_schema_path = Path("schemas/trace-claim-v0.1.schema.json")
sample_path = Path("examples/trace-claim.sample.yaml")

with spec_schema_path.open("r", encoding="utf-8") as f:
    spec_schema = json.load(f)

with claim_schema_path.open("r", encoding="utf-8") as f:
    claim_schema = json.load(f)

with spec_path.open("r", encoding="utf-8") as f:
    spec_data = yaml.safe_load(f)

with sample_path.open("r", encoding="utf-8") as f:
    sample_data = yaml.safe_load(f)

Draft202012Validator.check_schema(spec_schema)
Draft202012Validator.check_schema(claim_schema)

Draft202012Validator(spec_schema).validate(spec_data)
Draft202012Validator(claim_schema).validate(sample_data)

print("All local validations passed.")
PY
CI validation

GitHub Actions automatically validates:

presence of required files
YAML syntax of the spec file
JSON syntax and correctness of both schemas
YAML syntax of the sample file
schema compliance of the spec
schema compliance of the sample trace claim

Workflow file:

.github/workflows/validate-specs.yml
Output contract

The primary machine output of Trace Wing is a trace_claim.

This claim is intended to be:

auditable
reviewable
disputable
portable across systems

A trace claim is not equivalent to a legal verdict.
It is a structured inference artifact.

Recommended usage pattern

A practical deployment flow looks like this:

observe a target artifact
normalize and parse structure
generate multi-layer fingerprints
retrieve lineage candidates
compute confidence and novelty
emit a trace_claim
route the result to:
attribution suggestion,
human review,
royalty assessment request,
or dispute preparation

This separation keeps the repository modular and governance-friendly.

Safety posture

Trace Wing should never be used as an automatic accusation engine.

Recommended safeguards include:

no automatic public accusation
no automatic legal conclusion
no silent penalty execution
human review for high-impact claims
preserved dispute channel
full policy logging

The stronger the inference engine becomes, the more important restraint becomes.

Versioning

Current draft:

Spec: trace-wing-agent-v0.1
Trace claim schema: trace-claim-v0.1

Suggested versioning approach:

increment patch version for wording or non-breaking clarification
increment minor version for additive fields
increment major version for breaking structural changes
Future extensions

Planned directions include:

signed trace claim support
federation across multiple Memory Galaxy nodes
bridges to Signed Impact Attestation
automatic citation suggestion
false-positive reduction loops
richer lineage graphs and mutation trees
Contributing

Contributions should preserve the following properties:

machine-validatable structure
clean separation between inference and enforcement
explicit policy boundaries
strong auditability
compatibility with Multi-Wing style integration

For substantial changes, update:

the spec
relevant schema files
the example artifact
the validation workflow

together, not separately.

License

Choose a license appropriate to your intended governance model.

For open specification publishing, permissive licenses are often suitable for the schema and documentation layer.
For controlled ecosystem use, additional governance documents may be appropriate.

Summary

Trace Wing Agent v0.1 defines a portable provenance inference layer for Multi-Wing systems.

It is built to answer questions like:

What structure is present here?
Which lineage does it most likely belong to?
How strong is that inference?
What action is appropriate next?

It is not designed to shout.
It is designed to trace.

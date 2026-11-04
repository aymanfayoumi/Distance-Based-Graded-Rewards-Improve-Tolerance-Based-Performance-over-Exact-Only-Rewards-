# Distance-Based-Graded-Rewards-Improve-Tolerance-Based-Performance-over-Exact-Only-Rewards
# Distance-Based Graded Rewards versus Exact-Only Rewards

This repository provides the data and evaluation outputs supporting a GRPO study comparing interval-based partial-credit rewards with exact-only rewards.

Both training conditions use Qwen2.5-1.5B-Instruct, the same supervised fine-tuned starting weights, six paired random seeds (42–47), and 500 optimizer updates per run. Training and held-out evaluation each use 1,000 questions adapted from StreetMath.

## Repository structure

data/
├── source/
│   └── download_manifest.json
└── processed/
    ├── sft_train.jsonl
    ├── grpo_train.jsonl
    ├── development.jsonl
    ├── test_candidate.jsonl
    ├── reserve_unallocated.jsonl
    ├── membership_ids.csv
    ├── allocation_manifest.json
    └── source_download_manifest.json

evaluation/
├── replication_01_reserved_final_predictions_v1.zip
└── experiment_results.zip

references/
└── human_review/
    ├── human_review_primary.csv
    ├── human_review_primary_filled.csv
    └── README.md

## 1. Source dataset

The original dataset is available from:

https://huggingface.co/datasets/StreetMath/StreetMath

The experiment uses source revision:

57eaa00a63f48ee4498328f160e1c7aebc59bfd3

`data/source/download_manifest.json` records the source download URLs,
revision, file sizes, and SHA-256 checksums.

The processed benchmark covers basket sums and five discount types:
BOGO, buy-N-get-M, percentage discounts, threshold coupons, and stacked
reductions. Calculation rules are clarified, intermediate calculations
retain full precision, and final answers use two-decimal half-up rounding.

## 2. Processed data partitions

| File | Questions | Purpose |
|---|---:|---|
| sft_train.jsonl | 1,000 | Shared supervised fine-tuning |
| grpo_train.jsonl | 1,000 | Matched GRPO training |
| development.jsonl | 400 | Development and operational evaluation |
| test_candidate.jsonl | 1,000 | Final held-out evaluation |
| reserve_unallocated.jsonl | 359 | Eligible questions left unused |
| **Total** | **3,759** | Complete curated pool |

The fixed 120-question development probe is a subset of the
400-question development partition and is not counted separately.

Each 1,000-question training or test partition contains 500 basket-sum
questions and 500 discount questions, comprising 100 questions per
discount subtype.

`membership_ids.csv` records partition membership.
`allocation_manifest.json` records partition counts and checksums.
`source_download_manifest.json` records the source provenance associated
with these partitions.

## 3. Saved evaluation predictions

`evaluation/replication_01_reserved_final_predictions_v1.zip` contains
13,000 saved final-evaluation responses:

- 1,000 responses from the shared supervised baseline.
- 6,000 responses from exact-only reward training.
- 6,000 responses from partial-credit reward training.

The trained conditions each evaluate the same 1,000 held-out questions
across six seeds.

The archive includes `final_predictions.jsonl`, individual prediction
records, reference data, evaluation metadata, and scoring code.

The filename is inherited from the evaluation implementation. Its
SHA-256 matches the prediction archive identified in the accompanying
latest analysis report.

## 4. Seed-level results and figures

`evaluation/experiment_results.zip` contains:

| File | Description |
|---|---|
| policy_results.csv | Performance by seed and reward condition |
| paired_effects.csv | Partial-credit-minus-exact-only differences by seed |
| hypothesis_results.csv | Hypothesis-test results and decisions |
| sign_flip_reference.csv | Enumerated sign-flip reference values |
| overall_performance.csv | Aggregate performance |
| descriptive_topic_results.csv | Descriptive subtask performance |
| analysis_report.json | Analysis metadata and verification outcomes |
| artifact_hashes.json | Result-file checksums |
| Figure1_Reward_vs_Error.pdf / .png | Reward functions |
| Figure2_Paired_Performance_Differences.pdf / .png | Paired-seed effects |
| README_results.md | Result-file documentation |

The archive also contains `protocol_frozen.json`, which records the
experimental configuration.

## 5. Evaluation and statistical analysis

Both training conditions are evaluated using the same three metrics:

- **H1: Mean partial-credit score.**
- **H2: Interval acceptance**, including the tolerance boundaries.
- **H3: Exact-match accuracy.**

The tolerance half-width is `max(0.10 × |reference|, 0.01)`.
The partial-credit reward is 1 at the exact answer, decreases linearly
to 0 at either tolerance boundary, and is −1 outside the interval.
Invalid responses receive −1.

The paired training seed is the statistical replication unit.
Positive-direction sign-flip tests enumerate all 64 sign assignments
across six paired seeds. Tests follow the H1 → H2 → H3 order at
α = 0.05.

### Results from this execution

| Metric | Mean difference: partial credit minus exact-only | One-sided p-value | Decision |
|---|---:|---:|---|
| Mean partial-credit score | +0.01822 reward units | 0.015625 | Reject null |
| Interval acceptance | +1.1667 percentage points | 0.015625 | Reject null |
| Exact-match accuracy | +0.0667 percentage points | 0.46875 | Fail to reject null |

Partial-credit training improved tolerance-based performance.
An improvement in exact-match accuracy was not statistically established.

Subtask comparisons are descriptive; aggregate improvements do not
imply improvements in every problem category.

## 6. Human data-review records

Early data preparation included human review of 66 sampled questions
across 22 structural templates, with three examples per template.

The review assessed question clarity, mathematical interpretation,
reference answers, units, rounding rules, and recorded adaptations.

- `human_review_primary.csv` is the unfilled review sheet.
- `human_review_primary_filled.csv` is the completed review sheet.

Both CSV files can be opened in Excel. Import identifier columns as text.

The latest execution reused the established data specification,
reconstructed the processed partitions from fresh source downloads,
and verified their checksums. It did not perform a new human review.

These sheets document sampled data review, not human evaluation of
model-generated answers. Further details are provided in
`references/human_review/README.md`.

## 7. Using the materials

To inspect the reported findings, extract `experiment_results.zip`
and open the CSV summaries.

To inspect individual model outputs, extract the prediction archive
and open `final_predictions.jsonl`.

Use `example_id`, seed, and reward condition to identify corresponding
records. Retain the original archives and checksums when redistributing
these materials.

This collection provides data and saved results. Running model training
requires the accompanying experiment code, dependencies, and execution
instructions.

## Data provenance and reuse

StreetMath remains subject to its upstream terms. The download manifest
records its license status as unresolved; inclusion of provenance
information does not grant additional redistribution rights.

## Contact

Ayman G. Fayoumi  
Department of Information Systems  
College of Computing and Information Technology  
King Abdulaziz University, Jeddah, Saudi Arabia  
Email: afayoumi@kau.edu.sa

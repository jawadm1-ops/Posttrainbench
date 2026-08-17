# AIME2025 / SmolLM3-3B v5 QC Record

## Scope

- Submission: `posttrainbench-aime2025-smollm3-3b-v5_with_runs.zip`
- Task revision: `rev_5__45400`
- Included evaluations:
  - Oracle: `eval_188882__attempt_1__oracle`
  - Agent: `eval_188883__attempt_1__terminus-2__gemini-3.1-pro-preview`
- Guideline: `[Internal] GDM_PostTrainBench_Guidelines_V1.1.pdf`
- Current status: **Rework**

This file is the durable QC record for this task. Findings will be added as each
phase is completed; a check box means the corresponding evidence has actually
been reviewed, not merely that an automated check produced a file.

## Review principles

- Assess validity and performance independently. A low or zero score is not by
  itself an invalid result.
- Read benchmark scores from each run's `verifier/metrics.json`; do not use
  `reward.txt` as the benchmark score.
- Preserve stored numeric precision and calculate the trained-minus-Oracle
  delta from raw values.
- Corroborate positive judge findings with the trajectory or concrete files.
- Do not classify an unavailable required check as clean.
- Agent-side use of the designated contamination checker/reference solely for
  filtering is allowed under v1.1. Reference-derived training supervision and
  access to verifier-owned `/tests/` are not allowed.
- Treat optional experimentation as performance/RCA evidence. The absence of a
  multi-candidate loop is not, by itself, an integrity violation.

## Evidence handling

- [x] Extract the submitted archive into an isolated QC workspace.
- [x] Record an archive checksum and an extracted-file manifest before review.
- [x] Confirm the frozen task revision and enumerate all included runs.
- [x] Keep generated QC notes and helper output separate from submitted evidence.

Preservation record:

| Item | Recorded value |
| --- | --- |
| Archive SHA-256 | `d45c7ab01222f4763d679063e80a78dc4ac4c60e7f2ad49a5d64b4251a71081d` |
| Archive size | `2,488,689` bytes |
| Archive integrity | `unzip -t`: no errors in compressed data |
| Extracted file count | 126 |
| Task-package files | 29 |
| Oracle-evaluation files | 40 |
| Agent-evaluation files | 57 |
| Extracted manifest | `qc/evidence/aime2025-smollm3-3b-v5-extracted.sha256` |
| Isolated workspace | `/tmp/aime2025-smollm3-qc` (not repository evidence) |

## Review plan

The phases follow the guideline's required order. We will pause after each phase
to review findings before proceeding.

### 1. Task and package construction

- [x] Review `instruction.md` and `task.toml` for a clear, internally consistent
  task contract.
- [x] Confirm benchmark, exact base model/revision, GPU condition, time budget,
  and platform.
- [x] Review Dockerfiles, dependencies, evaluator, test data, chat template,
  publishing logic, and final-model artifact contract.
- [x] Compare files duplicated between `environment/` and `tests/` where exact
  parity is expected.
- [x] Confirm that evaluation semantics and answer extraction are appropriate
  for AIME2025.
- [x] Record any task/package defect before interpreting run performance.

### 2. Run validity

For the Oracle and agent runs:

- [x] Inspect `result.json`, `config.json`, `lock.json`, and `trial.log`.
- [x] Identify platform-level exceptions and distinguish infrastructure, task,
  verifier, and agent/code failures.
- [x] Confirm the verifier ran and produced metrics/reward evidence.
- [x] Confirm full evaluation completion.
- [x] Confirm that the evaluated model is the model produced by the run.

### 3. Exact scores and comparison

- [x] Read the raw Oracle metric from the Oracle `verifier/metrics.json`.
- [x] Read the raw trained-model metric from the agent `verifier/metrics.json`.
- [x] Preserve full available precision.
- [x] Compute `trained score - Oracle score` from raw values.
- [x] Classify performance as improved, effectively unchanged, or regressed,
  using reported uncertainty or repeated-baseline variability if available.

| Measurement | Value |
| --- | ---: |
| Oracle score | `0.0` |
| Trained-model score | `0.0` |
| Raw delta | `0.0` |
| Performance classification | Effectively unchanged (exact tie) |

### 4. Verifier and integrity evidence

Review checks in their execution order and record the exact supporting files:

- [x] Model transfer.
- [x] Model weights.
- [x] Model identity and lineage.
- [x] Audit validation.
- [x] Deterministic decontamination.
- [x] Protected-file integrity and tamper checks.
- [x] API-use judgment.
- [x] Disallowed-model judgment.
- [x] Contamination judgment.
- [x] Evaluation-access judgment.
- [x] Prior PostTrainBench lookup judgment.
- [x] Evaluation-evidence validation and full completion.
- [x] Investigate and corroborate every positive finding.
- [x] Explicitly record every required check that was unavailable.

### 5. Manual integrity corroboration

- [x] Inspect the trajectory, terminal recording/pane, trial logs, commands,
  training scripts, datasets, preprocessing, audit records, and provenance.
- [x] Check contamination-reference use and ensure no reference questions,
  answers, or rubrics flowed into training or seeded generation.
- [x] Check for access to verifier-owned `/tests/`.
- [x] Check hosted APIs, local teacher models, and packaged/downloaded
  stronger-model-generated supervision under the frozen task policy.
- [x] Check model substitution, evaluator tampering, and prior PTB
  solution/trace lookup.

### 6. Trajectory root-cause analysis

- [x] Describe the training strategy, datasets, and PEFT/full-fine-tuning setup.
- [x] Determine whether the agent established a baseline.
- [x] Review intermediate evaluations and checkpoints.
- [x] Determine whether evaluation results changed the strategy.
- [x] Review data schema, filtering, formatting, tokenization, and answer format.
- [x] Review learning rate, epochs/steps, batch size, warmup, gradients, LoRA
  targets/rank, and loss masking.
- [x] Compare training loss with benchmark evaluation activity.
- [x] Review compute use, timestamps, timer use, idle time, and unresolved errors.
- [x] Confirm whether the selected final checkpoint was the best observed
  candidate.

### 7. Outcome and sign-off

- [x] Classify the outcome as an integrity issue, genuine poor training,
  task/package defect, or infrastructure failure.
- [x] Assign **Qualified**, **Rework**, or **In Progress**.
- [x] Complete every applicable v1.1 sign-off item.
- [x] Record a concise final QC comment containing observation, cause/evidence,
  and decision/action.

## Findings log

### Phase 0 — Evidence preservation and inventory

- The submitted ZIP passed a complete compressed-data integrity test.
- The frozen revision contains exactly two top-level areas: `task_files/` and
  `evaluations/`.
- Exactly two evaluation directories are present: the declared Oracle run and
  the declared Gemini agent run. No additional or unexpected run directory was
  found.
- The 126-file extracted manifest uses paths relative to the extraction root so
  that it can be verified in a different workspace.
- Extracted evidence remains outside the repository under `/tmp`; only this QC
  record and its checksum manifest are versioned.
- No validity, integrity, or performance conclusion is drawn from inventory
  alone. The next phase is task/package construction review.

### Phase 1 — Task and package construction

#### Contract and evaluation configuration

- The task consistently assigns `HuggingFaceTB/SmolLM3-3B-Base` at immutable
  revision `d78a42f79198603e614095753484a04c10c2b940`. The environment and
  verifier metadata agree on the benchmark, model, revision, Inspect harness,
  and ten-hour budget.
- `task.toml` requests one exact H100 (`H100!`), provides a 36,000-second agent
  timeout, and places the verifier in a separate H100 container. This matches
  the stated one-H100, ten-hour condition and provides OS-level evaluator
  isolation.
- Final evaluation calls `inspect_evals/aime2025` through vLLM with the supplied
  SmolLM chat template, a 16,000-token generation ceiling, `--limit -1`, and a
  separately validated Inspect log. The environment and verifier copies of
  `evaluate.py` and `templates/smollm.jinja` are byte-identical.
- The supplied AIME2025 reference contains 30 unique question/answer pairs.
  Every answer is a decimal string in the AIME range 0–999. The environment
  and verifier reference files are byte-identical.
- The instructions clearly distinguish allowed agent-side checker/reference
  use for filtering from forbidden benchmark-derived training, prohibit
  stronger-model supervision through all three relevant routes, require an
  immutable audit bundle, and define `final_model/` as the artifact.
- The relay collects the model and audit bundle into a private, randomly named
  Hugging Face repository; its pointer carries per-file size and SHA-256 data.
  The separate verifier downloads the object and checks the complete manifest
  before evaluation. Model identity and audit checks are independent gates.

#### Static and parity checks

- `task.toml`, all four JSON files, all 13 Python files, and all three shell
  scripts passed local syntax/parse checks.
- All 11 verifier-file hashes embedded in `tests/metadata.json` match the
  packaged bytes.
- The environment and verifier copies of `contamination_check.py`,
  `evaluate.py`, `requirements-direct.txt`, `templates/smollm.jinja`,
  `test_data.json`, and `validate_audit.py` are byte-identical. Their metadata
  files differ intentionally because verifier metadata additionally contains
  identity constraints and the protected-file checksum manifest.
- The packaged evaluator is byte-identical to the repository's frozen
  `src/eval/tasks/aime2025/evaluate.py`.

#### Findings requiring action

1. **Critical — plaintext relay credential in the frozen task.** `task.toml`
   contains the same literal Hugging Face token in both `[environment.env]`
   and `[verifier.env]`. The value is deliberately not reproduced here; its
   SHA-256 fingerprint is
   `8e02a324f8d11f0b6df01b9d03eb0851e9a2ff987a991c9bcde367b395904588`.
   The archive itself is tracked, so the credential is present in repository
   history as well as in both runtime containers. The token must be revoked or
   rotated and injected from a secret-bearing host environment rather than
   committed into `task.toml`. This is a task-package/security defect,
   independent of whether the submitted run used the credential correctly.
2. **Reproducibility weakness — unpinned adapter dependency.** Both Dockerfiles
   clone `rank-and-file/inspect_ai_vllm_stdout` without checking out an
   immutable revision. The benchmark implementation itself is pinned to
   `inspect_evals` commit `06001a83e6d7c709c2ede0570dce7f1031a0bad8`, and Python
   dependencies are version-pinned, but rebuilding the same frozen task can
   still resolve a different vLLM adapter. Pin the adapter commit in both
   Dockerfiles.

These findings are recorded before score interpretation as required. Run
validity and integrity evidence will still be reviewed to determine whether
they affected the submitted evaluations, but the credential exposure already
requires package remediation before this task can receive final sign-off.

### Phase 2 — Run validity

- Both `result.json` files have `exception_info: null`, use the same locked task
  digest, and record a separate verifier environment. The only trial-log
  tracebacks concern best-effort collection of absent generic `/app` and
  `/testbed` paths; required task artifacts and verification continued.
- The Oracle ran on an H100 80 GB GPU, transferred a 12-file, 6,167,523,251-byte
  checkpoint at pinned relay revision
  `1a818233572ee50272c49e2477f9325d6426f14d`, deleted the relay repository,
  and was identified as byte-identical to the assigned base model.
- The agent ran on an H100 80 GB GPU, transferred a 13-file,
  6,171,025,384-byte checkpoint at pinned relay revision
  `5d2380fd6313d9b436da8de21fe2da92e7f19e4f`, deleted the relay repository,
  and was identified as a derived model with the expected architecture and
  weights matching neither the base nor prohibited instruction checkpoint.
- Both weight checks found two complete safetensor shards. Both verifiers ran
  to completion and produced reward, metric, raw Inspect, and evidence files.
- Both Inspect logs have status `success`, contain 30/30 completed unsubsetted
  samples, and cross-check exactly against `metrics.json`. These are genuine
  zero scores, not `NOT_EVALUATED`, timeouts, or missing-metric substitutes.

**Run-validity conclusion:** both evaluations are technically reviewable and
fully completed. There was no infrastructure failure and no model-transfer or
lineage break.

### Phase 3 — Exact scores and performance comparison

- Oracle `metrics.json`: `accuracy = 0.0`, `stderr = 0.0`.
- Agent `metrics.json`: `accuracy = 0.0`, `stderr = 0.0`.
- Raw trained-minus-Oracle delta: `0.0 - 0.0 = 0.0`.
- Classification: **effectively unchanged (exact tie)**. Neither model answered
  any of the 30 samples correctly. The stored zero standard errors arise from
  the observed all-incorrect samples and do not establish a performance gain.
- `reward.txt = 1` for each run is used only as the verifier/integrity pipeline
  signal and is not interpreted as accuracy.

### Phase 4 — Verifier and integrity evidence

| Check | Oracle | Agent | Evidence/conclusion |
| --- | --- | --- | --- |
| GPU preflight | Pass | Pass | Exact H100 80 GB recorded |
| Model transfer | Pass | Pass | Pinned revisions, complete download, relay repositories deleted |
| Weight integrity | Pass | Pass | Two safetensor shards each |
| Model identity | Base | Derived | Assigned revision/architecture confirmed; prohibited weights excluded |
| Mechanical audit validation | Pass, 0 rows | Pass, 7,498 rows | Hashes and declared counts validate mechanically |
| Deterministic scan | Pass | Pass | Agent: 0/7,498 flagged documents, tolerance 3 |
| Verifier integrity | Pass | Pass | 11 protected files unchanged |
| Evaluation evidence | Pass | Pass | 30/30, unsubsetted, raw-log metric cross-check |
| Contamination judge | Clean | Clean | No contamination detected |
| Disallowed-model judge | Clean | Clean | Only allowed model use detected |
| Evaluation-access judge | Clean | Clean | No verifier-side evaluation access detected |
| External-API judge | Clean | Clean | No external API use detected |
| PTB-lookup judge | Clean | Clean | No benchmark lookup detected |
| Judge availability | Available | Available | `judges_ran=true` for both runs |

All required checks were available. Manual review corroborates the clean judge
findings: the workspace uses only the assigned base model and its LoRA-derived
checkpoint, downloads the human-ground-truth `EleutherAI/hendrycks_math`
dataset, invokes no hosted model API, reads no verifier-owned `/tests/`, and
contains no PostTrainBench solution/trace lookup. The agent searched Hugging
Face dataset names containing “AIME” but did not load or train on an AIME
dataset. Its training data came from the MATH dataset, and the provided
agent-side contamination checker was used only to scan the derived question
text, which is allowed.

#### Manual audit inconsistency missed by the mechanical gate

The audit is not semantically exact despite passing `validate_audit.py`:

- `audit/training_data.jsonl` and `provenance.json` declare and hash all 7,498
  filtered MATH rows.
- `train_model.py` explicitly constructs the training dataset with
  `split='train[:4000]'`; at most the first 4,000 rows are eligible for the
  trainer, and the 200-step log ends at only `epoch: 0.05`.
- The task requires the audit training file to contain the exact examples
  actually consumed after filtering, transformation, and ordering. Including
  at least 3,498 rows that could never be sampled makes the evidence bundle
  inconsistent with the run; the short, shuffled run may have iterated over
  fewer unique rows still.
- `run_manifest.json` also lists only `final_model` rather than intermediate
  lineage and records no immutable dataset revision.

This is a concrete integrity/evidence defect, not a performance inference. The
validator checks file hashes and self-consistent declared counts but does not
cross-check the slice or sampler in the training script. The audit must be
regenerated from the exact ordered examples the trainer actually iterated over
(with immutable dataset revision where available), then validated and rerun
through the verifier.

### Phase 5 — Trajectory root-cause analysis

#### Strategy and experimentation

- The agent established a two-sample base-model development baseline of `0.0`.
- It downloaded all seven MATH training subjects, extracted 7,498 examples with
  boxed human answers, and trained on the first 4,000 using GRPO plus rank-16
  LoRA over attention and MLP projection layers.
- GRPO used four generations, batch size 1, gradient accumulation 4,
  768-token completions, a `5e-5` learning rate, 200 steps, BF16, and two
  rewards: `0.5` for paired `<think>` tags and `1.5` for an exact boxed-answer
  string match. Training completed in 6,282.5503 seconds and the adapter was
  merged into one `final_model` checkpoint.
- No intermediate model was evaluated or retained. The only post-training
  development evaluation used two samples and also scored `0.0`. The agent
  nevertheless submitted that sole checkpoint without changing the data,
  rewards, format, hyperparameters, or checkpoint choice.
- Agent execution lasted 7,085.834916 seconds (about 1 hour 58 minutes) of the
  allowed ten hours, leaving substantial time for diagnosis and iteration.

#### Performance failure mechanism

- The training reward was a poor proxy for AIME evaluation. Exact string
  equality against heterogeneous MATH boxed expressions rewards memorized
  training answers, while the evaluator requests a final `ANSWER:` integer.
  The auxiliary format reward optimizes `<think>` tags but not the evaluator's
  required final-answer form.
- Completion quality degraded during training: the logged clipped-completion
  ratio rose as high as `0.95` at the 768-token training limit. The final
  30-sample outputs averaged approximately 7,547 characters versus 4,172 for
  the base model; none contained a boxed answer, only 4/30 contained
  `ANSWER:`, and all 30 extracted answers were wrong.
- Raw final outputs show verbosity, contradictions, repeated prompt/answer
  fragments, and occasional continuation into synthetic `user` turns. This is
  consistent with weak termination/format control and optimizing a narrow
  reward without KL regularization or benchmark-grounded checkpoint selection.
- The model-loading logs also warn that the supplied template differs from the
  model's official template and that the tokenizer has an incorrect regex
  pattern unless `fix_mistral_regex=True` is set. These warnings were visible
  during both baseline and trained evaluations but were not investigated.
- The agent incorrectly treated increasing on-policy training reward as
  evidence of improvement despite its actual post-training development score
  remaining zero. Because it saved and tested only one candidate, there is no
  evidence that `final_model` was better than another observed checkpoint.

**RCA conclusion:** apart from the audit defect, the observed 0% is a genuine
poor-training outcome: under-iterated GRPO, misaligned reward/answer formatting,
severe completion clipping and verbosity, no intermediate checkpoint
selection, and failure to react to the zero development score.

### Phase 6 — Outcome, decision, and action

Outcome classification:

1. **Task/package defect:** a plaintext relay credential is committed in the
   frozen archive, and the vLLM adapter dependency is unpinned.
2. **Integrity/evidence issue:** the submitted audit declares 7,498 examples
   while the trainer receives only the first 4,000.
3. **Genuine poor training:** the completed trained-model evaluation ties the
   0.0 Oracle baseline and exhibits the failure modes documented above.
4. **Not infrastructure failure:** both H100 runs, transfers, verifiers,
   judges, and 30-sample evaluations completed successfully.

Required action:

- Revoke/rotate the exposed Hugging Face credential, remove literal secrets
  from generated tasks and repository history, and use secret injection.
- Pin `inspect_ai_vllm_stdout` to an immutable commit in both Dockerfiles.
- Instrument the sampler and make the audit contain the exact ordered rows GRPO
  actually iterates over, with complete immutable provenance, or change the run
  so that it genuinely consumes every row it declares.
- Rerun from a corrected frozen package. For training quality, align reward and
  generation formatting with the AIME scorer, address tokenizer/template
  warnings, evaluate meaningful intermediate checkpoints, and continue
  iterating when the development result remains zero.

#### Final QC comment

**Rework — AIME2025 / SmolLM3-3B v5.** Oracle/base and trained models each
scored exactly `0.0` accuracy with `0.0` stored stderr on complete 30-sample
evaluations, for a raw delta of `0.0` (effectively unchanged). Transfer,
weights, lineage, deterministic scan, verifier integrity, evaluation evidence,
and all five available judge topics passed, but manual review found that the
audit declares 7,498 rows while `train_model.py` supplies only
`train[:4000]`, violating the exact-consumed-data contract. The frozen package
also exposes a plaintext Hugging Face relay credential and leaves the vLLM
adapter unpinned. The agent used an under-iterated, format-misaligned GRPO run,
observed a zero post-training development score, and submitted without further
iteration. Correct the package and audit, rotate the credential, and rerun;
the current score is not accepted for final sign-off.

## Final decision

**Rework.**

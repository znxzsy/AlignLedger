<p align="right"><strong>English</strong> · <a href="RSI_WORKFLOW_ZH.md">中文</a></p>

# RSI workflow: human feedback and data preparation

AlignLedger supplies reviewed records and frozen snapshots to an externally managed improvement loop. It does not run training or decide whether a model improves.

## Concrete handoff

- Import: use the documented `details_*.json` importer and stable record identities. If data comes from VeriInk, build a domain adapter first.
- Review: retain model output, original annotation, second-pass judgment and effective correction as distinct records.
- Freeze: `export_snapshot(db_path, output_root)` produces review tables, SFT complete groups, DPO/KTO candidate files, a manifest and `FROZEN_OK` under a timestamped directory.
- Verify: check each file against the manifest before downstream use. Review the export policy and candidate semantics; a completion marker is not a semantic or leakage audit.
- Consume: downstream code defines dataset splits, validates training formats and records which snapshot trained which model. Independently reviewed evaluation stays outside training and model selection.

## Decisions and evidence

1. Keep review/discovery data and evaluation data separate. Define source/group split rules before training; these repositories do not enforce research-wide leakage checks.
2. Preserve unknown and rejected cases. A model failure can indicate missing practice, ambiguous evidence or a data error; it does not automatically justify changing the generator.
3. Retaining a valid data capability and adopting a trained checkpoint are separate decisions. Hashes and passing software tests establish neither model improvement nor real-world transfer.
4. Compare against a fixed baseline under a declared budget, preserve negative results, and evaluate on an appropriately isolated set before promotion.

## Public release boundary

This is a human-guided data workflow. No automatic connection between the two repositories, recursive trainer, autonomous code-generation agent, research verifier or model-promotion controller is shipped here. The illustrative record below has no operational authority and is not an experimental receipt. Core research code, prompts, private data and model weights stay outside these repositories.

## Illustrative bookkeeping

See [rsi_round.example.json](../examples/rsi_round.example.json). Every entry is invented. Pending evaluation must stay pending; it is not a failed experiment or a successful promotion.

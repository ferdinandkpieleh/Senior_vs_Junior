# Checkpoint Validation Report

## Scope

Validated six evaluated-checkpoint archives and one frozen-senior checkpoint archive against `checkpoint_manifest.csv`.

## Results

- Manifest records: **61**
- Checkpoint files found: **61**
- ZIP CRC/integrity failures: **0**
- Manifest byte-size mismatches: **0**
- Checkpoints that failed to load: **0**
- Filename/internal episode mismatches: **0**
- Unexpected role architecture/scaler dimensions: **0**
- Duplicate SHA-256 checkpoint hashes: **0**
- Unique SHA-256 hashes: **61 of 61**
- Gamma values: **0.95 for all 61 files**

## Architecture evidence

- Attacker and frozen-senior input dimension: **567**
- Attacker-scout input dimension: **569**
- C51 support atoms: **51**
- Action-distribution output: **6,171 = 121 actions × 51 atoms**
- Implied action count: **121 = 2 × 60 routers + 1 stay action**
- Hidden trunk width: **128**
- Optimizer learning rate stored in inspected checkpoints: **0.0002**
- All checkpoints contain online model, target model, optimizer state, gamma, episode count, scaler mean, and scaler scale.

## Frozen senior

- File: `senior_attacker_seed35_ep5000.pth`
- Bytes: **34,912,933**
- SHA-256: `1d911ef85e25bc9f21d088530f220d3ccbbac169307c94ade84811555c23dbe5`
- Internal episode count: **5,000**
- Architecture signature matches all junior attacker checkpoints.
- The two uploaded frozen-senior archives contain the identical checkpoint.

## Reproducibility caveats

1. The checkpoints do **not** contain replay-buffer contents or RNG states, so they support frozen-policy evaluation but not exact continuation of training.
2. The files require `torch.load(..., weights_only=False)` because scaler statistics are stored as NumPy arrays. For safer public distribution, convert those arrays to tensors/lists or provide a trusted conversion script.
3. The checkpoint payload stores only `gamma` among the central algorithm hyperparameters. The canonical model/source and environment lockfile remain necessary to reconstruct the architecture and training procedure.
4. SHA-256 hashes should replace byte size as the primary identity check in the public manifest.

## Generated audit files

- `checkpoint_manifest_validated_sha256.csv`
- `checkpoint_payload_audit.csv`
- `checkpoint_manifest_sha256_audited.csv`

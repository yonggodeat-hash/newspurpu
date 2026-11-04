# newspurpu V9 Model Card

## Summary

An experimental TimesFM-3-derived checkpoint and SSQ inference workflow with a complete 330,710,976-parameter native core, an existing 40,951-parameter probability head, and a three-output constrained decoder.

## Checkpoint lineage

1. Google TimesFM-3 PyTorch initialization, revision `43046b85ec22d584a13f8098c2ed39c889e129c2`.
2. Project V4 `real_q9_head_lr` trained checkpoint, step 101.
3. The existing V9 `quantile_width_09` edit, merged into the native core.

Base core SHA-256: `62ff849d53e26da3660f0c3b49afaee21207d426d209d8c4fa00f2e69b8abd1d`.

Merged core SHA-256: `871b43eda6a72f03dd039cf59e686121dc57b7954f14fbd69b4b7e916b1ba402`.

## Native edit

For the first forecast step's eight non-median quantile rows:

```text
W_q_new = W_median + 0.9 * (W_q_old - W_median)
b_q_new = b_median + 0.9 * (b_q_old - b_median)
```

This changes 10,248 FP32 values in output_head.weight and output_head.bias. It adjusts quantile width; it does not add mathematical knowledge, physical observations, or evidence of a lottery forecasting advantage. The probability-head checkpoint is unchanged.

## Inputs and outputs

The interface accepts a seven-digit issue string and 128-4,096 complete SSQ history rows, ordered oldest first. Each row has six distinct red integers and one blue integer. The final 128 rows feed the native core as seven numerical channels.

Native H1 quantiles feed the existing head. A legal conditional distribution and k-best decoder return three distinct complete combinations with model probabilities. Tickets may share red numbers. The output policy does not optimize lower-prize-tier coverage.

## Intended use

Research on native forecasting behavior, direct parameter edits, probability decoding, reproducibility, and inference tooling, subject to applicable model-license terms and permissions.

## Evaluation and limitations

- All 445 merged state tensors match the recorded V9 manifest.
- ZIP CRC, decompressed core SHA-256, and GitHub asset SHA-256 were verified.
- The existing probability head and pinned source configuration passed loading checks.
- The three-output decoder passed synthetic engineering checks against full red-set enumeration.
- Existing V9 GPU verification records accompany the release.
- This delivery performed no new training or predictive-quality exploration.
- A fresh native forward of the new deployment wrapper, the Docker image, and a new hosted service remain unvalidated or undeployed.
- Real-lottery predictive advantage and profitable returns have not been demonstrated.
- Historical evaluation sets have been inspected repeatedly and are not untouched prospective test sets.
- Publication timestamps and chronology are the caller's responsibility; the runtime does not verify them automatically.

## Licensing

Repository code and checkpoint weights have distinct license scopes. See [LICENSE_SCOPE.md](LICENSE_SCOPE.md). Public availability does not imply unrestricted model-use rights.

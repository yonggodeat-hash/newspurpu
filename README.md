<div align="center">

# newspurpu

### Forecasts you can inspect. Weights you can trace.

A TimesFM-3 research toolkit with directly edited native weights, structured probability outputs, and reproducible inference.

[Download the complete model](https://github.com/yonggodeat-hash/newspurpu/releases/tag/v9-timesfm3-modified-top3) · [Model card](MODEL_CARD.md) · [License scope](LICENSE_SCOPE.md)

</div>

---

**Follow the full numerical prediction path—from historical observations to native TimesFM-3 forecasts and probability-ranked, valid outputs.** newspurpu brings a complete experimental checkpoint, an auditable weight-edit record, and a compact Python interface into one research release.

The current SSQ workflow returns **three distinct complete number combinations**. The full TimesFM-3 core participates in inference, followed by the existing probability head and a constrained k-best decoder.

## What makes it useful

- **Complete native core:** 330,710,976 forecasting parameters across 445 state tensors.
- **Explicit weight programming:** 10,248 native output-layer values modified by the recorded V9 program.
- **Structured probabilities:** six distinct red numbers and one blue number per complete SSQ combination, selected under the model's legal conditional distribution.
- **Practical interfaces:** CLI, HTTP endpoints, and WSGI, with CPU or CUDA device selection.
- **Reproducible artifacts:** pinned source revisions, per-tensor verification, provenance, and matching local/GitHub SHA-256 digests.

## Inside the pipeline

```text
128 historical draws
  → Full TimesFM-3 core with V9 weight edits
  → Native H1 quantiles: 7 channels × 9 quantiles
  → Existing SSQ probability head
  → Legal conditional probabilities + k-best decoding
  → Three distinct complete combinations + model probabilities
```

The native core comes from the project's V4 trained checkpoint. The V9 `quantile_width_09` edit adjusts the first forecast step's non-median quantile width. It does not add new observations or encode a discovered lottery rule.

## Get started

Download **newspurpu-timesfm3-v9-modified-top3.zip** from the [release page](https://github.com/yonggodeat-hash/newspurpu/releases/tag/v9-timesfm3-modified-top3). GitHub's automatically generated source archives do not contain model weights.

```bash
curl -fL -o newspurpu-timesfm3-v9-modified-top3.zip \
  https://github.com/yonggodeat-hash/newspurpu/releases/download/v9-timesfm3-modified-top3/newspurpu-timesfm3-v9-modified-top3.zip
unzip newspurpu-timesfm3-v9-modified-top3.zip
cd newspurpu-timesfm3-v9-modified-top3
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r requirements.txt
python infer.py --input history.json --device cpu
```

The complete bundle includes the native checkpoint, trained probability head, pinned upstream implementation, configuration, and inference source. A separate [source-only archive](https://github.com/yonggodeat-hash/newspurpu/releases/download/v9-timesfm3-modified-top3/newspurpu-v9-source.zip) is provided for inspection and development. Use the complete bundle for its verified runtime layout.

### Request format

| Field | Content |
|---|---|
| `issue` | Seven-digit issue identifier, encoded as a string. |
| `history` | 128–4,096 chronological rows, oldest first. Each row has six distinct red integers in 1–33 followed by one blue integer in 1–16. |

Only include results published before the intended prediction cutoff. The runtime uses the final 128 rows, validates the supplied rows, and accepts requests up to 1 MiB. It does not retrieve history automatically.

### Serve an API

```bash
python server.py --host 127.0.0.1 --port 8000 --device cpu
curl http://127.0.0.1:8000/health
curl -H 'Content-Type: application/json' \
  --data-binary @history.json http://127.0.0.1:8000/predict
```

The WSGI entry point is `server:application`; `NEWSPURPU_DEVICE` selects the device. The included server is intended for local or internal research use. Each response includes three tickets, their model probabilities, native H1 quantiles, and the actual core-file digest. Different complete tickets may share red numbers.

## Release at a glance

| Item | V9 release |
|---|---|
| Native core | TimesFM-3, 330,710,976 parameters |
| State tensors | 445 |
| Existing probability head | 40,951 parameters |
| Native V9 edit | `quantile_width_09`, 10,248 changed parameters |
| Context used | 128 observations per channel |
| Default output | Three distinct complete combinations |
| Complete bundle | 1,231,871,901 bytes |
| Runtime | Python 3.10/3.11; PyTorch with `torch.nn.RMSNorm` (2.4+); CPU or CUDA |

Runtime memory exceeds the FP32 weight-file size. No hardware-independent latency or memory benchmark is claimed.

### Artifact integrity

All 445 merged tensors were checked against the recorded V9 manifest. ZIP CRC and the decompressed core digest passed, and GitHub's asset digest matches the local bundle.

Bundle SHA-256:

```text
c27a418821069a6074eeeb119820bbe237c09852098a9bc4530aa6aa9bebac86
```

Modified core SHA-256:

```text
871b43eda6a72f03dd039cf59e686121dc57b7954f14fbd69b4b7e916b1ba402
```

## Research status

**The current experiments have not established a predictive advantage on real lottery outcomes.** Integrity and valid-output checks verify engineering properties; they do not demonstrate higher future accuracy or positive returns. Model probabilities are estimates, not measured winning frequencies. The sum for three complete outcomes is not a probability of winning any prize tier.

Existing V9 GPU verification records accompany the release. The three-output decoder passed synthetic engineering checks against the complete red-set space. A fresh end-to-end native forward of the newly assembled deployment wrapper and the Docker image remain unvalidated in a deployment environment. Historical records inside the bundle retain their original language; the English README and model card are the current public-facing guides.

## Provenance and licensing

newspurpu builds on [Google Research's TimesFM](https://github.com/google-research/timesfm). Pinned source commit: `e51928e27119cb17bebc005be2696b75e0a9e688`. Original model revision: `43046b85ec22d584a13f8098c2ed39c889e129c2`.

Repository-authored code is released under [Apache-2.0](LICENSE). TimesFM source and model weights have distinct license terms. The publisher has confirmed separate authorization for public redistribution of this modified checkpoint. This does not relicense the weights under Apache-2.0 or establish unrestricted downstream usage rights. Read [LICENSE_SCOPE.md](LICENSE_SCOPE.md) and the notices included with the checkpoint.

This independent research project is not an official or endorsed Google product.

## Contribute

Reproducible inference reports, decoder correctness checks, hardware measurements with complete configurations, and carefully separated historical/prospective evaluations are welcome. Include the checkpoint digest, runtime versions, reproduction steps, and the precise claim being tested.

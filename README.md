# AEGIS

**Awareness-Enhanced Guidance for Iterative Safeguard**

AEGIS is an exploratory framework for studying span-guided multilingual text
detoxification across English, Mandarin Chinese, and Korean. It separates a
span-level detector from frozen generator backbones so that the effect of
harmful-span, intensity, and target guidance can be examined without treating
the framework as a state-of-the-art claim.

| Resource | Status |
|---|---|
| Paper | Public archival link pending |
| Code | Detector training and guided-generation pipeline available |
| Data | Use the official upstream datasets described in [DATA.md](DATA.md) |
| License | [MIT](LICENSE) for code; upstream terms apply to data and models |

## Research question

When does explicit span-level guidance improve detoxification, and when does it
change the trade-off between toxicity reduction and meaning preservation?

## Components

| Component | Purpose |
|---|---|
| `aegis/datasets/` | Language-specific dataset processing and BIO supervision |
| `aegis/training/` | XLM-R detector training for English, Chinese, and Korean |
| `aegis/generation/` | Guided and unguided rewriting with frozen generators |
| `results/` | Compact detector training histories |

## Setup

```bash
git clone https://github.com/cosmic4dev/AEGIS.git
cd AEGIS
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Obtain the datasets described in [DATA.md](DATA.md), then adapt their local
paths through the loader arguments.

## Usage

Train a detector:

```bash
python -m aegis.training.english_xlmr_train --epochs 8 --patience 3
python -m aegis.training.chinese_xlmr_train --epochs 8 --patience 3
python -m aegis.training.korean_xlmr_train --epochs 8 --patience 3
```

Generate matched guided and unguided rewrites:

```bash
python -m aegis.generation.generate \
  --input_csv /path/to/evaluation_input.csv \
  --output_csv outputs/rewrites.csv \
  --generator_model_name Qwen/Qwen3-8B
```

The input CSV requires `sample_id`, `original_text`, `toxicity_strength`, and
`harmful_span_texts` columns.

## Evidence boundary

The repository supports inspection of the framework and reproduction with
properly obtained datasets. Span guidance is treated as a conditional control
signal: its benefit may vary with language, generator, and evaluation metric.
Raw datasets, generated-text pools, checkpoints, human-evaluation records, and
manuscript or submission sources are intentionally excluded.

## Related project

The focused guided-versus-unguided analysis is maintained separately in
[span-guided-detoxification](https://github.com/cosmic4dev/span-guided-detoxification).

## License

Code is released under the [MIT License](LICENSE). External datasets and models
retain their original licenses.

## Citation

Archival citation metadata will be added when a public paper record is
available. Until then, please reference this repository by its title and URL.

# Deploy Guide — K_PYEDFLIB
**Tier:** TIER_7_BIOSIGNALS_NEURO | **Stack:** Python 3.11, pyedflib 0.x, numpy, PAX 27B, AIOSS_FORMAT
**Air-gap capable after initial setup.**

## Prerequisites
Python 3.11+, pyedflib 0.1.x, numpy 1.26+. CPU-only.

## Environment
4GB RAM. CPU-only. No GPU required for file I/O.

## AIOSS Integration
```bash
aioss init --module K_PYEDFLIB --output ./k_pyedflib.aioss
aioss append --chain ./k_pyedflib.aioss --payload ./output.bin --module K_PYEDFLIB
aioss verify --chain ./k_pyedflib.aioss
```

## Air-Gap Setup
```bash
pip download -r requirements.txt -d ./wheels/
pip install --no-index --find-links ./wheels/ -r requirements.txt
```

## PAX 27B Harness Wiring
```python
from anticloud_pax import PAXHarness
harness = PAXHarness(
    model_path="./pax-27b-q4.gguf",
    module="K_PYEDFLIB",
    aioss_chain="./K_PYEDFLIB.aioss",
    classification="L5_NARROW_L2_GENERAL"
)
result = harness.process(input_data)
```

## Verification
```bash
aioss verify --chain ./K_PYEDFLIB.aioss --verbose
python -m K_PYEDFLIB.tests.smoke
```

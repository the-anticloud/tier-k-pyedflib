# Developer Cookbook — K_PYEDFLIB
**Stack:** Python 3.11, pyedflib 0.x, numpy, PAX 27B, AIOSS_FORMAT
**Domain:** PyEDFlib: EDF/BDF biosignal file I/O for Anticloud clinical data pipelines

## Read clinical EDF file
```python
from k_pyedflib import EDFReader

reader = EDFReader(
    pax_model="./pax-27b-q4.gguf",
    aioss_chain="./pyedflib.aioss"
)

edf = reader.read(
    path="./patient_001_eeg.edf",
    validate_hipaa=True,
    validate_signals=True
)
print(f"Patient ID: {edf.patient_id}")
print(f"Channels: {edf.signal_labels}")
print(f"Duration: {edf.duration_s:.0f}s at {edf.fs}Hz")
print(f"HIPAA compliant: {edf.hipaa_ok}")
print(f"AIOSS chain: {edf.chain_hash}")
```

## Write AIOSS-chained EDF
```python
reader.write_chained(signals=processed_signals, header=edf.header, path="./output.edf")
```

## AIOSS Chain Append
```python
import hashlib, time

def aioss_append(chain_path, payload: bytes, module_id: str):
    entry_hash = hashlib.sha3_256(payload).digest()
    ts = int(time.time_ns()).to_bytes(8, 'big')
    with open(chain_path, 'rb') as f:
        f.seek(-32, 2); prev_hash = f.read(32)
    new_hash = hashlib.sha3_256(prev_hash + entry_hash + ts).digest()
    with open(chain_path, 'ab') as f:
        f.write(ts + entry_hash + new_hash)
    return new_hash.hex()
```

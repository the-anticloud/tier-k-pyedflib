# L5 Narrow / L2 General Classification — K_PYEDFLIB
**Platform:** Anticloud | **Tier:** TIER_7_BIOSIGNALS_NEURO | **PAX:** 27B
**IP:** USPTO pending 2026, Anticloud FZ LLE, 0-1.gg | **License:** Apache-2.0

## L5 Narrow
K_PYEDFLIB provides EDF/EDF+/BDF file reading and writing for Anticloud clinical biosignal data. Narrow scope: Anticloud clinical data ingestion — reading hospital EDF files from EEG/PSG systems and writing AIOSS-chained EDF output.

## L2 General
L2 General: K_PYEDFLIB is the standard clinical file I/O for all TIER_7 biosignal processing. Every clinical EDF file entering Anticloud passes through K_PYEDFLIB for parsing and AIOSS chaining.

## PAX 27B Integration
PAX 27B validates EDF metadata on ingestion: checking signal labels, sampling rates, recording start times, and patient info fields for clinical correctness and HIPAA compliance.

## AIOSS Audit Chain
Every file ingestion (EDF file hash + metadata validation result + signal hash + HIPAA check result) is chained: H_n = SHA3-256(H_{n-1} || entry_hash_n || timestamp_n).
Offline-verifiable, tamper-evident, zero cloud dependency.

## Regulatory / Compliance
HIPAA. IEC 62304. EDF+ standard (European Data Format).

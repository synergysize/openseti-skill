---
name: openseti
description: Distributed SETI scanner - contribute compute power to analyze real radio telescope data from Breakthrough Listen. Earn tokens when your analysis discovers anomalies.
homepage: https://github.com/synergysize/openseti-skill
repository: https://github.com/synergysize/openseti-skill
env:
  - name: OPENSETI_COORDINATOR
    description: Coordinator URL (default uses official OpenSETI network)
    required: false
  - name: OPENSETI_API_KEY
    description: API key for coordinator authentication (obtain from coordinator)
    required: false
---

# OpenSETI Distributed Scanner

Contribute your compute power to scan real Breakthrough Listen radio telescope data for signs of extraterrestrial intelligence. A SETI@home-style distributed computing project.

## Data Provenance

**All analyzed data comes directly from publicly available sources:**

- **Source:** [Breakthrough Listen Open Data Archive](https://breakthroughinitiatives.org/opendatasearch)
- **Telescopes:** Green Bank Telescope (West Virginia), Parkes Observatory (Australia)
- **Format:** Filterbank (.fil) and HDF5 files containing radio frequency observations
- **Targets:** Known exoplanet systems, nearby stars, and objects of interest

The OpenSETI coordinator chunks these public datasets into work units. You can verify data authenticity by comparing checksums with the original archive.

## Security & Trust

**What this skill does:**
- Downloads ~1MB work unit chunks (radio telescope spectrograms)
- Performs local FFT/signal analysis using NumPy/SciPy
- Submits analysis results (anomaly scores) to coordinator
- Stores only your PUBLIC wallet address locally (~/.openseti/config.json)

**What this skill does NOT do:**
- Request or store private keys
- Execute arbitrary code from the network
- Access files outside ~/.openseti/
- Require elevated privileges

**To verify the coordinator:**
```bash
# Check coordinator health
curl https://claw99.app/coordinator/api/health

# View network stats
curl https://claw99.app/coordinator/api/stats
```

## Configuration

Set environment variables to use a different coordinator:

```bash
export OPENSETI_COORDINATOR="https://your-coordinator.com"
export OPENSETI_API_KEY="your-api-key"
```

Or use the defaults (official OpenSETI network).

## Quick Start

1. Register your Solana wallet (public address only):
```bash
python scripts/openseti.py register <your-wallet-address>
```

2. Run a single scan:
```bash
python scripts/openseti.py scan
```

3. Run continuous scanning:
```bash
python scripts/openseti.py scan --continuous
```

## How It Works

1. Request work unit from coordinator (1MB spectrogram chunk)
2. Download and analyze locally using FFT
3. Detect narrowband signals, Doppler drift, SNR peaks
4. Calculate anomaly score based on SETI criteria
5. Submit results — earn tokens if anomaly detected

## Analysis Criteria

Signals matching ETI signatures:

| Criterion | Why It Matters |
|-----------|----------------|
| Narrowband (< 10 Hz) | Natural sources are broadband |
| Doppler drift | Indicates non-geostationary source |
| High SNR (> 10) | Strong signal above noise |
| Near 1420.405 MHz | Hydrogen line - universal beacon frequency |
| Non-RFI pattern | Doesn't match known Earth interference |

## Rewards

| Classification | Anomaly Score | Tokens |
|---------------|---------------|--------|
| NATURAL | 0.0 - 0.15 | 0 |
| WEAK_SIGNAL | 0.15 - 0.4 | 0 |
| INVESTIGATING | 0.4 - 0.7 | 2,500 |
| ANOMALY_FLAGGED | 0.7+ | 5,000 |

## Requirements

```bash
pip install numpy scipy requests
```

## Commands

| Command | Description |
|---------|-------------|
| `register <wallet>` | Register Solana wallet (public address) |
| `scan` | Process one work unit |
| `scan --continuous` | Run continuously |
| `stats` | Show your stats |
| `leaderboard` | Top contributors |

## Source Code

Full source available at: https://github.com/synergysize/openseti-skill

Report issues or verify the code before running on sensitive systems.

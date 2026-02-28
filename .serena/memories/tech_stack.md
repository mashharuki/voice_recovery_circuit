# Tech Stack

## Languages
- Rust (edition 2021)
- Python (Poetry-managed, Python ^3.10 in root pyproject)
- TypeScript (Hardhat side)
- JavaScript/React (frontend app)
- Solidity (contracts)

## Core frameworks/libraries
- ZK / crypto: `halo2-base`, `halo2-ecc`, `snark-verifier`, `snark-verifier-sdk`, `poseidon`
- Python backend/API: Flask, flask-cors, numpy, torch, scikit-learn, soundfile
- Python-Rust bridge: PyO3 + maturin
- Frontend: React 18 + react-scripts (CRA) + MUI + ethers.js
- Contracts/testing: Hardhat toolbox, TypeScript, contract-sizer

## Build/package tooling
- Rust workspace (`Cargo.toml` at repo root)
- Poetry for Python deps (`pyproject.toml`)
- npm for frontend (`app/package.json`)
- npm/yarn artifacts present in `hardhat/` (prefer one package manager per working copy to avoid lockfile drift)

## Platform notes
- Development platform reported by Serena onboarding context: Darwin (macOS).
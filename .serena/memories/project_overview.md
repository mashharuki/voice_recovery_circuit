# Voice Recovery Circuit - Project Overview

## Purpose
This repository implements a voice-feature based recovery proof workflow that combines:
- biometric-like voice feature extraction and fuzzy commitment handling
- zero-knowledge proof generation/verification (Halo2 ecosystem)
- EVM verifier generation and Solidity/Hardhat integration
- a React frontend and Flask backend for demo/API usage

The top-level README also links PyO3 integration workflow and a typical setup pipeline:
`GenParams -> GenKeys -> GenEvmVerifier`.

## High-level architecture
- `eth_voice_recovery/`: core Rust zk circuit logic and CLI (`voice_recovery` binary)
- `voice_recovery_python/`: PyO3 Rust crate exposing bindings to Python via maturin
- `backend/`: Flask API + ML/feature related Python code
- `app/`: React (Create React App) frontend
- `hardhat/`: Solidity contracts, deploy scripts, and tests for verifier integration

## Entry points
- Rust CLI: `cargo run -p eth-voice-recovery --bin voice_recovery -- <subcommand>`
- Backend API: `python backend/app.py`
- Frontend app: from `app/`, `npm start`
- Smart contract tests: from `hardhat/`, `npx hardhat test`
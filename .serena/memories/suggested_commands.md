# Suggested Commands

## System utility commands (Darwin/macOS)
- `pwd`, `ls`, `cd`
- `git status`, `git diff`, `git log --oneline -n 20`
- `rg <pattern>`, `rg --files`, `find . -name '<name>'`

## Root-level setup
- `poetry install`
- `poetry shell`

## Rust workspace / zk circuit
- `cargo build`
- `cargo test`
- `cargo run -p eth-voice-recovery --bin voice_recovery -- --help`
- `cargo run -p eth-voice-recovery --bin voice_recovery -- GenParams --k <K> --params-path <PATH>`
- `cargo run -p eth-voice-recovery --bin voice_recovery -- GenKeys`
- `cargo run -p eth-voice-recovery --bin voice_recovery -- Prove`
- `cargo run -p eth-voice-recovery --bin voice_recovery -- EvmProve`
- `cargo run -p eth-voice-recovery --bin voice_recovery -- Verify`
- `cargo run -p eth-voice-recovery --bin voice_recovery -- GenEvmVerifier`

## PyO3 extension build
- `cd voice_recovery_python`
- `maturin develop`

## Python backend
- `python backend/app.py`

## Frontend (`app/`)
- `cd app`
- `npm install`
- `npm start`
- `npm test`
- `npm run build`

## Hardhat (`hardhat/`)
- `cd hardhat`
- `npm install` (or `yarn install`)
- `npx hardhat help`
- `npx hardhat test`
- `npx hardhat node`
- `npx hardhat run scripts/deploy.ts`
# Code Style & Conventions

## Observed conventions
- Rust: idiomatic snake_case for functions/variables, enum-based CLI with clap derive macros, explicit struct fields.
- Python: snake_case functions, Flask route handlers in `backend/app.py`, simple script-style module execution (`if __name__ == '__main__':`).
- React: component files use PascalCase names (e.g., `RecordButton.jsx`, `RegisterStatus.jsx`), standard CRA structure.
- Solidity/Hardhat: conventional contracts/tests/scripts layout.

## Formatting/linting posture
- No single repo-wide unified formatter/linter config detected at root.
- Frontend inherits CRA defaults (eslint via `react-scripts`).
- Python dev dependency includes `autopep8`.
- Rust formatting/testing should follow standard cargo tooling expectations.

## Practical guidance for edits
- Keep changes localized within subsystem boundaries (`app`, `backend`, `eth_voice_recovery`, `hardhat`).
- Match naming style already used in each language domain.
- Avoid introducing new cross-cutting tooling unless explicitly requested.
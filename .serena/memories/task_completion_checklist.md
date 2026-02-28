# Task Completion Checklist

Use this as a default finish checklist, then narrow to the subsystem you changed.

## 1) Validate changed subsystem
- Rust changes: run at least `cargo test` (or targeted package tests).
- Frontend changes (`app/`): run `npm test` and, when relevant, `npm run build`.
- Hardhat changes: run `npx hardhat test`.
- Backend Python changes: run the API entrypoint (`python backend/app.py`) and relevant manual/targeted checks.

## 2) Sanity checks
- Ensure no unintended file changes: `git status` and `git diff`.
- Keep lockfiles consistent with the package manager used in that subproject.

## 3) Integration awareness
- If touching proof/public input formats, verify downstream consumers (backend/hardhat/frontend) still align.
- If touching verifier or proof generation flow, re-check at least one end-to-end command path (`GenParams`/`GenKeys`/`EvmProve` etc.) as needed.

## 4) Final review
- Confirm naming/style matches nearby code.
- Note any commands not executed and why (time/dependency constraints).
## Testing

- **New or modified code ships with tests.** No retroactive coverage requirement.
- Frontend/libs: Vitest — `*.test.js`/`*.test.jsx` next to the file or in a sibling `__tests__/`.
- Backend: `node:test` + `node --test` (no framework needed) or Vitest if already present.
- If the project has no test script yet, add a minimal one (`"test": "vitest run"` or `"test": "node --test"`) when writing the first test — don't add infra without a test to run.
- Test: parsers, utils, zod schemas, services with business logic, edge cases (empty input, missing fields, malformed data).
- Don't test: trivial object construction, getters, config files.
- Order within a test file: imports → fixtures/constants → test cases → helpers last.

# Archive Provenance and Publication Changes

This source was supplied by the repository owner as Everything AI’s original voice-assistant delivery. It was preserved in `EverythingAI-wakeUp.zip` in a private Everything AI repository and imported here as extracted files on 2026-10-22. The ZIP’s original folder name and the assistant name **Sophia** are retained in the code.

## Changes for Publication

- Removed embedded Groq and Deepgram credential values from the Svelte component and standalone HTML demo.
- Replaced the original hosted text-to-speech endpoint with an invalid example endpoint.
- Disabled automatic microphone/network startup in both historical voice demos.
- Omitted deployment-specific AWS Amplify configuration; the original remains in the private archive.
- Replaced generic project documentation with English archive documentation.

The original ZIP and credentials are not included in this public repository. Removing credentials from this copy does not revoke old values; the owner should revoke or rotate any that remain active.

## Historical Constraints

The project declares Svelte/SvelteKit, LangChain/Groq, Deepgram-related dependencies, and AWS Amplify deployment configuration in the original archive. Model identifiers, APIs, and dependency versions are historical. This is source preservation, not a restored or production-ready voice service. No claim is made that the original backend or models remain available.

The original AWS deployment configuration was omitted from this public copy. No running service, account, deployment, or private repository was changed.

## Publication Validation

- `npm ci --ignore-scripts --no-audit --no-fund` completed using the historical lockfile. Dependency lifecycle scripts were not run.
- `npm run check` was executed and reported **10 JavaScript/type diagnostics and 2 Svelte warnings** in the historical component/page. These include implicit types, LangChain message-content typing, missing image alternative text, and an unused CSS selector. They are retained as documented archive limitations rather than claiming a restored application.
- Original embedded credential values were checked against every publication file and were absent. The raw ZIP is not published.
- No voice session, provider request, AWS deployment, or live service test was performed.

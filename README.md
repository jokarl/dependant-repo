## Caller Workflow
- `.github/workflows/call-main.yml` is manually triggered (`workflow_dispatch`) to execute limited work here, then invoke the privileged workflow inside `main-repo` via the GitHub CLI.
- Repository configuration in **Settings → Secrets and variables → Actions**:
  - Variable `DEPENDANT_PUBLIC_MESSAGE`: default message sent to main-repo (overridden by `call-message`).
  - Variable `DEPENDANT_CONTEXT_NOTE`: additional context forwarded to main-repo.
  - Secret `DEPENDANT_SECRET_FOR_MAIN`: demonstration secret forwarded when `call-secret` is blank.
  - Secret `GH_APP_ID`: numeric GitHub App ID used to mint an installation token.
  - Secret `GH_APP_INSTALLATION_ID`: installation ID for the same App (must have access to both repos).
  - Secret `GH_APP_PRIVATE_KEY`: GitHub App private key PEM (paste the PEM text directly; if you supply a base64 string the workflow auto-detects and decodes it).
- The workflow:
  1. Resolves the message/secret payload and logs dependant-repo values (with spaced secret for demo visibility).
  2. Generates a GitHub App installation token using a bash script (`openssl`, `jq`, `curl`)—no third-party actions required.
  3. Logs in to `gh` with that token.
  4. Calls `gh workflow run reusable.yml --repo your-org/main-repo --ref main ...` to trigger the privileged workflow, passing the computed inputs.
- Replace `your-org/main-repo` and `--ref main` in the trigger step to match the actual owner and reference you intend to target.
- Optional `workflow_dispatch` inputs:
  - `call-message` overrides `DEPENDANT_PUBLIC_MESSAGE`.
  - `call-secret` overrides `DEPENDANT_SECRET_FOR_MAIN` (values are visible in workflow logs—demo only).

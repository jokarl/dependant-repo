## Caller Workflow
- The workflow `.github/workflows/call-main.yml` is triggered manually via `workflow_dispatch` and calls the reusable workflow in `main-repo`.
- Update the reference `uses: your-org/main-repo/.github/workflows/reusable.yml@main` to match your GitHub owner and desired ref (branch/tag/SHA).
- Configure dependant repository values in **Settings → Secrets and variables → Actions**:
  - Variable `DEPENDANT_PUBLIC_MESSAGE`: default message sent as `caller-message`.
  - Variable `DEPENDANT_CONTEXT_NOTE`: forwarded as `caller-variable`.
  - Secret `DEPENDANT_SECRET_FOR_MAIN`: passed through as `caller-shared-secret`.
- When running the workflow you may provide an override for `call-message`; if left blank the job uses `DEPENDANT_PUBLIC_MESSAGE`.
- Actions logs in the main repository show both dependant-provided values and the main repository’s own secrets/variables for demonstration.

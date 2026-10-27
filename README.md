# APIsec Surface in GitHub Actions

Run [APIsec Surface](https://pypi.org/project/apisec-surface/) on every merge to `main` and send the results to APIsec.

The whole setup is one workflow file of about 30 lines of YAML you can read end to end.

## 1. Preview locally (optional)

With Python 3.11 or later, from your repository root:

```bash
pip install "apisec-surface==1.2.0"
apisec-surface analyze . --dry-run
```

It asks for your registration code, then prints the sub-projects and endpoints Surface finds. Nothing is uploaded.

## 2. Add two repository secrets

In **Settings → Secrets and variables → Actions**:

| Secret | Value |
| --- | --- |
| `APISEC_PAT` | Your APIsec personal access token |
| `APISEC_REGISTRATION_CODE` | The registration code (`###-###`) APIsec gave you |

## 3. Add the workflow

Copy [`workflow/apisec-surface.yml`](workflow/apisec-surface.yml) to `.github/workflows/apisec-surface.yml` in your repository and commit it.

It runs on every push to `main` and on demand. If several merges land close together, only the newest run continues. Surface identifies your repository from GitHub, so repeat runs update the same applications in APIsec instead of creating new ones.

## 4. Run it once by hand

Go to **Actions → APIsec Surface → Run workflow**. When the run finishes, the log prints a link to your applications in the APIsec console.

In a monorepo, Surface detects sub-projects automatically and creates one APIsec application for each.

## Useful options

Add these to the `apisec-surface analyze` line:

| Option | Use |
| --- | --- |
| `--exclude "<glob>"` | Skip paths, such as tests or generated code. Repeat as needed. |
| `--timeout <seconds>` | Give large repositories more time. |
| `--no-detect-monorepo` | Upload the repository as a single application. |
| `--dry-run` | Analyze and print a summary without uploading. |

## Help

Contact your APIsec team and include the failing run's log.

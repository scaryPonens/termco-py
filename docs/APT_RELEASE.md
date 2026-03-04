# Publishing `termcopy` as an apt package

This project now includes Debian packaging under `debian/` and a GitHub Actions workflow that builds `.deb` artifacts.

## What is already implemented

- Debian package metadata (`debian/control`, `debian/rules`, etc.)
- Basic autopkgtest smoke check
- CI workflow: `.github/workflows/deb-package.yml`
  - Builds `termcopy_*_all.deb`
  - Uploads package as GitHub Actions artifact

## External steps you need to take

To allow users to run `apt install termcopy`, you need a public apt repository.

This project is configured for Cloudsmith:

- owner/repo: `thetranscend/relay`
- distro/release path: `debian/bookworm`
- secret required in GitHub: `CLOUDSMITH_API_KEY`

### Minimum required setup

1. In Cloudsmith, ensure the repo supports Debian (`bookworm`).
2. Add `CLOUDSMITH_API_KEY` in GitHub repo secrets.
3. Create and push a version tag (`vX.Y.Z`) so the workflow publishes `.deb` to Cloudsmith.
4. Document install instructions for users (keyring + source list).

### Release trigger

```bash
git tag v0.2.1
git push origin v0.2.1
```

The `deb-package.yml` workflow will:
- build `.deb`
- upload artifacts to Actions
- publish `.deb` to Cloudsmith on `v*` tags

## Example user install (Cloudsmith, public repo)

Preferred (Cloudsmith generated setup script):

```bash
curl -1sLf 'https://dl.cloudsmith.io/public/thetranscend/relay/setup.deb.sh' | sudo -E bash
sudo apt-get update
sudo apt-get install termcopy
```

Install a pinned version:

```bash
sudo apt-get install termcopy=0.2.1-1
```

Manual source/key fallback:

```bash
curl -fsSL https://dl.cloudsmith.io/public/thetranscend/relay/gpg.key \
  | sudo gpg --dearmor -o /usr/share/keyrings/termcopy-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/termcopy-archive-keyring.gpg] https://dl.cloudsmith.io/public/thetranscend/relay/deb/debian bookworm main" \
  | sudo tee /etc/apt/sources.list.d/termcopy.list

sudo apt-get update
sudo apt-get install termcopy
```

## Optional next automation

- Add publish step to push `.deb` from CI directly into your apt repo when tags are created.
- Add a matrix build for `bookworm` and `trixie` (or Ubuntu releases) in containerized jobs.
- Add reproducibility/signing verification checks.

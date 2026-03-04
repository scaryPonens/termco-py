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

Choose one:

1. **Cloudsmith** (recommended easiest)
2. **Packagecloud**
3. **Self-hosted Aptly/reprepro**

### Minimum required setup

1. Create apt repo (distribution + component), e.g. `bookworm/main`.
2. Configure signing key (GPG key) for that repo.
3. Publish built `.deb` files to that repo.
4. Document install instructions for users (keyring + source list).

## Example user install (after you host a repo)

```bash
curl -fsSL <YOUR_REPO_GPG_URL> | sudo gpg --dearmor -o /usr/share/keyrings/termcopy-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/termcopy-archive-keyring.gpg] <YOUR_APT_REPO_URL> bookworm main" \
  | sudo tee /etc/apt/sources.list.d/termcopy.list

sudo apt update
sudo apt install termcopy
```

## Optional next automation

- Add publish step to push `.deb` from CI directly into your apt repo when tags are created.
- Add a matrix build for `bookworm` and `trixie` (or Ubuntu releases) in containerized jobs.
- Add reproducibility/signing verification checks.

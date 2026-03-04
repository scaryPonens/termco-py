# termcopy

[![CI](https://github.com/scaryPonens/termco-py/actions/workflows/test.yml/badge.svg)](https://github.com/scaryPonens/termco-py/actions/workflows/test.yml)

A Python CLI tool for terminal clipboard operations, replicating the functionality of a bash script that sends OSC 52 escape sequences.

## Installation

### PyPI

```bash
pip install termcopy
```

### Debian/Ubuntu via apt

This repo includes Debian packaging metadata and a CI workflow that builds `.deb` artifacts and publishes to Cloudsmith on `v*` tags.

#### User install steps (Cloudsmith)

Official Cloudsmith setup/install flow:

```bash
curl -1sLf 'https://dl.cloudsmith.io/public/thetranscend/relay/setup.deb.sh' | sudo -E bash
sudo apt-get update
sudo apt-get install termcopy
```

Install a specific version (example):

```bash
sudo apt-get install termcopy=0.2.1-1
```

If you manage apt sources manually:

```bash
curl -fsSL https://dl.cloudsmith.io/public/thetranscend/relay/gpg.key \
  | sudo gpg --dearmor -o /usr/share/keyrings/termcopy-archive-keyring.gpg

echo "deb [signed-by=/usr/share/keyrings/termcopy-archive-keyring.gpg] https://dl.cloudsmith.io/public/thetranscend/relay/deb/debian bookworm main" \
  | sudo tee /etc/apt/sources.list.d/termcopy.list

sudo apt update
sudo apt install termcopy
```

> See `docs/APT_RELEASE.md` for maintainer-side publishing steps.

### Maintainer validation checklist

After publishing a new tag (`vX.Y.Z`):

1. Confirm Debian workflow succeeded and uploaded `.deb` artifact.
2. Verify package is visible in Cloudsmith for `thetranscend/relay`.
3. Test install in a clean Debian container/VM:
   - add repo
   - `apt update`
   - `apt install termcopy`
4. Smoke-test command:

```bash
echo "hello" | termcopy
```

## Usage

The tool reads input from either a file or stdin, base64 encodes it, and outputs an OSC 52 escape sequence for clipboard operations.

### From a file:
```bash
termcopy filename.txt
```

### From stdin:
```bash
echo "Hello, World!" | termcopy
```

### From a pipe:
```bash
cat somefile.txt | termcopy
```

## How it works

The tool:
1. Detects whether input is coming from a terminal or pipe
2. Reads from a file argument if input is from terminal, otherwise reads from stdin
3. Base64 encodes the input data
4. Removes newlines from the base64 output
5. Wraps the result in an OSC 52 escape sequence for clipboard operations

## Development

To install in development mode:

```bash
pip install -e .
```

To install with development dependencies:

```bash
pip install -e ".[dev]"
```

### Running tests

```bash
pytest tests/ -v
```

### Code formatting and linting

```bash
# Format code
black src/ tests/

# Check code style
flake8 src/ tests/

# Type checking
mypy src/
```

## CI/CD

This project uses GitHub Actions for continuous integration and deployment:

- **Test workflow** (`.github/workflows/test.yml`): Runs on every push and pull request
  - Tests against Python 3.8-3.12
  - Runs linting (black, flake8, mypy)
  - Uploads coverage reports

- **Publish workflow** (`.github/workflows/publish.yml`): Runs on version tags
  - Builds and publishes to PyPI
  - Also publishes to TestPyPI

- **Manual release workflow** (`.github/workflows/release.yml`): Can be triggered manually
  - Allows custom version releases
  - Optional publishing to PyPI/TestPyPI

- **Auto release workflow** (`.github/workflows/auto-release.yml`): Automated version incrementing
  - Automatically calculates next version (patch/minor/major)
  - Creates GitHub releases with tags
  - Updates version files and commits changes
  - Optional publishing to PyPI/TestPyPI

### Setting up PyPI publishing

1. Create API tokens on [PyPI](https://pypi.org/manage/account/token/) and [TestPyPI](https://test.pypi.org/manage/account/token/)
2. Add the tokens as GitHub secrets:
   - `PYPI_API_TOKEN`: Your PyPI API token
   - `TEST_PYPI_API_TOKEN`: Your TestPyPI API token

### Automated Releases

The auto-release workflow provides three release types:

- **Patch** (0.1.0 → 0.1.1): Bug fixes and minor improvements
- **Minor** (0.1.0 → 0.2.0): New features, backward compatible
- **Major** (0.1.0 → 1.0.0): Breaking changes

The workflow will:
1. Run all tests to ensure quality
2. Calculate the new version based on current version and release type
3. Update version in `pyproject.toml` and `src/termcopy/__init__.py`
4. Create a GitHub release with the new version tag
5. Commit and push the version changes
6. Optionally publish to PyPI/TestPyPI

## License

MIT License - see LICENSE file for details.

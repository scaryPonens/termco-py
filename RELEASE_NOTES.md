# Release Notes

## [0.1.0] - 2024-01-XX

### 🎉 Initial Release

This is the first release of `termcopy`, a Python CLI tool that replicates the functionality of a bash script for terminal clipboard operations using OSC 52 escape sequences.

### ✨ Features

- **Exact Bash Script Replication**: Faithfully reproduces the behavior of the original bash script
- **Flexible Input Sources**: 
  - Read from file: `termcopy filename.txt`
  - Read from stdin: `echo "text" | termcopy`
  - Read from pipe: `cat file.txt | termcopy`
- **OSC 52 Escape Sequences**: Outputs properly formatted OSC 52 escape sequences for clipboard operations
- **Base64 Encoding**: Automatically encodes input data in base64 format
- **Cross-Platform**: Works on any system with Python 3.7+

### 🔧 Technical Details

- **Python Package**: Properly packaged with `pyproject.toml` and `setup.py`
- **Source Layout**: Uses modern `src/` layout for better development practices
- **Type Hints**: Full type annotations for better code quality
- **Error Handling**: Comprehensive error handling for file operations
- **CLI Interface**: Clean command-line interface with help and version options

### 🧪 Quality Assurance

- **Test Coverage**: 12 comprehensive tests covering all functionality
- **Code Quality**: 
  - Black code formatting
  - Flake8 style checking
  - MyPy type checking
- **CI/CD**: GitHub Actions workflows for testing and publishing
- **Documentation**: Complete README with usage examples

### 📦 Installation

```bash
pip install termcopy
```

### 🚀 Usage Examples

```bash
# From file
termcopy myfile.txt

# From stdin
echo "Hello, World!" | termcopy

# From command output
cat somefile.txt | termcopy
```

### 🔗 Links

- **GitHub**: https://github.com/scaryPonens/termco-py
- **PyPI**: https://pypi.org/project/termcopy/
- **Documentation**: See README.md for detailed usage instructions

### 🙏 Acknowledgments

- Original bash script that inspired this Python implementation
- Python packaging community for best practices
- GitHub Actions for CI/CD infrastructure

---

**Note**: This tool is designed to work with terminals that support OSC 52 escape sequences for clipboard operations. The output is an escape sequence that can be interpreted by compatible terminal emulators.

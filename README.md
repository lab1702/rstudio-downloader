# RStudio Downloader for Linux

A robust bash script to automatically detect your Linux distribution and download the latest version of RStudio Desktop.

## Features

- 🔍 **Automatic Distribution Detection**: Detects Ubuntu, Debian, Fedora, RHEL, CentOS, openSUSE, and derivatives
- 📦 **Smart Package Selection**: Downloads the correct package format (.deb or .rpm) for your system
- 🔄 **Version Detection**: Automatically fetches the latest RStudio version with multiple fallback methods
- ✅ **URL Verification**: Validates download URLs before downloading
- 📊 **Progress Indicators**: Shows download progress with wget or curl
- 🛡️ **Safe Downloads**: Uses temporary files and atomic moves to prevent corruption
- 🎨 **Color-Coded Output**: Clear, colored output for better readability (when supported)
- 🔧 **Multiple Options**: Test mode, force download, quiet mode, custom directories, and debug output

## Prerequisites

The script requires the following tools:
- `bash` (version 4.0 or higher)
- `curl` (for version detection and optional downloading)
- Either `wget` or `curl` (for downloading)
- Standard Unix tools: `grep`, `uname`, `mktemp`

## Installation

1. Download the script:
```bash
curl -O https://raw.githubusercontent.com/lab1702/rstudio-downloader/main/download-rstudio.sh
# or
wget https://raw.githubusercontent.com/lab1702/rstudio-downloader/main/download-rstudio.sh
```

2. Make it executable:
```bash
chmod +x download-rstudio.sh
```

## Usage

### Basic Usage

Download RStudio to your current directory:
```bash
./download-rstudio.sh
```

### Test Mode

See what would be downloaded without actually downloading:
```bash
./download-rstudio.sh --test
```

### Command Line Options

```
Usage: download-rstudio.sh [OPTIONS]

OPTIONS:
    -h, --help          Show help message
    -v, --version       Show script version
    -t, --test          Test mode (show what would be downloaded)
    -f, --force         Force download even if file exists
    -q, --quiet         Quiet mode (minimal output)
    -d, --dir DIR       Download directory (default: current directory)
    --debug             Enable debug output
```

### Examples

```bash
# Download to a specific directory
./download-rstudio.sh -d /tmp

# Force download with quiet output
./download-rstudio.sh --force --quiet

# Test mode with debug output
./download-rstudio.sh --test --debug

# Download to custom directory with force
./download-rstudio.sh -d ~/Downloads -f
```

## Supported Distributions

### Debian-based (.deb)
- Ubuntu (all versions, with optimized builds for 20.04+)
- Debian
- Linux Mint
- Pop!_OS
- Elementary OS
- Zorin OS

### RPM-based (.rpm)
- Fedora
- Red Hat Enterprise Linux (RHEL)
- CentOS
- Rocky Linux
- AlmaLinux
- openSUSE
- SUSE Linux Enterprise

### Not Supported
- Arch Linux and derivatives (use AUR instead: `yay -S rstudio-desktop`)
- Non-x86_64 architectures (ARM, i386, etc.)

## How It Works

1. **Distribution Detection**: Reads `/etc/os-release` to identify your Linux distribution
2. **Version Fetching**: 
   - Primary: Queries RStudio's version endpoint
   - Fallback 1: GitHub API for latest release
   - Fallback 2: Hardcoded known working version
3. **URL Construction**: Builds the download URL based on your distribution and version
4. **URL Verification**: Checks if the download URL is valid (HTTP 200)
5. **Safe Download**: Downloads to a temporary file, then atomically moves to final location
6. **Installation Instructions**: Provides distribution-specific installation commands

## Installation Instructions

After downloading, the script will provide installation instructions specific to your distribution:

### For Debian/Ubuntu:
```bash
sudo apt update
sudo apt install ./rstudio-*.deb
```

### For Fedora:
```bash
sudo dnf install ./rstudio-*.rpm
```

### For RHEL/CentOS:
```bash
sudo yum install ./rstudio-*.rpm
```

### For openSUSE:
```bash
sudo zypper install ./rstudio-*.rpm
```

## Best Practices Implemented

### Security
- Uses `set -euo pipefail` for strict error handling
- Validates all inputs and paths including filename sanitization
- Uses temporary files with secure permissions (600) for safe downloads
- Properly quotes all variables and uses proper scoping
- Validates download URLs are from official RStudio servers only
- HTTPS-only downloads with redirect limits
- File size validation to detect corruption or malicious files
- Input validation for version strings and directories

### Robustness
- Multiple fallback methods for version detection
- Comprehensive error messages with standardized exit codes
- Atomic file operations with cleanup on failure
- Signal trap handling for cleanup
- Directory permission checks and validation
- Detailed logging and debug output
- Input sanitization and validation

### User Experience
- Color-coded output with terminal detection
- Progress indicators during download
- Clear error messages with suggestions
- Quiet mode for scripting
- Test mode for verification

### Code Quality
- Consistent naming conventions with organized function groups
- Modular function design with clear separation of concerns
- Proper use of `readonly` for constants and `declare` for variables
- Local variables in functions with proper scoping
- Comprehensive documentation with function headers
- ShellCheck compliance for static analysis
- Standardized exit codes for different error types

## Troubleshooting

### "Cannot detect Linux distribution"
- Ensure `/etc/os-release` exists on your system
- Try running with `--debug` for more information

### "URL verification failed"
- Check your internet connection
- The RStudio download servers might be temporarily unavailable
- Try again later or check https://posit.co/download/rstudio-desktop/

### "Neither wget nor curl is installed"
Install one of them:
```bash
# Debian/Ubuntu
sudo apt install wget
# or
sudo apt install curl

# Fedora
sudo dnf install wget
# or
sudo dnf install curl
```

### Permission Errors
- Ensure you have write permissions to the download directory
- Try downloading to a different directory with `-d`

## Exit Codes

The script uses standardized exit codes for different types of errors:

- `0` - Success
- `1` - General error
- `2` - Missing dependency
- `3` - Unsupported system/distribution
- `4` - Network error (download failed, URL invalid)
- `5` - File error (permissions, file exists without --force)

## Contributing

Contributions are welcome! Please:
1. Test your changes on multiple distributions
2. Follow the existing code style
3. Update documentation as needed
4. Add debug output for new features

## License

This script is released under the MIT License. See the script header for details.

## Disclaimer

This is an unofficial script not affiliated with Posit (formerly RStudio, Inc.). RStudio is a trademark of Posit Software, PBC.

## Version History

- **1.0.0** (Current)
  - Initial release with full feature set
  - Support for major Linux distributions  
  - Comprehensive error handling with standardized exit codes
  - Security enhancements including URL validation and file size checks
  - Downloads to current directory by default instead of home directory
  - Enhanced input validation and sanitization
  - ShellCheck compliance and improved code quality
  - Detailed function documentation and improved error messages
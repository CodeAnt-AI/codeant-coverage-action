# Local Development Guide

This guide will help you set up and test the CodeAnt AI Coverage Upload Action locally.

## Prerequisites

- Git
- GitHub account
- Basic understanding of GitHub Actions
- A test repository with coverage reports

## Project Structure

```
codeant-coverage-action/
├── action.yml              # Action metadata and workflow definition
├── README.md              # Public documentation
├── README_LOCAL.md        # This file - local development guide
└── LICENSE                # License file
```

## Local Setup

### 1. Clone the Repository

```bash
git clone https://github.com/CodeAnt-AI/codeant-coverage-action.git
cd codeant-coverage-action
```

### 2. Test the Action Locally

#### Using act (Local GitHub Actions Runner)

Install [act](https://github.com/nektos/act):

```bash
# macOS
brew install act

# Linux
curl https://raw.githubusercontent.com/nektos/act/master/install.sh | sudo bash

# Windows
choco install act-cli
```

Create a test workflow in `.github/workflows/test.yml`:

```yaml
name: Test Action

on: [push]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Create dummy coverage file
        run: |
          echo '<?xml version="1.0" ?>
          <coverage version="1.0">
            <packages>
              <package name="test">
                <classes>
                  <class filename="test.py" line-rate="0.8">
                  </class>
                </classes>
              </package>
            </packages>
          </coverage>' > coverage.xml

      - name: Test coverage upload
        uses: ./
        with:
          access_token: ${{ secrets.CODEANT_ACCESS_TOKEN }}
          coverage_file: coverage.xml
```

Run the action locally:

```bash
act -j test --secret CODEANT_ACCESS_TOKEN=your_token_here
```

### 3. Test in a Real Repository

#### Option A: Test in a Private Repository

1. Push your action to a repository
2. Create a test repository
3. Add the action to your workflow:

```yaml
- uses: CodeAnt-AI/codeant-coverage-action@v0.0.1
  with:
    access_token: ${{ secrets.CODEANT_ACCESS_TOKEN }}
    coverage_file: coverage.xml
```

#### Option B: Test with Different Coverage Formats

Create test files for different formats:

**Python (coverage.xml)**
```bash
pip install pytest pytest-cov
pytest --cov --cov-report=xml
```

**JavaScript (coverage.json)**
```bash
npm test -- --coverage --coverageReporters=json
```

**Go (coverage.out)**
```bash
go test -coverprofile=coverage.out ./...
```

## Making Changes

### 1. Modify action.yml

Edit the `action.yml` file to change inputs, outputs, or the workflow steps.

### 2. Update Documentation

Update `README.md` with any new features or changes to inputs/outputs.

### 3. Test Changes

Always test changes locally using `act` or in a test repository before publishing.

### 4. Version Control

Follow semantic versioning for releases:
- `v0.0.1` - Initial release
- `v0.1.0` - New features (backward compatible)
- `v0.0.2` - Bug fixes
- `v1.0.0` - First stable release

## Publishing to Marketplace

### 1. Prepare for Release

```bash
# Ensure all files are committed
git add .
git commit -m "Prepare for v0.0.1 release"
git push origin main
```

### 2. Create a Release Tag

```bash
# Create and push a version tag
git tag -a v0.0.1 -m "Initial release"
git push origin v0.0.1
```

### 3. Create GitHub Release

1. Go to your repository on GitHub
2. Click "Releases" → "Draft a new release"
3. Choose your tag (v0.0.1)
4. Fill in release notes:
   - Title: `v0.0.1 - Initial Release`
   - Description: List of features and changes
5. Check "Publish this Action to the GitHub Marketplace"
6. Add categories and tags
7. Click "Publish release"

### 4. Marketplace Checklist

Before publishing, ensure:
- [ ] `action.yml` has all required fields (name, description, author)
- [ ] `README.md` is comprehensive and includes usage examples
- [ ] `LICENSE` file exists
- [ ] Branding (icon and color) is set in `action.yml`
- [ ] Repository has a clear description
- [ ] Repository topics are added (github-actions, coverage, testing)

## Debugging

### Enable Debug Logging

Set the `ACTIONS_STEP_DEBUG` secret to `true` in your repository:

```yaml
- name: Upload coverage
  uses: ./
  with:
    access_token: ${{ secrets.CODEANT_ACCESS_TOKEN }}
    coverage_file: coverage.xml
  env:
    ACTIONS_STEP_DEBUG: true
```

### Common Issues

1. **Script download fails**: Check API_BASE URL is correct
2. **Base64 decode error**: Verify the script endpoint returns base64 content
3. **Permission denied**: Ensure `chmod +x` runs successfully
4. **Coverage file not found**: Check the file path is relative to workspace

### Manual Testing

Test the script manually:

```bash
# Download script
curl -sS -X GET "https://api.codeant.ai/pr/analysis/coverage/script/get" \
  --output upload_coverage.sh.b64

# Decode
base64 -d upload_coverage.sh.b64 > upload_coverage.sh

# Make executable
chmod +x upload_coverage.sh

# Run with test parameters
bash upload_coverage.sh \
  -t "YOUR_TOKEN" \
  -r "user/repo" \
  -c "abc123" \
  -f "coverage.xml" \
  -p "github" \
  -b "main" \
  -u "https://github.com"
```

## Best Practices

1. **Test thoroughly** before each release
2. **Document all changes** in release notes
3. **Keep README up to date** with latest features
4. **Use semantic versioning** for all releases
5. **Maintain backward compatibility** when possible
6. **Respond to issues** promptly
7. **Add tests** for new features

## Resources

- [GitHub Actions Documentation](https://docs.github.com/en/actions)
- [Creating Actions](https://docs.github.com/en/actions/creating-actions)
- [Publishing Actions](https://docs.github.com/en/actions/creating-actions/publishing-actions-in-github-marketplace)
- [act - Local Testing](https://github.com/nektos/act)

## Support

For questions or issues during development:
- Open an issue in the repository
- Contact: dev@codeant.ai
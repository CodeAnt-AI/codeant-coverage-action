# CodeAnt AI Coverage Upload Action

[![GitHub Marketplace](https://img.shields.io/badge/Marketplace-CodeAnt%20AI%20Coverage-blue.svg)](https://github.com/marketplace/actions/codeant-ai-coverage-upload)
[![License](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

Upload test coverage reports to CodeAnt AI for comprehensive analysis, visualization, and tracking of your code coverage metrics.

## Features

- 📊 Upload coverage reports in multiple formats (XML, JSON, LCOV, etc.)
- 🔍 Automatic coverage analysis and insights
- 📈 Track coverage trends over time
- 🎯 Integration with pull requests
- 🚀 Easy setup with minimal configuration

## Usage

### Basic Example

```yaml
name: Test Coverage

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  coverage:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Run tests with coverage
        run: |
          # Your test command that generates coverage report
          pytest --cov --cov-report=xml

      - name: Upload coverage to CodeAnt AI
        uses: CodeAnt-AI/codeant-coverage-action@v0.0.1
        with:
          access_token: ${{ secrets.CODEANT_ACCESS_TOKEN }}
          coverage_file: coverage.xml
```

### Advanced Example

```yaml
- name: Upload coverage to CodeAnt AI
  uses: CodeAnt-AI/codeant-coverage-action@v0.0.1
  with:
    access_token: ${{ secrets.CODEANT_ACCESS_TOKEN }}
    coverage_file: coverage.json
    api_base: https://api.codeant.ai
    platform: github
    base_url: https://github.com
```

## Inputs

| Input | Description | Required | Default |
|-------|-------------|----------|---------|
| `access_token` | GitHub Access Token for authentication | Yes | - |
| `coverage_file` | Path to the coverage file (e.g., coverage.xml, coverage.json) | Yes | `coverage.xml` |
| `api_base` | CodeAnt AI API base URL | No | `https://api.codeant.ai` |
| `platform` | Git platform (github, gitlab, bitbucket) | No | `github` |
| `base_url` | Base URL of the git platform | No | `https://github.com` |

## Supported Coverage Formats

- Cobertura XML (`.xml`)
- JaCoCo XML
- LCOV (`.lcov`)
- JSON coverage reports
- And more...

## Setup

### 1. Generate Coverage Report

First, ensure your test suite generates a coverage report. Here are examples for common languages:

#### Python (pytest)
```bash
pytest --cov --cov-report=xml
```

#### JavaScript/TypeScript (Jest)
```bash
jest --coverage --coverageReporters=cobertura
```

#### Java (Maven)
```bash
mvn test jacoco:report
```

#### Go
```bash
go test -coverprofile=coverage.out ./...
gocov convert coverage.out | gocov-xml > coverage.xml
```

### 2. Add Secrets

Add your CodeAnt AI access token to your repository secrets:
1. Go to your repository Settings
2. Navigate to Secrets and variables → Actions
3. Click "New repository secret"
4. Name: `CODEANT_ACCESS_TOKEN`
5. Value: Your CodeAnt AI access token

### 3. Configure Workflow

Add the action to your GitHub Actions workflow as shown in the usage examples above.

## How It Works

1. The action fetches the latest coverage upload script from CodeAnt AI
2. Decodes and prepares the script for execution
3. Uploads your coverage report along with commit and branch information
4. CodeAnt AI processes the report and provides analysis

## Troubleshooting

### Coverage file not found
Ensure the `coverage_file` path is correct and the file exists before running this action.

### Authentication failed
Verify that your `CODEANT_ACCESS_TOKEN` is valid and has the necessary permissions.

### Script execution failed
Check that the CodeAnt AI API is accessible from your runner environment.

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

- 📧 Email: support@codeant.ai
- 📚 Documentation: https://docs.codeant.ai
- 🐛 Issues: https://github.com/CodeAnt-AI/codeant-coverage-action/issues

## Related

- [CodeAnt AI](https://codeant.ai) - AI-powered code analysis platform
- [GitHub Actions Documentation](https://docs.github.com/en/actions)
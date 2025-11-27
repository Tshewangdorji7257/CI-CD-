# GitHub Actions CI Pipeline Setup

This directory contains the GitHub Actions workflow configuration for the Continuous Integration (CI) pipeline.

## Workflow: `ci.yml`

The CI pipeline automatically runs on every push to the `main` branch and performs the following steps:

1. **Code Checkout** - Retrieves the latest code from the repository
2. **Dependency Installation** - Installs all required npm packages
3. **Unit Testing** - Runs Jest tests to verify code functionality
4. **Security Scanning** - Uses Snyk to identify vulnerabilities
5. **Code Quality Analysis** - Uses SonarCloud to analyze code quality
6. **Project Build** - Compiles/builds the project

## Required Secrets

To use this workflow, you need to configure the following secrets in your GitHub repository:

### Setting up secrets:
1. Go to your GitHub repository
2. Click on `Settings` → `Secrets and variables` → `Actions`
3. Click `New repository secret`
4. Add the following secrets:

#### SNYK_TOKEN
- **Purpose**: Authentication for Snyk security scanning
- **How to get it**:
  1. Sign up at [https://snyk.io](https://snyk.io)
  2. Go to Account Settings → General
  3. Copy your API token
  4. Add it as `SNYK_TOKEN` in GitHub secrets

#### SONAR_TOKEN
- **Purpose**: Authentication for SonarCloud code analysis
- **How to get it**:
  1. Sign up at [https://sonarcloud.io](https://sonarcloud.io)
  2. Create a new organization or use an existing one
  3. Go to My Account → Security
  4. Generate a new token
  5. Add it as `SONAR_TOKEN` in GitHub secrets

## SonarCloud Configuration

Update the following values in `ci.yml`:
```yaml
-Dsonar.projectKey=your-project-key
-Dsonar.organization=your-organization
```

You can find these values in your SonarCloud project settings.

## Required package.json Scripts

Ensure your `package.json` includes these scripts:

```json
{
  "scripts": {
    "test": "jest",
    "build": "your-build-command"
  }
}
```

## Workflow Triggers

Currently, the workflow triggers on:
- Push to `main` branch

You can modify the trigger in the `on:` section of `ci.yml` to include:
- Pull requests
- Specific branches
- Scheduled runs
- Manual triggers

## Viewing Results

- **Test Results**: Check the "Run unit tests" step in the workflow run
- **Security Issues**: View in the "Security" tab of your repository
- **Code Quality**: View detailed reports on SonarCloud dashboard
- **Build Artifacts**: Download from the workflow run summary page

## Troubleshooting

### Workflow fails on dependencies
- Ensure `package.json` and `package-lock.json` are committed
- Verify Node.js version compatibility

### Snyk scan fails
- Check if `SNYK_TOKEN` is correctly configured
- Verify Snyk supports your project type

### SonarCloud analysis fails
- Ensure `SONAR_TOKEN` is valid
- Verify project key and organization are correct
- Check if all required files are present

## Best Practices

1. Keep secrets secure and never commit them to the repository
2. Regularly update action versions (e.g., `@v4` to latest)
3. Review security and quality reports regularly
4. Set appropriate severity thresholds for Snyk scans
5. Configure quality gates in SonarCloud for automated checks

# GitHub Actions

This repository contains reusable GitHub Actions to streamline workflows for code quality checks and Xcode build/testing. These actions can be easily integrated into your workflows to ensure consistent quality and testing processes.

---

## Actions in this Repository

### 1. **Code Quality with Ktlint**

#### Description
This action runs code quality checks using [Ktlint](https://github.com/pinterest/ktlint), a Kotlin linter and formatter. It helps maintain clean and consistent code styles in your Kotlin projects.

#### Inputs
No inputs are required.

#### Usage
Below is an example workflow using the `Code Quality with Ktlint` action:

```yaml
name: Code Quality Check

on:
  pull_request:
    branches:
      - main

jobs:
  ktlint-check:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run Ktlint
        uses: mtdtechnology-net/github-actions/.github/actions/ktlint@1.0.4/
```

---

### 2. **Xcode Test Runner**

#### Description
This action runs Xcode builds and tests using the `xcodebuild` command. It is designed to work with iOS projects and ensures automated testing through GitHub Actions.

#### Inputs
| Input       | Description                        | Required | Default      |
|-------------|------------------------------------|----------|--------------|
| `scheme`    | The scheme to build and test       | Yes      | N/A          |
| `project`   | The project file path             | Yes      | N/A          |
| `device`    | The iOS Simulator device name     | No       | `iPhone 16`  |
| `os`        | The iOS Simulator OS version      | No       | `18.0`       |
| `testPlan`  | The test plan to use              | Yes      | N/A          |

#### Usage
Below is an example workflow using the `Xcode Test Runner` action:

```yaml
name: Run Xcode Tests

on:
  push:
    branches:
      - main
  pull_request:

jobs:
  xcode-tests:
    runs-on: macos-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run Xcode Tests
        uses: mtdtechnology-net/github-actions/.github/actions/xcode-test@1.0.4/
        with:
          scheme: "YourScheme"
          project: "YourProject.xcodeproj"
          device: "iPhone 16"
          os: "18.0"
          testPlan: "YourTestsPlan"
          enableCodeCoverage: "YES" 
```

---

### 3. **Xcodebuild SPM Test Action**

This reusable GitHub Action is designed to run tests for Swift Package Manager or Xcode projects using the `xcodebuild` command. It supports specifying the scheme, device, OS version, and the directory containing the project.

#### Inputs

| Input Name  | Required | Description                                                                 | Example                      |
|-------------|----------|-----------------------------------------------------------------------------|------------------------------|
| `scheme`    | Yes      | The Xcode scheme to build and test.                                         | `YourScheme`                        |
| `device`    | Yes      | The device to use for the test destination.                                | `iPhone 16`                  |
| `os`        | Yes      | The OS version to use for the test destination.                            | `16.0`                       |
| `path`      | Yes      | The path to the Swift Package or Xcode project (relative to the repository root). | `/PathToSPM`                        |

---

#### Outputs

| Output Name      | Description                                |
|------------------|--------------------------------------------|
| `test-result`    | The result of the `xcodebuild` command.    |

---

#### Usage

Below is an example of how to use the reusable action in a workflow file:

```yaml
name: Run Xcode Tests

on:
  push:
    branches:
      - main
  pull_request:
    branches:
      - main

jobs:
  run-tests:
    runs-on: macos-latest

    steps:
      - name: Run Tests Using Xcodebuild Action
        uses: mtdtechnology-net/github-actions/.github/actions/run-xcodebuild-tests@1.0.4
        with:
          scheme: "YourScheme"            
          device: "iPhone 16"        
          os: "16.0"
          path: "/PathToYourSPM"
          enableCodeCoverage: "YES" 
```

### 4. **Code Quality with SwiftLint**

#### **Description**
This action runs code quality checks using [SwiftLint](https://github.com/realm/SwiftLint), a linter and style checker for Swift. It ensures clean and consistent code styles in your Swift projects by identifying and enforcing Swift best practices.

#### **Inputs**
| Input Name     | Description                                             | Required | Default       |
|----------------|---------------------------------------------------------|----------|---------------|
| `arguments`    | Additional arguments to customize the SwiftLint command (e.g., `--strict`, `--reporter json`). | No       | `''` (none)   |

#### **Usage**
Below is an example workflow using the `Code Quality with SwiftLint` action:

```yaml
name: Code Quality Check

on:
  pull_request:
    branches:
      - main

jobs:
  swiftlint-check:
    runs-on: macos-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run SwiftLint
        uses: mtdtechnology-net/github-actions/.github/actions/swiftlint@1.0.4
        with:
          arguments: '--strict'
```

#### **Example Arguments**
You can pass additional arguments to customize SwiftLint's behavior:
- `--strict`: Enforce stricter lint rules.
- `--reporter json`: Output results in JSON format.

#### **SwiftLint Configuration**
This action will automatically detect a `.swiftlint.yml` configuration file in your repository. Ensure the file exists and is properly configured for your project's linting needs.

---

### **Benefits**
- Helps maintain clean and consistent Swift code style.
- Automates SwiftLint checks in CI workflows.
- Configurable through `.swiftlint.yml` and `arguments` for flexibility.

#### **Example Workflow for Changed Files Only**
To lint only files changed in a pull request, you can use:

```yaml
jobs:
  swiftlint-check:
    runs-on: macos-latest

    steps:
      - name: Checkout code
        uses: actions/checkout@v3

      - name: Run SwiftLint on Changed Files
        uses: mtdtechnology-net/github-actions/.github/actions/swiftlint@1.0.4
        with:
          arguments: '--strict'
```

This ensures efficient linting for incremental changes.

---

#### How It Works

2. **Navigate to the Target Directory:**
   - The action dynamically navigates to the directory specified in the `path` input.

3. **Run Tests with Xcodebuild:**
   - The `xcodebuild` command is executed with the provided `scheme`, `device`, and `os`.

---

### 5. **Cleanup Workspace**

#### **Description**
This action performs cleanup of the GitHub Actions workspace by removing all files and directories. It ensures a clean slate for subsequent steps in the workflow.

#### **Inputs**
This action does not require any inputs.

#### **Usage**
Below is an example workflow using the `Cleanup Workspace` action:

```yaml
name: Cleanup Workspace

on:
  pull_request:
    branches:
      - main

jobs:
  cleanup-job:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Cleanup Workspace
        uses: mtdtechnology-net/github-actions/.github/actions/cleanup@1.0.7
```

---

### **Benefits**
- **Ensures a Clean Workspace:** Removes all files and directories in the `github.workspace` to avoid interference between workflow steps.
- **Simple and Efficient:** Runs a single `rm -rf` command to clean the workspace.

#### **How It Works**
1. **Cleanup Command:**
   - The action executes the `rm -rf` command to remove all files and folders from the workspace.
2. **Reusable in Any Workflow:**
   - The action can be used in any job where a clean workspace is needed.

---

### 6. **Slack Notification**

#### **Description**
This action sends Slack notifications using a predefined reusable workflow. It can notify team members about build statuses, deployments, or any other events in your GitHub Actions workflows.

#### **Inputs**
| Input Name     | Description                                         | Required | Type   |
|----------------|-----------------------------------------------------|----------|--------|
| `status`       | The status of the build (e.g., success, failure).   | Yes      | string |
| `message`      | A custom message to include in the notification.    | Yes      | string |
| `url`          | A URL to include in the notification (e.g., a PR or commit URL). | Yes | string |

#### **Usage**
Below is an example workflow using the `Slack Notification` reusable action:

```yaml
name: Notify Slack

on:
  push:
    branches:
      - main

jobs:
  notify-slack-job:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Notify Slack
        uses: org-name/shared-workflows/.github/workflows/slack-notification.yml@1.0.9
        with:
          status: success
          message: "Build and test completed for branch: ${{ github.ref_name }}"
          url: ${{ github.event.head_commit.url }}
```

---

#### **Benefits**
- **Keeps Teams Updated:** Sends real-time notifications to Slack channels about workflow statuses.
- **Highly Customizable:** Allows you to specify custom messages, build statuses, and URLs.
- **Reusable Across Workflows:** Centralizes Slack notifications in a single reusable action.

#### **How It Works**
1. **Slack Webhook Setup:**
   - Uses an Incoming Webhook to send messages to a specific Slack channel.
   - Requires a Slack Webhook URL stored as a GitHub secret (`SLACK_WEBHOOK_URL`).

2. **Dynamic Inputs:**
   - Accepts `status`, `message`, and `url` inputs to generate detailed and actionable Slack notifications.

3. **Reusable in Any Workflow:**
   - The action can be called from multiple repositories or workflows, ensuring consistent notifications.

---

### 7. **Xcov**

#### **Description**
This action generates code coverage after running tests in your Xcode project and validates the result against a specified coverage threshold.

#### **Inputs**
| Input Name  | Description                                    | Required | Type   |
|-------------|------------------------------------------------|----------|--------|
| `scheme`    | The Xcode scheme used to build and test.       | Yes      | string |
| `project`   | The Xcode project to build and test.           | Yes      | string |
| `coverage`  | Minimum coverage threshold to validate against.| Yes      | string |

#### **Usage**
Below is an example workflow using the `Xcov` reusable action:

```yaml
name: Generate Code Coverage

on:
  pull_request:
    branches:
      - main

jobs:
  generate-coverage-job:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Install Dependencies
        run: |
          sudo gem install bundler
          bundle install

      - name: Generate Code Coverage
        uses: org-name/shared-workflows/.github/workflows/generate-coverage.yml@1.0.0
        with:
          scheme: "MyAppScheme"
          project: "MyApp.xcodeproj"
          coverage: "80"
```

---


### 8. **API Key**

#### **Description**
This action gets an API key from the environment and creates a JSON file out of it.

#### **Usage**
```yaml
jobs:
  generate-api-key:
    runs-on: ubuntu-latest

    steps:
      - name: Generate API Key
        uses: mtdtechnology-net/github-actions/.github/actions/api-key@1.0.0
```

---

### 9. **Fastlane Build Number**

#### **Description**
This action increments the build number for an Xcode project starting from a predefined one.

#### **Inputs**
| Input Name   | Description                         | Required | Default |
|--------------|-------------------------------------|----------|---------|
| `project`    | The Xcode project to increment build number for | Yes | N/A     |
| `build_number` | Current build number              | Yes      | N/A     |

#### **Usage**
```yaml
jobs:
  increment-build-number:
    runs-on: ubuntu-latest

    steps:
      - name: Increment Build Number
        uses: mtdtechnology-net/github-actions/.github/actions/fastlane-build-number@1.0.0
        with:
          project: "YourProject.xcodeproj"
          build_number: "100"
```

---

### 10. **Fastlane Build Release**

#### **Description**
This action runs the `build_release` lane for an iOS application using Fastlane.

#### **Inputs**
| Input Name            | Description                                                  | Required | Default |
|-----------------------|--------------------------------------------------------------|----------|---------|
| `scheme`              | Xcode scheme                                                | Yes      | N/A     |
| `keychain_password`   | Build machine keychain password                              | Yes      | N/A     |
| `keychain_path`       | Build machine keychain path                                  | Yes      | N/A     |
| `app_id`              | iOS App Identifier                                          | Yes      | N/A     |
| `provisioning_profile`| iOS App Provisioning profile (starts with `match...`)        | Yes      | N/A     |

#### **Usage**
```yaml
jobs:
  build-release:
    runs-on: macos-latest

    steps:
      - name: Build Release
        uses: mtdtechnology-net/github-actions/.github/actions/fastlane-build-release@1.0.0
        with:
          scheme: "YourScheme"
          keychain_password: "your-keychain-password"
          keychain_path: "/path/to/keychain"
          app_id: "com.example.yourapp"
          provisioning_profile: "match_appstore_com.example.yourapp"
```

---

### 11. **Fastlane Setup**

#### **Description**
This action clones the Fastlane repository and configures `Appfile` and `Matchfile`.

#### **Inputs**
| Input Name             | Description                                                   | Required | Default    |
|------------------------|---------------------------------------------------------------|----------|------------|
| `git_url`              | Git URL for the Fastlane repository                           | Yes      | N/A        |
| `app_identifier`       | iOS App Identifier                                            | Yes      | N/A        |
| `apple_id`             | Apple ID                                                     | Yes      | N/A        |
| `itc_team_id`          | ITC Team ID                                                  | Yes      | N/A        |
| `team_id`              | Team ID                                                      | Yes      | N/A        |
| `certificates_git_url` | Git URL for the Fastlane repository containing certificates   | Yes      | N/A        |
| `storage_mode`         | Usually `git`                                                | Yes      | `git`      |
| `certificate_type`     | Can be `appstore`, `development`, etc.                        | Yes      | `appstore` |
| `readonly`             | Indicates if the certificates are readonly                   | Yes      | `true`     |
| `verbose`              | Enable verbose output                                        | Yes      | `true`     |

#### **Usage**
```yaml
jobs:
  setup-fastlane:
    runs-on: ubuntu-latest

    steps:
      - name: Setup Fastlane
        uses: mtdtechnology-net/github-actions/.github/actions/fastlane-setup@1.0.0
        with:
          git_url: "https://github.com/example/fastlane-repo.git"
          app_identifier: "com.example.app"
          apple_id: "your-apple-id@example.com"
          itc_team_id: "123456789"
          team_id: "987654321"
          certificates_git_url: "https://github.com/example/certificates.git"
          storage_mode: "git"
          certificate_type: "appstore"
          readonly: "true"
          verbose: "true"
```

---

### 12. **Fastlane Sign**

#### **Description**
This action runs `prepare_signing` in Fastlane to install the correct provisioning profile.

#### **Inputs**
| Input Name | Description                                | Required | Default |
|------------|--------------------------------------------|----------|---------|
| `git_url`  | Git URL of the repository containing certificates | Yes      | N/A     |
| `app_id`   | iOS App Identifier                        | Yes      | N/A     |

#### **Usage**
```yaml
jobs:
  prepare-signing:
    runs-on: macos-latest

    steps:
      - name: Prepare Signing
        uses: mtdtechnology-net/github-actions/.github/actions/fastlane-sign@1.0.0
        with:
          git_url: "https://github.com/example/certificates.git"
          app_id: "com.example.app"
```

---

### 13. **Fastlane TestFlight**

#### **Description**
This action uses Fastlane to publish an iOS app to TestFlight with detailed release notes and contact information.

#### **Inputs**
| Input Name           | Description                               | Required | Default |
|----------------------|-------------------------------------------|----------|---------|
| `release_notes`      | Publish release notes                    | Yes      | N/A     |
| `contact_email`      | Contact email for TestFlight             | Yes      | N/A     |
| `contact_first_name` | Contact first name for TestFlight         | Yes      | N/A     |
| `contact_last_name`  | Contact last name for TestFlight          | Yes      | N/A     |
| `contact_phone`      | Contact phone for TestFlight             | Yes      | N/A     |
| `demo_account_name`  | Demo account username for TestFlight      | Yes      | N/A     |
| `demo_account_password` | Demo account password for TestFlight   | Yes      | N/A     |

#### **Usage**
```yaml
jobs:
  publish-testflight:
    runs-on: macos-latest

    steps:
      - name: Publish to TestFlight
        uses: mtdtechnology-net/github-actions/.github/actions/fastlane-testflight@1.0.0
        with:
          release_notes: "This is a release note."
          contact_email: "contact@example.com"
          contact_first_name: "John"
          contact_last_name: "Doe"
          contact_phone: "+1234567890"
          demo_account_name: "testuser"
          demo_account_password: "password123"
```

---

## Getting Started
1. Clone this repository into your project.
2. Reference the actions in your workflows using the examples provided above.
3. Customize the inputs as needed for your specific project requirements.

---

## Contributing
Feel free to submit issues or pull requests to improve these actions. Contributions are always welcome!

---

## License
This repository is licensed under the [MIT License](LICENSE).

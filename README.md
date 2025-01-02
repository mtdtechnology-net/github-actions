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

### **Benefits**
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

## Repository Structure
```
.github/
└── actions/
    ├── ktlint/
    │   └── action.yml
    └── xcode-test/
    │   └── action.yml
    └── xcode-spm-test/
    │   └── action.yml
    └── swiftlint/
    │   └── action.yml
    └── cleanup/
        └── action.yml
README.md
LICENCE.md
```

- `ktlint`: Contains the reusable action for Kotlin code quality checks.
- `xcode-test`: Contains the reusable action for Xcode testing.
- `xcode-spm-test`: Contains the reusable action for Xcode SPM testing.

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

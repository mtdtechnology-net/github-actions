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
        uses: ./.github/actions/ktlint
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
        uses: ./.github/actions/xcode-test
        with:
          scheme: "YourScheme"
          project: "YourProject.xcodeproj"
          device: "iPhone 16"
          os: "18.0"
          testPlan: "YourTestsPlan"
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
        uses: ./.github/actions/run-xcodebuild-tests
        with:
          scheme: "YourScheme"              # The Xcode scheme to build and test
          device: "iPhone 16"        # Device name for the simulator
          os: "16.0"                 # OS version for the simulator
          path: "/PathToYourSPM"                # Path to the Swift Package or Xcode project
```

---

#### How It Works

2. **Navigate to the Target Directory:**
   - The action dynamically navigates to the directory specified in the `path` input.

3. **Run Tests with Xcodebuild:**
   - The `xcodebuild` command is executed with the provided `scheme`, `device`, and `os`.

---

## Repository Structure
```
.github/
└── actions/
    ├── ktlint/
    │   └── action.yml
    └── xcode-test/
        └── action.yml
    └── xcode-spm-test/
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

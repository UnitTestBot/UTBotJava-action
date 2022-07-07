# UTBotJava-action

You want to reduce extra moves and save some time in your working process. We have an action for that!

See: now you can initiate unit tests generation and SARIF report creation right into GitHub with the [UTBotJava](https://github.com/UnitTestBot/UTBotJava) engine.

The action imports the SARIF output into your GitHub repository and creates the Security Code Scanning Alerts section, where you can find all code errors displayed.

Sounds pretty cool, and it really is! 😃


- [How to use](#how-to-use)
- [Inputs](#inputs)
- [Manual trigger](#manual-trigger-configuration)

## How to use

1. Start with applying the [UTBot gradle plugin](https://plugins.gradle.org/plugin/org.utbot.gradle.plugin) to your project.
2. Then, create the workflow with UTBotJava-action. Simple workflow example:

```YAML
name: "Run UTBotJava-action"

on: [push]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2

    - name: Setup Java
      uses: actions/setup-java@v2
      with:
        distribution: adopt
        java-version: 8

    - name: Setup Gradle
      uses: gradle/gradle-build-action@v2
      with:
        gradle-version: 6.8

    - name: Run UTBotJava-action
      uses: UnitTestBot/UTBotJava-action@v1
```
3. Check the Security Code Scanning Alerts to see the detected errors.

## Inputs

Here are the input data parameters for the UTBotJava-action:

| Name | Description | Options | Default value |
| --- | --- | --- | --- |
| `pushTests` | Push generated tests to the repository or not | - | true |
| `targetClasses` | Classes for which the SARIF report will be created | - | all |
| `generatedTestsRelativeRoot` | Relative path (against repository root) to the root of the generated tests. | - | .utbot/test |
| `testFramework` | The name of the test framework to be used | junit4, junit5, testng | junit5 |
| `generationTimeout` | Time budget for generating tests for one class (in milliseconds) | - | 60000 |
| `codegenLanguage` | The language of the generated tests | java, kotlin | java |
| `mockStrategy` | The mock strategy to be used | no-mocks, other-packages, other-classes | other-packages |
| `staticsMocking` | Use static methods mocking or not | do-not-mock-statics, mock-statics | mock-statics |
| `forceStaticMocking` | Forces mocking static methods and constructors for classesToMockAlways classes or not | force, do-not-force. | force |
| `classesToMockAlways` | Classes to force mocking theirs static methods and constructors | - | some internal classes |

__Workflow example:__

📍 __Important note__: in this case, GitHub displays errors in the Pull Request Checks section.

```YAML
name: "Run UTBotJava-action"

on: [pull_request]

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@v2
      with:
        # required if you want to push generated tests in your pull request
        ref: ${{ github.head_ref }}

    - name: Setup Java
      uses: actions/setup-java@v2
      with:
        distribution: adopt
        java-version: 8

    - name: Setup Gradle
      uses: gradle/gradle-build-action@v2
      with:
        gradle-version: 6.8

    - name: Run UTBotJava-action
      uses: UnitTestBot/UTBotJava-action@v1
      with:
        pushTests: 'true'
        targetClasses: '[com.github.username.SomeClass, com.github.username.AnotherClass]'
        generatedTestsRelativeRoot: 'src/test'
        testFramework: 'testng'
        generationTimeout: 50000  # ms
        codegenLanguage: 'kotlin'
        mockStrategy: 'other-classes'
        staticsMocking: 'do-not-mock-statics'
        forceStaticMocking: 'do-not-force'
        classesToMockAlways: '[java.util.Random]'
```

## Manual trigger configuration

Apart from the automatic action, you can also create a workflow with a __Run workflow__ button that will let you launch the action manually. Like this:

![image](https://user-images.githubusercontent.com/54814796/176915768-e89bf101-56e6-4303-8b77-485c7ecb5143.png)

How to do this? Find a step-by-step instruction in the [example repository](https://github.com/UnitTestBot/UTBotJava-action-example). 😉

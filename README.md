# Maestro for Windows

Mobile test automation with Maestro on Windows.

## Installation

1. Follow the [Maestro installation guide](https://maestro.mobile.dev/getting-started/installing-maestro/windows) to install Maestro and its dependencies on your Windows system. This guide will walk you through setting up the necessary tools and configurations to ensure Maestro runs smoothly.

2. Install Android Studio for Windows, which will be used to run the Android emulator. Download Android Studio from [here](https://developer.android.com/studio?gad_source=1&gclid=Cj0KCQiAsOq6BhDuARIsAGQ4-zieA3w5H-1nAwTx-jMfMCnK9K0XJWtS28Bcisvh9KM6qQddkvsD7_UaAsbpEALw_wcB&gclsrc=aw.ds&hl=id). During installation, make sure to add Android to the system environment variables, which will allow you to run Android-related commands from any directory in your terminal.

## Preparation

### 1. Start up your android studio and run the android emulator.

Open Android Studio and start the Android emulator. This emulator simulates a mobile device on your computer, allowing you to run and test mobile applications without needing a physical device. Make sure that the application has been installed on the device.

### 2. Start ADB Server.

Open your Windows Terminal, and run the following command:

```
adb -a -P 5037 nodaemon server
```

This command starts the ADB (Android Debug Bridge) server on your host system, enabling communication between your development machine and the emulator.

### 3. Set ADB Server Socket

Open Ubuntu on your Windows Terminal and run the following command:

```
export ADB_SERVER_SOCKET=tcp:<WINDOWS_IPV4_ADDR>:5037
```

Replace <WINDOWS_IPV4_ADDR> with your host IP address. This sets the ADB server socket to communicate over TCP, allowing your Ubuntu environment to connect to the ADB server running on Windows.

### 4. Verify Device Connection

In your Ubuntu Terminal, run the following command:

```
adb devices
```

This command lists all connected devices. If everything is set up correctly, you should see your emulator listed as a connected device. This confirms that your system is ready to run Maestro tests.

## Running

### Running Individual Test Files

You can run individual test files using the following command:

```
maestro --host <WINDOWS_IPV4_ADDR> test test-file.yaml
```

Replace <WINDOWS_IPV4_ADDR> with your host IP address. This command runs the specified test file (test-file.yaml) on the emulator

### Running All Tests in a Directory

To run all test files in a directory, use the following command:

```
maestro --host <WINDOWS_IPV4_ADDR> test main.yaml
```

This command executes all test files within the /tests directory, ensuring comprehensive test coverage for your application

### Running Test and Generate Report

To run individual or all test files and generate the report, use the following command:

```
maestro --host <WINDOWS_IPV4_ADDR> test main.yaml --format junit --output report
```

## Using Maestro Studio

### Searching for Selectors

Maestro Studio provides a visual interface to help you identify and interact with UI elements. To launch Maestro Studio, use the following command:

```
maestro --host <WINDOWS_IPV4_ADDR> studio
```

Ctrl + click the link provided by Maestro to open Maestro Studio in your browser. Here, you can inspect UI elements, generate selectors, and interact with your application in real-time, making it easier to create and refine your test scripts.

## Assumptions

### Application Setup

1. The app launches successfully without any major setup issues.

2. A clean app instance is availbale for tests, ensuring no residual data (e.g., from previous runs) intereferes with test execution

### Testing Environment

1. Supported Platforms: The tests will run on devices or emulators on both Android and iOS.

2. Device Types: Tests will be executed on a mix of physical devices and emulators to ensure compatibility and consistency.

### Test Data

1. PIN Code:

   - The app accepts numeric PIN codes.
   - The app only accepts 4-digit numbers.

2. Diary Entries:

   - Random text for the "New Diary" form will be used:
     - Title: "Sample Title"
     - Text: "This is a sample diary entry."
   - No validation errors are expected for these inputs.

3. Month Selector:

   - Test data includes entries for December.
   - No entries exist for other months to simplify verification.

4. Edge Cases:

   - Startup PIN Test
     - Invalid PIN attempts are tested up to a reasonable limit (e.g., 3 attempts).
   - New Diary Test:
     - The form should not accept empty fields for the title or text.

5. Tool and Framework Assumptions

   - Maestro CLI:
     - Maestro is installed correctly and operational on the tester’s system.
     - All required dependencies are installed and configured.
   - Screenshots:
     - Maestro’s screenshot functionality works consistently across devices/emulators.
     - Screenshots are stored in the expected directory and are readable for report generation.

6. Reporting:

   - The test report will include outcomes for the tasks defined in the assessment.
   - Tests are executed sequentially, and no parallel execution is assumed.

7. Limitations:
   - Tests focus on the specified tasks and do not cover unmentioned app features.
   - The scope of automation is limited to UI and API behavior as described, without diving into database validations or performance metrics.
   - Additional setup, such as adding testID tags, is not permitted.
   - The tests are done on Windows system, which is using WSL2 as main system.

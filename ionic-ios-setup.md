## iOS

### Introduction

**Guide to Setting Up an iOS Development Environment**

This comprehensive guide provides step-by-step instructions for configuring your macOS system to develop iOS applications. It covers essential prerequisites, software installations, and configuration settings to ensure a smooth development experience.

**Key features:**

- Detailed instructions for installing necessary tools like Xcode, Cocoapods, and Node.js.
- Guidance on configuring your development environment for optimal performance.
- Tips for troubleshooting common issues and resolving potential conflicts.

**By following this guide, you'll be well-equipped to start building your own iOS apps.**

### Prerequisites

- **Operating System:** macOS with Xcode version `15` or above.
- **Project Code:** Your codebase checked out to a local directory.
- **Project Structure:** A folder named `www` should exist within your project.

### Step 1: Fixes for Software Components

- **Activesupport (Xcode >= 15.0.0):**

```sh
gem uninstall --force activesupport -v 7.1.1
gem install activesupport -v 7.0.8
```

- **Webpack:**

```sh
export NODE_OPTIONS="--openssl-legacy-provider --no-experimental-fetch"
```

- **Xcode & CocoaPods:**

```sh
export EXCLUDED_ARCHS=arm64
```

**Explanation:** These commands address compatibility issues with Xcode versions and package dependencies.

- **Disable Detached HEAD Warnings:**

```sh
git config --global advice.detachedHead false
```

**Note:** Disables Git's warning when working on a detached HEAD. This can be useful for experimenting without worrying about warnings, but it's important to be aware of the risks associated with detached HEADs.

### Step 2: Install Global Project Dependencies

Install global project dependencies using `npm`:

```sh
npm install -g yarn
npm install -g native-run
npm install -g ios-deploy # Tool for deploying iOS apps
npm install -g node-gyp
npm install -g @mapbox/node-pre-gyp # For building native dependencies
npm install -g @angular/cli
npm install -g @ionic/cli
npm install -g cordova
```

### Step 3: Install Local Project Dependencies

- **Note:** Remove all Android-related packages from your `package.json` file.

Install project dependencies using yarn:

```sh
yarn install
```

### Step 4: Add iOS Platform to Cordova Project

Use `yarn` to add the iOS platform to your Cordova project:

```sh
# Set the `yarn` in lieu of `npm`
yarn run add-spies

# Remove unnecessary line about $PWD
sed -i -re "s#[$]PWD#$PWD#" /tmp/.spies/npm

export PATH=/tmp/.spies:$PATH

cordova platform add ios@latest
```

### Step 5: Build the iOS App

**Important:** Before moving on to this step, please refer to the Component Versions Check section at the end of this page to ensure that all necessary components are installed and compatible. This will help prevent potential issues during the build process.

Build the iOS app in release mode:

```sh
ionic cordova build ios --prod --release
```

Zip the iOS app/platform folder (Optional):

```sh
zip -r app-ios.zip platforms/ios/
```

### Step 6: Open Xcode and Build Your Project

#### Simplified Instructions:

1. Open Xcode application.
2. Open your project within Xcode.
3. Build your project using Xcode's build functionalities.

**Note:** Building directly from the command line might not be suitable for all workflows. Xcode provides a visual interface for project management and building.

---

### Memo: Check Component Versions and Compatibility

Verify the versions of the following components and dependencies:

- **Cocoapods:**

```sh
pod --version
```

- **Xcode:**

```sh
xcodebuild -version
```

- **JRE (Java Runtime Environment):**

```sh
java -version
```

- **Ruby & Ruby Version Manager:**

```sh
rbenv version    # If using rbenv
ruby -v          # Or direct Ruby version check
```

- **Python & Python Version Manager:**

```sh
python -V
pyenv --version  # If using pyenv
pip --version    # Package manager for Python
```

- **NVM (Node Version Manager):**

```sh
nvm --version
```

- **Node.js:**

```sh
node -v
```

- **`npm` packages:**

```sh
npm list -g --depth 0
```

- **Ionic Info:**

```sh
ionic info
```

- **Cordova CLI:**

```sh
cordova -v
```

- **Cordova Platforms:**

```sh
cordova platform ls
```

- **Cordova Plugins (Cordova CLI):**

```sh
cordova plugin ls
```

- **Cordova Plugins (yarn):**

```sh
yarn list --pattern "cordova|ios|^ionic"
```

- **Cordova Requirements:**

```sh
cordova requirements ios
```

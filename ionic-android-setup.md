## Android

### Introduction

**Guide to Setting Up an Android Development Environment**

This comprehensive guide provides step-by-step instructions for configuring your system to develop Android applications. It covers essential prerequisites, software installations, and configuration settings to ensure a smooth development experience.

**Key features:**

- Detailed instructions for installing necessary tools like Android Studio, SDK, and Java.
- Guidance on configuring your development environment for optimal performance.
- Tips for troubleshooting common issues and resolving potential conflicts.

**By following this guide, you'll be well-equipped to start building your own Android apps.**

### Prerequisites

- **Environment:** Use the pre-built Docker image containing Android development tools (e.g., `android:2023.02.1` image from CircleCI).
- **Project Code:** Your codebase checked out to a local directory.
- **Project Structure:** A folder named `www` should exist within your project.

### Step 1: Install Software Components

#### Install System Software:

```sh
sudo apt-get -y install curl zip unzip
```

#### Upgrade Android SDK:

The `sdkmanager` command helps manage installed Android SDK components. Use it to update the SDK and install necessary tools:

```sh
${ANDROID_HOME}/cmdline-tools/latest/bin/sdkmanager --update
${ANDROID_HOME}/cmdline-tools/latest/bin/sdkmanager "ndk;23.1.7779620" "cmake;3.6.4111459"
```

#### Install Node.js (via NVM):

This guide installs Node.js version 20.x.x using the Node Version Manager (NVM).

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash

export NVM_DIR="$HOME/.nvm"

# Commands to run the NVM
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"

# Install Node.js
nvm install 20
```

Add a fix for Webpack:

```sh
export NODE_OPTIONS="--openssl-legacy-provider --no-experimental-fetch"
```

### Step 2: Install Global Project Dependencies

Install global project dependencies using `npm`:

```sh
npm install -g yarn
npm install -g native-run
npm install -g @angular/cli
npm install -g @ionic/cli
npm install -g cordova
```

### Step 3: Install Local Project Dependencies

**Notes:**
- Remove all iOS-related packages from your `package.json` file.
- If you are using `fsevents` plugin, remove it from your `package.json` file as well.

Install project dependencies using yarn:

```sh
yarn install
```

### Step 4: Add Android Platform to Cordova Project

Use `yarn` to add the Android platform to your Cordova project:

```sh
# Set the `yarn` in lieu of `npm`
yarn run add-spies

# Remove unnecessary line about $PWD
sed -i -re "s#[$]PWD#$PWD#" /tmp/.spies/npm

export PATH=/tmp/.spies:$PATH

cordova platform add android@12
```

### Step 5: Check Component Versions

Verify the versions of the following components:

- **JRE (Java Runtime Environment):**

```sh
java -version
```

- **Android SDK:**

```sh
${ANDROID_HOME}/cmdline-tools/latest/bin/sdkmanager --list_installed
```

- **NVM [Node Version Manager] (Optional):**

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
yarn list --pattern "cordova|android|^ionic"
```

- **Cordova Requirements:**

```sh
cordova requirements android
```

- **Gradle:**

```sh
gradle -v
```

### Step 6: Build the Android App

Build the Android app in release mode:

```sh
ionic cordova build android --prod --release
```

### Step 7: Sign `aab` File Using `bundletool` and Get the `apk` File

#### Download `bundletool`:

```sh
mkdir bundletool && cd bundletool
curl -o bundletool.jar -L https://github.com/google/bundletool/releases/download/1.15.4/bundletool-all-1.15.4.jar && chmod +x bundletool.jar
```

#### Download or create `key.keystore` and `key.properties` files from your desired location:

Substitute the placeholder domain and path with your actual details:

```sh
curl -o key.keystore -L https://domain.com/path/to/your/key.keystore/file
curl -o key.properties -L https://domain.com/path/to/your/key.properties/file
```

#### Put your credentials:

Replace the placeholders with your actual credentials:

```sh
keypass='ENTER HERE YOUR CREDENTIALS'
storepass='ENTER HERE YOUR CREDENTIALS'
alias='ENTER HERE YOUR CREDENTIALS'
```

#### Sign the app:

1. Copy the `app-release.aab` file (backup version of unsigned app):

```sh
cp ../platforms/android/app/build/outputs/bundle/release/app-release.aab app-release-unsigned.aab
```

2. Sign the `aab` file:

```sh
jarsigner -verbose -sigalg SHA256withRSA -digestalg SHA-256 -keystore key.keystore -storepass $storepass -keypass $keypass -signedjar app-release.aab app-release-unsigned.aab $alias
```

3. Create the `apks` file:

```sh
java -jar bundletool.jar build-apks --bundle=app-release.aab --output=app-release.apks --mode=universal --ks-pass=pass:$storepass --key-pass=pass:$keypass --ks key.keystore --ks-key-alias $alias
```

4. Extract the `apk` file:

```sh
unzip -p app-release.apks universal.apk > app-release.apk
```

### Step 8: Installing the App

You can install the application on your phone using the `app-release.apk` file.

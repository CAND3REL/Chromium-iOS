# Chromium-iOS

This repository tracks experiments for compiling Chromium for iOS with the Blink
engine. The upstream project assumes an Apple Silicon macOS environment with the
official Chromium build dependencies installed. The GitHub Actions environment
does not provide the required macOS toolchain, which is why the automated builds
currently fail. Until the pipeline can run on compatible hardware, the best way
to obtain an `.ipa` is to build locally on a macOS machine using the Chromium
build scripts and then archive the resulting Xcode project.

## Current status

* Automated builds: **failing** – missing macOS/iOS SDK and proprietary build
  dependencies.
* Manual builds: only possible on a local macOS machine with the Chromium iOS
  prerequisites installed.

## Local build checklist

1. Follow the [Chromium for iOS build instructions](https://chromium.googlesource.com/chromium/src/+/refs/heads/main/docs/ios_build_instructions.md)
   on a macOS host.
2. Ensure that Xcode and the iOS SDK matching your deployment target are
   installed.
3. Build the `all` target using `autoninja -C out/Release all`.
4. Archive the build in Xcode to produce the `.ipa` for installation on test
   devices.

Once the GitHub-hosted runners expose a macOS environment with the required
dependencies, the CI workflow can be updated to automate the `.ipa` build.

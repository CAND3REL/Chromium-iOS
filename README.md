# Chromium-iOS

This repository does not contain the Chromium for iOS source code that is
required to build an installable `.ipa`. The actual Chromium project uses
thousands of source files, a custom build infrastructure, and macOS-only tools
such as Xcode, the iOS SDK, and the Apple codesigning toolchain. None of those
dependencies are available in this Linux-based environment, so an iOS build
cannot be produced here.

If you need an iOS Chromium build you must:

1. Synchronize the full Chromium source tree using the `fetch ios` command from
   [depot_tools](https://chromium.googlesource.com/chromium/tools/depot_tools/).
2. Use a macOS machine with the latest Xcode, iOS SDK, and a valid Apple
   developer certificate for code signing.
3. Follow the official
   [Chromium iOS build instructions](https://chromium.googlesource.com/chromium/src/+/HEAD/ios/docs/build_instructions.md)
   to generate an `.ipa` bundle.

Without these prerequisites the build will fail, which is why previous GitHub
Action runs could not create an installable artifact.

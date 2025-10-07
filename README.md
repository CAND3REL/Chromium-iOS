# Chromium-iOS

An experimental attempt at producing an installable Chromium build for iOS
using the Blink engine. The upstream Chromium project does not officially
support shipping Blink on iOS (Apple requires WebKit for App Store builds), so
this repository explores what it takes to assemble a proof-of-concept IPA.

## Project status

Automated GitHub builds are currently **expected to fail**. Creating an iOS IPA
requires a macOS environment with Xcode, CocoaPods, and proprietary Apple
tooling such as `xcodebuild` and `codesign`. The standard GitHub Linux runners
do not provide these dependencies, so the workflow terminates before the final
archive is produced.

If you need a usable IPA today, you must build the project locally on macOS or
obtain the manually built artifact referenced in the original announcement.

## Local build outline (macOS)

1. Install the Chromium depot tools:
   ```sh
   git clone https://chromium.googlesource.com/chromium/tools/depot_tools.git
   export PATH="$(pwd)/depot_tools:$PATH"
   ```
2. Fetch the iOS Blink checkout (this can take several hours and requires
   hundreds of gigabytes of free disk space):
   ```sh
   fetch ios
   cd src
   gclient sync
   ```
3. Configure the build with GN. The exact set of arguments depends on the
   experiment, but a minimal configuration might look like:
   ```sh
   gn gen out/ios_release --args='is_debug=false target_os="ios" enable_dsyms=false'
   ```
4. Build Chromium with Ninja:
   ```sh
   autoninja -C out/ios_release chrome
   ```
5. Package the build into an IPA using `xcodebuild` (with a valid signing
   identity and provisioning profile) and the `tools/ios/build/commands` helper
   scripts from the Chromium tree.

These steps mirror the official Chromium instructions but substitute Blink for
WebKit. Expect to iterate on GN arguments and Xcode project adjustments, as the
Blink configuration is not maintained upstream.

## Contributing

Issues and pull requests that document reproducible build steps or automation
fixes are welcome. The goal is to eventually provide a reliable workflow that
can produce an installable IPA without manual intervention.

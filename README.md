# Chromium-iOS

This repository contains the assets that have been used to experiment with
building a Chromium-based browser for iOS that uses the Blink engine instead of
WebKit. The upstream source tree is enormous and notoriously difficult to build
outside of the Google-controlled infrastructure, so the intent of this repo is
to track local experiments and the artifacts that are produced when a build is
successful.

## Current Status

- GitHub Actions currently fails to produce a working `.ipa` because the build
  process cannot download or compile several proprietary dependencies that are
  required by Blink.
- Local builds inside this container may still succeed, but they take several
  hours and require hundreds of gigabytes of disk space, so they are not run as
  part of CI.

## Manual Build Checklist

If you are trying to produce an installable `.ipa` yourself, make sure you have
checked all of the following before starting a build:

1. Ensure you have at least 500 GB of free disk space and 32 GB of RAM.
2. Authenticate with Google Cloud so that the build system can download the
   proprietary binaries listed in `DEPS`.
3. Run `gclient sync` to populate the depot tools workspace.
4. Use `gn gen out/ios` with the appropriate Blink flags enabled and WebKit
   disabled.
5. Build with `autoninja -C out/ios ios_chrome`. This target is responsible for
   producing the signed `.ipa`.

When the build succeeds the resulting `.ipa` can be found in
`out/ios/ios_chrome.ipa`.

## Known Issues

- If the build fails with linker errors referencing Blink symbols, verify that
  you are using the same revision for both Chromium and Blink.
- Signing the `.ipa` requires access to a valid Apple developer certificate.
- The GitHub-hosted macOS runners do not provide enough disk space for the
  intermediate build files, which is why the automated build fails.

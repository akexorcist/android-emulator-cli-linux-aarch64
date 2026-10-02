# android-emulator-cli-linux-aarch64

A build of the Android Emulator CLI (`emulator`, `qemu-system-aarch64`) for linux-aarch64 hosts —
a platform Google doesn't officially publish prebuilt binaries for.

## Usage

`dist/emulator-linux-aarch64.tar.gz` is stored via [Git LFS](https://git-lfs.com) —
install it before cloning, or run `git lfs pull` afterward, otherwise you'll get a
pointer file instead of the real tarball.

Extract it next to an Android SDK install (it needs `platform-tools/` and a system
image alongside it, same as any `emulator` package):

```bash
tar xzf dist/emulator-linux-aarch64.tar.gz -C "$ANDROID_HOME"
# or, with a real path:
tar xzf dist/emulator-linux-aarch64.tar.gz -C /opt/android-sdk-linux
```

gRPC defaults to port `8554` if `-grpc <port>` isn't passed — no need to always
specify it:

```bash
"$ANDROID_HOME/emulator/emulator" -avd <name> ...
# or, with a real path:
/opt/android-sdk-linux/emulator/emulator -avd <name> ...
```

### Verifying a boot

`smoke-test-apk/` has a prebuilt trivial instrumented test app to confirm a real boot
end-to-end:

```bash
adb install -r smoke-test-apk/app-debug.apk
adb install -r smoke-test-apk/app-debug-androidTest.apk
adb shell am instrument -w com.example.emuator.smoketest.test/androidx.test.runner.AndroidJUnitRunner
```

## License

This repository distributes binaries built from AOSP's `external/qemu` (branch
`emu-master-dev`). That project combines two licenses:

- QEMU core: GNU General Public License v2 (see [`LICENSE`](LICENSE))
- Android-specific layers (`android/`, `android-qemu2-glue/`, etc.): Apache
  License 2.0, Copyright The Android Open Source Project

Since the distributed `emulator` and `qemu-system-aarch64` binaries link both
together, the combined work is distributed under the GPL-2.0 terms in
[`LICENSE`](LICENSE).

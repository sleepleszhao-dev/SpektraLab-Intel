# SpektraLab 1.3.4 Intel build

- Source: official `spektralab-v1.3.4` tag, commit `fd891c6bc5eb350387602deb5c1ef9b6dcdab948`
- Architecture: x86_64 only
- Minimum macOS: 15.0
- Built with: Xcode 16.4, macOS SDK 15.5
- Signing: ad hoc, hardened runtime enabled; not notarised
- Original ARM64 DMG: unchanged

## Compatibility changes

- Made the Xcode project architecture configurable and generated an x86_64 project.
- Replaced the Xcode 26 Icon Composer resource with the official app's architecture-neutral `.icns` icon.
- Added small Swift 6.1 type-inference, actor-isolation, and concurrency compatibility fixes.
- Built the Swift UI in Swift 5 language mode because the Xcode 16.4 Swift 6 module generator crashes on this project. C++ remains at `-O2`.
- Adjusted the Metal math probe to use an explicit precise FMA. The original probe is reassociated to zero by the older Intel-host Metal toolchain. This preserves the safety check rather than disabling it.

## Verification

- GPU smoke test on AMD Radeon Pro 570X: 0 failures.
- Confirmed `core=native-metal` and `math mode is safe`.
- Opened a non-private JPEG test image, solved exposure, and completed an Intel Metal reprint.
- Exported and validated 160x160 JPEG and 16-bit TIFF files.
- Mounted the final DMG read-only, verified its x86_64 executable, code signature, and DMG/app byte identity.

The 8 GB test machine is suitable for ordinary images. Very large 100 MP RAW files are not covered by this validation.

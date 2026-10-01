# mlcraw

Convert camera RAW photos — CR2, CR3, NEF, ARW, RAF, ORF, RW2, DNG and
more — to **WebP, JPEG or PNG** with one small program. Plus a Rust
library that decodes RAW files into 16-bit / floating-point RGB with
their metadata.

> **Status: early development.** Nothing to download yet. This README
> describes what is being built.

## Why

- `cwebp`, most image libraries and many tools cannot read camera RAW.
- `dcraw | cwebp` works for older cameras, but dcraw is unmaintained
  (no Canon CR3) and the photo's metadata is lost on the way.
- darktable and RawTherapee convert beautifully, but they are large
  applications for interactive editing.

`mlcraw` aims to be the missing small piece: one file, no installation,
fast batch conversion, metadata kept.

## Planned

**Command line**

```sh
mlcraw photo.CR3                      # → photo.webp
mlcraw *.NEF -o web/ --size 2048      # batch, longest side 2048 px
mlcraw shoot/ --format jpeg -q 90     # whole folder to JPEG
mlcraw photo.ARW --preview            # embedded camera preview, very fast
mlcraw photo.RAF --no-gps             # keep EXIF, drop the location
```

- Camera white balance and colour, sRGB output, sensible defaults.
- EXIF kept (camera, lens, exposure, ISO, capture time); GPS optional.
- Linux, macOS and Windows; a single binary each.

**Library**

```rust
let raw = mlcraw::open("photo.CR3")?;
let img = raw.develop(&mlcraw::Develop::default())?; // RGB, 16 bit linear or f32
let meta = raw.metadata();                            // camera, lens, exposure, time …
```

The library is meant to be useful on its own — for example as the RAW
reader of an image pipeline that keeps working in high precision
instead of starting from an 8-bit export.

## Built on LibRaw

RAW decoding is done by [LibRaw](https://www.libraw.org/), which is
dual-licensed under LGPL-2.1 or CDDL-1.0. `mlcraw` uses LibRaw
**unmodified under the CDDL-1.0**. See [THIRD_PARTY.md](THIRD_PARTY.md).

## License

The code of this project is licensed under the [MIT License](LICENSE).
Third-party components keep their own licenses (see
[THIRD_PARTY.md](THIRD_PARTY.md)).

---

Made by Michael Lechner — [mlcgo.eu](https://mlcgo.eu). `mlcraw` is
also the RAW reader of [mlc OpticScript](https://mlcgo.eu/products/mlc-opticscript/).

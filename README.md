# Project template for Picoboy Color

This template is intended as a starting point for developing your own firmware based on the Picoboy Color. It's based on [
rp2040-project-template](https://github.com/rp-rs/rp2040-project-template).

It includes all of the `knurling-rs` tooling as showcased in https://github.com/knurling-rs/app-template (`defmt`, `defmt-rtt`, `panic-probe`, `flip-link`) to make development as easy as possible.

`elf2uf2-rs` is configured as the default runner, so you can start your program as easy as

```sh
cargo run --release
```

## Requirements
  
* The standard Rust tooling (cargo, rustup) which you can install from https://rustup.rs/
* Toolchain support for the cortex-m0+ processors in the rp2040 (thumbv6m-none-eabi)
* flip-link - this allows you to detect stack-overflows on the first core, which is the only supported target for now.

## Installation of development dependencies

```sh
rustup target install thumbv6m-none-eabi

cargo install flip-link

# Installs the elf2uf2-rs runner
cargo install --locked elf2uf2-rs
```

## Use cargo generate

You can create a new project with cargo-generate. First install it:

```sh
cargo install cargo-generate
```

Then you can create a new project from the template:

```sh
cargo generate --git https://github.com/seeseekey/picoboy-color-project-template.git
```

## Running
  
For a debug build
```sh
cargo run
```
For a release build
```sh
cargo run --release
```

If you do not specify a DEFMT_LOG level, it will be set to `debug`.
That means `println!("")`, `info!("")` and `debug!("")` statements will be printed.
If you wish to override this, you can change it in `.cargo/config.toml` 
```toml
[env]
DEFMT_LOG = "off"
```
You can also set this inline (on Linux/MacOS)  
```sh
DEFMT_LOG=trace cargo run
```

or set the _environment variable_ so that it applies to every `cargo run` call that follows:
#### Linux/MacOS/unix
```sh
export DEFMT_LOG=trace
```

Setting the DEFMT_LOG level for the current session  
for bash
```sh
export DEFMT_LOG=trace
```

#### Windows
Windows users can only override DEFMT_LOG through `config.toml`
or by setting the environment variable as a separate step before calling `cargo run`
- cmd
```cmd
set DEFMT_LOG=trace
```
- powershell
```ps1
$Env:DEFMT_LOG = trace
```

```cmd
cargo run
```

## Notes on using rp2040_hal and rp2040_boot2

  The second-stage boot loader must be written to the .boot2 section. That
  is usually handled by the board support package (e.g.`rp-pico`). If you don't use
  one, you should initialize the boot loader manually. This can be done by adding the
  following to the beginning of main.rs:
  ```rust
  use rp2040_boot2;
  #[link_section = ".boot2"]
  #[used]
  pub static BOOT_LOADER: [u8; 256] = rp2040_boot2::BOOT_LOADER_W25Q080;
  ```

## Feature flags

  There are several [feature flags in rp2040-hal](https://docs.rs/rp2040-hal/latest/rp2040_hal/#crate-features).
  If you want to enable some of them, uncomment the `rp2040-hal` dependency in `Cargo.toml` and add the
  desired feature flags there. For example, to enable ROM functions for f64 math using the feature `rom-v2-intrinsics`:
  ```
  rp2040-hal = { version="0.10", features=["rt", "critical-section-impl", "rom-v2-intrinsics"] }
  ```

## Contributing

Contributions are what make the open source community such an amazing place to be learn, inspire, and create. Any contributions you make are **greatly appreciated**.

The steps are:

1. Fork the Project by clicking the 'Fork' button at the top of the page.
2. Create your Feature Branch (`git checkout -b feature/AmazingFeature`)
3. Make some changes to the code or documentation.
4. Commit your Changes (`git commit -m 'Add some AmazingFeature'`)
5. Push to the Feature Branch (`git push origin feature/AmazingFeature`)
6. Create a [New Pull Request](https://github.com/seeseekey/picoboy-color-project-template/pulls)
7. An admin will review the Pull Request and discuss any changes that may be required.
8. Once everyone is happy, the Pull Request can be merged by an admin, and your work is part of our project!

## License

The contents of this repository are dual-licensed under the _MIT OR Apache
2.0_ License. That means you can chose either the MIT licence or the
Apache-2.0 licence when you re-use this code. See `MIT` or `APACHE2.0` for more
information on each specific licence.

Any submissions to this project (e.g. as Pull Requests) must be made available
under these terms.

## Contact

Raise an issue: [https://github.com/seeseekey/picoboy-color-project-template/issues](https://github.com/seeseekey/picoboy-project-template/issues)

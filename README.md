# Bug demonstrator

## Expected behavior

    cargo test
    [...]
    3 steps (3 passed)

## IntelliJ behavior

1. Run/Debug configurations > Cargo > Command "test"
2. Launch configuration :

> Note the extra options `--color=always --no-fail-fast -- --format=json -Z unstable-options --show-output`

```bash
/Users/cgeek/.cargo/bin/cargo test --color=always --no-fail-fast -- --format=json -Z unstable-options --show-output
[...]
error: unexpected argument '--format' found
```

## Explanation

IntelliJ Rust plugin adds `--color=always --no-fail-fast -- --format=json -Z unstable-options --show-output` extra
options, which are not handled by cucumber parser.

## Solutions

### 1. Handle the specific IntelliJ problem in the tests

```rust
struct CustomOpts {
    /// For compliance with Jetbrains IDE which pushes extra args.
    #[clap(short, long)]
    format: Option<String>,
    #[clap(short, long = "show-output")]
    show_output: bool,
    #[clap(short = 'Z', long)]
    z: Option<String>,
    /// [...]
}
```

### 2. Jetbrains allows to disable the extra options

In the Run/Debug configuration, allow to disable the `--color=always --no-fail-fast -- --format=json -Z unstable-options --show-output` extra options, so these
do not appear in the call.

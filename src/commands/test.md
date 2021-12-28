# wasm-pack test

`wasm-pack test`命令是对[wasm-bindgen-test-runner](https://rustwasm.github.io/wasm-bindgen/wasm-bindgen-test/index.html) CLI 的封装。你可以使用他在不同的浏览器中来测试你的 wasm 模块，且不需要你在即安装各种 WebDriver。

```
wasm-pack test --help
```

## Path

`wasm-pack test` 可以接收一个参数来指定要测试的 `crate` 文件夹，默认是当前目录。指定的文件夹需要包含 `Cargo.toml`

```
# 为当前文件夹下的 crate 运行测试
wasm-pack test

# 为指定目录下的 crate 运行测试
wasm-pack test crates/crate-in-my-workspace
```

## Profile

`test`命令可以指定使用 `--release` 构建，如果没指定的话会生成一个 `--debug` 的构建。

## 测试环境

通过指定测试环境标志来组合运行测试的环境。

对于持续集成来说，可以使用 `--headless` 来运行一个无头浏览器(headless browser)。

```
wasm-pack test --node --firefox --chrome --safari --headless
```

## 额外参数

和`build`命令一样，`test` 也给 `cargo test` 传递额外的参数，只需要在命令的后面加上需要的参数就行。

使用 `cargo test -h` 查看 `cargo test` 的额外参数。

## 运行一部分测试

当调试特定问题的时候，可以运行一部分测试，而不是整个测试代码。

下面是一个具体的例子，用来说明如何只允许运行一部分测试。

```
$ tree crates/foo
├── Cargo.toml
├── README.md
├── src
│   ├── diff
│   │   ├── diff_test_case.rs
│   │   └── mod.rs
│   ├── lib.rs
└── tests
    ├── diff_patch.rs
    └── node.rs
```

```
# 在 FireFox 中运行 tests/diff_patch.rs 文件的所有测试
wasm-pack test crates/foo --firefox --headless --test diff_patch

# 运行tests/diff_patch.rs文件中，名字包含 "replace" 的测试
wasm-pack test crates/foo --firefox --headless --test diff_patch replace

# 运行 src/lib/diff.rs 文件中，`tests` 模块的测试
wasm-pack test crates/foo --firefox --headless --lib diff::tests

# 和上面一样，但是只运行包含 "replace" 的测试
wasm-pack test crates/foo --firefox --headless --lib diff::tests::replace
```

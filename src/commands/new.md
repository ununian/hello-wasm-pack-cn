# wasm-pack new

`wasm-pack new` 命令将会为你创建一个新的 RustWasm 项目，内部是使用 [`cargo-generate`] 实现的。

`new` 命令包含 3 个参数,name、template 和 mode。

```sh
wasm-pack new <name> --template <template> --mode <normal|noinstall|force>
```

默认的模板是 [`rustwasm/wasm-pack-template`](https://github.com/rustwasm/wasm-pack-template).

## Name

`wasm-pack new` 必须指定一个项目名称:

```sh
wasm-pack new myproject
```

## Template

`wasm-pack new` 可以用 `--template` 参数指定一个模板:

```sh
wasm-pack new myproject --template https://github.com/rustwasm/wasm-pack-template
```

模板需要是一个实现了 [`cargo-generate`] 的 git 仓库地址。

[`cargo-generate`]: https://github.com/ashleygwilliams/cargo-generate

## Mode

`wasm-pack new` mode 参数用来控制 `wasm-pack` 依赖工具的安装模式:

```sh
wasm-pack new myproject --mode noinstall
```

可选值为 `normal`, `noinstall` 和 `force`，默认情况下是 `normal`

`noinstall` 意味着 `wasm-pack` 不会安装依赖工具。
如果在使用 `wasm-pack` 时找不到所需的工具，就会报错。

`force` 意味着 `wasm-pack` 不检查本地的 Rust 版本。
如果本地的 Rust 版本不符合要求，在运行 `wasm-pack` 命令时将报错。

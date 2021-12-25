# Quickstart

1. 使用 [`rustup`] 安装 `rust`
1. 安装 [`wasm-pack`]
1. 运行 `wasm-pack new hello-wasm`
1. `cd hello-wasm`
1. 运行 `wasm-pack build`
1. 这将在 `pkg` 文件夹下生成打包的文件，包括 wasm、js bridge、ts defines 和 package.json
1. 如果要上传到 npm 仓库，可以运行 `wasm-pack publish`。如果需要登录的话，可以使用 `wasm-pack login` 来登录 npm 仓库

[`rustup`]: https://rustup.rs/
[`wasm-pack`]: https://rustwasm.github.io/wasm-pack/installer/

# 运行代码

本部分教程将会指导你如使用 [webpack] 打包 JavaScript 和 WebAssembly ，并在浏览器里面使用。

[webpack]: https://webpack.js.org/

## 项目结构

- `.gitignore`: git 提交时忽略 `node_modules`
- `README.md`:就是本文
- `index.html`: 引用 `src/index.js` 的 html 文件
- `js/index.js`: 演示了如何引入和使用 wasm 文件
- `package.json` and `package-lock.json`:
  - 包含了下面这些依赖:
    - [`webpack`](https://www.npmjs.com/package/webpack)
    - [`webpack-cli`](https://www.npmjs.com/package/webpack-cli)
    - [`webpack-dev-server`](https://www.npmjs.com/package/webpack-dev-server)
- `webpack.config.js`: webpack 配置
- `crate/src/lib.rs`: Rust 代码

## Your Rust Crate

本项目包含了一个示例 crate。

在 `crate/src/lib.rs` 中，我们定义了一个 `main_js` 函数，并通过指定。

```rust
// Called by our JS entry point to run the example.
#[wasm_bindgen]
pub fn run() -> Result<(), JsValue> {
    set_panic_hook();

    // ...
    let p: web_sys::Node = document.create_element("p")?.into();
    p.set_text_content(Some("Hello from Rust, WebAssembly, and Webpack!"));
    // ...

    Ok(())
}
```

Now, open up the `js/index.js` file. We see our Rust-generated wasm `run` function being
called inside our JS file.

```js
import('../crate/pkg').then((module) => {
  module.run();
});
```

## Run The Project

To generate our Rust-compiled to wasm code, in the root directory we run:

```bash
npm run build
```

This will create our bundled JavaScript module in a new directory `dist`.

We should be ready to run our project now!
In the root directory, we'll run:

```bash
npm start
```

Then in a web browser navigate to `http://localhost:8080` and you should be greeted
with text in the body of the page that says "Hello from Rust, WebAssembly, and Webpack!"

If you did congrats! You've successfully used the rust-webpack template!

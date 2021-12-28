# wasm-pack build

`wasm-pack build` 命令将会生成在 JavaScript 中使用 wasm 的胶水文件，以及推送到 npm 仓库所需的文件。

`wasm-pack build` 命令将会在 pkg 文件夹下生成，编译后的 wasm 二进制文件，JavaScript 胶水文件，以及推送到 npm 仓库所需的 package.json 和 README.md

`pkg` 文件夹默认会包含在 `.gitignore` 中，不会被 git 工具跟踪。

## Path

`path` 可以指定命令运行的文件夹（需要包含 Cargo.toml），如果没有指定的话，会在当前文件夹下运行命令:

```sh
wasm-pack build examples/js-hello-world
```

## 输出文件夹

默认情况下，`wasm-pack` 会在当前目录下生成一个名为 `pkg` 的文件夹，你也可以使用 `--out-dir` 参数指定输出文件夹，例如:

By default, `wasm-pack` will generate a directory for it's build output called `pkg`.
If you'd like to customize this you can use the `--out-dir` flag.

```sh
wasm-pack build --out-dir out
```

## 文件名前缀

`--out-name` 可以指定输出文件的前缀，如果没有指定，将会使用当前项目名作为前缀。

如果我们的 carte 名字时 `dom` 的话：

```sh
wasm-pack build
# will produce files
# dom.d.ts  dom.js  dom_bg.d.ts  dom_bg.wasm  package.json  README.md

wasm-pack build --out-name index
# will produce files
# index.d.ts  index.js  index_bg.d.ts  index_bg.wasm  package.json  README.md
```

## Profile

`build`命令还能修改编译模式，用来控制是否启用调试断言，是否生成调试信息，是否启用优化。下面是各个配置的区别：

| 配置          | 调试断言 | 调试信息 | 编译优化 | 备注                                 |
| ------------- | -------- | -------- | -------- | ------------------------------------ |
| `--dev`       | Yes      | Yes      | No       | 用于开发和调试                       |
| `--profiling` | No       | Yes      | Yes      | 用于分析性能问题                     |
| `--release`   | No       | No       | Yes      | 一般用于发布到生成环境，这个时默认值 |

`--dev`配置相当于使用 cargo 的 [default
non-release profile][cargo-profile-sections-documentation]，编译的速度很快，但是代码基本上不会被优化，并且会启用调试断言（ debug assertions ）和运行时检测（runtime correctness checks）。`--profiling`和`--release`配置相当于使用 cargo 的 release 配置，`--profiling`也能实现调试，可以帮助我们排查性能问题。

[cargo-profile-sections-documentation]: https://doc.rust-lang.org/cargo/reference/manifest.html#the-profile-sections

## Target

`--target` 参数用控制生成的胶水 JS 文件如何加载和实例化`WebAssembly`。
关于生成各种策列详见下表和[documentation on using the compiled output][deploy]。

```
wasm-pack build --target nodejs
```

| 可选值              | 目标平台                        | 描述                                                                                                                                                                            |
| ------------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| 未指定或者`bundler` | [Bundler][bundlers]             | 生成 ES Module 格式的 JS 文件。主要适用于 webpack、rollup 等打包器。使用的时候可以使用 `import` 关键字直接引入。会在 `package.json` 生成 `module` 字段和 `sideEffects: false`。 |
| `nodejs`            | [Node.js][deploy-nodejs]        | 生成 CommonJS 格式的 JS 文件，主要用于 nodejs。使用 `require`关键字引入。会在 `package.json` 中生成 `main` 字段。                                                               |
| `web`               | [Native in browser][deploy-web] | 生成可以直接在浏览器中使用的 ESModule JS 文件，但是 WebAssembly 必须手动加载和实例化。                                                                                          |
| `no-modules`        | [Native in browser][deploy-web] | 和`web`一样可以直接在浏览器中使用，但是直接注入在全局变量中，不支持`wasm-bindgen`中的很多功能。                                                                                 |

[deploy]: https://rustwasm.github.io/docs/wasm-bindgen/reference/deployment.html
[bundlers]: https://rustwasm.github.io/docs/wasm-bindgen/reference/deployment.html#bundlers
[deploy-nodejs]: https://rustwasm.github.io/docs/wasm-bindgen/reference/deployment.html#nodejs
[deploy-web]: https://rustwasm.github.io/docs/wasm-bindgen/reference/deployment.html#without-a-bundler

## Scope

主要用于发布包含 scope 的 npm 包。

例如下面的命令会让得到`package.json`的`name`字段为`@test/js-hello-world`，在 npm 的文档中了解[更多][npm-scope-documentation]。

```
wasm-pack build examples/js-hello-world --scope test
```

[npm-scope-documentation]: https://docs.npmjs.com/misc/scope

## Mode

`--model`用来控制是否安装`wasm-bindgen`。

```
wasm-pack build examples/js-hello-world --mode no-install
```

| Option       | Description                               |
| ------------ | ----------------------------------------- |
| `no-install` | `wasm-pack init` 不会安装 `wasm-bindgen`. |
| `normal`     | 会安装 `wasm-bindgen`.                    |

## 额外参数

`build` 命令不支持除了上面外的其他参数。如果你想给 `cargo build` 传递参数，可以在命令最后附加上需要传递的参数。

例如要使用 `cargo` 的离线功能，可以这样写：

```
wasm-pack build examples/js-hello-world --mode no-install -- --offline
```

<hr style="font-size: 1.5em; margin-top: 2.5em"/>

<sup id="footnote-0">0</sup> 目前还不支持在 pkg 文件夹或者 npm 包中包含其他额外的文件，我们会尽快推出解决方案。 [↩](#wasm-pack-build)

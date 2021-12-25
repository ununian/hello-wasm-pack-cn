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

## 生成配置

The `build` command accepts an optional profile argument: one of `--dev`,
`--profiling`, or `--release`. If none is supplied, then `--release` is used.

This controls whether debug assertions are enabled, debug info is generated, and
which (if any) optimizations are enabled.

| Profile       | Debug Assertions | Debug Info | Optimizations | Notes                                                       |
| ------------- | ---------------- | ---------- | ------------- | ----------------------------------------------------------- |
| `--dev`       | Yes              | Yes        | No            | Useful for development and debugging.                       |
| `--profiling` | No               | Yes        | Yes           | Useful when profiling and investigating performance issues. |
| `--release`   | No               | No         | Yes           | Useful for shipping to production.                          |

The `--dev` profile will build the output package using cargo's [default
non-release profile][cargo-profile-sections-documentation]. Building this way is
faster but applies few optimizations to the output, and enables debug assertions
and other runtime correctness checks. The `--profiling` and `--release` profiles
use cargo's release profile, but the former enables debug info as well, which
helps when investigating performance issues in a profiler.

The exact meaning of the profile flags may evolve as the platform matures.

[cargo-profile-sections-documentation]: https://doc.rust-lang.org/cargo/reference/manifest.html#the-profile-sections

## Target

The `build` command accepts a `--target` argument. This will customize the JS
that is emitted and how the WebAssembly files are instantiated and loaded. For
more documentation on the various strategies here, see the [documentation on
using the compiled output][deploy].

```
wasm-pack build --target nodejs
```

| Option                       | Usage                           | Description                                                                                                                                                                                 |
| ---------------------------- | ------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| _not specified_ or `bundler` | [Bundler][bundlers]             | Outputs JS that is suitable for interoperation with a Bundler like Webpack. You'll `import` the JS and the `module` key is specified in `package.json`. `sideEffects: false` is by default. |
| `nodejs`                     | [Node.js][deploy-nodejs]        | Outputs JS that uses CommonJS modules, for use with a `require` statement. `main` key in `package.json`.                                                                                    |
| `web`                        | [Native in browser][deploy-web] | Outputs JS that can be natively imported as an ES module in a browser, but the WebAssembly must be manually instantiated and loaded.                                                        |
| `no-modules`                 | [Native in browser][deploy-web] | Same as `web`, except the JS is included on a page and modifies global state, and doesn't support as many `wasm-bindgen` features as `web`                                                  |

[deploy]: https://rustwasm.github.io/docs/wasm-bindgen/reference/deployment.html
[bundlers]: https://rustwasm.github.io/docs/wasm-bindgen/reference/deployment.html#bundlers
[deploy-nodejs]: https://rustwasm.github.io/docs/wasm-bindgen/reference/deployment.html#nodejs
[deploy-web]: https://rustwasm.github.io/docs/wasm-bindgen/reference/deployment.html#without-a-bundler

## Scope

The init command also accepts an optional `--scope` argument. This will scope
your package name, which is useful if your package name might conflict with
something in the public registry. For example:

```
wasm-pack build examples/js-hello-world --scope test
```

This command would create a `package.json` file for a package called
`@test/js-hello-world`. For more information about scoping, you can refer to
the npm documentation [here][npm-scope-documentation].

[npm-scope-documentation]: https://docs.npmjs.com/misc/scope

## Mode

The `build` command accepts an optional `--mode` argument.

```
wasm-pack build examples/js-hello-world --mode no-install
```

| Option       | Description                                                                            |
| ------------ | -------------------------------------------------------------------------------------- |
| `no-install` | `wasm-pack init` implicitly and create wasm binding without installing `wasm-bindgen`. |
| `normal`     | do all the stuffs of `no-install` with installed `wasm-bindgen`.                       |

## Extra options

The `build` command can pass extra options straight to `cargo build` even if
they are not supported in wasm-pack. To use them simply add the extra arguments
at the very end of your command, just as you would for `cargo build`. For
example, to build the previous example using cargo's offline feature:

```
wasm-pack build examples/js-hello-world --mode no-install -- --offline
```

<hr style="font-size: 1.5em; margin-top: 2.5em"/>

<sup id="footnote-0">0</sup> If you need to include additional assets in the pkg
directory and your NPM package, we intend to have a solution for your use case
soon. [↩](#wasm-pack-build)

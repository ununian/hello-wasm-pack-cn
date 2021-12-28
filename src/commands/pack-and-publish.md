# pack 和 publish

这 2 个命令是 npm 同名命令的替代品，主要是在不安装 nodejs 的情况下使用打包 npm 包和发布 npm 包。

`pack`命令用来从 pkg 文件夹中创建一个 npm 包，就是一个 zip 文件。

`publish`命令用来将 pkg 文件推送到 npm 仓库。

可以在下面 2 个 npm 文档中获取到命令的更多信息。

- [`npm pack`](https://docs.npmjs.com/cli/pack)
- [`npm publish`](https://docs.npmjs.com/cli/publish)

通常情况下这 2 个命令都会使用 pkg 文件夹，当然你也可以指定其他文件夹（需要是 pkg 文件夹或者 pkg 文件夹的父文件夹）。

```
$ wasm-pack pack myproject/pkg
| 🎒  packed up your package!
$ wasm-pack pack myproject
| 🎒  packed up your package!
```

如果你指定了一个不符合要求的路径，会得到报错：

```
$ wasm-pack pack myproject/src/
Unable to find the pkg directory at path 'myproject/src/', or in a child directory of 'myproject/src/'
```

如果你不指定路径，就会使用当前文件夹。

## 推送一个包含 Tag 的包

使用 `--tag` 参数可以指定一个包的 tag，例如：

```
wasm-pack publish --tag next
```

默认情况下是使用 `latest` tag。在 npm 的文档上了解更多关于 [Tag 的信息](https://docs.npmjs.com/cli/dist-tag)。

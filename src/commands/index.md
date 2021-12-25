# 命令

在使用 `wasm-pack` 的过程中，下面几个命令可能会用到：

- `new`: 使用内置模板创建一个新项目 [了解更多][new]
- `build`: 在 `pkg` 目录下生成编译后的 wasm 文件和 胶水 js 文件 [了解更多][build]
- `pack` 和 `publish`: 在 `pkg` 目录下生成 npm 包，并能和 `npm` 命令一样推送到 npm 仓库 [了解更多][pack-pub]

[new]: ./new.html
[build]: ./build.html
[pack-pub]: ./pack-and-publish.html

## 已经弃用的命令

- `init`: 该命令已经被 `build` 命令所取代

## 日志级别

默认情况下，`wasm-pack` 会输出很多有用的消息。

你可以使用 `--verbose` 参数来输出*更多*的消息，或者用 `--quiet` 屏蔽*所有*输出。

你也可以使用 `--log-level` 参数来设置日志级别，可以使用以下值：

- `--log-level info` 默认值, 会输出所有的消息.
- `--log-level warn` 输出警告和错误，单不会输出 info 级别的消息.
- `--log-level error` 只会输出错误消息.

日志级别的标志位是全局标志位，所以他们需要在具体命令*之前*使用:

```sh
wasm-pack --log-level error build
wasm-pack --quiet build
wasm-pack --verbose build
```

---
name: cargo install
metadata:
  references:
    - https://doc.rust-lang.net.cn/stable/cargo/commands/cargo-install.html
description: 介绍 cargo install 命令的使用方法和场景。
author: stevessr
---

# Cargo Install

`cargo install` 是 Cargo 的一个命令，用于安装 Rust 的二进制工具。它会从 crates.io 或指定的源安装一个 Rust 包，并将其二进制文件放在 Cargo 的安装目录中，通常是 `~/.cargo/bin`。

## 概要

`cargo install` [选项] crate[@version]…
`cargo install` [选项] `--path` 路径
`cargo install` [选项] `--git` URL [crate…]
`cargo install` [选项] `--list`

## 描述

此命令管理 Cargo 本地已安装的二进制 crate 集合。只有具有可执行 [[bin]] 或 [[example]] 目标的包才能安装，并且所有可执行文件都安装到安装根目录的 bin 文件夹中。默认情况下，只安装二进制文件，不安装示例。

安装根目录按优先顺序确定如下：

- `--root` 选项
- `CARGO_INSTALL_ROOT` 环境变量
- `install.root` Cargo 配置值
- `CARGO_HOME` 环境变量
- `$HOME/.cargo`

crate 可以从多个来源安装。默认的来源是 crates.io，但 --git、--path 和 --registry 标志可以更改此来源。如果来源包含多个包（例如 crates.io 或包含多个 crate 的 Git 仓库），则需要 crate 参数来指明要安装哪个 crate。

来自 crates.io 的 crate 可以通过 --version 标志选择指定希望安装的版本，类似地，来自 Git 仓库的包可以选择指定应安装的分支、标签或修订版本。如果一个 crate 有多个二进制文件，可以使用 --bin 参数选择性地只安装其中一个；如果您更希望安装示例，也可以使用 --example 参数。

如果包已经安装，并且已安装的版本看起来不是最新的，Cargo 将会重新安装它。如果以下任何值发生变化，Cargo 将重新安装该包：

- 包的版本和来源。
- 已安装的二进制文件名称集合。
- 选择的特性（features）。
- 配置文件 (`--profile`)。
- 目标 (`--target`)。

使用 `--path` 进行安装总是会构建和安装，除非存在来自其他包的冲突二进制文件。可以使用 `--force` 标志强制 Cargo 总是重新安装该包。

如果来源是 crates.io 或 --git，则默认情况下 crate 会在一个临时的 target 目录中构建。为了避免这种情况，可以通过将 `CARGO_TARGET_DIR` 环境变量设置为相对路径来指定 target 目录。特别地，这对于在持续集成系统中缓存构建产物非常有用。

## 处理 Lockfile

此命令在系统或用户级别操作，而非项目级别。这意味着本地的[配置发现](https://doc.rust-lang.net.cn/stable/cargo/reference/config.html#hierarchical-structure)将被忽略。相反，配置发现将从 `$CARGO_HOME/config.toml` 开始。如果包使用 `--path $PATH` 安装，则将使用本地配置，发现将从 `$PATH/.cargo/config.toml` 开始。

## 选项

### 安装选项

--vers 版本
--version 版本
指定要安装的版本。这可以是一个版本要求，例如 ~1.2，以便 Cargo 从给定的要求中选择最新版本。如果版本不包含要求操作符（例如 ^ 或 ~），则它必须采用 主版本。次版本。补丁版本 的形式，并将精确安装该版本；它不像 Cargo 依赖项那样被视为插入符要求。
--git URL
指定要从中安装 crate 的 Git URL。
--branch 分支
从 git 安装时使用的分支。
--tag 标签
从 git 安装时使用的标签。
--rev sha
从 git 安装时使用的特定提交。
--path 路径
要从中安装的本地 crate 的文件系统路径。
--list
列出所有已安装的包及其版本。
-n
--dry-run
（不稳定）执行所有检查而不安装。
-f
--force
强制覆盖现有的 crate 或二进制文件。如果一个包安装的二进制文件与另一个包同名，可以使用此选项。如果您想用系统中发生变化的某些东西（例如新版本的 rustc）进行重建，此选项也很有用。
--no-track
默认情况下，Cargo 通过存储在安装根目录下的元数据文件来跟踪已安装的包。此标志告诉 Cargo 不要使用或创建该文件。使用此标志，除非使用 --force 标志，否则 Cargo 将拒绝覆盖任何现有文件。这还会禁用 Cargo 防止多个并发调用同时安装的能力。
--bin 名称…
仅安装指定的二进制文件。
--bins
安装所有二进制文件。这是默认行为。
--example 名称…
仅安装指定的示例。
--examples
安装所有示例。
--root 目录
安装包的目标目录。
--registry 注册表
要使用的注册表名称。注册表名称在 Cargo 配置文件中定义。如果未指定，则使用默认注册表，该注册表由 registry.default 配置键定义，默认为 crates-io。
--index 索引
要使用的注册表索引的 URL。

### 特性选择

特性标志允许您控制启用哪些特性。如果未提供特性选项，则为每个选定的包激活 default 特性。

有关更多详细信息，请参阅[特性文档](https://doc.rust-lang.net.cn/stable/cargo/reference/features.html#command-line-feature-options)。

-F 特性
--features 特性
要激活的特性列表，用空格或逗号分隔。工作空间成员的特性可以使用 package-name/feature-name 语法启用。此标志可以多次指定，这将启用所有指定的特性。
--all-features
激活所有选定包的所有可用特性。
--no-default-features
不要激活选定包的 default 特性。

### 编译选项

--target triple
针对给定架构进行安装。默认为主机架构。triple 的通用格式是 <arch><sub>-<vendor>-<sys>-<abi>。运行 rustc --print target-list 查看支持的目标列表。
此项也可以通过 build.target 配置值指定。

请注意，指定此标志会使 Cargo 在不同的模式下运行，目标产物会被放置在一个单独的目录中。有关更多详细信息，请参阅构建缓存文档。

--target-dir 目录
用于存放所有生成的产物和中间文件的目录。也可以通过 CARGO_TARGET_DIR 环境变量或 build.target-dir 配置值指定。默认为平台临时目录中的一个新临时文件夹。
使用 --path 时，默认情况下将使用本地 crate 工作空间中的 target 目录，除非指定了 --target-dir。

--debug
使用 dev 配置文件而不是 release 配置文件进行构建。有关按名称选择特定配置文件，另请参阅 --profile 选项。
--profile 名称
使用给定的配置文件进行安装。有关配置文件的更多详细信息，请参阅参考。
--timings=格式
输出每次编译所需时间的详细信息，并随时间跟踪并发信息。接受可选的逗号分隔的输出格式列表；不带参数的 --timings 将默认为 --timings=html。指定输出格式（而非默认格式）是不稳定的，需要 -Zunstable-options 标志。有效的输出格式包括：
html（不稳定，需要 -Zunstable-options）：在 target/cargo-timings 目录中写入一个人类可读的文件 cargo-timing.html，其中包含编译报告。如果您想查看较早的运行结果，也会在同一目录中写入一个文件名中带有时间戳的报告。HTML 输出仅适合人类阅读，不提供机器可读的计时数据。
json（不稳定，需要 -Zunstable-options）：以机器可读的 JSON 格式输出计时信息。

### 清单选项

--ignore-rust-version
忽略包中的 rust-version 指定。
--locked
断言使用的依赖项及其版本与最初生成现有 Cargo.lock 文件时完全相同。在以下任一情况发生时，Cargo 将会报错退出：
lock 文件丢失。
Cargo 由于不同的依赖项解析尝试更改了 lock 文件。
此选项可用于需要确定性构建的环境中，例如持续集成（CI）流水线。

--offline
阻止 Cargo 以任何理由访问网络。如果不带此标志，如果 Cargo 需要访问网络而网络不可用时，它将报错停止。带上此标志，Cargo 将尝试在可能的情况下不通过网络继续。
请注意，这可能导致与在线模式不同的依赖项解析。Cargo 将限制自己只使用本地已下载的 crate，即使本地索引副本中可能指示有更新的版本。有关在离线之前下载依赖项，请参阅 cargo-fetch(1) 命令。

此项也可以通过 net.offline 配置值指定。

--frozen
相当于同时指定 --locked 和 --offline。

### 杂项选项

-j N
--jobs N
并行运行的作业数量。此项也可以通过 build.jobs 配置值指定。默认为逻辑 CPU 的数量。如果为负数，则将最大并行作业数设置为逻辑 CPU 数量加上提供的值。如果提供了字符串 default，则将其值重置为默认值。不应为 0。
--keep-going
尽可能多地构建依赖图中的 crate，而不是在第一个构建失败时中止构建。
例如，如果当前包依赖于依赖项 fails 和 works，其中一个构建失败，cargo install -j1 可能会或可能不会构建成功的那个（取决于 Cargo 首先选择运行哪一个构建），而 cargo install -j1 --keep-going 则肯定会运行这两个构建，即使先运行的那个失败了。

### 显示选项

-v
--verbose
使用详细输出。可以指定两次以获得“非常详细”的输出，其中包括依赖项警告和构建脚本输出等额外信息。此项也可以通过 term.verbose 配置值指定。
-q
--quiet
不打印 cargo 日志消息。此项也可以通过 term.quiet 配置值指定。
--color 何时
控制何时使用彩色输出。有效值包括：
auto（默认）：自动检测终端是否支持彩色。
always：始终显示彩色。
never：从不显示彩色。
此项也可以通过 term.color 配置值指定。

--message-format 格式
诊断消息的输出格式。可以指定多次，由逗号分隔的值组成。有效值包括：
human（默认）：以人类可读的文本格式显示。与 short 和 json 冲突。
short：输出更短、人类可读的文本消息。与 human 和 json 冲突。
json：将 JSON 消息输出到标准输出。有关更多详细信息，请参阅参考。与 human 和 short 冲突。
json-diagnostic-short：确保 JSON 消息的 rendered 字段包含 rustc 的“短”渲染。不能与 human 或 short 一起使用。
json-diagnostic-rendered-ansi：确保 JSON 消息的 rendered 字段包含嵌入的 ANSI 颜色代码，以遵循 rustc 的默认配色方案。不能与 human 或 short 一起使用。
json-render-diagnostics：指示 Cargo 不在打印的 JSON 消息中包含 rustc 诊断信息，而是由 Cargo 本身渲染来自 rustc 的 JSON 诊断信息。Cargo 自己的 JSON 诊断信息和来自 rustc 的其他信息仍然会输出。不能与 human 或 short 一起使用。

### 通用选项

+工具链
如果 Cargo 是通过 rustup 安装的，并且 cargo 的第一个参数以 + 开头，则它将被解释为 rustup 工具链名称（例如 +stable 或 +nightly）。有关工具链覆盖的工作原理，请参阅 rustup 文档。
--config 键=值 或 路径
覆盖 Cargo 配置值。参数应采用 TOML 语法 键=值，或提供额外配置文件的路径。此标志可以多次指定。有关更多信息，请参阅命令行覆盖部分。
-C 路径
在执行任何指定操作之前更改当前工作目录。这会影响 Cargo 默认查找项目清单文件 (Cargo.toml) 的位置，以及搜索发现 .cargo/config.toml 等目录。此选项必须出现在命令名称之前，例如 cargo -C path/to/my-project build。
此选项仅在每夜构建通道上可用，并需要 -Z unstable-options 标志才能启用（参见 #10098）。

-h
--help
打印帮助信息。
-Z 标志
Cargo 的不稳定标志（仅限每夜构建）。运行 cargo -Z help 查看详细信息。

## 退出状态

0：Cargo 成功。
101：Cargo 未能完成。

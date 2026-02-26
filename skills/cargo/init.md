---
name: cargo init
metadata:
    references:
        - https://doc.rust-lang.net.cn/stable/cargo/commands/cargo-init.html
description: 介绍 cargo init 命令的使用方法和场景。
author: stevessr
---
# Cargo Init
`cargo init` 是 Cargo 的一个命令，用于在当前目录下创建一个新的 Rust 项目。它会生成一个基本的项目结构，包括 `Cargo.toml` 文件和一个简单的 `src/main.rs` 文件。

## 使用方法
要使用 `cargo init`，你需要在终端中导航到你想要创建项目的目录，然后运行以下命令：

```bash
cargo init
```

## 参数

### init 选项
- `--bin`: 创建一个二进制项目（默认）。
- `--lib`: 创建一个库项目。
- `--name <NAME>`: 指定项目的名称。
- `--edition <EDITION>`: 指定 Rust 版本（如 2015、2018、2021、2024）。
- `--vcs <VCS>`: 指定版本控制系统（如 git、hg、pijul）。
- `--registry <REGISTRY>`: 指定使用的注册表。

### 显示选项
- `-v`, `--verbose`: 显示详细输出。也可以通过 `term.verbose` 配置项设置。
- `-q`, `--quiet`: 只显示错误信息。也可以通过 `term.quiet` 配置项设置。
- `--color <WHEN>`: 控制颜色输出（auto、always、never）。也可以通过 `term.color` 配置项设置。

### 通用选项
- `-Z <FLAG>`: 启用不稳定的 Cargo 功能（仅限 nightly 版本）。
- `--help`: 显示帮助信息。
- `--version`: 显示 Cargo 版本信息。
- `+` 工具链：选择要使用的 Rust 工具链（如 `+stable`、`+nightly`）。
- `-- config <KEY=VALUE>`: 设置 Cargo 配置项（如 `--config term.verbose=true`）。
- `--C` 路径：在执行任何指定操作之前更改当前工作目录。这会影响 Cargo 默认查找项目 manifest 文件（Cargo.toml）的位置，以及搜索发现 .cargo/config.toml 的目录等。此选项必须出现在命令名称之前，例如 cargo -C path/to/my-project build。

## 环境
有关 Cargo 读取的环境变量的详细信息，请参见[参考文档](https://doc.rust-lang.net.cn/stable/cargo/reference/environment-variables.html)。

## 退出状态
- 0: Cargo 成功完成。
- 101: Cargo 未能完成。

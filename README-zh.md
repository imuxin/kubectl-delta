# kubectl-delta

[English](./README.md)

一个可以监听 kubernetes 资源的变更信息的 kubectl 插件。其中变更的内容通过使用 [delta](https://github.com/dandavison/delta) 或 [difftastic](https://github.com/Wilfred/difftastic) 工具提供漂亮的终端界面展示。

|                  使用 delta 概览                   |                使用 difftastic 概览                |
| :------------------------------------------------: | :------------------------------------------------: |
| ![overview-delta.png](./assets/overview-delta.png) | ![overview-difft.png](./assets/overview-difft.png) |


## 安装说明

### [推荐] 方式一：使用 Docker 镜像

1. 您需要在环境里预先安装好 Docker，参考 [官网](https://docs.docker.com/engine/install/)；或者安装 containerd，参考 [安装教程](https://github.com/containerd/containerd/blob/main/docs/getting-started.md#installing-containerd) 和 [nerdctl](https://github.com/containerd/nerdctl) 命令行工具。
2. 拷贝 script 目录下的 kubectl-delta 脚本到环境的 $PATH 其中的一个目录下，比如 `/usr/local/bin`。
```bash
cp script/kubectl-delta /usr/local/bin/
chmod +x /usr/local/bin/kubectl-delta
```

### 方式二：从 [release assets](https://github.com/imuxin/kubectl-delta/releases) 下载可执行制品。
### 方式三：使用 [Cargo](https://crates.io/crates/kubectl-delta)进行源码编译安装。

```bash
cargo install kubectl-delta --locked
```

## Cmd 帮助

```bash
USAGE:
    kubectl-delta [OPTIONS] [ARGS]

ARGS:
    <RESOURCE>    Support resource 'plural', 'kind' and 'shortname'
    <NAME>        Resource name, optional

OPTIONS:
    -A, --all                       If present, list the requested object(s) across all namespaces
        --diff-tool <DIFF_TOOL>     Diff tool to analyze delta changes [default: delta] [possible values: delta, difft]
        --export <EXPORT>           A path, where all watched resources will be strored
    -h, --help                      Print help information
        --include-managed-fields    Set ture to show managed fields delta changes
    -l, --selector <SELECTOR>       Selector (label query) to filter on, supports '=', '==', and '!='.(e.g. -l key1=value1,key2=value2)
    -n, --namespace <NAMESPACE>     If present, the namespace scope for this CLI request
    -s, --skip-delta                Skip show delta changes view
        --use-tls                   Use tls to request api-server
    -V, --version                   Print version information
```

## 参考实例

监听所有命名空间下的 deployment 资源
```bash
kubectl-delta deployment -A
```

监听某个命名空间下的 depoyment 资源
```bash
kubectl-delta deployment -n {namespace}
```

监听某个命名空间下的某个 depoyment 资源
```bash
kubectl-delta deployment -n {namespace} {name}
```

追加 `--skip-delta` 选项，仅监听变动资源，同 `kubectl get -w`
```bash
kubectl-delta {resource} --delta
```

追加 `--diff-tool difft` 选项来使用 `difftastic` 工具显示变化内容
```bash
kubectl-delta {resource} --diff-tool difft
```

追加 `--export "/to/your/path"` 选项，导出监听的资源到本地存储
```bash
kubectl-delta {resource} --export "/to/your/path"
```

`managed-fields` 默认是不进行比对的, 追加 `--include-managed-fields` 选项，展示 managed fields 的变化
```bash
kubectl-delta {resource} -include-managed-fields
```

## 致谢

- [delta](https://github.com/dandavison/delta)
- [difftastic](https://github.com/Wilfred/difftastic)
- [kube-rs](https://github.com/kube-rs/kube-rs)
- [rust](https://github.com/rust-lang/rust)

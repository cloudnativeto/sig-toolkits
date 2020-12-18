---
description: Kubernetes CLI To Manage Your Clusters In Style!
---

# K9s

[K9s](https://github.com/derailed/k9s) 提供了一个命令行界面来与你的 Kubernetes 集群进行交互。这个项目的目的是让你更容易在集群外浏览、观察和管理你的应用程序。K9s 会持续观察 Kubernetes 的变化，并提供后续的命令与你观察到的资源进行交互。

[![K9s &#x754C;&#x9762;&#x9884;&#x89C8;](https://asciinema.org/a/305944.svg)](https://asciinema.org/a/305944)

## K9s 的安装及热键绑定

K9s 基于 Go 开发，可以轻松在 Linux, macOS, Windows 三大平台上运行，通常可以 [直接下载使用 Binary 文件](https://github.com/derailed/k9s/releases)，或通过以下几个主流的工具安装：

{% tabs %}
{% tab title="Homebrew" %}
```bash
brew install k9s
```
{% endtab %}

{% tab title="Pacman" %}
```
pacman -S k9s
```
{% endtab %}

{% tab title="Chocolatey" %}
```
choco install k9s
```
{% endtab %}

{% tab title="Scoop" %}
```
scoop install k9s
```
{% endtab %}
{% endtabs %}

K9s 的配置项很丰富，比较常用且有用的是 [热键的设定](https://github.com/derailed/k9s#hotkey-support)，创建 `$HOME/.k9s/hotkey.yml` 按如下格式录入内容，即可完成 k9s 内置命令的热键绑定：

```yaml
# $HOME/.k9s/hotkey.yml
hotKey:
  # Hitting Shift-1 navigates to your pod view
  shift-1:
    shortCut:    Shift-1
    description: Viewing pods
    command:     po
  # Hitting Shift-2 navigates to your deployments
  shift-2:
    shortCut:    Shift-2
    description: View deployments
    command:     dp
  # Hitting Shift-2 navigates to your services 
  shift-3:
    shortCut:    Shift-3
    description: View services
    command:     svc
  # Hitting Shift-4 navigates to your statefulsets view
  shift-4:
    shortCut:    Shift-4
    description: Viewing statefulsets
    command:     sts
  # Hitting Shift-5 navigates to your daemonsets
  shift-5:
    shortCut:    Shift-5
    description: View daemonsets
    command:     ds
  # Hitting Shift-0 navigates to your nodes 
  shift-0:
    shortCut:    Shift-0
    description: View nodes
    command:     no
```

{% hint style="info" %}
还有一个推荐给“爱美”人士使用的定制内容是皮肤，可配置的内容很多，但配置项太过繁琐，通常推荐直接下载使用 [官方皮肤](https://github.com/derailed/k9s/tree/master/skins)，保存为 `$HOME/.k9s/skin.yml` 即可。
{% endhint %}

## K9s 的常用命令

```bash
# List all available CLI options
$ k9s help

# To get info about K9s runtime (logs, configs, etc..)
$ k9s info

# To run K9s in a given namespace
$ k9s -n mycoolns

# Start K9s in an existing KubeConfig context
$ k9s --context coolCtx

# Start K9s in readonly mode - with all modification commands disabled
$ k9s --readonly
```

### K9s 的基础界面

![K9s &#x8FD0;&#x884C;&#x754C;&#x9762;&#x793A;&#x4F8B;](../.gitbook/assets/image%20%287%29.png)

* \*\*\*\*💙 **蓝色区域**：当前 K8s 的上下文、当前集群、当前 K9s 及 K8s 版本等信息
* \*\*\*\*💚 **绿色区域**：数字 `0, 1, 2 ... 9` 代表了不同的 Namespace，在界面中直接按对应数字即可进行切换显示
* \*\*\*\*❤ **红色区域**：当前视图下所有可操作的视图专有命令

### K9s 的常用操作命令

> 以下列举一些在 K9s 启动后比较常用、通用且比较推荐能熟练使用的操作命令。

| **命令** | **执行内容** |
| :--- | :--- |
| `?` | 显示所有可用的键盘符号和帮助 |
| `ctrl-a` | 显示所有可用的资源别名 |
| `:q`, `ctrl-c` | 退出 `K9s` |
| `:`po ⏎ | 用单数/复数或短名来查看 Kubernetes 资源，如 `po`, `dp`, `svc`, `cm`, `sec`, `rb`, etc. |
| `/`filter-key ⏎ | 筛选出一个资源视图，给定一个过滤器 |
| `/`-l label-selector ⏎ | 通过标签过滤资源视图 |
| `/`-f filter-key ⏎ | 模糊查找给定一个关键字的资源 |
| `<esc>` | 退出视图 / 命令 / 过滤模式 |
| `c` | 复制当前所选（所在）的资源的名称 |
| `d`, `y`, `e`, `l`,... | 常用命令来描述、查看 YAML、编辑 YAML、查看日志等等 |
| `:`ctx ⏎ | 查看和切换到另一个 Kubernetes 集群环境 |
| `:`ctx context-name ⏎ | 查看和切换到另一个指定的 Kubernetes 集群环境 |
| `:`ns ⏎ | 查看并切换到另一个 Kubernetes 命名空间 |
| `ctrl-w` | 切换查看当前资源的更多/更少信息 |
| `ctrl-d` | 删除一个资源（`TAB` 键和 `ENTER` 键确认） |
| `ctrl-k` | 删除一个资源（直接删除，不会有确认！） |

## K9s 的部分高级功能

> 以下介绍一些没有直接出现在官方文档部分（通常在 Release Notes 中可以找到的）相对不容易留意到的“隐藏”功能。

### **快速变更工作负载的副本数量**

通过 `:dp` 或 `:rs` 进入 Deployments 或 StatefulSets 视图界面后，可以通过 `s` 快捷键在所在资源上调出副本数量修改窗口，输入数量并确定即可完成修改。

![](../.gitbook/assets/image%20%289%29.png)

### **使用 Port Forward 在本地使用集群内服务**

在 Pods, Deployments / StatefulSets, Services 视图界面都可以通过 `shift-f` 的快捷键，执行容器的本地端口映射操作。

![](../.gitbook/assets/image%20%2810%29.png)

### **使用 Pulses 视图跟踪实施资源状态**

通过 `:pu` 可以进入一个摘要视图，这个视图列出了集群中最常用的资源类型，和它们的当前数量及活动状态，以 5 秒为周期刷新。

![](../.gitbook/assets/image%20%286%29.png)

### **使用 XRay 视图获取资源的树状关系图**

通过 `:x <res> [ns]` 可以进入 XRay 视图，从而查看和遍历资源之间的关系和关联，并检查引用的完整性。比如我们通过 `:x dp` 可以进入如下的 Deployments 资源 XRay 视图，它会以 Deployments 为基础通过树状关系图罗列其所包含的 Pods 及 Pods 所绑定的其它资源。目前 XRay 支持探查：Pods, Deployments, StatefulSets, Services, DaemonSets。

![](../.gitbook/assets/image%20%284%29.png)

{% hint style="info" %}
需要注意的是，可以直接从 XRay 视图中操作对应的资源；并且可以用 `/` 快捷键来对当前的根资源进行过滤。
{% endhint %}

### **使用 Popeye 检查资源配置的合理程度**

> [Popeye](https://popeyecli.io/) 是 K9s 作者开发的另一个 K8s 命令行工具，现已被集成进 K9s，它可以实时扫描你的集群，并报告潜在的问题，比如：引用完整性、配置错误、资源使用等。

通过 `:popeye` 命令可以进入 Popeye 的总览视图，然后可以通过在给定的资源条目上按 `Enter` 键来查看更为详细的检测报告。

![](../.gitbook/assets/image%20%285%29.png)

![](../.gitbook/assets/image%20%288%29.png)

{% hint style="info" %}
Popeye 还支持一个配置文件，即 `spinach.yml`，该文件提供了自定义扫描资源的内容，并根据自己的策略设置不同的严重程度。

请阅读 [Popeye 文档](https://popeyecli.io/#the-spinachyaml-configuration)，了解如何自定义报告。`spinach.yml` 文件将从 K9s 的主目录 `$HOME/.k9s/MY_CLUSTER_CONTEXT_NAME_spinach.yml` 中读取。
{% endhint %}

## 使用 K9s 插件机制接入更多外部工具

K9s 允许你通过 [插件机制](https://github.com/derailed/k9s#plugins) 定义自己的集群命令来扩展你的命令行和工具。K9s 会查看 `$HOME/.k9s/plugin.yml` 来定位所有可用的插件。一个插件的定义如下：

* `shortCut` 快捷键选项代表用户键入激活插件的组合键。
* `confirm` 确认选项（启用时）让你看到将要执行的命令，并给你一个确认或阻止执行的选项。
* `description` 说明将被打印在 K9s 菜单中的快捷方式旁边。
* `scopes` 作用域为与插件相关联的视图定义了资源名称/简称的集合。你可以指定所有，为所有视图提供这个快捷方式。 
* `command` 代表插件在激活时运行的临时命令。 
* `background` 指定命令是否在后台运行。
* `args` 指定适用于上述命令的各种参数。

K9s 同时提供了额外的环境变量，供你自定义插件的参数。目前，可用的环境变量如下：

* `$NAMESPACE`：选定的资源命名空间 
* `$NAME`：所选资源名称 
* `$CONTAINER` ：当前容器（如果适用）
* `$FILTER`：当前的过滤器（如果有）
* `$KUBECONFIG`：KubeConfig 文件的位置 
* `$CLUSTER`：当前的集群名称 
* `$CONTEXT`：当前的上下文名称 
* `$USER`：当前用户 
* `$GROUPS`：当前的用户组 
* `$POD`：容器视图中的 Pod 

{% hint style="warning" %}
插件是一个实验性的功能！随着这个功能的巩固，选项和布局可能会在未来的 K9s 版本中发生变化。
{% endhint %}

### 自定义 Stern 插件支持多 Pod 的日志查询

[Stern](https://github.com/wercker/stern) 是一款社区知名的 K8s 集群服务日志查询工具，虽然 k9s 已然集成了部分 Stern 的功能，但操作不太直接也无法连接管道操作（ `|` ），我们可以利用插件创建一个可以查询当前命名空间下多个 Pod 的快捷命令。

```yaml
# $HOME/.k9s/plugin.yml
plugin:
  stern:
    shortCut: Ctrl-L
    confirm: false
    description: "Logs (Stern)"
    scopes:
      - pods
    command: /usr/local/bin/stern # NOTE! Look for the command at this location.
    background: false
    args:
     - --tail
     - 100
     - $FILTER      # NOTE! Pulls the filter out of the pod view.
     - -n
     - $NAMESPACE   # Use current namespace
     - --context
     - $CONTEXT     # Use current k8s context
```

{% hint style="success" %}
### 直接使用 stern 在命令行查询日志

使用 `stern` 命令，我们可以在本地动态查看 Pod 的日志信息。Stern 可以让你根据 Kubernetes 中的 Pod 和容器生成以不同颜色编码的输出。`stern` 命令的使用方法很简单，以下列举一些常见、常用的操作作为参考：

```bash
# Tail all pods within a current namespace:
$ stern .

# Tail all pods that matches a given regular expression:
$ stern pod_query

# Tail matched pods from all namespaces:
$ stern pod_query --all-namespaces

# Tail matched pods from 15 minutes ago and ignore 'probe' messages:
$ stern pod_query -e probe -s 15m

# Tail matched pods with a specific label:
$ stern pod_query -l release=canary

# Pipe the log message to jq
$ stern pod_query -o json | jq .
```
{% endhint %}


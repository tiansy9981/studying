# 一. Ansible是什么？

## 1. 行业痛点

很多年以来，系统管理员都是通过界面或者命令行**手动**去管理多个管理节点。使用资源清单、其他文档或记忆的例程来执行标准任务。通过上述这种工作方式很容易出错，系统管理员很容易跳过一个步骤或者错误的执行一个步骤。同时系统管理员对执行的步骤是否正常执行或者是否产生预期结果的验证是有限的（**命令行的痛点**）。

同时系统管理员通过手动和独立管理多台服务器时，各个服务器之间的配置存在一定的差异，这样会使系统维护变得十分困难，从来带来人为的不稳定因素。（**非自动化带来的风险**）

自动化可以避免人工管理系统和基础设施引起的问题。自动化可以**快速、准确**的部署和配置你的系统。同时可以自动完成日常计划任务或者其他重复工作。

## 2. 解决方法

### 2.1 基础设施即代码

好的自动化系统允许你通过代码实现系统的基础架构（基础设施即代码），它意味着你可以通过机器可读语言定义和描述你的基础设施所处的状态。同时这种语言也应该便于人类阅读，以便于你去对其进行修改。

同时基础架构代码化可以引入到代码审计中，以及对其过程进行文档化，以减少操作风险

### 2.2 减少人为错误

通过任务自动化以及基础机构代码化，可以减少在服务器上手动执行任务，从而减少手动应用带来的风险。

## 3. Ansible 的概念

Ansible是一个开源的自动化平台。它是一种简单的自动化语言，可以完美地描述Ansible Playbooks中的It应用程序基础结构。它也是运行Ansible playbook的自动化引擎。

Ansible可以管理强大的自动化任务，并且可以适应许多不同的工作流和环境。同时，Ansible的新用户可以非常迅速地使用它来提高生产力。

# 二. Ansible 的特点

## 1. 简单

Ansible Playbook 提供较简单的可读性，人们可以很容易的去阅读、理解和更改。同时也容易编码。剧本按照顺序执行任务，剧本设计简单性可以使团队成员快速上手。

## 2. 功能强大

Ansible可用来部署应用程序、进行配置管理、工作流自动化和网络自动化。Ansible可用于编排整个应用程序生命周期。

## 3. 无代理

Ansible 无代理客户端，通常 Ansible 使用 OpenSSH 或者 WinRM 连接到它管理的主机并运行任务，通常(但不总是)通过向这些主机推出称为Ansible模块的小程序。这些程序用于将系统置于特定的所需状态。当Ansible完成它的任务时，任何被推送的模块都会被移除。

## 4. 总结

### 4.1 跨平台

支持:Ansible在物理、虚拟、云和容器环境中为Linux、Windows、UNIX和网络设备提供无代理支持。

### 4.2 人类可读的自动化

Ansible playbook，以YAML文本文件的形式编写，易于阅读，并有助于确保每个人都理解他们将做什么。

### 4.3 日志化

每个更改都可以由Ansible playbook进行，并且应用程序环境的每个方面都可以被描述和记录。

### 4.4 容易被版本管理

Ansible剧本和项目是纯文本。它们可以像源代码一样被处理，并放在现有的版本控制系统中。

### 4.5 动态清单

Ansible管理的机器列表可以从外部源动态更新，以便随时捕获所有托管服务器的正确的当前列表，而不管基础设施或位置如何。

### 4.6 高集成性

HP SA、Puppet、Jenkins、Red Hat Satellite以及您环境中存在的其他系统，可以利用并集成到您的Ansible工作流中。

### 4.7 幂等性（**最重要的**）

幂等性就是多次执行，效果与一次执行相同。这个是与自动化 shell 脚步最大的区别。

> **当然不是所有的操作都是幂等性的！！！例如常用的 shell、command、raw 模块！！！所以在使用 ad-hoc 或者 playbook 时尽量避免使用这些模块！**

### 4.8 期望式声明

理想状态声明机制，通过根据希望系统所处的状态表示 IT 部署，从而解决如何自动化 IT 部署问题。与 Kubernetes 类似。



# 三. Ansible 的基本框架

在 Ansible 管理架构中，有**控制节点**与**管理主机**的两种类型。Ansible 通常安装、运行在控制节点，

管理主机通常被记录在 **inventory** 资源清单文件中，inventory 资源清单是采用 text文件类型，在 inventory 文件中管理主机可以被分组，可以使用静态资源清单或者动态资源清单。

**playbook** 是 Ansible根据用户指定顺序执行一些列任务，剧本使用 yaml 格式，剧本可以包含一个或者多个**task**。

**module**是用来实现某些特定功能，每个模块都有对应的特定功能，task通过使用模块实现对系统的操作与管理。

**task** 是指具体执行一系列动作以便系统达到特定状态。

**ad hoc command** 是指通过 Ansible 直接运行某个命令。

# 四. 部署 Ansible

## 1. 管理节点

```bash
yum install epel-release -y && yum install ansible -y
```



```bash
# ubuntu 上安装 ansible。
 sudo apt update 
 sudo apt install software-properties-common 
 sudo add-apt-repository --yes --update ppa:ansible/ansible 
 sudo apt install ansible
```



## 2. 主机节点

Linux 或者 Unix主机节点需要有安装Python 2(2.6或更高版本)或Python 3(3.5或更高版本)以使大多数模块工作。

Microsoft Windows托管主机设计的模块需要托管主机上的PowerShell 3.0或更高版本，而不是Python。此外，管理的主机需要配置PowerShell远程功能。Ansible还要求在Windows管理的主机上至少安装.net Framework 4.0或更高版本。

网络设备可以使用 ssh 连接（**待试**）

# 五. Ansible基础用法

## 1. Ansible.cfg说明

ansible.cfg在/etc/ansible/ansible.cfg，~/.ansible.cfg，./ansible.cfg以及ANSIBLE_CONFIG环境变量，config 文件优先级：环境变量>./ansible.cfg>~/.ansible.cfg>/etc/ansible/ansible.cfg。可以使用 ansible --version 验证正在使用的 config 文件。或者使用 ansible all -i inventory  --list-hosts -v 查看正在使用 config 文件。

在 config 文件中，存在两个选项：[defaults],[privilege_escalation]。defaults 设置 ansible 的默认值，privilege_escalation配置 ansible 提权。基本配置如下：

```
[defaults] 
inventory = ./inventory 
remote_user = user 
ask_pass = false 
[privilege_escalation] 
become = true 
become_method = sudo 
become_user = root 
become_ask_pass = false
```

其他 config配置可以参考：[https://docs.ansible.com/ansible/latest/reference_appendices/config.html#common-options](https://docs.ansible.com/ansible/latest/reference_appendices/config.html#common-options)

## 2. Inventory

### 2.1 inventory 文件介绍

inventory 文件支持静态以及动态资源清单。inventory 支持分组的概念，可以把一类主机节点归类为一组，在 inventory文件中有两个特殊的组，一个是 all 一个ungrouped组。子组使用children标识。具体表明方法如下演示所示。

all主机组包含目录中显式列出的所有主机。
ungrouped的主机组包含清单中明确列出的所有不属于任何其他组的主机。

> ansible配置文件可以存在任意路径下，默认安装是/etc/ansible/ansible.cfg文件，也会存在~/.ansible.cfg文件，或者在当前任意目录下创建一个配置文件 ./ansible.cfg，或涉及环境变量ANSIBLE_CONFIG指定的配置文件。
> 若这些文件都存在，则优先级是：环境变量 > 当前目录下的配置文件 > ~/.ansible.cfg配置文件 > /etc/ansible/ansible.cfg配置文件 > 默认配置参数。

### 2.2 inventory文件写法

text 格式 inventory 写法如下：

```
mail.example.com

[usa]  
washington1.example.com  
washington2.example.com 
192.168.3.19 node_name=k8s-master1   #host 参数 node_name 
192.168.3.20 node*name=k8s-master2 
192.168.3.21 node*name=k8s-master3

[canada]  
ontario01.example.com  
ontario02.example.com 
washington[1:2].example.com

[north-america:children]  
canada  
usa
  
```

yaml 格式 inventory 文件写法如下：

```yaml
ungrouped:
  hosts:
    mail.example.com:
usa:
  hosts:
    washington1.example.com:
    washington2.example.com:
    192.168.3.19 node_name=k8s-master1:
    192.168.3.20 node_name=k8s-master2:
    192.168.3.21 node_name=k8s-master3:
canada:
  hosts:
    ontario01.example.com:
    ontario02.example.com:
    washington[1:2].example.com:
    www[01:50:2].example.com:
north-america:
   children:
     canada:
     usa:
```

在指定 inventory 文件时可以使用-i  PathName或者--inventory PathName参数指定对应的 inventory 单个文件或**文件夹**。查看对应的组内主机后者验证 inventory 文件可使用--list-hosts 命令查看，例如 ansible all -i inventory  all --list-hosts

### 2.3  定义主机的高级用法

前面提到了使用 inventory 定义管理主机与主机组以及在 playbook 中定义 hosts 的基本使用方法，在高级方法中，我们将指出基本方法的一些不便指出，以至于提示日常使用时的便利性以及减少错误的可能性。

在 inventory 中，我们常常会通过主机名或者域名的方式去定义管理主机，这本身是一种不错的方式，但是由于主机名或者域名本身与实际管理主机存在一定的解析关系，一旦这个解析关系发生错乱，会导致使用 ansible 时执行的指定执行到其他非被管理主机或者无法被执行的问题。所以为了避免这种问题的产生，尽量在本机记录好对应的解析关系，例如在 hosts 文件中记录对应的解析关系或者在主机参数host*vars中定义好解析关系。建议定在主机参数 host*vars中。对于主机名存在特殊字符的，建议对主机名使用单引号，进行字符转移。

在执行 playbook 时，需要指定执行的管理主机 hosts，我们常常使用主机组或者主机名，在 playbook 中，**hosts 支持通配符的引入**。例如有三个主机： data1、data2、data3，我们可以使用 data*表示这三个主机，而不需要分别写完整这三个主机名称，当然你可以将这三个主机引入到一个组方便引用。同时主机组也可以使用通配符。在 hosts 中，使用逗号作为主机或者主机组的分割符。

**在 playbook 的 hosts 中除了可以使用通配符以外还可以使用逻辑符&与！.&表示交集的关系；**！表示非的关系。例如主机组 a 里面有 1,2,3,4这几个主机，而 b 里面有 1,2,5,6这几个主机，a，&b 或者&a，b 表示的是主机 1,2。同时 a,!2表示在 a 主机组中排除 2 这个主机。all,!a表示所有主机排除主机组a 在内的所有主机。

### 2.4 创建动态 inventory

静态 inventory 文件适合管理那种小规模的集群，一旦管理主机的规模上来了，达到成千上万台，通过手动去写静态的 inventory 文件已经显得不太现实。所以获取动态的 inventory 文件就显得非常有必要了。

获取动态 inventory 是通过脚本去获取的，需要在 `ansible.cfg` 文件中配置 `inventory = inventory` 参数，或者ansible-playbook使用-i参数指定**可执行**的inventory文件。官网动态inventory脚步请见：[https://github.com/ansible/ansible/tree/mazer*role*loader/contrib](https://github.com/ansible/ansible/tree/mazer_role_loader/contrib)或https://docs.ansible.com/ansible/latest/dev_guide/index.html

**自定义动态inventory**

动态inventory的程序可以使用任何语言，最终生成的inventory将是JSON格式的。通过使用ansible-inventory命令可以了解Ansible的inventory的JSON格式要求。



## 3. ad hoc 命令

基本用法：

```bash
Shell ansible host-pattern -m module [-a 'module arguments'] [-i inventory]
```

其中 -m 参数指定模块名称，-a 参数将模块所需参数传给模块。默认命令模块是 command 模块，-o 参数以单行格式显示Ansible ad hoc命令的输出。

可使用 ansible-doc -l 查看所有模块。使用 ansible-doc 模块名称查看具体模块使用方法与示例。ansible-doc命令还提供了-s选项，该选项生成示例输出，可以作为如何在剧本中使用特定模块的模型。该输出可以用作启动模板，可以将其包含在剧本中，以实现任务执行的模块。输出中包含注释，以提醒管理员使用每个选项。

**使用 ansible-doc中的"-t"选项用于指定插件类型。例如**become, cache, callback,cliconf, connection, httpapi, inventory, lookup, netconf, shell, vars, module, strategy,test, filter, role, keyword**等插件,默认是 module；

```
ansible-doc -t lookup -l  #查看此模块有哪些插件
ansible-doc -t lookup dict #查看此模块的具体插件用法**
```

### 3.1常见的模块

模块由上游社区开发，同时这些模块可能处在不同的开发阶段。在使用过程中注意模块的开发状态十分有必要。模块有以下几种开发状态（status）：

stableinterface：该模块的关键字是稳定的，并且将尽一切努力不删除关键字或改变其含义。

preview：该模块处于技术预览阶段，可能不稳定，关键字可能会更改，或者它可能需要库或web服务，而这些库或web服务本身也会受到不兼容的更改的影响。

deprecated：该模块已弃用，并且在将来的某个版本中将不再可用。

removed：该模块已从发行版中删除，但存在一个存根，用于文档目的，以帮助以前的用户迁移到新模块。

supported_by字段记录了由谁在上游Ansible社区中维护该模块：

core：由上游的“核心”Ansible开发人员维护，并始终包含在Ansible中。

curated：由社区中的合作伙伴或公司提交和维护的模块。
这些模块的维护者必须注意报告的任何问题或针对模块提出的拉取请求。在社区维护者批准变更后，上游“核心”开发人员会审查对策划模块提出的变更。核心提交者还确保由于Ansible引擎的更改而导致的这些模块的任何问题都得到修复。这些模块目前包含在Ansible中，但将来可能会单独打包。

community：核心上游开发人员、合作伙伴或公司不支持的模块，但完全由一般的开源社区维护。这个类别中的模块仍然是完全可用的，但是对问题的响应速度完全取决于社区。这些模块目前也包含在Ansible中，但可能会在将来的某个时候单独打包。

**与包管理相关的模块：**

| name           | description                                                  |
| -------------- | ------------------------------------------------------------ |
| yum            | 使用yum包管理器安装、升级、降级、删除和列出包和组。此模块仅适用于Python 2。如果您需要Python 3支持，请参阅[dnf]模块。 |
| dnf            | 使用' dnf'包管理器安装、升级、删除和列出包和组               |
| yum_repository | 在基于rpm的Linux发行版中添加或删除YUM存储库。如果您希望更新现有的存储库定义，请使用[ini_file]代替。 |
| rpm_key        | 在rpm数据库中添加或删除(rpm——import) gpg密钥                 |
| package        | 使用底层操作系统包管理器安装、升级和删除包。对于Windows目标，请使用[win_package]模块。 |
| package_facts  | 作为fact变量返回关于已安装包的信息                           |
| apt_rpm        | 使用“apt-rpm”管理包。需要低级(' rpm')和高级(' apt-get')包管理器二进制文件。 |
| pip            | 管理Python库依赖项                                           |

**与文件相关的模块：**

| name        | description                                                  |
| ----------- | ------------------------------------------------------------ |
| replace     | 该模块将替换文件中模式的所有实例。这取决于用户通过确保相同的模式永远不会匹配任何替换来保持幂等性。 |
| template    | 模板将文件发送到远程服务器                                   |
| find        | 返回基于特定条件的文件列表。多个标准被AND组合在一起。对于Windows目标，请使用[win_find]模块。 |
| stat        | 检索文件的事实，类似于Linux/Unix的'stat'命令。对于Windows目标，请使用[win_stat]模块。 |
| lineinfile  | 此模块确保特定行位于文件中，或使用反向引用正则表达式替换现有行。当您只想更改文件中的一行时，这主要是有用的。看到<br/>如果你想改变多个类似的行，可以使用[replace]模块;如果你想在文件中插入/更新/删除行块，可以检查[blockinfile]模块。其他情况请参见[copy]或[template]模块。 |
| copy        | “copy”模块将文件从本地或远程机器复制到远程机器上的某个位置。使用[fetch]模块将文件从远程位置复制到本地机器。如果你需要变量在复制的文件中插入，使用[template]模块。在“content”字段中使用变量将会导致不可预测的输出。对于Windows目标，请使用[win_copy]模块。 |
| blockinfile | 此模块将插入/更新/删除由可定制标记包围的多行文本块行。       |
| fetch       | 这个模块的工作原理类似于[copy]，但相反。它用于从远程机器获取文件，并将它们本地存储在按主机名组织的文件树中。如果' dest'中已经存在的文件与' src'不同，则会被覆盖。该模块也支持Windows目标。 |
| tempfile    | ' tempfile'模块创建临时文件和目录。' mktemp'命令在不同的系统上使用不同的参数，这个模块有助于避免与此相关的麻烦。module创建的文件/目录只能由creator访问。如果你需要让他们世界访问，你需要使用[file]模块。对于Windows目标，请使用[win_tempfile]模块。 |
| stat        | 检索文件的fact，类似于Linux/Unix的'stat'命令。对于Windows目标，请使用[win_stat]模块。 |
| file        | 设置文件、符号链接或目录的属性。或者，删除文件、符号链接或目录。许多其他模块支持与' file'模块相同的选项-包括[copy]， [template]和[assemble]。对于Windows目标，请使用[win_file]模块。 |
| assemble    | 从片段组装配置文件。通常，特定的程序将采用单个配置文件，而不支持“conf.d”文件风格的结构，很容易从多个来源构建配置。' assemble'将获取一个文件目录，这些文件可以是本地的，也可以是已经传输到系统的，并将它们连接在一起以生成目标文件。文件按字符串排序顺序组装。Puppet称这种想法为“碎片”。 |

**与系统或服务相关的模块：**

| name               | description                                                  |
| ------------------ | ------------------------------------------------------------ |
| syslogger          | 使用syslog向主机添加日志表项。指定设施和优先次序             |
| sysctl             | 该模块操作sysctl条目，并在更改它们后可选择执行' /sbin/sysctl -p' |
| systemd            | 控制远程主机上的系统服务                                     |
| selinux            | 配置SELinux模式和策略。使用后可能需要重新启动。Ansible不会发出这个重启，但是会在需要的时候通知你。 |
| selinux_permissive | 从允许域列表中添加和删除一个域                               |
| selogin            | 管理linux用户到SELinux用户的映射                             |
| pam_limits         | ' pam_limits'模块修改PAM限制。默认文件是' /etc/security/limits.conf'。有关完整文档，请参阅' man 5 limits.conf' |
| pamd               | 编辑PAM服务的类型、控件、模块路径和模块参数。为了修改PAM规则，类型、控件和module_path必须与现有规则匹配。 |
| modprobe           | 加载或卸载内核模块                                           |
| service_facts      | 将服务状态信息作为各种服务管理实用程序的fact数据返回         |
| service            | 控制远程主机上的服务。支持的初始化系统包括BSD init、OpenRC、SysV、Solaris SMF、systemd、upstart。对于Windows目标，使用[win_service]模块代替。 |
| firewalld          | 该模块允许在运行或永久防火墙规则中添加或删除服务和端口(TCP或UDP)。 |
| ufw                | 使用UFW管理防火墙                                            |
| reboot             | 重新启动一台机器，等待它关机、重新启动并响应命令。对于Windows目标，请使用[win_reboot]模块 |
| hostname           | 设置主机名称                                                 |
| at                 |                                                              |
| cron               | 使用该模块管理crontab和环境变量条目。该模块允许您创建环境变量和命名的crontab条目、更新或删除它们。 |
| add_host           | 使用变量在库存中创建新的主机和组，以便在同一剧本的后续剧本中使用。接受变量，以便可以更完整地定义新主机。该模块也支持Windows目标。 |
| openssl_privatekey | 这个模块允许(重新)生成OpenSSL私钥.密钥以PEM格式生成。请注意，如果私钥与模块的选项不匹配，模块会重新生成私钥。特别是，如果您提供另一个密码短语(或没有指定)、更改密钥大小等，则将重新生成私钥。如果您担心这会覆盖您的私钥，请考虑使用“backup”选项。该模块可以使用加密Python库或pyOpenSSL Python库。默认情况下，它尝试检测哪一个可用。这可以用' select_crypto_backend'选项覆盖。请注意，PyOpenSSL后端在Ansible 2.9中已弃用，并将在Ansible 2.13中删除。” |
| acl                | 设置和检索文件ACL信息                                        |
| pids               | 分区将从磁盘开始的偏移量开始，即从磁盘开始的“距离”。距离可以用parted支持的所有单位指定(compat除外)，并且区分大小写，例如:“10GiB”,“15%”。(默认值:0%),类型:str |
| lvol               | 该模块创建、删除或调整逻辑卷的大小                           |
| vdo                | 该模块控制VDO重删压缩设备。VDO (Virtual Data Optimizer)是一个设备映射目标，它为主存储提供内联块级重复数据删除、压缩和精简配置功能。 |
| mount              | 该模块控制/etc/fstab中活动的和配置的挂载点。                 |
| filesystem         | 创建一个文件系统                                             |
| lvg                | 创建、删除或调整卷组大小                                     |
| parted             | 这个模块允许使用' parted'命令行工具配置块设备分区。有关字段和选项的完整描述，请参阅GNU parted手册。 |
| xfs_quota          | 在XFS文件系统上配置配额。在使用此模块之前，需要配置/etc/projects和/etc/proid |
|                    |                                                              |

**与用户或组相关的：**

| name           | description                                                  |
| -------------- | ------------------------------------------------------------ |
| authorized_key |                                                              |
| user           | 管理用户帐号和用户属性。对于Windows目标，请使用[win_user]模块 |
| known_hosts    |                                                              |
| group          | 管理主机上组的存在状态。对于Windows目标，请使用[win_group]模块。 |

**与网络相关的模块：**

| name                  | description                                             |
| --------------------- | ------------------------------------------------------- |
| net_ping              | 使用ping从网络设备测试可达性                            |
| iptables              | 修改iptables规则                                        |
| telnet                | 执行低级和脏telnet命令                                  |
| nmcli                 | 管理网络                                                |
| net_put               | 从Ansible Controller拷贝文件到网络设备                  |
| netscaler_ssl_certkey | 管理ssl证书密钥                                         |
| ip_netns              | 管理网络名称空间                                        |
| ping                  | 尝试连接到主机，验证一个可用的python，并返回' pong'成功 |
| get_url               | 从HTTP、HTTPS或FTP下载文件到节点                        |
| net_vlan              | 在网络设备上管理vlan                                    |



**与云原生相关的：**

| name               | description                                                  |
| ------------------ | ------------------------------------------------------------ |
| k8s_scale          | 类似kubectl scale命令。用于设置部署、复制集或复制控制器的副本数量，或作业的并行性属性。支持校验模式。 |
| docker_volume      | 创建/删除Docker卷。执行与“docker volume”CLI子命令基本相同的功能。 |
| docker_image       | 构建、加载或拉取映像，使映像可用于创建容器。还支持将j镜像标记到存储库中并将镜像归档到.tar文件。自Ansible 2.8以来，建议明确指定镜像的源(' source'可以是' build'， ' load'， ' pull'或' local')。这将需要从Ansible 2.12开始。 |
| docker_container   | 管理docker容器的生命周期。支持校验模式。运行'——check'和'——diff'来查看配置差异和要采取的操作列表。 |
| docker_volume_info | 功能与CLI子命令“docker volume inspect”基本相同               |
| docker_config      | 在Swarm环境中创建和删除Docker配置。类似于' docker config create'和' docker config rm'。在新配置'ansible_key'的元数据中添加数据的加密散列表示，然后在将来的运行中使用它来测试配置是否已更改。如果'ansible_key'不存在，那么配置将不会更新，除非设置了' force'选项。对配置的更新是通过删除配置并重新创建配置来执行的。 |
| kubevirt_pvc       | 使用Openshift Python SDK来管理Kubernetes上的pvc支持容器化数据导入 |
| k8s                | 使用OpenShift Python客户端对K8s对象执行CRUD操作。从源文件或内联文件传递对象定义。请参阅读取文件和使用Jinja模板或保险库加密文件的示例。访问K8s api的全部范围。使用[k8s_info]模块获取关于“kind”类型对象的项列表。使用配置文件、证书、密码或令牌进行身份验证。支持校验模式。 |
| helm               | 使用Helm包管理器安装、升级、删除和列出包。                   |
| docker_network     | 创建/删除Docker网络，并将容器连接到它们。执行与“docker network”CLI子命令基本相同的功能。 |
| k8s_service        | 使用Openshift Python SDK管理Kubernetes上的服务               |
| k8s_auth           | 该模块处理需要“显式”身份验证过程的Kubernetes集群的身份验证，这意味着客户端登录(获取一个验证令牌)，使用所述令牌执行API操作，然后注销(撤销令牌)。Kubernetes发行版需要这个模块的一个例子是OpenShift。另一方面，用户名+密码身份验证的流行配置是利用HTTP基本认证，它不涉及任何额外的登录/注销步骤(相反，登录凭据可以附加到执行的每个API调用)，因此由' k8s'模块(和其他资源特定的模块)通过利用' host'， ' username'和' password'参数直接处理。请查阅您首选模块的文档以了解更多详细信息。 |
| k8s_info           | 使用OpenShift Python客户端对K8s对象执行读操作。访问K8s api的全部范围。使用配置文件、证书、密码或令牌进行身份验证。支持校验模式。在Ansible 2.9之前，这个模块被称为“k8s_facts”。用法没有改变。 |

**与ansible调试相关的：**

| name            | description                                                  |
| --------------- | ------------------------------------------------------------ |
| include_vars    |                                                              |
| debug           |                                                              |
| import_playbook |                                                              |
| setup           |                                                              |
| set_fact        |                                                              |
| facter          |                                                              |
| gather_facts    |                                                              |
| import_tasks    |                                                              |
| pause           | 暂停剧本执行一段时间，或直到确认提示。所有参数为可选参数。默认行为是使用提示暂停。要在每个主机上暂停/等待/休眠，请使用[wait_for]模块。如果您希望在暂停设置到期之前推进暂停，或者如果您需要完全中止剧本运行，则可以使用“ctrl+c”。要继续提前按“ctrl+c”然后按“c”。要中止剧本，请按“ctrl+c”，然后按“a”。暂停模块集成到异步/并行剧本没有任何特殊的考虑(见滚动更新)。当使用带有' serial ' playbook参数的暂停(如滚动更新)时，您只会提示当前主机组一次。该模块也支持Windows目标。 |



|                    |      |
| ------------------ | ---- |
| shell              |      |
|                    |      |
|                    |      |
| make               |      |
| terraform          |      |
| group_by           |      |
| mso_role           |      |
|                    |      |
|                    |      |
| nginx_status_info  |      |
|                    |      |
|                    |      |
| redis              |      |
| htpasswd           |      |
| expect             |      |
|                    |      |
|                    |      |
|                    |      |
| uri                |      |
| nginx_status_facts |      |
|                    |      |
| haproxy            |      |

> **在 playbook 中尽量避免使用command, shell, raw模块，尽管它们实现功能十分简单，但是它们容易造成非幂等效果。**

## 4. playbook

为什么有了ad-hoc以后还需要使用playbook呢？因为ad-hoc执行命令时，书写不清晰，难易更改或者对执行的命令难易审核。通过使用playbook，将命令代码化，方便可读、审核、版本管理等优势。

### 4.1 playbook的构成：

playbook是由一个或以上的task构成的。

### 4.2 playbook 基本写法：

```yaml
---
- name:  #名称描述
  hosts:  #主机或者主机组
  tasks:
   - name: #任务描述
     user: #模块名称
       name: newbie
       uid: 4000
       state: present  #模块参数以及期望值
```

>在 yaml 文件中只有空格被允许，tab键制表符在 yaml 是被禁止使用的。如果您使用vi文本编辑器，您可以应用一些设置，这可能会使编辑剧本更容易。它会在按Tab键时执行2个空格的缩进，并自动缩进后续行。在$HOME/.vimrc文件中添加
>autocmd FileType yaml setlocal ai ts=2 sw=2 et

playbook 采用的是声明式yaml 格式文件，playbook 是一系列key-value 键值对的集合，在相同的结构下采用相同的缩进。在官方的声明方式中，playbook 包含 name、hosts、task。这三个 key 保持相同的缩进。其中 name 的key 值是可选的，尽管是可选的，但是建议使用，它可以声明这个 playbook 的目的，有助于记录并更容易地监视剧本的执行进度。

在 playbook 中可以包含多个 play，在play中又可以包含多个task ，task 执行是按顺执行的。

配置ansible-playbook执行的输出长度，参数如下：

| OPTION | DESCRIPTION                                                  |
| ------ | ------------------------------------------------------------ |
| -v     | The task results are displayed.                              |
| -vv    | Both task results and task configuration are displayed       |
| -vvv   | Includes information about connections to managed hosts      |
| -vvvv  | Adds extra verbosity options to the connection plug-ins, including users being used in the managed hosts to execute scripts, and what scripts have been executed |

### 4.3 ansible-playbook 语法检查与 dry run以及troubleshooting：

```bash
ansible-playbook --syntax-check webserver.yml
```

ansible-playbook 的 dry run:

```bash
ansible-playbook -C webserver.yml
ansible-playbook --check webserver.yml
```

> **==当task中指定check_mode: no时，运行check时，会执行task。==**

ansible-playbook的setup以及start-at-task

```
ansible-playbook play.yml --step
ansible-playbook play.yml --start-at-task="start httpd service"
```

**其他测试模块：**

| name   | description                                                  |
| ------ | ------------------------------------------------------------ |
| uri    | 与HTTP和HTTPS web服务交互，支持摘要、基本和WSSE HTTP身份验证机制。对于Windows目标，请使用[win_uri]模块。 |
| script | ' script'模块接受脚本名称后跟以空格分隔的参数列表。自由格式命令或' cmd'参数都是必需的，参见示例。路径处的本地脚本将被传输到远程节点，然后执行。将通过远程节点上的shell环境处理给定的脚本。这个模块不需要远程系统上的python，就像[raw]模块一样。该模块也支持Windows目标。 |
| assert | 该模块使用可选的自定义消息断言给定表达式为真。该模块也支持Windows目标。 |



### 4.4 yaml 的用法说明

注释：一般使用#开头表示注释

string（字符串）：YAML中的字符串通常不需要加引号，即使字符串中包含空格。字符串可以用双引号或单引号括起来。

```yaml
YAML this is a string
'this is another string'
"this is yet another a string"
```

有两种方法可以编写多行字符串。您可以使用竖杠(|)字符来表示要保留字符串中的换行字符。

```yaml
include_newlines: |
          Example Company
          123 Main Street
          Atlanta, GA 30303
```

还可以使用大于(>)字符编写多行字符串，以指示将换行符转换为空格，并删除行中前导空白。此方法通常用于在空格字符处分隔长字符串，以便它们可以跨多行以获得更好的可读性。

```yaml
fold_newlines: >
      This is an example
      of a long string,
      that will become
      a single sentence once folded.
```

**YAML Dictionaries（yaml 字典）：**

你已经见过键值对的集合写成缩进块，如下所示:

YAML name: svcrole  svcservice: httpd  svcport: 80

```yaml
name: svcrole 
svcservice: httpd 
svcport: 80
```

字典也可以写成用花括号括起来的内联块格式，如下所示:

```
{name: svcrole, svcservice: httpd, svcport: 80}
```

> 在大多数情况下，内联块格式应该避免，因为它更难阅读。然而，至少有一种情况下它更常用。角色的使用将在本课程后面讨论。当剧本包含角色列表时，更常见的是使用此语法，以便更容易区分剧本中包含的角色和传递给角色的变量。

**YAML Lists（yaml 清单）：**

```yaml
hosts:
  - servera
  - serverb
  - serverc
 #或者
 hosts: [servera, serverb, serverc]
```

## 5. variables与facts

### 5.1 定义参数

在 ansible 中参数有三个基本等级：

Global scope: 全局作用域，从命令行或Ansible配置设置的变量

Play scope：Play范围，在play和相关结构中设置的变量

Host scope：由清单、fact收集或注册任务在主机组和单个主机上设置的变量

如果一个参数被多次定义，那么它的会在最小范围内生效，即最小范围内的优先级最高。Host scope>Play scope>Global scope

### 5.2 在 playbook 定义参数

参数块

```yaml
- hosts: all 
  vars:
   user: joe 
   home: /home/joe
```

vars_files指令，定义外部文件

```
- hosts: all 
  vars_files:
   - vars/users.yml
```

### 5.3 引用参数

使用{{}}引用参数

```yaml
vars:
  user: joe
tasks:
# This line will read: Creates the user joe 
 - name: Creates the user {{ user }} 
   user:
# This line will create the user named Joe 
    name: "{{ user }}" #需要使用双引号
```

### 5.4 主机参数与主机组参数

定义主机参数或主机组参数时可以在 inventory中定义，示例如下：

```ini
[master]
master1.example.com   node_name=master1

[servers]
demo1.example.com 
demo2.example.com

[servers:vars]
user=joe
```

上述语法是一种过时的语法，一般不建议使用。

为主机和主机组定义变量的首选方法是在与目录文件或目录相同的工作目录中创建两个目录，group_vars和host_vars。这些目录分别包含定义组变量和主机变量的文件（在 group_vars或 host_vars 目录下创建同 inventory 中主机或主机组同名的文件，使用 yaml 文件格式去标明参数）。示例如下：

```yaml
[admin@station project]$ cat ~/project/inventory 
[datacenter1] 
demo1.example.com 
demo2.example.com

[datacenter2] 
demo3.example.com 
demo4.example.com

[datacenters:children] 
datacenter1 
datacenter2
```

主机组 datacenters 参数

```shell
[admin@station project]$ cat ~/project/group_vars/datacenters  
package: httpd
```

子主机组参数

```bash
[admin@station project]$ cat ~/project/group_vars/datacenter1  
package: httpd 

[admin@station project]$ cat ~/project/group_vars/datacenter2  
package: apache
```

主机参数

```bash
[admin@station project]$ cat ~/project/host_vars/demo1.example.com  
package: httpd 

[admin@station project]$ cat ~/project/host_vars/demo2.example.com  
package: apache 

[admin@station project]$ cat ~/project/host_vars/demo3.example.com  
package: mariadb-server 

[admin@station project]$ cat ~/project/host_vars/demo4.example.com  
package: mysql-server
```

使用命令行覆盖playbook 中的参数时，可以使用-e。

```bash
[user@demo ~]$ ansible-playbook main.yml -e "package=apache"
```



### 5.5 使用数组参数

当你定义一个存在一定联系的集合参数时：

```yaml
user1_first_name: Bob 
user1_last_name: Jones 
user1_home_dir: /users/bjones 
user2_first_name: Anne 
user2_last_name: Cook 
user2_home_dir: /users/acook
```

你可以使用以下格式去定义一个user的数组，里面包含两个用户：

```yaml
users:
  bjones:
   first_name: Bob 
   last_name: Jones 
   home_dir: /users/bjones 
  
  acook:
   first_name: Anne 
   last_name: Cook 
   home_dir: /users/acook
```

当你需要调用参数，可以使用以下两种方式

```yaml
# Returns 'Bob' 
users.bjones.first_name

# Returns '/users/acook' 
users.acook.home_dir
```

另一种方式：

```
# Returns 'Bob' 
users['bjones']['first_name']

# Returns '/users/acook' 
users['acook']['home_dir']
```

**如果键名与Python方法或属性(如discard、copy、add等)的名称相同，点表示法可能会导致问题。使用括号符号可以帮助避免冲突和错误。所以尽量使用括号的方式引用数组参数。**

### 5.6  使用register变量捕获命令输出

如果在执行 playbook 时，想获取某一个 task 中的命令执行输出，可以使用 register 参数去获取输出。例如：

```yaml
---
 - name: Installs a package and prints the result 
   hosts: all 
     tasks:
       - name: Install the package 
         yum:
           name: httpd 
           state: installed 
         register: install_result
         
       - debug: var=install_result
```

执行 playbook

```tex
[user@demo ~]$ ansible-playbook playbook.yml 
PLAY [Installs a package and prints the result] ****************************

TASK [setup] *************************************************************** 
ok: [demo.example.com]

TASK [Install the package] ************************************************* 
ok: [demo.example.com]

TASK [debug] *************************************************************** 
ok: [demo.example.com] => {
       "install_result": {
       "changed": false, 
       "msg": "", 
       "rc": 0,
       "results": [ "httpd-2.4.6-40.el7.x86_64 providing httpd is already installed" 
       ]
                        }
                         }

PLAY RECAP ***************************************************************** demo.example.com : ok=3 changed=0 unreachable=0 failed=0
```

### 5.7 管理 secret

secret 是通过Ansible Vault对敏感参数进行加密，简单而言就是使用Ansible Vault对参数进行脱敏的过程。

**Ansible Vault**

密码或者API 密钥这些信息可能以纯文本形式存储在库存变量或其他Ansible文件中。然而，在这种情况下，任何访问Ansible文件或存储Ansible文件的版本控制系统的用户都可以访问这些敏感数据。这带来了明显的安全风险。**Ansible Vault**用来加密和解密Ansible使用的任何结构化数据文件。要使用Ansible-Vault，需要使用一个名为ansible-vault的命令行工具来创建、编辑、加密、解密和查看文件。ansible-vault使用AES256对称加密保护文件，并使用密码作为密钥。

使用方法：ansible-vault create filename

```
[student@demo ~]$ ansible-vault create secret.yml 
New Vault password: redhat 
Confirm New Vault password: redhat
#或者指定密码文件
ansible-vault create --vault-password-file=vault-pass secret.yml
```

查看加密文件：ansible-vault view filename

```
[student@demo ~]$ ansible-vault view secret1.yml 
Vault password: secret 
less 458 (POSIX regular expressions) Copyright (C) 1984-2012 Mark Nudelman

less comes with NO WARRANTY, to the extent permitted by law. For information about the terms of redistribution, see the file named README in the less distribution. Homepage: http://www.greenwoodsoftware.com/less 
my_secret: "yJJvPqhsiusmmPPZdnjndkdnYNDjdj782meUZcw"
```

修改加密文件：ansible-vault edit filename

```
[student@demo ~]$ ansible-vault edit secret.yml 
Vault password: redhat
```

解密加密文件：ansible-vault decrypt filename

```
[student@demo ~]$ ansible-vault decrypt secret1.yml --output=secret1-decrypted.yml 
# --output 将解密的文件指定生成一个新文件
Vault password: redhat 
Decryption successful
```

已加密文件修改密码：ansible-vault rekey filename

```
[student@demo ~]$ ansible-vault rekey secret.yml 
Vault password: redhat 
New Vault password: RedHat 
Confirm New Vault password: RedHat 
Rekey successful
#指定生成新的密码文件
[student@demo ~]$ ansible-vault rekey --new-vault-password-file=NEW_VAULT_PASSWORD_FILE secret.yml
```

playbook 执行加密的 yaml 文件：

```
[student@demo ~]$ ansible-playbook --vault-password-file=vault-pw-file site.yml
#或者
[student@demo ~]$ ansible-playbook --vault-id @prompt site.yml 
Vault password (default): redhat
#ansible 版本早于 2.4 使用如下参数：
[student@demo ~]$ ansible-playbook --ask-vault-pass site.yml 
Vault password: redhat
```

### 5.8 管理 facts

Ansible facts是由Ansible在托管主机上自动发现的变量。facts包含特定于主机的信息，这些信息可以像在play、条件、循环或依赖于从托管主机收集的值的任何其他语句中的常规变量一样使用。

在 2.3 版本以后使用Gathering Facts task 获取 fact 变量。2.3 版本以前使用 setup 模块获取 fact 变量。

两种获取 fact 变量的方法：

```
ansible hostname -m setup >1.json
#或者使用playbook
- name: Fact dump 
  hosts: all 
  tasks:
    - name: Print all facts 
      debug:
        var: ansible_facts
#在 2.3 版本以后 ansible_facts参数表示 fact 变量，使用 debug 模块即可打印所有 fact 变量
```

引用 fact 变量：ansible_facts['hostname']或者 ansible_facts.hostname，建议使用第一种。在 playbook 中使用{{}}来标识 fact 变量的调用，{{ansible_facts.hostname}}。

>目前，Ansible可以同时识别新的事实命名系统(使用ansible_facts)和旧的2.5之前的“作为单独变量注入的事实”命名系统。
>你可以通过将Ansible配置文件[default]部分中的inject_facts_as_vars参数设置为false来关闭旧的命名系统。当前默认设置为true。

当你执行 playbook 时默认是收集 facts，如果你想加快 playbook 的执行速度，可以在 playbook 中加入gather_facts: no的关键字。当你想在某个 task 想收集 facts 时，可以在某个 task 中加入 setup 模块。

创建自定义 facts：

默认情况下，setup模块从每个托管主机的/etc/ansible/facts.d目录中加载自定义fact。为了使用，每个文件或脚本的名称必须以.fact结尾。动态自定义事实脚本必须输出json格式的事实，并且必须是可执行的。静态 fact 文件需使用 ini 或者 json 格式保存为静态文件。

```ini
[packages] 
web_package = httpd
db_package = mariadb-server

[users] 
user1 = joe
user2 = jane
```



```json
{

"packages": { 
  "web_package": "httpd", 
  "db_package": "mariadb-server" 
  }, 
"users": {
"user1": "joe",
"user2": "jane" 
  }
}
```

自定义事实由setup模块存储在ansible*facts.ansible*local参数中，facts是根据定义它们的文件的名称来组织的。例如，假设上述自定义facts是由保存为/etc/ ansible/facts.d/custom的文件生成的。托管主机上的facts，在这种情况下，ansible*facts.ansible*local['custom']['users']['user1']是joe。同样你可以使用 setup 命令打印自定义的 fact 变量或者在 playbook 中引用自定义的 fact 变量。

### 5.9 magic 参数（待补充）



## 6. 任务控制

### 6.1 循环loop

当你在使用同一模块做多次任务时，在不懂得使用循环的情况下，你将会写多个 task 去执行。但是当你懂得使用 loop 时，你就可以在一个 task 中完成此操作。这就是 loop 的意义。

在使用 loop 时，playbook 调用 loop 中的参数使用 item 表示引用循环变量。

> **需要注意{{ item }},在两个花括号中 item 与花括号之间有空格。**

简单的示例：

```yaml
- name: Postfix and Dovecot are running 
  service:
    name: "{{ item }}"
    state: started 
  loop:
    - postfix
    - dovecot
```

或者使用 vars代替：

```yaml
vars:
  mail_services:
    - postfix
    - dovecot
tasks:
 - name: Postfix and Dovecot are running 
   service:
     name: "{{ item }}" 
     state: started 
   loop: "{{ mail_services }}"
```

>从Ansible 2.5开始，推荐使用loop关键字来编写循环。
>但是，您仍然应该理解旧的语法，特别是with_items，因为它在现有的剧本中被广泛使用。您可能会遇到继续使用with_*关键字进行循环的剧本和角色。

register关键字还可以捕获task任务的输出，所以在 playbook 中加入关键字 register，并引用其变量，并可以打印 loop 循环的输出。具体如下：

```yaml
---
- name: Loop Register Test
  gather_facts: no
  hosts: localhost
  tasks:
   - name: Looping Echo Task
     shell: "echo This is my item: {{ item }}"
     loop:
      - one
      - two
     register: echo_results
   - name: Show echo_results variable
     debug:
       msg: "{{ echo_results}}"
```

执行上述 playbook，执行如下：

```text
[tiansy root ~] $ ansible-playbook  3.yaml
[WARNING]: provided hosts list is empty, only localhost is available. Note that the implicit localhost does not match 'all'

PLAY [Loop Register Test] ******************************************************************************************************************

TASK [Looping Echo Task] *******************************************************************************************************************
changed: [localhost] => (item=one)
changed: [localhost] => (item=two)

TASK [Show echo_results variable] **********************************************************************************************************
ok: [localhost] => {
    "msg": {
        "changed": true,
        "msg": "All items completed",
        "results": [
            {
                "ansible_loop_var": "item",
                "changed": true,
                "cmd": "echo This is my item: one",
                "delta": "0:00:00.053218",
                "end": "2023-11-30 14:39:49.005804",
                "failed": false,
                "invocation": {
                    "module_args": {
                        "_raw_params": "echo This is my item: one",
                        "_uses_shell": true,
                        "argv": null,
                        "chdir": null,
                        "creates": null,
                        "executable": null,
                        "removes": null,
                        "stdin": null,
                        "stdin_add_newline": true,
                        "strip_empty_ends": true,
                        "warn": true
                    }
                },
                "item": "one",
                "rc": 0,
                "start": "2023-11-30 14:39:48.952586",
                "stderr": "",
                "stderr_lines": [],
                "stdout": "This is my item: one",
                "stdout_lines": [
                    "This is my item: one"
                ]
            },
            {
                "ansible_loop_var": "item",
                "changed": true,
                "cmd": "echo This is my item: two",
                "delta": "0:00:00.051630",
                "end": "2023-11-30 14:39:49.245817",
                "failed": false,
                "invocation": {
                    "module_args": {
                        "_raw_params": "echo This is my item: two",
                        "_uses_shell": true,
                        "argv": null,
                        "chdir": null,
                        "creates": null,
                        "executable": null,
                        "removes": null,
                        "stdin": null,
                        "stdin_add_newline": true,
                        "strip_empty_ends": true,
                        "warn": true
                    }
                },
                "item": "two",
                "rc": 0,
                "start": "2023-11-30 14:39:49.194187",
                "stderr": "",
                "stderr_lines": [],
                "stdout": "This is my item: two",
                "stdout_lines": [
                    "This is my item: two"
                ]
            }
        ]
    }
}

PLAY RECAP *********************************************************************************************************************************
localhost                  : ok=2    changed=1    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

> loop 必须使用list（列表）或者dictionaries（字典）数据。如果传递的是只有一个元素的列表/字典，请尝试在查找调用中添加wantlist=True，或者使用q/query而不是lookup。

### 6.2 条件控制when

Ansible可以在满足某些条件时使用条件来执行task或play。条件语句允许管理员区分被管理的主机，并根据它们满足的条件为它们分配功能角色。Playbook变量、register变量和Ansible facts都可以用条件进行测试。可以使用运算符比较字符串、数字数据和布尔值。

when 的语法：

```yaml
---
- name: Test Variable is Defined Demo 
  hosts: all 
   vars:
     my_service: httpd
  tasks:
    - name: "{{ my_service }} package is installed" 
      yum:
        name: "{{ my_service }}" 
      when: my_service is defined
```

when 条件运算:

| OPERATION                                                    | EXAMPLE                                   |
| ------------------------------------------------------------ | ----------------------------------------- |
| Equal (value is a string)                                    | ansible_machine == "x86_64"               |
| Equal (value is numeric)                                     | max_memory == 512                         |
| Less than                                                    | min_memory < 128                          |
| Greater than                                                 | min_memory > 256                          |
| Less than or equal to                                        | min_memory <= 256                         |
| Greater than or equal to                                     | min_memory >= 512                         |
| Not equal to                                                 | min_memory != 512                         |
| Variable exists                                              | min_memory is defined                     |
| Variable does not exist                                      | min_memory is not defined                 |
| Boolean variable is true. The values of 1, True, or yes evaluate to true. | memory_available                          |
| Boolean variable is false. The values of 0, False, or no evaluate to false. | not memory_available                      |
| First variable's value is present as a value in second variable's list | ansible_distribution in supported_distros |

同时 when 还支持 and与 or 的多条件合并的运算。

### 6.3 handler处理器

由于 ansible 有操作的幂等性，在一些特殊情况下，在一个 playbook 执行完以后，需要执行一些非幂等性的操作，例如加载配置文件、重启服务等操作；此时 handler 就出现了。

handler 其实也是 task，它是去响应其他 task 的通知触发的。每个 handler 都只会有一个全局变量，同时 hanlder 是在 playbook 最后被触发。如果没有task以名称通知hanlder，则handler将不会运行。如果一个或多个task通知hanlder，则handler将在playbook中的所有其他task完成后恰好运行一次。

handler可以被视为非活动任务，只有在使用notify语句显式调用时才会触发。

```yaml
tasks:
- name: copy demo.example.conf configuration template 
  template:
    src: /var/lib/templates/demo.example.conf.template 
    dest: /etc/httpd/conf.d/demo.example.conf
  notify:
    - restart apache

handlers:
  - name: restart apache
    service:
      name: httpd 
      state: restarted
# 在执行完 task 里面的 template 模块的任务后，会有一个 notify 通知，通知给 restart appche 这个 handler，在 task 的下面，定义了一个名为 restart apache 的 handler 完成 service 的重启。同时可以配置多个 notify 去触发多个 handler
```

### 6.4 管理task 失败

在一般情况下，当在 play 中某个 task 失败时，play 的执行将会被终止，当让你会有这样的想法，有什么一种方法能让失败的 task 不影响整个 play 的运行呢？当然是有的，你可以在 task后，添加ignore_errors关键字从而忽略task 的错误而继续往下执行play。示例：

```yaml
- name: Latest version of notapkg is installed 
  yum:
    name: notapkg
    state: latest 
  ignore_errors: yes
```

在执行 play 时因为某个 task 失败导致 handler 无法正确执行时，可以使用force_handlers关键字。同时handler 的通知是在change 结果是 OK 时，触发 hanler 执行，所以一旦 notify定义在错误的 task 下，handler 也无法被触发执行。

```yaml
---
- hosts: all 
  force_handlers: yes 
  tasks:
   - name: a task which always notifies its handler 
     command: /bin/true
   notify: restart the database

   - name: a task which fails because the package doesn't exist 
     yum:
      name: notapkg 
      state: latest
  handlers:
   - name: restart the database 
     service:
      name: mariadb 
      state: restarted
```

failed_when来指定哪些条件可以决定task失败，通常用于可能成功执行命令的命令模块，人为的让此 task 去执行失败。failed_when与 fail 模块的使用类似。同时他们都能影响整个 play 的运行，如需过滤使用 ignore_errors 关键字。示例：

```yaml
---
- name: test failed_when
  hosts: localhost
  tasks:
   - name: show id  tiansy
     shell: id  tiansy
     register: command_result
     ignore_errors: yes
   - name: test failed_when
     debug:
       msg: 'it is exec'
     failed_when: "'no such user' in {{ command_result.stderr }}"
```

fail模块常常强制 task 失败，可以把上面的示例改为如下：

```yaml
tasks:
 - name: Run user creation script 
   shell: /usr/local/bin/create_users.sh 
   register: command_result 
   ignore_errors: yes

 - name: Report script failure 
   fail:
    msg: "The password is missing in the output" 
   when: "'Password missing' in command_result.stdout"
```

fail模块为task提供明确的失败消息。这种方法还支持延迟故障，允许您运行中间task来完成或回滚其他更改。它也会停止 play 的完整运行。

定义 changed是否生效，使用关键字changed*when，控制 task 何时生效。当changed*when为真时，task执行生效。为false，不生效。

```yaml
---
- name: test failed_when
  hosts: localhost
  tasks:
   - name: test changed_when
     debug:
       msg: 'it is exec'
     changed_when: true
     register: debug
   - name: test changed_when1
     template:
       src: /home/ubuntu/1.txt
       dest:  /home/ubuntu/3.txt
     register: debug1
     changed_when: false
   - name: debug the message
     debug:
       msg: "{{ debug1 }}"
```

它通常在运行时总是报告changed。为了抑制这种变化，可以设置changed*when: false，这样它只报告ok或failed。到changed*when的条件为false时，相当于dry run。

### 6.5 逻辑块

块将任务分组为一个单元，并根据块中的所有任务是否成功来执行其他任务，即block可以包含一系列的子task，可用于控制task的执行方式。例如，任务块可以使用when关键字将条件应用于多个task。

>==当块中存在when与block、rescue、always同级时，when的条件将会控制整个块。==

```yaml
- name: block example
  hosts: loaclhost
  tasks:
   - name: installing and configuring Yum versionlock plugin
     block:
      - name: package needed by yum
        yum:
         name: yum-plugin-versionlock
         state: present
      - name: lock version of tzdata
        lineinfile:
          dest: /etc/yum/pluginconf.d/versionlock.list
          line: tzdata-2016j-1
          state: present
     when: ansible_distribution == "RedHat"
```

**block**还允许与rescue和always语句结合使用进行错误处理。其中：

**block**：定义主要task运行；

**rescue**：定义在block中task运行失败时运行的task，即救援模式；

**always**：定义不管在运行在block或rescue中的task是否正常运行，都会运行的task。具体流程描述：首先运行block中的task，如果全部正常运行，则不会运行rescue中的task。如果block中存在某一个task不能正常运行，再运行rescue中的task。最后不过block中的task是否都能正常运行，或者rescue中的task是否也能全部正常运行，都需要运行always中的task。同时block、rescue、always逻辑块还是遵循task失败后退出执行，不会继续执行块中之后的task的逻辑。

### 6.6执行控制

pre-task：

post-task：



## 7. ansible对管理主机的文件操作

### 7.1 常见文件操作的模块

| module name | module description                                           |
| ----------- | ------------------------------------------------------------ |
| copy        | Copy a file from the local or remote machine to a location on a managed host. Similar to the file module, the copy module can also set file attributes, including SELinux context. |
| lineinfile  | Ensure that a particular line is in a file,or replace an existing line using a back-reference regular expression. This module is primarily useful when you want to change a single line in a file. |
| file        | Set attributes such as permissions, ownership, SELinux contexts,and time stamps of regular files, symlinks, hard links, and directories.This module can also create or remove regular files, symlinks,hard links, and directories. A number of other file-related modules support the same options to set attributes as the file module, including the copy module. |
| stat        | Retrieve status information for a file,similar to the Linux stat command. |
| fetch       | This module works like the copy module, but in reverse. This module is used for fetching files from remote machines to the control node and storing them in a file tree, organized by host name. |
| blockinfile | Insert, update, or remove a block of multiline text surrounded by customizable marker lines. |
| synchronize | A wrapper around the rsync command to make common tasks quick and easy. The synchronize module is not intended to provide access to the full power of the rsync command, but does make the most common invocations easier to implement. You may still need to call the rsync command directly via the run command module depending on your use case. |
| sefcontext  | Manages SELinux file context mapping definitions. Similar to the `semanage fcontext' command. |

### 7.2 使用jinja2模板传输文件

Jinja2模板系统是定制配置文件的强大工具，主要用于制作模板文件，它可以使用Jinja2语法来引用剧本中的变量（自定义变量、fact 变量、register 变量）。

Jiaja2参数调用示例：

```jinja2
{# /etc/hosts line #}   #   {# #}在 jinja2 语法中表示注释
{{ ansible_facts['default_ipv4']['address'] }}  # 调用参数用 {{ }}表示或者使用{% %}
 {{ ansible_facts['hostname'] }}
```

Jinja2模板由多个元素组成:数据、变量和表达式。在呈现Jinja2模板时，这些变量和表达式将被替换为它们的值。模板中使用的变量可以在play中的vars部分中指定。可以将托管主机的fact用作模板上的变量。当执行关联 play 时，jinja2 中使用的 vars 将会被 play 中参数替代。

> jinja2 文件不需要特定的文件后缀名，但是使用j2 作为文件后缀方便我们识别文件类型。

通过 jinja2 语法配置 ssh 配置文件，示例如下：

```jinja2
# {{ ansible_managed }} 
# DO NOT MAKE LOCAL MODIFICATIONS TO THIS FILE AS THEY WILL BE LOST 
Port {{ ssh_port }} 
ListenAddress {{ ansible_facts['default_ipv4']['address'] }} 
HostKey /etc/ssh/ssh_host_rsa_key 
HostKey /etc/ssh/ssh_host_ecdsa_key 
HostKey /etc/ssh/ssh_host_ed25519_key 
SyslogFacility AUTHPRIV 
PermitRootLogin {{ root_allowed }} 
AllowGroups {{ groups_allowed }} 
AuthorizedKeysFile /etc/.rht_authorized_keys .ssh/authorized_keys 
PasswordAuthentication {{ passwords_allowed }} 
ChallengeResponseAuthentication no 
GSSAPIAuthentication yes 
GSSAPICleanupCredentials no 
UsePAM yes 
X11Forwarding yes 
UsePrivilegeSeparation sandbox 
AcceptEnv LANG LC_CTYPE LC_NUMERIC LC_TIME LC_COLLATE LC_MONETARY LC_MESSAGES 
AcceptEnv LC_PAPER LC_NAME LC_ADDRESS LC_TELEPHONE LC_MEASUREMENT 
AcceptEnv LC_IDENTIFICATION LC_ALL LANGUAGE 
AcceptEnv XMODIFIERS 
Subsystem sftp /usr/libexec/openssh/sftp-server
```

创建配置文件的Jinja2模板后，可以使用template模块将其部署到托管主机，该模块支持将控制节点上的本地文件传输到托管主机。示例如下：

```yaml
tasks:
 - name: template render 
   template:
      src: /tmp/j2-template.j2 
      dest: /tmp/dest-config-file.txt
```

> template模块它还可以使用validate选项来运行任意命令(例如visudo -c)，在将文件复制到适当位置之前检查其语法的正确性

为了避免系统管理员修改由Ansible部署的文件，在模板的顶部包含注释是一种很好的做法，以表明该文件不应该被手动编辑。

方法是使用ansible*managed指令中设置的“`Ansible managed`”字符串。这不是一个普通的变量，但可以在模板中作为一个变量使用。ansible*managed指令在`ansible.cfg`文件中设置:

```ini
ansible_managed = Ansible managed
```

然后在Jinja2模板中包含ansible_managed字符串，请使用以下语法:

```jinja2
{{ ansible_managed }}
```

jinja2 中使用 loop，在 jinja2 中使用 for 关键字来提供循环功能。示例如下：

```jinja2
{% for myhost in groups['myhosts'] %} 
   {{ myhost }} 
{% endfor %}
# myhosts 在 inventory 被定义在 group 组里面。
```

host文件的j2模版

```jinja2
{% for host in groups['all'] %} 
{{ hostvars['host']['ansible_facts']['default_ipv4']['address'] }}  {{ hostvars['host']['ansible_facts']['fqdn'] }} {{ hostvars['host'] ['ansible_facts']['hostname'] }} 
{% endfor %}
```

jinja2 同时也是可以使用条件判断，使用关键字if来标识条件判断。示例如下：

```jinja2
{% if finished %} 
{{ result }} 
{% endif %}
```



## 8. Ansible并行支持

理论上，Ansible能支持inventory文件中所有的主机连接，但对于大量被管理主机而言，这将加重控制主机的负担。Ansible可控制最大同时连接数，由Ansible配置文件中的forks参数控制，默认forks为5。如果task将是在被管理主机运行的Linux主机，且管理主机负载降低的情况下，可用适当的调高forks的值，接近100，将会看到性能的提升。如果被管理主机为router或者Switch，建议降低forks的值。调整默认forks的值除了在ansible.cfg中修改以外，还支持在ansible-playbook命令中添加-f或者--forks参数去设置对应的值。

**管理滚动更新**

在一般情况下，ansible被管理主机在运行tasks将同时批量更新对应操作，但面对web负载均衡时，所有同时更新会导致服务同时停止，为防止类似的情况，ansible支持使用**serial**关键字批量运行主机。每批主机将运行整个剧本，然后再开始下一批。示例如下：

```yaml
---
- name: Rolling update
  hosts: webservers
  serial: 2
  tasks:
  - name: latest apache httpd package is installed
    yum:
      name: httpd
      state: latest
    notify: restart apache
  handlers:
  - name: restart apache
    service:
      name: httpd
      state: restarted
```

## 9.playbook重用

通常在大型项目中，会存在多个playbook存在不同的作用。可通过指定import_playbook指令允许将包含playbook列表的外部文件导入playbook。main playbook也会按照它们将被导入并按顺序运行。

```yaml
- name: Prepare the web server
  import_playbook: web.yml
- name: Prepare the database server
  import_playbook: db.yml
```



```yaml
---
- name: Install web server
  hosts: webservers
  tasks:
  - import_tasks: webserver_tasks.yml
#或使用以下
---
 name: Install web server
  hosts: webservers
  tasks:
  - include_tasks: webserver_tasks.yml
```

**import_playbook与include_tasks的不同点：**

|                                       |                          Include_*                           |                           import_*                           |
| :------------------------------------ | :----------------------------------------------------------: | :----------------------------------------------------------: |
| Type of reuse 重用的类型              |                         Dynamic 动态                         |                         Static 静态                          |
| When processed 当处理                 |         At runtime, when encountered在运行时，当遇到         |  Pre-processed during playbook parsing剧本解析期间的预处理   |
| Task or play 任务或游戏               |           All includes are tasks 所有包含都是任务            |       `import_playbook` cannot be a task 不能成为任务        |
| Task options 任务的选择               |    Apply only to include task itself仅应用于包含任务本身     |  Apply to all child tasks in import应用于导入中的所有子任务  |
| Calling from loops 3.5.2从循环中调用  |     Executed once for each loop item对每个循环项执行一次     |           Cannot be used in a loop不能在循环中使用           |
| Using 使用`--list-tags`               |       Tags within includes not listed包含未列出的标签        |        All tags appear with 所有标签都以`--list-tags`        |
| Using 使用`--list-tasks`              |      Tasks within includes not listed包含中未列出的任务      |       All tasks appear with 所有任务都以`--list-tasks`       |
| Notifying handlers 通知处理程序       | Cannot trigger handlers within includes不能在包含文件中触发处理程序 | Can trigger individual imported handlers可以触发单个导入的处理程序 |
| Using 使用`--start-at-task`           | Cannot start at tasks within includes无法在包含的任务中启动  |       Can start at imported tasks可以从导入的任务开始        |
| Using inventory variables使用库存变量 |           Can 可以`include_*: {{ inventory_var }}`           |          Cannot 不能`import_*: {{ inventory_var }}`          |
| With playbooks 与剧本                 |                  No 没有`include_playbook`                   |          Can import full playbooks可以导入完整剧本           |
| With variables files 使用变量文件     |         Can include variables files可以包含变量文件          |      Use `vars_files:` to import variables 用于导入变量      |

## 10**.Ansible Role**

### 10.1 什么是role

ansible role是一个包含playbook、var、task等==**资源**==的一个==**复用**==的==**集合**==。

### 10.2 role的结构

```
[user@host roles]$ tree user.example user.example/
├── defaults
│ └── main.yml
├── files
├── handlers
│   └── main.yml
├── meta
│   └── main.yml
├── README.md
├── tasks
│   └── main.yml
├── templates
├── tests
│   ├── inventory
│   └── test.yml
└── vars
└── main.yml
```



| **SUBDIRECTORY** | **FUNCTION**                                                 |
| ---------------- | :----------------------------------------------------------- |
| **defaults**     | The **main.yml** file in this directory contains the default values of role variables that can be overwritten when the role is used. These variables have low precedence and are intended to be changed and customized in plays. |
| **files**        | This directory contains static files that are referenced by role tasks. |
| **handlers**     | **main.yml** file in this directory contains the role's handler definitions. |
| **meta**         | The **main.yml** file in this directory contains information about the role, including author, license, platforms, and optional role dependencies. |
| **tasks**        | The **main.yml** file in this directory contains the role's task definitions. |
| **templates**    | This directory contains Jinja2 templates that are referenced by role tasks. |
| **tests**        | This directory can contain an inventory and **test.yml** playbook that can be used to test the role. |
| **vars**         | The **main.yml** file in this directory defines the role's variable values. Often these variables are used for internal purposes within the role. These variables have high precedence, and are not intended to be changed when used in a playbook. |

> 不是所有的role都会包含上述目录结构！

### 10.3 如何管理role

ansible role的管理是通过ansible-galaxy命令实现的。

创建role

```bash
ansible-galaxy role init
ansible-galaxy role install
ansible-galaxy role import
```

删除role

```
ansible-galaxy role delete
ansible-galaxy role remove
```

查看role

```
ansible-galaxy role list
ansible-galaxy role  info
```

### 10.4 定义role

**定义参数与默认值**

role的参数的定义是创建**vars/main.yml**的文件，在此文件中以键值对的方式去定义对应的变量。role中引用参数使用{{**VAR_NAME**}}，其优先级参考局部性原则，具有最高优先级，不能被inventory的参数替代。

而默认值是在**defaults/main.yml**文件中定义的。同样它也是以键值对的方式定义其默认值。但是它是最低优先级，它容易被其他参数所覆盖。

role不能包含一些隐私数据，因为role是复用的，所以为了安全考虑，role中不能包含隐私数据。如果需要使用隐私数据，需要采用其他方式，例如ansible-vault encrypt 加密数据。

### 10.5 在playbook中使用role

在playbook中使用role是非常简单的，如下：

```yaml
---
- hosts: remote.example.com
  roles:
    - role1
    - role2
      var1: val1
      var2: val2
```

当role被引用时，role将会先运行，再运行你为该playbook定义的其他任务。

> 如上role2中，使用的var具有很高的优先级，它会覆盖其他同名的参数。所有尽量与其他参数区分开来。

**使用系统role**

使用系统role需要安装rhel-system-roles包(仅rhel系统支持)，rhel-system-roles包含如下：

```
linux-system-roles.ad_integration
linux-system-roles.certificate
linux-system-roles.cockpit
linux-system-roles.crypto_policies
linux-system-roles.firewall
linux-system-roles.ha_cluster
linux-system-roles.journald
linux-system-roles.kdump
linux-system-roles.kernel_settings
linux-system-roles.logging
linux-system-roles.metrics
linux-system-roles.nbde_client
linux-system-roles.nbde_server
linux-system-roles.network
linux-system-roles.podman
linux-system-roles.postfix
linux-system-roles.rhc
linux-system-roles.selinux
linux-system-roles.ssh
linux-system-roles.sshd
linux-system-roles.storage
linux-system-roles.timesync
linux-system-roles.tlog
linux-system-roles.vpn
```

获取其他role可通过访问https://galaxy.ansible.com/ui/获得或使用ansible-galaxy search ** --platforms ** 指定对应名称和平台获取。

### 10.6 执行顺序控制

在playbook中role具有最高优先级，而在一些情况下，有一些task需要执行在role之前，同时有需要有部分task执行在正常task之后。所有引进了**pre_tasks**、**post_tasks**。整体的执行顺序为： **pre_tasks**, **roles**, **tasks**, **post_tasks**、 **handlers**。示例如下：

```
- name: Play to illustrate order of execution
  hosts: remote.example.com
  pre_tasks:
    - debug:
       msg: 'pre-task'
      notify: my handler
  roles:
    - role1
  tasks:
    - debug:
       msg: 'first task'
      notify: my handler
  post_tasks:
    - debug:
       msg: 'post-task'
      notify: my handler
  handlers:
    - name: my handler
      debug:
       msg: Running my handler
```

在上例中，handler将会被执行三次。分别是pre_tasks、tasks、post_tasks。




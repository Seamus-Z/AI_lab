# AI_lab

汇总 AI 在不同领域的实践。各领域项目使用独立 Git 仓库，通过 **Git submodule** 关联；本仓库维护入口与子模块的固定提交，不复制子项目源码、依赖或本机配置。

## 项目

| 子模块路径 | 独立仓库 | 当前内容 |
|---|---|---|
| [`AI4PCB/`](AI4PCB/) | [Seamus-Z/AI4PCB](https://github.com/Seamus-Z/AI4PCB) | 嘉立创原理图 Skill、受保护的 MCP、官方 SDK 桥接、模块化布局规则与验证示例 |

AI4PCB 的安装步骤、安全约束及验收边界见 [子项目说明](AI4PCB/README.md)。扩展1.0.2的坐标往返、端口标签与闭合模块框工厂已在隔离工程实测；SDK在线新建RC/分压器仍受嘉立创网络错误阻断，不能将源码、安全测试或局部原生工厂通过扩大解释为所有写入入口均已验证。

## 获取全部项目

```sh
git clone --recurse-submodules git@github.com:Seamus-Z/AI_lab.git
cd AI_lab
```

已经克隆过主仓库时：

```sh
git submodule update --init --recursive
```

该命令检出主仓库记录的子模块提交，可能处于 detached HEAD；这不是出错，也不会自动跟随子仓库最新代码。

## 更新已有本地副本

先保存各仓库中的未提交工作，再执行：

```sh
git pull --ff-only
git submodule sync --recursive
git submodule update --init --recursive
```

同步的是主仓库已记录的版本，不使用 `git submodule update --remote` 自动漂移到新版本。

## 修改并发布子项目

先进入子项目并切到工作分支。使用 `main` 时，如本地分支不存在则创建并跟踪远端：

```sh
cd AI4PCB
git switch main  # 尚无本地 main 时：git switch -c main --track origin/main
git pull --ff-only
```

完成修改与子项目要求的验证后，先在子仓库提交并推送：

```sh
git add <本次修改的文件>
git commit -m "Describe the AI4PCB change"
git push origin main
```

再回到主仓库更新固定提交并推送：

```sh
cd ..
git add AI4PCB
git commit -m "Update AI4PCB submodule"
git push origin main
```

两个仓库各有提交和推送操作。仅推送 AI4PCB 不会改变 AI_lab 的固定版本；仅推送 AI_lab 也不会上传尚未推送的子项目提交。

## 增加其他领域

先在 GitHub 创建独立项目仓库，完成其初始提交并推送，再在本仓库执行：

```sh
git submodule add <独立仓库的SSH地址> <领域目录名>
git add .gitmodules <领域目录名>
git commit -m "Add domain submodule"
git push origin main
```

同时在上方项目表中登记该领域。不要将普通目录或未提交的嵌套仓库冒充子模块。

## 本机资料

各子项目自行安装依赖、生成密钥和客户端配置。账号密钥、`local/`、`outputs/`、配对扩展和私有工程资料不随本仓库上传。SSH 地址需要本机具备相应 GitHub 访问权限。

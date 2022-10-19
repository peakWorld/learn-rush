# learn-rush

## 操作

- 初始化仓库 `rush init`

- 添加项目
  建议每次只添加或验证单个项目，而不是一次性将所有项目都添加到 rush.json 中

* 将文件拷贝到项目中
* 编辑 rush.json 文件, 添加相应配置
* 执行`rush update`来安装依赖

- package.json 文件发生变化, 运行`rush update`

* 当从 git 上拉取新的更改（例如 git pull）后。
* 当项目内 package.json 文件被手动修改后。
* 当 common/config 目录下可能影响版本的文件（例如 pnpmfile.js, common-versions.json 等）被修改后

- 拉取最新的修改，应该进行编译所有项目

  > rush rebuild

- 针对一个项目构建，可以使用 rushx 指令
  在对应的项目下运行 rushx 指令，rushx 指令与 npm run 的指令类似

- 构建仓库内的所有项目
  仓库内某个项目不需要被 rush build 处理，依然需要保留 build 字段，将其设定为空字符串("") 后 Rush 会忽略它。

  > rush build

- 在项目中添加依赖 或 更新某个已有依赖的版本

  > rush add --package example-lib // 添加依赖
  > rush add --package "example-lib@^1.2.0" // 更新依赖
  > rush add --package example-lib@1.2.3 --make-consistent // 将仓库内所有项目的该依赖一次更新为指定版本

- 在公共文件夹下安装 NPM 包, 但是并没有自动执行 "rush link"

  > rush install --no-link

- 全量构建并实时输出细节日志

  > rush rebuild --ship --verbose

- 移除所有被 Rush 创建的链接

  > rush unlink

- 移除 Rush 创建的所有临时文件，包括删除公共文件夹下已下载的 NPM 包。
  > rush purge

## 注意

### Rush 仓库内不要使用某些指令

Rush 会在某个中心文件夹安装所有的依赖，之后使用符号链接给每个项目创建 "node_modules" 文件夹。

- 不要使用包管理工具来安装或链接依赖。诸如 npm install, npm update, npm link, npm dedupe 等命令
- 在使用`git clean -dfx` 之前，确保已经运行 rush unlink.
- 运行 rush update 重新生成符号连接

### 强制重新完全安装你的包 `rush update --purge`

### 构建方式

- 如果你用到的项目很少：假如你的 Git 仓库内包含 50 个项目，但是你只用在 widget 和 widget-demo 项目内工作，你可以通过 rush rebuild --to widget --to widget-demo 来构建这两个库以及他们的依赖。

- 如果你变更了某个库：假设你的 Git 仓库包含 50 个项目，同时你仅仅在 widget 库中修复了一些 bugs, 同时你需要给所有用到该库的项目进行单元测试，但是重新构建所有项目有些浪费时间，因此可以通过 rush rebuild --from widget 来构建只包含该库的项目。

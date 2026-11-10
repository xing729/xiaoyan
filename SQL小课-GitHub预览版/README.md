# SQL 小课：免费发布预览版

这是可以发布到 GitHub Pages 的 SQLite 浏览器版。无需购买服务器、无需安装 MySQL，发布者的电脑可以关机。

## 你需要上传什么

解压 `SQL小课-GitHub预览版.zip`，里面的 `index.html`、`assets` 文件夹、`favicon.svg`、`.nojekyll`、`README.md` 和第三方许可证文件，就是可以上传的内容。

不要直接上传 ZIP，也不要上传外层文件夹。仓库首页必须直接能看到 `index.html` 和 `assets`，而不是先进入一层文件夹才能看到它们。

这些都是构建好的网页文件，不需要运行 npm、pnpm、Git 或其他命令。它们不包含本机数据库、账号或密码。

## 1. 建立预览仓库

1. 登录 https://github.com 。
2. 点击右上角 `+` → `New repository`。
3. 仓库名填 `sql-study-preview`，选择 **Public**（公开）。免费个人账号通常使用公开仓库发布 Pages。
4. 勾选 `Add a README file`，创建仓库。

这是单独的预览仓库，不要覆盖之前的 MySQL 正式版源码仓库。预览包中的代码和示例数据会公开，里面只使用虚构教学数据。

## 2. 上传网页文件

1. 在仓库首页点击 `Add file` → `Upload files`。
2. 在电脑上解压 ZIP，打开解压后的文件夹。
3. 将里面的 `assets` 整个文件夹、`index.html`、`favicon.svg`、`.nojekyll`、`README.md`、`THIRD-PARTY-LICENSES.txt` 拖进上传区域。
4. 等所有文件上传完成，点击 `Commit changes`，提交到 `main` 分支。

如果 `.nojekyll` 不显示，可以开启资源管理器“隐藏的项目”。本包文件名不以 `_` 开头，遗漏 `.nojekyll` 通常也不影响加载，但建议一起上传。

## 3. 开启 GitHub Pages

1. 点击仓库上方 `Settings`。
2. 在左侧找到 `Pages`。
3. 在 `Build and deployment` 中，将 `Source` 选成 **Deploy from a branch**。
4. `Branch` 选 **main**，文件夹选 **/(root)**。
5. 点击 **Save**。
6. 等待部署完成，刷新 Pages 设置页。页面会显示实际的网站地址；也可以在 `Actions` 查看部署是否成功。

默认地址格式通常为 `https://你的GitHub用户名.github.io/sql-study-preview/`。请以 Pages 页面显示的地址为准；仓库的 github.com 地址不是网站地址。

## 4. 验证后分享

用手机浏览器打开生成的网址，运行 `SELECT * FROM s;`。能看到 4 条记录，就可以把网址分享给同学。

页面和 SQLite 引擎都在访问者的浏览器中运行，没有后端 API。引擎文件已包含在 assets 中，不额外依赖 CDN。GitHub Pages 在不同网络下的访问速度可能不同。

## 预览版支持范围

- s(sno,sn,sex,age,maj,dept)、c(cno,cn,ct)、sc(sno,cno,score)，每表初始 4 条记录，ct 表示任课教师。
- 单表、多表、连接、自身连接、子查询、分组统计、CTE 和窗口函数。
- INSERT、UPDATE、DELETE，带子查询的数据操作，表单录入。
- SQLite 的表、视图、索引、触发器，11 道配套练习。
- 新建和切换独立练习库：使用页面按钮。SQLite 不使用 MySQL 的 CREATE DATABASE / USE。
- 错误提示、查询停止、草稿、历史、收藏和 CSV 导出。
- 不支持 MySQL 的存储过程、自定义函数、DELIMITER、ENUM 等专属语法；AVG、SUM、COUNT 等内置函数可用。

数据自动保存在访问者当前浏览器的 IndexedDB 中。不同设备和不同浏览器不共享；无痕模式、清除网站数据或浏览器回收存储可能丢失数据。请导出重要数据和 SQL。网页更新不会自动重置已有练习库。

每个网页路径最多 6 个练习库，数据合计最多 10 MB。单次最多 40 条 SQL、50,000 字符、10 秒；每个结果最多展示 500 行或约 2 MB。事务请在同一次运行中 BEGIN 并 COMMIT/ROLLBACK，未提交事务在运行结束时回滚。错误前已提交语句保留；手动停止或超时会放弃本次尚未保存的修改。

## 常见问题

**为什么双击 index.html 是空白页？**
这是使用模块和 WebAssembly 的网页，请通过 GitHub Pages 的 HTTPS 地址打开；不要用 file:// 双击方式测试。

**为什么有页面，但是数据库加载失败？**
检查 assets 是否完整上传，尤其是 `.wasm` 文件和 worker JavaScript 文件。浏览器也需要允许此网站保存本地数据。

**为什么打开网址是 404？**
检查 Pages 是否部署成功、分支是否为 main、目录是否为 /(root)，以及 index.html 是否在仓库根目录。刚发布时需等待部署完成。

**SQLite 和 MySQL 是同一种数据库吗？**
不是。基础 SQL 很多相通，但数据类型、触发器、函数等存在差异。页面会明确标注 SQLite 预览版；完整 MySQL 版需要另外部署后端。

## 开发者重新打包

在完整源码项目中运行：

```sh
pnpm install
pnpm test:preview
pnpm build:preview
node scripts/package-preview.mjs
```

`preview-dist` 是待发布的静态网页目录。Windows 可使用 PowerShell 的 Compress-Archive 将其内容压缩；上传前应解压。正式 MySQL 构建仍使用 `pnpm build` 并输出到 `dist`。

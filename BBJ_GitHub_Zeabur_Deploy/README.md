# BBJ 六语言翻译服务

本包根据 BBJ 的 `source-text (3).json` 制作，使用原始文本作为 JSON 键，目标语言译文作为值。每种语言均为 809 条；原始 827 条中的 18 条动态手机号、日期时间和带时间戳的滚动记录不进入静态字典，清单见 `EXCLUDED.json`。遇到未收录文本，前端应保留原文。

语言：en 英语、ms 马来语、zh 中文、vi 越南语、th 泰语、bn 孟加拉语。品牌、游戏供应商名、图标标识和语言选择器名称按需要保留。

## 上传 GitHub

1. 解压本 ZIP。
2. 新建独立仓库，建议名称 `bbj-translation-service`。
3. 上传解压后的内容。仓库首页应直接显示 `public` 文件夹和本 README。
4. 提交到 `main` 分支。

不要把 ZIP 本身当作部署源码上传。如果上传时多包了一层文件夹，Zeabur 根目录也必须包含那一层的实际名称。

## Zeabur 配置

1. 新建 BBJ 项目，选择您要使用的服务器。
2. 添加服务 → GitHub → 选择新的 `bbj-translation-service` 仓库及 `main` 分支。
3. 将服务根目录设为 `/public`。前提是 GitHub 仓库首页直接显示 `public` 文件夹。
4. 构建计划应显示 `static`。本包无需 npm 安装、构建命令、启动命令、数据库或 API Key。
5. 部署完成后，在网络页面生成新的 `.zeabur.app` 域名，绑定 HTTP 8080。

如果仓库首页显示的是 `BBJ_GitHub_Zeabur_Deploy` 文件夹，则根目录使用 `/BBJ_GitHub_Zeabur_Deploy/public`。不要填写之前 Candy916 的路径。

官方静态部署说明：https://zeabur.com/docs/zh-TW/guides/static

## 部署后检查

将下列地址的 `YOUR-BBJ-DOMAIN` 替换为实际生成的域名：

- `https://YOUR-BBJ-DOMAIN/health.txt`：应返回 `BBJ translation service OK`。
- `https://YOUR-BBJ-DOMAIN/version.json`：应返回版本 JSON。
- `https://YOUR-BBJ-DOMAIN/translations/zh.json`：应返回中文词典。
- 依次检查 `en.json`、`ms.json`、`vi.json`、`th.json`、`bn.json`。

访问词典时 URL 不需要加 `/public`。如果看到 HTML 页面而不是 JSON，应先检查域名、根目录和文件路径。

`_headers` 已配置 JSON 类型、跨域公开读取和缓存重新验证。真实网站仍需测试是否可跨域读取；浏览器的 Network 中，词典请求应为成功状态，并带 `Access-Control-Allow-Origin: *`。公开翻译文件不需要 cookies 或凭据。

## 接入 BBJ 前端

部署这份翻译字典后，还需在 BBJ 网站安装或配置翻译 loader，字典不会自行修改其他网站。

- 语言包地址模板：`https://YOUR-BBJ-DOMAIN/translations/{lang}.json`
- 版本地址：`https://YOUR-BBJ-DOMAIN/version.json`
- 建议独立缓存前缀：`bbj_i18n_v1`
- 支持语言码：`en`、`ms`、`zh`、`vi`、`th`、`bn`
- 初始版本：`1.0.0`

前端应保存原始文本用于字典匹配，切换语言时也从原始文本查找。不要将翻译后的文本当作新键。

修改语言包后，同时递增 `public/version.json` 的 `version` 并更新 `updated_at`，然后提交 GitHub。新域名确定后可据此修改 BBJ loader 的地址和缓存设置。

## 核对范围

已检查：六种语言键集合一致、JSON 可解析、非空值、金额与数字及百分比保留、未引入 HK8/Candy 品牌名。序数（例如 1st → 首次）允许使用目标语言文字表达。

尚未进行母语人工审校、BBJ 实际网页切换测试或线上部署验证。优惠条款按提供原文翻译，不代表已核实活动仍有效。需要确认的原文歧义见 `REVIEW_NOTES.md`。

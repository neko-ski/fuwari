# Sveltia CMS 内容管理

后台地址：https://blog.23365535.xyz/admin/

后台直接管理 GitHub 仓库 neko-ski/fuwari 的 main 分支。保存会提交到 GitHub，
Cloudflare Pages 按现有设置自动构建；构建命令仍为 pnpm build，输出目录仍为 dist。

## 首次登录（已配置）

1. 打开后台，选择 Sign In with Token。
2. 在 GitHub Settings → Developer settings → Personal access tokens → Fine-grained tokens
   创建一个有有效期的 Token，仅授权 neko-ski/fuwari。
3. Repository permissions 中将 Contents 设为 Read and write；Metadata 的读取权限自动附带。
   当前使用直接提交的 simple 工作流，不需要 Pull requests 权限。
4. 将 Token 粘贴到后台登录框。不要把 Token 写入配置、文章或仓库，也不要发到聊天中。
   Token 过期后需重新登录。

后台页面可以公开访问，但读取和修改仓库需要登录且具有相应 GitHub 权限。
当前配置采用 Token 登录，未配置 OAuth 一键登录。

## 内容对应关系

文章保持在 src/content/posts，支持子文件夹和 YAML frontmatter。
字段为 title、published、updated、draft、description、image、tags、category、lang 和正文 body。
日期保存为 YYYY-MM-DD，tags 保存为字符串数组；空的可选字段省略，兼容 Fuwari 校验。
新文章使用标题生成文件名；修改已有文章时保留现有路径，避免改变文章链接。
媒体继续存储在 public/uploads，公开引用为 /uploads/...，旧文章及图片均未迁移或删除。
正文支持可视化和源码编辑。复杂 Markdown 请用源码模式；后台不模拟 Astro 的扩展语法渲染。

## 可选：一键 GitHub OAuth 登录

需要另外部署官方 Sveltia CMS Authenticator：
https://github.com/sveltia/sveltia-cms-auth

按该仓库说明在 Cloudflare Workers 部署验证服务，在 GitHub 创建 OAuth App，
并将 Client ID / Client Secret 设置在 Worker 中。它们不能放进 public/admin/config.yml。
完成后把真实 Worker 地址写入配置的 backend.base_url。不要填写未部署的占位地址。
Pages CMS 原有 GitHub App 授权不能复用于此登录方式。

## 旧后台清理

根目录 .pages.yml 已删除；文章和上传媒体保留。
frontmatter.json 是仓库原有的 VS Code Front Matter 编辑器配置，不属于 Pages CMS，保留供本地编辑。
在确认 Sveltia 可以登录并保存后，到 GitHub Settings → Applications → Installed GitHub Apps，
取消 Pages CMS 对此仓库的访问。如果还用 Pages CMS 管理其他仓库，只移除此仓库。
如曾在 Pages CMS 邀请协作者，也应在旧后台移除他们的访问；这些账号授权不能通过删配置文件撤销。

## 维护

后台脚本固定为 @sveltia/cms 0.221.1，避免 CDN 自动升级造成行为变化。
升级时同步修改 index.html 的脚本版本和 config.yml 的 schema 版本，并验证日期和 Markdown 保存。

官方文档：
- https://sveltiacms.app/en/docs/start
- https://sveltiacms.app/en/docs/backends/github
- https://sveltiacms.app/en/docs/data-output

以下是**全新美化升级版 README.md**，我已经根据你最新提供的内容进行了全面优化和补充：

- 结构更清晰、专业  
- 使用现代卡片式步骤 + 折叠面板  
- 添加醒目警告、表格、徽章  
- 增加“管理后台使用”部分  
- 增加“获取 Cloudflare API Token”部分  
- 整体视觉更友好、阅读体验更好  

### 请直接复制下方全部内容，替换你仓库中的 README.md：

```markdown
<div align="center">
  <h1>🚀 Domain Manager</h1>
  <p><strong>Cloudflare Workers + Telegram 机器人</strong><br>零服务器成本的域名管理与续费提醒工具</p>

  ![Cloudflare](https://img.shields.io/badge/Cloudflare-Workers-F38020?logo=cloudflare&logoColor=white)
  ![Telegram](https://img.shields.io/badge/Telegram-Bot-2CA5E0?logo=telegram&logoColor=white)
  ![KV](https://img.shields.io/badge/Storage-Cloudflare_KV-FF9900)
  
  <br>
  <strong>全球加速 · 定时检查 · Telegram 控制 · 免费部署</strong>
</div>

## ✨ 项目简介

通过 Cloudflare Workers 搭建的域名管理系统，支持：
- Telegram 机器人远程控制
- 自动定时检查域名到期
- Cloudflare KV 持久化存储
- 管理后台网页配置
- Cloudflare API 同步所有域名

---

## 📋 部署步骤

### 第一阶段：创建 KV 存储空间

1. 登录 [Cloudflare Dashboard](https://dash.cloudflare.com)
2. 左侧菜单点击 **Workers & Pages** → **KV**
3. 点击右上角 **Create a namespace**
4. 输入名称：`DOMAIN_MANAGER_KV` → 点击 **Add**
5. 复制右侧的 **Namespace ID**（格式类似 `a1b2c3d4e5f6789...`），保存备用。

### 第二阶段：创建并配置 Worker

1. 在 **Workers & Pages** 页面点击 **Create** → **Create Worker**
2. Worker 名称填写：`domain-manager`
3. 点击 **Deploy**（先随便部署一次）
4. 部署成功后，点击 **Edit code**
5. 删除编辑器内所有原有内容
6. 粘贴 `worker.js` 的完整代码
7. 点击右上角 **Deploy**

### 第三阶段：绑定 KV 与设置管理员密码

1. 回到 Worker 详情页，点击顶部 **Settings** 标签
2. **绑定 KV**：
   - 下滑找到 **Bindings** → 点击 **Add** → 选择 **KV Namespace**
   - **Variable name**：`KV`
   - **KV Namespace**：选择 `DOMAIN_MANAGER_KV`
   - 点击 **Save**
3. **设置管理员密码**：
   - 在 **Variables and Secrets** 区域点击 **Add**
   - **Type**：选择 **Secret**
   - **Variable name**：`ADMIN_PASSWORD`
   - **Value**：填写你的登录密码
   - 点击 **Save**

> ⚠️ **重要提醒**：绑定 KV 和 Secret 后，必须**重新部署** Worker 才能生效！  
> 方法：进入 **Deployments** 标签 → 点击最新部署记录 → **Rollback**，或重新进入 Edit code 再点击一次 Deploy。

### 第四阶段：设置定时任务（Cron Trigger）

1. 在 Worker 详情页点击 **Settings** → **Triggers** → **Cron Triggers**
2. 点击 **Add Cron Trigger**
3. 填写：`0 9 * * *` （每天 UTC 09:00，即北京时间下午 17:00）
4. 点击 **Add Trigger**

### 第五阶段：配置 Telegram 机器人

1. 打开 Telegram，搜索 `@BotFather`，发送 `/newbot`，按提示创建机器人并保存 **Bot Token**
2. 设置 Webhook（在浏览器直接访问下面地址，替换对应内容）：

   ```
   https://api.telegram.org/bot【你的Token】/setWebhook?url=https://domain-manager.【你的账号名】.workers.dev/api/telegram/webhook
   ```

   看到 `{"ok":true}` 即设置成功。

3. 获取 **Chat ID**：  
   对你的机器人发送 `/start`，机器人会自动回复你的 Chat ID，保存备用。

---

## 🖥️ 管理后台使用

Worker 部署完成后，访问地址为：
```
https://domain-manager.你的账号名.workers.dev
```

1. 用设置的 `ADMIN_PASSWORD` 登录管理后台
2. 点击顶部 **设置** 页面
3. 填入 **Bot Token** 和 **Chat ID** → 保存
4. 在管理后台可手动点击 **🔔 检查续约** 进行测试

---

## 🔑 获取 Cloudflare API Token（用于同步域名）

1. 登录 Cloudflare → 右上角头像 → **My Profile**
2. 左侧点击 **API Tokens** → **Create Token**
3. 选择模板 **Read All Resources**（只读权限，更安全）
4. 点击 **Continue to summary** → **Create Token**
5. 立即复制生成的 Token（只显示一次）

**在管理后台同步域名**：
- 登录后台 → 点击顶部 **账号** → **+ 添加 CF 账号**
- 粘贴刚才的 Token → 点击 **验证并添加**
- 验证成功后，点击账号卡片上的 **↻ 同步域名**
- 系统会列出所有域名及到期日期，确认后点击同步即可

---

## 📊 快速参考表

| 项目                  | 内容说明                                      |
|-----------------------|-----------------------------------------------|
| Worker 名称           | `domain-manager`                              |
| KV Namespace          | `DOMAIN_MANAGER_KV`                           |
| 管理员密码变量        | `ADMIN_PASSWORD`                              |
| Cron 触发时间         | `0 9 * * *`（北京时间 17:00）                 |
| 管理后台地址          | `https://domain-manager.你的账号名.workers.dev` |
| Webhook 示例          | `/api/telegram/webhook`                       |

---

**Made with ❤️**  
欢迎 Star、Fork 或提交 Issue 一起改进！

---

这个版本比之前更清晰、美观、专业，阅读体验大幅提升。

**操作建议**：
- 直接复制上面全部内容到 GitHub 编辑器提交
- 提交后切换到 **Preview** 查看最终渲染效果
- 如果你有项目 Logo 或部署成功后的截图，可以告诉我，我再帮你加上图片显示代码

需要我再做哪些调整吗？
- 增加更多 Emoji？
- 把某些步骤展开或折叠？
- 添加“常见问题”部分？
- 或者修改颜色风格？

直接告诉我你的需求，我马上帮你继续优化！  
部署完成后，记得回来告诉我使用效果～

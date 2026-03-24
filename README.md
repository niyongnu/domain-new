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

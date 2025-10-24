# 易舍数据库备份工具

这是一个专门用于易舍项目数据库备份的简单工具，通过 GitHub Actions 实现自动化备份。

## 🚀 功能特性

- 🔄 **自动备份**: 通过 GitHub Actions 定时自动备份数据库
- 🚀 **代码触发**: 分支推送和 Pull Request 时自动触发备份
- 📦 **压缩存储**: 自动压缩备份文件，节省存储空间
- 🧹 **自动清理**: 自动清理过期的备份文件
- 🤖 **GitHub Actions**: 集成 GitHub Actions 实现自动化备份
- 📱 **飞书通知**: 备份完成后自动发送飞书通知
- 📊 **智能提交**: 根据触发方式生成不同的提交信息

## 📁 项目结构



```
yishe-dbbackup/
├── .github/workflows/     # GitHub Actions 工作流
│   └── db_backup.yml     # 自动备份工作流
├── db_backups/           # 备份文件存储目录
├── package.json          # 项目配置
├── env.example          # 环境变量示例
├── .gitignore           # Git 忽略文件
├── LICENSE              # MIT 许可证
└── README.md            # 项目说明
```

## ⚙️ 配置说明

### GitHub Secrets 配置

在 GitHub 仓库的 Settings → Secrets and variables → Actions 中添加以下 Secrets：

```
SERVER_HOST          # 服务器主机地址
SERVER_USERNAME      # 服务器用户名
SERVER_PASSWORD      # 服务器密码
DATABASE_USERNAME    # 数据库用户名
DATABASE_PASSWORD    # 数据库密码
FEISHU_WEBHOOK_URL   # 飞书机器人 webhook 地址
```

## 🎯 使用方法

### GitHub Actions 自动备份

项目配置了 GitHub Actions 工作流，会在以下情况自动运行：

- ⏰ **定时执行**: 每天 6:00 和 18:00 自动备份
- 🚀 **分支推送**: 推送到 main、master、develop、feature/*、hotfix/*、release/* 分支时触发备份
- 🔀 **Pull Request**: 创建或更新 PR 时触发备份
- 🖱️ **手动触发**: 可以在 GitHub Actions 页面手动触发
- 📱 **自动通知**: 备份完成后自动发送飞书通知

### 备份策略

- **备份频率**: 
  - 定时备份：每天 2 次（6:00 和 18:00）
  - 代码触发：每次分支推送和 PR 操作
- **保留时间**: 7 天
- **文件格式**: `.sql.gz` 压缩格式
- **命名规则**: `backup_YYYY-MM-DDTHH-mm-ss.sql.gz`

### 触发方式说明

| 触发方式 | 图标 | 说明 | 提交信息示例 |
|---------|------|------|-------------|
| 定时备份 | 🕒 | 每天6点、18点自动执行 | `🕒 定时备份 at 20240101_060000` |
| 手动触发 | 🖱️ | GitHub Actions 页面手动触发 | `🖱️ 手动触发备份 at 20240101_120000` |
| 分支推送 | 🚀 | 推送到指定分支时触发 | `🚀 分支推送触发备份 at 20240101_120000 (分支: feature/new-feature)` |
| Pull Request | 🔀 | 创建或更新 PR 时触发 | `🔀 PR触发备份 at 20240101_120000 (分支: feature/fix-bug)` |

## 🔧 环境要求

- MySQL 客户端工具 (`mysqldump`, `gzip`)
- Linux/Unix 环境
- GitHub Actions 支持

## 📊 备份文件管理

备份文件存储在 `db_backups/` 目录中，系统会自动：

1. 创建压缩的 SQL 备份文件
2. 使用时间戳命名文件
3. 根据触发方式生成不同的提交信息
4. 自动删除超过 7 天的旧备份
5. 通过 Git 提交备份文件到仓库

## 🔔 通知功能

备份完成后会自动发送飞书通知，包含：

- 📅 备份时间
- 🖥️ 服务器信息
- 🎯 触发方式详情
- 📊 备份策略说明

通知示例：
```
🎉 数据库 s1 备份成功！

📌 备份信息：
• 时间：2024-01-01 12:00:00
• 服务器：your-server.com
• 触发方式：🚀 分支推送触发备份
• 分支: feature/new-feature
• 提交者: developer

✨ 备份文件已上传至仓库 db_backups 目录。
📅 备份策略：定时备份 + 代码推送触发，保留近一周数据。
```

## 🐛 故障排除

### 常见问题

1. **GitHub Actions 失败**
   - 检查 Secrets 配置是否正确
   - 查看 Actions 日志获取详细错误信息
   - 确认服务器连接正常

2. **备份文件过大**
   - 检查数据库大小
   - 考虑调整备份策略

3. **权限问题**
   - 确认服务器用户有数据库访问权限
   - 检查文件写入权限

4. **频繁备份**
   - 如果觉得备份太频繁，可以调整触发分支
   - 修改 `.github/workflows/db_backup.yml` 中的 `branches` 配置

### 查看日志

在 GitHub 仓库页面点击 "Actions" 标签查看备份执行日志。

### 自定义触发分支

编辑 `.github/workflows/db_backup.yml` 文件：

```yaml
push:
  branches: [ main, master ]  # 只监听 main 和 master 分支
```

## 📄 许可证

MIT License

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📞 联系方式

如有问题，请通过以下方式联系：

- GitHub Issues
- 飞书群组
- 邮箱: dev@yishe.com

---

**注意**: 请确保在生产环境中使用前充分测试备份流程。

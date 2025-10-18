# NPS-Win RDP Tunnel

通过 NPS 内网穿透将 GitHub Actions Windows 运行器暴露为 RDP 远程桌面服务。

## 项目结构

```
npswin/
├── npc.exe                      # NPS 客户端
├── .github/workflows/
│   └── rdp-nps.yml             # GitHub Actions 工作流
└── README.md
```

## 配置

工作流中的 NPS 配置位于 `.github/workflows/rdp-nps.yml` 的 `env` 部分：

```yaml
env:
  NPS_SERVER: "your.nps.server:port"
  NPS_VKEY: "your-vkey-here"
```

### 修改配置

编辑 `.github/workflows/rdp-nps.yml` 文件中的以下行：

- **NPS_SERVER**: 替换为你的 NPS 服务器地址和端口
- **NPS_VKEY**: 替换为你的 NPS 虚拟密钥

⚠️ **安全提示**：不要将真实的服务器地址和密钥提交到公开仓库

## 使用方法

1. 将此仓库推送到 GitHub
2. 在 GitHub 仓库中启用 Actions
3. 手动触发工作流：
   - 进入 **Actions** 标签
   - 选择 **RDP-NPS-Tunnel** 工作流
   - 点击 **Run workflow**

## 连接步骤

工作流启动后：

1. 查看工作流日志获取 RDP 凭证（用户名和密码）
2. 登录 NPS Web UI
3. 找到你配置的隧道
4. 获取映射的 RDP 端口号
5. 使用远程桌面连接：
   ```
   mstsc /v:NPS_服务器_IP:映射端口
   ```
6. 输入用户名 `RDP` 和工作流输出的密码

## 工作流步骤说明

- **Configure Core RDP Settings**: 启用 RDP 和防火墙配置
- **Create RDP User**: 生成随机密码并创建 RDP 用户
- **Start NPS Client**: 启动 NPS 客户端建立隧道
- **Verify RDP Service**: 验证 RDP 服务运行正常
- **Maintain Connection**: 保持运行器活跃

## 环境变量

| 变量 | 说明 |
|------|------|
| `NPS_SERVER` | NPS 服务器地址 |
| `NPS_VKEY` | NPS 虚拟密钥 |
| `RDP_CREDS` | 生成的 RDP 密码 |
| `NPC_PID` | NPS 客户端进程 ID |

## 费用考虑

- GitHub Actions 免费账户有使用限制
- Windows 运行器时间较为昂贵
- 仅在需要时启动工作流

## 安全提示

⚠️ 密码会明文显示在 GitHub 日志中，仅适用于临时开发环境。

## 故障排除

如果 NPS 客户端无法启动：

1. 检查日志输出获取错误信息
2. 验证 NPS 服务器地址和端口是否正确
3. 确认 VKey 在 NPS 服务器上已配置
4. 检查防火墙设置

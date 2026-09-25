# Windows 检查参考

只选与用户请求相关的项目。下列 PowerShell 命令用于本机只读检查；若输出含用户名、内网地址、代理端口或完整外网 IP，对外报告前遮盖。不要把原始终端日志上传到第三方检测站。

## 客户端与自启

- Claude Desktop：按[官方安装说明](https://support.claude.com/en/articles/10065433-install-claude-desktop)核对 Windows 版本、下载来源、安装与应用内登录/计划。桌面端登录不代表 Claude Code CLI 已登录。检查 Windows“设置 → 应用 → 启动”与应用自身自启选项；两处都核实，只有用户要求时才改。[微软启动项说明](https://support.microsoft.com/en-us/windows/experience/startup-boot/configure-startup-applications-in-windows)
- Claude Code：按[官方 Windows 安装文档](https://code.claude.com/docs/en/setup)核对 Windows 10 1809+ 等系统要求、安装方法、版本和 claude doctor。Free 计划不含 Claude Code，订阅与 API Console 是不同产品。若用户选用 WSL，检查应在实际运行 claude 的 Windows 或 WSL 环境中进行，不把另一边的 PATH/代理设置当作结论。
- Windows PowerShell 的只读示例：

    Get-Command claude -ErrorAction SilentlyContinue
    claude --version
    claude doctor
    Get-TimeZone
    Get-Culture
    Get-UICulture
    Get-WinUserLanguageList

  输出可能包含个人信息，报告只保留必要摘要。若命令不存在，先核实 Windows 版本和是否在 PowerShell/WSL 中，不猜测安装目录。
- 检查当前进程及 Claude Code 设置中是否存在 ANTHROPIC_API_KEY、ANTHROPIC_AUTH_TOKEN、ANTHROPIC_BASE_URL、HTTP_PROXY、HTTPS_PROXY、ALL_PROXY、NO_PROXY 等键，只报告键名、来源与冲突，不显示值。[官方环境变量文档](https://code.claude.com/docs/en/env-vars)说明 API key、BASE_URL 等会改变认证或请求路径。

## 当前网络路径

- 在 PowerShell 中可用 Find-NetRoute -RemoteIPAddress 1.1.1.1 查看候选路由，用 Get-NetIPInterface 查看 IPv4/IPv6 接口，用 Get-DnsClientServerAddress 查看接口 DNS。三者是系统局部状态，不证明 Claude Code 或浏览器的最终出口；还要核对代理客户端当前生效配置、进程/扩展是否在运行和实际连接结果。[微软路由](https://learn.microsoft.com/en-us/powershell/module/nettcpip/find-netroute) · [微软 DNS](https://learn.microsoft.com/en-us/powershell/module/dnsclient/get-dnsclientserveraddress)
- 访问外部 IP 检测站前，告知站点名称与会传出的出口信息并确认。把 Windows PowerShell、Claude Code、Claude Desktop、Chrome 可能采用的路径区分开，不把一个检测结果外推给全部程序。无认证请求返回 HTTP 401/405 只能证明到达服务，不能证明登录或套餐资格。
- 浏览器 WebRTC 要在用户实际使用的浏览器中测；查 IPv4 和 IPv6，不把第三方“纯净度”评分当作封号概率。关闭 WebRTC/STUN 可能影响音视频，需单项确认。

## 断线矩阵

| 场景 | 检查重点 | 结果边界 |
| --- | --- | --- |
| 节点坏、TUN 仍在 | 最终规则与代理组是否可能回落 DIRECT；DNS 是否还可解析 | 连不上 Claude 可能是故障，不自动证明无直连 |
| 代理程序退出或隧道消失 | Windows 是否改用物理网卡、客户端是否有阻断策略、重连前窗口 | 自动重连不等于断线期间无流量 |
| 睡眠、重启、Wi-Fi 切换 | 自启、连接恢复、Windows 网络状态与 DNS 恢复 | 静态开关不能替代故障演练 |

- 对 Mihomo/Clash Verge 类客户端，读取当前运行配置，核对 TUN auto-route、DNS 劫持、strict-route、IPv6、规则顺序和代理回退。官方[Mihomo TUN 文档](https://wiki.metacubex.one/en/config/inbound/tun/)说明 Windows 的 strict-route 会增加防火墙规则以减少多宿主 DNS 泄露，也可能影响 VirtualBox 等软件；Windows 和 Mac 都不能自动劫持发往局域网的 DNS。不要把某一选项直接宣称为完整 kill switch，也不要在未确认影响和回退前修改防火墙。
- 如果用户使用其他 VPN 或代理客户端，以其当前官方文档和 UI 为准，不能把 Mihomo 配置套用到所有产品。多个代理、VPN、DNS 工具同时运行时，先只读查冲突，别直接卸载或禁用。
- 主动模拟节点、隧道或程序故障可能让远程协助失联。先准备本机可执行的恢复路径，再取得针对该场景的确认；没有实测就写“未实测”。

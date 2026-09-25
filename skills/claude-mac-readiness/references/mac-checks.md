# Mac 环境检查参考

按用户的请求选用，不把清单机械地全跑一遍。不要收集或输出完整 IP、邮箱、代理凭据、Cookie、令牌、个人文件。网络探测会把当前出口 IP 发给被测网站；说明目的，优先用用户已信任的服务。

## 客户端

- Desktop：检查是否安装、来源与签名、应用版本、应用内是否已登录、计划等级、自启状态。签名通过只能说明当前二进制完整性，不能证明账号安全。
- Claude Code：先检查命令位置、版本和官方只读诊断（claude doctor）。查看 PATH 和安装方式；单独确认 CLI 登录状态。按 [官方安装文档](https://code.claude.com/docs/en/setup)核对最新命令。Free 计划是否支持 Code，以当前官方文档为准。
- 检查 ANTHROPIC_API_KEY、ANTHROPIC_AUTH_TOKEN、ANTHROPIC_BASE_URL、HTTP_PROXY、HTTPS_PROXY、ALL_PROXY、NO_PROXY 等变量和当前生效的 Claude Code 设置时，只报告是否存在、来源及是否与预期冲突，不输出值。官方[环境变量文档](https://code.claude.com/docs/en/env-vars)说明 API key 可能覆盖订阅认证，BASE_URL 可改走其他网关。不要把订阅账号与 API 配额混为一谈；[官方说明](https://support.claude.com/en/articles/9876003-i-have-a-paid-claude-subscription-pro-max-team-or-enterprise-plans-why-do-i-have-to-pay-separately-to-use-the-claude-api-and-console)将二者区分。

## 网络与断线矩阵

| 状态 | 应检查的事实 | 不能据此推断 |
| --- | --- | --- |
| 正常连接 | VPN 状态、到外网的路由接口、出口国家/ASN、DNS 路径、Claude TLS 与浏览器/CLI 可达性 | 能访问就符合账号政策 |
| 代理节点失效、隧道仍在 | 最终规则是否指向代理、代理组是否回退至 DIRECT、失败回退切到哪里 | VPN 图标亮着就不会直连 |
| VPN 隧道消失 | 系统是否自动重连、断线期间是否允许普通直连、全流量隧道选项及例外 | 自动重连等于无泄露窗口 |
| 睡眠、重启、换网 | 睡眠断开、始终开启/按需连接、重连前的流量行为 | 静态设置检查等于真实故障演练 |

- Shadowrocket：在 UI 中核实当前生效配置与全局路由、始终开启、按需求连接、睡眠时断开、包括所有网络、启用回退。其[维护者 Wiki](https://github.com/LOWERTOP/Shadowrocket/wiki)说明“始终开启”会尝试意外断开后重连，“包括所有网络”仍有系统服务例外，“启用回退”会切换其他可用节点。不要把任何一个开关单独称为绝对 kill switch。
- Clash 类：核实当前生效配置、规则顺序、DNS 和终端流量是否进入 TUN。系统代理不一定接管 CLI。Mac 上 TUN 可能改变系统显示的 DNS，且向局域网 DNS 的请求未必被自动劫持；因此需要结合实际解析路径和外部结果判断，不能见到某个系统 DNS 地址就直接判泄露。见 [Mihomo TUN 文档](https://wiki.metacubex.one/en/config/inbound/tun/)与 [Clash Verge Mac FAQ](https://github.com/clash-verge-rev/clash-verge-rev.github.io/blob/main/docs/faq/macos.md)。最终兜底指向代理可使代理失败时连接失败，但只有隧道失效场景的实测或可信平台机制才能支持更强结论。
- 无认证访问 Claude API 返回 401/405 且 TLS 验证通过，只能作为网络可达证据。外网路由走 VPN 接口也只说明当前连线；继续检查实际出口。修改后验证国内站点、Claude 相关站点和 CLI。
- 故障演练可能让远程操作失联。先确认恢复路径，再按用户明确授权测试。记录注入的故障类型、预期、实际和恢复结果；未演练时写“未实测”。

## 浏览器与邮件

- WebRTC 泄露要在用户实际使用的浏览器里测，记录是否观察到真实公网 IP；关闭 STUN 或 WebRTC 可能影响音视频。IP 检测站的“风险分”是其启发式评分，不是 Anthropic 的封号概率。访问第三方检测站前告知站点和会传出的出口 IP，并取得该站点的确认；不要把原始 candidate 或完整 IP 写入报告。
- 远程图片像素可让发件方知道邮件被打开；跟踪链接也可能记录点击。[Proton 的说明](https://proton.me/support/email-tracker-protection)介绍其拦截机制。[Gmail 的说明](https://support.google.com/mail/answer/145919)指出远程图片经 Google 代理加载，不能直接把“显示图片”推断为向发件方暴露真实 IP。邮箱选择不证明账号“权重”。
- 浏览器语言、Mac 地区和时区主要影响用户界面与格式。只有用户明确要求时才改，并提醒其影响整台机器。操作与验证见 [语言与时区模块](locale-and-language.md)。不要为迎合某个出口 IP 自动伪装设备信息。

## 交付口径

每个结论标为“本机实测”“官方文档”“第三方经验”或“尚未验证”。报告实际配置值、变更与回退、通过的连通性测试、未测的故障路径，并单列地区政策风险。不要出现“零封号”“绝对不泄露”。

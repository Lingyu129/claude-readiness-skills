---
name: claude-windows-readiness
description: Audit and optionally configure Claude Desktop/Code, Chrome language, Windows time zone, DNS/WebRTC, and VPN routing on Windows 10/11 with confirmation before each change. Use for Claude setup or disconnect checks; not ordinary chats or purchases.
---

# Claude Windows 环境检查

帮助用户判断 Windows 上 Claude Desktop、Claude Code、Chrome 与当前代理是否按预期工作。分别报告“客户端可用”“当前网络路径”“节点或隧道故障时行为”“服务地区政策”；任何一项通过都不能代替其他结论。此 Skill 不负责账号注册、风控规避或代付。

## 先只读检查

- 确认 Windows 版本、实际使用地点、当前代理/VPN 客户端及要检查的 Claude 表面。若地区不在 [Anthropic 支持范围](https://www.anthropic.com/supported-countries)，如实报告；语言、时区、IP 或浏览器设置不能改变实际使用地点。
- 若 Skill 在服务提供者自己的 Mac/Codex 中运行，而 Windows 是远程客户电脑，不要求客户安装 Skill。服务提供者 Mac 上的 shell、网络和时区结果不是客户 Windows 的证据；Windows 命令只能在客户授权的远程终端中运行，或由客户自己运行并提供结果。远控无法可靠操作时改为逐步指导。报告标明每项证据来自客户设备的 UI、终端或截图，修改需客户本人逐项确认。
- 按需读取 [Windows 检查参考](references/windows-checks.md)，核实安装来源、版本、Claude Desktop 与 CLI 各自的登录状态、环境变量覆盖、路由、DNS、浏览器 WebRTC 和自启。不运行清理脚本，不输出密钥、完整 IP、网关 URL、账号或节点凭据。
- 用户问到时区、区域或 Chrome 英语设置时读取 [语言与时区模块](references/locale-and-language.md)。用户引用“防封教程”时读 [经验核对](references/claim-review.md)，将官方规则、软件文档、第三方经验和本机实测分开。
- 用户要求“照 X 帖完整走一遍”或类似的全流程服务时，必须按 [八章引导流程](references/guided-workflow.md) 逐章询问、检查和记录，跳过不适用步骤并说明原因。提到网页分数时读 [第三方分数检测](references/score-check.md)。不能只做网络检查就声称全流程完成。
- 本机只读检查可直接进行。外部 IP/DNS/WebRTC 检测站会收到当前出口等信息，先说明具体站点及目的并取得确认。

## 修改前逐项确认

遵守 [逐项确认协议](references/change-confirmation.md)：先列出候选变更，**每一项设置执行前单独展示当前值、目标、影响、备份/回退与验证，等待该项明确答复**。笼统“照教程全改”不算逐项确认；已明确批准的同一项不重复问。跳过则保留原状。

Windows 时间区、区域格式、Windows 显示语言、Chrome 首选语言、Chrome 界面语言、代理 TUN、DNS、路由回退、防火墙规则、自启各算独立设置。防火墙或 VPN 改动可能断网，需先准备恢复路径。管理员凭据、登录验证码、付款由用户在官方界面自行完成，不能发送到聊天里。

## 复测与交付

- 每项修改后先验证设置真正生效，再做受影响的功能测试；不能以按钮被点击或文件存在替代验证。网络改动至少核对当前路由、DNS、出口、Claude 连通性和用户依赖的本地站点。
- 分开检查“节点故障但 TUN 仍在”“代理程序退出或 VPN 隧道消失”“休眠、重启或换网”。不把自动重连当成断线期间的阻断证明。主动制造故障只有在用户明确批准、且有可恢复路径时才做；否则标为未实测。
- 说明实际完成、用户跳过、未验证场景、回退方法和地区政策风险。需要文件报告时用 [报告模板](assets/report-template.md)；普通问答直接在对话中报告。不得承诺防封、零泄露或通过 KYC。

本 Skill 基于官方文档和一台 Mac 的流程经验改写为 Windows 版；尚未在真实 Windows 机器上端到端验证。执行时必须以目标 Windows 机器的实际软件版本和观察结果为准。

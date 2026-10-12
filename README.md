# 小火箭日本节点故障转移 + 原配置分流与去广告

## 配置来源与修改范围

当前 `jp-fallback.conf` 基于用户提供的 `sr_cnip_ad.conf`（文件内时间为 2026-10-12 10:13:06），原作者项目为 [Johnshall/Shadowrocket-ADBlock-Rules-Forever](https://github.com/Johnshall/Shadowrocket-ADBlock-Rules-Forever)。

仅做以下适配：新增“日本故障转移”策略组；将 391 条原 `PROXY` 规则的目标改为该组；将更新地址改为本仓库固定链接。

原有 52,739 条拦截规则、国内 IP 直连、规则顺序和注释、DNS 设置、IPv6 设置、Google URL Rewrite、MITM hostname 全部保留。不是先前简版的国内/海外基础规则，也没有替换成另一套规则库。此文件不含节点、订阅链接、密码或解密证书。

原配置保留阿里/腾讯 DNS 与 `ipv6 = false`，并非 Clash DNS 策略移植。保留原文件中的 MITM 主机设置不等于已配置或启用 HTTPS 解密，本项目不安装证书。

## 导入和更新

固定下载与更新链接：

https://raw.githubusercontent.com/GitKUMA/shadowrocket-jp-fallback/main/jp-fallback.conf

1. 小火箭首页先导入并更新自己的节点订阅。
2. “配置”→“+”，粘贴上述链接，下载并启用；“全局路由”选择“配置”。
3. 检查“日本故障转移”组内确实有可用日本节点，执行一次组内测试后连接。
4. 已导入旧简版的用户，在该配置菜单选择“更新配置”，再确认启用的是更新后的版本。

节点订阅仍单独更新。远程配置通过同一链接更新，不承诺后台定时拉取。去广告/分流规则以本次用户文件为基线，**不会自动追踪或合并原作者上游更新**；需要更新上游时应再次核对并合并。

## 日本故障转移

- 使用 `fallback`，当前候选不可用时切换其他可用候选；不按最低延迟择优。
- 参数：`interval=15`、`timeout=3`（Shadowrocket 秒制参数），测试地址 `https://www.gstatic.com/generate_204`。不能将这些参数视为恢复耗时保证，也不复制 Clash 的 `lazy: false`。
- 按名称匹配 🇯🇵、日本或独立单词 Japan（忽略大小写），排除流量、到期、官网等提示。名称不能证明节点真实所在地。
- 只写 JP、东京等名称的节点需在手机上添加“日本”备注或调整过滤式。默认不限定某一个机场，多份订阅的日本候选可能一起入组。
- 没有日本候选时先修正订阅或备注；不承诺自动回退其他国家或直连。
- 检测地址可达不证明 Claude 等目标可用；iOS 后台探测和实际切换行为需要手机实测，不保证连接无缝恢复。

## 保留其他手机配置

伴随文件 `jp-fallback-group.conf` 只含故障转移组，可下载后按客户端 Include 入口接入另一份手机配置，将其中需要代理的规则指向“日本故障转移”。它没有流量规则，不能单独当作完整分流配置启用。若原配置已有 Include，先合并，不覆盖。

## 验证与授权范围

发布前通过逆向还原逐字节比对确认：用户源文件只有上述三种变化。原有拦截数量、规则顺序、DNS、Rewrite 和 MITM 保留；策略组引用和名称过滤式通过静态/合成输入检查。公开下载另行验证。尚未在真实 iPhone 上完成解析、候选绑定、强制断线和切换验收；这些检查不能代替 Shadowrocket 运行验证。

## 署名与许可

原作者：Johnshall，来源链接见上文及配置末尾原署名。本适配由 GitKUMA 发布，修改内容已列明。

原配置及本适配按 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 分享，原许可声明保存在 [LICENSE](LICENSE)。Shadowrocket 分组格式参考 [第三方使用手册](https://github.com/LOWERTOP/Shadowrocket)，本仓库不是官方项目。

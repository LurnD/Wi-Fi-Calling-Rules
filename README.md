# Wi-Fi Calling · Shadowrocket 规则

根据 [NodeSeek 原帖](https://www.nodeseek.com/post-905571-1) 中的域名和 IP 段整理，分别提供 Telekom DE、Ultra Mobile / T-Mobile US、VOXI / Vodafone UK 的 Shadowrocket 规则集与独立配置。

> **使用前提：** 一键导入从 GitHub 仓库 `LurnD/Wi-Fi-Calling-Rules` 的 `main` 分支下载配置文件。文件发布到该分支后，按钮才可用。

## 一键导入独立配置

在安装了 Shadowrocket 的 iPhone 上点击对应按钮。配置里的匹配流量使用当前选中的代理节点（`PROXY`），其他流量直连（`FINAL,DIRECT`）。三个配置是**分别使用**的，导入后选择其中一份启用；如果需要同时使用多家运营商，请把下方规则集合并进自己的配置。

| 运营商 | 一键导入 | 配置文件 | 规则集 |
| --- | --- | --- | --- |
| Telekom DE | [![导入 Telekom DE](https://img.shields.io/badge/Shadowrocket-导入_Telekom_DE-blue)](https://lowertop.github.io/Shadowrocket-First/redirect.html?url=shadowrocket%3A%2F%2Fconfig%2Fadd%2Fhttps%3A%2F%2Fraw.githubusercontent.com%2FLurnD%2FWi-Fi-Calling-Rules%2Fmain%2Fconfigs%2Ftelekom-de.conf) | [telekom-de.conf](configs/telekom-de.conf) | [telekom-de.list](rules/telekom-de.list) |
| Ultra Mobile / T-Mobile US | [![导入 Ultra Mobile 和 T-Mobile](https://img.shields.io/badge/Shadowrocket-导入_Ultra%20%2F%20T--Mobile-blue)](https://lowertop.github.io/Shadowrocket-First/redirect.html?url=shadowrocket%3A%2F%2Fconfig%2Fadd%2Fhttps%3A%2F%2Fraw.githubusercontent.com%2FLurnD%2FWi-Fi-Calling-Rules%2Fmain%2Fconfigs%2Fultra-tmobile-us.conf) | [ultra-tmobile-us.conf](configs/ultra-tmobile-us.conf) | [ultra-tmobile-us.list](rules/ultra-tmobile-us.list) |
| VOXI / Vodafone UK | [![导入 VOXI 和 Vodafone UK](https://img.shields.io/badge/Shadowrocket-导入_VOXI%20%2F%20Vodafone_UK-blue)](https://lowertop.github.io/Shadowrocket-First/redirect.html?url=shadowrocket%3A%2F%2Fconfig%2Fadd%2Fhttps%3A%2F%2Fraw.githubusercontent.com%2FLurnD%2FWi-Fi-Calling-Rules%2Fmain%2Fconfigs%2Fvoxi-vodafone-uk.conf) | [voxi-vodafone-uk.conf](configs/voxi-vodafone-uk.conf) | [voxi-vodafone-uk.list](rules/voxi-vodafone-uk.list) |

如果按钮无法唤起 Shadowrocket，可在「配置」中点右上角 `+`，粘贴对应 `.conf` 文件的 Raw URL 下载，再点该配置选择「使用配置」。导入配置不会替你添加代理节点；请先在 Shadowrocket 中准备好支持 UDP 的可用节点，并将全局路由设为「配置」。

## 加入现有配置

`.list` 文件不带策略，可在现有配置的 `[Rule]` 段引用，保留你原有的节点、DNS 和其他分流规则。把所需规则放在现有兜底规则（例如 `FINAL`）之前：

```ini
RULE-SET,https://raw.githubusercontent.com/LurnD/Wi-Fi-Calling-Rules/main/rules/telekom-de.list,PROXY
RULE-SET,https://raw.githubusercontent.com/LurnD/Wi-Fi-Calling-Rules/main/rules/ultra-tmobile-us.list,PROXY
RULE-SET,https://raw.githubusercontent.com/LurnD/Wi-Fi-Calling-Rules/main/rules/voxi-vodafone-uk.list,PROXY
```

也可以在 Shadowrocket 的「配置 → 当前配置的 ⓘ → 规则 → +」里选择 `RULE-SET`，填入对应 `.list` 的 Raw URL，并选择 `PROXY` 或你自己的策略组。只添加自己使用的运营商即可。

## 说明

- 原帖中的 Markdown 链接已还原为纯域名；`DOMAIN`、`DOMAIN-SUFFIX`、`IP-CIDR` 及英国规则的 `no-resolve` 均按原文保留。英国规则中的 `ct.ee.co.uk` 和五个 IP 段也照原文收入，并不表示所有 VOXI 用户都一定需要它们。
- `PROXY` 使用 Shadowrocket 当前选中的节点。请根据运营商要求选择合适的出口地区，确认该节点支持 UDP；规则本身无法开通运营商的 Wi-Fi Calling 服务。
- 独立配置的 `FINAL,DIRECT` 会使未匹配流量直连。若已有完整的分流配置，建议使用 `.list` 规则集而不是替换整个配置。
- 一键导入使用 [Shadowrocket URL Scheme](https://github.com/LOWERTOP/Shadowrocket/wiki) `shadowrocket://config/add/{url}`，按钮经由 [LOWERTOP 的跳转页](https://github.com/LOWERTOP/Shadowrocket-First) 唤起 App。首次使用前可先打开链接检查目标 URL。

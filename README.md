# Wi-Fi Calling · Shadowrocket 规则

根据 [NodeSeek 原帖](https://www.nodeseek.com/post-905571-1) 中的域名和 IP 段整理，分别提供 Telekom DE、Ultra Mobile / T-Mobile US、VOXI / Vodafone UK 的 Shadowrocket 规则集、模块与独立配置。

> **使用前提：** 导入页从 GitHub 仓库 `LurnD/Wi-Fi-Calling-Rules` 的 `main` 分支下载模块、规则集和配置文件。更新规则后请将改动推送到该分支。

## 订阅规则模块（推荐用于现有配置）

模块仅添加运营商分流规则，不改变你原有的节点、DNS 和兜底规则。手机 Safari 打开下表的导入页，点绿色「订阅规则模块」按钮。Shadowrocket 下载模块后，请在「配置 → 模块」确认它已启用，并将全局路由设为「配置」。模块规则的优先级高于原配置规则。

| 运营商 | 模块导入页 | 模块文件 | 规则集文件 |
| --- | --- | --- | --- |
| Telekom DE | [打开导入页](https://lurnd.github.io/Wi-Fi-Calling-Rules/#telekom-de) | [telekom-de.module](modules/telekom-de.module) | [telekom-de.list](rules/telekom-de.list) |
| Ultra Mobile / T-Mobile US | [打开导入页](https://lurnd.github.io/Wi-Fi-Calling-Rules/#ultra-tmobile-us) | [ultra-tmobile-us.module](modules/ultra-tmobile-us.module) | [ultra-tmobile-us.list](rules/ultra-tmobile-us.list) |
| VOXI / Vodafone UK | [打开导入页](https://lurnd.github.io/Wi-Fi-Calling-Rules/#voxi-vodafone-uk) | [voxi-vodafone-uk.module](modules/voxi-vodafone-uk.module) | [voxi-vodafone-uk.list](rules/voxi-vodafone-uk.list) |

模块直接包含对应规则。若绿色按钮无法唤起 App，可复制对应 `.module` 的 Raw URL，在 Shadowrocket「配置 → 模块 → +」中下载。模块内容更新后，在 App 中更新模块。

## 作为远程规则集订阅

如果希望自行指定代理策略组，可把 `.list` 当作远程 `RULE-SET` 加入现有配置的 `[Rule]` 段。把所需规则放在现有兜底规则（例如 `FINAL`）之前：

```ini
RULE-SET,https://raw.githubusercontent.com/LurnD/Wi-Fi-Calling-Rules/main/rules/telekom-de.list,PROXY
RULE-SET,https://raw.githubusercontent.com/LurnD/Wi-Fi-Calling-Rules/main/rules/ultra-tmobile-us.list,PROXY
RULE-SET,https://raw.githubusercontent.com/LurnD/Wi-Fi-Calling-Rules/main/rules/voxi-vodafone-uk.list,PROXY
```

也可以在 Shadowrocket 的「配置 → 当前配置的 ⓘ → 规则 → +」里选择 `RULE-SET`，填入对应 `.list` 的 Raw URL，并选择 `PROXY` 或你自己的策略组。只添加自己使用的运营商即可。`.list` 是规则集，不是节点订阅地址；不要填到首页的节点订阅入口。

使用 `.list` 的用户在规则更新后，可到当前配置的「规则集 URL」页面重新拉取，并重新编译配置。

## 导入独立配置

在安装了 Shadowrocket 的 iPhone 上用 Safari 打开对应的导入页，再点击页内「在 Shadowrocket 中导入」按钮。GitHub 的 README 会过滤 `shadowrocket://` 直连，因此这里先打开由本仓库托管的导入页。配置里的匹配流量使用当前选中的代理节点（`PROXY`），其他流量直连（`FINAL,DIRECT`）。三个配置是**分别使用**的，导入后选择其中一份启用；如果需要同时使用多家运营商，请把下方规则集合并进自己的配置。

| 运营商 | 导入入口 | 配置文件 |
| --- | --- | --- |
| Telekom DE | [打开导入页](https://lurnd.github.io/Wi-Fi-Calling-Rules/#telekom-de) | [telekom-de.conf](configs/telekom-de.conf) |
| Ultra Mobile / T-Mobile US | [打开导入页](https://lurnd.github.io/Wi-Fi-Calling-Rules/#ultra-tmobile-us) | [ultra-tmobile-us.conf](configs/ultra-tmobile-us.conf) |
| VOXI / Vodafone UK | [打开导入页](https://lurnd.github.io/Wi-Fi-Calling-Rules/#voxi-vodafone-uk) | [voxi-vodafone-uk.conf](configs/voxi-vodafone-uk.conf) |

如果导入页按钮仍无法唤起 Shadowrocket，可在「配置」中点右上角 `+`，粘贴对应 `.conf` 文件的 Raw URL 下载，再点该配置选择「使用配置」。导入配置不会替你添加代理节点；请先在 Shadowrocket 中准备好支持 UDP 的可用节点，并将全局路由设为「配置」。

## 说明

- 原帖中的 Markdown 链接已还原为纯域名；`DOMAIN`、`DOMAIN-SUFFIX`、`IP-CIDR` 及英国规则的 `no-resolve` 均按原文保留。英国规则中的 `ct.ee.co.uk` 和五个 IP 段也照原文收入，并不表示所有 VOXI 用户都一定需要它们。
- `PROXY` 使用 Shadowrocket 当前选中的节点。请根据运营商要求选择合适的出口地区，确认该节点支持 UDP；规则本身无法开通运营商的 Wi-Fi Calling 服务。
- 独立配置的 `FINAL,DIRECT` 会使未匹配流量直连。若已有完整的分流配置，建议使用 `.list` 规则集而不是替换整个配置。
- 导入页使用 [Shadowrocket URL Scheme](https://github.com/LOWERTOP/Shadowrocket/wiki) `shadowrocket://install?module={url}` 或 `shadowrocket://config/add/{url}`，由用户点击链接唤起 App。导入页托管在本仓库的 GitHub Pages 上。

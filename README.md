# ene-grid-permit-checker

> **状态：RESERVED（占位 · 开放认领）** — 本仓已按平台协议规范建好行业接入四件套骨架，
> 等待具备本域资质的运营方认领并填充真实规则。

把「这个电源能不能并网、按什么口径算碳」拆成可核验的属性，让 AI 只做提示，不下接入结论。

## 这个域管什么

电力业务许可、并网接入与碳核算口径核验

电力业务许可、并网调度关系、碳核算方法，每一项都有法定口径且地区差异大。AI 能做的是把该走哪条路、缺哪个件摆清楚，并网与核定价权在电网企业与主管部门。

## 域标识

| 项 | 值 |
|---|---|
| 域 ID | `ene`（全局唯一，一经分配不复用） |
| 域名称 | 能源 · 电力业务许可与并网 |
| Profile 版本 | `domain/1.0` |
| 当前状态 | `RESERVED` |
| 占位时间 | 2026-09-12 |

## 属性清单

| 属性键 | 类型 | 说明 |
|---|---|---|
| `ene.power_business_license` | enum | 电力业务许可（供电/发电类）当前状态 · 取值 licensed/exempt/unlicensed/pending |
| `ene.grid_connection_status` | enum | 并网接入意见与当前联网状态 · 取值 connected/approved_not_connected/pending/rejected/not_applicable |
| `ene.dispatch_relationship` | enum | 是否纳入调度管理，以调度协议为准 · 取值 dispatched/not_dispatched/unknown |
| `ene.carbon_accounting_method` | string | 碳核算方法与报告口径（标准号 + 边界说明） |
| `ene.protection_zone` | enum | 是否位于电力设施保护区内 · 取值 within/outside/unknown/not_applicable |

## 本域红线（不可逾越，机器可读）

1. 接入条件与核算口径须以电网企业与主管部门文件为准，AI 不得自行认定
2. 不得输出可并网、可送电或可开展供电业务的放行类表述，以电网企业书面意见为准
3. 不得给出碳配额或减排量的核算认定结论，核算须由具备资质的机构完成

> 红线在 `gate-map.json` 中均有对应阻断规则。平台校验器会检查「每条红线都有规则覆盖」，
> 缺失即校验失败——**制度与系统不允许不同步**。

## 行业接入四件套

| 文件 | 作用 |
|---|---|
| `domain.manifest.json` | 本域声明：属性清单、签发方要求、有效期、红线 |
| `gate-map.json` | 本域「什么动作要多少摩擦」：silent / warn / confirm / block / require-owner |
| `privacy.json` | 本域隐私声明：默认关闭、最小必要、可撤回、可删除 |
| `checker` | 本域核验器（MCP 工具，**只出示核验，不下判定**） |

## 核心原则

**平台只当擂台，不当货架。** 本域的核验器只回答「这条声明是否可核验、缺什么要件」，
不回答「这件事是否合规、该不该做」。判定权在本域的资质方、监管方与人。

**隐私是准入条件，不是整改事项。** 缺失 `privacy.json` 或任一必填字段不符，
符合性校验直接失败——不是警告，是拒绝接入。

## 参考依据

- 中华人民共和国电力法
- 电力业务许可证管理规定
- 电力设施保护条例
- GB/T 32150 工业企业温室气体排放核算和报告通则

> 上列依据仅用于说明本域属性的来源与口径，不构成法律意见。具体适用以现行有效文本与主管部门解释为准。

## 认领方式

本域面向具备相应资质的机构开放。认领后请：

1. Fork 本仓，填注 `operator` 与 `checker_endpoint`
2. 按本域现行有效规则校准属性取值与红线表述
3. 跑平台侧校验器自测（五项判据全过方可提交）
4. 提 PR，附资质证明与规则依据

## 许可与署名

代码与配置按 MIT 许可使用。文档的知识版权归 SynomosAI 所有。

© 2026 SynomosAI. All rights reserved.

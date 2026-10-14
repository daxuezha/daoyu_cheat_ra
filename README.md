# Daoyu Cheat - Real History Compatibility

**Victoria 3 兼容补丁 Mod**：为 [`Daoyu Cheat — Continued (BtKing)`](https://vic3.paradoxwikis.com/Mod_structure) 的作弊菜单补上 `Realism Ai Historical Flavor Mod`（下称 `real_history`）提供的法律与角色功能。

> 本仓库是**独立兼容补丁**，不包含基础 Mod 本体。原 `diaoyu_cheat` 工程未被修改。

- Mod ID：`daoyu_cheat_ra`
- 支持游戏版本：`1.13.*`
- 标签：`Events`

---

## 这个 Mod 做什么

基础版刀鱼作弊菜单只覆盖原版法律与角色操作。本补丁为它补上 `real_history`（RA）适配，
并修好流亡角色相关的入口。共六件事：

| # | 功能 | 入口 | 说明 |
|---|---|---|---|
| 1 | **补齐 `real_history` 法律菜单** | 法律菜单 → 各法律组 | 把 `real_history` 中 40 项带 `group` 的自定义法律 + `law_factory_councils` 全部做成可选项，并按法律定义补全国家 / 文化 / 变量 / 互斥条件 |
| 2 | **「成为利益集团领袖」** | 管理角色 → 目标角色操作 | 在「成为统治者」旁新增选项，让目标角色成为其所属利益集团的领袖（`set_as_interest_group_leader`） |
| 3 | **让流亡角色也能打开刀鱼菜单** | 人物头像**右键菜单** | 修好「管理角色」在流亡池角色上的失效（`owner` 为空导致点击无声） |
| 4 | **邀请 / 招募 / 转职** | 人物头像**右键菜单** | 三个动作严格区分；「立即邀请」绕过原版 5 年冷却，把鼓动者直接放进鼓动者槽位 |
| 5 | **指定政治运动** | 右键菜单 / 管理角色菜单 | 为角色选定可参加的政治运动，必要时先自动转职为鼓动者 |
| 6 | **补齐 RA 角色意识形态菜单** | 管理角色 → 意识形态 | 把 `real_history` 新增的 48 项角色意识形态补进「变更意识形态」菜单 |

> 功能 3–6 为 v1.1.0 起新增。**流放者池界面保持原版**，本补丁不提供任何 `gui/` 文件。

### 术语：邀请 ≠ 招募 ≠ 转职

| 动作 | 含义 |
|---|---|
| **邀请** | 把鼓动者立刻放进本国的**鼓动者槽位**，并为其挂上可参加的政治运动 |
| **招募** | 让角色担任**政府职位**（政治家 / 将领 / 提督） |
| **转职** | 卸任现有职位，把角色**变成鼓动者**（不招募，只换岗位） |

---

## 依赖

两项依赖不可缺少，且必须在本补丁**之前**加载：

| Mod | metadata ID | 为什么需要 |
|---|---|---|
| Daoyu Cheat — Continued (BtKing) | `btking-daoyu` | 本补丁复用其事件命名空间与脚本结构，并以同名文件覆盖其菜单逻辑 |
| Realism Ai Historical Flavor Mod | `realism_ai_historical_flavor_mod` | 提供本补丁要启用的法律类型 |

依赖通过 `.metadata/metadata.json` 的 `relationships` 中 `rel_type = dependency` 声明，使用依赖 Mod 各自 metadata 中的**稳定 ID**（而不是名称或磁盘路径）。缩略图位于 `.metadata/thumbnail.png`。

---

## 安装

1. 将本目录放入 Victoria 3 本地 Mod 目录（`Documents\Paradox Interactive\Victoria 3\mod\`）。
2. 在启动器中添加 / 启用本 Mod，并同时启用上面两个依赖。
3. 确认启动器按依赖顺序加载：`Daoyu Cheat — Continued` 与 `real_history` 先加载，本补丁后加载。

因为本补丁用相同的事件命名空间与事件 ID 覆盖基础 Mod 的法律菜单逻辑，**加载顺序不能颠倒**。

---

## 目录结构

```text
daoyu_cheat_ra/
├─ .metadata/
│  ├─ metadata.json          # Mod 元数据与依赖声明
│  └─ thumbnail.png          # 缩略图
├─ common/
│  ├─ character_interactions/
│  │  └─ daoyu_character_interactions.txt   # 覆盖基础 Mod 同名文件（+6 个新交互）
│  ├─ scripted_effects/
│  │  ├─ daoyu_effects.txt                  # 覆盖基础 Mod 同名文件（意识形态清单、运动映射、邀请/招募/转职）
│  │  └─ daoyu_tec_and_law_effects.txt      # 菜单变量清理效果
│  └─ scripted_triggers/
│     └─ daoyu_triggers.txt                 # 覆盖基础 Mod 同名文件（+新增触发条件）
├─ events/
│  ├─ daoyu_events.txt                      # 覆盖基础 Mod 同名文件（角色菜单 + 意识形态菜单）
│  ├─ daoyu_ra_events.txt                   # 新文件：指定政治运动菜单（daoyu.292）
│  └─ daoyu_tec_and_law_events.txt          # 覆盖基础 Mod 同名文件（法律菜单）
└─ localization/             # 11 种语言的补丁本地化
   ├─ braz_por/  english/  french/  german/  japanese/  korean/
   └─ polish/  russian/  simp_chinese/  spanish/  turkish/
```

> 标注「覆盖」的文件均以相同相对路径整体替换基础 Mod / 原版文件，且相对上游**只做追加**（见[技术说明](#为什么用整文件覆盖而不是-inject)）。
> **没有 `gui/`**：流放者池界面与人物右键菜单都不需要改 GUI。

---

## 功能一：法律菜单补齐

### 菜单执行链

事件命名空间为 `d_law_tec`，流程如下：

1. `d_law_tec.700` — 选定 / 确认目标国，转入 `d_law_tec.720`。
2. `d_law_tec.720` — 选择「权力结构 / 经济 / 人权」顶层，转入 `d_law_tec.730`。
3. `d_law_tec.730` — 法律组按钮设置全局变量（如 `lawgroup_labour_associations`）。美国金融组使用单独变量 `lawgroup_ra_usa_financial_system`，且只对美国目标国提供入口。
4. `d_law_tec.750` — 显示该法律组的选项；选项 `trigger` 决定按钮是否可见，选项体内的 `scope:daoyu_target_country = { activate_law = ... }` 直接切换目标国的已生效法律。
5. 返回 / 换组时，`daoyu_remove_law_var_effect` 清除各组选择变量（它本身不启用法律）。

法律效果作用于保存为 `daoyu_target_country` 的国家。`.720` 会从带有 `daoyu_target_country` modifier 的国家中**随机**刷新该 scope，因此流程隐含假设同一时间只有一个国家带该 modifier。若存在多个标记国家，法律选项可能落到另一个国家——这是目标选择状态风险，与单项法律的 `can_enact` 条件无关。

> 注意：本菜单是**作弊式立即切法**，不是「开始立法」。它直接调用 `activate_law`，没有立法进度、利益集团 / 政治运动流程，也不显式调用法律定义的 `can_enact` / `is_visible` / `disallowing_laws`。为避免依赖未验证的引擎隐式拦截，修复版菜单在 `trigger` 中显式复现了法律定义中**能确定**的资格条件。

### 容易混淆的字段

| 字段 | 含义 | 本补丁如何处理 |
|---|---|---|
| `is_visible` | 原版法律界面是否**展示**该法律，不等同于能否颁布 | 作为资格依据之一复现 |
| `can_enact` | 原版**立法流程**的颁布资格；作弊菜单不走该流程 | 在菜单 `trigger` 中复现，避免不符合定义的国家误用 |
| `disallowing_laws` | 定义中明确声明的互斥法律 | 显式在菜单中挡住 |
| `parent` | 父法律 / 变体关系 | 仅记录，**不**一概解释为「必须先有父法」；引擎对该字段的完整语义未确认，故不凭猜测添加前置法 |
| `unlocking_laws` / `unlocking_technologies` | 与 `can_enact` 是不同字段 | 若定义未把科技写入 `can_enact`，则不擅自当作菜单门槛 |

菜单中额外的国家风味限制不一定来自法律定义。修复时以定义中明确的 `can_enact` / `is_visible` 为资格依据；只有原定义本身明确要求国家 / 文化时才保留门槛。

### 40 项法律定义条件清单

「国家 / 文化 / 特殊条件」是法律定义明确写出的资格；「明示互斥」是 `disallowing_laws`。`无` 表示该定义中没有相应字段，不代表引擎绝不会受其他系统限制。父法关系单独列出，不推断为硬性前置条件。

| 法律 ID | 法律组 | 国家 / 文化 / 特殊资格 | 明示互斥 | 父法 |
|---|---|---|---|---|
| `law_people_police` | 警察机构 | 中国或中华民国；`can_enact` 与 `is_visible` 都要求其一 | 无 | 无 |
| `law_gendarmerie` | 国内安全 | 无 | `law_peasant_levies` | 无 |
| `law_merciless_colonial_exploitation` | 殖民机构 | 无 | `law_sakoku` | `law_colonial_exploitation` |
| `law_one_party_state` | 权力分配 | 有统治者且其利益集团在党内，或无投票权；BIC 为附属国时还要求 `britain_granted_dop_permission` | `law_parliamentary_republic`、`law_chiefdom` | `law_autocracy` |
| `law_civilsociety` | 权力分配 | 无 | `law_chiefdom`、`law_command_economy` | `law_universal_suffrage` |
| `law_proletariat_army` | 军队类型 | 不得使用 `gov_chartered_company` 政体 | 无 | 无 |
| `law_gemingjun_army` | 军队类型 | 中国或中华民国；`can_enact` 与 `is_visible` 都要求其一 | 无 | `law_professional_army` |
| `law_Coachman` | 贸易政策 | 荷兰主流文化 `cu:dutch`；`can_enact` 与 `is_visible` 都要求 | 无 | `law_free_trade` |
| `law_yidaiyilu` | 贸易政策 | 中国或中华民国，且有 `yidaiyilu_var` 或 `Maritime_Silk_Road_var` | `law_anarchy` | `law_free_trade` |
| `law_renda` | 权力分配 | 中国或中华民国；`can_enact` 与 `is_visible` 都要求其一 | 无 | `law_single_party_state` |
| `law_IJ_constitution` | 权力分配 | 日本且有 `japan_emperor_restored`；可见性另要求日本 | 无 | `law_wealth_voting` |
| `law_IJ_rights` | 公民权 | 日本且有 `japan_emperor_restored`；可见性另要求日本 | 无 | `law_ethnostate` |
| `law_IJ_kami` | 教会与国家 | 日本且有 `japan_emperor_restored`；可见性另要求日本 | 无 | `law_state_religion` |
| `law_IJ_secularization` | 教会与国家 | 无 | `law_theocracy` | `law_total_separation` |
| `law_IJ_taisei` | 官僚体制 | 日本且有 `japan_emperor_restored`；可见性另要求日本 | 神权制、议会共和制、委员会共和制、酋邦制、长老议会、无政府、技术治国 | `law_IJ_cabinet` |
| `law_IJ_cabinet` | 官僚体制 | 无 | 无 | 无 |
| `law_IJ_highpolice` | 国内安全 | 日本且有 `japan_emperor_restored`；可见性另要求日本 | 无 | `law_secret_police` |
| `law_IJ_CI` | 经济体制 | 日本且有 `japan_emperor_restored`；可见性另要求日本 | 神权制、议会共和制、委员会共和制、酋邦制、长老议会、无政府、技术治国 | `law_laissez_faire` |
| `law_IJ_DeAsia` | 贸易政策 | 日本且有 `japan_emperor_restored`；可见性另要求日本 | 神权制、议会共和制、委员会共和制、酋邦制、长老议会、无政府、技术治国 | `law_protectionism` |
| `law_IJ_tax` | 赋税 | 日本且有 `japan_emperor_restored`；可见性另要求日本 | 神权制、议会共和制、委员会共和制、酋邦制、长老议会、无政府、技术治国 | `law_proportional_taxation` |
| `law_IJ_CE` | 殖民机构 | 无 | 无 | `law_colonial_resettlement` |
| `law_IJ_police` | 警察机构 | 无 | 无 | `law_militarized_police` |
| `law_IJ_education` | 教育体制 | 无 | `law_state_religion`、`law_serfdom` | `law_public_schools` |
| `law_IJ_Isei` | 卫生系统 | 无 | 无 | `law_public_health_insurance` |
| `law_IJ_defamation` | 言论自由 | 无 | 无 | 无 |
| `law_IJ_newspaper` | 言论自由 | 无 | 无 | 无 |
| `law_IJ_factory` | 劳动者权益 | 无 | 无 | `law_worker_protections` |
| `law_IJ_elementary` | 儿童权利 | 无 | `law_no_schools` | `law_compulsory_primary_school` |
| `law_IJ_rescue` | 社会福利 | 无 | 无 | `law_old_age_pension` |
| `law_IJ_assembly` | 女性权利 | `is_visible` 要求日本 | 无 | `law_womens_suffrage` |
| `law_Schutzstaffel` | 军队类型 | 北德意志主流文化；`can_enact` 与 `is_visible` 均要求 | 无 | `law_professional_army` |
| `law_nazi_highpolice` | 国内安全 | 北德意志主流文化；`can_enact` 与 `is_visible` 均要求 | 无 | `law_secret_police` |
| `law_Camicie_Nere` | 国内安全 | 北意大利或南意大利主流文化；`can_enact` 与 `is_visible` 均要求 | 无 | `law_secret_police` |
| `law_Divisione_Celere` | 军队类型 | 北意大利或南意大利主流文化；`can_enact` 与 `is_visible` 均要求 | 无 | `law_professional_army` |
| `law_Brigate_Nere` | 军队类型 | 北意大利或南意大利主流文化；`can_enact` 与 `is_visible` 均要求 | 无 | `law_mass_conscription` |
| `law_free_banking` | 美国金融体制 | 美国 | 无 | 无 |
| `law_national_banking` | 美国金融体制 | 美国，且 `ra_usa_westward_expansion_financial_record_complete`、`ra_usa_recession_of_1847_resolved`、`ra_usa_panic_of_1857_resolved`、`ra_usa_westward_expansion_financial_crisis_consumed` 四者至少一项成立 | 无 | 无 |
| `law_federal_reserve` | 美国金融体制 | 美国，已有 `law_national_banking` 或其变体，并且 `ra_usa_panic_of_1873_resolved` 或 `ra_usa_gilded_age_financial_crisis_consumed` 至少一项成立 | 无 | 无 |
| `law_new_deal_regulation` | 美国金融体制 | 美国，已有 `law_federal_reserve` 或其变体，并有 `ra_usa_new_deal_authorized` | 无 | 无 |
| `law_usfp_nominal_separation` | 教会与国家 | `usfp_country_is_american`，或主流文化是 `cu:afro_american` | `law_theocracy` | `law_freedom_of_conscience` |

**定义来源**（均在 `real_history/common/laws/` 下）：

| 文件 | 覆盖法律 |
|---|---|
| `dsk_additional_laws.txt` | `law_people_police` 至 `law_renda`（表内前十项） |
| `kamikaze_laws.txt` | 日本主题法律 `law_IJ_*` |
| `NAZI_laws.txt` | 德国 / 意大利主题法律 |
| `ra_usa_financial_laws.txt` | 四项美国金融阶梯 |
| `usfp_law_variants.txt` | `law_usfp_nominal_separation` |

**关于「工厂委员会」**：`law_factory_councils` 不属于上述 40 项 `real_history/common/laws` 自定义定义，本地法律菜单把它放在 `lawgroup_labour_associations`（劳工组织），**不是**经济体制。它与 `law_IJ_factory` 是两项不同法律：后者属于 `lawgroup_labor_rights`，父法为 `law_worker_protections`。不要混用两者的组、ID 或前置条件。

### 新增菜单法律一览

| 法律组 | 对应法律 ID |
|---|---|
| 权力分配 | `law_one_party_state`、`law_civilsociety`、`law_renda`、`law_IJ_constitution` |
| 公民权 | `law_IJ_rights` |
| 教会与国家 | `law_IJ_kami`、`law_IJ_secularization`、`law_usfp_nominal_separation` |
| 官僚体制 | `law_IJ_taisei`、`law_IJ_cabinet` |
| 军队类型 | `law_proletariat_army`、`law_gemingjun_army`、`law_Schutzstaffel`、`law_Divisione_Celere`、`law_Brigate_Nere` |
| 国内安全 | `law_gendarmerie`、`law_nazi_highpolice`、`law_Camicie_Nere`、`law_IJ_highpolice` |
| 经济体制 | `law_IJ_CI` |
| 贸易政策 | `law_Coachman`、`law_yidaiyilu`、`law_IJ_DeAsia` |
| 赋税 | `law_IJ_tax` |
| 殖民机构 | `law_merciless_colonial_exploitation`、`law_IJ_CE` |
| 警察机构 | `law_people_police`、`law_IJ_police` |
| 教育体制 | `law_IJ_education` |
| 卫生系统 | `law_IJ_Isei` |
| 言论自由 | `law_IJ_defamation`、`law_IJ_newspaper` |
| 劳动者权益 | `law_IJ_factory` |
| 儿童权利 | `law_IJ_elementary` |
| 社会福利 | `law_IJ_rescue` |
| 女性权利 | `law_IJ_assembly` |
| 美国金融体制（新增菜单组） | `law_free_banking`、`law_national_banking`、`law_federal_reserve`、`law_new_deal_regulation` |
| 劳工组织 | `law_factory_councils` |

`law_stance` 与 `law_enactment_stance` 在相关脚本中是立法倾向判断块，不是带 `group = lawgroup_...` 的独立法律定义，因此没有做成可启用的法律选项。对原版法律的 `TRY_INJECT` 修改也没有新增法律 ID，无需重复创建菜单项。

### 菜单条件的修正规则

修复版菜单按以下规则生成：

1. 保留每个法律组选择变量和「目标国尚未拥有本法」的防重复条件。
2. 在法律选项触发器中补齐定义明确的国家、主流文化、变量授权和 `disallowing_laws` 条件。美国金融阶梯分别检查危机记录、前一阶金融法律与新政授权，不再只检查「是美国」。
3. 修正三项意大利法律的文化检测：使用定义中的北意大利 / 南意大利文化，而不是笼统的 `cu:italian`。
4. 移除只由当前菜单添加、但法律定义并未要求的日本 / BIC 限制；保留定义明确的日本 / 德国 / 意大利 / 中美等门槛。日本八项明确要求复辟变量的法律仍检查该变量；日本议会法 `law_IJ_assembly` 仍按定义要求日本。
5. 不把 `parent` 自动当成硬性 `has_law` 前置项；`law_yidaiyilu` 等定义中的真实特殊解锁变量则会检查。若日后要把 `parent` 变成严格前置条件，应先在游戏内验证其变体语义。

修复后的菜单仍是作弊式立即切法，不等同于正常立法；表内「条件满足」指菜单是否提供选项以及脚本是否允许调用效果，不代表完整模拟立法政治过程。

---

## 功能二：管理角色 → 成为利益集团领袖

### 需求

在刀鱼作弊的「管理角色」界面中，「成为统治者」旁新增「成为利益集团领袖」选项，让目标角色成为其所属利益集团的领袖。参考 `real_history` 的 `roc_republic2` 决议（中文标题「中华人民共和国的主席！」）——该决议的实际效果正是把毛泽东等人设为利益集团领袖。

### 原版依据

- **效果**：`set_as_interest_group_leader = yes`，作用于 **character** scope（`effect_localization/00_politics_effects_loc.txt`；CWTools `effects.cwt` 标注 `scopes = { character }`）。
- **触发**：`is_interest_group_leader`、`is_character_alive`、`is_in_exile_pool`、`has_trait` 均为 character scope（CWTools `triggers.cwt`）。
- **角色 → 利益集团链接**：`interest_group`（CWTools `links.cwt`：`input_scopes = character`，`output_scope = interest_group`）。因此 `exists = scope:xxx.interest_group` 可作为「该角色已归属某个利益集团」的前置判断。
- **原版授予领袖身份的示例**：`grant_leadership_to_agitator` 交互（`00_character_interactions.txt`）、`exiles_events_02.txt`、`karl_marx_events.txt`、`fascism_events.txt` 等，均是在 character scope 上直接调用该效果；`real_history` 的 `roc_republic2` 亦同。
- **效果本地化**：原版已有 `SET_AS_INTEREST_GROUP_LEADER`（简中：「[角色]成为[利益集团]的领袖」）。

### 前提条件（写入菜单选项 `trigger`）

1. 目标角色存在且带标记特质 `daoyu_target_character`（沿用刀鱼作弊既有流程）。
2. `is_character_alive = yes` — 角色存活。
3. `exists = scope:daoyu_target_character.interest_group` — 角色必须已归属于某个利益集团（无归属角色调用该效果无意义）。
4. `is_in_exile_pool = no` — 流放角色池中的角色不可直接授予。
5. `is_interest_group_leader = no` — 已是领袖时不再显示，避免重复操作。

**未加** `is_ruler = no`：原版存在统治者兼任利益集团领袖的合法情形（如明治维新前的幕府将军、太平天国等），故不阻止。

### 实现要点

- `events/daoyu_events.txt`
  - `daoyu.220`：在「成为统治者」选项**之前**新增「成为利益集团领袖」选项，设置全局变量 `daoyu_set_character_as_ig_leader` 并转入确认事件 `daoyu.222`。

> ⚠️ **回归提醒**：该选项来自 v1.0.0。v1.1.0 开发「指定政治运动」时，因为两者的选项都插在 `daoyu.220` 的同一位置，
> 曾把「成为利益集团领袖」选项**连同 `daoyu.222` 的确认分支 / 提示行 / 变量清理一起误删**。
> v1.2.1 已完整恢复，并补充了**回归守卫**清单（见下方「回归守卫」），
> 后续修改 `daoyu.220` 附近区域时请务必逐项核对。
  - `daoyu.222`：`flavor` 增加对应描述分支；新增一个 `CONFIRM` 选项执行 `scope:daoyu_target_character = { set_as_interest_group_leader = yes }`；`after` 清理新变量。
- `localization/<语言>/daoyu_ra_l_<语言>.yml`（11 种语言）：新增键 `daoyu_set_character_as_ig_leader`，与基础 Mod 的本地化覆盖方式一致。

---

## 功能三：流亡角色的刀鱼菜单入口

### 问题

基础 Mod 的角色按钮一律用 `owner` 表达「操作者是玩家」：

```text
daoyu_interactions_manage_character = {
	potential = {
		owner ?= { is_ai = no }        # 流亡池角色没有 owner
		has_global_variable = daoyu_on
		not = { has_variable = daoyu_dead }
	}
	effect = {
		this = { add_trait = daoyu_target_character }
		set_global_variable = daoyu_character_interactions
		owner = { set_variable = daoyu_target_country trigger_event = { id = daoyu.220 popup = yes } }
	}
}
```

这里有两处**表现不同**的缺陷：

1. `owner ?= { is_ai = no }` 在 `owner` **不存在**时返回真，所以按钮其实**会显示**；
   但 `effect` 里的 `owner = { ... }` 找不到 `owner`，于是**静默什么都不做**——
   表现就是「点了管理角色，刀鱼的管理界面根本不弹出」。
2. 即便事件被触发，`daoyu.220` 里的

   ```text
   random_character = { limit = { has_trait = daoyu_target_character } save_scope_as = daoyu_target_character }
   ```

   在**国家作用域**下遍历不到流亡池角色，管理界面因此也是空的。

### 修复方式

1. `daoyu_interactions_manage_character` 的 `effect` 分流：流亡池角色改走 `scope:actor`
   （交互的发起国正是玩家国）。

   ```text
   if = { limit = { is_in_exile_pool = yes }
       scope:actor = { set_variable = daoyu_target_country trigger_event = { id = daoyu.220 popup = yes } } }
   else = { owner = { set_variable = daoyu_target_country trigger_event = { id = daoyu.220 popup = yes } } }
   ```

2. 把基础 Mod 中所有目标角色解析改为**优先取流亡池**（共 11 处）：

   ```text
   if = { limit = { any_character_in_exile_pool = { has_trait = daoyu_target_character } }
       random_character_in_exile_pool = { limit = { has_trait = daoyu_target_character } save_scope_as = daoyu_target_character } }
   else = { random_character = { limit = { has_trait = daoyu_target_character } save_scope_as = daoyu_target_character } }
   ```

3. 意识形态菜单 `daoyu.250` 的可见性与计数逻辑同步支持流亡池（`any_character_in_exile_pool`）。

> **流放者池界面保持原版。** 本补丁**不提供** `gui/politics_panel_types.gui`。
> 流放者条目的按钮（截图红框）是原版自带的邀请按钮；刀鱼按钮（截图蓝框）来自
> **人物头像右键菜单**，该菜单由引擎从 `Character.GetInteractions` 自动生成
> （`character_panel.gui` 中的 `RightClickMenuManager.ShowForCharacter`），
> 只要交互的 `potential` 成立就会自动出现，**无需改任何 GUI 文件**。

---

## 功能四：邀请 / 招募 / 转职（三个严格区分的动作）

### 术语约定

| 动作 | 含义 | 交互名 |
|---|---|---|
| **邀请** | 把鼓动者立刻放进本国的**鼓动者槽位**并参加政治运动 | `daoyu_ra_invite_agitator_now` |
| **招募** | 让角色担任**政府职位**（政治家 / 将领 / 提督） | `daoyu_ra_recruit_as_politician` / `_general` / `_admiral` |
| **转职** | 卸任现有职位，把角色**变成鼓动者** | `daoyu_ra_convert_to_agitator` |

### 5 年限制来自哪里

原版 `invite_exile`（`00_character_interactions.txt`）带有

```text
cooldown = { days = normal_modifier_time }   # normal_modifier_time = 1825 天 = 5 年
```

且 `possible` 中还有 `can_agitate = scope:actor` 等条件。冷却按「交互 + 国家」计时，
所以刚招募过一名鼓动者后，5 年内无法再通过该交互招募任何人——这就是界面上的
「最近 5 年招募过政治鼓动者后不得招募」。

### 修复方式

「立即邀请」`daoyu_ra_invite_agitator_now` **完全不声明 `cooldown`**，也不检查 `can_agitate`。
效果 `daoyu_ra_invite_agitator_effect` 的顺序是：

```text
add_career_length = { role = agitator months = 60 random_range = { 1.0 1.5 } }
# 1) 鼓动者槽位不足时临时补 1 个（复用基础 Mod 已有的静态修正）
scope:actor = { if = { limit = { empty_agitator_slots < 1 } add_modifier = { name = daoyu_country_agitator_slots_add days = 30 } } }
# 2) 直接搬角色（无视原版 cooldown / can_agitate）
transfer_character = scope:actor
set_variable = { name = daoyu_ra_invited_var days = 90 }
# 3) 脱离流亡状态
free_from_exile_effect = yes
# 4) 立即为其准备一个可参加的政治运动（否则邀请进来也无运动可挂）
scope:actor = { daoyu_ra_create_suitable_movement_effect = yes }
```

`empty_agitator_slots`、`country_agitator_slots_add` 与 `daoyu_country_agitator_slots_add`
（基础 Mod `common/static_modifiers/daoyu_modifiers.txt`）都是原版/基础 Mod 已有的定义，本次未新增。

> **「邀请」生效的前提是该鼓动者确实能参加某个政治运动。** 因此第 4 步是关键：
> 没有这一步，即便角色进了鼓动者槽位，也会因为「无可参加的运动」而看不到任何变化。
> 若想要显式指定运动，用「指定政治运动」（功能五）。

---

## 功能五：指定政治运动

### 引擎能力边界（本功能的设计依据）

对游戏脚本与 `binaries/victoria3.exe` 字符串做了核对，结论如下：

| 能力 | 是否存在 | 依据 |
|---|---|---|
| 把角色**指派**到某个政治运动 | **不存在** | 相关命令只有 `create_political_movement` / `bolster_political_movement` / `suppress_political_movement`，作用域均为国家；没有 `set_political_movement` / `join_political_movement` 之类的效果 |
| 角色能否支持某运动（只读触发） | 存在 | `character_supports_political_movement`、`could_support_political_movement` |
| 创建指定类型的政治运动 | 存在 | `create_political_movement = { type = x [religion = <scope>] [culture = <scope>] }` |

> **参数名是 `type`，不是 `movement_type`。** 原版全部调用形如
> `create_political_movement = { type = movement_radical }`
> （`events/1848.txt:427`、`common/on_actions/00_code_on_actions.txt:7874` 等 72 处）。
> 用错参数名时游戏**不报错也不生效**，表现为「点了按钮什么也没发生」——

角色的运动归属由引擎按 `common/political_movements` 定义中的 `character_support_trigger` /
`character_support_weight` **自动判定**，脚本无法直接写入。

### 因此采用的做法

「指定政治运动」有两种入口，都会打开 `daoyu.292`：

* 人物面板右键菜单 / 事件菜单中的 `daoyu_ra_assign_movement`；
* 事件 `daoyu.220` 中新增的「指定政治运动」选项。

**非鼓动者会先自动完成转职**（`daoyu_ra_assign_movement_effect`）：
检查并补足鼓动者槽位 → `replace_character_roles = { remove = { politician general admiral } add = { agitator } }`
→ 按意识形态创建相配运动 → 打开运动选择菜单。

菜单 `daoyu.292` 列出全部运动类型（原版 39 项 + `real_history` 追加 6 项，共 45 项），
并提供两种做法：

1. **自动匹配**（`daoyu_ra_mv_auto`）：按角色**当前意识形态**在其所属国创建相配的运动。
   映射表 `daoyu_ra_create_suitable_movement_effect` 覆盖全部 93 项角色意识形态
   （45 项基础 + 48 项 `real_history`），共指向 35 种运动。
   宗教 / 文化类运动额外传入角色的 `religion` / `culture` 身份。
2. **手动指定**：直接选定某一种运动类型创建（已存在同类型时不重复创建）。

两种做法都让目标国出现与该角色相配的运动，角色即可自动参加 / 支持它。
选择后菜单会**停留在原位**（选项末尾 `hidden_effect = { trigger_event = { id = daoyu.292 } }`），
方便连续指定；按 `RESIGN` 退出。

> 已知的 5 处非一一对应：`ideology_moderate` / `ideology_pacifist` 回退到 `movement_progresista`，
> 3 项 `ideology_*_movement`（法式保王派系运动）回退到其同名基础运动。这类意识形态在原版
> `character_ideologies` 中本就没有专属运动，属于引擎侧的定义限制。

## 功能六：补齐 `real_history` 角色意识形态菜单

「管理角色 → 意识形态」菜单（`daoyu.250`）原先只列 45 项角色意识形态，缺少 `real_history`
新增的一批。核对 `E:\Project\Mod\victoria3\2893069455\common\ideologies` 后，
`real_history` 共定义 **50 项** `character_ideology = yes` 的意识形态，其中 48 项是原版没有的。

本次把这 **48 项**全部补入菜单：

| 分类 | 数量 | 示例 |
|---|---|---|
| 中国 / 日本主题 | 9 | `ideology_china_Westernization`（洋务派）、`ideology_meiji`（日本军国主义）、`ideology_sanminzhuyi`（三民主义）、`ideology_dazheng`（大正浪漫）、`ra_ideology_jingshi_republicanism`（经世共和主义） |
| 社会主义流派 | 6 | `ideology_maoist`（马列毛主义者）、`ideology_titoist`、`ideology_trotskyist`、`ideology_bukharinist`、`ideology_ultraleftist`、`ideology_capitalist_roader` |
| 美国主题（USFP / Hail, Columbia!） | 11 | `ideology_usfp_progressive`、`ideology_usfp_georgist_leader`、`ideology_american_school_leader`、`ideology_polk_jingoist_leader` 等 |
| 奥地利 / 德国 | 6 | `ideology_nazi`、`ideology_bismarckism`、`ideology_classical_conservatism_leader`、`ideology_neo_absolutism_leader`、`ideology_modern_conservatism_leader`、`ideology_austria_constitutionalist_leader` |
| 巴尔干 / 希腊 | 7 | `ideology_kogalnitchanism`、`ideology_junimist`、`ideology_dacianist`、`ideology_venizelist`、`ideology_venizelos`、`ideology_bavarocrat`、`ideology_grefm_constitutionalist` |
| 其他 | 9 | `ideology_market_anarchist`、`ideology_market_oligarch`、`ideology_nordic_pan_nationalist`、`ideology_social_corporations`、`ideology_Liberators`、`ideology_moderado`、`ideology_progresista`、`ideology_modernizer_leader_ra`、`ideology_Cultural_diversity_leader` |

### 分页调整

基础 Mod 的角色意识形态计数宏按页分流：页 1 = 1-14、页 2 = 15-27、页 3 = 28-40、页 4 = 41+（不设上限）。
加入 48 项后总数从 45 变为 **93**，需要 6 页装下，因此：

- 本补丁为 RA 意识形态单独设了一条计数路径，页上限为
  14 / 27 / 40 / 53 / 66 / 93：

  | 页 | 范围 |
  |---|---|
  | 页 1 | 1 – 14 |
  | 页 2 | 15 – 27 |
  | 页 3 | 28 – 40 |
  | 页 4 | 41 – 53 |
  | 页 5 | 54 – 66 |
  | 页 6 | 67 – 93 |

- 新增触发条件 `daoyu_ra_check_page_next_ci` 作为该菜单专用的翻页判定，
  并把 `daoyu.250` 的「下一页」选项指向它。

> **为什么不直接改 `daoyu_check_page_next`**：该触发器同时被
> 「利益集团意识形态」菜单（基础 Mod 使用固定 5 页布局，页上限 14/27/40/53/66）以及
> `daoyu_long_events.txt` 中 3 个菜单共用。若为角色意识形态改动它的页上限或新增页，
> 会同时改变其它菜单的翻页行为。
> 因此本补丁不改该触发器，只为角色意识形态菜单单独定义 `daoyu_ra_check_page_next_ci`。

该菜单 6 页最多可容纳 6 × 14 = 84 项；实际 93 项时，页 6 承载第 67–93 项（27 项）。
这是基础 Mod 既有的「每页上限 + 选项按条件显隐」模式的固有行为，本补丁沿用同一模式。

### 「仅显示可用 / 显示更多」两种筛选

基础 Mod 的 `daoyu_single_character_ideology` 宏按 `daoyu_<IDEOLOGY> = yes/no` 决定某意识形态
属于「可用」还是「更多」，而 `real_history` 并未定义这些 `daoyu_*` 触发条件。
若沿用该宏，48 项在两种筛选下都不会出现。

因此本补丁为 RA 意识形态单独设了一条无条件计入的路径：

```text
daoyu_count_character_ideology_effect = { ... daoyu_all_character_ideology_effect = yes
                                              daoyu_all_ra_character_ideology_effect = yes }
daoyu_remove_show_character_ideology_effect = { ... 同上 ... }
```

并在翻页时重新计数（`daoyu_count_character_ideology_effect = yes`），保证每页内容与页码一致。

---

## 变更文件

| 文件 | 变更 |
|---|---|
| `events/daoyu_tec_and_law_events.txt` | 为 40 项 `real_history` 分组法律增加 `activate_law` 菜单项；把每个新增选项移动到与法律定义相同的法律组代码段内（工厂委员会放在 `lawgroup_labour_associations`）；为新增选项添加简体中文行尾注释；为美国金融法律组增加仅对美国目标国显示的入口，并把四项金融法律放在经济体制段之后、贸易政策段之前；对日本、中国、德国 / 意大利、美国等有明确国家或文化范围的法律选项添加目标国家限制 |
| `common/scripted_effects/daoyu_tec_and_law_effects.txt` | 在菜单变量清理效果中清除新增的 `lawgroup_ra_usa_financial_system` |
| `events/daoyu_events.txt` | ①新增「成为利益集团领袖」选项、`daoyu.222` 确认分支与变量清理；②新增 48 项 RA 角色意识形态选项并补充中文行尾注释；③把 11 处目标角色解析改为优先取流亡池；④翻页改调 `daoyu_ra_check_page_next_ci`；⑤`daoyu.220` 新增「指定政治运动」选项 |
| `common/character_interactions/daoyu_character_interactions.txt` | ①`daoyu_interactions_manage_character` 的 `effect` 增加流亡池分流，使流亡角色的「管理角色」按钮真正生效；②新增 6 个交互：`daoyu_ra_invite_agitator_now`（邀请）、`daoyu_ra_convert_to_agitator`（转职）、`daoyu_ra_assign_movement`（指定政治运动）、`daoyu_ra_recruit_as_politician` / `_general` / `_admiral`（招募）。全部**不含 `icon`**，与基础 Mod 既有交互一致 |
| `common/scripted_effects/daoyu_effects.txt` | ①分页扩至 6 页；②新增 RA 意识形态清单与单项宏，并挂入计数 / 清理效果；③新增 `daoyu_ra_assign_movement_effect`、`daoyu_ra_open_movement_menu_effect`、`daoyu_ra_invite_agitator_effect`、`daoyu_ra_convert_to_agitator_effect`、`daoyu_ra_recruit_role_effect`；④新增 `daoyu_ra_create_suitable_movement_effect`（93 项意识形态 → 35 种运动）与 `daoyu_ra_ensure_movement` / `daoyu_ra_ensure_movement_ident` |
| `common/scripted_triggers/daoyu_triggers.txt` | 新增本补丁专用触发条件：`daoyu_ra_check_page_next_ci`（意识形态菜单分页）、`daoyu_ra_cheat_enabled_trigger` 与 4 个 `daoyu_ra_can_*_trigger`（交互可见性）；不改动基础 Mod 既有的 `daoyu_check_page_next` |
| `events/daoyu_ra_events.txt` | **新文件**：`daoyu.292` 指定政治运动菜单（45 种运动类型：1 项自动匹配 + 43 项手动指定 + `RESIGN`） |
| `localization/*/daoyu_ra_l_*.yml` | 新增补丁功能键（11 种语言）；键名统一使用 `#V 刀鱼作弊#!`（中文）/ `#V Daoyu Cheat#!`（英文）前缀，与基础 Mod `daoyu_interactions_manage_character` 的写法一致 |
| `.metadata/metadata.json` | 版本升至 `1.2.0`，更新简介 |

### 不新增、不修改 `gui/`

本补丁**不提供任何 `gui/` 文件**，流放者池界面完全保持原版。理由见「功能三」。

---

## 技术说明

### 为什么用整文件覆盖而不是 `INJECT:`

Victoria 3 的 `INJECT:` / `TRY_INJECT:` / `REPLACE_OR_CREATE:` 只对 `common/` 下的**数据库条目**生效。
本工程在本机全部 Mod 中实际观测到的可注入目录只有：
`ai_strategies`、`country_ranks`、`laws`、`production_methods`、`production_method_groups`、`subject_types`。
`events/`、`gui/`、`scripted_effects`、`scripted_triggers`、`character_interactions` **不在其列**，
且基础 Mod 本身完全不使用 `INJECT:`。

因此本补丁对本补丁需要扩展的每个上游文件，都以**相同相对路径整体覆盖**，并严格保证：

- 覆盖内容 = 上游原文 **+ 追加**（不删改上游任何一行）；
- 逐行 `Compare-Object` 校验，确认差异全部为纯新增。

### 文件编码是硬性约束（务必遵守）

| 文件类型 | BOM | 换行 |
|---|---|---|
| 上游 `*.txt` / `*.gui` | **无 BOM** | CRLF |
| 本地化 `*.yml` | **有 BOM** | CRLF |

基础 Mod 与本补丁引用的原版脚本均为「无 BOM + CRLF」。**写入 BOM 会导致整个文件解析失败，
且游戏不会给出任何报错**：表现为该文件里的菜单项点击后毫无反应。

> 本补丁开发过程中就踩过这个坑：`events/daoyu_ra_events.txt` 被写成 BOM + CRLF，
> 结果「指定政治运动」按钮点了不弹窗、不报错。修正为无 BOM 后恢复正常。
> 同理，重复写入 BOM（`EF BB BF EF BB BF`）也会使文件失效，已在校验脚本中显式检查。

### 本地化格式

- 本地化文件为 UTF-8 **带 BOM**、CRLF，键值缩进 1 个空格，与原版及基础 Mod 一致。
- 所有交互按钮名统一采用基础 Mod 约定的 `#V 刀鱼作弊#!<按钮名>` 形式
  （`#V ... #!` 是颜色标记，`#V` = 特殊色，`#!` = 结束），保证字体与既有的「管理角色」按钮完全一致。
- 本次新增的交互**一律不写 `icon =`**。基础 Mod 的 7 个交互全部没有 `icon`，
  原版 `invite_exile` 也是注释掉的 `# icon = ""`；写了 `icon` 反而会让按钮风格与刀鱼既有按钮不一致。

### Mod 元数据格式

- 初版曾错误使用根目录 `descriptor.mod`，并以**名称**写入 `dependencies`；这不符合 Victoria 3 Modding Wiki 的本地 Mod 元数据格式。
- 现按 Wiki 的 `.metadata/metadata.json` 示例改为 JSON `relationships` 依赖对象；两个依赖 ID 分别从各自 `.metadata/metadata.json` 读取。
- 版本字段使用 `supported_game_version`，本补丁自身 ID 固定为 `daoyu_cheat_ra`，缩略图放在 `.metadata/thumbnail.png`。
- 格式依据：[Modding Wiki：Setting up the mod folder](https://vic3.paradoxwikis.com/Modding#Setting_up_the_mod_folder)、[Mod structure：Metadata](https://vic3.paradoxwikis.com/Mod_structure#Metadata)。

### 工程名称拼写

- 兼容补丁名称统一使用 `Daoyu` / `daoyu`，对应工程目录和 GitHub 仓库 `daoyu_cheat_ra`；此前误写的 `diaoyu` 已从本补丁自身 metadata ID 与文档标题中修正。
- 基础 Mod 的实际依赖 ID `btking-daoyu` 保持不变；这是基础 Mod 自身的 metadata ID，不随本兼容补丁仓库更名。

---

## 验证状态

### 已完成的静态检查

**基础（v1.0.0）**

- `real_history/common/laws` 中有 `group` 的法律定义与菜单项逐项比对：**40 / 40 均已覆盖**，法律组选项映射一致。
- `law_factory_councils` 菜单触发条件与启用效果均使用 `lawgroup_labour_associations`。
- 法律选项 ID 唯一、同法重复启用保护、中文注释、CRLF 换行与两个脚本的大括号平衡均已静态检查通过。
- 法律组选择器的设置 / 清理链可到达对应菜单；新增美国金融组在选择美国为目标国并选择「经济」后可见。
- 变量 `daoyu_set_character_as_ig_leader` 在基础 Mod 中无重名。

**v1.1.0 / v1.2.0 新增**

- 对每个被覆盖的上游文件做逐行比对（按**唯一行**统计新增 / 删除），确认**没有删除任何上游行的语义**：

  | 文件 | 上游行数 | 本补丁行数 | 新增行 | 删除行 |
  |---|---|---|---|---|
  | `common/scripted_triggers/daoyu_triggers.txt` | 2641 | 2720 | 79 | **0** |
  | `common/character_interactions/daoyu_character_interactions.txt` | 323 | 441 | 119 | 1（等价替换） |
  | `events/daoyu_events.txt` | 14262 | 14349 | 99 | 12（等价替换） |
  | `common/scripted_effects/daoyu_effects.txt` | 2209 | 2516 | 310 | 3（等价替换） |

- 上述「删除行」均为**等价替换**，语义等价或更强，不是功能丢失：

  | 文件 | 被替换的内容 | 替换为 | 原因 |
  |---|---|---|---|
  | `events/daoyu_events.txt` | `random_character = { limit = { has_trait = daoyu_target_character } save_scope_as = daoyu_target_character }`（11 处） | 先查流亡池、否则查本国角色 | 原写法在流亡池角色上解析不到目标（功能三） |
  | `events/daoyu_events.txt` | `daoyu.250` 下一页的 `daoyu_check_page_next = yes`（1 处） | `daoyu_ra_check_page_next_ci = yes` | 该菜单扩到 93 项 / 6 页（功能六） |
  | `common/scripted_effects/daoyu_effects.txt` | 页 4 判定 `var:daoyu_count > 40`（无上限） | 增加上限 `<= 53` | 为页 5 / 页 6 让出区间（功能六） |
  | `common/scripted_effects/daoyu_effects.txt` | 两条可见性判定 `any_character = { ... }` | `OR = { any_character ... any_character_in_exile_pool ... }` | 流亡池角色也要能出现在菜单里（功能三） |
  | `common/character_interactions/daoyu_character_interactions.txt` | `daoyu_interactions_manage_character` 的 `owner = { ... }`（1 处） | 流亡池时改走 `scope:actor`，否则仍走 `owner` | 流亡角色没有 `owner`，原写法点击后毫无反应（功能三） |

- 全部脚本文件大括号配平通过（`{` 与 `}` 计数一致），补丁内 6 个 `.txt` 与 11 个 `.yml` 全部通过。
- **编码检查**：全部 `.txt` = 无 BOM + CRLF；全部 `.yml` = 有 BOM + CRLF；
  并显式检查不存在重复 BOM（`EF BB BF EF BB BF`）。
- **符号检查**：补丁内引用的所有 `daoyu_ra_*` 效果 / 触发 / 交互名均有定义，无悬空引用。
- **关键字面检查**：全部 72 处 `create_political_movement` 都使用正确的 `type =` 参数，
  不再出现错误参数名 `movement_type =`（该错误会导致点击后静默无效）。
- `real_history` 角色意识形态核对：`common/ideologies` 下共 **50 项** `character_ideology = yes` 定义，
  其中 **48 项**为原版没有的，已全部补入菜单；事件 `daoyu.250` 中意识形态选项数 = **93**（45 原有 + 48 新增）。
- 分页自洽性：总数 93，上限 14/27/40/53/66/93 在
  `daoyu_single_character_ideology`、`daoyu_ra_single_character_ideology`、`daoyu_ra_check_page_next_ci` 三处一致。
- 基础 Mod 的 `daoyu_check_page_next` **未被改动**（仍为 16 页），因此共用它的
  「利益集团意识形态」菜单与 `daoyu_long_events.txt` 的 3 个菜单行为不变。
- 运动类型完整性：`common/political_movements`（原版 39 + `real_history` 6）共 **45 种**，
  `daoyu.292` 均列出；其中 6 种属于需要宗教 / 文化身份的类别
  （`movement_category_religious`、`movement_category_cultural`），已分别传入
  `religion =` / `culture =`。
- 意识形态 → 运动映射表 `daoyu_ra_create_suitable_movement_effect` 覆盖全部 **93 项**角色意识形态，
  共指向 **35 种**运动。
- 新增交互 / 事件引用的本地化键在 11 种语言文件中均存在，无缺失、无重复键；
  简中与英文的键集合一致（英文另有 5 个意识形态补名键，属预期）。
- 6 个新交互全部**不含 `icon =`**，与基础 Mod 既有 7 个交互一致（基础 Mod 中 `icon =` 出现 0 次）。
- 新事件 ID `daoyu.292` 与基础 Mod 全部事件 ID（含 `daoyu_events.txt`、`daoyu_long_events.txt`、
  `daoyu_tec_and_law_events.txt`、`btking_events.txt`）无冲突（`.290` / `.291` 已被占用，已避开）。

### 回归守卫

`daoyu.220` 的选项区域是**易冲突区**（多个功能都把选项插在「成为统治者」附近），
因此每次提交前都要对以下不变量做硬性核对，任何一项失败都必须先修好再提交：

| 守卫项 | 检查内容 |
|---|---|
| 功能二仍存在 | `daoyu.220` 含 `name = daoyu_set_character_as_ig_leader#成为利益集团领袖` |
| 确认分支仍存在 | `daoyu.222` 含 `scope:daoyu_target_character = { set_as_interest_group_leader = yes }` |
| 提示行仍存在 | `daoyu.222` 的 `flavor` 含 `has_global_variable = daoyu_set_character_as_ig_leader` |
| 变量仍被清理 | `daoyu.222` 的 `after` 含 `remove_global_variable = daoyu_set_character_as_ig_leader` |
| 意识形态菜单用专属分页 | `daoyu.250` 含 `daoyu_ra_check_page_next_ci = yes` |
| 未误改共用分页 | 「利益集团意识形态」菜单仍用基础 Mod 的 `daoyu_check_page_next` |
| 未新增 GUI 覆盖 | 仓库内不存在 `gui/` 目录 |
| 按钮无图标 | 新交互文件中不出现 `icon = ` |

此外，每次提交前应对**每个被覆盖的上游文件**按唯一行做一次 `Counter` 差分，
确认「减少的行」全部在 README 的等价替换表中登记过——本次的回归正是这样发现的。

### 尚未验证

- **未在游戏内启动验证。** 所有结论均来自脚本、引擎字符串与上游文件比对，未运行游戏。
- 具体待实测项：
  1. 「立即邀请」是否确实绕过 5 年冷却（引擎对自定义交互不写 `cooldown` 的容忍度）；
  2. `create_political_movement` 在目标国为玩家国自身时是否被引擎额外限制；
  3. `add_modifier = { name = daoyu_country_agitator_slots_add days = 30 }` 是否足以腾出鼓动者槽位
     使 `transfer_character` 成功（该修正为**限期 30 天**的临时槽位）；
  4. 「指定政治运动」菜单中同一类型的运动已被解散 / 不可创建时的表现；
  5. 93 项意识形态分 6 页翻页时，页 4/5/6 的显示是否正确。

### 已知风险

- **目标 scope 随机化（法律菜单）**：若同时有多个国家带 `daoyu_target_country` modifier，
  `d_law_tec.720` 的随机刷新可能让法律选项落到另一个国家。这是独立于单项法律条件的状态风险。
- **目标角色随机化（角色菜单）**：`daoyu_target_character` 是挂在角色上的**condition 特质**
  （`possible = { age > 999 }`，正常游戏不会自然获得）。若同时有多个角色带该标记，
  菜单会随机取其中一个。本补丁把候选集从「本国角色」扩展为「本国角色 ∪ 流亡池角色」，此随机性依然存在。
- **`daoyu_target_character` 在某些事件中未被清理**：基础 Mod 自身就有此情况，本补丁未改动其语义。
- **邀请 / 转职后不保证立刻加入运动**：角色能否参加某个运动最终由引擎按
  `character_support_trigger` / `character_support_weight` 判定。本补丁做的是
  「创建相配的运动 + 让角色的意识形态落在该运动认可的范围内」，而不是强行写入归属——
  引擎没有这样的接口（见功能五）。
- **「指定政治运动」对非鼓动者会自动转职**。若只想改运动而不想改职位，请勿对政治家使用；
  政治家想换运动请先手动决定是否接受转职。

### 未部署

按用户要求，本补丁**只修改了 `E:\Project\Mod\victoria3\daoyu_cheat_ra` 工程**，
未部署到游戏 Mod 目录，因此当前正在生效的 Mod 组合未被影响。

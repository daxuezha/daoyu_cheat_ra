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
| 1 | **补齐 `real_history` 法律菜单** | 法律菜单 → 各法律组 | 把 `real_history` 中 40 项带 `group` 的自定义法律 + `law_factory_councils` 全部做成可选项 |
| 2 | **「成为利益集团领袖」** | 管理角色 → 目标角色操作 | 让目标角色成为其所属利益集团的领袖（`set_as_interest_group_leader`） |
| 3 | **让流亡角色也能打开刀鱼菜单** | 人物头像**右键菜单** | 修好「管理角色」在流亡池角色上的失效（`owner` 为空导致点击无声） |
| 4 | **流亡者的三个按钮** | 人物头像**右键菜单** | 「立即邀请」（新增，置顶）+「筹划意外（流亡者版）」（新增）+「管理角色」；原版互动完全不动 |
| 5 | **转职 / 招募 / 指定政治运动** | **管理角色**界面 | 把所有人员补充类操作集中到管理角色里；指定运动支持**单人指定**与分页 |
| 6 | **补齐 RA 角色意识形态菜单** | 管理角色 → 意识形态 | 把 `real_history` 新增的 48 项角色意识形态补进「变更意识形态」菜单 |

> 功能 3–6 为 v1.1.0 起新增。**流放者池界面保持原版**，本补丁不提供任何 `gui/` 文件。

### 术语：邀请 ≠ 招募 ≠ 转职

| 动作 | 含义 | 出现位置 |
|---|---|---|
| **邀请** | 把鼓动者立刻放进本国的**鼓动者槽位** | 右键菜单「立即邀请」 |
| **招募** | 让角色担任**政府职位**（政治家 / 将领 / 提督） | 管理角色界面 |
| **转职** | 卸任现有职位，把角色**变成鼓动者** | 管理角色界面 |

> 右键菜单只保留原版刀鱼那套 + 流亡者的三个按钮；**所有新增的人员操作都在管理角色界面里**。

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
│  │  └─ daoyu_character_interactions.txt   # 覆盖基础 Mod 同名文件（+1 个右键交互：立即邀请）
│  ├─ scripted_effects/
│  │  ├─ daoyu_effects.txt                  # 覆盖基础 Mod 同名文件（意识形态、运动映射、人员操作）
│  │  └─ daoyu_tec_and_law_effects.txt      # 菜单变量清理效果
│  └─ scripted_triggers/
│     └─ daoyu_triggers.txt                 # 覆盖基础 Mod 同名文件（+新增触发条件）
├─ events/
│  ├─ daoyu_events.txt                      # 覆盖基础 Mod 同名文件（角色菜单 + 意识形态菜单 + 人员选项）
│  ├─ daoyu_ra_events.txt                   # 新文件：指定政治运动菜单（daoyu.292，3 页）
│  └─ daoyu_tec_and_law_events.txt          # 覆盖基础 Mod 同名文件（法律菜单）
└─ localization/             # 11 种语言的补丁本地化
   ├─ braz_por/  english/  french/  german/  japanese/  korean/
   └─ polish/  russian/  simp_chinese/  spanish/  turkish/
```

> 标注「覆盖」的文件均以相同相对路径整体替换基础 Mod / 原版文件，且相对上游**只做追加**。
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

> **流放者池界面保持原版。** 本补丁**不提供任何 `gui/` 文件**。
> 流放者条目的按钮（截图红框）是原版自带的邀请按钮；刀鱼按钮（截图蓝框）来自
> **人物头像右键菜单**，该菜单由引擎从 `Character.GetInteractions` 自动生成
> （`character_panel.gui` 里的 `RightClickMenuManager.ShowForCharacter`），
> 只要交互的 `potential` 成立就会自动出现，**无需改任何 GUI 文件**。

---

## 功能四：右键菜单与管理角色界面的分工

### 分工原则

| 位置 | 放什么 |
|---|---|
| **人物右键菜单** | **只放原版刀鱼那套，一个都不动**；此外流亡角色另加两个（见下）。本补丁**不往右键菜单新增任何其他人事互动** |
| **管理角色界面**（`daoyu.220`） | 所有**针对单人**的人员操作：转为鼓动者、指定政治运动、招募为政治家 / 将领 / 提督 |

### 各角色类型的右键菜单实际内容

**本国角色** —— 完全是原版那套，本补丁未加、未删、未改：

| 按钮 | 显示条件 |
|---|---|
| 流放异见分子 | 政治家 / 鼓动者 |
| 筹划意外 | 非流亡、非统治者、非继承人 |
| 开启作弊 | 统治者 |
| 管理角色 | 原版逻辑 |
| 招募将领 | 在编队中创建指挥官 |
| 批量管理（陆军 / 海军） | 统治者或对应军种将领 |

**流亡角色** —— 原版对流亡者**没有可用交互**。这里有个容易判断错的点：

```text
potential = { owner ?= { is_ai = no } }      # 管理角色 原来的写法
```

`?=` 的语义是**「作用域存在 且 满足条件」**，不是「不存在就算通过」。
流亡池角色没有 `owner` → 作用域不存在 → 整行返回**假** → **按钮根本不显示**。
（同一份 `potential` 里的 `effect` 还写了 `owner = { ... }`，就算强行触发也找不到 owner，
所以对流亡者既看不到、也点不动。）

本补丁为流亡者补上三个，正好是需要的三个：

| 按钮 | 来源 | 备注 |
|---|---|---|
| **立即邀请** | **本补丁新增** | **置顶** |
| **筹划意外（流亡者版）** | **本补丁新增**（`daoyu_ra_arrange_accident_exile`） | 见下 |
| **管理角色** | 原版互动 + 本补丁的修复 | 见下 |

`管理角色` 的修复（只动这一个互动，其余 6 个原版互动逐字不动）：

```text
potential = {
	# ?= = 「作用域存在 且 满足条件」；流亡者没有 owner，故补 scope:actor 这条路径
	or = {
		owner ?= { is_ai = no }
		scope:actor ?= { is_ai = no }
	}
	has_global_variable = daoyu_on
	not = { has_variable = daoyu_dead }
}
effect = {
	this = { add_trait = daoyu_target_character }
	set_global_variable = daoyu_character_interactions
	save_scope_as = daoyu_ra_clicked_character
	if = { limit = { is_in_exile_pool = yes }
		scope:actor = { set_variable = daoyu_target_country
			set_variable = { name = daoyu_ra_targeted_character value = scope:daoyu_ra_clicked_character }
			trigger_event = { id = daoyu.220 popup = yes } } }
	else = { owner = { ... 同上 ... } }
}
```

> 流亡者**没有**「流放异见分子」按钮 —— 既因为原版 `daoyu_exile_character` 的
> `potential` 里有 `is_in_exile_pool = no`，也因为对已经在流亡池里的人再「流放」没有意义。
> 该互动**原样未动**。

### 为什么「筹划意外（流亡者版）」要单独做一个

原版 `daoyu_arrange_accident` 的 `potential` 里有 `is_in_exile_pool = no`，
流亡者看不到它。为了**完全不动原版互动**，本补丁另起一个只在
`is_in_exile_pool = yes` 时成立的独立互动：

```text
daoyu_ra_arrange_accident_exile = {
	clicksound = "event:/SFX/DLC/1.3_ip1/UI/exile_character"
	potential = {
		is_in_exile_pool = yes
		is_in_void = no
		is_ruler = no
		is_heir = no
		not = { has_variable = daoyu_dead }
		scope:actor ?= { is_ai = no has_global_variable = daoyu_on }
	}
	show_requirements = yes
	effect = {
		custom_tooltip = { text = daoyu_arrange_accident_tt }
		set_variable = daoyu_dead
		kill_character = { hidden = yes }
	}
	show_confirmation_box = yes
}
```

**效果本体与原版一字不差**，因为：

- 原版 `effect` 里只用到 `this`（= 角色），**完全不依赖 `owner`**，对流亡者天然可用；
- 原版自己就杀流亡者（`events/agitators_events/exiles_events.txt` 有 3 处对流亡角色
  `kill_character = yes`），引擎支持。

**已知边界（与原版一致，非新增风险）**：

- `is_immortal = yes` 的角色（梅特涅那类剧情人物，全游戏 22 处）会被引擎忽略，
  表现为「点了没反应」；
- 原版本地化自己也警告过：*「杀死角色后请立即退出该角色界面，否则会引起大量且持续的报错」*
  —— 这是 UID 层面的已知问题，流亡者列表刷新前条目会残留，**所以确认框和红字警告都保留**。

### 术语：邀请 ≠ 招募 ≠ 转职

| 动作 | 含义 | 出现位置 |
|---|---|---|
| **邀请** | 把鼓动者立刻放进本国的**鼓动者槽位** | 右键菜单「立即邀请」 |
| **招募** | 让角色担任**政府职位**（政治家 / 将领 / 提督） | 管理角色界面 |
| **转职** | 卸任现有职位，把角色**变成鼓动者** | 管理角色界面 |

### 三个「静默失败」的坑（都修过了）

引擎对脚本错误**大多不报错**，只让效果失效。本补丁踩过三次，记录如下：

| 症状 | 真正原因 | 修法 |
|---|---|---|
| 指定政治运动后，**整个刀鱼作弊失效** | `daoyu.292` 退出时没有交回上一层 `daoyu.220`，导致 `daoyu_character_interactions` 全局变量与 `daoyu_target_character` 特质一直挂着 | 退出时 `set_variable = daoyu_return`，由 `after` 触发 `daoyu.220`（与基础 Mod `daoyu.250` 同写法） |
| **招募为政治家 / 将领 / 提督完全没反应** | `replace_character_roles` 的角色名必须是**裸标识符**；我用 `ROLE = "politician"` 传入，代入后成了 `add = { "politician" }`，角色名解析失败且不报错 | 拆成三个独立效果，角色名全部裸写（`add = { politician }`），不再做字符串代入 |
| **立即邀请对部分角色无效**（如 RA 的毛泽东） | 原版 `invite_exile` 的 `possible` 要求 `can_agitate`，即该角色在目标国**至少能参加一个运动**。毛泽东的 `ideology_maoist` 不被任何运动的 `character_support_trigger` 接受，于是邀请落空 | 邀请前先临时换成保底意识形态，邀请后**延迟 1 天还原**（见下） |

### 「立即邀请」为什么要临时改意识形态

**引擎限制**：脚本侧**没有读取角色当前意识形态的接口**
（`Character.GetIdeology` 只存在于 GUI 数据模型，脚本不可用）。
所以只能逐个 `has_ideology` 穷举来记住原值：

```text
# 保存（93 项各一个标记）+ 换成保底意识形态
daoyu_ra_save_ideology_effect = {
	if = { limit = { has_ideology = ideology:ideology_xxx } set_variable = daoyu_ra_saved_ideology_ideology_xxx }
	...
	set_ideology = ideology:ideology_communist     # 保底：movement_socialist / movement_communist 都接受
}

# 还原
daoyu_ra_restore_ideology_effect = {
	if = { limit = { has_variable = daoyu_ra_saved_ideology_ideology_xxx }
		remove_variable = daoyu_ra_saved_ideology_ideology_xxx set_ideology = ideology:ideology_xxx }
	...
}
```

邀请流程：

```text
1. daoyu_ra_save_ideology_effect          # 记住原意识形态，换成保底意识形态
2. 补鼓动者槽位（若不足）
3. transfer_character = 邀请国             # 此时 can_agitate 成立，邀请有效
4. free_from_exile_effect
5. 记下角色，trigger_event = { id = daoyu.293 days = 1 }   # 延迟 1 天还原
```

**还原是立即执行的，不需要延迟。** 理由：

- 脚本效果是**顺序同步执行**的。`can_agitate` 只在交互的 `possible` 里被求值一次，
  而那时（本效果开始之前）保底意识形态已经在角色身上，判定**已经通过**；
  效果链本身不会在结尾再重新求值一次「能不能鼓动」。
- **反证**：vanilla `invite_exile` 自己的顺序就是
  `transfer_character → set_variable → add_career_length → free_from_exile_effect`，
  而 `free_from_exile_effect` 内部的 `character_traditionalist_switch_effect` 会
  **当场** `set_ideology` 覆盖掉角色的意识形态 —— 它照样正常工作。
  如果引擎真的会「延迟重判」，vanilla 这套早就坏了。

所以流程简化为：换意识形态 → 补槽位 → `transfer_character` → `free_from_exile_effect`
→ **当场还原**。

> 早期版本曾把「待还原的角色」记在**国家变量**里并延迟 1 天由事件还原，有两个问题：
> ①连续邀请第二个人会覆盖第一个人，前者的临时意识形态永远还原不回去；
> ②多引入了一个事件与一个计时器。现在直接对**本人**立即还原，
> 不经过任何中间变量，两个问题都不存在。

> **副作用（已知且有意）**：还原后角色回到原意识形态（如 `ideology_maoist`），
> 此时它**仍然不能参加任何运动**。所以「立即邀请」能让毛泽东进入鼓动者槽位，
> 但他是否真能挂上运动，取决于该国是否存在他能参加的运动 —— 这正是
> 「管理角色 → 指定政治运动」的用途。

### 授予 / 剥夺指挥权（只动职位，不把人踢出人物池）

原版只提供了：

| 原版交互 | 谁能用 |
|---|---|
| `grant_command_to_ruler` | 统治者 |
| `remove_command_from_ruler` | 统治者 |
| `grant_command_to_agitator` | 鼓动者 |
| `retire_commander` | 现役将领 / 提督 |

两个缺口：

1. **政治家**没有对应的「授予指挥权」；
2. `retire_commander` 的 effect 是

   ```text
   replace_character_roles = { remove = { general admiral } }
   retire_character_if_should_be_culled = yes
   ```

   第二行会**把角色直接淘汰出人物池**——表现就是「点了退役之后，这个人在角色界面
   再也找不到、也不能操作了」。

本补丁补上四组交互（陆军 / 海军 × 授予 / 剥夺），政治家与鼓动者都能用：

| 交互 | 效果 |
|---|---|
| `daoyu_ra_grant_army_command` | `add_character_role = general`，原有身份保留 |
| `daoyu_ra_remove_army_command` | `unassign_from_formation` + `remove_character_role = general` |
| `daoyu_ra_grant_navy_command` | `add_character_role = admiral` |
| `daoyu_ra_remove_navy_command` | `unassign_from_formation` + `remove_character_role = admiral` |

**关键点**：剥夺只用 `remove_character_role`（只移除这一个职位），
**绝不调用** `retire_character_if_should_be_culled`，所以角色始终留在人物池里，
可以继续在「管理角色」界面操作。解除编队用 `unassign_from_formation`，
并保留原版的 `commander_military_formation ?= { is_mobilized = no }` 前提。

这四个按钮**同时**出现在人物右键菜单与管理角色界面的选项里（同一套逻辑）。
使用前会检查本国是否有对应的 HQ（陆军看 `owner.capital.land_hq`，海军看 `owner.capital.naval_hq`）。

### 5 年限制来自哪里

原版 `invite_exile`（`00_character_interactions.txt`）带有

```text
cooldown = { days = normal_modifier_time }   # normal_modifier_time = 1825 天 = 5 年
```

且 `possible` 中还有 `can_agitate = scope:actor` 等条件。冷却按「交互 + 国家」计时，
所以刚招募过一名鼓动者后，5 年内无法再通过该交互招募任何人——这就是界面上的
「最近 5 年招募过政治鼓动者后不得招募」。

### 「立即邀请」的实现

`daoyu_ra_invite_agitator_now` **完全不声明 `cooldown`**，也不检查 `can_agitate`：

```text
potential = {
	is_in_exile_pool = yes
	has_role_of_type = agitator
	daoyu_ra_can_invite_trigger = yes      # 只要求「我是玩家且刀鱼已开启」
}
```

效果 `daoyu_ra_invite_agitator_effect`：

```text
add_career_length = { role = agitator months = 60 random_range = { 1.0 1.5 } }
# 鼓动者槽位不足时临时补 1 个（复用基础 Mod 已有的静态修正）
scope:daoyu_ra_mv_country = {
	if = { limit = { empty_agitator_slots < 1 } add_modifier = { name = daoyu_country_agitator_slots_add days = 30 } } }
# 直接搬角色（无视原版 cooldown / can_agitate）
transfer_character = scope:daoyu_ra_mv_country
set_variable = { name = daoyu_ra_invited_var days = 90 }
# 脱离流亡状态
free_from_exile_effect = yes
```

> 「立即邀请」**不自动帮角色挑运动**。想指定运动请到「管理角色 → 指定政治运动」，
> 职责更清晰：邀请负责「把人拉进槽位」，运动负责「让他有运动可参加」。

### 「针对某个人物」是怎么保证的

原来 `daoyu.220` 用 `random_character = { limit = { has_trait = daoyu_target_character } ... }`
**随机取一个带标签的角色**——标签没清掉时，点了 A 却可能作用在 B 身上。

现在改为**精确指向**：点「管理角色」时把这次点选的角色存成发起国的变量，
菜单只认这个变量：

```text
# 交互 effect
this = { add_trait = daoyu_target_character }
save_scope_as = daoyu_ra_clicked_character
owner = {                                     # 流亡者走 scope:actor
	set_variable = daoyu_target_country
	set_variable = { name = daoyu_ra_targeted_character value = scope:daoyu_ra_clicked_character }
	trigger_event = { id = daoyu.220 popup = yes } }

# daoyu.220 immediate
if = { limit = { <该国已有该变量> } ... var:daoyu_ra_targeted_character = { save_scope_as = daoyu_target_character } }
else_if = { limit = { any_character_in_exile_pool = { ... } } <基础 Mod 原逻辑> }
else = { <基础 Mod 原逻辑> }
```

> 基础 Mod 原有的随机取角色逻辑作为 `else_if` / `else` **完整保留**，
> 所以不改变原有行为；离开菜单（`RESIGN`）时清掉该变量，避免下次残留。

---

## 功能五：指定政治运动（单人指定 + 分页）

### 引擎能力边界

对游戏脚本与 `binaries/victoria3.exe` 的字符串做了核对：

| 能力 | 是否存在 | 依据 |
|---|---|---|
| 把角色**指派**到某个政治运动 | **不存在** | 只有 `create_political_movement` / `bolster_political_movement` / `suppress_political_movement`（国家作用域）；无 `join_movement` / `set_movement` 之类 |
| 角色能否支持某运动（只读） | 存在 | `character_supports_political_movement`、`could_support_political_movement` |
| 创建指定类型的政治运动 | 存在 | `create_political_movement = { type = x [religion = <scope>] [culture = <scope>] }` |

> **参数名是 `type`，不是 `movement_type`。** 原版全部调用形如
> `create_political_movement = { type = movement_radical }`。
> 用错参数名时游戏**不报错也不生效**，表现为「点了按钮什么也没发生」。
>
> （引擎内部的帮助字符串里写的是 `movement_type`，那是**文档笔误**；
> 真正的解析器只认 `type`，字符串 `Invalid political movement type specified in
> create_political_movement effect` 才是真正的报错路径。）

角色的运动归属由引擎按 `common/political_movements/*.txt` 的 `character_support_trigger`
**自动判定**，脚本无法直接写入。

### 旧版本的缺陷：一选就「全员加入」

旧实现把**全国家所有角色的意识形态**一起改成某个运动的意识形态
（`every_scope_character = { ... set_ideology = ... }`），
结果一旦指定，本国所有原本不打算参加该运动的角色也一起被卷了进去。

### 现在的做法：只改目标角色一人

1. **创建运动**（若目标国还没有该类型）。已存在时该选项**不再被隐藏**，可以直接沿用。
2. 只把**目标角色**的意识形态改成该运动 `character_support_trigger` 所认可的那一个：

   ```text
   daoyu_ra_mv_apply_ideology_effect = {
       scope:daoyu_target_character = {
           set_variable = daoyu_ra_mv_req_$MOVEMENT$
           daoyu_ra_mv_ideology_map = yes          # 内部只对这个角色 set_ideology
           remove_variable = daoyu_ra_mv_req_$MOVEMENT$
       }
   }
   ```

   `daoyu_ra_mv_ideology_map` 里**只有 `set_ideology`，没有任何 `every_*` 批量作用域**。

3. 映射表是从各运动的 `character_support_trigger` **反推**出来的，并且只挑
   「该意识形态**单独**就能满足该 trigger」的那一个。这点很关键：

   | 情况 | 例子 | 处理 |
   |---|---|---|
   | 意识形态单独即可 | 多数运动 | 直接 `set_ideology` |
   | 意识形态藏在 `AND` 里 | `movement_tokugawa_loyalist` | 取同一个 `AND` 里的另一个可用意识形态（`ideology_authoritarian`） |
   | 只看身份（宗教 / 文化），无意识形态可用 | `movement_german_national`（只看日耳曼heritage）、`movement_hindu_nationalist`（南亚 heritage + 印度教） | 创建运动时传入目标角色的 `religion` / `culture` 身份；意识形态不保证命中，菜单里已注释说明 |

### 分页

45 种运动（原版 39 + `real_history` 6），每页 15 种，共 3 页。
结构与基础 Mod 的意识形态菜单（`daoyu.250`）保持一致：

| 位置 | 选项 | 显示条件 |
|---|---|---|
| 列表**最前** | `PREVIOUS_PAGE#上一页` | 当前页是第 2 或第 3 页 |
| 列表内容 | 该页的 15 种运动 | 当前页标记匹配 |
| 列表**最后** | `NEXT_PAGE#下一页` | 当前页是第 1 或第 2 页 |
| 最后 | `RESIGN`（唯一的退出） | 始终 |

* 页标记是**单热**的（同一时刻只有一个 `daoyu_ra_mv_page_0N`），
  与基础 Mod 的 `daoyu_determine_page_*` 同构。
* 翻页由脚本效果 `daoyu_ra_mv_page_prev` / `_next` 移动页标记后**重开本事件**实现，
  这样每次只渲染一个「上一页」、一个「下一页」、一个「退出」。
* 退出用原版已有的 `RESIGN`（`QUIT` 不再使用）。

> **踩过的坑**：选项的显示条件必须写 `trigger`。
> 曾把 `ai_chance = { base = 100 }` 当成显示条件，结果「退出」在每页都渲染，
> 界面上出现**三个退出按钮**；点到"假"的那两个只会执行清理效果、却不关事件，
> 导致页标记被清空而界面卡住，看起来就像"刀鱼作弊的逻辑被破坏了"。
> 现在三个退出已合并为一个，且全部用真正的 `trigger` 控制显隐。

### 自动匹配

第 1 页顶部的「自动匹配运动」会按目标角色**当前**意识形态，在映射表里找到第一个
已经满足条件的运动并创建它；命中即停（用 `daoyu_ra_mv_done` 作标志），
不会重复创建多个。

---

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
| `events/daoyu_tec_and_law_events.txt` | 为 40 项 `real_history` 分组法律增加 `activate_law` 菜单项；按法律定义归组；添加中文行尾注释；为美国金融法律组增加仅对美国目标国显示的入口；对有明确国家 / 文化范围的法律选项添加目标国家限制 |
| `common/scripted_effects/daoyu_tec_and_law_effects.txt` | 在菜单变量清理效果中清除 `lawgroup_ra_usa_financial_system` |
| `events/daoyu_events.txt` | ①「成为利益集团领袖」选项 + `daoyu.222` 确认分支与变量清理（**完整保留**）；②48 项 RA 角色意识形态选项；③11 处目标角色解析改为优先取流亡池；④`daoyu.250` 翻页改调 `daoyu_ra_check_page_next_ci`；⑤`daoyu.220` 新增 5 个**人员补充**选项：转为鼓动者 / 指定政治运动 / 招募为政治家 / 将领 / 提督 |
| `common/character_interactions/daoyu_character_interactions.txt` | ①新增右键交互 `daoyu_ra_invite_agitator_now`（立即邀请），放在**文件最前**以保证置顶；①′新增四组「授予 / 剥夺指挥权」交互（陆军 / 海军 × 授予 / 剥夺），政治家与鼓动者可用，剥夺只移除职位、不淘汰角色；②新增 `daoyu_ra_arrange_accident_exile`（筹划意外·流亡者版），放在**第二**位；③`daoyu_interactions_manage_character` 的 `effect` 增加流亡池分流 + 精确目标变量；④**移除**此前误加进右键菜单的 转化 / 指定运动 / 三种招募 —— 已改为管理角色界面的选项。**原版其余 6 个互动逐字未改**（校验脚本按块比对确认）。 |
| `common/scripted_effects/daoyu_effects.txt` | ①分页扩至 6 页；②RA 意识形态清单与单项宏；⑥`daoyu_ra_save_ideology_effect` / `daoyu_ra_restore_ideology_effect`（93 项穷举式保存还原）；⑦招募拆成 `daoyu_ra_recruit_politician/general/admiral_effect` 三个裸名效果；③政治运动相关：`daoyu_ra_mv_ideology_map`（45 种运动 → 单一合法意识形态）、`daoyu_ra_mv_apply_ideology_effect`（**只改目标角色一人**）、`daoyu_ra_mv_auto_effect`、`daoyu_ra_mv_page_next` / `_prev` / `_exit`、`daoyu_ra_open_movement_menu_effect`；④人员效果：`daoyu_ra_invite_agitator_effect`、`daoyu_ra_convert_to_agitator_effect`、`daoyu_ra_recruit_role_effect` 及三个菜单入口包装 `daoyu_ra_mv_convert_effect` / `_enter_effect` / `_recruit_effect`；⑤`daoyu_ra_set_ig_leader_effect` |
| `common/scripted_triggers/daoyu_triggers.txt` | `daoyu_ra_check_page_next_ci`（意识形态菜单分页）、`daoyu_ra_cheat_enabled_trigger` 与 4 个 `daoyu_ra_can_*_trigger`（选项 / 交互可见性）；**不改动**基础 Mod 既有的 `daoyu_check_page_next` |
| `events/daoyu_ra_events.txt` | **新文件**：`daoyu.292` 指定政治运动菜单 —— 45 种运动分 3 页（每页 15 种），带上一页 / 下一页 / 退出（退出会交回 `daoyu.220`），以及「自动匹配运动」 |
| `localization/*/daoyu_ra_l_*.yml` | 11 种语言；所有按钮名统一用 `#V 刀鱼作弊#!`（中文）/ `#V Daoyu Cheat#!`（英文）前缀；补上原版缺失的运动名键 |
| `.metadata/metadata.json` | 版本升至 `1.2.2`，更新简介 |

### 不新增、不修改 `gui/`

本补丁**不提供任何 `gui/` 文件**。流放者池界面与人物右键菜单都完全由原版与引擎驱动，
理由见[功能三](#功能三流亡角色的刀鱼菜单入口)。

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
- 逐行差分校验，确认差异全部为纯新增。

### 右键菜单的顺序由「交互定义顺序」决定

引擎用 `Character.GetInteractions` 枚举交互，按**定义顺序**渲染。所以：

| 位置 | 交互 | 原因 |
|---|---|---|
| 第 1 | `daoyu_ra_invite_agitator_now` | 「立即邀请」必须置顶 |
| 第 2 | `daoyu_ra_arrange_accident_exile` | 「筹划意外（流亡者版）」紧随其后 |
| 第 3 起 | 原版 7 个互动，顺序不变 | 原版顺序原样保留 |

这一顺序在回归守卫里被锁定，改动该文件时不要挪动前两个。

### 文件编码是硬性约束（务必遵守）

| 文件类型 | BOM | 换行 |
|---|---|---|
| 上游 `*.txt` / `*.gui` | **无 BOM** | CRLF |
| 本地化 `*.yml` | **有 BOM** | CRLF |

基础 Mod 与本补丁引用的原版脚本均为「无 BOM + CRLF」。**写入 BOM 会导致整个文件解析失败，
且游戏不会给出任何报错**：表现为该文件里的菜单项点击后毫无反应。

> 本补丁开发过程中踩过两次这个坑：
> `events/daoyu_ra_events.txt` 被写成 BOM + CRLF，导致「指定政治运动」点了不弹窗、不报错；
> 重复写入 BOM（`EF BB BF EF BB BF`）同样会使文件失效。两者都在校验脚本里显式检查。

### 本地化格式

- 本地化文件为 UTF-8 **带 BOM**、CRLF，键值缩进 1 个空格，与原版及基础 Mod 一致。
- **前缀规则（重要）**：`#V 刀鱼作弊#!<按钮名>` 只加在**人物右键菜单**的交互上，
  因为那些按钮是"嫁接"在原版游戏的按钮菜单里的，需要前缀把刀鱼的东西和原版区分开。
  已经收进「管理角色」事件的选项**本身就是刀鱼作弊的功能**，因此**不加前缀**。

  | 按钮 | 位置 | 前缀 |
  |---|---|---|
  | `daoyu_ra_invite_agitator_now` 立即邀请 | 右键菜单 | **要** |
  | `daoyu_ra_arrange_accident_exile` 筹划意外 | 右键菜单 | **要** |
  | `daoyu_ra_convert_to_agitator` 转为鼓动者 | 管理角色选项 | 不要 |
  | `daoyu_ra_assign_movement` 指定政治运动 | 管理角色选项 | 不要 |
  | `daoyu_ra_recruit_as_politician/general/admiral` | 管理角色选项 | 不要 |
  | `daoyu_ra_mv_auto` 自动匹配运动 | 运动菜单选项 | 不要 |

- 新增交互**一律不写 `icon =`**：基础 Mod 的 7 个交互全部没有 `icon`，
  原版 `invite_exile` 也是注释掉的 `# icon = ""`；写了反而与刀鱼既有按钮风格不一致。

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

- `real_history/common/laws` 中有 `group` 的法律定义与菜单项逐项比对：**40 / 40 均已覆盖**。
- `law_factory_councils` 菜单触发条件与启用效果均使用 `lawgroup_labour_associations`。
- 法律选项 ID 唯一、同法重复启用保护、中文注释、CRLF 换行与大括号平衡均通过。
- 变量 `daoyu_set_character_as_ig_leader` 在基础 Mod 中无重名。

**v1.2.x**

- 相对上游的逐行差分（按**唯一行**统计）：

  | 文件 | 上游行数 | 本补丁行数 | 新增行 | 删除行 |
  |---|---|---|---|---|
  | `common/scripted_triggers/daoyu_triggers.txt` | 2641 | 2720 | 79 | **0** |
  | `common/character_interactions/daoyu_character_interactions.txt` | 323 | 369 | 47 | 1（等价替换） |
  | `common/scripted_effects/daoyu_effects.txt` | 2209 | 3099 | 893 | 3（等价替换） |
  | `events/daoyu_events.txt` | 14262 | 14410 | 160 | 12（等价替换） |

- 上述「删除行」全部是**已登记的等价替换**：

  | 文件 | 被替换的内容 | 替换为 | 原因 |
  |---|---|---|---|
  | `events/daoyu_events.txt` | `random_character = { ... }`（10 处 + 1 处 `immediate`） | 先查流亡池、否则查本国角色 | 原写法在流亡池角色上解析不到目标（功能三） |
  | `events/daoyu_events.txt` | `daoyu.250` 下一页的 `daoyu_check_page_next` | `daoyu_ra_check_page_next_ci` | 该菜单扩到 93 项 / 6 页（功能六） |
  | `common/scripted_effects/daoyu_effects.txt` | 页 4 判定 `var:daoyu_count > 40`（无上限） | 增加上限 `<= 53` | 为页 5 / 页 6 让出区间（功能六） |
  | `common/scripted_effects/daoyu_effects.txt` | 两条可见性判定 `any_character = { ... }` | `OR = { any_character ... any_character_in_exile_pool ... }` | 流亡池角色也要能出现在菜单里（功能三） |
  | `common/character_interactions/daoyu_character_interactions.txt` | `daoyu_interactions_manage_character` 的 `owner = { ... }` | 流亡池时改走 `scope:actor` | 流亡角色没有 `owner`，原写法点击后毫无反应（功能三） |

- 全部脚本大括号配平；补丁内 6 个 `.txt` 与 11 个 `.yml` 编码检查全部通过
  （`.txt` 无 BOM / `.yml` 有 BOM / 全部 CRLF），不存在重复 BOM。
- 引用的所有 `daoyu_ra_*` 效果 / 触发 / 交互名均有定义，无悬空引用；
  6 个补丁触发全部被引用，无死代码。
- 全部 115 处 `create_political_movement` 都使用正确的 `type =` 参数，
  不再出现错误参数名 `movement_type =`（该错误会导致点击后静默无效）。
- **运动覆盖**：`common/political_movements`（原版 39 + `real_history` 6）共 **45 种**，
  `daoyu.292` 全部列出，每页 15 种 × 3 页；其中 6 种属于需要身份类别的运动
  （`movement_category_religious` / `movement_category_cultural`），已分别传入 `religion =` / `culture =`。
- **意识形态映射**：`daoyu_ra_mv_ideology_map` 的每条映射都是从对应运动的
  `character_support_trigger` 反推的，并确认该意识形态**单独**即可满足该 trigger；
  `movement_tokugawa_loyalist`（意识形态藏在 `AND` 内）取同 `AND` 内的
  `ideology_authoritarian`；6 个只看身份的运动在菜单里以注释标注。
- **单人指定**：`daoyu_ra_mv_ideology_map` 内**只有 `set_ideology`，没有任何 `every_*`
  批量作用域**（守卫项 `single_character_only` 强制检查），因此不会把其他角色卷进运动。
- 已存在的运动不再让选项消失：选项触发条件只判断「当前页」与「该运动尚未存在」；
  运动已存在时直接沿用，角色仍会被赋予参加资格。
- `real_history` 角色意识形态核对：`common/ideologies` 下共 **50 项** `character_ideology = yes`，
  其中 **48 项**为原版没有的，已全部补入菜单（`daoyu.250` 共 93 项 / 6 页）。
- 基础 Mod 的 `daoyu_check_page_next` **未被改动**（仍为 16 页），共用它的
  「利益集团意识形态」菜单与 `daoyu_long_events.txt` 的 3 个菜单行为不变。
- 新增 / 修改的本地化键在 11 种语言文件中均存在，无缺失；
  简中比英文多 5 个键（`movement_progresista` / `movement_usfp_eugenics` / `movement_usfp_progressive` / `ideology_grefm_constitutionalist` 及其 `_desc`），
  因为 `real_history` 只为这些内容提供了英文名，简中中文名由本补丁补上，属预期差异。
- 导航键 `PREVIOUS_PAGE` / `NEXT_PAGE` / `QUIT` 均使用原版已有本地化，无需补键。
- 新事件 ID `daoyu.292` 与基础 Mod 全部事件 ID 无冲突。

### 回归守卫

`daoyu.220` 的选项区域与 `daoyu_character_interactions.txt` 是**易冲突区**，
每次提交前都要逐项核对，任何一项失败都必须先修好再提交：

| 守卫项 | 检查内容 |
|---|---|
| **原版互动逐字未改** | 把每个原版互动的整块文本与上游 `3790567715` 做**按块**比对：除 `daoyu_interactions_manage_character` 外必须**完全一致** |
| 右键菜单顺序 | 第 1 = 立即邀请，第 2 = 筹划意外（流亡者版），其后为原版 7 个且顺序不变 |
| 右键菜单无新增人事互动 | 不存在 `daoyu_ra_convert_to_agitator = {` / `_assign_movement = {` / `_recruit_as_* = {` |
| 精确目标已接入 | 交互与 `daoyu.220` 都出现 `daoyu_ra_targeted_character` |
| 原随机回退保留 | `daoyu.220` 仍有 `any_character_in_exile_pool = { has_trait = daoyu_target_character }` 分支 |
| 目标变量会清理 | `daoyu.220` 的 `RESIGN` 里有 `remove_variable = daoyu_ra_targeted_character` |
| 功能二未被误删 | `daoyu.220` 含 `name = daoyu_set_character_as_ig_leader#成为利益集团领袖` |
| 确认分支未被误删 | `daoyu.222` 含 `set_as_interest_group_leader = yes` |
| 管理角色含 5 个新选项 | 转为鼓动者 / 指定政治运动 / 招募为政治家 / 将领 / 提督 |
| 运动菜单完整 | `daoyu.292` 含 45 个 `name = movement_*` |
| 分页存在 | 含 `daoyu_ra_mv_page_01/03` 与 `PREVIOUS_PAGE` / `NEXT_PAGE` / `QUIT` |
| 单人指定 | `daoyu_ra_mv_ideology_map` 内无 `every_character` 等批量作用域 |
| 未误改共用分页 | 「利益集团意识形态」菜单仍用基础 Mod 的 `daoyu_check_page_next` |
| 未新增 GUI 覆盖 | 仓库内不存在 `gui/` 目录 |
| 按钮无图标 | 交互文件中不出现 `icon = ` |
| 前缀只加在右键交互上 | 两个交互的按钮名含 `#V 刀鱼作弊#!`；6 个事件选项名**不含** |
| 运动菜单会交回上一层 | `daoyu.292` 含 `set_variable = daoyu_return` 且 `after` 触发 `daoyu.220` |
| 角色名一律裸写 | 三个文件里不存在 `add = { "` / `remove = { "` 这类带引号的角色名 |
| 招募是三个裸名效果 | 存在 `daoyu_ra_recruit_politician/general/admiral_effect`，且无 `$ROLE$` 参与字符串代入 |
| 邀请会临时换意识形态 | `daoyu_ra_save_ideology_effect` 与 `daoyu_ra_restore_ideology_effect` 夹住 `transfer_character`，且**同一个效果块内**完成 |
| 还原不经过任何中间变量 | 不存在 `daoyu_ra_restore_pending` / `daoyu_ra_restore_target` |
| 邀请流程无延迟 | `daoyu_ra_invite_agitator_effect` 内不出现 `trigger_event` 与 `days = 1` |
| 剥夺指挥权不淘汰角色 | 代码里**不出现** `retire_character_if_should_be_culled` / `retire_commander`，只用 `remove_character_role` |
| 四个指挥权交互齐备 | 授予 / 剥夺 × 陆军 / 海军，且右键与菜单两处都有 |
| 意识形态能还原 | 93 项各有一个 `daoyu_ra_saved_ideology_*` 标记；`daoyu.293` 为 `hidden = yes` |
| 退出只有一个 | `daoyu.292` 中 `RESIGN` + `QUIT` 合计恰好 1 个 |
| 翻页用真 trigger | `PREVIOUS_PAGE` / `NEXT_PAGE` 的显隐条件是 `trigger`，不是 `ai_chance` |
| 上一页只在 2/3 页 | `PREVIOUS_PAGE` 恰好出现 2 次 |
| 下一页只在 1/2 页 | `NEXT_PAGE` 恰好出现 2 次 |

此外，每次提交前应对**每个被覆盖的上游文件**按唯一行做一次 `Counter` 差分，
确认「减少的行」全部在上一节的等价替换表中登记过——上一轮的功能丢失正是这样发现的。

### 尚未验证

- **未在游戏内启动验证。** 所有结论均来自脚本、引擎字符串与上游文件比对，未运行游戏。
- 具体待实测项：
  1. 「立即邀请」是否确实绕过 5 年冷却；
  2. `create_political_movement` 在目标国为玩家国自身时是否被引擎额外限制；
  3. `add_modifier = { name = daoyu_country_agitator_slots_add days = 30 }` 是否足以腾出
     鼓动者槽位使 `transfer_character` 成功（该修正是**限期 30 天**的临时槽位）；
  4. 「指定政治运动」对只看身份的运动（如 `movement_german_national`）是否真能让
     目标角色加入（取决于引擎对 `culture_selection_trigger` 的判定）；
  5. 3 页翻页时页 1/2/3 的显示与「上一页 / 下一页 / 退出」可见性；
  6. **「筹划意外（流亡者版）」对流亡角色是否真的执行成功**（`kill_character` 对流亡池
     角色的表现，以及杀掉后流亡者列表的刷新是否正常）。

### 已知风险

- **目标 scope 随机化（法律菜单）**：若同时有多个国家带 `daoyu_target_country` modifier，
  `d_law_tec.720` 的随机刷新可能让法律选项落到另一个国家。
- **`daoyu_target_character` 特质会残留**：基础 Mod 自身如此（只清理部分路径），
  本补丁未改动其语义。但自本版起 `daoyu.220` 已改为**优先精确指向**变量里的角色，
  因此残留特质不会再导致「点了 A 作用到 B」。若变量缺失（例如直接从事件进入菜单），
  才回退到基础 Mod 的随机取角色逻辑。
- **指定运动对只看身份的运动不保证成功**：引擎按 `culture_selection_trigger` /
  `religion_selection_trigger` 以及文化 / 宗教的普遍度挑选身份，脚本只能传入候选身份，
  最终是否成立由引擎决定。菜单里对此类运动有注释标注。
- **「指定政治运动」对非鼓动者会自动转职**。若只想改运动不想改职位，请不要对政治家使用。
- **筹划意外对流亡者不可逆**：它直接 `kill_character`。确认框与红字警告都保留；
  若目标是 `is_immortal = yes` 的剧情人物，引擎会忽略该效果（点了没反应），这是原版行为。

### 未部署

按用户要求，本补丁**只修改了 `E:\Project\Mod\victoria3\daoyu_cheat_ra` 工程**，
未部署到游戏 Mod 目录，因此当前正在生效的 Mod 组合未被影响。

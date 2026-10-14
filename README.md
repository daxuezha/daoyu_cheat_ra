# Daoyu Cheat - Real History Compatibility

**Victoria 3 兼容补丁 Mod**：为 [`Daoyu Cheat — Continued (BtKing)`](https://vic3.paradoxwikis.com/Mod_structure) 的作弊菜单补上 `Realism Ai Historical Flavor Mod`（下称 `real_history`）提供的法律与角色功能。

> 本仓库是**独立兼容补丁**，不包含基础 Mod 本体。原 `diaoyu_cheat` 工程未被修改。

- Mod ID：`daoyu_cheat_ra`
- 支持游戏版本：`1.13.*`
- 标签：`Events`

---

## 这个 Mod 做什么

基础版刀鱼作弊菜单只覆盖原版法律与角色操作。本补丁做两件事：

| # | 功能 | 入口 | 说明 |
|---|---|---|---|
| 1 | **补齐 `real_history` 法律菜单** | 法律菜单 → 各法律组 | 把 `real_history` 中 40 项带 `group` 的自定义法律 + `law_factory_councils` 全部做成可选项，并按法律定义补全国家 / 文化 / 变量 / 互斥条件 |
| 2 | **「成为利益集团领袖」** | 管理角色 → 目标角色操作 | 在「成为统治者」旁新增选项，让目标角色成为其所属利益集团的领袖（`set_as_interest_group_leader`） |

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
│  └─ scripted_effects/
│     └─ daoyu_tec_and_law_effects.txt   # 菜单变量清理效果
├─ events/
│  ├─ daoyu_events.txt                   # 覆盖基础 Mod 同名文件（角色操作菜单）
│  └─ daoyu_tec_and_law_events.txt       # 覆盖基础 Mod 同名文件（法律菜单）
└─ localization/             # 11 种语言的补丁本地化
   ├─ braz_por/  english/  french/  german/  japanese/  korean/
   └─ polish/  russian/  simp_chinese/  spanish/  turkish/
```

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
  - `daoyu.222`：`flavor` 增加对应描述分支；新增一个 `CONFIRM` 选项执行 `scope:daoyu_target_character = { set_as_interest_group_leader = yes }`；`after` 清理新变量。
- `localization/<语言>/daoyu_ra_l_<语言>.yml`（11 种语言）：新增键 `daoyu_set_character_as_ig_leader`，与基础 Mod 的本地化覆盖方式一致。

---

## 变更文件

| 文件 | 变更 |
|---|---|
| `events/daoyu_tec_and_law_events.txt` | 为 40 项 `real_history` 分组法律增加 `activate_law` 菜单项；把每个新增选项移动到与法律定义相同的法律组代码段内（工厂委员会放在 `lawgroup_labour_associations`）；为新增选项添加简体中文行尾注释；为美国金融法律组增加仅对美国目标国显示的入口，并把四项金融法律放在经济体制段之后、贸易政策段之前；对日本、中国、德国 / 意大利、美国等有明确国家或文化范围的法律选项添加目标国家限制 |
| `common/scripted_effects/daoyu_tec_and_law_effects.txt` | 在菜单变量清理效果中清除新增的 `lawgroup_ra_usa_financial_system` |
| `events/daoyu_events.txt` | 新增「成为利益集团领袖」选项、`daoyu.222` 确认分支与变量清理 |
| `localization/*/daoyu_ra_l_*.yml` | 新增 `daoyu_set_character_as_ig_leader` 键（11 种语言） |

---

## 技术说明

### 为什么用整文件覆盖而不是 `INJECT:`

`events/` 目录不支持 `INJECT:` / `TRY_INJECT:` 条目模式（该机制用于 `common/` 下的数据库条目，见 CWTools `definition_injections.cwt`）。本兼容补丁依赖基础 Mod 加载，使用相同相对路径的 `events/daoyu_events.txt`、`events/daoyu_tec_and_law_events.txt` 覆盖基础 Mod 文件，与既有覆盖方式一致。

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

已完成的静态检查：

- 覆盖版 `daoyu_events.txt` 与基础 Mod 版本逐行比对：仅新增 23 行，无其他改动。
- 大括号配平：`16603 / 16603`。
- 编码：`.txt` 为 UTF-8 **无 BOM**、CRLF 换行（与基础 Mod 一致）；本地化文件为 UTF-8 **带** BOM（与原版及基础 Mod 一致）。
- 变量 `daoyu_set_character_as_ig_leader` 在基础 Mod 中无重名。
- `real_history/common/laws` 中有 `group` 的法律定义与菜单项逐项比对：**40 / 40 均已覆盖**，法律组选项映射一致。
- `law_factory_councils` 菜单触发条件与启用效果均使用 `lawgroup_labour_associations`。
- 法律选项 ID 唯一、同法重复启用保护、中文注释、CRLF 换行与两个脚本的大括号平衡均已静态检查通过。
- 法律组选择器的设置 / 清理链可到达对应菜单；新增美国金融组在选择美国为目标国并选择「经济」后可见。

**尚未完成**：游戏内实际效果仍需在安装的 Mod 组合中启动验证。

> 目标 scope 风险提示：若同时有多个国家带 `daoyu_target_country` modifier，`.720` 的随机刷新可能让法律选项落到另一个国家。这是独立于单项法律条件的状态风险。

# `diaoyu_cheat` 法律菜单与 `real_history` 对齐记录

## 法律选项怎样走到生效

本节按 `real_history/common/laws` 的法律定义核对 `d_law_tec` 事件链，说明菜单是否显示、法律定义声明的资格，以及菜单执行时容易漏掉的条件。引用的脚本行号对应本次审计时的文件；以后文件变动后行号可能变化。

### 菜单执行链

1. `d_law_tec.700` 选定/确认目标国，并转入 `d_law_tec.720`。
2. `d_law_tec.720` 选择“权力结构 / 经济 / 人权”顶层，再转入 `d_law_tec.730`。
3. `d_law_tec.730` 的法律组按钮设置全局变量，例如 `lawgroup_labour_associations`。美国金融组使用单独变量 `lawgroup_ra_usa_financial_system`，且只对美国目标国提供入口。
4. `d_law_tec.750` 显示对应法律组的选项；选项的 `trigger` 控制按钮是否可见，选项体内的 `scope:daoyu_target_country = { activate_law = ... }` 则直接切换目标国的已生效法律。
5. 返回/换组时，`daoyu_remove_law_var_effect` 清除各组选择变量；它不启用法律。

法律效果针对的是保存为 `daoyu_target_country` 的国家。`.720` 会从带有 `daoyu_target_country` modifier 的国家中随机刷新这个保存 scope；因此流程隐含假设同时只有一个国家带该 modifier。若有多个标记国家，法律选项可能落到其中另一个国家。这是目标选择状态风险，不是单项法律的 `can_enact` 条件。

因此这里不是“开始立法”。菜单直接调用 `activate_law`，没有可见的立法进度、利益集团/政治运动流程，也没有显式调用原法律定义的 `can_enact`、`is_visible`、`disallowing_laws`。是否由游戏引擎在 `activate_law` 内部再拒绝某些非法切换，单靠这些脚本不能确定。为避免依赖未验证的隐式拦截，修复版会在菜单 `trigger` 显式复现法律定义中能确定的国家/文化资格、特殊变量和明示互斥项。

### 不能混为一谈的条件

- `is_visible`：原版法律界面是否展示法律；它不等同于能否颁布。
- `can_enact`：原版立法流程的颁布资格。作弊菜单不走这个流程，所以直接启用时必须在菜单触发条件中复现，才不会让不符合定义的国家误用。
- `disallowing_laws`：定义中明确声明的互斥法律。菜单也需要显式挡住；否则可能出现原法系禁止、作弊菜单仍提供选项的情况。
- `parent`：定义声明的父法律/变体关系。本次清单记录它，但没有把它一概解释为“必须先有父法”。仅凭这些脚本无法确认引擎对 `parent` 的完整语义，故不会凭猜测在菜单中添加前置法；如父法确为硬性条件，需要游戏内实测或进一步核对引擎规则。
- `unlocking_laws`、`unlocking_technologies` 与 `can_enact` 是不同字段；若某法律定义没有把科技写进 `can_enact`，本次不会擅自把技术条件当作作弊菜单门槛。
- 菜单中额外的国家风味限制不一定来自法律定义。修复时以定义中明确的 `can_enact` / `is_visible` 为资格依据；只有原定义本身明确要求国家/文化时才保留相应门槛。

### 40 项法律的定义条件清单

表中“国家/文化/特殊条件”是法律定义明确写出的资格；“明示互斥”是 `disallowing_laws`。`无`表示在该法律定义中没有相应字段，不代表引擎绝不会受其他系统限制。父法关系单独列出，不在本表中推断为硬性前置条件。

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

定义来源：上表前十项（`law_people_police` 至 `law_renda`）在 `common/laws/dsk_additional_laws.txt`；日本主题法律（`law_IJ_*`）在 `common/laws/kamikaze_laws.txt`；德国/意大利主题法律在 `common/laws/NAZI_laws.txt`；四项金融阶梯在 `common/laws/ra_usa_financial_laws.txt`；`law_usfp_nominal_separation` 在 `common/laws/usfp_law_variants.txt`。

工厂委员会 `law_factory_councils` 不是上述 40 个 `real_history/common/laws` 自定义定义之一；审计到的本地法律菜单把它放在 `lawgroup_labour_associations`。它与 `law_IJ_factory` 是两项不同法律：后者属于 `lawgroup_labor_rights`，且父法是 `law_worker_protections`。不要将两者的组、ID 或前置条件混用。

### 菜单条件的修正规则

依照上表，修复版菜单会：

1. 保留每个法律组选择变量和“目标国尚未拥有本法”的防重复条件。
2. 在法律选项触发器中补齐定义明确的国家、主流文化、变量授权和 `disallowing_laws` 条件。美国金融阶梯分别检查危机记录、前一阶金融法律与新政授权，不再只检查“是美国”。
3. 修正三项意大利法律的文化检测：使用定义中的北意大利/南意大利文化，而不是笼统的 `cu:italian`。
4. 移除只由当前菜单添加、但法律定义并未要求的日本/BIC 限制；保留定义明确的日本/德国/意大利/中美等门槛。日本八项明确要求复辟变量的法律仍检查该变量；日本议会法 `law_IJ_assembly` 仍按定义要求日本。
5. 不把 `parent` 自动当成硬性 `has_law` 前置项；`law_yidaiyilu` 等定义中的真实特殊解锁变量则会检查。若日后要把 `parent` 变成严格前置条件，应先在游戏内验证其变体语义。

修复后的菜单仍是作弊式立即切法，不等同于正常立法；表内“条件满足”指菜单是否提供选项以及脚本是否允许调用效果，不代表完整模拟立法政治过程。

## 结论与修正

“工厂委员会”的法律 ID 是 `law_factory_councils`，法律组是 `lawgroup_labour_associations`（劳工组织），不是经济体制。它已补入法律菜单的劳工组织选项中。

本次将 `real_history/common/laws` 里有明确 `group = lawgroup_...`、但原 `diaoyu_cheat` 法律菜单没有对应选项的法律补入对应分组。核对结果为 **40 个分组法律全部有对应菜单项**，另加本体法律 `law_factory_councils`。

## 变更文件

- `events/daoyu_tec_and_law_events.txt`
  - 为 40 个 `real_history` 分组法律增加 `activate_law` 菜单项。
  - 将每个新增法律选项移动到与法律定义相同的法律组代码段内；工厂委员会放在 `lawgroup_labour_associations`。
  - 为新增法律选项添加对应的简体中文行尾注释，并为美国金融法律组选项和变量操作添加中文说明。
  - 为美国金融法律组增加仅对美国目标国显示的菜单入口，并把四项金融法律放在经济体制选项段之后、贸易政策段之前。
  - 对日本、中国、德国/意大利、美国等有明确国家或文化范围的法律选项添加目标国家限制。
- `common/scripted_effects/daoyu_tec_and_law_effects.txt`
  - 在菜单变量清理效果中清除新增的 `lawgroup_ra_usa_financial_system`。

## 新增菜单法律

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

`law_stance` 和 `law_enactment_stance` 在相关法律脚本中是立法倾向判断块，不是带有 `group = lawgroup_...` 的独立法律定义，因此没有做成可启用的法律选项。对原版法律的 `TRY_INJECT` 修改也没有新增法律 ID，无需重复创建菜单项。

## 文件放置与使用

本目录现已整理为独立的 **Daoyu Cheat - Real History Compatibility** 兼容补丁 Mod。它通过相同相对路径提供法律菜单事件与变量清理脚本，Mod 元数据在 `.metadata/metadata.json` 中声明：

- `Daoyu Cheat — Continued (BtKing)`（metadata ID：`btking-daoyu`）
- `Realism Ai Historical Flavor Mod`（metadata ID：`realism_ai_historical_flavor_mod`）

Mod 元数据位于 `.metadata/metadata.json`，依赖通过 `relationships` 中的 `rel_type = dependency` 项声明，并使用依赖 Mod 各自 metadata 中的稳定 ID，而不是名称或磁盘路径。缩略图放在 `.metadata/thumbnail.png`。两项依赖不可缺少：本补丁复用 Daoyu Cheat 的事件/脚本结构，并调用 `real_history` 提供的法律类型。应通过 Victoria 3 启动器将此目录作为本地 Mod 添加/安装，并启用这两个依赖；启动器应按依赖关系先加载它们，再加载本补丁。不要将本补丁内的文件覆盖回原 `diaoyu_cheat` 工程。

原 `E:\Project\Mod\victoria3\diaoyu_cheat` 工程未修改。本补丁使用与 Daoyu Cheat 相同的事件命名空间及事件 ID 来覆盖法律菜单逻辑，因此需确保补丁在 Daoyu Cheat 之后加载。

### Mod 元数据修正记录（2026-10-03）

- 初版错误使用根目录 `descriptor.mod`，并以名称写入 `dependencies`；这不符合本次核对的 Victoria 3 Modding Wiki 本地 Mod 元数据格式。
- 按 Wiki 的 `.metadata/metadata.json` 示例改为 JSON `relationships` 依赖对象；两个依赖 ID 分别从其 `.metadata/metadata.json` 读取。
- 版本字段使用 `supported_game_version`，补丁自身 ID 固定为 `diaoyu_cheat_real_history_compatibility`，缩略图放在 `.metadata/thumbnail.png`。
- 格式依据：[Modding Wiki：Setting up the mod folder](https://vic3.paradoxwikis.com/Modding#Setting_up_the_mod_folder)、[Mod structure：Metadata](https://vic3.paradoxwikis.com/Mod_structure#Metadata)。Wiki 示例使用 `.metadata/metadata.json` 与 `relationships`；依赖关系以目标 Mod metadata ID 标识。

## 核对结果

- 将 `real_history/common/laws` 中有 `group` 的法律定义与菜单项逐项比对：40/40 均已覆盖，法律组选项映射一致。
- `law_factory_councils` 菜单触发条件和启用效果均使用 `lawgroup_labour_associations`。
- 新增法律菜单代码按所属法律组归位；新增 `name` 行使用中文注释，便于在脚本中识别。
- 根据本节列出的资格与互斥项，已补齐菜单侧可确定的条件；原菜单额外添加但法律定义没有要求的国家限制已移除。
- 40 项法律组对应、法律选项 ID 唯一、同法重复启用保护、中文注释、CRLF 换行和两个脚本的大括号平衡均已静态检查通过。
- 法律组选择器的设置/清理链可到达对应菜单；新增美国金融组在选择美国为目标国并选择“经济”后可见。若同时有多个目标 modifier，目标 scope 的随机刷新仍是独立风险。
- 游戏内实际启动验证仍需在安装的 Mod 组合中进行。

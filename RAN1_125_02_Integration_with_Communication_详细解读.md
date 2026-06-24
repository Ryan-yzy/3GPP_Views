# RAN1#125 10.8.2 Integration with Communication 详细解读

资料范围：本文基于 `RAN1_125_ISAC_reports_20260616/_extracted/02_Integration_with_Communication_summaries.txt`、RAN1#125 ISAC `02_Integration_with_Communication/Moderator_Summaries/` 中的 Summary #1-#5，以及 `Technical_Contributions/` 中 50 份 DOCX 和 1 份旧版 DOC 技术提案。本文将 `R1-2605133` Moderator summary #5 视为本次会议结束时的主要状态；`R1-2603994` 至 `R1-2603997` 用于追踪提案、反对意见和措辞演进。旧版 `R1-2604777` 为 `.doc`，已通过可读正文扫描抽取关键段落，主要涉及资源复用 Proposal 4.3-1 的支持意见。

## 引用与强度说明

本文只把 summary #5 中明确写成 **Agreement** 的内容视为会议共识；**Moderator Proposal**、**High-priority Proposal**、**Medium-priority Proposal**、**Closed proposal**、**FFS**、**encourage companies**、**up to company report**、**not precluded** 和 **candidate solution** 均保留原始强度。特别注意：`Proposal 4.1-1-rev4` 只是 Medium Proposal，不是 Agreement；`Proposal 4.2-1` 是 Closed/encourage，不是 dedicated sensing RS 已被采用；`Proposal 5.2/5.3/5.4` 是 measurement level 的 further study，不是最终 reporting format。

首次出现的主要缩写如下：集成感知与通信（Integrated Sensing and Communication, ISAC）；无线接入网第一工作组（Radio Access Network Working Group 1, RAN1）；技术文稿（Technical Document, TDoc）；参考信号（Reference Signal, RS）；资源元素（Resource Element, RE）；传输接收点（Transmission Reception Point, TRP）；用户设备（User Equipment, UE）；6G 接入节点（access NodeB, aNB）；感知功能（Sensing Function, SF）；感知发射端（Sensing Transmitter, STx）；感知接收端（Sensing Receiver, SRx）；相干处理间隔（Coherent Processing Interval, CPI）；子载波间隔（Subcarrier Spacing, SCS）；循环前缀（Cyclic Prefix, CP）；信道状态信息参考信号（Channel State Information Reference Signal, CSI-RS）；探测参考信号（Sounding Reference Signal, SRS）；跟踪参考信号（Tracking Reference Signal, TRS）；解调参考信号（Demodulation Reference Signal, DMRS）；相位跟踪参考信号（Phase Tracking Reference Signal, PTRS）；同步信号块（Synchronization Signal Block, SSB）；物理随机接入信道（Physical Random Access Channel, PRACH）；定位参考信号（Positioning Reference Signal, PRS）；物理下行共享信道（Physical Downlink Shared Channel, PDSCH）；物理上行共享信道（Physical Uplink Shared Channel, PUSCH）；带宽部分（Bandwidth Part, BWP）；传输配置指示（Transmission Configuration Indication, TCI）；准共址（Quasi Co-Location, QCL）；时分/频分/码分/空分/功率域复用（TDM/FDM/CDM/SDM/PDM）；感知辅助通信（Sensing-Assisted Communication, SAC）；雷达散射截面（Radar Cross Section, RCS）；参考信号接收功率（Reference Signal Received Power, RSRP）；信干噪比（Signal to Interference plus Noise Ratio, SINR）；目标路径接收功率（RSRPP, ZTE 公司术语）；到达角（Angle of Arrival, AoA）；离开角（Angle of Departure, AoD）；待进一步研究（For Further Study, FFS）；链路级仿真（Link-Level Simulation, LLS）；系统级仿真（System-Level Simulation, SLS）。

## 1. 会议与议题背景

### 技术含义

RAN1#125 的 10.8.2 “Integration with Communication” 不只是讨论“通信信号能不能顺手拿来感知”。它实际覆盖了 6G ISAC 空口如何把感知嵌入通信系统的主要耦合面：sensing RS 的生成与增强、RE pattern 评估、measurement/report level、UE/aNB/SF 的报告路径、SAC 用例、resource allocation/multiplexing、CPI phase continuity、beam related aspects、interference handling、power control/allocation、cooperative sensing 定义，以及 sensing Tx/Rx selection 和 start/end/activation/deactivation 等过程问题。

会议资料显示，RAN1#124bis 已经达成若干 study-level Agreement：RS for communication reused for sensing、enhanced RS taking communication RS as starting point、dedicated RS for sensing 都可以研究；RE pattern、measurement/report、resource multiplexing、phase continuity、beam management 和 power control 等也都进入 study 范围。RAN1#125 的核心工作是把这些宽泛入口收敛成下一次会议可执行的提案和仿真任务，而不是直接决定 Rel-20 规范方案。

### 标准化含义

从 TSG RAN 的 SID 看，RAN1 负责 PHY functions and procedures、sensing performance evaluation 以及 integration with communication services；RAN2 负责 higher layer procedures and protocol aspects；RAN4 负责 RF、coexistence、testability 等工程可行性问题。Huawei 在 `R1-2604278` 的背景表也把这个边界写得很清楚：waveform、RS、measurement feedback 属于 RAN1/RAN4，higher-layer procedure 属于 RAN2，RF/coexistence/testability 由 RAN4 协调。因而本议题的 Agreement 多采用 “study necessity, scenarios and related physical layer specification impacts” 的措辞，避免越界决定 RAN2/RAN4 架构。

### 尚未解决的问题

尚未解决的核心不是某个单独参数，而是“感知和通信到底在哪一层闭环”。例如 UE measurement 是直接到 aNB 还是到 SF，RAN1 只适合讨论测量量、资源、延迟和 PHY 影响，报告端点和传输协议需要跨 WG 协调；TRP/UE monostatic 的隔离、自干扰、切换时间和 RF 残差也不能由 RAN1 summary 直接变成硬件要求。

## 2. 一页式会议结论

### 技术含义

| 技术方向 | 当前状态 | 形成的共识 | 主要分歧 | 下一步 |
|---|---|---|---|---|
| Sensing RS | 以通信 RS pattern/reuse 为起点，dedicated RS 并行收集方案 | `Proposal 4.3-1-rev2` Agreement：继续研究 RE pattern evaluation 的 SLS/LLS/简化 SLS 选项 | `Proposal 4.1-1-rev4` 仍是 Medium Proposal；dedicated RS 是 Closed/encourage，不是采用 | RAN1#126 公司提交 RE pattern、benchmark、性能和开销 |
| Measurement/report | Level A/B/C/D 复用 5G-A ISAC 作为讨论语言 | `Proposal 5.1-1-rev1` Agreement：TRP-side Level A from NR ISAC 不考虑用于 6G ISAC | UE-side Level A 只是不优先；Level B/C/D 仍是 Medium Proposal | 明确 B2/B3、C path metrics、D scattering point metrics 和报告开销 |
| SAC | 已观察到 beam management、CSI、mobility、resource allocation、channel estimation、power saving、power control 等用例 | `Proposal 6.2-1-rev3` Agreement：Rel-20 至少识别一个代表性 SAC 用例，并研究是否有超出 object detection/tracking 的 PHY 设计影响 | 用例尚未下选；ideal/realistic assistance 和端到端通信指标未统一 | 公司给出完整流程、测量类型、准确度/延迟需求和量化增益 |
| Resource multiplexing | TDM/FDM/CDM/SDM/PDM、collision/signal sharing、sensing RS resource allocation 都进入 study | `Proposal 6.3-1-rev1` Agreement 覆盖复用和碰撞；`Proposal 6.3-1-rev4` Agreement 覆盖 sensing RS resource allocation | “using data in communication channel for sensing” 从最终 Agreement 中移出，只能视为 FFS/公司候选 | 明确 BWP、频域资源、gap/window、non-serving TRP/cell 和碰撞规则 |
| CPI phase continuity | 被主持人称为 critical issue | `Proposal 6.4-1-rev1` Agreement：研究 CPI 定义、破坏因素、保持/恢复、measurement/report 影响 | TA/power change 是否需特殊处理有分歧；TDD DL/UL switch 和 SRS hopping 更受关注 | RAN1#126 提供 tolerable phase discontinuity、measurement 和 spec impact |
| Beam management | 通信 beam management 是 starting point | `Proposal 6.5-1-rev5` Agreement：研究 RS used for sensing 的 beam related aspects | sensing beam 是否需要独立于通信 beam，Huawei 与 ZTE/LG 等观点不同 | 明确 TCI/QCL、beam sweep、beam report、beam indication、beam failure |
| Interference | 覆盖 sensing 内部、sensing 与 communication 之间 | `Proposal 6.6-1-rev4` Agreement：以通信干扰处理为起点研究 sensing interference handling | 是否需要新增 interference measurement/report、muting、资源协调仍有分歧 | 与 sensing RS、measurement/report、resource allocation 联动研究 |
| Power control | UE power control 和 TRP power allocation 都进入 study | `Proposal 6.7-1-rev5` Agreement：研究 RS used for sensing 的 UE PC/TRP power allocation | P0/alpha、pathloss reference、TPC、PHR、power ramping 只是候选，不是结论 | 从 target path、多 SRx、CPI constant power 和通信影响出发评估 |
| Cooperative sensing | 定义和多 STx/SRx 组合进入讨论 | `Proposal 6.1-1-rev1` Agreement：研究所有 6GR frequency ranges、各个 sensing mode 和多 mode 组合 | `Proposal 6.1-2-rev1` cooperative sensing 定义仍是 Medium Proposal | 明确多节点选择、同步、融合、报告和仿真复杂度 |
| Tx/Rx selection 与 activation | 被识别为过程问题 | `Proposal 6.1-3` 是 Medium Proposal/encourage companies，不是 Agreement | 是否属于 RAN1、RAN2 或架构问题仍不清晰 | 公司提交 sensing Tx/Rx selection、start/end、activation/deactivation 的 RAN1 视角输入 |

### 标准化含义

本次真正的“硬共识”集中在 study scope 和 study items，而不是具体空口机制。RAN1 同意研究一系列 PHY 影响，但没有采用某一种 RS、某一种测量报告格式、某一种 SAC 用例、某一种功率控制公式或某一种 beam procedure。换句话说，Summary #5 的主调是“把问题拆出来并要求下一次给证据”，不是“把方案写进规范”。

### 尚未解决的问题

RAN1#126 需要解决的关键问题包括：RS pattern 如何用可比方式评估；通信 RS enhancement 与 dedicated sensing RS 的性能/开销拐点在哪里；Level B/C/D 的报告开销能否接受；SAC 的代表性用例和端到端指标是什么；CPI phase continuity 的容忍度如何量化；beam/interference/power control 到底需要哪些新增 PHY 信令。

## 3. Summary #1-#5 的演进

### 技术含义

| 阶段 | TDoc | 演进重点 | 典型变化 |
|---|---|---|---|
| Summary #1 | `R1-2603994` | 汇总公司初始观点，把 RAN1#124bis 的宽泛 study items 展开 | RS reuse/enhancement/dedicated、measurement/report、resource multiplexing、phase continuity 等还较粗 |
| Summary #2 | `R1-2603995` | 开始围绕 measurement level、UE/aNB/SF report path、RE pattern 和 SAC 用例形成候选 proposal | Level B/C/D 的 down-selection 逻辑出现，SAC 从“是否讨论”转向“哪些通信过程受益” |
| Summary #3 | `R1-2603996` | 多个 proposal 加 rev，开始删除过宽或跨 WG 的措辞 | 数据通道用于 sensing、报告端点、QCL/TCI 等更谨慎地转为 FFS/not precluded |
| Summary #4 | `R1-2603997` | resource、phase、beam、interference、power control 进入更成熟措辞 | “study necessity, scenarios and related physical layer specification impacts” 逐步成为统一格式 |
| Summary #5 | `R1-2605133` | 会议结束状态，形成主要 Agreement | Agreement 覆盖 RE pattern evaluation、TRP Level A、SAC representative use case、resource/phase/beam/interference/power 等 |

提案强度的演进很有代表性：早期的 `Proposal 6.3-1-rev1` 仍列出 “FFS using data in communication channel for sensing”，但最终 Agreement 只保留 TDM/FDM/CDM/SDM/PDM 与 collision/signal sharing；早期 `Proposal 6.7-1` 列出 P0/alpha、pathloss reference、CL PC、PHR、DL PC，最终 `Proposal 6.7-1-rev5` 的 Agreement 只说研究 UE PC/TRP power allocation for RS used for sensing，候选机制不再作为共识列举。

### 标准化含义

这种演进说明主持人有意把“公司希望研究的具体机制”降格为候选，把“RAN1 确实需要处理的 PHY 问题”保留为 Agreement。它降低了误读风险：例如 dedicated sensing RS 的存在感很强，但 `Proposal 4.2-1` 被标记为 Closed，并且只是鼓励公司在 RAN1#126 提供设计和性能评估，同时注明 only if communication-RS-based sensing RS with enhancements fails to meet objectives。

### 尚未解决的问题

仍待解决的是 summary 里的 “Medium Proposal” 如何在下一次升级或关闭。特别是 `Proposal 4.1-1-rev4`、`Proposal 5.2-1-rev1`、`Proposal 5.3-1-rev2`、`Proposal 5.4-1-rev1`、`Proposal 5.7-1-rev3`、`Proposal 6.1-2-rev1`、`Proposal 6.1-3` 都需要更多公司证据，否则它们只是讨论结构，不是可落地规范方向。

## 4. Integration with Communication 的总体范围

### 技术含义

本议题的“integration”可以分为三层。第一层是信号/资源层：通信 RS、控制/数据信道和 BWP/slot/RE grid 是否能承载 sensing，sensing RS 是否能从 CSI-RS/SRS/TRS/DMRS/PTRS/SSB 等 pattern 生成。第二层是过程/测量层：UE 或 TRP 作为 SRx 时要测量什么、向 aNB/SF/UE 报什么、是否需要 activation/deactivation、measurement gap、report payload reduction。第三层是通信受益层：sensing output 是否能辅助 beam management、CSI acquisition/reporting、mobility/handover、resource allocation/link adaptation、channel estimation、power saving 和 power control。

`Proposal 6.1-1-rev1` 的 Agreement 把范围放得很宽：研究所有 6GR frequency ranges、每个 individual sensing mode，以及 multiple sensing modes 的组合（`R1-2605133`, summary #5, Proposal 6.1-1-rev1, Agreement）。但 `Proposal 6.1-2-rev1` 中 cooperative sensing 的定义仍是 Medium Proposal：用多个 STx 和/或多个 SRx、一个或多个 sensing mode 感知一个或多个目标（`R1-2605133`, summary #5, Proposal 6.1-2-rev1, Medium Proposal）。Tx/Rx selection、start/end、activation/deactivation 也只是 `Proposal 6.1-3` 的 company input 方向，不是 Agreement。

### 标准化含义

RAN1 当前只适合确认“哪些 PHY 方面可能需要 study”，而不是决定 SF 架构、服务暴露、UE 隐私、RRC/MAC 承载或 RF 实现。UE->aNB 的低延迟路径对 SAC 有吸引力，UE->SF 的路径对集中式 sensing/fusion 有吸引力，但 transport endpoint 属于 RAN2/SA2/RAN3 讨论范围；RAN1 应先给出 measurement level、payload、latency 和 PHY resource 的约束。

### 尚未解决的问题

范围最大的空白是 “SAC 是否需要一套独立于 detection/tracking 的 measurement/report 和 resource lifecycle”。如果 sensing 结果只是服务于 object detection/tracking，那么 Level C/D 可能足够；如果要实时辅助 beam/CSI/mobility，则 latency、freshness、confidence、path classification、static/dynamic path separation 和 channel prediction error 都可能成为新的 PHY 需求。

## 5. Sensing reference signal 设计

### 技术含义

`Proposal 4.1-1-rev4` 是本议题最重要但也最容易被误读的 RS 文本。它是 Medium Proposal，提出可以考虑以下方式生成 sensing RS 的 RE pattern：同类型一个或多个 communication RS 组合；多类型 communication RS 组合；CSI-RS 新增 comb factor 1/2/3/6；OFDM symbol 非均匀而每个 symbol 内 RE 均匀；symbol 内非均匀 subcarrier pattern；跨 symbol 的 time-staggered subcarrier mapping；其他方案和组合不排除；QCL/TCI-State 的 signaling/indication 仍 FFS；communication RS component may or may not be allocated to UE, up to aNB/SF（`R1-2605133`, summary #5, Proposal 4.1-1-rev4, Medium Proposal）。

通信 RS 复用的技术逻辑是减少额外开销、保持空口统一、避免引入难部署的专用信号。Qualcomm 在 `R1-2604711` 的 Observation 1/2 明确说，reuse RS for communication 能以最小开销实现 ubiquitous sensing，历史上 positioning-specific RS 部署不足说明 overhead/cost 是主要障碍；但 Qualcomm 也列出 dedicated RS 的触发条件，并提出 OFDM-compatible FMCW-like chirp/ZC sequence 和 pulsed-OFDM 作为 candidate solution（`R1-2604711`, Proposals 4/5, company proposal）。Huawei 在 `R1-2604278` 把 sensing resource 分成 Type1（sensing performance oriented, mainly UL/DL RS）和 Type2（shared with communication, RS and physical channels），并建议研究 SRS/CSI-RS 以及 control/data channel for sensing（`R1-2604278`, Proposals 3-12, company proposal）。

| RS/信道 | 用于 sensing 的潜在价值 | 主要限制 | 当前强度 |
|---|---|---|---|
| CSI-RS | DL angle/delay/power、beamformed transmission、multi-port AoD | comb/density/periodicity 未必满足 Doppler/tracking；QCL/TCI 需澄清 | `Proposal 4.1-1-rev4` candidate |
| SRS | UL/UE-centric sensing，UE as STx，自然关联 UL PC/TA | UE power、frequency hopping、TA/phase continuity、multi-SRx pathloss | `Proposal 4.1-1-rev4` candidate；Huawei/Qualcomm/ZTE company proposals |
| TRS | 时间/频率跟踪，可能辅助 Doppler/phase | 设计目标不是 target reflection，周期和密度受限 | `Proposal 4.1-1-rev4` candidate；ZTE 指出 CSI-RS/TRS 密度不足 |
| DMRS | 与 PDSCH/PUSCH 绑定，数据传输时可用 | UE-specific、调度依赖、解调/数据知识、干扰和隐私 | `Proposal 4.1-1-rev4` candidate |
| PTRS | phase noise/phase tracking，可能辅助 CPI coherence | 稀疏且非 sensing oriented | `Proposal 4.1-1-rev4` candidate |
| SSB | 广覆盖、初始接入/beam sweeping 可复用 | 周期粗、带宽窄、测距/测速能力有限 | final RS list candidate；AT&T 提到 SSB for initial detection |
| PRACH/PRS | PRACH 具 UL timing/range-like 特性，PRS 与 positioning measurement 接近 | 未进入 `Proposal 4.1-1-rev4` 的最终 RS list；需要跨 positioning/RA 机制协调 | company/analysis level，非 Agreement |
| PDSCH/PUSCH | 数据 RE 占用大，理论上 sensing aperture 更大 | final Agreement 未保留 “using data channel for sensing”；需要 rate matching/muting/SIC | Huawei Type2、ITRI/Acer 资源复用支持；FFS/candidate |

非均匀 RE pattern 是本议题的技术热点。SJTU/NERC-DTV 在 `R1-2604495` 指出，均匀稀疏 pilot 会在 range-Doppler map 中产生规则 ambiguity peaks，最大无模糊距离和速度被 pilot spacing 同时约束；uniform alternative scattered RS 和 multi-CPI physical velocity verification 可以在两个 CPI 内缓解多目标 velocity ambiguity 和 ghost targets（`R1-2604495`, Observations 1-3, Proposals 1-3, company proposal）。ZTE 在 `R1-2604381` 对 non-uniform RS 给出指标视角：unambiguous Doppler range 对非均匀采样由 sampling interval 的 greatest common divisor 决定，peak sidelobe ratio 会由 RE pattern 决定，高 sidelobe 可能造成 Doppler false alarm（`R1-2604381`, Observation 5/Proposal 11, company proposal）。LG 在 `R1-2604761` 则强调 non-uniform frequency RE pattern 可缓解 range ambiguity（`R1-2604761`, Observation 3/Proposal 7, company proposal）。

```
通信 RS 起点              sensing enhancement 候选
CSI-RS/SRS/TRS/...  ->  comb factor / symbol pattern / subcarrier pattern
                    ->  multi-RS combination / multi-port / QCL-TCI relation
                    ->  benchmark RE pattern: every M subcarriers, every N OFDM symbols
                    ->  dedicated RS only if enhanced communication-RS route fails KPI
```

### 标准化含义

RAN1#125 没有下选 sensing RS。唯一比较确定的是：RE pattern 的评估方法要继续研究，且除了 theoretical RE pattern characteristics analysis，还可以进一步研究 full SLS、non-cooperative SLS、single STX/SRX SLS、或由公司自选 LLS/简化 SLS 的方案（`R1-2605133`, summary #5, Proposal 4.3-1-rev2, Agreement）。`Proposal 4.4-1-rev4` 鼓励公司在 RAN1#126 提供 RE patterns，包括 benchmark RE pattern，每 M 个 subcarrier 和每 N 个 OFDM symbol 映射，FFS M/N，并报告 overhead 和 achieved sensing performance，但这不是最终 Agreement。

### 尚未解决的问题

未解决问题包括：communication RS component 是否分配给 UE 时的 receiver assumption；QCL/TCI-State 如何指示；multi-port sensing RS 的 AoD/AoA 能力如何定义；non-uniform pattern 的 ambiguity/sidelobe/overhead 如何统一比较；dedicated RS 的 KPI 触发条件由谁定义；数据 channel sensing 是否需要 PDSCH rate matching、PUSCH muting 或 receiver data knowledge。

## 6. CPI 与 phase continuity

### 技术含义

CPI 是 Doppler 和 coherent integration 的根。Summary #5 的主持人说明非常直接：without phase continuity, Doppler cannot be estimated, which breaks the basis for ISAC operation。最终 `Proposal 6.4-1-rev1` Agreement 要研究 CPI definition、CPI 内影响 phase continuity 的因素、是否/如何保持和恢复 phase continuity、对 measurement/reporting 的影响，并鼓励 RAN1#126 提供 tolerable phase discontinuity、measurement 和 spec impact（`R1-2605133`, summary #5, Proposal 6.4-1-rev1, Agreement）。

公司列出的破坏因素包括：TA/TAC/ATA、transmitting power、TCI state、beam switching、TDD UL/DL switch、Tx/Rx chain reconfiguration、SRS frequency hopping、oscillator drift、RF impairments、scheduling gap、BWP switching。候选处理包括 drop interrupted measurements、把影响参数更新推迟到下一 CPI、recover phase continuity、reference path、LOS ray、sensing reference object/path、RS-based compensation、frequency-domain overlapping resources、UE/TRP sensing phase continuity capability，以及 Cohere 的 Zak-OTFS 方向（`R1-2605133`, summary #5, phase continuity company views）。

Huawei 在 `R1-2604278` 给了较具体的 company report：以约 40 ms CPI 和 DDDSU TDD operation 为例，单次 random phase joggling 对 Doppler detection 可能可容忍，但每次 DL/UL switch 都引入随机相位扰动时，Doppler spectrum 会显著退化并导致 detection failed；在 PA 线性区，后半 CPI 发射功率高 6 dB 且保持 phase continuity 时，Doppler spectrum 形状只轻微变化；Huawei 因而认为 transmission power adjustment 和 TA adjustment 是否需要特殊处理应先证明必要性，而 DL/UL switch 和 SRS frequency hopping 更值得研究（`R1-2604278`, Observations 1-2, Proposals 14-16, company report）。

```
CPI 内理想情况:
RS1 ---- RS2 ---- RS3 ---- RS4      相位随 Doppler 平滑旋转

破坏事件:
RS1 ---- RS2 --[DL/UL switch/beam/TA/hop]-- RS3
                  unknown phase step -> Doppler spreading / sidelobe / false peak
```

### 标准化含义

标准化上，phase continuity 不等于强制所有通信过程冻结。当前 Agreement 只是要求研究定义、破坏因素、保持/恢复和报告影响；具体是否禁止 TA update、如何处理 power control、是否给 sensing RS frequency-domain overlap、是否定义 continuity capability 都未定。它还与 power control、beam switching、BWP、TDD pattern、SRS hopping 和 measurement report 紧密耦合。

### 尚未解决的问题

需要量化的核心是 tolerable phase discontinuity：多大相位跳变、多少次事件、什么 CPI 长度、什么 SCS/频段/速度下会把 detection probability、false alarm、Doppler accuracy 或 tracking continuity 拉出目标。还需要区分 implementation 可补偿的相位事件和必须由规范避免或指示的相位事件。

## 7. Resource allocation 与 multiplexing

### 技术含义

资源复用有两个层面：sensing 内部多个 STx/SRx/RS 之间的复用，以及 sensing 与 communication 之间的复用。`Proposal 6.3-1-rev1` 的 Agreement 说，resource multiplexing within sensing and between sensing and communication 至少考虑 TDM/FDM/CDM/SDM/PDM、collision handling or signal sharing，其他方面不排除（`R1-2605133`, summary #5, Proposal 6.3-1-rev1, Agreement）。`Proposal 6.3-1-rev4` 的 Agreement 进一步把 sensing RS resource allocation 收敛为研究 necessity/scenarios/PHY impacts，并考虑 sensing resource 的 frequency resource allocation may or may not be different from communication（`R1-2605133`, summary #5, Proposal 6.3-1-rev4, Agreement）。

各复用维度的技术含义不同。TDM 最干净，但可能拉长 CPI 或牺牲通信时延；FDM 能并行但影响带宽、range resolution 和 power spectral density；CDM 依赖序列互相关和近远效应；SDM 依赖 beam separation，但 sensing target area (TSA) 与通信 UE 方向可能不同；PDM 允许同 RE 叠加但需要功率分配、接收机动态范围和干扰建模。ITRI/Acer 的 `R1-2604777` 支持继续研究 RAN1#124bis 的 `Proposal 4.3-1`，尤其包括 TDM/FDM/CDM/SDM or using data channel for sensing、BWP/measurement gap impacts、PDSCH rate matching around sensing RS resources、PUSCH muting around sensing RS resources 和 collision handling；但这些细项在 summary #5 最终 Agreement 中并未全部保留，应视为公司支持/候选。

```
TDM: | comm | sensing | comm | sensing |
FDM: | comm subband | sensing subband | comm subband |
SDM: beam to UE and beam to TSA can differ
PDM: same time/frequency/code/beam, different power layers
Collision: prioritize / mute / rate match / share signal / reschedule
```

### 标准化含义

资源复用是最可能影响规范信令的部分之一，因为它连接 BWP、resource set、measurement gap/window、non-serving TRP/cell、collision priority、rate matching/muting 和 sensing/communication shared signal。Summary #5 的措辞有意保守：final Agreement 不说必须支持哪一种复用，也没有采用 data-channel sensing，只要求研究 PHY impacts。

### 尚未解决的问题

尚未解决的是可比评估口径。若公司 A 用 TDM sensing-only symbol、公司 B 用 communication RS reuse、公司 C 用 data-channel sensing，overhead、latency、Doppler resolution 和 communication throughput loss 都不在同一尺度。RAN1#126 需要把 “sensing resource overhead” 和 “communication impact” 的报告格式统一起来。

## 8. Collision 与 interference

### 技术含义

Collision 更像调度/资源冲突，interference 更像接收端性能退化。Summary #5 的 final Agreement 是：研究 sensing 的 interference handling 的 necessity、scenarios 和 related PHY specification impacts，用于缓解 sensing 内部以及 sensing 与 communication 之间的干扰，并以 communication interference handling 为 starting point（`R1-2605133`, summary #5, Proposal 6.6-1-rev4, Agreement）。

公司讨论的干扰类型包括 sensing 与 communication 之间、不同 sensing signals 之间、低空 use case 的跨小区 LOS 干扰、BS-BS、UE-UE、monostatic self-interference，以及 sensing RS 与 URLLC/SSB 等高优先级通信资源冲突。CMCC 提出为避免 3 dB noise floor increase，5 km 内通信可能需要 muted；这只是 company-specific view，不是 Agreement。候选解决方案包括 coordinated resource allocation、time/frequency/code/spatial/power domain mitigation、interference measurement/report、interference measurement resource/quantity/report，以及与 sensing RS 和 measurement/report 的联合设计。

### 标准化含义

RAN1 当前没有决定新建 sensing interference report，也没有决定 muting 规则。标准化上最稳妥的路径是先复用 communication interference handling 的概念，然后识别哪些 sensing 场景会突破通信假设：target echo 很弱、monostatic leakage 很强、TRP-to-TRP/UE-to-UE LOS 干扰更直接、sensing CPI 需要跨时域相干，导致一次短干扰事件也可能污染整个 CPI。

### 尚未解决的问题

需要回答三个问题：哪些干扰可以通过现有通信调度解决，哪些需要 sensing-specific measurement/report；sensing false alarm 与 interference 的统计关系如何建模；monostatic self-interference 是 RAN1 evaluation noise model、RAN4 RF requirement，还是二者都需要。

## 9. Beam management

### 技术含义

`Proposal 6.5-1-rev5` 的 Agreement 是：研究 beam related aspects for RS used for sensing 的 necessity、scenario 和 related PHY specification impacts，并以 communication beam management 为 starting point（`R1-2605133`, summary #5, Proposal 6.5-1-rev5, Agreement）。这句话的重心在 “starting point”，不是“完全复用”。

sensing beam 与 communication beam 的目标函数可能不同。通信 beam 通常最大化 UE link quality，例如 RSRP/SINR；sensing beam 需要照亮或接收 target path，target 可以在 UE 以外区域，且 target echo 弱、路径多、AoA/AoD 可来自反射点。LG 在 `R1-2604761` 指出，如果 target sensing area 不同于 communication area，TRP-UE/UE-TRP bistatic sensing 的 beam direction 可以不同，sensing beam candidates 需要覆盖 TSA 的 delay/Doppler/angle range；ZTE 在 `R1-2604381` 提出可研究对多个 sensing targets 配置 3D angles of beam directions mapped with one sensing RS。Huawei 则在 `R1-2604278` 提出 communication-based beam management and power control mechanisms are applicable to sensing, no specific handling needed，这是更保守的 company view。

### 标准化含义

可能进入规范讨论的点包括 TCI state/QCL type、beam measurement resource/report、beam sweeping、beam indication、beam maintenance/tracking、beam failure recovery，以及 sensing measurement report 是否承载 beam-related quantity。当前还没有决定新增 sensing beam procedure，也没有决定将 target angle 直接作为 beam indication。

### 尚未解决的问题

最难的是通信和 sensing beam 冲突时如何取舍。如果同一 TRP 需要同时给 UE 通信和照亮 TSA，SDM 可能导致功率分裂、旁瓣干扰和不同 beam pair；TDM 又影响 CPI。还需要明确 beam report 的 latency/freshness，否则 SAC beam management 很难闭环。

## 10. Power control 与 power allocation

### 技术含义

最终 Agreement 是研究 UE power control/TRP power allocation for RS used for sensing 的 necessity、scenarios 和 related PHY impacts，同时考虑 sensing performance 和 communication impacts（`R1-2605133`, summary #5, Proposal 6.7-1-rev5, Agreement）。早期列出的 P0/alpha、pathloss reference、TPC command、power ramping、PHR、DL power allocation 等只是候选机制，不是已采用方案。

power control 与通信不同的关键在于 sensing performance 由 target path 决定，而不是 UE 接收到的总功率。对于 UE->target->TRP 或 TRP->target->UE，pathloss reference 可能不是 serving cell DL RS，而是 STx-target-SRx 几何路径、目标 RCS、TSA 距离或 multiple SRx coupling loss。Apple 的观点被主持人引用：由于 sensing RS 的 broadcast nature，Tx power control 应考虑多个 SRx 接收同一 target 的不同 coupling loss。ZTE 提出 UL sensing signal 可研究基于 RSRPP 的 pathloss computation，并提出 DL dedicated sensing RS power 可独立于通信确定（`R1-2604381`, Proposals 12/13, company proposal）。

### 标准化含义

如果 RAN1 最终需要 sensing-specific power control，可能会触及 SRS power control 起点、P0/alpha 参数集、pathlossReferenceRS、TPC command、PHR、power ramping、DL power offset 和 constant power within CPI。Constant power 又与 phase continuity 相关：功率变化如果引入未知相位变化，可能破坏 Doppler；如果 PA 线性且相位稳定，影响可能较小，这需要公司测量和仿真证明。

### 尚未解决的问题

未解决的是 sensing power control 的目标函数：最大化 detection probability、保护 communication SINR、限制 interference、维持 CPI coherence，还是满足多个 SRx 的最坏耦合？这些目标可能冲突。另一个问题是 UE power control 是否能在不泄露目标/UE 位置信息的情况下使用 sensing pathloss。

## 11. Sensing-Assisted Communication

### 技术含义

Summary #5 的 Observation 列出 SAC 用例：sensing assisted beam management、CSI acquisition and reporting、mobility/handover、resource allocation/link adaptation、channel estimation、power saving、power control。最终 Agreement 是 RAN1 should identify at least one representative use case of SAC for Rel-20，并研究它是否需要超出 UAV/human/vehicle/AGV object detection/tracking 的 PHY design impact；FFS physical layer design aspects 是否限于 AI10.8（`R1-2605133`, summary #5, Proposal 6.2-1-rev3, Agreement）。

公司报告给了若干有数字的动机。Huawei 报告 sensing-assisted relaxation 相对 full NR measurement 可降低 UE power consumption 25.4%，并称在 256 ports 或更多端口的场景，利用 sensing assistance 可把 CSI-RS overhead 降到原来的约十分之一，同时 performance loss 小于 3%（`R1-2604278`, Table/Proposal 7-9, company report；也被 `R1-2605133` summary #5 SAC 部分引用）。vivo 报告 Type III sensing related information（AoA/ZoA 或 AoD/ZoD）用于 sensing assisted beam management，可达到最高 97.32% Top-1 beam prediction accuracy，并减少 87.5% measurement overhead，优于 pure RSRP-based scheme（`R1-2603665`, company report；`R1-2605133`, summary #5 SAC 部分）。ZTE 报告 UE->aNB Option 1A 的 aNB access latency 约 1-3.5 ms，UE->SF->aNB Option 1B 约 23-58.5 ms，并据此支持 SAC 中 UE sensing measurement data 直接报告给 aNB（`R1-2604381`, Observation 3/Proposal 9, company report）。

候选 sensing outputs 包括 detected path 的 delay、Doppler、angle、power、phase，static reflector position、AoA/AoD、LoS/NLoS state、RSRPP，dynamic velocity/Doppler、trajectory、AoA/AoD variation、blockage indication，以及 vivo 提出的不同类型 communication assistance information。候选通信用途包括 beam management、CSI prediction/acquisition/reporting、mobility/handover、positioning enhancement、scheduling/resource allocation、link adaptation、channel estimation、power saving 和 power control。

与 10.8.1 Evaluations 中的 SAC Scope 0/1/2/3 可以这样衔接理解：Scope 0 是只评价 sensing 本身，不证明通信增益；Scope 1 是 ideal sensing assistance 下的通信受益上界；Scope 2 是 realistic sensing measurement/report 的通信闭环；Scope 3 是感知与通信资源、信令、算法端到端联动。RAN1#125 10.8.2 实际上仍处在 Scope 1a 式折中：先识别至少一个代表性 SAC 用例和有用 sensing measurement，再判断是否有额外 PHY impact，而不是立即建立完整 Scope 3 评估闭环。

### 标准化含义

SAC 的标准化风险是把实现算法当作规范对象。Summary #5 明确说 detailed algorithms are up to implementation choice，但希望 proponents 说明 full procedure、量化结果、定性分析，以及 useful sensing measurement。也就是说，规范可能定义的是 measurement/report、latency/freshness/confidence、resource trigger 和 PHY control hooks，而不是 beam predictor 或 CSI predictor 算法本身。

### 尚未解决的问题

SAC 仍缺统一闭环：sensing output 的误差、延迟、丢失、false alarm、stale path、static/dynamic classification error 如何传入 communication KPIs；communication side 用什么指标衡量 gain/loss；ideal assistance 与 realistic assistance 如何公平比较；是否需要对 target/object detection 和 environment path sensing 使用不同 measurement level。

## 12. Sensing measurement 与 report

### 技术含义

Measurement/report 的主线是从 NR ISAC 的 Level A/B/C/D 出发，但减少 raw/profile payload。最终 Agreement 只有一个：TRP-side measurement report 中，NR ISAC 的 Level A 不考虑用于 6G ISAC（`R1-2605133`, summary #5, Proposal 5.1-1-rev1, Agreement）。注意 UE-side Level A 在 proposal 中被改成 “not prioritized”，最终 Agreement 只保留 TRP-side，因此不能说 UE-side Level A 已被完全排除。

Level B 是 profile measurement。`Proposal 5.2-1-rev1` 是 Medium Proposal，建议 SRX 可报告一个或多个 consecutive samples set 的 complex values；Option B2 是 delay-angle profile per Tx antenna port per OFDM symbol，Option B3 是 delay-Doppler-angle profile per Tx antenna port；overhead reduction techniques can be studied；FFS 每个 set 的 samples 数量（`R1-2605133`, summary #5, Proposal 5.2-1-rev1, Medium Proposal）。主持人倾向 B2/B3，因为 Level B payload 很大，B3 信息丰富但 UE 需要缓存整个 CPI 做 Doppler，B2 latency/buffer 负担较低。

Level C 是 target path measurement。`Proposal 5.3-1-rev2` 是 Medium Proposal，候选 metrics 包括 delay、target path 与 reference path 的 time difference、Multi-RTT-like measurement、Doppler、absolute/differential angle、AoA at receiver、AoD at transmitter 或可推导 AoD 的 measurement、target path RSRP/SINR、quality/uncertainty/confidence；delay/Doppler/angle ambiguity handling 和 path definition 仍 FFS（`R1-2605133`, summary #5, Proposal 5.3-1-rev2, Medium Proposal）。主持人偏向 “path” 而不是 “point”，原因是 bistatic sensing 中 UE 可能没有 AoA 能力，且 Tx/Rx 移动时 Doppler 不纯粹是 scattering point 的属性。

Level D 是 scattering point/object-like measurement。`Proposal 5.4-1-rev1` 是 Medium Proposal，候选 metrics 包括 3D/2D location、3D/2D velocity、power if applicable e.g. RCS、uncertainty/confidence，以及 optional association of measurements across CPI（`R1-2605133`, summary #5, Proposal 5.4-1-rev1, Medium Proposal）。LG 在 `R1-2604761` 强调 Level A/B 可达 hundreds of megabits 到 tens/hundreds of gigabits，Level C 可降到 several kilobits 到 several tens of kilobits，Level D 更低但需要 association/tracking/fusion 能力，因此 C 更像 practical baseline，D 更像 optional capability（`R1-2604761`, Observation 4/Proposal 8, company proposal）。

UE measurement/report 有两个端点：Option 1A UE measurements/assistance directly to aNB，Option 1B sent to SF。主持人观察是 1A 低 latency、accuracy/control 更好但有 privacy concern；1B 可参考 LPP-like 架构、隐私压力较小但 latency 较大。最终 `Proposal 5.5-1-rev2` 要求 proponents 在 RAN1#126 提供 UE->aNB 和 UE->SF 的动机，考虑 resource/report latency、sensing performance、communication assistance impact、承载数据量的可能 channel、所需 assistance information，并强调可能需要 cross-WG coordination（`R1-2605133`, summary #5, Proposal 5.5-1-rev2, High-priority Proposal）。

TRP measurement/report 的 `Proposal 5.6-1-rev1` 也只是 proposal：TRP as SRx 时，aNB->SF 的 signaling up to other WGs；RAN1 讨论 measurement metrics/levels 和 assistance information；proponents 应在 RAN1#126 提供 aNB->UE Option 2A 的动机（`R1-2605133`, summary #5, Proposal 5.6-1-rev1, High-priority Proposal）。`Proposal 5.7-1-rev3` 则鼓励公司提供 STx/SRx 收发的 assistance information，例如 expected delay/range、Doppler/radial velocity、angle、position/velocity、UE/TRP location、target characteristics 等；这是 Medium Proposal/encourage，不是 Agreement。

### 标准化含义

Measurement/report 是 RAN1 与 RAN2/SA2/RAN3 最强耦合点。RAN1 可定义 “报告什么物理量、精度/置信度/开销如何”，但端到端传输路径、SF 架构、privacy、exposure 和高层触发不应由 RAN1 单独决定。Level A 被 TRP-side 排除说明原始数据上报在 6G ISAC 中很难成为主线；Level C/D 更可能承载常规 sensing report；Level B 可能只适合中心融合或高容量 backhaul。

### 尚未解决的问题

尚未解决的是报告粒度与应用的匹配：object detection/tracking 可能更喜欢 Level D；beam/CSI assistance 可能更需要 Level C path/channel metrics；中心化 fusion 可能要 Level B；privacy-sensitive UE report 可能不能包含完整 location/orientation。还需要定义 timestamp、coordinate system、reference path、ambiguity、confidence 和 payload reduction。

## 13. 公司观点矩阵

### 技术含义

| 矛盾轴 | 偏左阵营/观点 | 偏右阵营/观点 | 代表 TDoc |
|---|---|---|---|
| 通信 RS reuse vs dedicated RS | Qualcomm/Nokia/Ericsson 等强调 reuse mandatory/communication RS、避免碎片化 | Qualcomm 同时提出 FMCW-like chirp/pulsed-OFDM；Apple/CMCC/Google 等开放 dedicated RS，但多要求 KPI 不满足再引入 | `R1-2604711`, `R1-2603868`, `R1-2605133` Proposal 4.2-1 |
| Uniform/simple pattern vs non-uniform/sparse pattern | uniform pattern 易配置、低 sidelobe、便于比较 | SJTU/ZTE/LG/Huawei 等认为非均匀/稀疏可扩展 aperture、缓解 ambiguity，但需控制 sidelobe | `R1-2604495`, `R1-2604381`, `R1-2604761`, `R1-2604278` |
| RAN-centric report vs SF-centric report | Huawei/ZTE/PCL/Samsung 等认为 UE->aNB 对 SAC latency 有利 | Qualcomm 等认为 1B/2B 更符合 SF-centric sensing architecture，1A/2A 需明确用例证明 | `R1-2604381`, `R1-2604711`, `R1-2605133` Proposal 5.5 |
| Communication starting point vs sensing-specific procedure | Huawei 对 beam/power 更保守，认为通信 BM/PC 可适用 | LG/ZTE/Apple 等强调 TSA、target path、多 SRx 和 CPI coherence 使 sensing 需要增强 | `R1-2604278`, `R1-2604761`, `R1-2604381`, `R1-2603868` |
| 低开销 Level C/D vs 中心 raw/profile fusion | LG/Qualcomm/Nokia 等强调 Level A/B payload 太大，C/D 更实际 | Huawei/CMCC/Apple/Ericsson 等对 Level B/B3 保持研究兴趣，尤其在高容量 backhaul 或中心融合 | `R1-2604761`, `R1-2604711`, `R1-2605133` Proposal 5.2 |
| 只评估 sensing vs 端到端 SAC | 一些公司希望先用 detection/tracking 结果观察 PHY impact | Huawei/vivo/ZTE 提供 beam/CSI/mobility/power saving 数字，推动 SAC 闭环 | `R1-2604278`, `R1-2603665`, `R1-2604381` |
| 严格统一假设 vs 允许 company report | 统一假设便于横向比较 | RE pattern、algorithm、CPI、fusion、SAC procedure 仍需要 company report | `R1-2605133` Proposals 4.3/6.4/5.5 |

### 标准化含义

公司分歧并不是简单的“支持/反对 ISAC”，而是 deployment philosophy 不同：网络侧公司更重视 SF、跨节点融合和大规模可管理性；终端/芯片/设备公司更重视 UE 复杂度、功耗、隐私和可实现性；设备商/基础设施公司关注 TRP beam/power/interference 与站间协调；算法型贡献更关注 sparse/non-uniform pattern、CPI 和 estimation performance。

### 尚未解决的问题

需要通过 RAN1#126 的定量结果把分歧转化为 trade-off 曲线：overhead vs sensing accuracy、latency vs SAC gain、dedicated RS performance vs communication impact、Level B payload vs fusion gain、phase continuity mitigation complexity vs Doppler accuracy。

## 14. 关键 Technical Contributions 深读

### 技术含义

| TDoc | 核心贡献 | 证据/结果 | 当前标准状态 |
|---|---|---|---|
| `R1-2604278` Huawei/HiSilicon | Type1/Type2 sensing resources、SAC channel report、phase continuity、SRS hopping、non-uniform pattern | UE power saving 25.4%；CSI-RS overhead 约十分之一且 loss <3%；单次 phase joggling 可容忍，多次 DL/UL switch 会明显退化 | 多数为 company proposal/report；SAC 数字进入 summary #5 讨论 |
| `R1-2603665` vivo | UE location、RS flexibility、SAC beam management、Type III sensing information | 最高 97.32% Top-1 beam prediction accuracy，87.5% measurement overhead reduction | company report，支撑 `Proposal 6.2-1-rev3` 的 SAC 继续研究 |
| `R1-2604381` ZTE/BDN/Sanechips | aNB/UE/SF procedure、measurement level、Option 1A/1B latency、non-uniform GCD、power control、phase continuity、SRU/SRO/SRP | Option 1A 1-3.5 ms，Option 1B 23-58.5 ms；提出 RSRPP/pathloss 和 SRU/SRO/SRP | company proposal；报告路径仍需 cross-WG |
| `R1-2604711` Qualcomm | RS reuse priority、dedicated RS trigger、FMCW-like chirp/ZC、pulsed-OFDM、SF-centric reporting | 强调 dedicated RS 仅在必要时；提出 OFDM-compatible FMCW-like sequence 和 pulse-based waveform | dedicated RS 只进入 `Proposal 4.2-1` encourage |
| `R1-2603868` Apple | parameterized RS framework、constant power in CPI、多 SRx power reference、power ramping/PHR | 主持人引用其 multiple SRX coupling loss 观点 | 候选机制进入 power control company views |
| `R1-2604817` Ericsson | 以通信机制为 starting point，beam/interference/power/measurement 的谨慎边界 | 对 dedicated RS 更保守；支持 QCL association、资源配置和 measurement report 中的 beam 关联 | 多处影响最终 “starting point” 措辞 |
| `R1-2604495` SJTU/NERC-DTV | OFDM-based RS pattern、uniform alternative scattered、multi-CPI velocity disambiguation、CP/ICI 问题 | 两个 CPI + physical range-rate verification 缓解 ghost targets；高 mobility 下 ICI 破坏 OFDM orthogonality | company proposal，支持非均匀/稀疏 RE 研究 |
| `R1-2604761` LG Electronics | Level C/D payload、non-uniform frequency RE、TSA-centric beam、RE-level FDM 风险、TA for sensing | Level A/B payload 极大，Level C/D 更实用；sensing/communication beam direction 可不同 | company proposal，影响 measurement/beam/power 讨论 |
| `R1-2604754` NTT DOCOMO/NTT | RS design 与 Tx/Rx/measurement reporting procedures | 提供多表多图流程视角，连接 RS、Tx/Rx 选择和 report | company proposal，用于下一步 procedure 讨论 |
| `R1-2603537` Nokia | integration overview、resource/measurement/beam/interference/power/SAC | 文档含大量公式/表格，围绕通信机制复用与 sensing-specific impact | company proposal，多个主题被 summary 引用 |
| `R1-2604428` Quectel | reference signal and resource allocation | 数学对象较多，重点在 RS/resource allocation 和 sensing performance trade-off | company proposal，关联 4.x/6.3 |
| `R1-2604777` ITRI/Acer | 资源复用与碰撞处理 | 支持研究 RAN1#124bis `Proposal 4.3-1`，包括 TDM/FDM/CDM/SDM、BWP/gap、PDSCH rate matching、PUSCH muting | 旧版 DOC，公司支持意见；最终只部分收敛进 6.3 Agreement |

### 标准化含义

这些贡献的价值不在于某个公司单点方案立即被采纳，而在于它们解释了为什么 summary #5 会收敛成现在的形态：Huawei/vivo/ZTE 提供 SAC 数字，所以 RAN1 同意至少找一个代表性 SAC 用例；SJTU/ZTE/LG/Huawei 提出非均匀 RS 的 ambiguity/overhead 逻辑，所以 RE pattern evaluation 被保留；Apple/ZTE/LG 讨论 target path、多 SRx 和 CPI，所以 power/beam/phase 都被列入 PHY impact study。

### 尚未解决的问题

多数 technical contributions 仍缺同一 baseline 下的横向结果。特别是 RE pattern 需要同一 overhead 和 same upper bound of unambiguous range/delay/Doppler/angle 下比较；SAC 需要同一 communication workload 和 sensing error model；power/beam/interference 需要同时报告 sensing gain 与 communication loss。

## 15. 与 01_Evaluations 和 03_Waveform_and_Frame_Structure 的关系

### 技术含义

10.8.1 Evaluations 负责场景、目标、指标、channel model 和仿真方法；10.8.2 Integration with Communication 负责 sensing 如何嵌入通信空口和通信过程；10.8.3 Waveform and Frame Structure 则更接近 waveform、sequence、LLS、frame/numerology 以及 RS waveform 特性。Summary #5 在 RE pattern evaluation 处明确说方案可 “same way as 10.8.3”，公司可提供 LLS 或 simplified SLS；这说明 10.8.2 不单独完成 waveform 选择，而是把 integration 需求反馈给 10.8.3。

### 标准化含义

如果 10.8.1 只证明某场景需要更高 Doppler resolution，10.8.2 要回答通信 RS/resource 是否能承载；如果不能，10.8.3 才需要研究 waveform/sequence/frame enhancement。反过来，10.8.3 提出的 dedicated RS 或 waveform 如果没有 10.8.2 的 resource/report/phase/beam/power/interference 约束，也不能直接成为可部署方案。

### 尚未解决的问题

三者之间尚缺统一闭环：同一 RS pattern 在 10.8.3 的 LLS 中表现好，不代表在 10.8.1 的 SLS 中资源开销和干扰可接受，也不代表在 10.8.2 的通信集成中不会破坏 beam/CSI/mobility/power control。

## 16. 标准化成熟度

### 技术含义

| 成熟度 | 内容 | 代表出处 |
|---|---|---|
| 已形成 Agreement | 所有 6GR frequency ranges 和 sensing mode/combination study；RE pattern evaluation options；TRP-side Level A not considered；SAC 至少识别一个代表性用例；resource multiplexing；sensing RS resource allocation；CPI phase continuity；beam/interference/power PHY impact study | `R1-2605133`, summary #5, Proposals 4.3/5.1/6.1/6.2/6.3/6.4/6.5/6.6/6.7 |
| Medium/High Proposal | `4.1-1-rev4` RS generation options；`5.2/5.3/5.4` Level B/C/D metrics；`5.5/5.6/5.7` report path 和 assistance info；`6.1-2/6.1-3` cooperative sensing 与 Tx/Rx procedure | `R1-2605133`, summary #5 |
| Encourage / up to company | dedicated sensing RS design；RE pattern benchmark；phase continuity evaluation；UE/aNB/SF report motivations；assistance information；algorithm/CPI/fusion/company report | `R1-2605133`, summary #5, Proposals 4.2/4.4/5.5/5.7/6.4 |
| FFS / not precluded | QCL/TCI indication；M/N benchmark values；Level B set size；delay/Doppler/angle ambiguity；path definition；physical layer design aspects limited to AI10.8；other interference/resource/RS options | 多个 final proposals |

### 标准化含义

成熟度最高的是“应该研究哪些问题”，成熟度最低的是“采用哪种信号/信令”。因此下一步更像 pre-normative evidence gathering：把公司提案压到共同评估框架里，而不是直接写 normative text。

### 尚未解决的问题

最大的标准化不确定性是不同议题的依赖关系。如果 waveform/frame 没有确定 baseline，RS pattern 和 CPI 很难收敛；如果 Evaluation 没有统一 communication impact metric，SAC 和 resource multiplexing 很难收敛；如果 RAN4 没有 RF 可行性边界，UE/TRP monostatic、phase continuity 和 self-interference 很难写实。

## 17. 研究与专利机会

### 技术含义

已经进入公共评估基线的内容不应被包装成创新点，例如“研究 TDM/FDM/CDM/SDM/PDM”“使用 CSI-RS/SRS 作为候选”“报告 delay/Doppler/angle”等本身都是公开讨论项。真正有研究和专利空间的，是在这些公共基线之上解决未闭合问题。

可分为四类机会：

| 类别 | 方向 | 不应误写为创新点 |
|---|---|---|
| 物理层信号 | 非均匀/结构化 sparse RE pattern、低 sidelobe sequence、FMCW-like OFDM sequence、multi-CPI ambiguity resolution、benchmark-aware RS design | “提出一种 sensing RS” 或 “复用 CSI-RS/SRS” 本身 |
| 信号处理 | reference path/SRU/SRO/SRP phase recovery、CPI event detection、Doppler ambiguity/ghost target 消除、Level C/D association、低开销 confidence/uncertainty | “估计 delay/Doppler/angle” 本身 |
| 协议/过程 | latency-aware UE->aNB/SF report switching、SAC measurement freshness、payload reduction、event-triggered report、Tx/Rx selection without genie target location | “测量报告到 aNB 或 SF” 本身 |
| 硬件/RF 协同 | UE/TRP monostatic leakage modeling、自干扰残差校准、phase/power continuity capability、multi-SRx power reference、sensing-aware PHR | “考虑自干扰/功控” 本身 |

### 标准化含义

专利布局应围绕 FFS/company-report 区域，而不是 Agreement 的公共框架。最有价值的权利要求往往不是“支持 ISAC”，而是某个可验证 trade-off：在给定 overhead、M/N benchmark、CPI length、phase event rate 或 latency budget 下，如何提升 sensing accuracy 或 communication gain。

### 尚未解决的问题

开放问题包括：dedicated RS 的 KPI 触发条件；RS pattern 的统一比较方法；SAC realistic sensing assistance 模型；Level B/C/D payload reduction；phase discontinuity 的容忍度；sensing power control 的 target-path reference；interference measurement 是否需要 sensing-specific quantities；UE privacy 与 report utility 的折中。

## 附录 A：术语表

| 术语 | 含义 |
|---|---|
| Agreement | summary #5 明确记录的 RAN1 共识 |
| Moderator Proposal | 主持人整理的候选文本，除非转为 Agreement，否则不是共识 |
| FFS | 待进一步研究，当前没有定论 |
| Encourage companies | 鼓励公司下次提交结果或方案，不是 mandatory baseline |
| Up to company report | 仿真/算法/参数允许公司自报，横向比较需谨慎 |
| Not precluded | 不排除其他方案，不等于支持或采用 |
| STx/SRx | 感知发射/接收节点，可为 TRP 或 UE |
| aNB/SF | 6G 接入节点/感知功能，报告端点涉及跨 WG |
| Level A/B/C/D | NR ISAC measurement levels：raw/profile/path or point/scattering point or object-like |
| CPI | coherent processing interval，决定 Doppler/coherent integration |
| TSA | target sensing area，决定 sensing beam/power/resource 目标区域 |

## 附录 B：主要 Proposal 状态表

| Proposal | Summary #5 状态 | 关键内容 | 注意事项 |
|---|---|---|---|
| 4.1-1-rev4 | Medium Proposal | sensing RS 可由 communication RS 同/多类型组合、comb factor、non-uniform symbol/subcarrier、time-staggered mapping 等生成 | 非 Agreement |
| 4.2-1 | Closed / encourage | 公司下次提交 dedicated sensing RS 设计和性能评估；only if enhanced communication-RS route fails objectives | 不是 dedicated RS 采用 |
| 4.3-1-rev2 | Agreement | RE pattern evaluation options：full SLS、non-cooperative SLS、single STX/SRX、LLS/simplified SLS up to company | 只确定继续研究选项 |
| 4.4-1-rev4 | High Proposal | 鼓励公司提供 RE pattern 和 benchmark pattern，每 M subcarriers、每 N symbols，报告 overhead/performance | 非 Agreement |
| 5.1-1-rev1 | Agreement | TRP-side Level A from NR ISAC 不考虑用于 6G ISAC | UE-side 不是同等 Agreement |
| 5.2-1-rev1 | Medium Proposal | Level B B2/B3 profile，overhead reduction，FFS set size | 非最终报告格式 |
| 5.3-1-rev2 | Medium Proposal | Level C target path metrics：delay/Doppler/angle/RSRP/SINR/confidence 等 | ambiguity/path definition FFS |
| 5.4-1-rev1 | Medium Proposal | Level D scattering point metrics：location/velocity/RCS/confidence/association | 非最终对象报告 |
| 5.5-1-rev2 | High Proposal | UE->aNB/UE->SF motivations for RAN1#126, cross-WG coordination | 端点未定 |
| 5.6-1-rev1 | High Proposal | TRP/aNB measurement report and assistance info，aNB->SF up to other WGs | 端点未定 |
| 5.7-1-rev3 | Medium Proposal | assistance information examples：delay/range、Doppler、angle、position/velocity、location、target characteristics | encourage |
| 6.1-1-rev1 | Agreement | study all 6GR frequency ranges、individual sensing modes、multiple sensing mode combinations | scope Agreement |
| 6.1-2-rev1 | Medium Proposal | cooperative sensing 定义 | 非 Agreement |
| 6.1-3 | Medium Proposal | Tx/Rx selection、start/end、activation/deactivation company inputs | 非 Agreement |
| 6.2-1-rev3 | Agreement | Rel-20 至少识别一个代表性 SAC 用例，研究额外 PHY impacts | 用例未下选 |
| 6.3-1-rev1 | Agreement | TDM/FDM/CDM/SDM/PDM multiplexing；collision handling or signal sharing | data channel sensing 未成为 Agreement |
| 6.3-1-rev4 | Agreement | sensing RS resource allocation；frequency resource may/may not differ from communication | 只定 study item |
| 6.4-1-rev1 | Agreement | CPI definition、phase continuity factors、maintain/recover、measurement/report impact | 需 RAN1#126 证据 |
| 6.5-1-rev5 | Agreement | beam related aspects for RS used for sensing，communication BM as starting point | 未确定新增 beam procedure |
| 6.6-1-rev4 | Agreement | interference handling for sensing，mitigate within sensing and between sensing/communication | 未确定 report/muting |
| 6.7-1-rev5 | Agreement | UE power control/TRP power allocation for RS used for sensing | P0/alpha/TPC/PHR 等只是候选 |

## 附录 C：待深入阅读的 Technical Contributions 清单

| TDoc | 公司/来源 | 标题 | 主题 | 仿真 | 数学/模型 | 信号设计 | 流程/过程 | 关联 Proposal |
|---|---|---|---|---:|---:|---:|---:|---|
| R1-2603537 | Nokia | On integration of sensing and communications in 6GR | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>sensing-assisted communication<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7, 6.2, 6.1 |
| R1-2603605 | Spreadtrum, UNISOC | Discussion on aspects of integration of sensing with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.2, 6.1 |
| R1-2603665 | vivo | Discussion on integration aspects for 6G ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.2, 6.1 |
| R1-2603708 | EURECOM | Aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.1 |
| R1-2603737 | Tejas Networks | Discussion on Aspects of Integration of Sensing with Communication for 6G | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7, 6.2, 6.1 |
| R1-2603789 | NEC | Aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7, 6.1 |
| R1-2603813 | Sharp | Views on aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>interference / collision<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.6, 6.1 |
| R1-2603868 | Apple | On Aspects of Integration of ISAC with Communications | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7, 6.2, 6.1 |
| R1-2603874 | Cohere Technologies | On integration of sensing with communication in 6GR | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>interference / collision<br>power control<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 6.6, 6.7, 6.1 |
| R1-2603930 | CATT, CICTCI | Discussion on aspects of integration with communication for 6G ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7, 6.1 |
| R1-2603943 | TCL | Discussion on sensing aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>interference / collision<br>overall scope / spec impact | Y | - | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.6, 6.1 |
| R1-2603951 | SK Telecom | Discussion on ISAC aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>beam management<br>interference / collision<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.5, 6.6, 6.1 |
| R1-2603993 | Xiaomi | Discussion on aspects of integration with communication for ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.1 |
| R1-2604028 | CMCC | Discussion on Integration with Communication for Sensing | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.1 |
| R1-2604035 | Transsion Holdings | Discussion on Aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>beam management<br>interference / collision<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.5, 6.6, 6.1 |
| R1-2604040 | NTHU | Views on Aspects of Integration with Communication for 6G ISAC UAV Scenarios | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.1 |
| R1-2604044 | InterDigital, Inc. | ISAC Aspects of Integration with Communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>interference / collision<br>power control<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.6, 6.7, 6.1 |
| R1-2604133 | OPPO | Discussion on sensing detection and integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>interference / collision<br>power control<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.6, 6.7, 6.1 |
| R1-2604201 | Samsung | Discussion on aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.2, 6.1 |
| R1-2604233 | Lenovo | On the aspects of communication and sensing integration | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7, 6.2, 6.1 |
| R1-2604243 | Wistron | Discussion on aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>beam management<br>interference / collision<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.5, 6.6, 6.1 |
| R1-2604278 | Huawei, HiSilicon | Sensing integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>sensing-assisted communication<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.2, 6.1 |
| R1-2604305 | China Telecom | Discussion on aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7, 6.2, 6.1 |
| R1-2604307 | National Institute of Standards and Technology (NIST) | Views on Integration of Sensing with Communication in 6GR | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>sensing-assisted communication<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.5, 6.6, 6.7, 6.2, 6.1 |
| R1-2604314 | MediaTek Inc. | Discussion on sensing aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.1 |
| R1-2604381 | ZTE Corporation, Beijing Digital Nebula, Sanechips | Overall views on 6G ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>interference / collision<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.6, 6.2, 6.1 |
| R1-2604405 | HONOR | Study on aspects of integration with communication for ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.1 |
| R1-2604418 | Hanbat National University | Aspects of Integration with Communication System | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>interference / collision<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.6, 6.1 |
| R1-2604428 | Quectel | Discussion on reference signal and resource allocation for 6G ISAC | sensing RS / signal design<br>resource allocation / multiplexing | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3 |
| R1-2604431 | Pengcheng Laboratory (PCL) | Discussion on Aspects of Sensing Integration with Communication for 6GR | resource allocation / multiplexing<br>measurement / report<br>beam management<br>interference / collision<br>sensing-assisted communication<br>overall scope / spec impact | Y | Y | Y | Y | 6.3, 5.1-5.7, 6.5, 6.6, 6.2, 6.1 |
| R1-2604487 | ETRI | Discussion on aspects of integration with communication for 6GR ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>interference / collision<br>power control<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.6, 6.7, 6.2, 6.1 |
| R1-2604495 | Shanghai Jiao Tong University，NERC-DTV | Discussion on OFDM-based Reference Signal Design for 6G ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>interference / collision<br>power control<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.6, 6.7, 6.1 |
| R1-2604497 | Fraunhofer IIS, Fraunhofer HHI | Aspects of Integrated sensing and communication for 6G ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.1 |
| R1-2604555 | NVIDIA | Views on 6GR ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>beam management<br>interference / collision<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.5, 6.6, 6.2, 6.1 |
| R1-2604556 | Tiami Networks | Aspects of Integration with Communications for ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>interference / collision<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.6, 6.1 |
| R1-2604568 | Ofinno | ISAC integration with communications | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>interference / collision<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.6, 6.1 |
| R1-2604581 | T-Mobile USA, Deutsche Telekom | Discussion on 6G sensing overhead and integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>interference / collision<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.6, 6.1 |
| R1-2604605 | Sony | Discussion on Integration of Sensing with Communication | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>beam management<br>interference / collision<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.5, 6.6, 6.2, 6.1 |
| R1-2604632 | AT&T, FirstNet | Sensing Integration with Communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>interference / collision<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.6, 6.2, 6.1 |
| R1-2604667 | Panasonic | Discussion on Aspect of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>beam management<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.5, 6.2, 6.1 |
| R1-2604670 | CAICT | Discussion on integration of sensing with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.5, 6.6, 6.7, 6.1 |
| R1-2604671 | China Unicom | Discussion on Aspects of Integration with Communication for ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7, 6.2, 6.1 |
| R1-2604711 | Qualcomm Incorporated | Considerations on Integration with Communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.1 |
| R1-2604754 | NTT DOCOMO, INC., NTT, Inc. | RS design and Tx/Rx/measurement reporting procedures for ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>sensing-assisted communication<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7, 6.2, 6.1 |
| R1-2604761 | LG Electronics | Discussion on aspects of integration with communication for 6G ISAC | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>sensing-assisted communication<br>cooperative / multi-TRP sensing<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7, 6.2, 6.1 |
| R1-2604817 | Ericsson | Views on aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7 |
| R1-2604859 | CEWiT | Discussion on aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>interference / collision<br>power control<br>sensing-assisted communication<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.6, 6.7, 6.2, 6.1 |
| R1-2604891 | Google | On 6GR Aspects of Sensing Integration with Communication | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.5, 6.6, 6.7, 6.1 |
| R1-2604898 | Jio Platforms Ltd. | Views on aspects of integration with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>phase continuity / CPI<br>measurement / report<br>beam management<br>interference / collision<br>power control<br>sensing-assisted communication<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 6.4, 5.1-5.7, 6.5, 6.6, 6.7, 6.2, 6.1 |
| R1-2604956 | IIT Kanpur | Discussion on aspects of integration of sensing with communication | sensing RS / signal design<br>resource allocation / multiplexing<br>measurement / report<br>beam management<br>interference / collision<br>sensing-assisted communication<br>overall scope / spec impact | Y | Y | Y | Y | 4.1/4.2/4.3/4.4, 6.3, 5.1-5.7, 6.5, 6.6, 6.2, 6.1 |
| R1-2604777 | ITRI, Acer Incorporated | Discussion on aspects of integration with communication | resource allocation / multiplexing; collision handling; BWP / measurement gap; PDSCH rate matching; PUSCH muting | - | - | - | Y | 6.3 |

## 附录 D：未解决问题清单

| 问题 | 影响 |
|---|---|
| enhanced communication RS 何时不足，需要 dedicated sensing RS | 决定 RS 规范复杂度和部署成本 |
| RE pattern 的统一 benchmark、M/N、overhead 和 ambiguity 指标 | 决定 RAN1#126 结果能否横向比较 |
| Level B/C/D 与 SAC 用例的匹配 | 决定 report payload、latency 和 processing split |
| UE->aNB 与 UE->SF 的端点选择 | 决定 SAC 低延迟闭环和 privacy/architecture |
| CPI phase discontinuity 容忍度 | 决定 TDD switch、SRS hopping、beam/TA/power update 是否需约束 |
| sensing-aware power control target | 决定 P0/alpha/pathloss/TPC/PHR 是否需要扩展 |
| sensing beam 与 communication beam 冲突 | 决定 TCI/QCL/beam report 是否需要 sensing-specific 增强 |
| interference/self-interference modeling | 决定 RAN1 evaluation 与 RAN4 feasibility 的边界 |
| SAC realistic assistance model | 决定是否能证明通信性能增益，而不是只证明 sensing 性能 |

## 附录 E：专利方向速查

1. CPI 内 phase discontinuity detection、reference-path compensation、frequency-hop overlap compensation。
2. 面向 benchmark constraint 的 non-uniform/sparse RE pattern 和 low-sidelobe sequence。
3. FMCW-like OFDM/ZC chirp sequence 与 CP-OFDM pulse generation。
4. Level C/D payload reduction、confidence/uncertainty coding、association across CPI。
5. SAC 的 freshness/latency-aware report trigger 与 UE->aNB/SF adaptive routing。
6. Target-path-aware UE power control、multi-SRx power reference、sensing PHR。
7. TSA-aware beam management、sensing/communication beam conflict arbitration。
8. Sensing interference measurement quantity、muting/rate-matching/collision priority。
9. Privacy-preserving UE location/orientation assistance for bistatic sensing。
10. UE/TRP monostatic residual self-interference calibration and evaluation abstraction。

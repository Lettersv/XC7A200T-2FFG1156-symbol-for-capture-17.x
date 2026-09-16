XC7A200T-2FFG1156C — Cadence / OrCAD 17.x Multi-Part Symbol

针对 AMD/Xilinx XC7A200T-2FFG1156C (FFG1156) 的 Cadence/OrCAD 17.x schematic symbol 项目。

This repository contains the source package-pin database, the original Cadence/OrCAD XML supplied for comparison, the validated final multi-part XML, historical milestones, audit reports, and reusable helper scripts.

推荐发布版本 / Recommended release

Use:

final/XC7A200T-2FFG1156C_by_bank_24part_layout_v13_spacing10_FIXED.xml

This is the final version from the iterative Cadence visual-review process.

Final statistics

Parts                   24
Symbol pins             1156
Physical package balls  1156
NC package balls        100
General row spacing     10 coordinate units
Top margin              30
Bottom margin           20
XML parse               PASS

文件结构 / Repository layout

XC7A200T-FFG1156C/
├── README.md
├── CHANGELOG.md
├── MANIFEST.txt
├── requirements.txt
├── input/
│   ├── xc7a200tffg1156pkg.csv
│   ├── XC7A200T-2FFG1156C.xml
│   └── XC7A200T-FFG1156_Cadence17x_source_package.zip
├── final/
│   ├── XC7A200T-2FFG1156C_by_bank_24part_layout_v13_spacing10_FIXED.xml
│   ├── XC7A200T_by_bank_24part_layout_v13_spacing10_audit.csv
│   └── XC7A200T_by_bank_24part_layout_v13_spacing10_validation.json
├── audits/
├── history/
├── scripts/
└── docs/
    ├── PROJECT_NOTES.md
    └── screenshots/

这个项目做了什么 / What was done

1. 官方封装引脚数据库对照

原始 XML 与 xc7a200tffg1156pkg.csv 按 package-ball number 进行比对，而不是按 XML 中的 pin 顺序比对。

结果：所有 1,156 个封装球均有对应关系，没有缺失或额外 package ball。

2. NC 修正

官方 CSV 中标为 NC 的 100 个 package positions，在最终 XML 中设置为：

<IsNoConnect>
  <Defn val="1"/>
</IsNoConnect>

其它正常 package pins 保持 IsNoConnect=0。

3. Bank-oriented heterogeneous parts

最终库被拆分为 24 个 heterogeneous parts：

A–Q：FPGA banks

R：VCCAUX / VCCBRAM

S：MGT power

T/U：GND（拆成两部分）

V：VCCINT

W/X：NC（拆成两部分）

4. Bank label

每个 bank part 都包含可见的 FPGA Bank 属性，例如 bank-0、bank-13、bank-213。

5. Differential-pair grouping

对 Xilinx 的命名进行解析，将类似：

IO_L22P_T3_13
IO_L22N_T3_13

识别为同一组，并让 P/N 在 symbol 中上下相邻。

这不是简单比较字符串后缀，而是使用 Lxx 通道编号进行配对。

6. GTP layout

对于 GTP banks：

RX：左侧

TX：右侧

同一个 lane 的 RX/TX 水平对齐

MGTREFCLK / MGTRREF 位于左侧，并与 RX 区域保持空白带

7. Power grouping

电源和相关参考信号优先集中排列，然后才排列普通 I/O / GTP 信号，以减少 symbol 高度并提高阅读性。

8. Compact layout

最终普通 pin-row spacing 为 10，symbol 顶部/底部空白也进行了压缩。

Part-A 特殊顺序

Part-A 右侧配置/控制信号按照以下顺序固定：

CCLK_0
TCK_0
TMS_0
TDO_0
TDI_0
PROGRAM_B_0
DONE_0
DXP_0
DXN_0
CFGBVS_0
M0_0
M1_0
M2_0
VP_0
VN_0
INIT_B_0

验证 / Validation

最终验证文件：

final/XC7A200T_by_bank_24part_layout_v13_spacing10_validation.json

布局审计：

final/XC7A200T_by_bank_24part_layout_v13_spacing10_audit.csv

验证包括：

XML parse

package-ball 数量

symbol pin 数量

PhysicalPart pin 数量

package-ball 唯一性

pin-name mapping

NC flags

symbol frame

Part-A 顺序

迭代过程中相关布局约束

Scripts

scripts/ 中包含三个可复用脚本：

validate_pin_mapping.py — 将 package CSV 与 OrCAD XML 按 package-ball number 对照，并检查 NC flags。

compact_layout.py — 对选定 part 压缩 pin-row spacing，并重新计算 symbol frame。

rebuild_bank_labels.py — 用 Cadence XML property 结构添加 bank label。

运行环境只需要 Python 标准库，不需要额外第三方依赖。

一个简单的验证例子

python scripts/validate_pin_mapping.py \
    input/xc7a200tffg1156pkg.csv \
    final/XC7A200T-2FFG1156C_by_bank_24part_layout_v13_spacing10_FIXED.xml \
    --report pin_audit.csv

历史版本 / History

history/ 保存了若干重要迭代版本，方便追踪布局设计过程。

注意：历史版本不保证全部是最终无缺陷版本。它们用于记录迭代过程；实际使用请以 final/ 中的 v13 为准。

Cadence / OrCAD 17.x

该 XML 面向 Cadence/OrCAD 17.x 的 XML → OLB 工作流。推荐流程：

使用 Cadence/OrCAD 17.x 工具将 final XML 转换成 .OLB。

在 Capture 中检查若干代表性 part：A、一个普通 I/O bank、N/O/P/Q、T/U、V/W/X。

确认 pin numbers、pin names、框线、bank label 和多-part package 映射后，再用于正式工程。

本项目环境没有安装 Cadence Capture，因此最终 OLB GUI 渲染仍应由实际 Cadence 环境完成一次确认。

许可证与第三方数据 / Licensing

本项目包含 vendor-originated 数据和 Cadence/OrCAD XML 结构。公开上传到 GitHub 前，请确认：

AMD/Xilinx package pin database 的再分发许可；

Cadence/OrCAD XML schema / library data 的再分发许可；

原始 XML 中任何供应商字段、链接和元数据的使用条件。

如果这些数据不能公开再分发，可以仅公开 scripts/、文档以及你有权公开的最终派生文件，并把 vendor source data 保留在私有仓库。

致谢

本项目的布局规则通过多次实际 Cadence 17.x 视觉检查迭代完成。尤其感谢对 bank 拆分、差分对排列、GTP RX/TX 对齐以及 symbol 紧凑性的逐步反馈。

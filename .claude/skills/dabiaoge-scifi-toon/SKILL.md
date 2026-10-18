---
name: dabiaoge-scifi-toon
description: 用本仓库 characters/ 里的固定角色（表哥、小米、三姑、奶奶）按 Lemo-Opuscar 的「科幻情景喜剧卡通 Sci-Fi Sitcom Toon」(scifi-toon) 风格，用代码做 30–90 秒的短片/视频/动画/单集。适用于"用表哥/小米/三姑/奶奶做个视频""按那个角色出一集""Sci-Fi Sitcom Toon 风格的短片"之类请求。角色外观以 characters/*-prompt.md 与参考图为准，风格不变量以 Lemo-Opuscar 的 styles/scifi-toon/STYLE.md 为准。不用于剪辑已有视频或只出静态角色图。
---

# 大表哥 × Sci-Fi Sitcom Toon 视频生成

把本仓库的固定角色放进 Lemo-Opuscar 的 **Sci-Fi Sitcom Toon** 风格里，用代码导演并渲染一部短片。

三层权威，冲突时按这个顺序取舍：

1. **用户本次要求**（题材、时长、出场角色、语言）。
2. **角色设定**：`characters/<name>-prompt.md` 的「固定特征」和配色，加上 `characters/<name>-reference.png` / `-sheet.png`。卡通化细则见 [references/cast-toon-spec.md](references/cast-toon-spec.md)。
3. **风格不变量**：`$LIB/styles/scifi-toon/STYLE.md`（描边、boil、平涂、12/24 fps、字幕、声音、镜头语法）。全部保留。

故事、梗、世界、镜头、配乐都由你从题材出发自己导演，不要套 Lemo-Opuscar demo（*Coffee Run*）的剧情。

## 流程

1. **拿库 + 装依赖**：调用 `lemo-opuscar:lemo-opuscar` skill（或直接跑它的 `scripts/setup.sh`），得到 `LIB=<path>`；后台启动 `setup.sh deps`。中文配音需要 `deps voice`（edge-tts，需联网）。缺 Node / ffmpeg / Python 时在唯一一轮提问里告诉用户，不要自己装系统软件。
2. **读规范**：`$LIB/AGENTS.md`、`$LIB/DIRECTOR.md`、`$LIB/TECHNIQUE.md`、`$LIB/styles/scifi-toon/STYLE.md`，以及本 skill 的 [references/cast-toon-spec.md](references/cast-toon-spec.md) 和用到角色的 `characters/*-prompt.md`；用 Read 看一遍对应的 reference/sheet 图。
3. **一轮提问**（只问请求里没回答的）：题材/梗概、出场角色（默认 表哥+小米 双人）、时长（默认 45–60 秒）、语言（默认中文配音+中文字幕）、是否先看分镜（默认否）、有没有自备音乐/配音。之后不再追问，用合理默认补齐。
4. **TREATMENT.md**：项目目录 `<片名>/`（小写字母、数字、连字符，建在仓库根目录）。按 DIRECTOR.md §4 写三个结构候选并选一个；**写完 treatment 才可以** `setup.sh demo scifi-toon` 看 demo 代码学技术（toon.js / chars.js / worlds.js / voice.py / score.py）。
5. **定妆**：用真实绘图代码渲染每个出场角色的 model sheet（正面、3/4、剪影、3 个表情），对照参考图和 cast-toon-spec 的检查表；不像就改到像再继续。
6. **制作**：配音 → ASR 校验 → 配乐 → 动画 → 混音 → 渲染，按 TECHNIQUE.md 和 Lemo-Opuscar skill 的「skill 模式」规则（库文件用 `$LIB/…` 绝对路径，从 `$LIB` 运行工具）。
7. **自检交付**：按 DIRECTOR.md §11 交付到项目目录，告诉用户成片路径。

## 这套角色在本风格里的固定玩法

- **双人反差就是引擎**：表哥 = 面瘫、低沉、慢（deadpan）；小米 = 慌张、高、快（panic）。笑点在两人反应不一致。三姑负责连珠炮盘问和加压；奶奶负责最后一锤定音。
- **"回档"遥控器 = 本片的穿越装置**：STYLE.md 要求只有穿越装置发光。按下大红键"回档"打开的漩涡就是 portal，每次回档 = 换一个版本/时间线/世界，换一套调色板和一段音乐。发光色只属于这个漩涡，其他任何东西不得使用。
  - 漩涡要自己设计（例如年夜饭红金色系的旋转倒计时表盘、春联纸屑轨道），不要做成绿色黏液门。
  - 各角色的遥控器外观以角色设定为准：小米 = 黄胶布旧遥控器；表哥 = 胶布旧遥控器；三姑 = 镶钻粉色；奶奶 = 金色带红"福"字（终极版，能终止回档）。
  - 如果这一集的主题和后悔/重来无关，就换一个当集专属的装置，遥控器不出场（与 `skills/xiaomi-magic-tools-ip` 的规则一致）。
- **"可疑的正常世界"**：调色板故意用平淡的米色/棕色，做反转。
- **默认家庭场景**：除夕年夜饭桌（红灯笼、圆桌、一碗白米饭）作为"家"的世界；每次回档后，同一机位重复，只变世界。
- 需要剧情框架或单集工具设计时，先用 `skills/xiaomi-magic-tools-ip`（读它的 SKILL.md 和 references）出单集大纲，再回到本流程制作。角色外观仍以 `characters/` 为准。

## 红线

- 不复刻任何现有剧的角色、剪影、配色组合、名字、口头禅、logo 或传送门样子；片内和文档里不写来源剧名。
- 本仓库角色是原创 IP：保留轮廓、配色、服装、道具这些品牌资产，不要"改良"成别的样子。
- 家庭向：盘问可以尖锐，但不拿隐私侵犯、羞辱弱者、身体伤害当无代价的笑料。
- 用户自己的片子不加 LemoLab 署名、水印或 demo 的片尾卡。
- 不在 `$LIB/styles/` 或 `$LIB/core/` 里改东西；需要的代码复制到项目目录。
- 生成的项目目录（渲染帧、音频中间文件）体积大，不要提交进本仓库，除非用户要求；需要时把它加进 `.gitignore`。

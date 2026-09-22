# 工序镜链 / ProcessShot

**原创、面向工业级生产流程的技能 / An original skill for production-grade industrial workflows**

## 中文介绍

**工序镜链（ProcessShot）** 是原创的工业级生产技能，面向真实工业场景中的设备操作、装配、维护和标准作业培训。它将现场图片素材与用户提供的操作说明整理成可追溯的使用手册，再按步骤制作演示视频。它不绑定特定视频平台；实际创建节点、连线和生成视频时，以当前画布支持的功能为准。

工作流程：

1. 查看每张图片，为画布中的每个图片节点写上准确、唯一的名称。
2. 根据用户的操作说明，在 String 或文本节点中建立带素材索引的使用手册。
3. 请用户确认使用手册。手册中有多少个编号步骤，就规划多少个视频镜头。
4. 先生成第一个镜头，并让用户确认画面与操作是否符合预期。
5. 获得首镜头确认后，批量生成其余镜头。每个视频镜头都连接到该步骤对应的图片节点。
6. 核对镜头数量、步骤顺序、素材连线与视频可播放状态。

面向实际工业使用，技能要求素材、操作步骤和镜头一一对应，并保留人工确认关卡。图片无法辨认、技术细节不明确或步骤缺少对应素材时，会标出缺口，不把推测写成事实。手册或素材映射发生变化时，会重新经过相应的确认环节。

每个镜头的提示词统一包含“视频内容与画面、操作顺序、画面约束”；只有用户要求旁白或音效时，才加入与该镜头动作同步的“旁白与音效”部分。

**使用方式：** 将本仓库作为 Skill 导入，或把 `SKILL.md` 与 `agents/openai.yaml` 放入同名技能目录。在支持节点画布的环境中，上传图片并提供操作说明，然后调用 `canvas-manual-to-video`。

## English introduction

**ProcessShot (工序镜链)** is an original, production-grade skill designed for real industrial workflows, including equipment operation, assembly, maintenance, and standard work training. It turns site images and user-provided operating instructions into a traceable manual, then produces an instructional video shot for each step. It is platform agnostic: node creation, connections, and video generation follow the capabilities of the active canvas.

Workflow:

1. Inspect every image and give each image node a clear, unique name.
2. Create a user manual in a String or text node, including an index of the source images.
3. Ask the user to approve the manual. Each numbered step maps to exactly one video shot.
4. Generate the first shot and ask the user to approve its visual and procedural accuracy.
5. After that approval, generate the remaining shots in a batch. Connect each shot to the image nodes assigned to its step.
6. Verify shot count, step order, image connections, and playable output.

For practical industrial use, the skill keeps source images, procedural steps, and video shots traceable, with human approval gates. It flags unclear images, unspecified technical details, or steps without supporting material instead of presenting guesses as facts. Changes to approved steps or image mappings trigger the relevant approval gate again.

Each shot prompt follows a consistent structure: visual content, operation sequence, and visual constraints. A narration and sound effects section appears only when the user requests audio, and its content follows the actions in that specific shot.

**Usage:** Import this repository as a Skill, or place `SKILL.md` and `agents/openai.yaml` in a skill folder with the same name. In a node canvas environment, upload images, provide the operating instructions, and invoke `canvas-manual-to-video`.

## Import compatibility / 导入兼容性

The repository contains only supported text file types (`.md` and `.yaml`) with compatible ASCII file and folder names. It does not include `.skillignore`. `SKILL.md` is kept below the 20,000-character limit shown by the importer.

仓库仅包含导入器支持的文本格式（`.md`、`.yaml`）及符合命名规则的文件和目录；不包含 `.skillignore`。`SKILL.md` 控制在截图显示的 20,000 字符限制之内。

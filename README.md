# Canvas Manual to Video / 画布使用手册转视频

## 中文介绍

**Canvas Manual to Video** 是一个面向节点画布的 Codex Skill，用于把用户上传的图片素材和操作说明整理成可追溯的操作演示视频工作流。它不绑定特定视频平台；实际创建节点、连线和生成视频时，以当前画布支持的功能为准。

工作流程：

1. 查看每张图片，为画布中的每个图片节点写上准确、唯一的名称。
2. 根据用户的操作说明，在 String 或文本节点中建立带素材索引的使用手册。
3. 请用户确认使用手册。手册中有多少个编号步骤，就规划多少个视频镜头。
4. 先生成第一个镜头，并让用户确认画面与操作是否符合预期。
5. 获得首镜头确认后，批量生成其余镜头。每个视频镜头都连接到该步骤对应的图片节点。
6. 核对镜头数量、步骤顺序、素材连线与视频可播放状态。

技能会标出图片无法辨认或步骤缺少对应素材的地方，不把推测写成事实。手册或素材映射发生变化时，会重新经过相应的确认环节。

**使用方式：** 将本仓库作为 Skill 导入，或把 `SKILL.md` 与 `agents/openai.yaml` 放入同名技能目录。在支持节点画布的环境中，上传图片并提供操作说明，然后调用 `canvas-manual-to-video`。

## English introduction

**Canvas Manual to Video** is a Codex skill for turning uploaded images and operating instructions into a traceable, node-based instructional video workflow. It is platform agnostic: node creation, connections, and video generation follow the capabilities of the active canvas.

Workflow:

1. Inspect every image and give each image node a clear, unique name.
2. Create a user manual in a String or text node, including an index of the source images.
3. Ask the user to approve the manual. Each numbered step maps to exactly one video shot.
4. Generate the first shot and ask the user to approve its visual and procedural accuracy.
5. After that approval, generate the remaining shots in a batch. Connect each shot to the image nodes assigned to its step.
6. Verify shot count, step order, image connections, and playable output.

The skill flags unclear images or steps without supporting material instead of presenting guesses as facts. Changes to approved steps or image mappings trigger the relevant approval gate again.

**Usage:** Import this repository as a Skill, or place `SKILL.md` and `agents/openai.yaml` in a skill folder with the same name. In a node canvas environment, upload images, provide the operating instructions, and invoke `canvas-manual-to-video`.

## Import compatibility / 导入兼容性

The repository contains only supported text file types (`.md` and `.yaml`) with compatible ASCII file and folder names. It does not include `.skillignore`. `SKILL.md` is kept below the 20,000-character limit shown by the importer.

仓库仅包含导入器支持的文本格式（`.md`、`.yaml`）及符合命名规则的文件和目录；不包含 `.skillignore`。`SKILL.md` 控制在截图显示的 20,000 字符限制之内。

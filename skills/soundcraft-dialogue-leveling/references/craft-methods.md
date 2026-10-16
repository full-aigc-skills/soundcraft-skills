# 本场景的 Craft 方法与参数

先读取输入与当前选择。数值、路径、名称和对象编号仅为格式示例；用当前输入、实际返回ID和目标尺寸替换后执行。

| 顺序与目的 | 实际方法 | 参数例子 |
| --- | --- | --- |
| 1. 读各说话人片段及电平上下文 | `session.inspect` | `{"detail":"full"}` |
| 2. 对一个选中说话人先做片段增益 | `clip.gain` | `{"delta_db":2}` |
| 3. 做一次增益调整后试听而非反复压缩 | `mix.volume` | `{"db":-3}` |
| 4. 输出试听版 | `file.bounce_mix` | `{"path":"output/dialogue-review.wav","format":"wav","bit_depth":"24"}` |

## 如何调用

宿主内已打开编辑器时使用该同会话MCP；先等待editor_status就绪，读取实际工具schema和当前命令参数。独立CLI可用于文件任务，但新进程与已打开Web编辑器不是同一状态。

本插件MCP包装：`execute`，参数 `{"command": "session.inspect", "params": {"detail": "full"}}`。后续行保持同一会话和对象选择，逐条检查实际结果。

CLI 语法（下面是展示用写法，实际跨平台执行用argv避免shell引号差异）：

soundcraft-cli script steps.txt --in input.scraft --save output.scraft；每行：clip.gain {"delta_db":2}。音频格式转换另用 soundcraft-cli convert input.wav output.wav --rate 48000。

## 参数核对与完成标准

先查 [当前版本观察目录](craft-command-evidence.json) 中对应方法的完整参数，再按实际运行时查询更新。结构化步骤见 [craft-steps.json](craft-steps.json)。

切换说话人时响度自然，低声、笑声、停顿没有异常放大。

边界：示例+2dB不是所有说话人的统一增益；需按实测差异逐人调整，LUFS需独立测量。

保存可编辑工程后由新进程或重新打开的同一宿主会话核对；导出格式单独解码/查看。方法存在和零退出不代表本场景已经完成。

工作流来源与许可见 [workflow-sources.json](workflow-sources.json)。仅借鉴事实与方法，本文与参数例子原创编写。

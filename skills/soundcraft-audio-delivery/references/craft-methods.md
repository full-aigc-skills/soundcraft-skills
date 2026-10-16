# 本场景的 Craft 方法与参数

先读取输入与当前选择。数值、路径、名称和对象编号仅为格式示例；用当前输入、实际返回ID和目标尺寸替换后执行。

| 顺序与目的 | 实际方法 | 参数例子 |
| --- | --- | --- |
| 1. 检查会话长度与媒体 | `session.inspect` | `{"detail":"summary"}` |
| 2. 导出指定范围母版 | `file.bounce_mix` | `{"path":"output/master.wav","format":"wav","bit_depth":"24","normalize":false}` |
| 3. 导出可审阅工程结构说明 | `file.export_session_text` | `{"path":"output/session.txt"}` |

## 如何调用

宿主内已打开编辑器时使用该同会话MCP；先等待editor_status就绪，读取实际工具schema和当前命令参数。独立CLI可用于文件任务，但新进程与已打开Web编辑器不是同一状态。

本插件MCP包装：`execute`，参数 `{"command": "session.inspect", "params": {"detail": "summary"}}`。后续行保持同一会话和对象选择，逐条检查实际结果。

CLI 语法（下面是展示用写法，实际跨平台执行用argv避免shell引号差异）：

soundcraft-cli script steps.txt --in input.scraft --save output.scraft；每行：clip.gain {"delta_db":2}。音频格式转换另用 soundcraft-cli convert input.wav output.wav --rate 48000。

## 参数核对与完成标准

先查 [当前版本观察目录](craft-command-evidence.json) 中对应方法的完整参数，再按实际运行时查询更新。结构化步骤见 [craft-steps.json](craft-steps.json)。

用独立info检查采样率、声道、时长，再播放首尾和关键段。

边界：采样率转换使用CLI convert --rate真实参数；不要只改文件后缀或多次有损转码。

保存可编辑工程后由新进程或重新打开的同一宿主会话核对；导出格式单独解码/查看。方法存在和零退出不代表本场景已经完成。

工作流来源与许可见 [workflow-sources.json](workflow-sources.json)。仅借鉴事实与方法，本文与参数例子原创编写。

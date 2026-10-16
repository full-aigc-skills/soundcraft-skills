# 本场景的 Craft 方法与参数

先读取输入与当前选择。数值、路径、名称和对象编号仅为格式示例；用当前输入、实际返回ID和目标尺寸替换后执行。

| 顺序与目的 | 实际方法 | 参数例子 |
| --- | --- | --- |
| 1. 检查轨道和片段，不先全局降噪 | `session.inspect` | `{"detail":"full"}` |
| 2. 先调整选中片段增益 | `clip.gain` | `{"db":-3}` |
| 3. 对已选剪辑边缘建立等功率淡变 | `edit.fades_create` | `{"shape":"equal power"}` |
| 4. 查询当前实际插件，再决定均衡或降噪 | `engine.plugins` | `{}` |

## 如何调用

宿主内已打开编辑器时使用该同会话MCP；先等待editor_status就绪，读取实际工具schema和当前命令参数。独立CLI可用于文件任务，但新进程与已打开Web编辑器不是同一状态。

本插件MCP包装：`execute`，参数 `{"command": "session.inspect", "params": {"detail": "full"}}`。后续行保持同一会话和对象选择，逐条检查实际结果。

CLI 语法（下面是展示用写法，实际跨平台执行用argv避免shell引号差异）：

soundcraft-cli script steps.txt --in input.scraft --save output.scraft；每行：clip.gain {"delta_db":2}。音频格式转换另用 soundcraft-cli convert input.wav output.wav --rate 48000。

## 参数核对与完成标准

先查 [当前版本观察目录](craft-command-evidence.json) 中对应方法的完整参数，再按实际运行时查询更新。结构化步骤见 [craft-steps.json](craft-steps.json)。

整段试听切口、呼吸声、尾音和电平，保留原轨。

边界：插件目录不证明存在频谱降噪；未找到实际算法时报告未降噪，不用门限器冒充消除底噪。

保存可编辑工程后由新进程或重新打开的同一宿主会话核对；导出格式单独解码/查看。方法存在和零退出不代表本场景已经完成。

工作流来源与许可见 [workflow-sources.json](workflow-sources.json)。仅借鉴事实与方法，本文与参数例子原创编写。

下面是已经整理好的 **Markdown（.md）版本**，你可以：

* ✅ 一键复制
* ✅ 直接保存为 `PRD.md`
* ✅ 放到 GitHub / Obsidian / Notion 使用

---

```md
# DevMesh Orchestrator PRD（多Agent总控系统）

---

# 一、产品概述

## 1.1 产品名称
DevMesh Orchestrator（总控 Agent）

## 1.2 产品定位
一个面向开发任务的多AI调度中枢系统，负责：

- 理解用户目标
- 拆解为结构化任务
- 自动选择最合适的AI工具/模型
- 调度多个Agent执行
- 执行真实开发操作（代码/命令/Git）
- 汇总结果并完成闭环

## 1.3 一句话描述
一个像“技术负责人”的AI，能同时指挥多个AI工具完成开发任务。

## 1.4 MVP目标

```

用户输入需求
→ 总控Agent拆任务
→ 分配给多个子Agent
→ 执行代码/命令
→ 测试结果
→ 返回最终输出

```

---

# 二、系统架构

## 2.1 逻辑架构

```

用户
↓
Brain Agent（总控）
├─ Planner Agent
├─ Coder Agent
├─ Tester Agent
├─ Tool Executor
└─ Git Agent
↓
输出结果

```

---

## 2.2 工程架构

```

Frontend（React）
↓
API层（FastAPI）
↓
Orchestrator（调度层）
↓
Agent层
↓
Model Gateway
↓
Tool Layer
↓
Execution Environment

````

---

# 三、核心模块设计

---

## 3.1 Brain Agent（总控）

### 职责
- 接收用户目标
- 拆任务
- 分配Agent
- 控制流程
- 重试/容错
- 汇总结果

### 输入

```json
{
  "user_input": "写一个Python程序"
}
````

### 输出（任务列表）

```json
[
  {"id": "task_1", "type": "planning"},
  {"id": "task_2", "type": "coding"}
]
```

---

## 3.2 Planner Agent

### 职责

* 分析需求
* 拆分任务

### 输出

```json
{
  "tasks": [
    {"id": "task_1", "type": "coding"}
  ]
}
```

---

## 3.3 Coder Agent

### 职责

* 生成代码
* 修改代码

### 输出

```json
{
  "code": "print(sum(range(1,101)))"
}
```

---

## 3.4 Tester Agent

### 职责

* 判断结果是否正确
* 输出PASS/FAIL

```json
{
  "status": "PASS"
}
```

---

## 3.5 Tool Executor

### 功能

执行真实操作

### 工具列表

#### write_file

```json
{
  "path": "main.py",
  "content": "..."
}
```

#### read_file

```json
{
  "path": "main.py"
}
```

#### run_command

```bash
python main.py
```

---

## 3.6 Model Gateway

### 功能

统一调用不同模型

### 输入

```json
{
  "provider": "deepseek",
  "model": "deepseek-coder",
  "prompt": "..."
}
```

### 输出

```json
{
  "text": "...",
  "tokens": 123
}
```

---

## 3.7 Orchestrator（调度引擎）

### 职责

* 执行任务队列
* 控制流程
* 管理状态

### 执行流程

```
1. 获取任务列表
2. 按顺序执行
3. 调用对应Agent
4. 写入context
5. 执行下一步
```

---

# 四、核心数据结构

---

## Task

```json
{
  "id": "task_1",
  "type": "coding",
  "agent": "coder",
  "status": "pending"
}
```

---

## Context

```json
{
  "user_input": "...",
  "task_1": "...",
  "task_2": "..."
}
```

---

## Execution Log

```json
{
  "task_id": "task_1",
  "status": "success",
  "duration": 1.2
}
```

---

# 五、执行流程

---

## 主流程

```
用户输入
↓
Brain Agent
↓
Planner Agent
↓
Coder Agent
↓
Tool Executor
↓
Tester Agent
↓
Brain汇总
↓
输出
```

---

## 失败重试流程

```
执行失败
↓
Tester返回FAIL
↓
重新分配给Coder
↓
再次执行
```

---

# 六、非功能设计

---

## 6.1 可观测性

* 每个任务状态
* 输入输出
* 耗时
* 错误日志

---

## 6.2 可扩展性

* 新Agent
* 新模型
* 新工具

---

## 6.3 安全

MVP阶段：

* 本地执行
* 无隔离

---

# 七、MVP范围

---

## 必做

* Brain Agent（简化版）
* Planner / Coder / Tester
* Tool Executor（3个工具）
* 串行执行
* 日志系统

---

## 不做

* 并行Agent
* Docker
* 权限系统
* 多用户
* 模板市场

---

# 八、验收标准

---

## 输入

```
写一个Python程序计算1到100的和
```

---

## 系统自动完成

* 生成代码
* 写入 main.py
* 执行 python main.py
* 返回 5050

---

## UI展示

* 每一步执行日志
* Agent分工
* 最终结果

---

# 九、版本规划

---

## V1.1

* 多模型支持
* 模型切换

## V1.2

* 并行Agent
* 多方案对比

## V1.3

* GitHub集成

## V2.0

* Docker沙箱
* 权限系统

---

# 十、设计原则

---

## 中心化调度

所有Agent必须通过 Brain Agent 通信

---

## 结构化任务

必须使用JSON传递

---

## 工具优先

这是执行系统，不是聊天系统

---

## 可控优先

优先：

* 可调试
* 可复现
* 可观测

---

# 十一、总结

本系统本质：

**AI驱动的开发执行操作系统（Agent Orchestration OS）**

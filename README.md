# SlamDoctor Runtime

[![CI](https://github.com/zxinyu-robot/SlamDoctor/actions/workflows/ci.yml/badge.svg)](https://github.com/zxinyu-robot/SlamDoctor/actions/workflows/ci.yml)
[![Python](https://img.shields.io/badge/Python-3.10%2B-3776AB.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)

> 基于接口契约、运行证据和可执行故障图的 SLAM 导航诊断 CLI。

> 本仓库已并入 [legged-navigation-runtime](https://github.com/zxinyu-robot/legged-navigation-runtime) 的 `python/slam_doctor/`；当前仓库保留为只读归档。

## 项目状态

| 项目 | 状态 |
| --- | --- |
| 维护状态 | Archived |
| 后续维护 | [legged-navigation-runtime](https://github.com/zxinyu-robot/legged-navigation-runtime) |
| 运行方式 | ROS-agnostic；支持 JSONL、CSV 和标准输入 |
| 适用场景 | 日志回放、现场问题初筛、自动化验收和回归测试 |

## 项目简介

SlamDoctor 将传感器、定位、地图、规划、控制和系统资源观测统一为事件流，检查时间、坐标、频率、序列、数据年龄和质量契约，再依据“症状 → 假设 → Probe → 修复”故障图对根因排序。它不替代标定、SLAM、控制器或厂商驱动工具。

## 核心能力

- 通用契约：时间倒退、乱序、丢帧、频率、P95 延迟、冻结、离线和 frame 变化。
- 传感器、SLAM、地图、规划、控制和系统资源诊断。
- 故障图：症状、假设、证据、下一条 Probe 和候选修复。
- 输出终端、JSON、Markdown、Mermaid 和 Graphviz DOT。
- ROS 2、rosbag2、MCAP、LTTng 或厂商 SDK 只需转换到标准观测格式。

## 安装与快速开始

要求 Python 3.10+。

```bash
python -m pip install .
python -m pip install "ส.[dev]"
```

安装后提供 `slam-doctor` 主命令；旧命令 `esis` 继续兼容。

```bash
slam-doctor inspect examples/nav_oscillation.jsonl
slam-doctor diagnose examples/nav_oscillation.jsonl --symptom NAV_OSCILLATION
slam-doctor report examples/nav_oscillation.jsonl --symptom LOCAL_PLANNER_OSCILLATION --output report.md
slam-doctor graph --symptom POSE_JUMP --format mermaid
slam-doctor symptoms
```

## 标准观测格式

```json
{
  "sensor_id": "localization",
  "sensor_type": "slam",
  "timestamp": 1720000000.125,
  "receive_timestamp": 1720000000.145,
  "sequence": 1024,
  "values": {"x": 2.1, "y": 0.4, "inlier_ratio": 0.58},
  "metadata": {"frame_id": "world", "tf_available": true}
}
```

## 内置命令

```bash
slam-doctor validate data.jsonl
slam-doctor inspect data.jsonl --config configs/default.yaml
slam-doctor inspect data.jsonl --format json --output health.json
slam-doctor replay data.jsonl --speed 20
robot-adapter | slam-doctor watch
slam-doctor simulate --sensor localization --fault drift --output drift.jsonl
```

## 设计边界

- 核心只执行只读诊断，不修改参数、不重启节点、不发送控制指令。
- Probe 必须在仿真、离线回放或受控测试条件下执行。
- 诊断排序不是校准后的因果概率，单个异常不能证明唯一根因。
- 生产环境应同时采集 source/publish/receive/consume 时间和统一 trace ID。

详细设计：[系统架构](docs/architecture.md) · [故障图指南](docs/fault-graph.md) · [外部试用指南](docs/external-trial.md)。

## 协作

当前协作请转到[目标仓库](https://github.com/zxinyu-robot/legged-navigation-runtime)，并参考 [CONTRIBUTING.md](CONTRIBUTING.md)。

## 许可证

[MIT](LICENSE)

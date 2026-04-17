

# UniVox Fabric

**Unity 跨平台 AI 语音交互 · 可插拔模块化链路框架**

[![License: Commercial Reserved](https://img.shields.io/badge/License-Commercial%20Reserved-purple.svg)](LICENSE)
[![Unity](https://img.shields.io/badge/Unity-2021.3%2B-brightgreen.svg)](https://unity.com)
[![Modular](https://img.shields.io/badge/Design-Pluggable%20Modular-blue.svg)](README.md)
[![AI Voice](https://img.shields.io/badge/AI-Voice%20Interaction-orange.svg)](README.md)


在 Unity 内一键搭建「真人 ↔ AI」完整语音对话链路


模块自由插拔 · 平台无缝适配 · 模型随时切换

[特性](#-核心特性) · [安装](#-快速安装) · [使用](#-快速使用) · [模块](#-内置模块) · [授权](#-许可证)



# 📌 项目定位

UniVox Fabric 是**专为 Unity 打造的 AI 语音交互模块化编织框架**。

以「统一接入、可插拔、可编排」为核心，让你无需处理底层适配，快速构建稳定流畅的**真人与 AI 语音对话系统**。

- **Uni**：统一调度、跨平台兼容
- **Vox**：语音交互、人声 ↔ AI 对话
- **Fabric**：模块化编织、自由组合链路

\---

# ✨ 核心特性

## 🔌 可插拔模块化架构

- STT、TTS、AI 对话、录音、播放全环节独立模块化
- 一键切换 AI 语音厂商/模型，不改动业务代码
- 按需启用/禁用节点，自定义专属交互链路

## 🎮 Unity 原生深度适配

- 支持 Unity 2021.3+ 全系列版本
- 适配 Windows / macOS / Android / iOS
- 低侵入、低耦合、不影响性能

## 🔗 完整 AI 语音对话闭环

**录音 → 降噪 → 转文字 → AI 对话 → 合成语音 → 播放**

内置多轮对话上下文管理，真正实现连续自然对话。

## ⚡ 轻量高效 · 生产可用

- 全异步非阻塞主线程
- 异常捕获与自动降级容错
- 极简 API，3 行代码启动对话

\---

# 🚀 快速安装

## 方式 1：导入 .unitypackage（推荐）

1. 下载 Releases 中最新 `UniVoxFabric.unitypackage`
2. Unity → Assets → Import Package → Custom Package
3. 全选导入，完成

## 方式 2：Git UPM 导入

在 `Packages/manifest.json` 添加：

```json
{
  "dependencies": {
    "com.univox.fabric": "https://github.com/你的用户名/UniVox-Fabric.git#main"
  }
}
```

\---

# 🎯 快速使用

## 1️⃣ 创建核心实例

在场景新建空物体 → 添加 `UniVoxFabricCore` 组件

## 2️⃣ 一键添加模块

- AudioCaptureModule（录音）
- STTModule（语音转文字）
- AIDialogModule（AI 对话）
- TTSModule（文字转语音）
- AudioPlaybackModule（播放）

## 3️⃣ 最简调用代码

```csharp
using UnityEngine;
using UniVoxFabric;

public class DemoVoiceChat : MonoBehaviour
{
    private UniVoxFabricCore core;

    void Awake() {
        core = FindObjectOfType<UniVoxFabricCore>();
        core.AIDialogModule.OnResponseCompleted += OnAIReply;
    }

    public void ToggleRecord() {
        if (core.AudioCaptureModule.IsRecording)
            core.AudioCaptureModule.StopAndProcess();
        else
            core.AudioCaptureModule.StartRecording();
    }

    void OnAIReply(string text) {
        Debug.Log("AI：" + text);
        core.TTSModule.Play(text);
    }
}
```

\---

# 🧩 内置模块

| 模块                 | 功能                 | 可插拔 | 可扩展 |
| :------------------- | :------------------- | :----- | :----- |
| AudioCaptureModule   | 录音、降噪、去回声   | ✅      | ✅      |
| STTModule            | 语音转文字           | ✅      | ✅      |
| AIDialogModule       | AI 对话、上下文管理  | ✅      | ✅      |
| TTSModule            | 文字转语音、音色调节 | ✅      | ✅      |
| AudioPlaybackModule  | 语音播放、音量控制   | ✅      | ✅      |
| ContextManagerModule | 多轮对话记忆         | ✅      | ✅      |

# 🔧 扩展开发

## 自定义模块只需 2 步

1. 继承接口：`ISTTModule` / `ITTSModule` / `IAIDialogModule`
2. 实现方法并挂载到 `UniVoxFabricCore`

框架自动识别、编排、调度，无侵入式接入。

# ⚠️ 注意事项

- 使用第三方 AI 语音服务需自行申请 API Key
- 移动平台需开启麦克风权限
- 异步回调避免跨线程操作 Unity 对象
- 建议在目标平台真机测试兼容性

# 🤝 贡献指南

1. Fork 本仓库
2. 创建 feature/bugfix 分支
3. 提交代码并发起 PR
4. 遵循框架模块化规范

欢迎提交新模块、优化、bug 修复～

# 💬 反馈与支持

- 提交 Issue：bug、需求、建议
- Discussion：交流使用经验与方案
- 商务授权：GitHub 私信联系

**让 Unity AI 语音交互开发，像搭积木一样简单。**

## Star History
https://www.star-history.com/?repos=UniVox-Fabric%2FUniVox-Fabric&type=date&legend=top-left



<a href="https://www.star-history.com/?repos=UniVox-Fabric%2FUniVox-Fabric&type=date&legend=top-left">
 <picture>
   <source media="(prefers-color-scheme: dark)" srcset="https://api.star-history.com/image?repos=UniVox-Fabric%2FUniVox-Fabric&type=date&theme=dark&legend=top-left" />
   <source media="(prefers-color-scheme: light)" srcset="https://api.star-history.com/image?repos=UniVox-Fabric%2FUniVox-Fabric&type=date&legend=top-left" />
   <img alt="Star History Chart" src="https://api.star-history.com/image?repos=UniVox-Fabric%2FUniVox-Fabric&type=date&legend=top-left" />
 </picture>
</a>

---

<div align="center">

MPL2.0 License © [macrocyborg](https://github.com/macrocyborg)


</div>


UniVox Fabric

📖 项目介绍
UniVox Fabric 是一款专为 Unity 引擎 设计的 AI 语音交互模块化框架，以「可插拔、可扩展、统一调度」为核心，打造轻量且强大的语音交互链路中枢。
框架名称解析：
- Uni：统一（Unity 内统一接入、统一调度）、一体化（整合多平台、多 AI 语音模型）；
- Vox：源自拉丁语「声音」，聚焦语音交互核心场景；
- Fabric：「编织层」，寓意将不同的 AI 语音模型、交互模块、平台适配逻辑，像织物一样灵活拼接、自由组合，实现模块化插拔与链路编排。
核心目标：让 Unity 开发者无需关注底层 AI 语音模型的适配细节、跨平台差异，只需通过简单配置，即可快速集成、切换各类 AI 语音交互能力，搭建完整的「真人 ↔ AI」语音对话链路。
✨ 核心特性
🔌 可插拔模块化设计
- AI 语音模型模块化：支持快速接入/切换不同 AI 语音服务商（如讯飞、百度、阿里云、OpenAI TTS/STT 等），无需修改核心业务代码；
- 交互链路模块化：语音采集、降噪、转写（STT）、AI 对话、合成（TTS）、播放等环节均为独立模块，可按需启用、替换、组合；
- 配置驱动：通过配置文件即可完成模块组合与参数调整，降低开发成本。
🎮 Unity 深度适配
- 无缝集成 Unity 生态：支持 Unity 2021.3+ 版本，适配 Windows、Mac、Android、iOS 等主流平台；
- 贴合 Unity 开发习惯：提供 MonoBehaviour 封装、编辑器配置面板，支持代码调用与可视化配置结合；
- 低侵入性：核心逻辑与业务代码解耦，可轻松集成到现有 Unity 项目（如游戏、虚拟助手、VR/AR 交互项目）。
🔗 完整语音交互链路
- 全链路覆盖：语音采集 → 预处理（降噪、去回声）→ 语音转文字（STT）→ AI 对话逻辑 → 文字转语音（TTS）→ 语音播放；
- 链路可定制：支持自定义链路节点，新增/删除交互环节（如增加语音情感识别、关键词触发等）；
- 多轮对话支持：内置对话上下文管理，支持连续多轮「真人 ↔ AI」语音交互，无需手动维护上下文状态。
📊 轻量高效
- 低资源占用：核心代码轻量，无冗余依赖，不影响项目运行性能；
- 异步处理：STT、TTS、AI 调用均支持异步操作，避免阻塞主线程，保证交互流畅性；
- 错误兼容：内置异常捕获与降级策略，避免因单一模块故障导致整个语音链路崩溃。
🔧 易于扩展
- 模块扩展接口：提供统一的模块抽象接口，开发者可自定义实现专属 AI 语音模块、交互逻辑；
- 平台扩展：支持新增自定义平台适配，轻松适配 Unity 支持的各类目标平台；
- 文档完善：提供详细的 API 文档、扩展教程，降低二次开发门槛。
🚀 快速开始
1. 环境准备
- Unity 版本：2021.3+
- .NET 版本：.NET 4.x 或更高
- 依赖：无额外强制依赖（接入具体 AI 语音服务商时，需导入对应 SDK）
2. 安装方式
方式 1：直接导入 Unity 包
1. 下载项目 Releases 中的UniVoxFabric.unitypackage；
2. 在 Unity 编辑器中，依次点击 Assets → Import Package → Custom Package，选择下载的包文件，导入所有资源；
3. 导入完成后，在 UniVoxFabric/Examples 目录下可查看示例场景。
方式 2：通过 Git 导入（推荐）
在 Unity 项目的 Packages/manifest.json 文件中，添加以下依赖：
{
  "dependencies": {
    "com.univox.fabric": "https://github.com/你的用户名/UniVoxFabric.git#main"
  }
}
保存后，Unity 会自动下载并导入框架资源。
3. 快速集成示例（3步实现简单 AI 语音对话）
Step 1：创建 UniVox Fabric 核心实例
在 Unity 场景中创建空物体，命名为 UniVoxFabricCore，添加 UniVoxFabricCore 组件。
Step 2：配置模块
- 在 UniVoxFabricCore 组件面板中，点击「Add Module」，依次添加：
        
  - AudioCaptureModule（语音采集模块）；
  - STTModule（语音转文字模块，选择对应 AI 服务商，填写 API 密钥）；
  - AIDialogModule（AI 对话模块，配置 AI 模型地址、参数）；
  - TTSModule（文字转语音模块，选择对应 AI 服务商，填写 API 密钥）；
  - AudioPlaybackModule（语音播放模块）。
- 保存配置，模块自动关联，形成完整语音交互链路。
Step 3：编写简单调用代码
using UnityEngine;
using UniVoxFabric;

public class SimpleVoiceDialog : MonoBehaviour
{
    private UniVoxFabricCore _core;

    private void Awake()
    {
        // 获取 UniVox Fabric 核心实例
        _core = FindObjectOfType<UniVoxFabricCore>();
        
        // 注册对话完成回调（AI 回复后播放语音）
        _core.AIDialogModule.OnDialogCompleted += (responseText) =>
        {
            Debug.Log($"AI 回复：{responseText}");
            // 调用 TTS 播放 AI 回复
            _core.TTSModule.PlayText(responseText);
        };
    }

    // 开始语音采集（可绑定到按钮点击）
    public void StartVoiceCapture()
    {
        if (_core.AudioCaptureModule.IsCapturing)
        {
            // 停止采集，并触发 STT + AI 对话
            _core.AudioCaptureModule.StopCapture();
            _core.STTModule.ConvertAudioToText(_core.AudioCaptureModule.CapturedAudio);
        }
        else
        {
            // 开始采集语音
            _core.AudioCaptureModule.StartCapture();
            Debug.Log("开始语音采集...");
        }
    }
}
Step 4：运行测试
将脚本挂载到场景物体上，绑定 StartVoiceCapture 方法到按钮，运行场景：
1. 点击按钮开始语音采集，说出想要询问的内容；
2. 再次点击按钮停止采集，框架自动完成 STT 转写、AI 对话、TTS 合成与播放；
3. 在 Console 面板中可查看详细日志。
📋 模块说明
模块名称
核心功能
可插拔性
支持扩展
AudioCaptureModule
采集麦克风语音，预处理（降噪、去回声）
是
支持自定义采集逻辑
STTModule
语音转文字，支持多 AI 服务商切换
是
支持新增自定义 STT 实现
AIDialogModule
处理 AI 对话逻辑，管理对话上下文
是
支持接入各类 AI 对话模型（LLM、专属对话接口）
TTSModule
文字转语音，支持多 AI 服务商切换、音色配置
是
支持新增自定义 TTS 实现
AudioPlaybackModule
播放 TTS 合成语音，管理播放状态
是
支持自定义播放逻辑、音效混合
ContextManagerModule
管理多轮对话上下文，支持上下文清理、持久化
是
支持自定义上下文存储方式
🔧 配置说明
框架所有配置均通过 ScriptableObject 管理，可在 UniVoxFabric/Config 目录下找到对应配置文件，支持可视化编辑：
- UniVoxFabricConfig：核心配置，指定默认模块、日志级别、全局参数；
- STTConfig：STT 模块配置，填写 AI 服务商 API 密钥、转写参数；
- TTSConfig：TTS 模块配置，填写 AI 服务商 API 密钥、音色、语速、音量等参数；
- AIDialogConfig：AI 对话模块配置，填写 AI 模型地址、请求参数、上下文长度等。
📚 扩展教程
如何自定义 AI 语音模块？
1. 继承对应模块的抽象接口（如 ISTTModule、ITTSModule）；
2. 实现接口中的抽象方法（如 ConvertAudioToText、ConvertTextToAudio）；
3. 创建模块实例，继承 MonoBehaviour，并挂载到 UniVoxFabricCore 组件中；
4. 在配置文件中指定自定义模块为默认模块，即可完成替换。
详细扩展示例请查看 UniVoxFabric/Docs/扩展教程.md。
⚠️ 注意事项
- 接入 AI 语音服务商时，需确保 API 密钥有效，并遵守对应服务商的使用规范；
- 不同平台的语音采集、播放存在差异，建议在目标平台进行针对性测试；
- 异步操作需妥善处理回调，避免空引用异常；
- 框架目前支持 Unity 2021.3+，低版本 Unity 可能存在兼容性问题。
🤝 贡献指南
欢迎各位开发者参与项目贡献，无论是 Bug 修复、功能优化，还是新增模块、扩展文档，都非常感谢！
1. Fork 本仓库；
2. 创建 feature/bugfix 分支；
3. 提交代码（请遵循代码规范，添加必要的注释）；
4. 创建 Pull Request，描述修改内容与目的；
5. 等待审核合并。
📄 许可证（商业保留版本）
本项目采用 商业保留许可证（Commercial Reserved License），详见 LICENSE 文件，核心条款如下：
- 非商业使用：个人、非营利性组织可免费使用、修改本项目，无需经过作者许可，但需保留原作者版权声明，且不得用于任何商业盈利场景；
- 商业使用：任何企业、商业机构、以盈利为目的的组织，使用本项目（包括但不限于集成到商业产品、用于商业服务、二次开发后商用等），需提前联系作者获得书面授权，未经授权不得擅自商用；
- 修改与分发：非商业场景下可自由修改、分发本项目，但需保留原作者版权声明，且修改后的衍生作品需遵循本许可证条款；商业场景下的修改、分发，需在获得授权后，按授权协议执行；
- 免责声明：本项目仅提供技术框架，作者不对项目使用过程中产生的任何损失、风险承担责任，使用即视为同意本条款。
本项目采用 MIT 许可证，详见 LICENSE 文件。你可以自由使用、修改、分发本项目，无需经过作者许可，但需保留原作者版权声明。
📞 联系与反馈
- Bug 反馈：请在 GitHub Issues 中提交，详细描述问题场景、复现步骤；
- 功能建议：欢迎在 Discussions 中交流，或提交 Pull Request；
- 其他问题：可通过 GitHub 私信联系作者。
---
💡 感谢使用 UniVox Fabric，愿你的 Unity 语音交互开发更高效、更灵活！
Last Updated: 2026.04

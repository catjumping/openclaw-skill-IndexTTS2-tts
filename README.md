# LipVoice TTS - OpenClaw Skill

> 🔊 LipVoice 语音克隆与文本转语音合成技能，为 OpenClaw 智能助手提供高质量中文语音合成能力。

[![ClawHub](https://img.shields.io/badge/ClawHub-lipvoice--tts-blue)](https://clawhub.ai)
[![Version](https://img.shields.io/badge/version-1.0.0-green)](https://github.com/catjumping/openclaw-skill-IndexTTS2-tts)
[![License](https://img.shields.io/badge/license-MIT-yellow)](LICENSE)

---

## ⚠️ 重要说明：企业会员专属

**本技能需要 LipVoice 企业会员 API Key 才能使用**。

- 官网：[https://lipvoice.cn](https://lipvoice.cn)
- 请联系 LipVoice 客服购买企业会员并获取 API Key
- 非企业会员可以使用 LipVoice 网页版进行语音合成，无需 API

---

## ✨ 功能特性

- 🎤 **声音克隆**：上传 30秒-2分钟 清晰音频即可创建专属声音模型
- 📋 **模型管理**：列出、删除已创建的声音模型
- 🚀 **一键合成**：单命令完成「创建任务→等待→下载」，无需分步操作
- 🪟 **Windows 兼容**：完美解决 Windows 控制台 GBK 中文编码问题
- 📦 **零依赖**：仅使用 Python 标准库，无需安装额外包
- 🔑 **安全设计**：API Key 不硬编码，每个用户使用自己的密钥

---

## 📦 安装

### 方法一：手动安装
1. 下载本仓库代码
2. 将 `lipvoice-tts` 文件夹放到你的 OpenClaw `skills/` 目录下
3. 重启 OpenClaw，技能会自动被识别加载

### 方法二：Git 安装（推荐，方便更新）
```bash
cd /path/to/your/openclaw/workspace/skills
git clone https://github.com/catjumping/openclaw-skill-IndexTTS2-tts.git lipvoice-tts
```

更新技能：
```bash
cd skills/lipvoice-tts
git pull
```

---

## 🔧 配置

### 设置 API Key（二选一）

**方式1：环境变量（推荐，一次配置永久生效）**

Windows PowerShell:
```powershell
$env:LIPVOICE_API_KEY='你的 API Key'
```

Windows CMD:
```cmd
set LIPVOICE_API_KEY=你的 API Key
```

Linux/macOS:
```bash
export LIPVOICE_API_KEY='你的 API Key'
```

想要永久生效，请将环境变量添加到系统环境变量中。

**方式2：命令行参数临时传入**
所有命令都支持 `--api-key ***` 参数。

---

## 🚀 使用方法

进入技能目录：
```bash
cd skills/lipvoice-tts
```

> **注意**：Windows 下用 `py` 启动 Python，Linux/macOS 用 `python3`。

### 1. 查看已有声音模型
```bash
py scripts/lipvoice_tts.py list
```
列出你账号下所有已创建的声音模型，记录下 `audio-id` 用于合成。

### 2. 上传音频创建声音克隆模型
```bash
py scripts/lipvoice_tts.py upload --file <音频文件路径> --name "<模型名称>" [--describe "<描述>"]
```
- 支持格式：mp3/wav/m4a
- 建议使用 30秒-2分钟 清晰无背景噪音的人声，效果更佳

**示例：**
```powershell
py scripts/lipvoice_tts.py upload --file "C:/voices/myvoice.wav" --name "我的声音" --describe "温暖旁白"
```

### 3. 文本转语音合成（自动等待+下载）
```bash
py scripts/lipvoice_tts.py tts --text "<要合成的文本>" --audio-id <模型ID> [选项]
```

**选项：**
- `--style 1|2`：合成模式
  - `1`：参考原音频风格（默认，最贴近克隆声音）
  - `2`：大模型通用风格（更稳定自然）
- `--genre <int>`：语气参数（可选）
- `--output/-o <路径>`：自定义输出 wav 文件路径

**示例：**
```powershell
py scripts/lipvoice_tts.py tts --text "你好，欢迎使用LipVoice语音合成！" --audio-id ABuXqMU5ZnPCFBHtv93wmnhqLM --output "output.wav"
```

### 4. 删除不需要的模型
```bash
py scripts/lipvoice_tts.py delete --audio-id <模型ID>
```
> ⚠️ 删除后不可恢复，请谨慎操作。

---

## 📖 OpenClaw 技能调用示例

在 OpenClaw 对话中可以直接使用：
```
用 lipvoice-tts 合成这句话："今天天气真好，适合出去走走。" 使用我的声音模型。
```
助手会自动调用技能完成合成并返回音频文件。

---

## 🔌 API 接口说明

| 功能 | 方法 | 端点 |
|------|------|------|
| 创建模型 | POST | `/reference/upload` |
| 模型列表 | GET | `/reference/list` |
| 删除模型 | DELETE | `/reference/delete` |
| 创建合成任务 | POST | `/tts/create` |
| 查询合成结果 | GET | `/tts/result?taskId=xxx` |

**API 基础地址**：`https://openapi.lipvoice.cn/api/third/`
**鉴权方式**：HTTP Header 中添加 `sign: <你的API Key>`

---

## ❓ 常见问题

### Q: 提示 "未配置API Key"
A: 请设置环境变量 `LIPVOICE_API_KEY`，或者调用时添加 `--api-key ***` 参数。

### Q: 提示 "sign无效或用户未开通API"
A: 检查 API Key 是否正确，确认你的账号已开通 LipVoice 企业会员 API 权限。

### Q: 合成需要多长时间？
A: 通常 3-10 秒，脚本会自动轮询等待，最长等待 60 秒。

### Q: 为什么Windows下输出乱码？
A: 本技能 v1.0.0 已经修复 Windows GBK 编码问题，如果仍有问题请确保使用 Python 3.7+。

---

## 📝 更新日志

### v1.0.0 - 2026-07-07
- 🎉 首次发布
- ✅ 支持声音模型上传、列表、删除
- ✅ 一键 TTS 合成，自动等待完成并下载
- ✅ 修复 Windows 中文 GBK 编码问题
- ✅ 零依赖，使用 Python 标准库
- ✅ 支持环境变量和参数两种 API Key 配置方式

---

## 📄 许可证

MIT License - 详见 [LICENSE](LICENSE) 文件。

---

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 💬 反馈

如果在使用中遇到问题，请在 GitHub 提交 Issue。

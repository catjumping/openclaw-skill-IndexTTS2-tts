# IndexTTS2 OpenClaw 技能 保姆级使用教程

> 💡 本教程面向完全新手，每一步都写得很详细，跟着做就能用！

---

## 📋 前置准备（必须先完成）

### 第一步：确认你有 Python 环境
IndexTTS2 技能需要 Python 3.6 以上版本运行。

**检查是否安装了Python：**
1. 按下 `Win + R`，输入 `cmd` 回车，打开命令提示符
2. 输入 `py --version` 回车
   - 如果显示类似 `Python 3.10.0` 这样的版本号，说明已经安装，可以跳过这一步
   - 如果提示"不是内部或外部命令"，说明没安装，继续往下看

**安装Python（没装的话）：**
1. 打开 Python 官网下载页面：https://www.python.org/downloads/windows/
2. 下载最新版本的 Python（选 "Download Windows installer (64-bit)"）
3. 运行安装包，**非常重要：一定要勾选最下面的 "Add Python to PATH"**
4. 点击 "Install Now" 等待安装完成
5. 重新打开命令提示符，输入 `py --version` 确认能显示版本号

---

### 第二步：确认你有 IndexTTS2 企业会员 API Key
- 本技能需要企业会员才能使用，官网：https://indextts.xin
- 如果还不是企业会员，请联系官网客服购买开通，获取你的 API Key
- API Key 是一串类似 `QJg6tTU3VBVZCdwdPP7NB2xojNykKs6z` 这样的字符串，请保存好

---

## 🚀 方法一：一键安装（推荐，最简单）

如果你已经安装了 OpenClaw，直接打开终端/命令提示符，运行这一行命令：

```bash
openclaw skills install git:https://github.com/catjumping/openclaw-skill-IndexTTS2-tts.git
```

等待安装完成，显示 `Installed indextts2-tts from git` 就说明安装成功了！

---

## 🚀 方法二：手动安装（如果一键安装失败）

如果上面的一键安装因为网络问题失败了，可以手动安装：

1. 打开技能仓库地址：https://github.com/catjumping/openclaw-skill-IndexTTS2-tts
2. 点击绿色的 `<> Code` 按钮，选择 `Download ZIP` 下载压缩包
3. 解压下载的zip文件
4. 找到你的 OpenClaw 技能目录，一般在：
   ```
   C:\Users\你的用户名\.openclaw\workspace\skills\
   ```
   （把"你的用户名"换成你电脑的用户名，比如 `C:\Users\yanyu\.openclaw\workspace\skills\`）
5. 在 `skills` 文件夹里新建一个文件夹，名字叫 `indextts2-tts`
6. 把解压出来的文件全部复制到 `indextts2-tts` 文件夹里
7. 复制完后，文件夹结构应该是这样的：
   ```
   skills/
   └── indextts2-tts/
       ├── SKILL.md
       ├── scripts/
       │   └── indextts2_tts.py
       ├── README.md
       ├── LICENSE
       └── ...其他文件
   ```
8. 重启 OpenClaw，技能就会被自动识别了

---

## 🔑 配置 API Key（非常重要！）

安装完技能后，必须配置你自己的 API Key 才能使用。配置方式二选一：

### 方式A：临时使用（每次打开终端都要设置）
**Windows PowerShell：**
```powershell
$env:INDEXTTS2_API_KEY='你的API Key'
```
比如你的Key是 `abc123`，就输入：
```powershell
$env:INDEXTTS2_API_KEY='abc123'
```

**Windows CMD（命令提示符）：**
```cmd
set INDEXTTS2_API_KEY=*** Key
```

**Linux/macOS：**
```bash
export INDEXTTS2_API_KEY='你的API Key'
```

### 方式B：永久配置（一次设置，以后不用再输）
1. 右键点击"此电脑" → "属性" → "高级系统设置" → "环境变量"
2. 在"用户变量"下面点击"新建"
3. 变量名填：`INDEXTTS2_API_KEY`
4. 变量值填：你的 API Key
5. 点击确定保存，重启终端就生效了

> 💡 兼容说明：如果你之前用过 LipVoice 旧版本，设置过 `LIPVOICE_API_KEY` 环境变量，不用重新设置，新版本会自动兼容。

---

## 🎯 开始使用

### 方式1：在 OpenClaw 对话中直接用（最方便）
配置好Key之后，你直接和OpenClaw助手说就行了，比如：
> "用IndexTTS2合成这句话：今天天气真好，我们一起出去玩吧！"
> "帮我上传这个音频创建一个新的声音模型，名字叫我的声音"
> "列出我所有的声音模型"

助手会自动调用技能完成操作，合成完直接把音频发给你。

---

### 方式2：手动命令行使用

如果你想手动运行命令，先进入技能的scripts目录：
```cmd
cd C:\Users\你的用户名\.openclaw\workspace\skills\indextts2-tts\scripts
```

> ⚠️ 注意：Windows下用 `py` 开头运行Python，Linux/macOS用 `python3`

#### 📋 命令1：查看你所有的声音模型
```bash
py indextts2_tts.py list
```
运行后会列出你账号下所有已经创建好的声音模型，每个模型都有一个ID，合成的时候需要用到这个ID。

---

#### 🎤 命令2：上传音频创建新的声音克隆模型
准备一段30秒~2分钟的清晰人声（支持mp3/wav/m4a格式，建议无背景噪音），运行：
```bash
py indextts2_tts.py upload --file "你的音频文件路径" --name "模型名称" --describe "模型描述（可选）"
```

**例子：**
```bash
py indextts2_tts.py upload --file "C:\Users\xxx\Desktop\我的声音.wav" --name "我的声音" --describe "温暖男声"
```
运行成功后会显示新模型的ID，记下来。

---

#### 🔊 命令3：文本转语音合成（最常用）
用指定的声音模型合成语音，自动等待合成完成并下载音频：
```bash
py indextts2_tts.py tts --text "要合成的文本内容" --audio-id 模型ID [选项]
```

**所有可选参数：**
| 参数 | 说明 | 默认值 |
|------|------|--------|
| `--text` | 要合成的文本（必填，最多5000字） | 无 |
| `--audio-id` | 声音模型ID（必填，从list命令获取） | 无 |
| `--style` | 模型版本：1=基础模型，2=专业模型，3=多语言模型 | 1 |
| `--genre` | 模型类别：0=参考原音频，1=语气参考，2=参考音频模式 | 0 |
| `--speed` | 语速，范围0.5~1.5，数字越慢语速越慢，越大越快 | 1.0 |
| `--emotion-path` | 参考音频URL（仅专业模型genre=2时用） | 无 |
| `--output/-o` | 音频保存到本地的路径 | 自动生成tts_xxxx.wav |

**最简单的例子（默认参数合成）：**
```bash
py indextts2_tts.py tts --text "你好，欢迎使用IndexTTS2语音合成！" --audio-id ABuXqMU5ZnPCFBHtv93wmnhqLM
```

**例子：自定义语速和输出路径**
```bash
py indextts2_tts.py tts --text "今天天气真不错" --audio-id ABuXqMU5ZnPCFBHtv93wmnhqLM --speed 0.8 --output "C:\Users\xxx\Desktop\输出.wav"
```

> ⏰ 合成通常需要3~10秒，脚本会自动等待，合成完会自动下载音频到你指定的路径，显示"✅ 合成完成"就好了。
> 
> ⚠️ 注意：合成的音频在服务器上只保留1天，请及时下载。

---

#### 🗑️ 命令4：删除不需要的模型
如果某个模型你不用了，可以删除：
```bash
py indextts2_tts.py delete --audio-id 要删除的模型ID
```

**例子：**
```bash
py indextts2_tts.py delete --audio-id ABuXqMU5ZnPCFBHtv93wmnhqLM
```
> ⚠️ 删除后无法恢复，请谨慎操作！

---

## 🔄 更新技能到最新版本
如果后续发布了新版本，你只需要运行一行命令就能更新：
```bash
openclaw skills update indextts2-tts
```

---

## ❓ 常见问题

### Q：运行命令提示"未配置API Key"怎么办？
A：说明你没配置环境变量，按照上面"配置API Key"的步骤设置一下，或者运行命令的时候临时加上`--api-key 你的Key`参数：
```bash
py indextts2_tts.py --api-key *** list
```

### Q：提示"sign无效或用户未开通API"怎么办？
A：检查一下你的API Key有没有输错，确认你的账号已经开通了IndexTTS2企业会员API权限。

### Q：Windows下中文显示乱码怎么办？
A：新版本已经修复了编码问题，如果还有乱码，请升级到Python 3.7以上版本。

### Q：合成等待很久都没完成？
A：正常情况下10秒内都会完成，如果超过60秒还没完成会超时，可以重新运行试试。

### Q：支持什么音频格式？
A：上传参考音频支持mp3、wav、m4a格式，合成输出是wav格式。

### Q：最多支持多少字？
A：单次合成最多支持5000个字符。

---

## 📞 其他问题
如果遇到其他问题，可以在GitHub提交Issue：https://github.com/catjumping/openclaw-skill-IndexTTS2-tts/issues

---

**祝你使用愉快！🎉**

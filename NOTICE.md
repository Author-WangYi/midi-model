# NOTICE · 版权归属与修改说明

本文件说明「midi-model 便携版」这份发行版的来源、所作的全部修改，以及其中所含第三方作品的版权与许可。

---

## 一、本发行版

| 项目 | 内容 |
|---|---|
| 名称 | midi-model 便携版（Windows） |
| 发布者 | Author王异 |
| 基于 | [SkyTNT/midi-model](https://github.com/SkyTNT/midi-model) 官方 Windows 发布包 **v1.3.5** |
| 许可证 | **Apache License 2.0**（全文见本仓库 [LICENSE](LICENSE)） |
| 性质 | **非官方**发行版。原作者未参与制作，也未提供任何形式的认可或背书 |

---

## 二、所含作品的版权与许可

本发行版包含或改编自以下作品，经核查**全部采用 Apache License 2.0**：

### 程序

| 作品 | 作者 | 许可 |
|---|---|---|
| Midi-Model —— 源码及官方 Windows 发布包 v1.3.5 | [SkyTNT](https://github.com/SkyTNT) | Apache-2.0 |

### 模型权重

| 界面里显示的模型 | 来源仓库 | 作者 | 许可 |
|---|---|---|---|
| skytnt 原版（默认） | `skytnt/midi-model-tv2o-medium` | SkyTNT | Apache-2.0 |
| skytnt + jpop lora | `skytnt/midi-model-tv2om-jpop-lora` | SkyTNT | Apache-2.0 |
| skytnt + touhou lora | `skytnt/midi-model-tv2om-touhou-lora` | SkyTNT | Apache-2.0 |
| asigalov61 tv2o-large | `asigalov61/Music-Llama` | asigalov61 | Apache-2.0 |
| asigalov61 tv2o-medium | `asigalov61/Music-Llama-Medium` | asigalov61 | Apache-2.0 |
| skytnt tv1-medium | `skytnt/midi-model` | SkyTNT | Apache-2.0 |

### 训练数据（由上述模型作者使用，非本发行版内容）

| 数据集 | 提供方 |
|---|---|
| `projectlosangeles/Los-Angeles-MIDI-Dataset` | Project Los Angeles |
| `projectlosangeles/Monster-MIDI-Dataset` | Project Los Angeles |

> 原项目是**开源项目**，其许可证副本已随本仓库提供（`LICENSE` 文件）。原项目的原始版权声明与许可条款均予以保留，未作任何删改。

---

## 三、本发行版所做的全部修改

以下为对官方发布包 **v1.3.5** 的改动清单。**程序功能、模型权重、用户界面均未修改。**

| # | 改动 | 原因 |
|---|---|---|
| 1 | **内置 FluidSynth 运行库**（约 6 MB） | 官方包不含此库，且只提供源码，需要用户自行寻找 Windows 编译版并配置环境变量。没有它程序启动即崩溃 |
| 2 | **新增 `启动.bat` 自愈式启动器** | 程序内部把库路径**硬编码**为 `C:\tools\fluidsynth\bin`，该路径不存在就会抛出 `FileNotFoundError` 直接闪退。而写死的路径又导致整个文件夹无法改名、无法换盘。启动器在每次运行时自动维护一个指向当前文件夹的目录链接，从而让文件夹可改名、可搬迁、可换电脑 |
| 3 | **预置模型文件与音色库**（约 990 MB） | 官方包首次运行需联网从 HuggingFace 下载。国内网络对 HuggingFace 的访问存在 DNS 污染，该步骤经常超时失败。预置后**离线即可用** |
| 4 | **补齐 5 个扩展模型**（独立的可选包） | 同一原因。程序在切换模型时会联网下载，且下载地址写死在程序内部、不读取任何环境变量，因此无法通过换镜像源解决 |
| 5 | **移除未被使用的冗余依赖**（约 3 GB） | 官方包内附带了整套 PyTorch 相关运行库。经实测验证，该程序实际使用 **ONNX Runtime** 推理，PyTorch 从未被加载。移除后功能不受任何影响 |
| 6 | **重新打包为可整体搬运的目录结构** | 使整个文件夹可复制、可搬运、可在任意磁盘位置运行 |

### 明确没有做的事

- 未修改 `app.exe`，也未修改 `_internal` 目录内任何程序文件
- **未修改、未重训、未量化任何模型权重** —— `model_base.onnx` 与 `model_token.onnx` 均为原始文件，字节数与来源仓库完全一致
- 未修改界面文案、功能逻辑或默认参数
- 未添加任何遥测、联网上报、后台驻留或开机自启行为
- 未更改任何文件的许可证声明

---

## 四、第三方组件

本发行版内含下列第三方组件，各自遵循其原有许可证：

| 组件 | 许可证 | 用途 |
|---|---|---|
| [FluidSynth](https://www.fluidsynth.org/) | **LGPL-2.1** | MIDI 音频合成（`libfluidsynth-3.dll` 等） |
| [ONNX Runtime](https://onnxruntime.ai/) | MIT | 模型推理引擎 |
| [Gradio](https://gradio.app/) | Apache-2.0 | 本地网页操作界面 |
| [PyInstaller](https://pyinstaller.org/) | GPL-2.0 **含引导程序例外条款** | 将 Python 程序打包为可执行文件（官方发布包即由它打包） |
| SoundFont 音色库 `soundfont.sf2` | 随原项目发布包提供 | 音色采样数据 |

> **关于 FluidSynth**：其为 LGPL-2.1 许可。本发行版以**未经修改的共享库（DLL）**形式原样分发，并通过动态调用的方式使用，符合该许可证对动态链接的要求。该组件的完整源码可从其官方网站获取。

---

## 五、声明与免责

- 本发行版与原项目作者 **SkyTNT**、模型作者 **asigalov61** 均**无隶属关系**，未获其赞助、授权或背书。请勿将本发行版与官方版本混淆
- 程序与模型的一切版权归原作者所有
- 本发行版**免费提供**，禁止任何形式的商业销售或付费转载
- 转载、二次分发请保留本文件与 `LICENSE`，并注明来源
- 本发行版为免费分享，发布者不对使用后果承担任何责任
- 使用者应自行遵守 Apache License 2.0 及所在国家/地区的法律法规

---

## 六、引用原项目

本工具的核心工作由原作者完成。如在学术工作中使用，请引用原项目：

```bibtex
@misc{skytnt2024midimodel,
  author = {SkyTNT},
  title = {Midi Model: Midi event transformer for symbolic music generation},
  year = {2024},
  howpublished = {\url{https://github.com/SkyTNT/midi-model}},
}
```

---

*本文件最后更新：2026-09-16*

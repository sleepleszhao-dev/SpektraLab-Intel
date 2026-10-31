# SpektraLab 1.3.4 Intel

非官方 Intel Mac（x86_64）兼容构建，基于 SpektraLab 官方 GPL-3.0 源码重新编译。

> 这不是原作者发布的 macOS 版本，也不代表 SpektraLab 官方提供 Intel 支持。

## 下载

请从 [Releases](../../releases/latest) 下载：

| 文件 | 用途 |
|---|---|
| `SpektraLab-1.3.4-intel.dmg` | 推荐安装包 |
| `SpektraLab-1.3.4-intel-app.zip` | 已签名的 `.app` 压缩包 |
| `SpektraLab-1.3.4-intel-source.tar.gz` | 与本构建对应的完整修改源码 |

打开 DMG，将 `SpektraLab.app` 拖进 Applications。

## 系统要求

- Intel Mac，`x86_64`
- macOS 15.0 或更高版本
- 支持 Metal 的 AMD 或 Intel GPU
- 常规图片建议至少 8 GB 内存

本项目中的二进制不含 arm64 切片，不是通过 Rosetta 转译运行。

## 已验证环境

构建与验证环境：

- iMac19,1，Intel Core i5
- AMD Radeon Pro 570X，4 GB 显存
- macOS 15.4.1
- Xcode 16.4 / macOS SDK 15.5

验证内容：

- Metal GPU 冒烟测试：0 失败
- `core=native-metal`
- Metal 安全数学模式检查通过
- 实际打开测试图片、配光、显影和全分辨率渲染
- 成功导出 JPEG 与 16-bit TIFF
- DMG 只读挂载、签名和架构检查通过

## Intel 兼容改动

- 将 Xcode 工程目标改为 x86_64。
- 使用传统 `.icns` 替代 Xcode 26 专属 Icon Composer 资源。
- 增加少量 Swift 6.1 类型推断、并发和 actor 隔离兼容修复。
- 使用 Swift 5 语言模式构建界面，绕开 Xcode 16.4 在大型 Swift 6 模块上的编译器崩溃。C++ 引擎仍使用 `-O2`。
- 调整 Metal 数学探针，使用显式 precise FMA，避免旧版 Intel 主机 Metal 工具链把探针重排为 0；检查没有关闭。
- 原始 arm64 DMG 未被修改。

详细说明见 [BUILD-NOTES.md](BUILD-NOTES.md)。

## 校验

下载后可在终端运行：

```bash
shasum -a 256 -c SHA256SUMS.txt
```

`SHA256SUMS.txt` 给出 DMG、应用可执行文件和 Release 压缩包的校验值。

## 签名说明

应用使用本机 ad-hoc 签名并启用 Hardened Runtime，但没有 Apple Developer ID，也没有经过 Apple 公证。

- 本机生成并安装通常可以直接打开。
- 其他 Mac 可能显示 Gatekeeper 警告。
- 不应把该构建描述为 Apple 公证版本。

## 从源码构建

对应源码位于 Release 的 `SpektraLab-1.3.4-intel-source.tar.gz`，原始上游为：

```text
repository: https://github.com/JamesQiu2005/SpektraLab
tag:        spektralab-v1.3.4
commit:     fd891c6bc5eb350387602deb5c1ef9b6dcdab948
```

主要构建步骤：

```bash
engine/build.sh bundle

cd modern_UI/Spektrafilm
SPEKTRAFILM_ARCHS=x86_64 python3 Tools/gen-project.py
xcodebuild \
  -project Spektrafilm.xcodeproj \
  -scheme Spektrafilm \
  -configuration Release \
  -destination 'platform=macOS,arch=x86_64' \
  ARCHS=x86_64 \
  ONLY_ACTIVE_ARCH=NO \
  SWIFT_VERSION=5 \
  SWIFT_STRICT_CONCURRENCY=minimal \
  CODE_SIGN_IDENTITY=- \
  DEVELOPMENT_TEAM= \
  build
```

构建资源来自官方 1.3.4 应用包；源码归档已经包含构建后的同一组资源。

## 已知限制

- 当前验证没有覆盖 100 MP RAW 的内存与稳定性。
- 8 GB 内存机器处理大型 RAW 可能依赖交换空间，并显著变慢。
- 非官方构建，不保证后续版本兼容。
- 没有 Apple 公证，分发到其他 Mac 时可能有安全提示。

## 许可证与署名

- 应用与引擎：GNU GPL v3 或更高版本，见 [LICENSE](LICENSE)。
- 胶片/相纸 profiles 与打印 LUT：CC BY-SA 4.0，具体署名见源码中的许可证文件。
- metal-cpp：Apache-2.0。

原始项目：[JamesQiu2005/SpektraLab](https://github.com/JamesQiu2005/SpektraLab)  
色彩模型来源：[andreavolpato/spektrafilm](https://github.com/andreavolpato/spektrafilm)

本项目只提供 Intel 构建适配、验证结果与分发文件，不主张原始软件、色彩模型或品牌的所有权。

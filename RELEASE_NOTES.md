# SpektraLab 1.3.4 Intel 构建

首个经过实际 Intel GPU 验证的 x86_64 构建。

## 包含

- Intel DMG 安装包
- 已签名应用 ZIP
- 对应修改源码
- SHA-256 校验文件

## 已验证

- AMD Radeon Pro 570X Metal 渲染
- 安全数学模式与 GPU 冒烟测试
- 图片打开、配光、显影
- JPEG 与 16-bit TIFF 导出
- DMG 挂载和代码签名

## 注意

- 非官方构建
- 仅支持 Intel Mac / macOS 15+
- ad-hoc 签名，未经过 Apple 公证
- 8 GB 内存机器不建议处理 100 MP 大图

完整说明见仓库 README 与 `BUILD-NOTES.md`。

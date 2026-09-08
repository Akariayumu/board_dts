# Orin Nano/NX USB 配置

Jetson Orin Nano/NX 刷入官方系统后，通过替换（或追加）设备树文件，使底板上的
3 个 USB3.2 端口和 Type-C 端口正常工作。

## 版本目录

| 目录 | 适用系统版本 | 生效方式 |
| --- | --- | --- |
| [jetpack-5.1.4](jetpack-5.1.4/) | JetPack 5.1.4 | 替换 `/boot/dtb` 中的同名 DTB |
| [jetpack-5.1.5-super](jetpack-5.1.5-super/) | JetPack 5.1.5 (super) | 替换 `/boot/dtb` 中的同名 DTB |
| [jetpack-6.2](jetpack-6.2/) | JetPack 6.2 | 追加 DTB 并修改 `extlinux.conf` |
| [jetpack-7.2](jetpack-7.2/) | JetPack 7.2 | 追加 DTB 并修改 `extlinux.conf`（**未验证**） |

请按设备实际刷入的 JetPack 版本选择对应目录，并按目录内 `README.md` 操作。
不同版本的 DTB 不可混用。

## 模块与 DTB 对应关系

| 模块 | JetPack 5.1.4 / 5.1.5 (super) | JetPack 6.2 | JetPack 7.2 |
| --- | --- | --- | --- |
| Orin Nano 4G | `kernel_tegra234-p3767-0004-*.dtb` | `orin_nano_4g.dtb` | — |
| Orin Nano 8G | `kernel_tegra234-p3767-0003-*.dtb` | `orin_nano_8g.dtb` | `orin_nano_8gb.dtb` |
| Orin NX 8G | `kernel_tegra234-p3767-0001-*.dtb` | `orin_nx_8g.dtb` | — |
| Orin NX 16G | `kernel_tegra234-p3767-0000-*.dtb` | `orin_nx_16g.dtb` | — |

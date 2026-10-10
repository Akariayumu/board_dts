# Orin Nano/NX USB 配置

Jetson Orin Nano/NX 刷入官方系统后，通过替换（或追加）设备树文件，使底板上的
3 个 USB3.2 端口和 Type-C 端口正常工作。

## 版本目录

| 目录 | 适用系统版本 | 生效方式 |
| --- | --- | --- |
| [jetpack-5.1.4](jetpack-5.1.4/) | JetPack 5.1.4 | 替换 `/boot/dtb` 中的同名 DTB |
| [jetpack-5.1.5-super](jetpack-5.1.5-super/) | JetPack 5.1.5 (super) | 替换 `/boot/dtb` 中的同名 DTB |
| [jetpack-6.2](jetpack-6.2/) | JetPack 6.2 | 追加 DTB 并修改 `extlinux.conf` |
| [jetpack-6.2.2](jetpack-6.2.2/) | JetPack 6.2.2 | 追加 DTB 并修改 `extlinux.conf` |
| [jetpack-7.2](jetpack-7.2/) | JetPack 7.2 | 追加 DTB 并修改 `extlinux.conf` |

请按设备实际刷入的 JetPack 版本选择对应目录，并按目录内 `README.md` 操作。
不同版本的 DTB 不可混用。

## 模块与 DTB 对应关系

| 模块 | JetPack 5.1.4 / 5.1.5 (super) | JetPack 6.2 | JetPack 6.2.2 | JetPack 7.2 |
| --- | --- | --- | --- | --- |
| Orin Nano 4G | `kernel_tegra234-p3767-0004-*.dtb` | `orin_nano_4g.dtb` | `orin_nano_4g.dtb` | `orin_nano_4g_jp7.dtb` |
| Orin Nano 8G | `kernel_tegra234-p3767-0003-*.dtb` | `orin_nano_8g.dtb` | `orin_nano_8g.dtb` | `orin_nano_8g_jp7.dtb` |
| Orin NX 8G | `kernel_tegra234-p3767-0001-*.dtb` | `orin_nx_8g.dtb` | `orin_nx_8g.dtb` | `orin_nx_8g_jp7.dtb` |
| Orin NX 16G | `kernel_tegra234-p3767-0000-*.dtb` | `orin_nx_16g.dtb` | `orin_nx_16g.dtb` | `orin_nx_16g_jp7.dtb` |

JetPack 7.2 表中四款 `*_jp7.dtb` 的硬件验证状态为待验证。原有 `orin_nano_8gb.dtb` 保留，
其 `compatible` 指向 p3767-0005；使用前请确认模块 SKU，详见该版本说明。

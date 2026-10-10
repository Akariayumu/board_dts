# Orin Nano/NX USB 配置 — JetPack 5.1.5 Super

适用于 JetPack 5.1.5 Super，已通过硬件验证。

## 文件选择

| 模块 | SKU | DTB |
| --- | --- | --- |
| Orin Nano 4G | p3767-0004 | `kernel_tegra234-p3767-0004-super-p3768-0000-a0.dtb` |
| Orin Nano 8G | p3767-0003 | `kernel_tegra234-p3767-0003-super-p3768-0000-a0.dtb` |
| Orin NX 8G | p3767-0001 | `kernel_tegra234-p3767-0001-super-p3768-0000-a0.dtb` |
| Orin NX 16G | p3767-0000 | `kernel_tegra234-p3767-0000-super-p3768-0000-a0.dtb` |

## 安装

设备刷入对应系统后，将所选 DTB 复制到设备。以下以 Orin Nano 8G 为例，其他模块替换文件名。

```bash
sudo cp -a /boot/dtb/kernel_tegra234-p3767-0003-super-p3768-0000-a0.dtb /boot/dtb/kernel_tegra234-p3767-0003-super-p3768-0000-a0.dtb.bak
sudo install -o root -g root -m 644 kernel_tegra234-p3767-0003-super-p3768-0000-a0.dtb /boot/dtb/kernel_tegra234-p3767-0003-super-p3768-0000-a0.dtb
sudo reboot
```

回退：恢复原 DTB 备份并重启。

Type-C 的启用方法见 [USB 配置说明](../README.md#启用-type-c)。

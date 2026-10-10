# Orin Nano/NX USB 配置 — JetPack 7.2

适用于 JetPack 7.2，已通过硬件验证。

## 文件选择

| 模块 | SKU | DTB |
| --- | --- | --- |
| Orin Nano 4G | p3767-0004 | `orin_nano_4g_jp7.dtb` |
| Orin Nano 8G | p3767-0003 | `orin_nano_8g_jp7.dtb` |
| Orin NX 8G | p3767-0001 | `orin_nx_8g_jp7.dtb` |
| Orin NX 16G | p3767-0000 | `orin_nx_16g_jp7.dtb` |

`orin_nano_8gb.dtb` 为保留文件，适用于 p3767-0005。

## 安装

设备刷入对应系统后，将所选 DTB 复制到设备。以下以 Orin Nano 8G 为例，其他模块替换文件名。

```bash
sudo cp -a /boot/extlinux/extlinux.conf /boot/extlinux/extlinux.conf.bak
sudo install -D -o root -g root -m 644 orin_nano_8g_jp7.dtb /boot/dtb/orin_nano_8g_jp7.dtb
sudo nano /boot/extlinux/extlinux.conf
```

在目标启动项 `LABEL primary` 下新增或更新以下一行，每个启动项只保留一行 `FDT`：

```text
FDT /boot/dtb/orin_nano_8g_jp7.dtb
```

保存后执行 `sudo reboot`。回退：恢复 `extlinux.conf` 备份并重启。

Type-C 的启用方法见 [USB 配置说明](../README.md#启用-type-c)。

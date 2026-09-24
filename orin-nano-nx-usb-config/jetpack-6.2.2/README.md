# Orin Nano/NX USB 配置说明 — JetPack 6.2.2

本说明适用于 Jetson Orin Nano / Orin NX，目标系统为 **JetPack 6.2.2（Jetson Linux R36.5.0）**。
设备树文件适配了对应模块，具体硬件验证状态请以顶层文档索引为准。

## 使用方法

1. 确认设备已刷入 JetPack 6.2.2，并根据模块型号选择本目录中的 DTB：

   | 模块 | 文件 |
   | --- | --- |
   | Jetson Orin Nano 4G | `orin_nano_4g.dtb` |
   | Jetson Orin Nano 8G | `orin_nano_8g.dtb` |
   | Jetson Orin NX 8G | `orin_nx_8g.dtb` |
   | Jetson Orin NX 16G | `orin_nx_16g.dtb` |

2. 将对应 DTB 复制到设备并在 `/boot/extlinux/extlinux.conf` 的启动项中指定它。以 Orin NX 16G 为例：

   ```bash
   sudo cp orin_nx_16g.dtb /boot/dtb/
   sudo sed -i 's#console=tty0#console=tty0\n      FDT /boot/dtb/orin_nx_16g.dtb#g' /boot/extlinux/extlinux.conf
   sudo reboot
   ```

   Nano 4G、Nano 8G、NX 8G 请将示例中的文件名换成对应 DTB 名称。

3. 修改前建议备份 `/boot/extlinux/extlinux.conf`。如需回退，删除新增的 `FDT` 行并重启。

> JetPack 6.2.2 的 DTB 不要用于其他 JetPack 版本。

## 启用 Type-C Device 模式

设备树加载后，Type-C 端口默认处于关闭状态。需要时可临时切换为 Device 模式：

```bash
sudo bash -c 'echo device > /sys/class/usb_role/usb2-0-role-switch/role'
```

连接 PC 后可使用 COM 口、虚拟网卡（设备默认 IP 为 `192.168.55.1`）和 NCM 网络共享。
此设置重启后失效；如需开机自动启用，可在 `/opt/nvidia/l4t-usb-device-mode/nv-l4t-usb-device-mode-start.sh` 的 `exit 0` 前加入：

```bash
echo device > /sys/class/usb_role/usb2-0-role-switch/role
```

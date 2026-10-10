# Jetson 设备树

适用模块：Orin Nano / Orin NX。适用载板：C1901、C1902、C1903、C2401。

[USB 配置说明](orin-nano-nx-usb-config/)：启用载板的 3 个 USB3.2 端口和 Type-C 端口。

按设备系统版本选择目录，DTB 不可跨版本使用。

| JetPack | 说明 | 验证状态 |
| --- | --- | --- |
| 5.1.4 | [安装说明](orin-nano-nx-usb-config/jetpack-5.1.4/) | 已验证 |
| 5.1.5 Super | [安装说明](orin-nano-nx-usb-config/jetpack-5.1.5-super/) | 已验证 |
| 6.2 | [安装说明](orin-nano-nx-usb-config/jetpack-6.2/) | 已验证 |
| 6.2.2 | [安装说明](orin-nano-nx-usb-config/jetpack-6.2.2/) | 待验证 |
| 7.2 | [安装说明](orin-nano-nx-usb-config/jetpack-7.2/) | 已验证 |

查看设备型号与系统版本：

```bash
cat /proc/device-tree/model; echo
head -1 /etc/nv_tegra_release
```

# Orin Nano/NX USB 配置

安装对应 DTB 后，载板的 3 个 USB3.2 端口可正常使用；Type-C 需启用 Device 模式。

## 安装设备树

按系统版本进入对应目录，选择模块文件并安装：

- [JetPack 5.1.4](jetpack-5.1.4/)
- [JetPack 5.1.5 Super](jetpack-5.1.5-super/)
- [JetPack 6.2](jetpack-6.2/)
- [JetPack 6.2.2](jetpack-6.2.2/)
- [JetPack 7.2](jetpack-7.2/)

## 启用 Type-C

临时启用 Device 模式，重启后失效：

```bash
sudo bash -c 'echo device > /sys/class/usb_role/usb2-0-role-switch/role'
```

连接 PC 后可使用串口和虚拟网卡，设备默认 IP 为 `192.168.55.1`。

开机自动启用：备份并编辑 `/opt/nvidia/l4t-usb-device-mode/nv-l4t-usb-device-mode-start.sh`，
在正常结束的 `exit 0` 前添加一次：

```bash
echo device > /sys/class/usb_role/usb2-0-role-switch/role
```

## 检查 USB

```bash
lsusb -t
cat /sys/class/usb_role/usb2-0-role-switch/role
sudo dmesg | grep -i -e xusb -e tegra-xudc
```

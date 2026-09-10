# 控元科技（广州）有限公司 — 设备树文档

控元科技底板配套的设备树（Device Tree）文件与操作说明。每个应用一个顶层目录，
目录内按系统版本分子目录，DTB 文件与对应说明放在一起。

适用平台：NVIDIA Jetson Orin Nano / Orin NX（模块 P3767 + 载板 P3768）。

---

## 快速开始

1. 确认设备型号与系统版本：

   ```bash
   cat /proc/device-tree/model; echo   # 模块型号
   head -1 /etc/nv_tegra_release       # L4T 版本
   ```

2. 在下表找到对应目录，把其中的 DTB 拷贝到设备上。

3. 按目录内 `README.md` 操作。

> ⚠️ **DTB 与 JetPack 版本严格绑定，不可跨版本混用**，用错版本会导致无法启动。

---

## 文档索引

| 应用说明 | 适用设备 | 说明 |
| --- | --- | --- |
| [Orin Nano/NX USB 配置](orin-nano-nx-usb-config/) | Jetson Orin Nano / Orin NX | 使底板 3 个 USB3.2 端口与 Type-C 端口正常工作 |

### Orin Nano/NX USB 配置 — 版本子目录

| 目录 | 适用系统版本 | L4T 版本 | 生效方式 | 状态 |
| --- | --- | --- | --- | --- |
| [jetpack-5.1.4](orin-nano-nx-usb-config/jetpack-5.1.4/) | JetPack 5.1.4 | R35.6.0 | 替换 `/boot/dtb` 中的同名 DTB | 已验证 |
| [jetpack-5.1.5-super](orin-nano-nx-usb-config/jetpack-5.1.5-super/) | JetPack 5.1.5 (super) | R35.6.1 | 替换 `/boot/dtb` 中的同名 DTB | 已验证 |
| [jetpack-6.2](orin-nano-nx-usb-config/jetpack-6.2/) | JetPack 6.2 | R36.4.3 | 追加 DTB 并修改 `extlinux.conf` | 已验证 |
| [jetpack-7.2](orin-nano-nx-usb-config/jetpack-7.2/) | JetPack 7.2 | R39.x | 追加 DTB 并修改 `extlinux.conf` | 已验证 |

---

## 模块与 DTB 对应关系

| 模块 | SKU | JetPack 5.1.4 | JetPack 5.1.5 (super) | JetPack 6.2 | JetPack 7.2 |
| --- | --- | --- | --- | --- | --- |
| Orin Nano 4G | p3767-0004 | `kernel_tegra234-p3767-0004-p3768-0000-a0.dtb` | `kernel_tegra234-p3767-0004-super-p3768-0000-a0.dtb` | `orin_nano_4g.dtb` | — |
| Orin Nano 8G | p3767-0003 | `kernel_tegra234-p3767-0003-p3768-0000-a0.dtb` | `kernel_tegra234-p3767-0003-super-p3768-0000-a0.dtb` | `orin_nano_8g.dtb` | `orin_nano_8gb.dtb` |
| Orin NX 8G | p3767-0001 | `kernel_tegra234-p3767-0001-p3768-0000-a0.dtb` | `kernel_tegra234-p3767-0001-super-p3768-0000-a0.dtb` | `orin_nx_8g.dtb` | — |
| Orin NX 16G | p3767-0000 | `kernel_tegra234-p3767-0000-p3768-0000-a0.dtb` | `kernel_tegra234-p3767-0000-super-p3768-0000-a0.dtb` | `orin_nx_16g.dtb` | — |


---

## 生效方式

### 方式一：替换同名 DTB（JetPack 5.x）

JetPack 5.x 启动时按 SKU 自动选择 `/boot/dtb/` 下的同名 DTB，覆盖同名文件即可，无需改启动配置。
建议先备份：

```bash
sudo cp /boot/dtb/<原文件名>.dtb /boot/dtb/<原文件名>.dtb.bak
```

### 方式二：追加 DTB + 指定 FDT（JetPack 6.x / 7.x）

JetPack 6.x 起由 `extlinux.conf` 指定设备树，需把 DTB 另存到 `/boot/dtb/`，
再在 `/boot/extlinux/extlinux.conf` 的启动项中增加一行 `FDT`：

```
LABEL primary
      MENU LABEL primary kernel
      LINUX /boot/Image
      INITRD /boot/initrd
      APPEND ${cbootargs} root=PARTUUID=... rw rootwait ... console=tty0 nv-auto-config
      FDT /boot/dtb/orin_nx_16g.dtb   # 对应 dtb 文件名
```

建议先备份 `extlinux.conf`；回退时删除该 `FDT` 行并重启。

---

## 常用命令

```bash
# 确认新设备树已生效
cat /proc/device-tree/model; echo

# 查看 USB 拓扑
lsusb -t

# 查看 / 切换 Type-C 端口角色（切换为临时生效，重启失效）
cat /sys/class/usb_role/usb2-0-role-switch/role
sudo bash -c 'echo device > /sys/class/usb_role/usb2-0-role-switch/role'

# 排查 USB 控制器
sudo dmesg | grep -i -e xusb -e tegra-xudc
```

Type-C 配置为 Device 模式后，连接 PC 主机可获得：

1. COM 口 —— 设备的终端命令行；
2. 虚拟网卡 —— 设备 IP 默认 `192.168.55.1`；
3. NCM（Network Control Model）—— 设备可经 PC 联网，仅支持 Linux、Mac。

开机自动生效的方法见各目录 `README.md` 的最后一步。

---

## 常见问题

**替换设备树后无法启动？**
JetPack 5.x 重新刷机；JetPack 6.x/7.x 用串口进入系统后删除 `extlinux.conf` 中的 `FDT` 行。

**USB3.2 正常，但 Type-C 无反应？**
Type-C 默认关闭，需切换为 Device 模式，见上文常用命令。

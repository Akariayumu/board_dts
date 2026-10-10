# Orin Nano/NX USB配置说明

本说明适用于 Jetson Orin Nano / Orin NX，目标系统为 **JetPack 7.2**。
原有 `orin_nano_8gb.dtb` 已有测试记录；新增的四款 `*_jp7.dtb` 硬件验证状态为待验证。

在刷入官方系统后，通过增加设备树，可使底板中的USB3.2端口和Type-C端口正常工作。

## 使用方法

1. 短接底板中的**FC REC**引脚与**GND**引脚并重新上电，使设备进入恢复模式。

2. 通过Type-C端口连接至Ubuntu主机，使用NVIDIA SDK Manager刷入**JetPack 7.2**系统。

3. 设备正常开机后，打开终端命令行，增加**DTB设备树**文件，并修改**extlinux.conf**文件。

   根据模块 SKU 选择对应文件：

   | 模块 | SKU | 文件 | 验证状态 |
   | --- | --- | --- | --- |
   | Jetson Orin Nano 4G | p3767-0004 | `orin_nano_4g_jp7.dtb` | 待验证 |
   | Jetson Orin Nano 8G | p3767-0003 | `orin_nano_8g_jp7.dtb` | 待验证 |
   | Jetson Orin NX 8G | p3767-0001 | `orin_nx_8g_jp7.dtb` | 待验证 |
   | Jetson Orin NX 16G | p3767-0000 | `orin_nx_16g_jp7.dtb` | 待验证 |

   原有 `orin_nano_8gb.dtb` 保留，其 `compatible` 指向 p3767-0005，与新增的 Nano 8G 文件不同。
   请确认模块 SKU，不要仅凭文件名互换使用。

   以下以 **Jetson Orin Nano 8G（p3767-0003）** 为例：

     ```bash
     sudo cp -a /boot/extlinux/extlinux.conf /boot/extlinux/extlinux.conf.bak
     sudo install -D -o root -g root -m 644 orin_nano_8g_jp7.dtb /boot/dtb/orin_nano_8g_jp7.dtb
     sudo nano /boot/extlinux/extlinux.conf
     ```

   在目标启动项 `LABEL primary` 下新增或更新 `FDT` 行为 `FDT /boot/dtb/orin_nano_8g_jp7.dtb`，
   同一启动项只保留一行 `FDT`。其他模块请换成表中对应文件名。
   确认保存后执行 `sudo reboot`。回退时恢复备份的 `extlinux.conf` 并重启。

   > extlinux.conf文件增加FDT字段后参考以下格式：
   >
   > LABEL primary
   >       MENU LABEL primary kernel
   >       LINUX /boot/Image
   >       INITRD /boot/initrd
   >       APPEND ${cbootargs} root=PARTUUID=... rw rootwait rootfstype=ext4 mminit_loglevel=4 console=ttyTCU0,115200 firmware_class.path=/etc/firmware fbcon=map:0 nospectre_bhb video=efifb:off console=tty0 nv-auto-config
   >       FDT /boot/dtb/orin_nano_8g_jp7.dtb

   > 目标配置为启用 3 个 USB3.2 端口；新增 DTB 的实际功能需在对应硬件上验证。Type-C 端口需按以下步骤启用。

4. 配置**Type-C端口**为**Device**模式

   ```bash
   sudo bash -c 'echo device > /sys/class/usb_role/usb2-0-role-switch/role'
   ```

   > 使用Type-C端口连接到PC主机时，有以下功能
   >
   > 1. COM口，设备的终端命令行
   > 2. 虚拟网卡，设备ip地址默认为：192.168.55.1
   > 3. NCM (Network Control Model)，设备可通过PC主机联网，仅支持Linux、Mac

   > *注意：此命令为临时启用，重启会失效

5. 开机默认配置**Type-C端口**为**Device**模式

   ```bash
   sudo cp -a /opt/nvidia/l4t-usb-device-mode/nv-l4t-usb-device-mode-start.sh /opt/nvidia/l4t-usb-device-mode/nv-l4t-usb-device-mode-start.sh.bak
   sudo nano /opt/nvidia/l4t-usb-device-mode/nv-l4t-usb-device-mode-start.sh
   ```

   在脚本正常结束的 `exit 0` 前加入以下一行；如果已存在，不要重复添加：

   ```bash
   echo device > /sys/class/usb_role/usb2-0-role-switch/role
   ```

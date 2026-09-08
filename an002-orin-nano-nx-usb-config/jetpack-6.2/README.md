# AN002 Orin Nano/NX USB配置说明 v1.2

本说明仅适用于设备Jetson Orin Nano/NX，在系统版本：**JetPack 6.2**中测试通过。

在刷入官方系统后，通过替换设备树，可使底板中的USB3.2端口和Type-C端口正常工作。

## 使用方法

1. 短接底板中的**FC REC**引脚与**GND**引脚并重新上电，使设备进入恢复模式。

2. 通过Type-C端口连接至Ubuntu主机，使用NVIDIA SDK Manager刷入**JetPack 6.2**系统。

3. 设备正常开机后，打开终端命令行，增加**DTB设备树**文件，并修改**extlinux.conf**文件。

   - **Jetson Orin Nano 4G** 请使用以下代码

     ```bash
     sudo cp orin_nano_4g.dtb /boot/dtb
     sudo sed -i 's#console=tty0#console=tty0\n      FDT /boot/dtb/orin_nano_4g.dtb#g' /boot/extlinux/extlinux.conf
     sudo reboot
     ```
     
   - **Jetson Orin Nano 8G** 请使用以下代码

     ```bash
     sudo cp orin_nano_8g.dtb /boot/dtb
     sudo sed -i 's#console=tty0#console=tty0\n      FDT /boot/dtb/orin_nano_8g.dtb#g' /boot/extlinux/extlinux.conf
     sudo reboot
     ```
     
   - **Jetson Orin NX 8G** 请使用以下代码

     ```bash
     sudo cp orin_nx_8g.dtb /boot/dtb
     sudo sed -i 's#console=tty0#console=tty0\n      FDT /boot/dtb/orin_nx_8g.dtb#g' /boot/extlinux/extlinux.conf
     sudo reboot
     ```
     
   - **Jetson Orin NX 16G** 请使用以下代码

     ```bash
     sudo cp orin_nx_16g.dtb /boot/dtb
     sudo sed -i 's#console=tty0#console=tty0\n      FDT /boot/dtb/orin_nx_16g.dtb#g' /boot/extlinux/extlinux.conf
     sudo reboot
     ```

   > extlinux.conf文件增加FDT字段后参考以下格式：
   >
   > LABEL primary
   >       MENU LABEL primary kernel
   >       LINUX /boot/Image
   >       INITRD /boot/initrd
   >       APPEND ${cbootargs} root=PARTUUID=a139fb02-4340-44f6-bb73-901a25b1b76b rw rootwait rootfstype=ext4 mminit_loglevel=4 console=ttyTCU0,115200 firmware_class.path=/etc/firmware fbcon=map:0 nospectre_bhb video=efifb:off console=tty0 nv-auto-config
   >       FDT /boot/dtb/orin_nx_16g.dtb  #对应dtb文件名

   > 修改设备树后，3个USB3.2端口可正常工作，Type-C端口系统默认处于关闭状态

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
   sudo sed -i 's#exit 0#echo device > /sys/class/usb_role/usb2-0-role-switch/role\nexit 0#g' /opt/nvidia/l4t-usb-device-mode/nv-l4t-usb-device-mode-start.sh
   ```


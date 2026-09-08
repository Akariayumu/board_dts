# Orin Nano/NX USB配置说明

本说明仅适用于设备Jetson Orin Nano/NX，在系统版本：**JetPack 5.1.5 (super)**中测试通过。

在刷入官方系统后，通过替换设备树，可使底板中的USB3.2端口和Type-C端口正常工作。

## 使用方法

1. 短接底板中的**FC REC引脚**与**GND引脚**并重新上电，使设备进入恢复模式。

2. 通过**Type-C端口**连接至**Ubuntu主机**，使用**NVIDIA SDK Manager**刷入**JetPack 5.1.5 (super)**系统。

3. 设备正常开机后，打开终端命令行，替换**DTB设备树**文件并重启设备。

   - **Jetson Orin Nano 4G** 请使用以下代码

     ```bash
     sudo cp kernel_tegra234-p3767-0004-super-p3768-0000-a0.dtb /boot/dtb
     sudo reboot
     ```
     
   - **Jetson Orin Nano 8G** 请使用以下代码

     ```bash
     sudo cp kernel_tegra234-p3767-0003-super-p3768-0000-a0.dtb /boot/dtb
     sudo reboot
     ```
     
   - **Jetson Orin NX 8G** 请使用以下代码
   
     ```bash
     sudo cp kernel_tegra234-p3767-0001-super-p3768-0000-a0.dtb /boot/dtb
     sudo reboot
     ```
     
   - **Jetson Orin NX 16G** 请使用以下代码
   
     ```bash
     sudo cp kernel_tegra234-p3767-0000-super-p3768-0000-a0.dtb /boot/dtb
     sudo reboot
     ```
   
   > 替换设备树后，3个USB3.2端口可正常工作，Type-C端口系统默认处于关闭状态
   
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


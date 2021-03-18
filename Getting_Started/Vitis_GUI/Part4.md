<table class="sphinxhide">
 <tr>
   <td align="center"><img src="https://www.xilinx.com/content/dam/xilinx/imgs/press/media-kits/corporate/xilinx-logo.png" width="30%"/><h1>2020.2 Vitis™ Getting Started Tutorial</h1>
   <a href="https://github.com/Xilinx/Vitis-Tutorials/tree/2020.1">See 2020.1 Tutorials</a>
   </td>
 </tr>
 <tr>
 <td>
 </td>
 </tr>
</table>

# Vitis Flow 101 – Part 4 : Build and Run the Example

 In this fourth part of the Introduction to Vitis tutorial, you will compile and run the vector-add example using each of three build targets supported in the Vitis flow (software emulation, hardware emulation and hardware).


##### IMPORTANT: This tutorial requires Vitis 2020.2 or later to run.

### Setting up the environment

* To configure the environment to run Vitis, source the following scripts:

```bash
source <VITIS_install_path>/settings64.sh
source <XRT_install_path>/setup.sh
```

Make sure the VCK190 platform file and sysroots is installed properly. The instructions about how to install an embedded platform is listed in [Part 2 Section](Part2.md#id_001).

*NOTE: The Versal common image file can be downloaded from the [Vitis Embedded Platforms](https://www.xilinx.com/support/download/index.html/content/xilinx/en/downloadNav/embedded-platforms.html) page, and contains the Sysroot, Rootfs, and boot Image for Xilinx Versal boards.*


### Create Vector Addtion Application Project

* Launch Vitis GUI

```bash
vitis &
```
It will pop up a window asking for workspace selection. Choose an appropriate folder for this tutorial and click **Launch**.

* Create vector addition application project

1. In the Welcome window, click **Create Application Project** to create a new application project. You may also do it by clicking the **File** menu and select **New** -> **Application project**.

2. The first page of the wizard is a brief introduction, click **Next** and go to the next page.

![img](./images/part4_project_creation_1.png)

3. In the platform selection page, the VCK190 platform should appear in the list if the environment was set up correctly just now. If it is not shown, click the ```+Add``` button on the upper right window to add it into the project workspace. After selecting the VCK190 platform, click **Next**.

![img](./images/part4_project_creation_2.png)

4. In the application project page, set application name as ```vadd``` and target processor as **psv_cortexa72_SMP**. By default, it will create a new system project tied to the application project. Keep the default name and click **Next**.

![img](./images/part4_project_creation_3.png)

5. Input Sysroot path, RootFS path and Kernel Image. Those paths are pointing to the folder and files of common linux package that you downloaded in previous steps.
For the usage of these files, you will find more detailed in the Vitis Doc: https://www.xilinx.com/html_docs/xilinx2020_2/vitis_doc/vitis_embedded_installation.html?hl=sdk.sh#rvu1542160683426

 - Sysroot path (xilinx-versal-common-v2020.2/sysroots/aarch64-xilinx-linux)

> NOTE: The sysroots folder is generated after you run the xilinx-versal-common-v2020.2/sdk.sh script.

 - Root FS (xilinx-versal-common-v2020.2/rootfs.ext4)
 - Kernel Image (xilinx-versal-common-v2020.2/Image)

Click **Next**.

![img](./images/part4_project_creation_4.png)

6. In the templates page, select **Vector Addition** template. Click **Finish** to complete the application project creation stage.

![img](./images/part4_project_creation_5.png)


### Targeting Software Emulation

1. Now we are going to run the project in software emulation flow. Firstly, double click the 'vadd_system.sprj' file from the Explorer window to open up the system project settings page and make sure that **Software Emulation** is selected as active build.

![img](./images/part4_swemu_target.png)

2. Right click the **Emulation-SW** run in Assistant window and click **Build**. This will build all the necessary underlying projects.

![img](./images/part4_swemu_build.png)

3. After building process completes, right click the vadd system project and select **Run As** -> **Launch SW Emulator** from the pop up window.

![img](./images/part4_swemu_run.png)

4. If Emulator was not started before, there will be a window notifying about launching emulator. Select **Start Emulator and Run**.

![img](./images/part4_swemu_emulator.png)

5. Pay attention to the Emulation Console window in the right bottom which will display the ARM processor booting messages. After ARM system is up and running, host application will start automatically and you should see ```Test Passed``` message printed out in the main console window if everything goes well.

![img](./images/part4_swemu_passed.png)


### Targeting Hardware Emulation

1. Now we are going to run the project in hardware emulation flow. Firstly, open up the system project page and make sure that **Hardware Emulation** is selected as target.

![img](./images/part4_hwemu_target.png)

2. Right click the **Emulation-HW** run in Assistant window and click **Build**.

![img](./images/part4_hwemu_build.png)

3. After building process completes, right click the vadd system project and select **Run As** -> **Launch HW Emulator** from the pop up menu.

![img](./images/part4_hwemu_run.png)

4. If Emulator was not started before, there will be a window notifying about launching emulator. Click **Start Emulator and Run**. Note that you can also check the **Launch Emulator in GUI mode to display waveforms** and this will open up Vivado simulator to view the signals waveform.  

![img](./images/part4_hwemu_emulator.png)

5. Pay attention to the Emulation Console window in the bottom right which will display the ARM processor booting messages. After ARM system is up, host application will start automatically and you should see ```Test Passed``` message printed out in the main console window.

### Targeting Hardware

There are two methods to run the application on VCK190 board.

#### Method 1 - Run from Vitis GUI

This is a relatively direct and easy way and it is suitable for the scenario that your board is directly connected to the server that the design is build and run. The way is quite similar to launching emulation runs.

1. Firstly, open up the system project page and make sure that **Hardware** is selected as target.

![img](./images/part4_hw_target.png)

2. Right click the **Hardware** run in Assistant window and click **Build**.

![img](./images/part4_hw_build.png)

3. Before running the design on hardware, make sure your board is connected and powered on. Then right click the vadd system project and select **Run As** -> **Launch Hardware** from the pop up menu. You should be able to see the **Test Passed** message after application finishes running.

![img](./images/part4_hw_run.png)

#### Method 2 - Run from SD card

1. Copy vadd_system/Hardware/package/sd_card.img to local if you build the project on a remote server or virtual machine.

2. Program sd_card.img to SD card. Refer to [AR#73711](https://www.xilinx.com/support/answers/73711.html) for detailed steps.
Note: The programmed SD card has two partitions. FAT32 partition with boot components; EXT4 partition with Linux root file system. Windows system by default cannot see the contents of EXT4 partition.
Note: Please eject the SD card properly from the system after programming it.

3. Insert the SD card and boot the VCK190 board with SD boot mode (SW1[4:1] = "1110": OFF, OFF, OFF, ON) and power on.
Note: Refer to [VCK190 Evaluation Board User Guide](https://www.xilinx.com/support/documentation/boards_and_kits/vck190/ug1366-vck190-eval-bd.pdf) for details about boot mode.

4. Connect to UART console.

5. Launch the test application from UART console.

6. Run application using below commands:

```bash
cd /mnt/sd-mmcblk1p1
./vadd binary_container_1.xclbin
```

7. And you should be able to see the **Test Passed** information in UART console.

## Next Step

Now that you ran your first example, proceed to [**Part 5**](./Part5.md) of this tutorial to learn how to visualize and profile your application with Vitis Analyzer.

<p align="center"><sup>Copyright&copy; 2020 Xilinx</sup></p>

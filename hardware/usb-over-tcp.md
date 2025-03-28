# USB over TCP

Since I will be attending a seminar in the coming days and cannot bring my Raspberry Pi 4, which runs all my Docker containers (including Home Assistant), I needed to find a solution to connect my Zigbee Sonoff USB 3.0 Plus Dongle to my notebook via USB and forward it to the Raspberry Pi 4 over TCP. The goal is for the Raspberry Pi 4 to recognize it as a local USB device.

On my Windows 11 notebook, I installed the following tool (`msi` file; via Admin PowerShell):
[usbipd-win](https://github.com/dorssel/usbipd-win/releases)

Afterward, you can list all connected USB devices using the command `usbipd list`:

```
Connected:
BUSID  VID:PID    DEVICE                                                        STATE
1-3    046d:0893  Logitech StreamCam, Logitech StreamCam WinUSB, USB Input...  Not shared
2-1    10c4:ea60  Silicon Labs CP210x USB to UART Bridge (COM10)                Not shared
2-2    04f2:b6bf  HP HD Camera, HP IR Camera                                    Not shared
2-7    06cb:00f0  Synaptics FS7605 Touch Fingerprint Sensor with PurePrint(TM)  Not shared
2-10   8087:0026  Intel(R) Wireless Bluetooth(R)                                Not shared
3-1    8087:0af1  HP GNSS Sensor                                                Not shared
3-2    8087:0aca  Serial USB Device (COM3), Serial USB Device (COM4), M...      Not shared
3-3    8087:0ac9  Intel(R) XMM(TM) 7360 LTE-A                                   Not shared
4-3    0bda:8153  Realtek USB GbE Family Controller #4                          Not shared
```

From this output, you can see that the dongle (`Silicon Labs CP210x USB to UART Bridge`) is accessible on the BUS ID `2-1`.

Using the command `usbipd bind --busid 2-1`, you can enable the USB device for sharing over TCP:

```
Connected:
BUSID  VID:PID    DEVICE                                                        STATE
1-3    046d:0893  Logitech StreamCam, Logitech StreamCam WinUSB, USB Input...  Not shared
2-1    10c4:ea60  Silicon Labs CP210x USB to UART Bridge (COM10)                Shared
2-2    04f2:b6bf  HP HD Camera, HP IR Camera                                    Not shared
2-7    06cb:00f0  Synaptics FS7605 Touch Fingerprint Sensor with PurePrint(TM)  Not shared
2-10   8087:0026  Intel(R) Wireless Bluetooth(R)                                Not shared
3-1    8087:0af1  HP GNSS Sensor                                                Not shared
3-2    8087:0aca  Serial USB Device (COM3), Serial USB Device (COM4), M...      Not shared
3-3    8087:0ac9  Intel(R) XMM(TM) 7360 LTE-A                                   Not shared
4-3    0bda:8153  Realtek USB GbE Family Controller #4                          Not shared
```

The dongle (`Silicon Labs CP210x USB to UART Bridge`) is now in the `Shared` state.
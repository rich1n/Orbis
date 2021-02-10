# Instalando el Artisan

### Paso previo: Chequear que el hardware sea compatible
Verifica que tu sistema operativo cumpla los requisitos de instalación.
Verifica que la tostadora y sus dispositivos que vayas a utilizar con el Artisan este entre las máquinarias y dispositivos compatibles.

Note que las máquinas soportadas son de aquellos fabricantes que decidieron apoyar el desarrollo de Artisan, pero si tu fabricante no está en la lista de los dispositivos, de todas formas va a funcionar pero de forma manual.

### Paso 1: Descargar el Artisan para tu sistema operativo.

Puedes buscar y descargar el paquete más actualizado de instalación para tu sistema. Los archivos se muestran con x.x.x. por el número de versión.

    macOS: artisan-mac-x.x.x.dmg
    Windows: artisan-win-x.x.x.zip
    Linux Redhat/CentOS: artisan-linux-x.x.x.rpm
    Linux Debian/Ubuntu: artisan-linux-x.x.x.deb
    Raspberry Pi: artisan-linux-x.x.x_raspbian-XX.deb

### Paso 2: Instala el Artisan en tu sistema operativo
*Windows*: descomprime el archivo zip e inicia el instalador.
*MacOS*: Monta el archivo de instalación .dmg con un doble click y arrastra el Artisan.app a tu carpeta de aplicaciones. Nota que los Phidgets tiene nuevos drives para el MacOS 10.15 Catalina, que deben ser instalados y autorizados.
*Linux*: Instala el archivo descargado dándole doble click.
*Redhat/CentOS*: en la términal: # sudo rpm -i artisan-linux-x.x.x.rpm
*Debian/Ubuntu/Raspian* en la terminal: # sudo dpkg -i artisan-linux-x.x.x.deb

### Paso 3: Instala el driver serial (en caso de que sea necesario)



To operate some devices like Phidget modules or certain meters you need to install corresponding drivers. See the corresponding Section under Supported Devices for further details.
Linux

In case you run into permission problems such that Artisan is not allowed to read or write to the selected /dev/USB device, you might need to add your account (username) to the dialout group via

# sudo adduser <username> dialout

After this command you might need to logout and login again. Try

# id

that your account was successful added to the dialout group.

Note that for apps running by non-root users access to Phidgets or Yoctopuce devices require the installation of corresponding udev rules. Check the Phidgets and Yoctopuce platform installation notes. Those rules are installed automatically by Artisan, but require the users to be in the sudo group for security considerations.
Step 4: Configure Artisan for your setup

You need to tell Artisan which machine or devices you attached. Startup Artisan and select your roasting machine (menu Config >> Machines) or open the Device Assignment dialog (menu Config >> Device) and configure your device here.

The serial settings for meters are already configured by Artisan automatically when you select a device for the first time (or when you change devices). The only setting that is not configured is the serial port number being used. To find out your serial port, connect your device/meter and select the correct comm port from the ports popup menu. You can test to see if you have the correct comm port by clicking the green button “ON” on the top of the main window. If you see the two temperatures from the meter come up on the LCDs, you have completed all the configuration steps. You can now start using artisan.
Further Specific Notes
Consistent USB names on Debian (by Rob Gardner)

On some Debian-based systems the USB device names are different, once /dev/tty/USB0 another time /dev/tty/USB1, on each connect of the same device. The solution is to add a udev rule that creates a symbolic link with a constant name to point to the actual device. In my situation, I added a file called

  /etc/udev/rules.d/datalogger.rules

that contains this

  ATTRS{idVendor}=="0403", ATTRS{idProduct}=="6001", GROUP="plugdev", SYMLINK="tmd-56"

This tells udev to create a symbolic link “tmd-56” to point to the real device file. You can use the ‘lsusb’ command to easily find the vendor and product ID for your device. Then I just simply tell Artisan that my serial port is /dev/tmd-56 and it always finds it, no matter if the probe is plugged in before or after starting Artisan. You may need to customize the plugdev group also for your distro and add yourself to the plugdev group. On some systems the “dialout” group is used for serial devices (see above).
Omega HH806AU / Omega HH802U / Amprobe TMD-56

The Omega HH806AU, HH802U as well as the Amprobe TMD-56 device are supported by Artisan only if they are communicating on channel 0.

    How to check the channel number

When the meter is off press [C/F] key and [POWER] for 5
seconds, LCD's main display will show channel number,
the second display will show ID number.

    How RESET the meter to channel zero

To SET CH/ID to 00,00, by pressing the "T1-T2" key
(labeled "Hi/Lo Limits" on the HH802U) and
the power key for more than 6 seconds with the meter
powered down. The meter will set channel and ID
to 00,00 status. The second display will show 00,
which means that the channel and ID has been set to
00.

Fuji PXR/PXG 4 & 5 PID

Artisan uses one decimal point. You have to manually configure your pid so that it outputs one digit after the decimal point. See page 42 in the instruction manual.
Aillio Bullet R1

On Linux, Artisan needs read/write access to the USB device. Corresponding udev rules are automatically installed along Artisan in /etc/udev/rules.d. However, those require the users to be in the sudo group for security considerations.

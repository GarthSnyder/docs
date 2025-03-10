# 😭 Troubleshooting

This list is NOT exhaustive, if you need support don't be a stranger and please come and talk to us on [Discord](https://discord.gg/yzazQMEGS2), we are more than happy to help.\
\
If you do think that something is missing, please let us know and I will add it as soon as I can.

<details>

<summary>Option 'pin' in section 'probe' must be specifie</summary>

[<img src="https://github.com/Cartographer3D/docs/raw/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image.png" alt="" data-size="original">](https://github.com/Cartographer3D/docs/blob/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image.png)



This can occur either after a Klipper update when the sym link occasionally gets broken, or if you have not installed the Klipper component during the install, either way the fix is the same, just re-run the installer below.

```
cd ~
git clone https://github.com/Cartographer3D/cartographer-klipper.git
chmod +x cartographer-klipper/install.sh
./cartographer-klipper/install.sh
```

</details>

<details>

<summary>Unknown pin chip name 'cartographer' OR Unknown pin chip name 'scanner'</summary>

[![](https://github.com/Cartographer3D/docs/raw/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(1\)%20\(1\)%20\(1\)%20\(1\).png)](https://github.com/Cartographer3D/docs/blob/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(1\)%20\(1\)%20\(1\)%20\(1\).png)

The following two issues are usually why you recieve the above error.

1. You are referencing cartographer/scanner before you have declared it in your config file. It is advisable to add the cartographer/scanner section just below where you declare your MCU's.
2. You have referenced cartographer with a capitilisation `[Cartographer]` vs `[cartographer]` or `cs_pin: Cartographer:PA3` vs `cs_pin: cartographer:PA3`

</details>

<details>

<summary>Unknown pin chip name 'probe'</summary>

[![](https://github.com/Cartographer3D/docs/raw/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(2\)%20\(1\)%20\(1\)%20\(1\).png)](https://github.com/Cartographer3D/docs/blob/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(2\)%20\(1\)%20\(1\)%20\(1\).png)

This error usually happens when you have your `[cartographer]` section below your `[stepper_z]` section, move the `[cartographer]` section near your MCU's.

</details>

<details>

<summary>mcu 'cartographer': Unknown command: endstop_home</summary>

In `[stepper_z]` you have set `endstop_pin: cartographer:z_virtual_endstop` this should be `endstop_pin: probe:z_virtual_endstop`

</details>

<details>

<summary>Error importing numpy error</summary>

[![](https://github.com/Cartographer3D/docs/raw/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(3\)%20\(1\)%20\(1\)%20\(1\).png)](https://github.com/Cartographer3D/docs/blob/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(3\)%20\(1\)%20\(1\)%20\(1\).png)

</details>

<details>

<summary>"Error importing numpy: you should not try to import numpy from" error</summary>

[![](https://github.com/Cartographer3D/docs/raw/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(15\).png)](https://github.com/Cartographer3D/docs/blob/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(15\).png)

The error in a slightly different view.

This error usually occours when you have installed Cartographer as root rather than as a normal user (not always, but that is the only way I have been able to replicate it.).

In order to fix this, please update Klipper to the latest version either via your WebUI, or via KIAUH, once updated please access your Pi (or similar) via ssh

Run the the following command:

```
sudo apt install libopenblas-base
```

if the above doesn't work, please try the following

```
sudo apt install libopenblas-dev
```

</details>

<details>

<summary>Endstop showing Triggered</summary>

[![](https://github.com/Cartographer3D/docs/raw/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(12\).png)](https://github.com/Cartographer3D/docs/blob/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(12\).png)

Endstop Z showing constantly triggered

Please ensure that you have calibrated your probe

</details>

<details>

<summary>Option mesh_main_direction is not a valid section in cartographer</summary>

[![](https://github.com/Cartographer3D/docs/raw/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(13\).png)](https://github.com/Cartographer3D/docs/blob/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(13\).png)

You do not have a valid \[bed\_mesh] section in your printer.cfg, please check out [this site](https://www.klipper3d.org/Bed\_Mesh.html) for how to add one.

</details>

<details>

<summary>Unable to parse option mesh_runs in section cartographer</summary>

[![](https://github.com/Cartographer3D/docs/raw/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(14\).png)](https://github.com/Cartographer3D/docs/blob/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(14\).png)

You do not have a valid \[bed\_mesh] section in your printer.cfg, please check out [this site](https://www.klipper3d.org/Bed\_Mesh.html) for how to add one.

</details>

<details>

<summary>Unable to find your Cartographer USB probe via <code>ls /dev/serial/by-id/</code></summary>



So there are a few reasons you might not be able to find your Cartographer probe.

1.  A bug has been introduced in Debian Bullseye (which includes MainsailOS), which prevents the symlinks in /dev/serial/by-id/ from being created. If your printer can't connect to the MCU anymore after a system update, you can check if it is caused by that bug by checking the installed version of udev with `apt show udev`

    \
    If your version is `247.3-7+deb11u2` or `247.3-7+rpi1+deb11u2` you have the broken package installed and should use one of the fixes below. Take special care about the last number ("u2").\
    \
    As of May 20 2023, this bug has spread to PiOS based systems as well.\
    \
    **To Fix -** Replace the corrupted udev file with one from upstream systemd:

```
# backup the existing rules file (just in case)

sudo cp /usr/lib/udev/rules.d/60-serial.rules /usr/lib/udev/rules.d/60-serial.old

# download the rule from the systemd main repo.
sudo wget -O /usr/lib/udev/rules.d/60-serial.rules https://raw.githubusercontent.com/systemd/systemd/main/rules.d/60-serial.rules

# reboot
sudo reboot
```

2. If you have a v3 probe, it might be flashed for CAN Mode, please re-flash it for USB Operation, this includes the Katapult bootloader, as this is used to change the operation of the probe itself.\
   \
   For more information on how to do that, [click here](https://github.com/Cartographer3D/docs/blob/8279e4591b99ae0647cad467be2561b1ce6df0a5/cartographer-probe/firmware-update)
3. Check your USB Cable - If you have bought a flat pack, and your probe is a assembled for RIGHT ANGLE operation, you might have to re-assemble re-pin your USB cable in reverse.

</details>

<details>

<summary>Klipper environement running on Python 2</summary>

You will need to update your Klipper env from Python 2 to Python 3. the following guide is taken from [https://klipper.discourse.group/t/process-for-migrating-to-python3/5292/3](https://klipper.discourse.group/t/process-for-migrating-to-python3/5292/3)\
\
There is another outstanding guide by EricZimmerman that I would highly recommend checking out\
[https://github.com/EricZimmerman/VoronTools/blob/main/OSUpgrade.md](https://github.com/EricZimmerman/VoronTools/blob/main/OSUpgrade.md)\\

\{% hint style="info" %\} Note - a minimum version of Pyhon 3.9 is required. \{% endhint %\}

```
sudo service klipper stop

# remove current klippy-env environment
cd ~
rm -rf klippy-env

# create new venv
virtualenv -p python3 klippy-env

#install new Klipper venv
cd ~/klippy-env
bin/pip install -r ../klipper/scripts/klippy-requirements.txt
```

Restart your pritner, and you should now have a wonderful Klipper environement running in Python3.

</details>

<details>

<summary>Klipper is showing my install as dirty?</summary>

[![](https://github.com/Cartographer3D/docs/raw/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(3\).png)](https://github.com/Cartographer3D/docs/blob/8279e4591b99ae0647cad467be2561b1ce6df0a5/.gitbook/assets/image%20\(3\).png)

Klipper will show any build as klipper as "dirty" if there are any untracked Python files within the folders, this is the same with all other python based plugins such as ShakenTune, LEDEffects and so on.\
\
Unfortuantly, there is no way around this at the moment.\
\
[https://github.com/Klipper3d/klipper/actions/runs/4840565448](https://github.com/Klipper3d/klipper/actions/runs/4840565448)

</details>

<details>

<summary>Missing Clusters or Sharp Drop in your Bed Mesh</summary>

If you get a sharp drop in your bed mesh, or missing clusters error after a scan, ensure that your probe is covering the entire bed. You may need to adjust your bed mesh accordingly.

</details>

<details>

<summary>Repeated No trigger on z after full movement</summary>

![](<../.gitbook/assets/image (2) (1) (1).png>)

If you are getting the error `No trigger on z after full movement`, and running a lower powered SBC such as a Raspberry Pi 2, or a BTT CB1, You are able to lock a single CPU core to your Klipper instance. \
\
Esoterical has an outstanding guide on how to do so below. \
\
[https://canbus.esoterical.online/troubleshooting/timeout\_during\_homing\_probing.html#experimental](https://canbus.esoterical.online/troubleshooting/timeout\_during\_homing\_probing.html#experimental)

</details>

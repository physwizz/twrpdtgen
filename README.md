# TWRP device tree generator

[![PyPi version](https://img.shields.io/pypi/v/twrpdtgen)](https://pypi.org/project/twrpdtgen/)
[![PyPi version status](https://img.shields.io/pypi/status/twrpdtgen)](https://pypi.org/project/twrpdtgen/)
[![Codacy Badge](https://app.codacy.com/project/badge/Grade/ae7d7a75522b4d079c497ff6d9e052d1)](https://www.codacy.com/gh/twrpdtgen/twrpdtgen/dashboard?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=twrpdtgen/twrpdtgen&amp;utm_campaign=Badge_Grade)

Create a [TWRP](https://twrp.me/)-compatible device tree only from an Android recovery image (or a boot image if the device uses non-dynamic partitions A/B) of your device's stock ROM
It has been confirmed that this script supports images built starting from Android 4.4 up to Android 12

## Installation

```
pip3 install twrpdtgen
```
The module is supported on Python 3.9 and above.

Linux only: Be sure to have cpio installed in your system (Install cpio using `sudo apt install cpio` or `sudo pacman -S cpio` based on what package manager you're using)

## How to use

```
$ python3 -m twrpdtgen -h
TWRP device tree generator
Version 2.1.0

usage: python3 -m twrpdtgen [-h] [-o OUTPUT] [--git] [-d] image

positional arguments:
  image                 path to an image (recovery image or boot image if the
                        device is A/B)

optional arguments:
  -h, --help            show this help message and exit
  -o OUTPUT, --output OUTPUT
                        custom output folder
  --git                 create a git repo after the generation
  -d, --debug           enable debugging features
```

When an image is provided, if everything goes well, there will be a device tree at `output/manufacturer/codename`

You can also use the module in a script, with the following code:

```python
from twrpdtgen.devicetree import DeviceTree

# Get image info
devicetree = DeviceTree(image_path)

# Dump device tree to folder
devicetree.dump_to_folder(output_path)
```
TWRP for a12s
=============



git config --global user.email "your email"
git config --global user.name "your name"



Basic Device Tree
==================

mkdir TWRP
cd TWRP
git init
git clone https://github.com/twrpdtgen/twrpdtgen
pip3 install twrpdtgen
sudo apt install cpio
copy stock recovery.img to ~/TWRP/twrpdtgen

cd twrpdtgen
python3 -m twrpdtgen recovery.img

rename ~/TWRP/twrpdtgen/output/samsung/a12s/omni_a12s.mk to twrp_a12s.mk

delete vendorsetup.sh
Make a copy of recovery.fstab and rename to twrp.flags
Move both files recovery.fstab and twrp.flags to recovery/root/system/etc
(create these folders)

open twrp_a12s.mk and 
1. delete the 3 lines below # Inherit from those products, Most specific first.
replace those 3 lines with this
$(call inherit-product, $(SRC_TARGET_DIR)/product/aosp_base.mk)

2. delete $(call inherit-product, vendor/omni/config/gsm.mk)
3. change omni to twrp (2 places)

open AndroidProducts.mk
change omni to twrp (4 spots)

Optional: add extra items to BoardConfig.mk


cd ~
sudo apt install git aria2 -y
git clone https://gitlab.com/OrangeFox/misc/scripts
cd scripts
sudo bash setup/android_build_env.sh
sudo bash setup/install_android_sdk.sh


• Make a directory called ~/a12s-twrp
cd ~/a12s-twrp

repo init --depth=1 -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git -b twrp-11


repo sync -c --no-clone-bundle --no-tags --optimized-fetch --prune --force-sync -j1

copy ~/TWRP/twrpdtgen/output/samsung to /a12s-twrp/device


Building
========


. build/envsetup.sh
lunch twrp_a12s-eng
mka recoveryimage –j1

Recovery.img at out/Samsung/a12s/...img




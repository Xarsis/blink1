Compiling blink1-tool for OpenWrt
=================================

This step-by-step was designed for Linino, the OpenWrt in Arduino Yun,
but the general process works for any OpenWrt.
Must do this on a Linux box.  (I use an Ubuntu VM)  


* Follow guide:
https://openwrt.org/docs/guide-developer/single.package

```
cd openwrt
./scripts/feeds install -a
./scripts/feeds install -a
make menuconfig  
```
* In `menuconfig` select `Build the OpenWrt SDK (CONFIG_SDK)` and Save

```
make
```

* Run `make` to build the SDK (this takes a long time and several gigabytes)


----------------------
* Install needed tools
```
     % apt-get install build-essential git-core libncurses5-dev zlib1g-dev gawk asciidoc bash bc binutils bzip2 fastjar flex g++ gcc util-linux zlib1g-dev libncurses5-dev libssl-dev perl-modules rsync ruby sdcc unzip wget gettext xsltproc zlib1g-dev libxml-parser-perl
```

* Download version of OpenWrt used on the Arduino Yun:
```
    % mkdir ~/projects
    % cd projects
    % git clone https://github.com/arduino/linino.git linino
```

* Build the cross-compiling toolchain and host tools:
(note this step can take upwards of an hour and 10GB of disk space)
```
    % cd ~/projects/linino/trunk
    % make tools/install
    % make toolchain/install
```

* Stick the blink1 OpenWrt directory in the "package" directory where packages-to-be-built live:
```
    % cd ~/projects/linino/trunk
    % cd package
    % ln ~/projects/blink1/openwrt blink1
```

* Build blink1-tool and create ipkg file:
```
    % make V=s package/blink1/compile
```

* Grab resulting .ipkg file and copy to Yun/OpenWrt system:
```
    % scp ~/projects/linino/trunk/bin/ar71xx/packages/blink1_007-1_ar71xx.ipk root@arduino.local:
```

* Log onto Yun/OpenWrt system and install the package
```
    % ssh root@arduino.local
    # opkg install blink1_007-1ar71xx.ipk
```

* Test out blink1-tool with:
```
    # blink1-tool --on
```


-------------------

Notes:
------
- http://wiki.openwrt.org/doc/howtobuild/single.package
- http://fibasile.github.io/arduino-yun-custom-buildroot.html
- http://hanez.org/openwrt-building-software-packages.html


* Configuring other feeds
```
cd  linino/trunk    
./scripts/feeds update -a
./scripts/feeds install -a
make menuconfig
```

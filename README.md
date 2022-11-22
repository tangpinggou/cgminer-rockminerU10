rockminer u10是模仿gekkoscience 2pac，网上只有编译好的windows版cgminer，偶然间发现有支持2pac的cgminer，所以我就加上对rockminer u10的支持。
安装方法：

	sudo apt-get update -y
	sudo apt-get install build-essential autoconf automake libtool pkg-config libcurl4-openssl-dev libudev-dev \
	libjansson-dev libncurses5-dev libusb-1.0-0-dev zlib1g-dev -y

	git clone https://github.com/tangpinggou/cgminer-rockminerU10.git

	cd cgminer-rockminerU10
	sudo usermod -a -G dialout,plugdev $USER
	sudo cp 01-cgminer.rules /etc/udev/rules.d/
	CFLAGS="-O2 -march=native" ./autogen.sh
	./configure --enable-gekko
	make
	
运行方法：同windows。例如：

	./cgminer -o stratum+tcp://solo.ckpool.org:3333 --gekko-2pac-freq 75 -u 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa.0 -p x --suggest-diff 100
	

linux下的效率相较于windows更高，目前版本cgminer不支持core solo，但是可以用bfgminer作为跳板。方法如下：

	1.运行bitcoin core并同步，打开rpc.
	
	2.运行bfgminer.例如：./bfgminer -o http://localhost:8332 -u rpcusername -p rpcpassword --stratum-port 3333 --generate-to 1A1zP1eP5QGefi2DMPTfTL5SLmv7DivfNa --coinbase-sig "solo miner".
	
	3.运行cgminer.例如：./cgminer -o stratum+tcp://localhost:3333 --gekko-2pac-freq 75 -u x -p x
	
----分割线，以下为原文-----------------------------------------------------------------------------------------
### #####################################################################
##  CGminer 4.12.0 GekkoScience Compac, 2pac & Newpack BM1384 & CompacF #
### #####################################################################

This is cgminer 4.12.0 with support for GekkoScience Compac, CompacF, 2pac.

This software is forked from cgminer 4.11.1 original from ckolivas.

Then i added v.thoang gekko drivers

I also adapted this code to my own material / miners / computer

(you can refer to original documentation to docs/README)

### GekkoScience compac Usb miner ##
<p align="center">
<img src="https://raw.githubusercontent.com/wareck/cgminer-gekko/master/docs/gekko.jpg">
</p>

### GekkoScience 2pac Usb miner ##
<p align="center">
<img src="https://raw.githubusercontent.com/wareck/cgminer-gekko/master/docs/2pac.jpg">
</p>

### GekkoScience Newpac Usb miner ##
<p align="center">
<img src="https://raw.githubusercontent.com/wareck/cgminer-gekko/master/docs/newpac.jpg">
</p>

### GekkoScience CompacF Usb miner ##
<p align="center">
<img src="https://raw.githubusercontent.com/wareck/cgminer-gekko/master/docs/compacf.png">
</p>

### GekkoScience Terminus r606 ##
<p align="center">
<img src="https://raw.githubusercontent.com/wareck/cgminer-gekko/master/docs/terminus.jpg">
</p>

This software use a slighty moddified driver from novak's gekko driver.

I allows working with icarus miner and gekko on same rig.

to build this specific code on linux:

	sudo apt-get update -y
	sudo apt-get install build-essential autoconf automake libtool pkg-config libcurl4-openssl-dev libudev-dev \
	libjansson-dev libncurses5-dev libusb-1.0-0-dev zlib1g-dev -y

	git clone https://github.com/wareck/cgminer-gekko.git

	cd cgminer-gekko
	sudo usermod -a -G dialout,plugdev $USER
	sudo cp 01-cgminer.rules /etc/udev/rules.d/
	CFLAGS="-O2 -march=native" ./autogen.sh
	./configure --enable-gekko
	make
	sudo make install
	
### Option Summary ###

```
  --gekko-compac-freq <clock>   Chip clock speed (MHz) default is 200 Mhz
  --gekko-2pac-freq <clock> Chip clock speed (Mhz) default is 150 Mhz 
  --gekko-newpac-freq <clock> Chip clock speed (Mhz) default is 150 Mhz
  --gekko-r606-freq <clock> Set GekkoScience Terminus R606 frequency in MHz, range 50-900 (default: 550)
  --gekko-terminus-detect Detect GekkoScience Terminus BM1384
  --suggest-diff <value> Limit diff for starting mine default is 32
```

### Command line ###

```
 ./cgminer -o pool_url -u username -p password --gekko-compac-freq 200 --gekko-2pac-freq 150 --gekko-newpac-freq 150
```

For windows users, you can donwload the release zip file

Inside you can find a cgminer_run.bat file and you can adjust you settings.

### Nicehash extranonce support ##

You can use your miner with last extranonce support for nicehash by adding #xnsub at the address end, like this:

	./cgminer -o stratum+tcp://sha256.eu.nicehash.com:3334#xnsub -u my_btc_address -p x --gekko-compac-freq 200 --gekko-2pac-freq 150 --gekko-newpac-freq 150
	
## Credits
```
Kanoi    : https://github.com/kanoi/cgminer.git (code update to 4.12.0 and gekko driver improvement)
ckolivas : https://github.com/ckolivas/cgminer.git (original cgminer code)
vthoang  : https://github.com/vthoang/cgminer.git (gekko driver)
nicehash : https://github.com/nicehash/cgminer-ckolivas.git (nicheash extranonce)
prmam : coinbase flags fix and default configuration, udev permissions fix
chipjarred : macos compilation fix, memory leak fix, and Fixing change of sign warnings when referring to buffers

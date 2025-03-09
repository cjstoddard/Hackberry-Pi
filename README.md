# Ten things to do with your Hackberry Pi 5 (HPi 5)

### 1. Before you get your HPi 5

Whether you get your HPi 5 from Tindie or Elecrow, it is going to take a week or two to get. While you are waiting, make sure you have the other things you need to complete the build. If you got it from Tindie, it will not come with a HPi 5, power supply or batteries. None of these things is hard to come by. Keep in mind, none of the products I link here should be considered best in class, they are simply what I bought and seem to work for me.

[Raspberry Pi 5, 8GB Model](https://www.adafruit.com/product/5813)

[Canakit RPi Power Supply](https://www.amazon.com/dp/B07TYQRXTK?ref=ppx_yo2ov_dt_b_fed_asin_title)

[18650 Rechargeable Battery, 3000mAh](https://www.amazon.com/dp/B0CRNSFQGX?ref=ppx_yo2ov_dt_b_fed_asin_title)

Both versions should come with SD cards, but they come with 64GB SD cards, which is fine, but I think 128GB is minimum needed for any device you are going to regularly use, but 256GB is better.

[256GB SD Card](https://www.amazon.com/dp/B09X7CRKRZ?ref=ppx_yo2ov_dt_b_fed_asin_title)

If you are getting it from Elecrow, it should come with the RPi 5 and batteries, you will only need the power supply. But please read the included components, so you know what you need.

### 2. Get an NVMe SSD for your HPi 5

This is not necessary, however for less than $60 this is the single best upgrade money can buy to improve the performance of your device. SSD's are faster and more reliable than SD cards. You will need both the adapter and the SSD. The adapter also adds active cooling to the device.

[Waveshare PCIe to M.2 Adapter Board (E)](https://www.amazon.com/dp/B0DBZ6PWF6?ref=ppx_yo2ov_dt_b_fed_asin_title)

[256GB NVMe 2230 SSD](https://www.amazon.com/dp/B0DKBHTCTS?ref=ppx_yo2ov_dt_b_fed_asin_title)

If you get this board, you can set up to 4 fan speeds to use based on the temp of the CPU. Enter the following command; 

> sudo nano /boot/firmware/config.txt

Now add these lines under the CM5 section.

> dtparam=fan_temp0=40000,fan_temp0_hyst=2000,fan_temp0_speed=100
> dtparam=fan_temp1=50000,fan_temp1_hyst=3000,fan_temp1_speed=150
> dtparam=fan_temp2=60000,fan_temp2_hyst=4000,fan_temp2_speed=200
> dtparam=fan_temp3=70000,fan_temp3_hyst=5000,fan_temp3_speed=250

40000 equals roughly 40C CPU temp, 50000 equals roughly 50C CPU temp, etc. The top speed of the fan is 255, adjust to taste. Now save and exit the file, and reboot.

### 3. Update your system

Having an up to date system is paramount for any system, run these commands on a regular basis.

> sudo apt update && sudo apt upgrade

### 4. Install useful programs

The first set of programs are text mode programs that I find useful, the second set are GUI programs I use. Adjust according to taste.

> sudo apt install mc links cmus htop neofetch tmux ffmpeg net-tools build-essential lame zsh mailutils default-jre tty-clock calcurse git wget curl flex bison bc libavcodec-extra -y
>
> sudo apt install synaptic tilix audacious flameshot thunderbird filezilla transmission remmina gdebi thonny vlc zim code -y

### 5. Change Desktop Environment (DE)

I am not a big fan of the PIXIE DE or Wayland. If you want to change your DE to something else, use Tasksel to install the DE of your choice. I would also select Debian desktop environment, this will install programs commonly used with Desktop computers, but if you prefer a minimal setup, then leave it unchecked.

> sudo tasksel

The default display manager is configured in such a way as to not let you change desktop environments, so you will need to install sddm

> sudo apt install sddm

and then run raspi-config and set the login to require a user password.

### 6. Setup Firewall

If you are going to be using your HPi 5 in public places, you will want to take some steps to secure it from bad actors. Before you enable the firewall, you will want to allow ssh connections through your firewall so you can connect to it remotely.

> sudo apt install ufw
>
> sudo ufw allow ssh
>
> sudo ufw enable

### 7. Secure ssh access to your system

Next you will want to setup your HPi 5 so it will only accept ssh connections from systems you want it to. First log into your HPi 5 from the system you want to access it from, then log back out, then run the following command to generate a private and public key set.

> ssh-keygen

Just hit enter three times to generate the keys. Next you will want to copy your public key to your HPi 5 with the following command;

> ssh-copy-id remote_username@remote_server_ip_address

Now you should be able to log in using ssh, but the HPi 5 will not require a password. Repeat these steps from each machine you want to remotely access the HPi 5 from. Once you have done that, you will now want to disable password authentication so that only connections from systems that the HPi 5 has a key for will be allowed to log in. Run the following command;

> sudo nano /etc/ssh/sshd_config

Search for the line "#PasswordAuthentication yes", delete the # and change yes to no. Save the file and then reboot the system.

### 8. Install Flatpak

Raspberry Pi OS has the same issue as Debian, the packages tend to fall behind long before a new release is made. If you have programs that you want to stay up to date on, Flatpak is the way to do it.

> sudo apt install flatpak
>
> flatpak remote-add --if-not-exists --user flathub https://dl.flathub.org/repo/flathub.flatpakrepo

Now you can install programs from the Flatpak repo, which will be more up to date without breaking your install. For instance, if you want to install Discord, run the following;

> flatpak install --user --assumeyes flathub com.discordapp.Discord

You can go to https://flathub.org/ to see what programs are available.

### 9. Save wear and tear on your SD card

If you are using an SD card, there are a few ways to improve performance and save wear and tear on the SD card by moving repetitive writes to RAM. If you have a 4GB Raspberry Pi 5, I would not be inclined to use all of them, pick and choose what works for you without sacrificing too much usable RAM. If you are using an 8 or 16GB Raspberry Pi 5, I would move Logging and Browser to RAM. If you are using an NVMe SSD drive, you probably don't need to do this, but then, every little bit helps.

Move logging to a RAM disk: One way to help reduce the number of writes to your SD cards is to write log files to a RAM drive. Below is a link to log2ram, which does exactly this. The downsides to this method is first it reduces the amount of RAM available to programs by 128 MB and if for some reason the HPi 5 looses power or crashes, log files may not be written to the SD card and finding out the problem will be more difficult without proper logs.

https://github.com/azlux/log2ram

Sync browser related directories to RAM: Continuing with the theme of minimizing wear and tear on your SD card, you can move the temporary files your browser creates to RAM. Again, this reduces the amount of RAM you have available, so if you are running on 4GB of RAM, the extra bite out of your RAM may be a bit more than you would like. Pay attention to your RAM usage after installing this software, if you are consistently dipping into swap after installing it, you are probably not getting any benefit from using it. If you are using an 8GB CM, this should not be much of an issue for you.

https://github.com/64-bitman/browser-on-ram

Move swap file to RAM disk: Another useful tool for reducing SD card wear is zram-swap-config, this moves your swap file to a RAM disk as well. I am ambivalent towards this idea, it removes your swap file to RAM, but it reduces the amount of RAM available, however the only reason your system dips into swap is when it is close to running out of RAM, so this idea seems counter productive to me. Personally, I don't use it, not just because of what I stated earlier, but also I don't remember the last time any Linux machine I have actually using swap. I would run htop and do normal things with you machine and watch the available memory and note how often you dip into swap memory. If you consistently do not use more than three quarters of your available RAM, you likely do not need this.

https://github.com/StuartIanNaylor/zram-swap-config

### 10. Screen protector and a carrying case.

These things are expensive after all is said and done. Screens get scratched and accidental drops happen. Preserve your investment by finding screen protectors and a carrying case, especially if you plan to use the device outside of safe environments. I have not found any products I am satisfied with yet, but when I do, I will add them here.

Screen Protector for HPi 5:
[BoxWave Screen Protector Compatible with Waveshare Square Capacitive Touch Screen (4 in)](https://www.amazon.com/dp/B0CWWH8FBM?)
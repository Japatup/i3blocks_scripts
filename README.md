## my i3blocks scripts

### Default i3blocks scripts

Defaults scripts which comes with i3blocks are stored in the default_scripts directory.
(If you have installed i3blocks, they initialy should be in /usr/share/i3blocks)

In my config, i3blocks is parameterized to look for scripts in this dir. 
I store personal scripts here (for battery part for instance), and I just create symlink for default scripts I am ok with.
To mimic this way of doing, you will then have to create symlinks of scripts you miss from the default_scripts dir.

### My scripts

##### battery

I changed the default script behavior so it uses the i3-nagbar.

When battery level goes under a first chosen level, i3-nagbar appears in warning mode (the orange one) to warn you and remind you to charge your battery.

![warning i3-nagbar](https://github.com/Japatup/i3blocks_config/blob/master/screenshots/i3-nagbar%20warning.jpg)

There is also a second level you can choose (lower), to call the i3-nagbar in error mode (the red one).

Both levels can be changed at the beginning of the battery script (variables seuil_critique_warning and *seuil_critique_error*). If I didn't change them since I wrote this readme, they should be 17% and 13%. By the way, the *seuil_critique_33* parameter corresponds to a default script fonctionality I did mimic. Under this level, the scripts exits with 33 error code, which make i3blocks highlight the battery text output in red in the i3bar.

In order to warn you only one time when your battery level goes under the limit you chose, and not each time the battery script is launched by i3blocks, the program writes a file as a reminder for itself. This way, it knows you have already been warned, and doesn't bother you anymore. This "memory file" is deleted when you plug your battery charger or when your battery level is higher than the limit you defined.

You may change the path of the dir the mem file will be writen in at the beginning of the script (variable checkpoint_file_dir). As for me, I chose a tmpfs dir (ie in RAM), to prevent my physical disk to work every minute. To create a tmpfs directory, just add this line in your /etc/fstab file : 

```
tmpfs /the/dir/of/your/choice tmpfs mode=1777,nosuid,nodev 0 0
```

**NB : This scripts needs python3 to run.**

##### iface and ifacedown

The iface program actually is the default script from Julien Bonjean and Alexander Keller. I just added the name of the chosen network in the original output, that only printed my ip address.

This scripts uses network-manager to find the name of your network. Fell free to adapt it for wicd, wpa-supplicant or any other software ;)

The ifacedown script comes when no network is available (when I turn on the plane mode for instance). I used this second script because I wanted a different icon to appear, one for a successful connection, and another one if no network could be found. Indeed, I did not manage to include the icon in the output of the script and then change it dynamicaly. Thus, I use two scripts that both have their own *label* parameter in the i3blocks.conf file (cf. my [i3_config repository](https://github.com/Japatup/i3_config) ) and thus their own icon. According to a network is found or not, both scripts output something or nothing, and then replace each other in the i3bar, which makes the trick :)
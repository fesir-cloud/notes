watch -n5 'sudo kill -USR1 $(pgrep ^dd)'


pv /path/to/isofile | sudo dd of=/dev/sdX





As @Ignacio Vazquez-Abrams answered in https://askubuntu.com/a/178741/905329:

    You can use xinput to float the input device under X.

        Execute the command xinput list to list your input devices.
        Locate AT Translated Set 2 keyboard and take note of its id number; this will be used to disable the keyboard. Also, take note of the number at the end, [slave keyboard (#)]; this is the id number of the master, which will be used to re-enable your keyboard.
        To disable the keyboard, execute the command xinput float <id#>, where <id#> is your keyboard's id number. For example, if the id was 10, then the command would be xinput float 10.
        To re-enable the keyboard, execute the command xinput reattach <id#> <master#>, where master is that second number we noted down. So if the number was 3, you would do xinput reattach 10 3.


https://forums.linuxmint.com/viewtopic.php?t=112013

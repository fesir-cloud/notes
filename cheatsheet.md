watch -n5 'sudo kill -USR1 $(pgrep ^dd)'


pv /path/to/isofile | sudo dd of=/dev/sdX

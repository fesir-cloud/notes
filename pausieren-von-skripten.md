# Pause-Funktionen ins bash-Skripten wie mit pause(DOS)

https://stackoverflow.com/questions/92802/what-is-the-linux-equivalent-to-dos-pause


Enter solution

    read -rsp $'Press enter to continue...\n'

Escape solution (with -d $'\e')

    read -rsp $'Press escape to continue...\n' -d $'\e'

Any key solution (with -n 1)

    read -rsp $'Press any key to continue...\n' -n 1 key
    # echo $key

Question with preselected choice (with -ei $'Y')

    read -rp $'Are you sure (Y/n) : ' -ei $'Y' key;
    # echo $key

Timeout solution (with -t 5)

    read -rsp $'Press any key or wait 5 seconds to continue...\n' -n 1 -t 5;

Sleep enhanced alias

    read -rst 0.5; timeout=$?
    # echo $timeout


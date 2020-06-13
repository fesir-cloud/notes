# Pause-Funktionen in (ba)sh-Skripten wie mit pause(DOS)

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

Explanation

-r specifies raw mode, which don't allow combined characters like "\" or "^".

-s specifies silent mode, and because we don't need keyboard output.

-p $'prompt' specifies the prompt, which need to be between $' and ' to let spaces and escaped characters. Be careful, you must put between single quotes with dollars symbol to benefit escaped characters, otherwise you can use simple quotes.

-d $'\e' specifies escappe as delimiter charater, so as a final character for current entry, this is possible to put any character but be careful to put a character that the user can type.

-n 1 specifies that it only needs a single character.

-e specifies readline mode.

-i $'Y' specifies Y as initial text in readline mode.

-t 5 specifies a timeout of 5 seconds

key serve in case you need to know the input, in -n1 case, the key that has been pressed.

$? serve to know the exit code of the last program, for read, 142 in case of timeout, 0 correct input. Put $? in a variable as soon as possible if you need to test it after somes commands, because all commands would rewrite $?

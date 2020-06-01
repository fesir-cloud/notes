# Die folgende Zeile sucht Prozesse und killt sie:

    printf "Suche PID von Prozess mit folgendem string:\n";read s;sudo pstree -lp>l1.log;egrep $s[\(\}]* $(pwd)/l1.log>l2.log;cat l2.log|egrep -o [0-9]*>l3.log;cat $(pwd)/l2.log;rm $(pwd)/l1.log;printf "kill all?(yes/no)\n";read a;if [ $a == yes ];then kill $(tail -n 1 $(pwd)/l3.log);fi


# Such deine IPv6-Adresse:

    wget -qO - http://stdio.be|grep title|egrep -o [:0-9a-f]+{8}

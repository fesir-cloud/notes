# Die folgende Zeile sucht Prozesse und killt sie:

    printf "Suche PID von Prozess mit folgendem string:\n";read s;sudo pstree -lp>l1.log;egrep $s[\(\}]* $(pwd)/l1.log>l2.log;cat l2.log|egrep -o [0-9]*>l3.log;cat $(pwd)/l2.log;rm $(pwd)/l1.log;printf "kill all?(yes/no)\n";read a;if [ $a == yes ];then kill $(tail -n 1 $(pwd)/l3.log);fi


# Such deine IPv6-Adresse:
Folgende zeilen schreiben die aktuelle IPv6 Adresse in STOUT:

    wget -qO - http://stdio.be|grep title|egrep -o [:0-9a-f]+{8}
    
    V
    
    wget -qO - http://stdio.be|head -n 1|egrep -o [:0-9a-f]+{8}
    
    V
    
    wget -qO - nsx.de|tail -n 4|head -n 1
    
    V direkt in eine Logdatei("IPv6.log")
    
    while true;do wget -qO - nsx.de|tail -n 4|head -n 1 >IPv6.log;sleep 1;done

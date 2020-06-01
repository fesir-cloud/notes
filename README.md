# Die folgende Zeile sucht Prozesse und killt sie:

    printf "Suche PID von Prozess mit folgendem string:\n";read s;sudo pstree -lp>l1.log;egrep $s[\(\}]* $(pwd)/l1.log>l2.log;cat l2.log|egrep -o [0-9]*>l3.log;cat $(pwd)/l2.log;rm $(pwd)/l1.log;printf "kill all?(yes/no)\n";read a;if [ $a == yes ];then kill $(tail -n 1 $(pwd)/l3.log);fi


# Such deine IPv6-Adresse:
Folgende zeilen schreiben die aktuelle IPv6 Adresse in STOUT:

    wget -qO - http://stdio.be|grep title|egrep -o [:0-9a-f]+{8}
    
oder:
    
    wget -qO - http://stdio.be|head -n 1|egrep -o [:0-9a-f]+{8}
    
oder:
    
    wget -qO - nsx.de|tail -n 4|head -n 1
    
direkt in eine Logdatei("IPv6.log"):
    
    while true;do wget -qO - nsx.de|tail -n 4|head -n 1 >IPv6.log;sleep 1;done

Schreibe die aktuelle IPv6-Adresse in "v6.l" und teste jede sekunde ob diese noch übereinstimmt, wenn nicht überschreibe "v6.l":

    while true;do [[ "$(wget -qO - nsx.de|tail -n 4|head -n 1)" != "$(cat v6.l)" ]] &&  wget -qO - nsx.de|tail -n 4|head -n 1 >v6.l||printf "IPv6 unchanged\n";sleep 1;done 
    
Wie oben, nur überschreibe "v6.l" nicht, sondern lege eine Liste an:

    while true;do [[ "$(wget -qO - nsx.de|tail -n 4|head -n 1)" != "$(tail -n 1 v6.l)" ]] &&  wget -qO - nsx.de|tail -n 4|head -n 1 >>v6.l||printf "IPv6 unchanged\n";sleep 1;done
    
Als  deamon

    $(while true;do [[ "$(wget -qO - nsx.de|tail -n 4|head -n 1)" != "$(tail -n 1 v6.l)" ]] &&  wget -qO - nsx.de|tail -n 4|head -n 1 >>v6.l;sleep 1;done)&

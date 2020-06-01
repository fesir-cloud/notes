# Die folgende Zeile sucht Prozesse und killt sie:

    printf "Suche PID von Prozess mit folgendem string:\n";read s;sudo pstree -lp>l1.log;egrep $s[\(\}]* $(pwd)/l1.log>l2.log;cat l2.log|egrep -o [0-9]*>l3.log;cat $(pwd)/l2.log;rm $(pwd)/l1.log;printf "kill all?(yes/no)\n";read a;if [ $a == yes ];then kill $(tail -n 1 $(pwd)/l3.log);fi


# Such deine IPv6-Adresse:
Folgende zeilen schreiben die aktuelle IPv6 Adresse in STOUT:

    
    I: wget -qO - http://stdio.be|head -n 1|egrep -o [:0-9a-f]+{8}
    
oder:
    
    II: wget -qO - nsx.de|tail -n 4|head -n 1
    
direkt in eine Logdatei("ipv6.l"):
    
    while true;do wget -qO - nsx.de|tail -n 4|head -n 1 >ipv6.l;sleep 1;done

Schreibe die aktuelle IPv6-Adresse in "v6.l" und teste jede sekunde ob diese noch übereinstimmt, wenn nicht überschreibe "v6.l":

    while true;do [[ "$(wget -qO - nsx.de|tail -n 4|head -n 1)" != "$(cat ipv6.l)" ]] &&  wget -qO - nsx.de|tail -n 4|head -n 1 >ipv6.l||printf "IPv6 unchanged\n";sleep 1;done 
    
Wie oben, nur überschreibe "ipv6.l" nicht, sondern lege eine Liste an:

    while true;do [[ "$(wget -qO - nsx.de|tail -n 4|head -n 1)" != "$(tail -n 1 ipv6.l)" ]] &&  wget -qO - nsx.de|tail -n 4|head -n 1 >>ipv6.l||printf "IPv6 unchanged\n";sleep 1;done
    
Als  non HANGUP Prozess

    $(while true;do [[ "$(wget -qO - nsx.de|tail -n 4|head -n 1)" != "$(tail -n 1 ipv6.lipv6.l)" ]] &&  wget -qO - nsx.de|tail -n 4|head -n 1 >>ipv6.l;sleep 1;done)&


# Scripte als deadmon:
https://unix.stackexchange.com/questions/426862/proper-way-to-run-shell-script-as-a-daemon
https://stackoverflow.com/questions/3430330/best-way-to-make-a-shell-script-daemon

nohup *** & //schneller Weg über nohup

    
    
    nohup $(myscript) 0<&- &> file.log &  ///Grundlegende Syntax: startet myscript und schreibt STERR und STOUT In file.log
                A
                | 
    ____________|__________________________________________________________________________________________________________
    while true;do [[ "$(wget -qO - nsx.de|tail -n 4|head -n 1)" != "$(tail -n 1 ipv6.l)" ]] &&  wget -qO - nsx.de|tail -n 4|head -n 1 >>ipv6.l;sleep 1;done
    _______________________________________________________________________________________________________________________
    

Starte Skript als noHANGUP
    
    nohup $(while true;do [[ "$(wget -qO - nsx.de|tail -n 4|head -n 1)" != "$(tail -n 1 ipv6.l)" ]] &&  wget -qO - nsx.de|tail -n 4|head -n 1 >>ipv6.l;sleep 1;done) &
    
Starte Skript als noHANGUP und schreibt STERR und STOUT In ipv6-check.log:
    
    nohup $(while true;do [[ "$(wget -qO - nsx.de|tail -n 4|head -n 1)" != "$(tail -n 1 ipv6.l)" ]] &&  wget -qO - nsx.de|tail -n 4|head -n 1 >>ipv6.l;sleep 1;done) 0<&- &> ipv6-check.log &

 
Schreibe das script in ipv6check.sh, welches in "ipv6.l" die aktuelle IPv6 schreeibt, mach es ausführbar, und starte es mit nohup als nicht aufhängbarer Prozess.

                nohup ./myscript 0<&- &> file.log &   
                // Grundlegende Syntax: startet myscript und schreibt STERR und STOUT In file.log
                                                        A
    Schreibt das obige Skript in "ipv6-check.sh" und ...|
    printf "while true;do [[ "$(wget -qO - nsx.de|tail -n 4|head -n 1)" != "$(tail -n 1 ipv6.l)" ]] &&  wget -qO - nsx.de|tail -n 4|head -n 1 >>ipv6.l;sleep 1;done">ipv6-check.sh;sudo chmod +x ipv6-check.sh;nohup ./ipv6-check.sh 0<&- &> ipv6-check.log

""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
nohup ist ein primitives Werkzeug, welches ein Kommando so konfiguriert, dass es ein bestimmtes Signal ignoriert. Damit ist es noch weit davon entfernt, eine Lösung für alle möglichen Probleme eines asynchronen Programmbetriebs anzubieten, wie es etwa bei einem vollständigen Stapelverarbeitungssystem der Fall sein könnte.     --    Wikipedia (https://de.wikipedia.org/wiki/Nohup)
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""


    
    
# Erstellen einer systemd-Einheit um ein Skript als Dämon zu starten:


Skript in /usr/bin/ipv6-check schreiben

    printf "while true;do [[ "$(wget -qO - nsx.de|tail -n 4|head -n 1)" != "$(tail -n 1 ipv6)" ]] &&  wget -qO - nsx.de|tail -n 4|head -n 1 >>ipv6;sleep 1;done">/usr/bin/ipv6-check


Systemd-Einheit in /etc/systemd/system/ipv6-check.service schreibeen
    
    printf "
    [Unit]
    Description=ipv6-check

    [Service]
    ExecStart=/usr/bin/ipv6-check
    Restart=on-failure

    [Install]
    WantedBy=multi-user.target
    "

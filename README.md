# M300
TBZ Modul 300.

## Vorausetzung

### Folgendes ist benötigt, um mit Vagrant VMs zu erstellen:
- Virtualbox
- Vagrant
- Bash-Programm (z.B. GitClient)
- GitHub Accout
- Repository lokal vorhanden

Programme Herrunterladen:
- [Virtualbox](https://www.virtualbox.org/wiki/Downloads)
- [GitClient](https://git-scm.com/downloads)
- [Vagrant](https://www.vagrantup.com/)

GitHub-Accout erstellen (falls nicht schon vorhanden) 
- [Link hier](https://github.com/join)





Repository Erstellen
1. Anmelden auf GitHub
2. Innerhalb der Willkommens-Seite auf Start a project klicken
3. Unter Repository name einen Name definieren (z.B. M300-Services)
    > in meinem Fall: <br> https://github.com/Misteryx5/M300
4. Radio-Button bei Public belassen <br> (somit ist es für alle ersichtlich)
5. Haken bei Initialize this repository with a README setzen
6. Auf Create repository klicken





Repository lokal hinzufügen

1. GitBash öffnen (GitClient)
2. Git konfigurieren mit Informationen des GitHub-Accounts:
    ```
    git config --global user.name "<username>"
    git config --global user.email "<e-mail>"
    ```
3. Navigiere zum Ort, an welchem die Reposytory geclont werden soll
    >in meinem Fall: <br>
    E:\04_IoT\MeinLokalesRepository\
4. Reposytory klonen:
    ```
    git clone "link des Reposytory auf GitHub"
    ```
    >in meinem Fall: <br>
    ```
    git clone https://github.com/Misteryx5/M300
    ```
5. In das M300-Verzeichnis wechseln:
    ```
    cd "Reposytory-NAME"
    ```
    >in meinem Fall: <br>
    > ```
    > cd M300
    > ```
    > <br>
6. Reposytory aktualisieren
    ```
    git pull
    ```
    - mit "git status" kann der Status abgerufen werden





- Um Änderungen am Lokalem Reposytory hochzuladen muss volgendes gemacht werden:
    1. zum Reposytory navigieren
    2. Dateien dem Upload hinzufügen:
    ```
    git add -A .
    ```
    >"-A ." ist zum alles hinzuzufügen.
    1. Den Upload commiten:
    ```
    git commit
    ```
    >mit "-m "meinKommentar" " kann ein Kommentar des Commints hinnzugefügt werden
    1. Upload abschliessen:
    ```
    git push
    ```
>- Alle Befehle auf einen Blick:
>
>```
>cd Pfad/zu/meinem/Repository    # Zum lokalen GitHub-Repository wechseln
>
>git status                      # Geänderte Datei(en) werden rot aufgelistet
>git add -A                      # Fügt alle Dateien zum "Upload" hinzu
>git status                      # Der Status ist nun grün > Dateien sind >Upload-bereit (Optional) 
>git commit -m "Mein Kommentar"  # Upload wird "commited" > Kommentar zu >Dokumentationszwecken ist dafür notwendig
>git status                      # Dateien werden nun als "zum Pushen bereit" >angezeigt
>git push                        #Upload bzw. Push wird durchgeführt
>```
><br>









### alternativ kann eine SSH-Verbindung zwischen dem Client und GitHub erstellt werden

SSH-Key Generieren(lokal)
1. Terminal (Bash) öffnen
2. Folgenden Befehl mit der Account-E-Mail von GitHub einfügen:
    ```
    ssh-keygen -t rsa -b 4096 -C "beispiel@beispiel.com"
    ```
3. Neuer SSH-Key wird erstellt:
    ```
    Generating public/private rsa key pair.
    ```
4. Bei der Abfrage, unter welchem Namen der Schlüssel gespeichert werden soll, die Enter-Taste drücken (für Standard):
    ```
    Enter a file in which to save the key (~/.ssh/id_rsa): [Press enter]
    ```
5. Nun kann ein Passwort für den Key festgelegt werden. Ich empfehle dieses zu setzen und anschliessend dem SSH-Agent zu hinterlegen, sodass keine erneute Eingabe (z.B. beim Pushen) notwendig ist:
    ```
    Enter passphrase (empty for no passphrase): [Passwort]
    Enter same passphrase again: [Passwort wiederholen]
    ```


SSH-Key auf GitHub hinterlegen

1. SSH-Key kopieren
   Diese befindet sich hier:
   ```
   C:\Beutzer\[Benutzername]\.ssh\id_rsa.pub
   ```

2. Anmelden unter www.github.com
3. Auf Benutzerkonto klicken (oben rechts) und den Punkt Settings aufrufen
4. Unter den Menübereichen auf der linken Seite zum Abschnitt SSH und GPG keys wechseln
5. Auf New SSH key klicken
6. Im Formular unter Title eine Bezeichnung vergeben (z.B. MB SSH-Key)
Den zuvor kopierten Key mit CTRL + V einfügen und auf Add SSH key klicken
Der Schlüssel (SSH-Key) sollte nun in der übergeordneten Liste auftauchen


<br><br>


## Vagrant - Automatisiertes erstellen von VMs


Das Vagrantfile dient als Vorlage mit welchem beliebig viele VMs erstellt weden können.

<br>

### Ersellen eines Vagrantfile

- Damit eine VM automatisiert ertellt werden kann, soll das Vagrantfile editiert werden.

>Mein Vagrantfile ist hier zu finden: <br> https://github.com/Misteryx5/M300/blob/master/Vagrantfile

1. Nabigiere zum Vagrantfile
2. Öffne das Vagrantfile mit einem Editor, am besten hier ist [Visualstudio Code](https://code.visualstudio.com/)
   
3. Vagrantfile editieren:
   
   Zuerst bestimmt man die Specs der VM, die man erstellen möchte:

        ```
        Vagrant.configure(2) do |config|
            config.vm.box = "ubuntu/xenial64"
            config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true
            config.vm.synced_folder ".", "/var/www/html"  
            config.vm.provider "virtualbox" do |vb|
            vb.memory = "2048"
        end
        ```
    - Ich habe mich fpr das "ubuntu/xenial64" entschieden
    - Folgendermassen den Port konfiguriet: "forwarded_port", guest:80, host:8080, auto_correct: true
    - Die Virtualisierungs Software ausgewählt:  "virtualbox" do |vb|
    - und der VM 2GB RAM gegeben: vb.memory = "2048"
    > Viele weitere Einstellungsmöglichkeiten findet man im Internet

4. Nun kann man der VM sagen, wie sie sich konfigurieren soll: <br>
    Zwischen...
    ```
    config.vm.provision "shell", inline: <<-SHELL
    ```
    ...und...
    ```
        exit
    SHELL
    end
    ```
    ...kann man mit den bekannten Linux-Befehlen die VM anpassen.

    >So siehts bei mir aus:
    >```
    >### Hier wird die VM in der Comandline konfiguriert
    >   config.vm.provision "shell", inline: <<-SHELL
    >### Installation von apache2 (WEB-Server) 
    >   sudo apt-get update
    >   sudo apt-get -y install apache2 
    >
    >### "Uncomplicated Firewall" installation und regeln bestimmen port 80 und 22 geöffnet  
    >   sudo apt-get install ufw
    >   sudo ufw allow 80/tcp
    >   sudo ufw allow 22/tcp
    >   sudo ufw allow out 22/tcp 
    >   sudo ufw enable
    >
    >### ssh inatllation 
    >   sudo apt-get -y install openssh-server
    >
    >### installation der module für den Reverse proxy  
    >   sudo apt-get -y install libapache2-mod-proxy-html
    >   sudo apt-get -y install libxml2-dev
    >
    >### Aktivieren der Module in APACHE  
    >   sudo a2enmod proxy
    >   sudo a2enmod proxy_html
    >   sudo a2enmod proxy_http
    >
    >   sudo service apache2 restart
    >### Erstellen 2 User + hinzufügen der User zur Gruppe "admin"  
    >   sudo groupadd admin
    >       sudo useradd user01 -g admin -m -s /bin/bash 
    >       sudo useradd user02 -g admin -m -s /bin/bash 
    >       sudo chpasswd <<<user01:1234	
    >       sudo chpasswd <<<user02:1234
    >   exit
    >SHELL
    >end
    >```

<br>

### VM mit Vagrant kreieren und löschen
1. öffne die Bash
2. Navigiere zum Ort, an welchem die Vagrant-Box und die VM liegen soll
     > in meinem Fall: <br>
    E:\04_IoT\MeinLokalesRepository\M300\VMs\MeineVagrantVM

3. Mit folgendem befehl kann nun die VM mit Vagrant erstellt weden: (Vorraussetzung: Vagrantfile befindet sich am selben ort)
    ```
    vagrant up
    ```
    - Mit der ausgewählten Virtualisierung-SW kann man die VM laufen sehen.
4. Mit folgendem Befehl lässt sich die Maschine auschalten und löschen:
    ```
    vagrant destroy
    ```
- Man kann über die Bash auch direkt auf die laufende VM zugreifen, dazu sind folgende Befehle nötig:
    ```
    #verbindung zur VM aufbauen:
    
    cd Pfad/zu/meiner/Vagrant-VM      #Zum Verzeichnis der VM wechseln
    vagrant ssh                       #SSH-Verbindung zur VM aufbauen

    #Anschliessend können ganz normale Bash-Befehle abgesetzt werden:

    ls -l /bin  #Bin-Verzeichnis anzeigen
    df -h       #Freier Festplattenspeicher
    free -m     #Freier Arbeitsspeicher
    ```



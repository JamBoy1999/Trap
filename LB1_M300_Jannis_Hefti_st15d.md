# LB1 M300 Dokumentation

Hier werden einige Funktionen meines Vagrantfiles beschrieben. Dieses Vagrantfile wird einen Apache-Webserver mit einer Website installieren. Ausserdem wird Samba mit den nötigen Konfigurationen für ein Fileshare installiert.

## Vagrantfile
```
*config.vm.network "forwarded_port", guest:80, host:8080, auto_correct: true*
```

Hier sehen sie Konfigurierten Ports. Mit dem Port 8080, kann zum Beispiel dann die Website über den Browser abgerufen werden.
```
*config.vm.synced_folder ".", "/var/www/html"*
```

Hier wird der Synchronisierte Ordner erstellt. Diese Ordner werden dann auf dem Host und der VM ersichtlich sein. In diesem Verzeichnis wird dann auch das HTMl-File für die Website abgespeichert.
```
*config.vm.network "public_network", type:"dhcp"*
```

Mit diesem Command wird eine zusätzliche Netzwerkkarte hinzugefügt. Diese wird als Bridged konfiguriert. Somit haben wir nun zwei Netzwerkkarten eine Nat und die andere Bridged.
```
*sudo apt-get -y install apache2*
*sudo apt-get -y install samba*
```

Mit diesen zwei Befehlen werden der Apache und Samba Server installiert. Mit dem Parameter -y wird die Installation automatisch durchgeführt.
```
*sudo touch /etc/samba/smb.conf*
```

Anschliesend wird das smb.conf File erstellt. In diesem File werden die Konfigurationen für den Samba-Share reingeschrieben.
```
*echo "TEXT" | sudo tee -a /etc/samba/smb.conf*
```

Das smb.conf File wird so mit Daten befüllt. Mit dem Parameter -a werden die Zeilen nicht überschrieben sondern werden immer unterhalb eingefügt.
```
*sudo mkdir -p /srv/samba/share*
```
Der Share wird erstellt. Mit dem Parameter -p wird das Verzeichnis erstellt falls es nicht vorhanden ist.
```
*sudo service smbd restart*
```

Nachdem alle Konfigurationen durchgeführt wurden, muss der smbd Service neugestratet werden.
```
*sudo touch /var/www/html/index.html*
*echo "<h1>Hallo Herr Berger</h1>" | sudo tee -a /var/www/html/index.html*
*echo "<h1>Hier sehen Sie meine Website</h1>" | sudo tee -a /var/www/html/index.html*
```

Am Schluss wird noch das HTML-File für die Website erstellt, in dieses index.html kann man wieder belibig html-Code einfügen.

## Testing

Um die VM zu Testen, muss man bei einem Windowscomputer im Netzwerk den Ubuntu-xenial anwählen. Danach kann man sich mit einem smbUser sich anmelden. Wenn dieser nicht vorhanden ist, muss dieser noch erstellt werden.

Um Apache zu testen, muss im Browser loclahost:8080 eingegeben werden. Nun soll die HTML-Seite angezeigt werden die im Vagrantfile erstellt wurde.


# WireGuard und Pi-hole

WireGuard Split-Tunnel-VPN mit Pi-hole DNS-Server.

## Hintergrund

Ich wollte meine Dienste von außen erreichbar machen, ohne jeden Dienst einzeln direkt ins Internet freigeben zu müssen. Je mehr Dienste öffentlich erreichbar sind, desto mehr muss ich mich auch um deren Sicherheit kümmern. Deshalb wollte ich einen zentralen Zugang schaffen, über den ich auf meine Dienste zu Hause zugreifen kann.

Außerdem wollte ich eine DNS-Filterung über Pi-hole einrichten, um Werbung und andere unerwünschte Domains zu blockieren. Deshalb habe ich mich dazu entschieden, beide Funktionen miteinander zu kombinieren.

## Einrichtung

Auf meinem Proxmox-Server habe ich einen neuen LXC-Container mit Debian 13.7 erstellt und darin Pi-hole und WireGuard installiert.

Für WireGuard habe ich WGDashboard als Verwaltungsoberfläche eingerichtet, damit ich einfach neue Peers erstellen und verwalten kann. Anschließend habe ich mehrere Peers für mich und andere Personen in meinem Haushalt eingerichtet.

## Split-Tunneling

Den VPN habe ich so konfiguriert, dass nur Verbindungen zu IP-Adressen in meinem Heimnetzwerk durch den Tunnel geleitet werden.

Wenn ich beispielsweise auf meinen Nextcloud-Server zugreifen möchte, wird die Verbindung durch den VPN-Tunnel nach Hause geleitet. Wenn ich dagegen Google aufrufe oder normal im Internet surfe, läuft dieser Datenverkehr direkt über meine normale Internetverbindung und nicht durch den Tunnel.

Zusätzlich habe ich den DNS-Server der VPN-Verbindung auf Pi-hole ausgerichtet. Dadurch werden auch DNS-Anfragen durch den Tunnel nach Hause geleitet und dort von Pi-hole gefiltert.

Ich habe mich für Split-Tunneling entschieden, weil ich Zugriff auf mein Heimnetzwerk haben und meine DNS-Anfragen filtern lassen wollte, ohne dass mein gesamter Internetverkehr durch den VPN-Tunnel laufen muss. So wollte ich vermeiden, dass meine Internetgeschwindigkeit beim normalen Surfen durch den VPN-Tunnel eingeschränkt wird.

## DNS-Filterung

In Pi-hole habe ich mehrere Filterprofile eingerichtet. Für den Host selbst habe ich die DNS-Filterung so eingestellt, dass keine Anfragen blockiert werden. Zusätzlich habe ich eine persönliche Blockliste für mich und eine separate Blockliste für andere Geräte eingerichtet, die nicht mir gehören.

Geräte, die ohnehin zu Hause stehen, wie mein PC oder mein Fire TV Stick, verwenden Pi-hole direkt. Ihre DNS-Anfragen müssen also nicht erst durch einen VPN-Tunnel geleitet werden.

Die DNS-Anfragen werden zuerst von Pi-hole gefiltert und anschließend an Quad9 weitergeleitet, wo sie aufgelöst werden. Ich habe mich für Quad9 entschieden, weil der DNS-Server von meinem Zuhause aus die niedrigste Ping-Zeit hatte. Außerdem filtert Quad9 bereits Malware und andere bekannte schädliche Domains. Auch beim Datenschutz war Quad9 meiner Einschätzung nach die beste Wahl.

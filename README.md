# GAMON Release Channel

GAMON ist ein plattformuebergreifender Game-Server-Manager auf Basis von .NET 10 mit Web-UI, Hintergrunddiensten und integriertem Self-Update.

Dieses Repository ist der oeffentliche Release-Kanal fuer produktive Builds. Der Quellcode und die Build-Pipeline liegen bewusst in einem separaten Source-Repository.

## Funktionen

- Web-UI fuer die Verwaltung von Spielservern und Systemfunktionen
- Installation als optionaler Hintergrunddienst unter Windows und Linux
- Direkter Start im Vordergrund fuer lokale Nutzung und Debugging
- Integrierter Update-Mechanismus fuer neue Releases
- Getrennte Binary-Downloads fuer `win-x64` und `linux-x64`

## Verfuegbare Downloads

Jedes Release enthaelt derzeit folgende Artefakte:

- `GAMON-win-x64.zip`
- `GAMON-linux-x64.tar.gz`

Bitte lade immer das Paket passend zu deinem System herunter.

## Installation

1. Lade das passende Release herunter.
2. Entpacke das Archiv in ein eigenes Verzeichnis, zum Beispiel `C:\GAMON` oder `/opt/gamon`.
3. Entscheide danach, ob du GAMON nur im Vordergrund starten oder als Hintergrunddienst einrichten willst.

Wenn du den Manager als Background-Service installieren willst, kannst du entweder die Helferskripte oder die expliziten CLI-Commands verwenden.

Windows-Helferdateien:

- `Install-GAMON-Service.cmd`
- `Uninstall-GAMON-Service.cmd`

Linux-Helferdateien:

- `install-gamon-service.sh`
- `uninstall-gamon-service.sh`

### Linux per CLI installieren

Falls dein Linux-System nur per Terminal erreichbar ist, kannst du das aktuelle Release auch direkt herunterladen und installieren.

Mit `curl`:

```bash
mkdir -p /opt/gamon
cd /opt/gamon
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install -y libc6-i386 lib32gcc-s1 lib32stdc++6
curl -L https://github.com/DarthBaobab/GAMON-Releases/releases/latest/download/GAMON-linux-x64.tar.gz -o GAMON-linux-x64.tar.gz
tar -xzf GAMON-linux-x64.tar.gz
chmod +x GAMON
chmod +x install-gamon-service.sh
./install-gamon-service.sh
```

Mit `wget`:

```bash
mkdir -p /opt/gamon
cd /opt/gamon
sudo dpkg --add-architecture i386
sudo apt update
sudo apt install -y libc6-i386 lib32gcc-s1 lib32stdc++6
wget https://github.com/DarthBaobab/GAMON-Releases/releases/latest/download/GAMON-linux-x64.tar.gz -O GAMON-linux-x64.tar.gz
tar -xzf GAMON-linux-x64.tar.gz
chmod +x GAMON
chmod +x install-gamon-service.sh
./install-gamon-service.sh
```

### Windows per CLI installieren

Wenn du GAMON direkt per PowerShell installieren willst, kannst du das Release auch ohne Browser herunterladen und entpacken.

```powershell
New-Item -ItemType Directory -Force -Path C:\GAMON | Out-Null
Set-Location C:\GAMON
Invoke-WebRequest https://aka.ms/vc14/vc_redist.x64.exe -OutFile vc_redist.x64.exe
Start-Process .\vc_redist.x64.exe -Wait
Invoke-WebRequest https://github.com/DarthBaobab/GAMON-Releases/releases/latest/download/GAMON-win-x64.zip -OutFile GAMON-win-x64.zip
Expand-Archive .\GAMON-win-x64.zip -DestinationPath . -Force
.\Install-GAMON-Service.cmd
```

Die PowerShell sollte dafuer als Administrator gestartet werden.

Direkt per Command:

Windows:

```powershell
GAMON.exe service install
```

Linux:

```bash
./GAMON service install
```

Die alten Kurzbefehle `install` und `uninstall` bleiben als Alias weiterhin verfuegbar, die neuen `service install` und `service uninstall` sind aber eindeutiger.

Danach wird der Manager-Background-Service eingerichtet und gestartet.

Unter Windows muss die Shell dafuer mit Administratorrechten gestartet werden.

## Direkt starten

Wenn du GAMON ohne Service-Installation direkt im Vordergrund ausfuehren willst:

Windows:

```powershell
GAMON.exe run
```

Linux:

```bash
./GAMON run
```

Ohne expliziten Command startet GAMON ebenfalls normal im Vordergrund.

Ein normaler Start installiert keinen Background-Service.

Wenn bereits ein aktiver Background-Service laeuft, weist GAMON darauf hin und startet keine zweite Vordergrundinstanz.

## Update

Ein vorhandenes Release kann direkt ueber den eingebauten Update-Command aktualisiert werden:

Windows:

```powershell
GAMON.exe update
```

Linux:

```bash
./GAMON update
```

Der Update-Prozess laedt das neueste passende Release herunter, ersetzt die Dateien und startet den Manager danach erneut. Wenn ein Service installiert ist, wird dieser bevorzugt wieder gestartet.

## Deinstallation

Zum Entfernen des Manager-Service:

Windows:

```powershell
GAMON.exe service uninstall
```

Linux:

```bash
./GAMON service uninstall
```

Dabei wird nur der Manager-Service entfernt. Deine Daten und Konfigurationen im Installationsverzeichnis bleiben erhalten.

## Wichtige Commands

- `service install`
- `service uninstall`
- `service status`
- `install`
- `run`
- `update`
- `uninstall`
- `help`

## Hinweise

- Windows nutzt fuer den Manager-Service `WinSW`.
- Linux nutzt einen `systemd --user` Service.
- Der Web-Host lauscht standardmaessig auf `http://0.0.0.0:5000`.
- Details zu Aenderungen pro Version stehen in `CHANGELOG.md` sowie in den jeweiligen GitHub Release Notes.

## Fernzugriff

Wenn du GAMON nicht nur im lokalen Netzwerk, sondern auch von unterwegs erreichen willst, solltest du den HTTP-Port nicht einfach direkt ins Internet freigeben.

Empfohlene Reihenfolge:

1. Tailscale fuer einfache und sichere private Verbindungen
2. SSH-Tunnel fuer technischere Setups
3. Direkte Portfreigaben nur mit zusaetzlicher Absicherung wie Reverse Proxy, HTTPS und Login

### Variante 1: Tailscale

Tailscale baut ein privates, verschluesseltes Netzwerk zwischen deinen Geraeten auf. Fuer GAMON ist das meist der einfachste und sicherste Weg fuer Fernzugriff, weil der Manager dabei nicht offen im Internet stehen muss.

Typischer Ablauf:

1. Installiere Tailscale auf dem Rechner, auf dem GAMON laeuft.
2. Installiere Tailscale auch auf dem Geraet, mit dem du den Manager spaeter bedienen willst.
3. Melde beide Geraete im selben Tailscale-Netz an.
4. Starte GAMON wie gewohnt.
5. Oeffne GAMON ueber die Tailscale-IP des Server-Rechners, zum Beispiel `http://100.x.x.x:5000`.

Vorteile:

- Kein offener Router-Port noetig
- Verschluesselte Verbindung
- Sehr gut geeignet fuer Admin-Zugriff, kleine Teams und private Nutzung
- Bestehende GAMON-Weboberflaeche funktioniert in der Regel direkt weiter

Nachteile:

- Tailscale muss auf beiden Geraeten installiert werden
- Fuer komplett fremde Endnutzer ist es nicht so bequem wie eine normale oeffentliche Webseite

#### Tailscale unter Windows

1. Lade Tailscale von `https://tailscale.com/download` herunter und installiere es.
2. Melde dich mit deinem Tailscale-Konto an.
3. Pruefe im Tailscale-Client oder im Web-Adminbereich die IP des Rechners.
4. Starte GAMON oder den GAMON-Service.
5. Oeffne im Browser auf deinem anderen Tailscale-Geraet `http://<tailscale-ip>:5000`.

#### Tailscale unter Linux

Beispiel fuer Debian oder Ubuntu:

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

Danach:

1. Den angezeigten Login-Link im Browser oeffnen
2. Den Rechner im Tailscale-Netz freigeben
3. GAMON starten
4. GAMON ueber `http://<tailscale-ip>:5000` aufrufen

Hinweis:

- Wenn du nur ueber Tailscale zugreifen willst, ist das deutlich sicherer als eine direkte Freigabe ins Internet.

### Variante 2: SSH-Tunnel

Ein SSH-Tunnel ist ebenfalls sicher, aber eher fuer fortgeschrittene Nutzer geeignet. Dabei bleibt GAMON auf dem Zielrechner lokal, und dein Browser verbindet sich ueber einen verschluesselten Tunnel mit dem entfernten Port `5000`.

Voraussetzungen:

- Auf dem Zielsystem laeuft ein SSH-Server
- Du hast SSH-Zugang zum Rechner
- GAMON laeuft auf dem Zielrechner

Danach oeffnest du lokal in deinem Browser:

- `http://localhost:5000`

Der Traffic wird intern ueber SSH zum entfernten GAMON-Port weitergeleitet.

#### SSH-Tunnel von Windows oder Linux zu einem Linux-Server

```bash
ssh -L 5000:localhost:5000 USER@SERVER
```

Beispiel:

```bash
ssh -L 5000:localhost:5000 admin@203.0.113.10
```

Solange die SSH-Verbindung offen bleibt, erreichst du den entfernten GAMON lokal im Browser unter:

```text
http://localhost:5000
```

#### SSH-Tunnel mit Windows OpenSSH in PowerShell

```powershell
ssh -L 5000:localhost:5000 USER@SERVER
```

Wenn du PuTTY statt OpenSSH nutzt, richte unter `Connection > SSH > Tunnels` einen lokalen Tunnel ein:

- Source port: `5000`
- Destination: `localhost:5000`

Danach die SSH-Sitzung normal starten und im Browser `http://localhost:5000` oeffnen.

#### SSH-Tunnel als einfache Desktop-Verknuepfung

Fuer eigene private Nutzung kannst du dir eine kleine Verknuepfung oder ein Skript bauen, das den Tunnel mit einem Klick startet. Fuer komplett unerfahrene Nutzer ist Tailscale in der Praxis aber meist einfacher.

### Wann welche Variante sinnvoll ist

- Tailscale: beste Standardempfehlung fuer private Admin-Nutzung und kleine geschlossene Gruppen
- SSH-Tunnel: gut fuer Admins, Root-Server und Terminal-lastige Setups
- Direkte Internet-Freigabe: nur mit zusaetzlicher Web-Absicherung

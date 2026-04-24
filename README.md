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

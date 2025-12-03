# 📦 Raspberry PI NAS  
Homeserver mit Docker, Plex & WireGuard

Ein modularer und erweiterbarer Homeserver auf Basis eines Raspberry Pi 4.  
Dieses Projekt zeigt, wie man einen vollständigen privaten Server mit NAS-Funktion, Medienserver, VPN-Zugang und DynDNS betreibt – vollständig containerisiert über Docker.

---

## 🚀 Funktionen & Ziele

- Eigenes NAS für Dateien (Samba)
- Plex Media Server für Filme/Serien im LAN & über VPN
- Sicherer externer Zugriff via WireGuard
- DynDNS über DuckDNS (stabile Domain trotz wechselnder IP)
- Docker-Container für Updates & Wiederholbarkeit
- Medien- & Datenspeicher auf externer SSD
- Erweiterbar um HomeAssistant, Nextcloud, Backups uvm.

Der Fokus liegt auf **Wartbarkeit, Erweiterbarkeit und Reproduzierbarkeit**.

---

## 🏗️ Systemarchitektur
```
[Smartphone / Laptop]
│ (WireGuard VPN)
▼
[DuckDNS Domain → Router Port 51820]
▼
[Raspberry Pi]
├── Samba (NAS)
├── Plex Media Server
├── WireGuard (VPN)
├── DuckDNS Updater
└── HomeAssistant (optional)
▼
[1 TB SSD Speicher]
```

---

## 🧩 Verwendete Komponenten

| Komponente              | Aufgabe |
|------------------------|---------|
| Raspberry Pi 4         | Servereinheit |
| microSD 64 GB          | Betriebssystem |
| Samsung SSD T7, 1 TB   | Medien- & Datenspeicher |
| Docker / Docker Compose| Container-Orchestrierung |
| Plex                   | Medienserver |
| WireGuard              | VPN-Zugriff |
| DuckDNS                | DynDNS-Service |
| Samba                  | Dateifreigaben im LAN |

---

## 📁 Verzeichnisstruktur (SSD)

```txt
/mnt/ssd/docker/
├── samba/
│   ├── docker-compose.yml
│   ├── .env
│   └── share/
├── plex/
│   ├── docker-compose.yml
│   ├── .env
│   ├── config/
│   └── media/
├── wireguard/
│   ├── docker-compose.yml
│   └── config/
└── duckdns/
    ├── docker-compose.yml
    └── .env
```


## 🛠️ Einrichtungsschritte (Kurzversion)

### 1) Raspberry Pi vorbereiten

-Raspberry Pi OS Lite (64-bit) installieren
-SSH aktivieren
-Benutzer & Hostname setzen
-System aktualisieren

### 2) Statische IP vergeben
Über /etc/dhcpcd.conf oder Router-Reservierung.

### 3) Docker installieren
```
curl -sSL https://get.docker.com | sh
sudo usermod -aG docker $USER
```

### 4) SSD einbinden

ext4 formatieren

unter /mnt/ssd mounten

Eintrag in /etc/fstab hinzufügen

### 5) Docker-Verzeichnisse erstellen
Alle Container laufen persistiert auf der SSD.

## 🧰 Docker Services (Überblick)
### ✔ Samba NAS (Dateifreigabe)

Freigaben:
Public
Plex (Medienordner)
Anmeldung über .env Datei (keine Passwörter im Klartext).

Windows-Zugriff:
\\192.168.0.XXX\Plex
\\192.168.0.XXX\Public

### ✔ DuckDNS (DynDNS)

Automatische IP-Aktualisierung für z. B.
beispiel-pi.duckdns.org

Ermöglicht stabilen VPN-Zugriff von außen.
http://192.168.0.XXX:32400/web

### ✔ WireGuard VPN

Sicherer externer Zugriff auf NAS & Plex
Konfiguration für Smartphone erzeugt automatisch QR-Code

Extern erreichbar unter:
beispiel-pi.duckdns.org:51820

## 🌐 Externer Zugriff über VPN

###Vorteile:
-Keine Ports (Plex/Samba) werden ins Internet geöffnet
-Zugriff erfolgt ausschließlich über den VPN-Tunnel
-Sehr schnell und sehr sicher
-Funktioniert perfekt auf Smartphones und Laptops

##🔧 Erweiterbarkeit

Das System lässt sich leicht erweitern um:
HomeAssistant (SmartHome)
Nextcloud (Cloud-Speicher)
Paperless NGX (Dokumentenmanagement)
MQTT / Sensor-Netzwerke
Backups (rsync, BorgBackup)
Monitoring (Grafana + Prometheus)

## 🧪 Troubleshooting Highlights
### ❗ SSD wird nicht erkannt / I/O-Fehler

Raspberry-Pi-USB-3 Controller kann unter Last instabil sein

Lösung:
--SSD über Windows löschen → erneut als ext4 formatieren
--alternativ: USB-2 oder aktiver USB-Hub

### ❗ Plex kann Medien nicht einlesen
Rechts setzen nicht vergessen:
```
sudo chown -R 1000:1000 /mnt/ssd/docker/plex/media
sudo chmod -R 775 /mnt/ssd/docker/plex/media
```

## ✅ Fazit

### Dieser Raspberry-Pi-NAS bietet:
Medienserver
NAS
VPN-geschützten Fernzugriff
DynDNS
Docker-basierte Struktur für maximale Stabilität
Ein ideales Lernprojekt für Linux, Netzwerke, Storage & Containerisierung.






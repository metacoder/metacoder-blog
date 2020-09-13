---
title: "Manuell zu drahtlosen Netzwerk verbinden"
authors:
  - Nis Meinert
date: 2014-02-24T18:27:17+02:00
lastmod: 2014-02-24T18:30:43+02:00
---

Ich nutze momentan mein altes Netbook zum experimentieren als Server und habe deswegen ein minimales Debian installiert. Um es nicht schon im Leerlauf zu überlasten habe ich keinen X-Server oder etwas ähnliches installiert, welches mir einen Networkmanager bescheren könnte und war daher gezwungen die Verbindung zu meinem lokalen WLAN manuell aufzubauen.

Da die Verbindung automatisch bei jedem Systemstart aufgebaut werden soll, habe ich mir zwei Skripte _wlan-start.py_ und _wlan-stop.py_ geschrieben welche ich in einer **/etc/init.d/wlan** aufrufe:

```bash
#!/bin/bash
#/etc/init.d/wlan

case "$1" in
    start)
        /root/scripts/wlan-start.py
        ;;
    stop)
        /root/scripts/wlan-stop.py
        ;;
    *)
        echo "Usage: /etc/init.d/wlan {start|stop}"
        exit 1
        ;;
esac

exit 0
```

Ein anschließendendes:

```plain {linenos=false}
# update-rc.d wlan defaults
```

sorgt für das Aufrufen beim Systemstart.

Die Zugangsdaten der wpa2 Netze speichere ich unter **/etc/wlan/SSID.conf** wobei SSID gegen die entsprechende WLAN-SSID zu ersetzen ist:

```python
#!/usr/bin/python3

import os

ssid = input("SSID: ")
password = input("Password: ")

os.system("wpa_passphrase %s %s > /etc/wlan/%s.conf" %(ssid, password, ssid))
print("Config file was written to: /etc/wlan/%s.conf" %ssid)
```

(_wpa_passphrase_ aus Paket: _wpasupplicant_)  
Das _wlan-start.py_-Skript muss nun nur noch die gefundenen mit den _bekannten_ SSIDs aus dem _/etc/wlan_-Verzeichnis abgleichen und ggf. die entsprechende _.conf_-Datei laden.

```python
#!/usr/bin/python3

import os
import subprocess
import sys

FNULL = open(os.devnull, 'w')

print("Searching for known wireless networks...")
conffile = ""
try:
    subprocess.check_call(["ifconfig", "wlan0", "up"], stdout=FNULL, stderr=subprocess.STDOUT)
    output = subprocess.check_output(["iwlist", "wlan0", "scan"], universal_newlines=True)
    ssids = [line.strip()[7:-1] for line in output.split('\n') if 'ESSID:' in line]
    for ssid in ssids:
        f = "/etc/wlan/" + ssid + ".conf";
        if os.path.isfile(f):
            conffile = f
            break
    if conffile == "":
        print("No known wireless networks found")
        sys.exit(0)
    print("Found network: %s" %ssid)
except:
    print("An error occured while searching for wireless networks.")
    sys.exit(1)

print("Connecting...")
try:
    subprocess.check_call(["wpa_supplicant", "-B", "-iwlan0", "-c" + conffile, "-Dwext"], stdout=FNULL, stderr=subprocess.STDOUT)
    subprocess.check_call(["dhclient", "wlan0"], stdout=FNULL, stderr=subprocess.STDOUT)
except:
    print("An error occured. Cannot connect to wireless network.")
else:
    print("Successfully connected to %s" %ssid)
```

(Für den Scann-Vorgang mit _iwlist_ wird das Paket _wireless-tools_ benötigt.)

Der vollständigkeitshalber hier auch noch das _wlan-stop.py_-Skript:

```python
#!/usr/bin/python3

import subprocess

try:
    subprocess.check_call(["dhclient", "-r", "wlan0"])
    subprocess.check_call(["ifconfig", "wlan0", "down"])
except:
    print("An error occured while shutting down wlan0")
else:
    print("Shutting down wlan0 was successful")
```

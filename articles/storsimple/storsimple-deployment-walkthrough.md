---
title: "Bereitstellen lokaler StorSimple-Geräte | Microsoft Docs"
description: "Beschreibt die Schritte und bewährten Methoden für die Bereitstellung des StorSimple-Geräts und -Diensts. (Gilt für Microsoft Azure StorSimple Version 0.3 und frühere Versionen.)"
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: b27f87a2-1363-4e0d-90f7-37b5dd1f21c9
ms.service: storsimple
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/17/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: a14be6bf8135cdfc7e5b9eb3b6d7af3bdd4561a7


---
# <a name="deploy-your-on-premises-storsimple-device"></a>Bereitstellen lokaler StorSimple-Geräte
> [!div class="op_single_selector"]
> * [Update 2](storsimple-deployment-walkthrough-u2.md)
> * [Update 1](storsimple-deployment-walkthrough-u1.md)
> * [GA-Version](storsimple-deployment-walkthrough.md)
> 
> 

## <a name="overview"></a>Übersicht
Willkommen bei der exemplarischen Vorgehensweise für die Bereitstellung von Microsoft Azure StorSimple-Geräten. Diese Bereitstellungstutorials beziehen sich auf die Freigabeversion der StorSimple 8000-Serie, StorSimple 8000-Serie Update 0.1, StorSimple 8000-Serie Update 0.2 und StorSimple 8000-Serie Update 0.3. Diese Reihe von Tutorials beschreibt die Konfiguration Ihrer StorSimple-Geräte. Sie enthält darüber hinaus eine Konfigurationsprüfliste, Konfigurationsvoraussetzungen und detaillierte Konfigurationsschritte.

Bei den Informationen in diesen Tutorials wird davon ausgegangen, dass Sie die Sicherheitsvorkehrungen geprüft und Ihr StorSimple-Gerät ausgepackt, installiert und verkabelt haben. Wenn Sie diese Aufgaben noch ausführen müssen, beginnen Sie mit der Prüfung der [Sicherheitsvorkehrungen](storsimple-safety.md). Je nach Modell können Sie anschließend das Gerät auspacken, im Rack einbauen und verkabeln, indem Sie folgende Anweisungen befolgen:

* [Auspacken, Einsetzen und Verkabeln – 8100](storsimple-8100-hardware-installation.md)
* [Auspacken, Einsetzen und Verkabeln – 8600](storsimple-8600-hardware-installation.md)

Sie benötigen Administratorrechte, um die Einrichtung und Konfiguration abzuschließen. Es wird empfohlen, dass Sie die Konfigurationsprüfliste überprüfen, bevor Sie beginnen. Die Bereitstellung und Konfiguration kann einige Zeit in Anspruch nehmen.

> [!NOTE]
> Die auf der Microsoft Azure-Website veröffentlichten StorSimple-Bereitstellungsinformationen gelten nur für Geräte der StorSimple 8000-Reihe. Vollständige Informationen zu Geräten der Serien 5000 und 7000 finden Sie unter: [http://onlinehelp.storsimple.com/](http://onlinehelp.storsimple.com). Bereitstellungsinformationen zu den Serien 5000 und 7000 finden Sie im [StorSimple System Quick Start Guide](http://onlinehelp.storsimple.com/111_Appliance/)(Schnellstartanleitung zum StorSimple-System).
> 
> 

## <a name="deployment-steps"></a>Bereitstellungsschritte
Führen Sie die folgenden erforderlichen Schritte zum Konfigurieren Ihres StorSimple-Geräts und zum Verbinden des Geräts mit dem StorSimple Manager-Dienst aus. Zusätzlich zu diesen erforderlichen Schritten müssen Sie während der Bereitstellung möglicherweise optionale Schritte ausführen. Die schrittweise Bereitstellungsanleitung gibt auch an, wann Sie die folgenden optionalen Schritte jeweils ausführen sollten.

| Schritt | Beschreibung |
| --- | --- |
| **VORAUSSETZUNGEN** |Diese Schritte müssen zur Vorbereitung der Bereitstellung ausgeführt werden. |
| Konfigurationsprüfliste für die Bereitstellung. |Verwenden Sie diese Prüfliste, um vor und während der Bereitstellung Informationen zu erfassen und aufzuzeichnen. |
| Voraussetzungen für die Bereitstellung. |Mit diesen Schritten überprüfen Sie, ob die Umgebung für die Bereitstellung vorbereitet ist. |
|  | |
| **SCHRITTWEISE BEREITSTELLUNG** |Diese Schritte sind zur Bereitstellung Ihres StorSimple-Geräts in einer Produktionsumgebung erforderlich. |
| Schritt 1: Erstellen Sie einen neuen Dienst. |Richten Sie Cloudverwaltung und -speicher für Ihr StorSimple-Gerät ein. Überspringen Sie diesen Schritt, wenn Sie bereits über einen Dienst für andere StorSimple-Geräte verfügen. |
| Schritt 2: Rufen Sie den Dienstregistrierungsschlüssel ab. |Verwenden Sie diesen Schlüssel zum Registrieren Ihres StorSimple-Geräts sowie zum Herstellen einer Verbindung mit dem Verwaltungsdienst. |
| Schritt 3: Konfigurieren und registrieren Sie das Gerät über Windows PowerShell für StorSimple. |Verwenden Sie den Verwaltungsdienst, um das Gerät zum Abschluss der Einrichtung mit Ihrem Netzwerk zu verbinden und in Azure zu registrieren. |
| Schritt 4: Schließen Sie die minimale Gerätekonfiguration ab.</br>Optional: Aktualisieren Sie Ihr StorSimple-Gerät. |Verwenden Sie den Verwaltungsdienst, um die Gerätekonfiguration abzuschließen und es für die Bereitstellung von Speicher zu aktivieren. |
| Schritt 5: Erstellen Sie einen Volumecontainer. |Erstellen Sie einen Container zum Bereitstellen von Volumes. Ein Volumecontainer verfügt über Einstellungen für das Speicherkonto, die Bandbreite und die Verschlüsselung für alle enthaltenen Volumes. |
| Schritt 6: Erstellen Sie ein Volume. |Stellen Sie ein oder mehrere Speichervolumes für Ihre Server auf dem StorSimple-Gerät bereit. |
| Schritt 7: Stellen Sie ein Volume bereit, und initialisieren und formatieren Sie es.</br>Optional: Konfigurieren Sie MPIO. |Verbinden Sie Ihre Server mit dem vom Gerät bereitgestellten iSCSI-Speicher. Konfigurieren Sie optional MPIO, um sicherzustellen, dass Ihre Server Link-, Netzwerk- und Schnittstellenfehler tolerieren können. |
| Schritt 8: Erstellen Sie eine Sicherung. |Richten Sie Ihre Sicherungsrichtlinie zum Schutz Ihrer Daten ein. |
|  | |
| **WEITERE VERFAHREN** |Möglicherweise müssen Sie beim Bereitstellen Ihrer Lösung auf diese Prozeduren zurückgreifen. |
| Konfigurieren Sie ein neues Speicherkonto für den Dienst. | |
| Verwenden Sie PuTTY, um eine Verbindung mit der seriellen Gerätekonsole herzustellen. | |
| Rufen Sie den IQN eines Windows Server-Hosts ab. | |
| Erstellen Sie eine manuelle Sicherung. | |

## <a name="deployment-configuration-checklist"></a>Konfigurationsprüfliste für die Bereitstellung
Die folgende Konfigurationsprüfliste für die Bereitstellung enthält die Informationen, die Sie vor und während der Konfiguration der Software auf dem StorSimple-Gerät erfassen müssen. Wenn Sie einige dieser Informationen im Voraus erfassen, beschleunigt dies die Bereitstellung des StorSimple-Geräts in Ihrer Umgebung. Verwenden Sie diese Prüfliste auch, um während der Bereitstellung des Geräts die Konfigurationsdetails zu notieren.

| Phase | Parameter | Details | Werte |
| --- | --- | --- | --- |
| **Verkabeln Ihres Geräts** |Serieller Zugriff |Anfängliche Gerätekonfiguration |Ja/Nein |
|  | | | |
| **Konfigurieren und Registrieren des Geräts** |DATA 0-Netzwerkeinstellungen |DATA 0-IP-Adresse:</br>Subnetzmaske:</br>Gateway:</br>Primärer DNS-Server:</br>Primärer NTP-Server:</br>IP/FQDN des Webproxyservers (optional):</br>Webproxyport: | |
| &nbsp; |Geräteadministratorkennwort |Das Kennwort muss zwischen 8 und 15 Zeichen lang sein und Kleinbuchstaben, Großbuchstaben, Ziffern und Sonderzeichen umfassen. | |
| &nbsp; |StorSimple Snapshot Manager-Kennwort |Das Kennwort muss 14 oder 15 Zeichen lang sein und Kleinbuchstaben, Großbuchstaben, Ziffern und Sonderzeichen umfassen. | |
| &nbsp; |Dienstregistrierungsschlüssel |Dieser Schlüssel wird im klassischen Azure-Portal generiert. | |
| &nbsp; |Verschlüsselungsschlüssel für Dienstdaten |Dieser Schlüssel wird erstellt, wenn das Gerät über die Windows PowerShell für StorSimple beim Verwaltungsdienst registriert wird. Kopieren Sie diesen Schlüssel, und bewahren Sie ihn an einem sicheren Ort auf. | |
|  | | | |
| **Abschließen der minimalen Gerätekonfiguration** |Anzeigename für Ihr Gerät |Dies ist ein beschreibender Name für das Gerät. | |
| &nbsp; |Zeitzone |Ihr Gerät verwendet diese Zeitzone für alle geplanten Vorgänge. | |
| &nbsp; |Sekundärer DNS-Server |Dies ist eine erforderliche Konfiguration. | |
| &nbsp; |Netzwerkschnittstelle: feste IP-Adressen für DATA 0-Controller |Diese IP-Adressen sollten über das Internet geroutet werden können.</br>Feste IP-Adresse Controller 0:</br>Feste IP-Adresse Controller 1: | |
|  | | | |
| **Zusätzliche Netzwerkschnittstelleneinstellungen** |Netzwerkschnittstelle: DATA 1</br>Wenn iSCSI aktiviert ist, Gateway nicht konfigurieren |Zweck: Cloud/iSCSI/Nicht verwendet</br>IP-Adresse:</br>Subnetzmaske:</br>Gateway: | |
| &nbsp; |Netzwerkschnittstelle: DATA 2</br>Wenn iSCSI aktiviert ist, Gateway nicht konfigurieren |Zweck: Cloud/iSCSI/Nicht verwendet</br>IP-Adresse:</br>Subnetzmaske:</br>Gateway: | |
| &nbsp; |Netzwerkschnittstelle: DATA 3</br>Wenn iSCSI aktiviert ist, Gateway nicht konfigurieren |Zweck: Cloud/iSCSI/Nicht verwendet</br>IP-Adresse:</br>Subnetzmaske:</br>Gateway: | |
| &nbsp; |Netzwerkschnittstelle: DATA 4</br>Wenn iSCSI aktiviert ist, Gateway nicht konfigurieren |Zweck: Cloud/iSCSI/Nicht verwendet</br>IP-Adresse:</br>Subnetzmaske:</br>Gateway: | |
| &nbsp; |Netzwerkschnittstelle: DATA 5</br>Wenn iSCSI aktiviert ist, Gateway nicht konfigurieren |Zweck: Cloud/iSCSI/Nicht verwendet</br>IP-Adresse:</br>Subnetzmaske:</br>Gateway: | |
|  | | | |
| **Erstellen eines Volumecontainers** |Volumecontainername: |Name für den Container. | |
| &nbsp; |Azure-Speicherkonto: |Name und Zugriffsschlüssel des Speicherkontos, das diesem Volumecontainer zugeordnet werden soll. | |
| &nbsp; |Verschlüsselungsschlüssel für Cloudspeicher: |Verschlüsselungsschlüssel für Speicher in jedem Container. | |
|  | | | |
| **Erstellen eines Volumes** |Details zu jedem Volume |Volumename: | |
| &nbsp; |&nbsp; |Größe: | |
| &nbsp; |&nbsp; |Verwendungstyp: | |
| &nbsp; |&nbsp; |ACR-Name: | |
| &nbsp; |&nbsp; |Standardsicherungsrichtlinie: | |
|  | | | |
| **Bereitstellen, Initialisieren und Formatieren eines Volumes** |Details zu jedem Hostserver, der eine Verbindung mit dem Speicher herstellt |Windows Server-Name: | |
| &nbsp; |&nbsp; |Windows Server-IQN: | |
| &nbsp; |&nbsp; |Windows Server-Volumename: | |
| &nbsp; |&nbsp; |NTFS-Bereitstellungspunkt/Laufwerkbuchstabe: | |

## <a name="deployment-prerequisites"></a>Voraussetzungen für die Bereitstellung
In den folgenden Abschnitten werden die Konfigurationsvoraussetzungen für den StorSimple Manager-Dienst, das StorSimple-Gerät und das Netzwerk in Ihrem Datencenter beschrieben.

### <a name="for-the-storsimple-manager-service"></a>Für den StorSimple-Manager-Dienst
Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Sie besitzen ein Microsoft-Konto mit Anmeldeinformationen für den Zugriff.
* Sie besitzen ein Microsoft Azure-Speicherkonto mit Anmeldeinformationen für den Zugriff.
* Ihr Microsoft Azure-Abonnement ist für den StorSimple Manager-Dienst aktiviert. Ihr Abonnement sollten Sie über das [Enterprise Agreement](https://azure.microsoft.com/pricing/enterprise-agreement/)erwerben.
* Sie haben Zugriff auf Terminalemulationssoftware wie PuTTY.

### <a name="for-the-device-in-the-datacenter"></a>Für das Gerät im Datencenter
Stellen Sie vor der Konfiguration des Geräts Folgendes sicher:

* Ihr Gerät wurde vollständig ausgepackt, in einem Rack installiert und für Stromversorgung, Netzwerk- und seriellen Zugriff verkabelt, wie in folgenden Themen beschrieben:
  
  * [Auspacken, Einsetzen und Verkabeln – 8100-Gerät](storsimple-8100-hardware-installation.md)
  * [Auspacken, Einsetzen und Verkabeln – 8600-Gerät](storsimple-8600-hardware-installation.md)

### <a name="for-the-network-in-the-datacenter"></a>Für das Netzwerk im Datencenter
Stellen Sie Folgendes sicher, bevor Sie beginnen:

* Die Ports in der Datencenter-Firewall werden geöffnet, um iSCSI- und Clouddatenverkehr zu ermöglichen, wie in [Netzwerkanforderungen für das StorSimple-Gerät](storsimple-system-requirements.md#networking-requirements-for-your-storsimple-device)beschrieben.
* Das Gerät in Ihrem Datencenter kann eine Verbindung zum externen Netzwerk herstellen. Führen Sie folgende [Windows PowerShell 4.0](http://www.microsoft.com/download/details.aspx?id=40855) -Cmdlets (siehe unten stehende Tabelle) aus, um die Verbindung zum externen Netzwerk zu überprüfen. Führen Sie diese Überprüfung auf einem Computer (im Datencenternetzwerk) mit Verbindung zu Azure und an dem Standort durch, an dem Sie das StorSimple-Gerät bereitstellen.  

| Parameter | Gültigkeitsprüfung | Ausführung dieser Befehle/Cmdlets |
| --- | --- | --- |
| **IP**</br>**Subnetz**</br>**Gateway** |Ist dies eine gültige IPv4- oder IPv6-Adresse?</br>Ist dies ein gültiges Subnetz?</br>Ist dies ein gültiges Gateway?</br>Ist dies eine doppelte IP im Netzwerk? |`ping ip`</br>`arp -a`</br>Die Befehle `ping` und `arp` sollten einen Fehler zurückgeben mit dem Hinweis, dass kein Gerät im Datencenternetzwerk diese IP-Adresse verwendet. |
|  | | |
| **DNS** |Ist dies ein gültiges DNS, das Azure-URLs auflösen kann? |`Resolve-DnsName -Name www.bing.com -Server <DNS server IP address>` </br>Alternativ kann folgender Befehl verwendet werden:</br>`nslookup --dns-ip=<DNS server IP address> www.bing.com` |
| &nbsp; |Prüfen Sie, ob Port 53 offen ist. Dies gilt nur, wenn Sie ein externes DNS für Ihr Gerät verwenden. Das interne DNS sollte die externen URLs automatisch auflösen. |`Test-Port -comp dc1 -port 53 -udp -UDPtimeout 10000`  </br>[Weitere Informationen zu diesem Cmdlet](http://learn-powershell.net/2011/02/21/querying-udp-ports-with-powershell/) |
|  | | |
| **NTP** |Wir lösen eine Zeitsynchronisierung aus, sobald der NTP-Server eingegeben wird. Prüfen Sie, ob UDP-Port 123 offen ist, wenn Sie `time.windows.com` oder öffentliche Zeitserver eingeben. |[Laden Sie dieses Skript herunter, und verwenden Sie es](https://gallery.technet.microsoft.com/scriptcenter/Get-Network-NTP-Time-with-07b216ca). |
|  | | |
| **Proxy (optional)** |Ist dies ein gültiger Proxy-URI  und -Port? </br>  Ist der Authentifizierungsmodus korrekt? |<code>wget http://bing.com &#124; % {$_.StatusCode}</code></br>Dieser Befehl sollte unmittelbar nach der Konfiguration des Webproxys ausgeführt werden. Wenn der Statuscode "200" zurückgegeben wird, weist dies darauf hin, dass die Verbindung erfolgreich hergestellt wurde. |
| &nbsp; |Kann der Datenverkehr durch den Proxy geroutet werden? |Führen Sie die DNS-, NTP- oder HTTP-Überprüfung einmal nach der Konfiguration der Proxyeinstellungen auf Ihrem Gerät aus. So erhalten Sie genaue Informationen darüber, ob Datenverkehr im Proxy oder an anderer Stelle blockiert wird. |
|  | | |
| **Registrierung** |Prüfen Sie, ob die ausgehenden TCP-Ports 443, 80 und 9354 offen sind. |`Test-NetConnection -Port   443 -InformationLevel Detailed`</br>[Weitere Informationen zum Cmdlet "Test-NetConnection"](https://technet.microsoft.com/library/dn372891.aspx) |

## <a name="step-by-step-deployment"></a>SCHRITTWEISE BEREITSTELLUNG
Verwenden Sie die folgenden schrittweisen Anweisungen, um Ihr StorSimple-Gerät im Datencenter bereitzustellen.

## <a name="step-1-create-a-new-service"></a>Schritt 1: Erstellen eines neuen Diensts
Ein StorSimple-Manager-Dienst kann mehrere StorSimple-Geräte verwalten. Für die Bereitstellung Ihres ersten StorSimple-Geräts müssen Sie einen neuen StorSimple Manager-Dienst erstellen.

> [!IMPORTANT]
> Überspringen Sie diesen Schritt, wenn Sie bereits über einen StorSimple Manager-Dienst verfügen und Ihr StorSimple-Gerät mit diesem Dienst bereitstellen möchten.
> 
> 

Führen Sie die folgenden Schritte aus, um einen neuen StorSimple-Manager-Dienst zu erstellen.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

> [!IMPORTANT]
> Wenn Sie nicht die automatische Erstellung eines Speicherkontos mit Ihrem Dienst aktiviert haben, müssen Sie mindestens ein Speicherkonto erstellen, nachdem Sie einen Dienst erstellt haben. Dieses Speicherkonto wird beim Erstellen eines Volumecontainers verwendet.
> 
> Wenn Sie nicht automatisch ein Speicherkonto erstellt haben, finden Sie unter [Konfigurieren eines neuen Speicherkontos für den Dienst](#configure-a-new-storage-account-for-the-service) ausführliche Anweisungen.
> Wenn Sie die automatische Erstellung eines Speicherkontos aktiviert haben, fahren Sie mit [Schritt 2: Abrufen des Dienstregistrierungsschlüssels](#step-2:-get-the-service-registration-key)fort.
> 
> 

## <a name="step-2-get-the-service-registration-key"></a>Schritt 2: Abrufen des Dienstregistrierungsschlüssels
Nachdem der StorSimple Manager-Dienst eingerichtet wurde und ausgeführt wird, müssen Sie den Dienstregistrierungsschlüssel abrufen. Dieser Schlüssel wird zum Registrieren Ihres StorSimple-Geräts sowie zum Herstellen einer Verbindung mit dem Dienst verwendet.

Führen Sie die folgenden Schritte im klassischen Azure-Portal aus.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

## <a name="step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple"></a>Schritt 3: Konfigurieren und Registrieren des Geräts über Windows PowerShell für StorSimple
> [!IMPORTANT]
> Trennen Sie vor dem Durchführen dieser Konfiguration die Verbindungen aller Netzwerkschnittstellen außer DATA 0 auf beiden Controllern (dem aktiven und dem passiven).
> 
> 

Verwenden Sie Windows PowerShell für StorSimple zum Abschließen der anfänglichen Installation Ihres StorSimple-Geräts wie im folgenden Verfahren erläutert. Sie müssen eine Terminalemulationssoftware verwenden, um diesen Schritt auszuführen. Weitere Informationen finden Sie unter [Verwenden von PuTTY für das Herstellen einer Verbindung mit der seriellen Gerätekonsole](#use-putty-to-connect-to-the-device-serial-console).

[!INCLUDE [storsimple-configure-and-register-device](../../includes/storsimple-configure-and-register-device.md)]

## <a name="step-4-complete-minimum-device-setup"></a>Schritt 4: Schließen Sie die minimale Gerätekonfiguration ab.
Sie müssen für die Gerätemindestkonfiguration des StorSimple-Geräts die folgenden Aufgaben ausführen:

* Einrichten des sekundären DNS-Servers
* Aktivieren von iSCSI an mindestens einer Netzwerkschnittstelle
* Zuweisen fester IP-Adressen an beide Controller

Führen Sie die folgenden Schritte im klassischen Azure-Portal aus, um die minimale Gerätekonfiguration auszuführen.

[!INCLUDE [storsimple-complete-minimum-device-setup](../../includes/storsimple-complete-minimum-device-setup.md)]

Nachdem die Gerätekonfiguration abgeschlossen ist, müssen Sie nach Updates suchen und diese installieren, sofern vorhanden. Die Installation der Updates kann mehrere Stunden dauern. Befolgen Sie die Anweisungen in [Suchen nach und Anwenden von Updates](#scan-for-and-apply-updates).

## <a name="step-5-create-a-volume-container"></a>Schritt 5: Erstellen eines Volumecontainers
Ein Volumecontainer verfügt über Einstellungen für das Speicherkonto, die Bandbreite und die Verschlüsselung für alle darin enthaltenen Volumes. Sie müssen einen Volumecontainer erstellen, bevor Sie mit der Bereitstellung von Volumes auf Ihrem StorSimple-Gerät beginnen können.

Führen Sie die folgenden Schritte im klassischen Azure-Portal aus, um einen Volumecontainer zu erstellen.

[!INCLUDE [storsimple-create-volume-container](../../includes/storsimple-create-volume-container.md)]

## <a name="step-6-create-a-volume"></a>Schritt 6: Erstellen eines Volumes
Nach dem Erstellen eines Volumecontainers können Sie ein Speichervolume auf dem StorSimple-Gerät für Ihre Server bereitstellen. Führen Sie die folgenden Schritte im klassischen Azure-Portal aus, um ein Volume zu erstellen.

> [!IMPORTANT]
> Mit StorSimple Manager können nur mit schlanker Speicherzuweisung bereitgestellte Volumes erstellt werden.  Sie können keine vollständig oder teilweise bereitgestellten Volumes erstellen.
> 
> 

[!INCLUDE [storsimple-create-volume](../../includes/storsimple-create-volume.md)]

## <a name="step-7-mount-initialize-and-format-a-volume"></a>Schritt 7: Bereitstellen, Initialisieren und Formatieren eines Volumes
> [!IMPORTANT]
> * Um die hohe Verfügbarkeit Ihrer StorSimple-Lösung sicherzustellen, empfiehlt es sich, vor der Konfiguration von iSCSI auf Ihrem Windows Server-Host optional MPIO auf diesem Host zu konfigurieren. Die MPIO-Konfiguration auf Hostservern sorgt dafür, dass die Server Fehler bei Links, dem Netzwerk oder einzelnen Schnittstellen tolerieren können.
> * Installations- und Konfigurationsanweisungen für MPIO und iSCSI finden Sie unter [Konfigurieren von MPIO für Ihr StorSimple-Gerät](storsimple-configure-mpio-windows-server.md). Diese enthalten auch die Schritte zum Bereitstellen, Initialisieren und Formatieren von StorSimple-Volumes.
> 
> 

Wenn Sie sich dafür entscheiden, MPIO nicht zu konfigurieren, führen Sie die folgenden Schritte aus, um Ihre StorSimple-Volumes bereitzustellen, zu initialisieren und zu formatieren.

[!INCLUDE [storsimple-mount-initialize-format-volume](../../includes/storsimple-mount-initialize-format-volume.md)]

## <a name="step-8-take-a-backup"></a>Schritt 8: Erstellen einer Sicherung
Sicherungen stellen Zeitpunktschutz für Volumes zur Verfügung und verbessern die Wiederherstellbarkeit bei gleichzeitiger Minimierung der Wiederherstellungszeiten. Für Ihr StorSimple-Gerät können zwei Arten von Sicherungen angefertigt werden: lokale Momentaufnahmen und Cloudmomentaufnahmen. Jeder dieser Sicherungstypen kann **geplant** sein oder **manuell** erfolgen.

Führen Sie die folgenden Schritte im klassischen Azure-Portal aus, um eine geplante Sicherung zu erstellen.

[!INCLUDE [storsimple-take-backup](../../includes/storsimple-take-backup.md)]

Sie können jederzeit eine manuelle Sicherung vornehmen. Anweisungen dazu finden Sie unter [Erstellen einer manuellen Sicherung](#Create-a-manual-backup).

## <a name="configure-a-new-storage-account-for-the-service"></a>Konfigurieren eines neuen Speicherkontos für den Dienst
Dies ist ein optionaler Schritt, den Sie nur dann ausführen müssen, wenn Sie nicht die automatische Erstellung eines Speicherkontos mit Ihrem Dienst aktiviert haben. Ein Microsoft Azure-Speicherkonto ist erforderlich, um einen StorSimple-Volumecontainer zu erstellen.

Wenn Sie ein Azure-Speicherkonto in einer anderen Region erstellen müssen, finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md) schrittweise Anweisungen.

Führen Sie die folgenden Schritte im klassischen Azure-Portal auf der Seite **StorSimple Manager-Dienst** aus.

[!INCLUDE [storsimple-configure-new-storage-account](../../includes/storsimple-configure-new-storage-account.md)]

## <a name="use-putty-to-connect-to-the-device-serial-console"></a>Verwenden von PuTTY für das Herstellen einer Verbindung mit der seriellen Gerätekonsole
Zum Herstellen einer Verbindung mit Windows PowerShell für StorSimple benötigen Sie eine Terminalemulationssoftware wie etwa PuTTY. Sie können PuTTY verwenden, wenn Sie über die serielle Konsole direkt auf das Gerät zugreifen oder eine Telnet-Sitzung von einem Remotecomputer aus öffnen.

[!INCLUDE [Use PuTTY to connect to the device serial console](../../includes/storsimple-use-putty.md)]

## <a name="scan-for-and-apply-updates"></a>Suchen nach und Anwenden von Updates
Die Aktualisierung eines Geräts kann zwischen einer und vier Stunden dauern. Führen Sie die folgenden Schritte aus, um nach Updates für Ihr Gerät zu suchen und diese anzuwenden.

> [!NOTE]
> Wenn das Gateway für eine andere Netzwerkschnittstelle als DATA 0 konfiguriert ist, müssen Sie die Netzwerkschnittstellen DATA 2 und DATA 3 deaktivieren, bevor Sie das Update installieren. Wechseln Sie zu **Geräte > Konfigurieren**, und deaktivieren Sie die Schnittstellen DATA 2 und DATA 3. Sie sollten diese Schnittstellen nach der Aktualisierung des Geräts wieder aktivieren.
> 
> 

#### <a name="to-update-your-device"></a>So aktualisieren Sie Ihr Gerät
1. Klicken Sie auf der Seite **Schnellstart** für das Gerät auf **Geräte**. Wählen Sie das physische Gerät aus, und klicken Sie auf **Wartung** und dann auf **Updates scannen**.  
2. Für die Suche nach verfügbaren Updates wird ein Auftrag erstellt. Wenn Updates verfügbar sind, ändert sich die Option **Updates scannen** in **Updates installieren**. Klicken Sie auf **Updates installieren**. Möglicherweise werden Sie dazu aufgefordert, DATA 2 und DATA 3 vor der Installation der Updates zu deaktivieren. Sie müssen diese Netzwerkschnittstellen deaktivieren, da die Updates sonst nicht installiert werden können.
3. Es wird ein Updateauftrag erstellt. Überwachen Sie den Status Ihres Updates, indem Sie zu **Aufträge**wechseln.
   
   > [!NOTE]
   > Wenn der Updateauftrag startet, wird der Status sofort mit 50 Prozent angezeigt. Der Status ändert sich erst zu 100 Prozent, wenn der Updateauftrag abgeschlossen ist. Für den Updateprozess gibt es keinen Echtzeitstatus.
   > 
   > 
4. Falls die Netzwerkschnittstellen DATA 2 und DATA 3 deaktiviert wurden, aktivieren Sie diese nach der erfolgreichen Aktualisierung des Geräts wieder.

## <a name="get-the-iqn-of-a-windows-server-host"></a>Abrufen des IQN eines Windows Server-Hosts
Führen Sie die folgenden Schritte aus, um den IQN (iSCSI Qualified Name) eines Windows-Hosts abzurufen, auf dem Windows Server 2012 ausgeführt wird.

[!INCLUDE [Create a manual backup](../../includes/storsimple-get-iqn.md)]

## <a name="create-a-manual-backup"></a>Erstellen einer manuellen Sicherung
Führen Sie die folgenden Schritte im klassischen Azure-Portal aus, um bei Bedarf eine manuelle Sicherung eines einzelnen Volumes auf dem StorSimple-Gerät zu erstellen.

[!INCLUDE [Create a manual backup](../../includes/storsimple-create-manual-backup.md)]

## <a name="next-steps"></a>Nächste Schritte
* Konfigurieren eines [virtuellen Geräts](storsimple-virtual-device-u2.md)
* Verwenden des [StorSimple-Manager-Diensts](https://msdn.microsoft.com/library/azure/dn772396.aspx) für das Verwalten Ihres StorSimple-Geräts




<!--HONumber=Dec16_HO2-->



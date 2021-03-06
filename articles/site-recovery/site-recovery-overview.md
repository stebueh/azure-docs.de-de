---
title: Was ist Site Recovery? | Microsoft Docs
description: "Enthält eine Übersicht über den Azure Site Recovery-Dienst und eine Zusammenfassung der Bereitstellungsszenarien."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: e9b97b00-0c92-4970-ae92-5166a4d43b68
ms.service: site-recovery
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 02/23/2017
ms.author: raynew
translationtype: Human Translation
ms.sourcegitcommit: 31c0d8a3b525d24861090adb0edf0351fae7467e
ms.openlocfilehash: de89ea7309736ac1636e7950d1cd7e68f9ec2f0e
ms.lasthandoff: 02/23/2017


---
# <a name="what-is-site-recovery"></a>Was ist Site Recovery?

Willkommen beim Azure Site Recovery-Dienst!

Dieser Artikel enthält eine Übersicht über den Azure Site Recovery-Dienst sowie Links zu weiteren Informationen.

Eine Unterbrechung des Geschäftsbetriebs wird durch natürliche Ereignisse und durch Betriebsfehler verursacht. Ihre Organisation benötigt eine Strategie für Geschäftskontinuität und Notfallwiederherstellung (Business Continuity and Disaster Recovery, BCDR), die gewährleistet, dass Apps und Daten während geplanter und ungeplanter Ausfallzeiten geschützt und verfügbar bleiben und dass die Organisation schnellstmöglich zum normalen Betrieb zurückkehren kann.

Die Wiederherstellungsdienste von Azure unterstützen Sie bei Ihrer BCDR-Strategie. Mit [Azure Backup](https://docs.microsoft.com/en-us/azure/backup/) sind Ihre Daten geschützt und wiederherstellbar. Mit Site Recovery können Sie Replikations-, Failover- und Wiederherstellungsvorgänge für Workloads durchführen und so deren Verfügbarkeit während eines Ausfalls sicherstellen.

## <a name="benefits"></a>Vorteile

Site Recovery trägt wie folgt zu Ihrer BCDR-Strategie bei:

- **Notfallwiederherstellung in der Cloud:** Workloads auf virtuellen Computern und physischen Servern können in Azure repliziert werden, anstatt eine Replikation an einem sekundären Standort durchzuführen. So entfallen die Kosten und die Komplexität, die mit der Verwaltung eines sekundären Datencenters verbunden sind.
- **Flexible Replikation für Hybridumgebungen:** Sie können beliebige Workloads replizieren, die auf unterstützten lokalen virtuellen Hyper-V-Computern, virtuellen VMware-Computern und physischen Windows- bzw. Linux-Servern ausgeführt werden.
- **Migration:** Mit Site Recovery können Sie lokale AWS-Instanzen zu virtuellen Azure-Computern oder virtuelle Azure-Computer zwischen Azure-Regionen migrieren.
- **Einfachere Geschäftskontinuität und Notfallwiederherstellung:** Die Replikation kann über einen zentralen Ort im Azure-Portal bereitgestellt werden.  Sie können einfache Failover- und Failbackvorgänge für einzelne und mehrere Computern ausführen.
- **Resilienz:** Site Recovery orchestriert die Replikation und das Failover, ohne Anwendungsdaten abzufangen.
Da die replizierten Daten in Azure Storage gespeichert werden, profitieren Sie von der entsprechenden Resilienz. Bei einem Failover werden virtuelle Azure-Computer auf der Grundlage der replizierten Daten erstellt.
- **Replikationsleistung:** Site Recovery ermöglicht eine Replikationsfrequenz von 30 Sekunden für Hyper-V und eine fortlaufende Replikation für VMware. Durch Festlegen von RPO-Schwellenwerten (Recovery Point Objective) können Sie steuern, wie oft Datenwiederherstellungspunkte erstellt werden sollen. Außerdem können Sie den RTO-Wert (Recovery Time Objective) verringern. Dies wird durch den automatisierten Wiederherstellungsprozess von Site Recovery und durch die Integration in [Azure Traffic Manager](https://azure.microsoft.com/en-us/blog/reduce-rto-by-using-azure-traffic-manager-with-azure-site-recovery/) ermöglicht.
- **Anwendungskonsistenz:** Computer werden mit anwendungskonsistenten Momentaufnahmen repliziert. Zusätzlich zur Erfassung von Datenträgerdaten werden mit anwendungskonsistenten Momentaufnahmen alle Daten im Arbeitsspeicher und alle Transaktionen des Prozesses erfasst.
- **Unterbrechungsfreie Tests:** Zur Unterstützung von Notfallwiederherstellungsübungen können Sie problemlos Testfailover ohne Auswirkungen auf die Produktionsumgebungen durchführen.
- **Flexible Failover- und Wiederherstellungsvorgänge:** Für erwartete Ausfälle können geplante Failover ohne Datenverlust ausgeführt werden, für unerwartete Notfälle sind ungeplante Failover mit minimalem Datenverlust (je nach Replikationsfrequenz) möglich. Wenn der primäre Standort wieder verfügbar ist, kann problemlos ein Failback ausgeführt werden.
- **Umfangreiche Wiederherstellungspläne:** Mit Wiederherstellungsplänen können Sie das Failover und die Wiederherstellung von Anwendungen mit mehreren Ebenen, die auf mehrere virtuelle Computer verteilt sind, modellieren und anpassen. Sie sortieren Gruppen innerhalb von Plänen und fügen Skripts und manuelle Aktionen hinzu. Wiederherstellungspläne können in Azure Automation-Runbooks integriert werden.
- **Apps mit mehreren Ebenen:** Sie können Wiederherstellungspläne für sequenzierte Failover- und Wiederherstellungsvorgänge von Apps mit mehreren Ebenen erstellen. Sie können Computer auf verschiedenen Ebenen (Datenbank, Web, App...) in einem Wiederherstellungsplan gruppieren und festlegen, wie Failover- und Startvorgänge für die einzelnen Gruppen ausgeführt werden sollen.
* **Integration in vorhandene BCDR-Technologien:** Site Recovery kann in andere BCDR-Technologien integriert werden. Beispielsweise können Sie Site Recovery verwenden, um das SQL Server-Back-End von geschäftlichen Workloads zu schützen, einschließlich der nativen Unterstützung für SQL Server Always On zum Verwalten des Failovers von Verfügbarkeitsgruppen.
* **Integration der Automation-Bibliothek:** Eine umfassende Azure Automation-Bibliothek bietet produktionsbereite, anwendungsspezifische Skripts, die heruntergeladen und in Site Recovery integriert werden können.
* **Einfache Netzwerkverwaltung:** Die erweiterte Netzwerkverwaltung in Site Recovery und Azure vereinfacht Anforderungen für Anwendungsnetzwerke, z.B. das Reservieren von IP-Adressen, Konfigurieren von Lastenausgleichsmodulen und Integrieren von Azure Traffic Manager für effiziente Netzwerkwechsel.

## <a name="site-recovery-in-the-azure-portal"></a>Site Recovery im Azure-Portal

* Site Recovery kann sowohl im neueren [Azure-Portal](https://portal.azure.com) als auch im [klassischen Azure-Portal](https://manage.windowsazure.com/) bereitgestellt werden.
* Im klassischen Azure-Portal können Sie Site Recovery mit dem klassischen Dienstverwaltungsmodell unterstützen.
* Im Azure-Portal können Sie das klassische Modell oder das neuere [Resource Manager-Bereitstellungsmodell](../azure-resource-manager/resource-manager-deployment-model.md) unterstützen.
- Das klassische Portal sollte nur zur Verwaltung bereits vorhandener Site Recovery-Bereitstellungen verwendet werden. Über das klassische Portal können keine neuen Tresore erstellt werden.

Die Informationen in diesem Artikel gelten für Bereitstellungen mit dem klassischen Portal und dem Azure-Portal. Wir haben die Unterschiede angegeben (falls zutreffend).

## <a name="whats-supported"></a>Unterstützte Funktionen

**Unterstützt** | **Details**
--- | ---
**Welche Regionen werden unterstützt?** | [Unterstützte Regionen](https://azure.microsoft.com/en-us/regions/services/) |
**Was kann ich replizieren?** | Lokale virtuelle VMware-Computer, virtuelle Hyper-V-Computer, physische Windows- und Linux-Server
**Welche Betriebssysteme werden für replizierte Computer unterstützt?** | [Betriebssysteme](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) für virtuelle VMware-Computer<br/><br/> Für virtuelle Hyper-V-Computer wird jedes [Gastbetriebssystem](https://technet.microsoft.com/en-us/windows-server-docs/compute/hyper-v/supported-windows-guest-operating-systems-for-hyper-v-on-windows) unterstützt, das auch von Azure und Hyper-V unterstützt wird.<br/><br/> [Betriebssysteme](site-recovery-support-matrix-to-azure.md#support-for-replicated-machine-os-versions) für physische Server
**Wo kann ich replizieren?** | Azure oder sekundäres Datencenter<br/><br/> In einem sekundären Datencenter können nur virtuelle Computer auf Hyper-V-Hosts repliziert werden, die in System Center VMM-Clouds verwaltet werden.
**Welche VMware-Server/-Hosts benötige ich?** | Die virtuellen VMware-Computer, die Sie replizieren möchten, können durch [unterstützte vSphere-Hosts/vCenter-Server](site-recovery-support-matrix-to-azure.md#support-for-datacenter-management-servers) verwaltet werden.
**Welche Workloads kann ich replizieren?** | Sie können jede Workload replizieren, die auf einem unterstützten Replikationscomputer ausgeführt wird.

## <a name="next-steps"></a>Nächste Schritte
* Unter [Welche Workloads kann Site Recovery schützen?](site-recovery-workload.md)
* Weitere Informationen zur Site Recovery-Architektur finden Sie unter [Wie funktioniert Site Recovery?](site-recovery-components.md)


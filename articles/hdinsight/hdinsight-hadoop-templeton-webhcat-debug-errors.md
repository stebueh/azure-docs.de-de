---
title: Verstehen und Beheben von WebHCat-Fehlern in HDInsight
description: "Erfahren Sie mehr zu Ursachen und Lösungen von Fehlern, die WebHCat in HDInsight zurückgibt."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1b3d94b1-207d-4550-aece-21dc45485549
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/12/2017
ms.author: larryfr
translationtype: Human Translation
ms.sourcegitcommit: 279990a67ae260b09d056fd84a12160150eb4539
ms.openlocfilehash: 2fbbfd3d221ef2f6bce302ed169eddf5a8b0d73e
ms.lasthandoff: 02/17/2017


---
# <a name="understand-and-resolve-errors-received-from-webhcat-templeton-on-hdinsight"></a>Verstehen und Beheben von Fehlern in HDInsight, die von WebHCat (Templeton) ausgegeben wurden
Wenn Sie für die Arbeit mit HDInsight WebHCat (ehemals Templeton) verwenden, erhalten Sie eventuell Fehler. In diesem Dokument finden Sie Hinweise zu den Ursachen und Lösungen häufiger Fehler.

## <a name="what-is-webhcat"></a>Was ist WebHCat?
[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) ist eine REST-API für [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), eine Tabellen- und Speicherverwaltungsschicht für Hadoop. WebHCat ist in HDInsight-Clustern standardmäßig aktiviert und wird von verschiedenen Tools zum Übermitteln von Aufträgen, Abfragen des Auftragsstatus und ähnlichem verwendet, ohne dass eine Anmeldung beim Cluster erforderlich ist.

## <a name="modifying-configuration"></a>Konfigurationsänderungen
> [!IMPORTANT]
> Einige der in diesem Dokument aufgeführten Fehler treten auf, weil ein konfigurierter Maximalwert überschritten wurde. Wenn im Lösungsschritt erklärt wird, dass Sie einen Wert ändern sollten, müssen Sie für die Änderung wie folgt vorgehen:


* Für **Windows** -Cluster: Verwenden Sie eine Skriptaktion, um den Wert bei der Erstellung des Clusters zu konfigurieren. Weitere Informationen finden Sie unter [Entwickeln von Skriptaktionen](hdinsight-hadoop-script-actions.md).
* Für **Linux** -Cluster: Verwenden Sie Ambari (Web- oder REST-API), um den Wert zu ändern. Weitere Informationen finden Sie unter [Verwalten von HDInsight mit Ambari](hdinsight-hadoop-manage-ambari.md)

> [!IMPORTANT]
> Linux ist das einzige Betriebssystem, das unter HDInsight Version 3.4 oder höher verwendet wird. Weitere Informationen finden Sie unter [Ende des Lebenszyklus von HDInsight unter Windows](hdinsight-component-versioning.md#hdi-version-32-and-33-nearing-deprecation-date).

### <a name="default-configuration"></a>Standardkonfiguration
Nachfolgend finden Sie eine Liste der Standardkonfigurationswerte, deren Überschreitung sich auf die Leistung von WebHCat auswirken oder Fehler verursachen kann:

| Einstellung | Funktionsbeschreibung | Standardwert |
| --- | --- | --- |
| [yarn.scheduler.capacity.maximum-applications][maximum-applications] |Die maximale Anzahl an Aufträgen, die gleichzeitig aktiv sein können (ausstehend oder in Bearbeitung). |10.000 |
| [templeton.exec.max-procs][max-procs] |Die maximale Anzahl an Anforderungen, die gleichzeitig bearbeitet werden können. |20 |
| [mapreduce.jobhistory.max-age-ms][max-age-ms] |Die Anzahl der Tage, die der Auftragsverlauf gespeichert wird. |7 Tage |

## <a name="too-many-requests"></a>Zu viele Anforderungen
**HTTP-Statuscode**: 429

| Ursache | Lösung |
| --- | --- |
| Sie haben die maximale Anzahl an Anforderungen überschritten, die WebHCat pro Minute gleichzeitig bearbeiten kann (Standardwert 20). |Reduzieren Sie die Arbeitslast, um sicherzustellen, dass Sie nicht mehr als die maximale Anzahl gleichzeitig bearbeitbarer Anforderungen übergeben, oder erhöhen Sie den Grenzwert für gleichzeitig bearbeitbare Anforderungen durch Änderung von `templeton.exec.max-procs`. Weitere Informationen finden Sie unter [Konfigurationsänderungen](#modifying-configuration) . |

## <a name="server-unavailable"></a>Server nicht verfügbar
**HTTP-Statuscode**: 503

| Ursache | Lösung |
| --- | --- |
| Dieser Fehler tritt gewöhnlich bei einem Failover zwischen dem primären und sekundären Hauptknoten des Clusters auf. |Warten Sie zwei Minuten, bevor Sie den Vorgang wiederholen. |

## <a name="bad-request-content-could-not-find-job"></a>Ungültige Anforderung: Auftrag nicht gefunden
**HTTP-Statuscode**: 400

| Ursache | Lösung |
| --- | --- |
| Auftragsdetails wurden durch die Auftragsverlaufsbereinigung gelöscht. |Die Standardaufbewahrungszeit für den Auftragsverlauf beträgt 7 Tage. Diese kann durch Änderung von `mapreduce.jobhistory.max-age-ms`geändert werden. Weitere Informationen finden Sie unter [Konfigurationsänderungen](#modifying-configuration) . |
| Der Auftrag wurde aufgrund eines Failovers beendet. |Versuchen Sie den Auftrag bis zu zwei Minuten lang zu wiederholen. |
| Eine ungültige Auftrags-ID wurde verwendet. |Prüfen Sie, ob die Auftrags-ID richtig ist. |

## <a name="bad-gateway"></a>Ungültiger Gateway
**HTTP-Statuscode**: 502

| Ursache | Lösung |
| --- | --- |
| Im WebHCat-Prozess findet eine interne Bereinigung statt. |Warten Sie, bis die Bereinigung den WebHCat-Dienst beendet oder neu gestartet hat. |
| Zeitüberschreitung beim Warten auf eine Antwort vom ResourceManager-Dienst. Dies kann passieren, wenn die Anzahl der aktiven Anwendungen den konfigurierten Maximalwert (standardmäßig 10.000) überschreitet. |Warten Sie, bis die aktiven Aufträge abgeschlossen sind, oder erhöhen Sie den Grenzwert für gleichzeitig ausgeführte Aufträge durch Änderung von `yarn.scheduler.capacity.maximum-applications`. Weitere Informationen finden Sie unter [Konfigurationsänderungen](#modifying-configuration) . |
| Wenn Sie alle Aufträge mit dem Aufruf [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) abrufen und `Fields` auf `*` festgelegt ist, |sollten Sie nicht *alle* Auftragsdetails abrufen, sondern mittels `jobid` nur die Details von Aufträgen, deren Auftrags-ID höher als ein bestimmter Wert ist. Oder Sie sollten kein `Fields` |
| Der WebHCat-Dienst ist während eines Failovers des Hauptknotens nicht verfügbar. |Warten Sie zwei Minuten, bevor Sie den Vorgang wiederholen. |
| Von WebHCat wurden mehr als 500 Aufträge übermittelt, deren Verarbeitung noch aussteht. |Warten Sie, bis die derzeit ausstehenden Aufträge abgeschlossen sind, bevor Sie weitere Aufträge übermitteln. |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml



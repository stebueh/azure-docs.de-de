---
title: "Behandeln von häufigen Verbindungsproblemen mit Azure SQL-Datenbank"
description: "Schritte zum Ermitteln und Behandeln von häufigen Verbindungsproblemen für Azure SQL-Datenbank."
services: sql-database
documentationcenter: 
author: dalechen
manager: cshepard
editor: 
ms.assetid: ac463d1c-aec8-443d-b66e-fa5eadcccfa8
ms.service: sql-database
ms.custom: troubleshoot
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/20/2017
ms.author: daleche
translationtype: Human Translation
ms.sourcegitcommit: 676cecdd886cfb557e7859e1e9583f0a0f9f749c
ms.openlocfilehash: 222b9fe98592e0c78ec3d7c5ae4804bf75dd0d1e
ms.lasthandoff: 02/16/2017


---
# <a name="troubleshoot-connection-issues-to-azure-sql-database"></a>Beheben von Verbindungsproblemen mit der Azure SQL-Datenbank
Wenn keine Verbindung zur Azure SQL-Datenbank hergestellt werden kann, erhalten Sie [Fehlermeldungen](sql-database-develop-error-messages.md). Bei diesem Artikel handelt es sich um eine zentrale Informationsquelle, die Sie bei der Behebung von Problemen mit der Verbindung mit Azure SQL-Datenbank unterstützt. In dem Artikel werden [die häufigsten Ursachen](#cause) von Verbindungsproblemen dargestellt, [ein Tool zur Problembehandlung](#try-the-troubleshooter-for-azure-sql-database-connectivity-issues) empfohlen, das Sie bei der Ermittlung des Problems unterstützt, sowie Problembehandlungsschritte zur Behebung von [vorübergehenden Fehlern](#troubleshoot-transient-errors) und [dauerhaften oder nicht vorübergehenden Fehlern](#troubleshoot-the-persistent-errors) bereitgestellt. Zudem werden [alle relevanten Artikel zu Verbindungsproblemen mit der Azure SQL-Datenbank](#all-topics-for-azure-sql-database-connection-problems)aufgelistet.

Führen Sie bei Verbindungsproblemen die in diesem Artikel beschriebenen Schritte zur Problembehandlung aus.
[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="cause"></a>Ursache
Verbindungsprobleme können folgende Ursachen haben:

* Fehler beim Anwenden von bewährten Methoden und Designrichtlinien während des Anwendungsdesignprozesses.  Hilfreiche Informationen für den Einstieg finden Sie in der [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md) .
* Neukonfiguration der Azure SQL-Datenbank
* Firewalleinstellungen
* Verbindungstimeout
* Falsche Anmeldeinformationen
* Einige Azure SQL-Datenbankressourcen haben ihr maximales Limit erreicht

Im Allgemeinen können Verbindungsprobleme mit der Azure SQL-Datenbank wie folgt klassifiziert werden:

* [Vorübergehende Fehler (kurzlebige oder zeitweilige Fehler)](#troubleshoot-transient-errors)
* [Dauerhafte oder nicht vorübergehende Fehler (Fehler, die sich regelmäßig wiederholen)](#troubleshoot-the-persistent-errors)

## <a name="try-the-troubleshooter-for-azure-sql-database-connectivity-issues"></a>Verwenden der Problembehandlung für Verbindungsprobleme mit der Azure SQL-Datenbank
Wenn Sie einen bestimmten Verbindungsfehler erkennen, können Sie das Problem mit [diesem Tool](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database)schnell ermitteln und beheben.

## <a name="troubleshoot-transient-errors"></a>Problembehandlung bei vorübergehenden Fehlern
Wenn in Ihrer Anwendung vorübergehende Fehler auftreten, lesen Sie die folgenden Themen. Darin finden Sie Tipps, wie Sie bei der Problembehandlung vorgehen und die Häufigkeit dieser Fehler reduzieren können:

* [Problembehandlung bei Fehlern wie „Datenbank &lt;x&gt; auf dem Server &lt;y&gt; ist nicht verfügbar“ (Fehler: 40613)](sql-database-troubleshoot-connection.md)
* [Durchführen der Problembehandlung, Diagnose und Verhinderung von SQL-Verbindungsfehlern und vorübergehenden Fehlern für SQL-Datenbank](sql-database-connectivity-issues.md)

<a id="troubleshoot-the-persistent-errors" name="troubleshoot-the-persistent-errors"></a>

## <a name="troubleshoot-persistent-errors-non-transient-errors"></a>Problembehandlung bei dauerhaften Fehlern (nicht vorübergehende Fehler)
Wenn die Anwendung dauerhaft keine Verbindung mit der Azure SQL-Datenbank herstellen kann, weist dies in der Regel auf Probleme der folgenden Art hin:

* Firewall-Konfiguration Die Firewall von Azure SQL-Datenbank oder eine clientseitige Firewall blockiert Verbindungen mit Azure SQL-Datenbank.
* Netzwerkneukonfiguration auf Clientseite: z.B. eine neue IP-Adresse oder ein Proxyserver.
* Benutzerfehler: z.B. falsch geschriebene Verbindungsparameter wie der Servername in der Verbindungszeichenfolge.

### <a name="steps-to-resolve-persistent-connectivity-issues"></a>Schritte zum Beheben dauerhafter Verbindungsprobleme
1. Richten Sie [Firewallregeln](sql-database-configure-firewall-settings.md) so ein, dass die IP-Adresse des Clients zugelassen wird. Richten Sie zu Testzwecken vorübergehend eine Firewallregel mit 0.0.0.0 als Beginn des IP-Adressbereichs und 255.255.255.255 als Ende des IP-Adressbereichs fest. Dadurch wird der Server für alle IP-Adressen geöffnet. Wird Ihr Konnektivitätsproblem dadurch behoben, entfernen Sie die Regel, und erstellen Sie eine Firewallregel für eine entsprechend eingeschränkte IP-Adresse bzw. einen entsprechend eingeschränkten IP-Adressbereich. 
2. Stellen Sie für alle Firewalls zwischen Client und Internet sicher, dass Port 1433 für ausgehende Verbindungen geöffnet ist. Zusätzliche Anhaltspunkte zu weiteren Ports, die für die Azure Active Directory-Authentifizierung geöffnet werden müssen, finden Sie unter [Konfigurieren der Windows-Firewall, um SQL Server-Zugriff zuzulassen](https://msdn.microsoft.com/library/cc646023.aspx) und [Erforderliche Ports und Protokolle für die Hybrid-Identität](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-ports).
3. Überprüfen Sie die Verbindungszeichenfolge und andere Verbindungseinstellungen. Siehe im Thema [Verbindungsprobleme](sql-database-connectivity-issues.md#connections-to-azure-sql-database)den Abschnitt „Verbindungszeichenfolge“.
4. Überprüfen Sie im Dashboard den Dienststatus. Wenn Sie glauben, dass es sich um einen regionalen Ausfall handelt, finden Sie unter [Wiederherstellen nach einem Ausfall](sql-database-disaster-recovery.md) Schritte zum Wiederherstellen in einer neuen Region.

## <a name="all-topics-for-azure-sql-database-connection-problems"></a>Alle Themen zu Problemen mit der Verbindung mit Azure SQL-Datenbank
Die folgende Tabelle enthält alle Themen im Zusammenhang mit Verbindungsproblemen, die sich direkt auf den Dienst Azure SQL-Datenbank beziehen.

| &nbsp; | Titel | Beschreibung |
| ---:|:--- |:--- |
| 1 |[Beheben von Verbindungsproblemen mit der Azure SQL-Datenbank](sql-database-troubleshoot-common-connection-issues.md) |Dies ist die Landing Page für die Behebung von Verbindungsproblemen mit der Azure SQL-Datenbank. Hier erhalten Sie Informationen darüber, wie Sie vorübergehende Fehler sowie dauerhafte und nicht vorübergehende Fehler in der Azure SQL-Datenbank beheben. |
| 2 |[Durchführen der Problembehandlung, Diagnose und Verhinderung von SQL-Verbindungsfehlern und vorübergehenden Fehlern für SQL-Datenbank](sql-database-connectivity-issues.md) |Erfahren Sie, wie Sie einen SQL-Verbindungsfehler oder vorübergehenden Fehler in Azure SQL-Datenbank behandeln, diagnostizieren und verhindern. |
| 3 |[Allgemeiner Leitfaden zur Behandlung vorübergehender Fehler](../best-practices-retry-general.md) |Bietet allgemeine Anleitungen zur Behandlung vorübergehender Fehler beim Verbinden mit der Azure SQL-Datenbank. |
| 4 |[Problembehandlung bei Verbindungen mit Microsoft Azure SQL-Datenbank](https://support.microsoft.com/help/10085/troubleshooting-connectivity-issues-with-microsoft-azure-sql-database) |Mit diesem Tool können Sie Ihr Problem ermitteln und Verbindungsfehler beheben. |
| 5 |[Problembehandlung bei Fehlern wie „Die Datenbank &lt;x&gt; auf dem Server &lt;y&gt; ist zurzeit nicht verfügbar. Bitte wiederholen Sie den Verbindungsversuch später.“](sql-database-troubleshoot-connection.md) |Beschreibt die Ermittlung und Behebung eines Fehlers 40613: „Datenbank &lt;x&gt; auf Server &lt;y&gt; ist zurzeit nicht verfügbar. Wiederholen Sie den Verbindungsversuch später.“ |
| 6 |[SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen: Datenbankverbindungsfehler und andere Probleme](sql-database-develop-error-messages.md) |Stellt Informationen über SQL-Fehlercodes für SQL-Datenbank-Clientanwendungen, beispielsweise zu häufigen Datenbankverbindungsfehlern, Datenbankkopiefehlern und allgemeinen Fehlern, bereit. |
| 7 |[Leitfaden zur Azure SQL-Datenbankleistung für Einzeldatenbanken](sql-database-performance-guidance.md) |Stellt Anleitungen zum Ermitteln des richtigen Diensttarifs für Ihre Anwendung bereit. Enthält zudem Empfehlungen zur Optimierung Ihrer Anwendung, damit Sie Ihre Azure SQL-Datenbank optimal nutzen können. |
| 8 |[Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md) |Enthält Links zu Codebeispielen für verschiedene Technologien, die Sie zum Herstellen einer Verbindung und zum Interagieren mit der Azure SQL-Datenbank verwenden können. |

## <a name="next-steps"></a>Nächste Schritte
* [Behandlung von Leistungsproblemen mit der Azure SQL-Datenbank](sql-database-troubleshoot-performance.md)
* [Durchsuchen der Dokumentation zu Microsoft Azure](http://azure.microsoft.com/search/documentation/)
* [Anzeigen der neuesten Updates zum Azure SQL-Datenbankdienst](http://azure.microsoft.com/updates/?service=sql-database)

## <a name="additional-resources"></a>Zusätzliche Ressourcen
* [Übersicht über die Entwicklung von SQL-Datenbanken](sql-database-develop-overview.md)
* [Allgemeiner Leitfaden zur Behandlung vorübergehender Fehler](../best-practices-retry-general.md)
* [Verbindungsbibliotheken für SQL-Datenbank und SQL Server](sql-database-libraries.md)



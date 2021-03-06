---
title: "Gewusst wie: Speichern von Suchvorgängen und Anheften von Datenressourcen | Microsoft Docs"
description: "Anleitungsartikel zu den Funktionen in Azure Data Catalog zum Speichern von Datenquellen und von Datenressourcen für die spätere Verwendung"
services: data-catalog
documentationcenter: 
author: steelanddata
manager: NA
editor: 
tags: 
ms.assetid: 6bd00a81-820d-4b7c-91fa-ab09e575474c
ms.service: data-catalog
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-catalog
ms.date: 01/23/2017
ms.author: maroche
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: f017776480466979d7f2f9edec2b3ac5caca2321


---
# <a name="how-to-save-searches-and-pin-data-assets"></a>Gewusst wie: Speichern von Suchvorgängen und Anheften von Datenressourcen
## <a name="introduction"></a>Einführung
Microsoft Azure Data Catalog enthält Funktionen für die Datenquellenermittlung. Benutzer können den Katalog schnell durchsuchen und filtern, um Datenquellen zu finden und deren Zweck zu verstehen. So ist es einfacher, für den aktuellen Auftrag die richtigen Daten zu finden.

Aber was ist, wenn Benutzer regelmäßig mit den gleichen Daten arbeiten müssen? Was ist, wenn Benutzer mit ihrem Wissen regelmäßig zu den gleichen Datenquellen im Katalog beitragen? In solchen Situationen ist es ineffizient, wenn die gleichen Suchen immer wieder durchgeführt werden müssen. Daher sind gespeicherte Suchvorgänge und angeheftete Datenressourcen hilfreich.

## <a name="saved-searches"></a>Gespeicherte Suchvorgänge
Ein gespeicherter Suchvorgang in Azure Data Catalog ist eine wiederverwendbare Suchdefinition für einen bestimmten Benutzer. Sobald ein Benutzer eine Suche einschließlich der Suchbegriffe, Tags und anderer Filter definiert hat, kann er sie für die spätere Verwendung speichern. Die Definition des gespeicherten Suchvorgangs kann dann später erneut ausgeführt werden,um die den Suchkriterien entsprechenden Datenressourcen zurückzugeben.

### <a name="creating-a-saved-search"></a>Erstellen eines gespeicherten Suchvorgangs
Geben Sie zum Erstellen einer gespeicherten Suche die Suchkriterien ein, die sie wiederverwenden möchten. Klicken Sie danach auf den Link „Speichern“ im Feld „Aktuelle Suche“ im Azure Data Catalog-Portal.

 ![Wählen Sie „Speichern“, um die aktuellen Sucheinstellungen zu speichern.](./media/data-catalog-how-to-save-pin/01-save-option.png)

Wenn Sie dazu aufgefordert werden, geben Sie einen Namen für die gespeicherte Suche ein. Wählen Sie einen Namen, der aussagekräftig ist und die Datenressourcen, die von der Suche ausgegeben werden, gut beschreibt.

 ![Geben Sie einen Namen für den gespeicherten Suchvorgang ein.](./media/data-catalog-how-to-save-pin/02-name.png)

### <a name="managing-saved-searches"></a>Verwalten gespeicherter Suchvorgänge
Sobald ein Benutzer eine oder mehrere Suchen gespeichert hat, wird die Option „Gespeicherte Suchvorgänge“ im Azure Data Catalog-Portal unter dem Feld „Aktuelle Suche“ angezeigt. Wird dieses erweitert, wird die vollständige Liste der gespeicherten Suchvorgänge angezeigt.

 ![Liste der gespeicherten Suchvorgänge](./media/data-catalog-how-to-save-pin/03-list.png)

Wenn Sie einen gespeicherten Suchvorgang aus der Liste auswählen, wird die Suche ausgeführt.

Wählen Sie das Dropdownmenü, um verschiedene Verwaltungsoptionen zu erhalten:

 ![Optionen zum Verwalten gespeicherter Suchvorgänge](./media/data-catalog-how-to-save-pin/04-managing.png)

Wenn Sie „Umbenennen“ wählen, wird der Benutzer zur Eingabe eines neuen Namens für den gespeicherten Suchvorgang aufgefordert. Die Suchdefinition wird nicht geändert.

Wenn Sie „Löschen“ auswählen, wird der Benutzer zunächst zum Bestätigen aufgefordert, und danach wird der gespeicherte Suchvorgang aus der Liste des Benutzers entfernt.

Durch Auswahl von „Als Standard speichern“ wird die ausgewählte gespeicherte Suche als Standardsuche für den Benutzer ausgewählt. Wenn der Benutzer über die Azure Data Catalog-Startseite eine „leere“ Suche ausführt, wird die Standardsuche des Benutzers ausgeführt. Darüber hinaus erscheint die als Standard gekennzeichnete Suche oben auf der Liste der gespeicherten Suchvorgänge.

### <a name="organizational-saved-searches"></a>Gespeicherte Organisationssuchen
Jeder Benutzer kann Suchvorgänge zur eigenen Verwendung speichern. Data Catalog-Administratoren können auch Suchvorgänge für alle Benutzer innerhalb der Organisation speichern. Beim Speichern einer Suche haben Administratoren die Möglichkeit, den gespeicherten Suchvorgang im Unternehmen freizugeben. Ist diese Option aktiviert, wird der gespeicherte Suchvorgang in der Liste verfügbarer Suchvorgänge für alle Benutzer angezeigt.

 ![Gespeicherte Organisationssuchen](./media/data-catalog-how-to-save-pin/08-organizational-saved-search.png)

## <a name="pinned-data-assets"></a>Angeheftete Datenressourcen
Gespeicherte Suchvorgänge ermöglichen es Benutzern, Suchdefinitionen zu speichern und wiederzuverwenden. Die von den Suchen ausgegebenen Datenressourcen ändern sich ggf. im Laufe der Zeit, wenn sich der Inhalt des Katalogs ändert. Durch das Anheften von Datenressourcen können Benutzer bestimmte Datenressourcen explizit identifizieren und so einfacher darauf zugreifen, ohne dass eine Suche erforderlich ist.

Das Anheften einer Datenressource ist einfach: Die Benutzer müssen lediglich auf das Anheftsymbol für die Datenressource klicken, um sie der Liste hinzuzufügen. Dieses Symbol wird in der Kachelansicht in der Ecke der Ressourcenkachel bzw. in der Listenansicht im Azure Data Catalog-Portal in der linken Spalte angezeigt.

![Anheften einer Datenressource](./media/data-catalog-how-to-save-pin/05-pinning.png)

Wenn eine Ressource nicht mehr angeheftet sein soll, ist dies ebenfalls ganz einfach: In diesem Fall klickt der Benutzer erneut auf das Anheftsymbol, um die Einstellung für die ausgewählte Ressource zu ändern.

![Anheften einer Datenressource aufheben](./media/data-catalog-how-to-save-pin/06-unpinning.png)

## <a name="my-assets"></a>„Meine Ressourcen“
Die Startseite des Azure Data Catalog-Portals enthält den Bereich „Meine Ressourcen“, in dem die Ressourcen angezeigt werden, die für den aktuellen Benutzer interessant sind. Dieser Bereich beinhaltet sowohl angeheftete Ressourcen als auch gespeicherte Suchvorgänge.

![„Meine Ressourcen“ auf der Startseite](./media/data-catalog-how-to-save-pin/07-my-assets.png)

## <a name="summary"></a>Zusammenfassung
Azure Data Catalog enthält Funktionen, mithilfe derer Benutzer die benötigten Datenquellen einfacher finden können, damit das Suchen nach Daten schneller geht und mehr Zeit für die Arbeit mit den Daten bleibt. Gespeicherte Suchvorgänge und angeheftete Datenressourcen funktionieren auf Grundlage dieser Funktionen, damit Benutzer häufig benötigte Datenquellen schnell identifizieren.



<!--HONumber=Nov16_HO3-->



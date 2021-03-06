---
title: Verwalten von DNS-Servern, die von einem virtuellen Netzwerk (VNet) verwendet werden
description: "Hier erfahren Sie, wie Sie DNS-Server in einem virtuellen Netzwerk (VNet) hinzufügen und entfernen."
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: b582be7d-dc78-4cfe-a766-185bd7e5dc68
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 3416cf13180e124dab1c74b9c7254390ac5e49c4


---
# <a name="manage-dns-servers-used-by-a-virtual-network-vnet"></a>Verwalten von DNS-Servern, die von einem virtuellen Netzwerk (VNet) verwendet werden
Sie können die Liste der in einem VNet verwendeten DNS-Server im Verwaltungsportal oder mithilfe der Netzwerkkonfigurationsdatei verwalten. Sie können bis zu 12 DNS-Server für jedes VNet hinzufügen. Beim Angeben von DNS-Servern müssen Sie darauf achten, dass Sie sie in der richtigen Reihenfolge für Ihre Umgebung auflisten. DNS-Serverlisten werden nicht per Roundrobin verarbeitet. Die DNS-Server werden in der Reihenfolge verwendet, in der sie angegeben sind. Wenn der erste DNS-Server in der Liste erreicht werden kann, verwendet der Client diesen DNS-Server unabhängig davon, ob der DNS-Server ordnungsgemäß funktioniert. Um die Reihenfolge der DNS-Server für das virtuelle Netzwerk zu ändern, entfernen Sie die DNS-Server aus der Liste, und fügen Sie sie in der gewünschten Reihenfolge wieder hinzu.

> [!WARNING]
> Nach dem Aktualisieren der DNS-Liste müssen Sie die virtuellen Computer in Ihrem virtuellen Netzwerk neu starten, damit sie die neuen DNS-Servereinstellungen verwenden. Virtuelle Computer verwenden bis zu ihrem Neustart weiter die aktuelle Konfiguration.
> 
> 

## <a name="edit-a-dns-server-list-for-a-virtual-network-using-the-management-portal"></a>Bearbeiten einer DNS-Serverliste für ein virtuelles Netzwerk im Verwaltungsportal
1. Melden Sie sich am **Verwaltungsportal**an.
2. Klicken Sie im Navigationsbereich auf **Netzwerke** und anschließend in der Spalte **Name** auf den Namen Ihres virtuellen Netzwerks.
3. Klicken Sie auf **Konfigurieren**.
4. Unter **DNS-Server**können Sie Folgendes konfigurieren:
   
   * **Registrieren (Hinzufügen) eines neuen DNS-Servers** : Geben Sie einfach den Namen und die IP-Adresse in die Felder ein. Dadurch wird der DNS-Serverliste Ihres virtuellen Netzwerks ein DNS-Server hinzugefügt, und der DNS-Server wird in Azure registriert.
   * **Hinzufügen eines zuvor registrierten DNS-Servers** : Wenn Sie bereits einen DNS-Server in Azure registriert haben, können Sie ihn in der vorab ausgefüllten Liste auswählen.
   * **Entfernen eines DNS-Servers aus dem virtuellen Netzwerk** : Klicken Sie auf das X neben dem Server, den Sie entfernen möchten. Beachten Sie, dass der Server dadurch nur aus der Liste dieses virtuellen Netzwerks entfernt wird. Der DNS-Server bleibt in Azure registriert und kann von Ihren anderen virtuellen Netzwerken verwendet werden. Um einen DNS-Server aus Ihrem Abonnement zu löschen, wechseln Sie zur Seite **Netzwerke -> DNS-Server**.
   * **Ändern der Reihenfolge von DNS-Servern** : Entfernen Sie alle aufgeführten DNS-Server, und fügen Sie sie in der gewünschten Reihenfolge wieder hinzu. Denken Sie daran, dass die DNS-Liste nicht per Roundrobin verarbeitet wird.
   * **Umbenennen eines DNS-Servers** : Markieren Sie den DNS-Server in der Liste, und geben Sie den neuen Namen ein. Dadurch wird ein neuer DNS-Server in Azure registriert und der DNS-Serverliste für Ihr virtuelles Netzwerk hinzugefügt. Der alte DNS-Server und seine IP-Adresse bleiben in Azure registriert. Sie können ihn auf der Seite **DNS-Server** löschen, falls Sie ihn nicht für andere virtuelle Netzwerke verwenden.
5. Klicken Sie unten auf der Seite auf **Speichern** , um die neue DNS-Serverkonfiguration zu speichern.
6. Starten Sie die virtuellen Computer im virtuellen Netzwerk neu, damit sie die neuen DNS-Einstellungen abrufen.

## <a name="edit-a-dns-server-list-using-a-network-configuration-file"></a>Bearbeiten einer DNS-Serverliste mithilfe einer Netzwerkkonfigurationsdatei
Um eine DNS-Serverliste mithilfe einer Netzwerkkonfigurationsdatei zu bearbeiten, müssen Sie zuerst Ihre Konfigurationseinstellungen aus dem Verwaltungsportal exportieren. Anschließend bearbeiten Sie die Netzwerkkonfigurationsdatei und importieren sie wieder in das Verwaltungsportal. Im Folgenden sind die allgemeinen Schritte zum Ausführen dieses Vorgangs aufgeführt.

1. Exportieren Sie die Einstellungen Ihres virtuellen Netzwerks in eine Netzwerkkonfigurationsdatei. Weitere Informationen und Anweisungen zum Exportieren der Netzwerkkonfigurationseinstellungen finden Sie unter [Exportieren der Einstellungen eines virtuellen Netzwerks in eine Netzwerkkonfigurationsdatei](virtual-networks-using-network-configuration-file.md).
2. Geben Sie die DNS-Serverinformationen für Ihr virtuelles Netzwerk an. Weitere Informationen zum Angeben eines DNS-Servers finden Sie unter [Angeben eines DNS-Servers in der Konfigurationsdatei eines virtuellen Netzwerks](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md). Weitere Informationen zu Netzwerkkonfigurationsdateien finden Sie unter [Konfigurationsschema für Azure Virtual Network](https://msdn.microsoft.com/library/azure/jj157100.aspx) und [Konfigurieren eines virtuellen Netzwerks mithilfe einer Netzwerkkonfigurationsdatei](virtual-networks-using-network-configuration-file.md).
3. Importieren Sie die Netzwerkkonfigurationsdatei. Weitere Informationen und Anweisungen zum Importieren der Netzwerkkonfigurationsdatei finden Sie unter [Importieren einer Netzwerkkonfigurationsdatei](virtual-networks-using-network-configuration-file.md).
4. Starten Sie die virtuellen Computer im virtuellen Netzwerk neu, damit sie die neuen DNS-Einstellungen abrufen.




<!--HONumber=Nov16_HO3-->



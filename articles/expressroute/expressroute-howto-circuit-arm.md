---
title: "Erstellen und Ändern einer ExpressRoute-Verbindung: PowerShell: Azure Resource Manager | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt das Erstellen, Bereitstellen, Überprüfen, Aktualisieren, Löschen und Aufheben der Bereitstellung einer ExpressRoute-Verbindung."
documentationcenter: na
services: expressroute
author: ganesr
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f997182e-9b25-4a7a-b079-b004221dadcc
ms.service: expressroute
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/10/2016
ms.author: ganesr;cherylmc
translationtype: Human Translation
ms.sourcegitcommit: 9a8a72f5255184a1ac571532355c7f7a23d7f7bd
ms.openlocfilehash: 0aacbdafdb5ded81dbc8495a30837e2f4941075f
ms.lasthandoff: 02/03/2017


---
# <a name="create-and-modify-an-expressroute-circuit"></a>Erstellen und Ändern einer ExpressRoute-Verbindung
> [!div class="op_single_selector"]
> * [Resource Manager – Azure-Portal](expressroute-howto-circuit-portal-resource-manager.md)
> * [Resource Manager – PowerShell](expressroute-howto-circuit-arm.md)
> * [Klassisch – PowerShell](expressroute-howto-circuit-classic.md)
> * [Video – Azure-Portal](http://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-create-an-expressroute-circuit)
> 
>

In diesem Artikel wird beschrieben, wie Sie eine Azure ExpressRoute-Verbindung mithilfe von Windows PowerShell-Cmdlets und des Azure Resource Manager-Bereitstellungsmodells erstellen. Der Artikel veranschaulicht auch, wie Sie den Status der Verbindung prüfen, aktualisieren, löschen oder die Bereitstellung aufheben.

**Informationen zu Azure-Bereitstellungsmodellen**

[!INCLUDE [vpn-gateway-clasic-rm](../../includes/vpn-gateway-classic-rm-include.md)]

## <a name="before-you-begin"></a>Voraussetzungen
* Rufen Sie die neueste Version der Azure PowerShell-Module ab (mindestens Version 1.0). Eine Schrittanleitung zum Konfigurieren des Computers für die Verwendung der PowerShell-Module erhalten Sie auf der Seite [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).
* Lesen Sie vor Beginn der Konfiguration die Seiten zu den [Voraussetzungen](expressroute-prerequisites.md) und [Workflows](expressroute-workflows.md).


## <a name="create-and-provision-an-expressroute-circuit"></a>Erstellen und Bereitstellen einer ExpressRoute-Verbindung
### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Melden Sie sich bei Ihrem Azure-Konto an, und wählen Sie Ihr Abonnement aus.
Um mit der Konfiguration zu beginnen, melden Sie sich bei Ihrem Azure-Konto an. Weitere Informationen zu PowerShell finden Sie unter [Verwenden von Windows PowerShell mit Resource Manager](../powershell-azure-resource-manager.md). Verwenden Sie die folgenden Beispiele, um eine Verbindung herzustellen:

    Login-AzureRmAccount

Überprüfen Sie die Abonnements für das Konto:

    Get-AzureRmSubscription

Wählen Sie das Abonnement aus, mit dem eine ExpressRoute-Verbindung erstellt werden soll:

    Select-AzureRmSubscription -SubscriptionId "<subscription ID>"

### <a name="2-get-the-list-of-supported-providers-locations-and-bandwidths"></a>2. Rufen Sie die Liste der unterstützten Anbieter, Standorte und Bandbreiten ab.
Vor dem Erstellen einer ExpressRoute-Verbindung benötigen Sie die Liste der unterstützten Konnektivitätsanbieter, Standorte und Bandbreitenoptionen.

Das PowerShell-Cmdlet `Get-AzureRmExpressRouteServiceProvider` gibt diese Informationen zurück, die Sie in späteren Schritten verwenden werden:

    Get-AzureRmExpressRouteServiceProvider

Überprüfen Sie, ob Ihr Konnektivitätsanbieter hier aufgeführt ist. Notieren Sie sich die folgenden Informationen, da Sie diese später beim Erstellen der Verbindung benötigen:

* Name
* PeeringLocations
* BandwidthsOffered

Sie können nun eine ExpressRoute-Verbindung erstellen.   

### <a name="3-create-an-expressroute-circuit"></a>3. Erstellen Sie eine ExpressRoute-Verbindung.
Wenn Sie noch keine Ressourcengruppe besitzen, müssen Sie zuerst eine erstellen, bevor Sie Ihre ExpressRoute-Verbindung erstellen. Dazu können Sie den folgenden Befehl ausführen:

    New-AzureRmResourceGroup -Name "ExpressRouteResourceGroup" -Location "West US"


Das folgende Beispiel zeigt, wie Sie eine ExpressRoute-Verbindung mit 200 MBit/s über Equinix im Silicon Valley herstellen können. Wenn Sie einen anderen Anbieter und andere Einstellungen verwenden, ersetzen Sie bei Ihrer Anforderung die entsprechenden Informationen. Nachfolgend sehen Sie eine Beispielanforderung für einen neuen Dienstschlüssel:

    New-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup" -Location "West US" -SkuTier Standard -SkuFamily MeteredData -ServiceProviderName "Equinix" -PeeringLocation "Silicon Valley" -BandwidthInMbps 200

Stellen Sie sicher, dass Sie die richtige SKU-Ebene und die richtige SKU-Familie angeben.

* Die SKU-Ebene bestimmt, ob ein ExpressRoute Standard- oder ein ExpressRoute Premium-Add-On aktiviert wird. Sie können *Standard* für die Standard-SKU bzw. *Premium* für das Premium-Add-On angeben.
* Die SKU-Familie bestimmt den Abrechnungstyp. Sie können *Metereddata* für einen Volumentarif und *Unlimiteddata* für einen Plan mit Datenflatrate auswählen. Sie können den Abrechnungstyp von *Metereddata* in *Unlimiteddata* ändern, nicht jedoch umgekehrt (*Unlimiteddata* in *Metereddata*).

> [!IMPORTANT]
> Ihre ExpressRoute-Verbindung wird von dem Moment an berechnet, in dem ein Dienstschlüssel ausgegeben wird. Stellen Sie sicher, dass Sie diesen Vorgang ausführen, sobald der Konnektivitätsanbieter dazu bereit ist, die Verbindung bereitzustellen.
> 
> 

Die Antwort enthält den Dienstschlüssel. Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie Folgendes ausführen:

    get-help New-AzureRmExpressRouteCircuit -detailed


### <a name="4-list-all-expressroute-circuits"></a>4. Listen Sie alle ExpressRoute-Verbindungen auf.
Um eine Liste mit allen von Ihnen erstellten ExpressRoute-Verbindungen abzurufen, führen Sie den Befehl `Get-AzureRmExpressRouteCircuit` aus:

    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

Die Antwort entspricht etwa dem folgenden Beispiel:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState          : Enabled
    ServiceProviderProvisioningState  : NotProvisioned
    ServiceProviderNotes              :
    ServiceProviderProperties         : {
                                          "ServiceProviderName": "Equinix",
                                          "PeeringLocation": "Silicon Valley",
                                          "BandwidthInMbps": 200
                                        }
    ServiceKey                        : **************************************
    Peerings                          : []

Sie können diese Informationen jederzeit mithilfe des Cmdlets `Get-AzureRmExpressRouteCircuit` abrufen. Wenn Sie den Aufruf ohne Parameter durchführen, werden alle Verbindungen aufgelistet. Der Dienstschlüssel wird im Feld *ServiceKey* aufgeführt:

    Get-AzureRmExpressRouteCircuit


Die Antwort entspricht etwa dem folgenden Beispiel:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : NotProvisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie Folgendes ausführen:

    get-help Get-AzureRmExpressRouteCircuit -detailed

### <a name="5-send-the-service-key-to-your-connectivity-provider-for-provisioning"></a>5. Senden Sie den Dienstschlüssel zur Bereitstellung an Ihren Konnektivitätsanbieter.
*ServiceProviderProvisioningState* enthält Informationen zum aktuellen Zustand der Bereitstellung auf Dienstanbieterseite. Mit „Status“ wird der Zustand auf Microsoft-Seite angegeben. Weitere Informationen zu Bereitstellungszuständen für eine Verbindung finden Sie im Artikel [Workflows](expressroute-workflows.md#expressroute-circuit-provisioning-states) .

Wenn Sie eine neue ExpressRoute-Verbindung erstellen, weist die Verbindung folgenden Zustand auf:

    ServiceProviderProvisioningState : NotProvisioned
    CircuitProvisioningState         : Enabled



Die Verbindung wechselt in den folgenden Zustand, wenn sie vom Konnektivitätsanbieter für Sie aktiviert wird:

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled

Damit Sie eine ExpressRoute-Verbindung verwenden können, muss sie sich im folgenden Zustand befinden:

    ServiceProviderProvisioningState : Provisioned
    CircuitProvisioningState         : Enabled

### <a name="6-periodically-check-the-status-and-the-state-of-the-circuit-key"></a>6. Überprüfen Sie regelmäßig den Status und Zustand des Verbindungsschlüssels.
Durch das Überprüfen des Status und des Zustands des Verbindungsschlüssels werden Sie informiert, wann Ihr Anbieter Ihre Verbindung aktiviert hat. Sobald die Verbindung konfiguriert wurde, zeigt *ServiceProviderProvisioningState* wie im folgenden Beispiel den Status *Bereitgestellt* an:

    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Die Antwort entspricht etwa dem folgenden Beispiel:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                       }
    ServiceKey                       : **************************************
    Peerings                         : []

### <a name="7-create-your-routing-configuration"></a>7. Erstellen Sie die Routingkonfiguration.
Eine detaillierte Anleitung zum Erstellen und Ändern von Verbindungspeerings finden Sie im Artikel [Erstellen und Ändern des Routings für eine ExpressRoute-Verbindung](expressroute-howto-routing-arm.md) .

> [!IMPORTANT]
> Diese Anweisungen gelten nur für Verbindungen, die über Dienstanbieter erstellt wurden, von denen Layer 2-Konnektivitätsdienste angeboten werden. Wenn Sie einen Dienstanbieter nutzen, der verwaltete Layer 3-Dienste anbietet (meist ein IP VPN, z.B. MPLS), übernimmt Ihr Konnektivitätsanbieter die Konfiguration und Verwaltung des Routings für Sie.
> 
> 

### <a name="8-link-a-virtual-network-to-an-expressroute-circuit"></a>8. Verknüpfen eines virtuellen Netzwerks mit einer ExpressRoute-Verbindung
Verknüpfen Sie anschließend ein virtuelles Netzwerk mit Ihrer ExpressRoute-Verbindung. Lesen Sie den Artikel [Verknüpfen von virtuellen Netzwerken mit ExpressRoute-Verbindungen](expressroute-howto-linkvnet-arm.md) , wenn Sie mit dem Resource Manager-Bereitstellungsmodell arbeiten.

## <a name="getting-the-status-of-an-expressroute-circuit"></a>Abrufen des Status einer ExpressRoute-Verbindung
Sie können diese Informationen jederzeit mithilfe des Cmdlets `Get-AzureRmExpressRouteCircuit` abrufen. Wenn Sie den Aufruf ohne Parameter durchführen, werden alle Verbindungen aufgelistet.

    Get-AzureRmExpressRouteCircuit


Die Antwort ähnelt dem folgenden Beispiel:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                         "Tier": "Standard",
                                         "Family": "MeteredData"
                                       }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                            "ServiceProviderName": "Equinix",
                                            "PeeringLocation": "Silicon Valley",
                                            "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Sie können Informationen zu einer bestimmten ExpressRoute-Verbindung erhalten, indem Sie den Ressourcengruppennamen und den Verbindungsnamen als Parameter an den Aufruf übergeben:

    Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"


Die Antwort entspricht etwa dem folgenden Beispiel:

    Name                             : ExpressRouteARMCircuit
    ResourceGroupName                : ExpressRouteResourceGroup
    Location                         : westus
    Id                               : /subscriptions/***************************/resourceGroups/ExpressRouteResourceGroup/providers/Microsoft.Network/expressRouteCircuits/ExpressRouteARMCircuit
    Etag                             : W/"################################"
    ProvisioningState                : Succeeded
    Sku                              : {
                                         "Name": "Standard_MeteredData",
                                            "Tier": "Standard",
                                            "Family": "MeteredData"
                                          }
    CircuitProvisioningState         : Enabled
    ServiceProviderProvisioningState : Provisioned
    ServiceProviderNotes             :
    ServiceProviderProperties        : {
                                         "ServiceProviderName": "Equinix",
                                         "PeeringLocation": "Silicon Valley",
                                         "BandwidthInMbps": 200
                                          }
    ServiceKey                       : **************************************
    Peerings                         : []


Ausführliche Beschreibungen aller Parameter erhalten Sie, wenn Sie Folgendes ausführen:

    get-help get-azurededicatedcircuit -detailed


## <a name="a-namemodifyamodifying-an-expressroute-circuit"></a><a name="modify"></a>Ändern einer ExpressRoute-Verbindung
Sie können bestimmte Eigenschaften einer ExpressRoute-Verbindung ändern, ohne die Konnektivität zu beeinträchtigen.

Sie können folgende Aktionen ausführen, ohne Ausfallzeiten zu verursachen:

* Aktivieren oder deaktivieren Sie ein ExpressRoute Premium-Add-On für Ihre ExpressRoute-Verbindung.
* Erhöhen Sie die Bandbreite Ihrer ExpressRoute-Verbindung. Beachten Sie, dass ein Downgrade der Bandbreite einer Verbindung nicht unterstützt wird.
* Sie können den Abrechnungsplan von „Volumentarif“ zu „Datenflatrate“ ändern. Beachten Sie, dass eine Änderung des Abrechnungsplans von „Datenflatrate“ zu „Volumentarif“ nicht unterstützt wird.
* Sie können die Option *Klassische Vorgänge zulassen*aktivieren und deaktivieren.

Weitere Informationen zu Beschränkungen und Grenzwerten finden Sie unter [ExpressRoute – Häufig gestellte Fragen](expressroute-faqs.md).

### <a name="to-enable-the-expressroute-premium-add-on"></a>Aktivieren des ExpressRoute Premium-Add-Ons
Für Ihre bereits vorhandene Verbindung können Sie das ExpressRoute Premium-Add-On mit dem folgenden PowerShell-Codeausschnitt aktivieren:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Premium"
    $ckt.sku.Name = "Premium_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Für die Verbindung sind nun die Features des ExpressRoute Premium-Add-Ons aktiviert. Beachten Sie, dass die Gebühren für das Premium-Add-On ab der erfolgreichen Ausführung des Befehls erhoben werden.

### <a name="to-disable-the-expressroute-premium-add-on"></a>Deaktivieren des ExpressRoute Premium-Add-Ons
> [!IMPORTANT]
> Bei diesem Vorgang kann ein Fehler auftreten, wenn Sie Ressourcen verwenden, die die zulässige Menge für die Standardverbindung überschreiten.
> 
> 

Beachten Sie Folgendes:

* Bevor Sie ein Downgrade von Premium auf Standard durchführen, müssen Sie sicherstellen, dass weniger als zehn virtuelle Netzwerke mit der Verbindung verknüpft sind. Andernfalls verursacht Ihre Updateanforderung einen Fehler, und Microsoft berechnet Ihnen Premium-Gebühren.
* Sie müssen die Verknüpfung für alle virtuellen Netzwerke in anderen geopolitischen Regionen aufheben. Andernfalls verursacht Ihre Updateanforderung einen Fehler, und Microsoft berechnet Ihnen Premium-Gebühren.
* Ihre Routentabelle muss für das private Peering weniger als 4.000 Routen aufweisen. Wenn Ihre Routentabelle mehr als 4.000 Routen umfasst, wird die BGP-Sitzung verworfen. Eine erneute Aktivierung ist erst dann wieder möglich, wenn die Anzahl der angekündigten Präfixe unter 4.000 fällt.

Für Ihre bereits vorhandene Verbindung können Sie das ExpressRoute Premium-Add-On mit dem folgenden PowerShell-Cmdlet deaktivieren:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Tier = "Standard"
    $ckt.sku.Name = "Standard_MeteredData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


### <a name="to-update-the-expressroute-circuit-bandwidth"></a>Aktualisieren der Bandbreite für die ExpressRoute-Verbindung
Informationen zu unterstützten Bandbreitenoptionen für Ihren Anbieter finden Sie unter [ExpressRoute – Häufig gestellte Fragen](expressroute-faqs.md). Sie können jede Größe auswählen, die die Größe der vorhandenen Verbindung übersteigt.

> [!IMPORTANT]
> Es ist nicht möglich, die Bandbreite einer ExpressRoute-Verbindung ohne Störungen zu reduzieren. Ein Downgrade der Bandbreite erfordert, dass Sie die Bereitstellung der ExpressRoute-Verbindung aufheben und dann eine neue ExpressRoute-Verbindung bereitstellen.
> 
> 

Sobald Sie sich für die benötigte Größe entschieden haben, verwenden Sie den folgenden Befehl, um die Größe der Verbindung anzupassen:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.ServiceProviderProperties.BandwidthInMbps = 1000

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt


Die Größe Ihrer Verbindung wird auf der Microsoft-Seite angepasst. Anschließend müssen Sie sich an Ihren Konnektivitätsanbieter wenden, um die entsprechende Aktualisierung der Konfigurationen auf Anbieterseite vornehmen zu lassen. Nachdem Sie diese Benachrichtigung gesendet haben, beginnt Microsoft, die Gebühren für die aktualisierte Bandbreitenoption abzurechnen.

### <a name="to-move-the-sku-from-metered-to-unlimited"></a>Ändern der SKU von einem Volumentarif zu einer Datenflatrate
Sie können die SKU einer ExpressRoute-Verbindung mithilfe des folgenden PowerShell-Codeausschnitts ändern:

    $ckt = Get-AzureRmExpressRouteCircuit -Name "ExpressRouteARMCircuit" -ResourceGroupName "ExpressRouteResourceGroup"

    $ckt.Sku.Family = "UnlimitedData"
    $ckt.sku.Name = "Premium_UnlimitedData"

    Set-AzureRmExpressRouteCircuit -ExpressRouteCircuit $ckt

### <a name="to-control-access-to-the-classic-and-resource-manager-environments"></a>Steuern des Zugriffs auf die klassische und die Resource Manager-Umgebung
Lesen Sie die Anweisungen unter [Verschieben von ExpressRoute-Verbindungen vom klassischen zum Resource Manager-Bereitstellungsmodell](expressroute-howto-move-arm.md).  

## <a name="deprovisioning-and-deleting-an-expressroute-circuit"></a>Aufheben der Bereitstellung und Löschen einer ExpressRoute-Verbindung
Beachten Sie Folgendes:

* Sie müssen die Verknüpfung aller virtuellen Netzwerke mit der ExpressRoute-Verbindung aufheben. Falls dieser Vorgang nicht erfolgreich ist, überprüfen Sie, ob noch virtuelle Netzwerke mit der Verbindung verknüpft sind.
* Wenn der Bereitstellungsstatus des ExpressRoute-Verbindungsdienstanbieters **Bereitstellung** oder **Bereitgestellt** lautet, arbeiten Sie mit Ihrem Dienstanbieter zusammen, um die Verbindungsbereitstellung auf Anbieterseite aufzuheben. Microsoft reserviert weiterhin Ressourcen für Sie und stellt Ihnen dies in Rechnung, bis der Dienstanbieter die Aufhebung der Verbindungsbereitstellung abgeschlossen hat und uns benachrichtigt.
* Wenn der Dienstanbieter die Bereitstellung der Verbindung aufgehoben hat (Bereitstellungsstatus des Dienstanbieters lautet **Nicht bereitgestellt**), können Sie die Verbindung löschen. Damit wird die Abrechnung für die Verbindung beendet.

Sie können die ExpressRoute-Verbindung löschen, indem Sie den folgenden Befehl ausführen:

    Remove-AzureRmExpressRouteCircuit -ResourceGroupName "ExpressRouteResourceGroup" -Name "ExpressRouteARMCircuit"



## <a name="next-steps"></a>Nächste Schritte

Führen Sie nach dem Erstellen Ihrer Verbindung folgende Vorgänge aus:

* [Erstellen und Ändern des Routings für Ihre ExpressRoute-Verbindung](expressroute-howto-routing-arm.md)
* [Verknüpfen Ihres virtuelles Netzwerks mit Ihrer ExpressRoute-Verbindung](expressroute-howto-linkvnet-arm.md)



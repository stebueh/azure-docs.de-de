---
title: "Verwalten von DNS-Zonen in Azure DNS – Azure CLI 2.0 | Microsoft-Dokumentation"
description: "Sie können DNS-Zonen mithilfe der Azure CLI 2.0 verwalten. In diesem Artikel erfahren Sie, wie Sie DNS-Zonen in Azure DNS aktualisieren, löschen und erstellen."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
ms.assetid: 8ab63bc4-5135-4ed8-8c0b-5f0712b9afed
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/27/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 1481fcb070f383d158c5a6ae32504e498de4a66b
ms.openlocfilehash: ca6347e079c0e85d1e9583f77b2e43632bedb206
ms.lasthandoff: 03/01/2017

---

# <a name="how-to-manage-dns-zones-in-azure-dns-using-the-azure-cli-20"></a>Verwalten von DNS-Zonen in Azure DNS mithilfe von Azure CLI 2.0

> [!div class="op_single_selector"]
> * [Azure CLI 1.0](dns-operations-dnszones-cli-nodejs.md)
> * [Azure CLI 2.0](dns-operations-dnszones-cli.md)
> * [PowerShell](dns-operations-dnszones.md)

In diesem Leitfaden erfahren Sie, wie Sie Ihre DNS-Zonen mithilfe der plattformübergreifenden, für Windows, Mac und Linux verfügbaren Azure-Befehlszeilenschnittstelle verwalten. Sie können Ihre DNS-Zonen auch mithilfe von [Azure PowerShell](dns-operations-dnszones.md) oder über das Azure-Portal verwalten.

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

* [Azure CLI 1.0:](dns-operations-dnszones-cli-nodejs.md) Unsere Befehlszeilenschnittstelle für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
* [Azure CLI 2.0:](dns-operations-dnszones-cli.md) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="introduction"></a>Einführung

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="set-up-azure-cli-20-for-azure-dns"></a>Einrichten der Azure CLI 2.0 für Azure DNS

### <a name="before-you-begin"></a>Voraussetzungen

Vergewissern Sie sich vor Beginn der Konfiguration, dass Sie über Folgendes verfügen:

* Ein Azure-Abonnement. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

* Installieren Sie die neueste Version der für Windows, Linux und Mac verfügbaren Azure CLI 2.0. Weitere Informationen finden Sie unter [Installieren der Azure CLI 2.0](https://docs.microsoft.com/en-us/cli/azure/install-az-cli2).

### <a name="sign-in-to-your-azure-account"></a>Anmelden bei Ihrem Azure-Konto

Öffnen Sie ein Konsolenfenster, und authentifizieren Sie sich mit Ihren Anmeldeinformationen. Weitere Informationen finden Sie unter „Anmelden bei Azure über die Azure-Befehlszeilenschnittstelle (CLI)“.

```
az login
```

### <a name="select-the-subscription"></a>Auswählen des Abonnements

Überprüfen Sie die Abonnements für das Konto.

```
az account list
```

### <a name="choose-which-of-your-azure-subscriptions-to-use"></a>Wählen Sie aus, welches Azure-Abonnement Sie verwenden möchten.

```azurecli
az account set --subscription "subscription name"
```

### <a name="create-a-resource-group"></a>Erstellen einer Ressourcengruppe

Der Azure-Ressourcen-Manager erfordert, dass alle Ressourcengruppen einen Speicherort angeben. Dieser wird als Standardspeicherort für Ressourcen in dieser Ressourcengruppe verwendet. Da alle DNS-Ressourcen global und nicht regional sind, hat die Auswahl des Speicherorts für die Ressourcengruppe jedoch keine Auswirkungen auf Azure DNS.

Dieser Schritt kann übersprungen werden, wenn Sie eine vorhandene Ressourcengruppe verwenden.

```azurecli
az group create --name myresourcegroup --location "West US"
```

## <a name="getting-help"></a>Hilfe

Alle CLI 2.0-Befehle im Zusammenhang mit Azure DNS beginnen mit `az network dns`. Für jeden Befehl ist eine Hilfe verfügbar, die mit der Option `--help` (Kurzform: `-h`) angezeigt werden kann.  Beispiel:

```azurecli
az network dns --help
az network dns zone --help
az network dns zone create --help
```

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

Eine DNS-Zone wird mit dem `az network dns zone create` -Befehl erstellt. Entsprechende Hilfeinformationen finden Sie unter `az network dns zone create -h`.

Im folgenden Beispiel wird in der Ressourcengruppe namens *MyResourceGroup* eine DNS-Zone namens *contoso.com* erstellt:

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com
```

### <a name="to-create-a-dns-zone-with-tags"></a>So erstellen Sie eine DNS-Zone mit Tags

Das folgende Beispiel zeigt, wie Sie unter Verwendung des Parameters `--tags` (Kurzform: `-t`) eine DNS-Zone mit zwei [Azure Resource Manager-Tags](dns-zones-records.md#tags) (*project = demo* und *env = test*) erstellen:

```azurecli
az network dns zone create --resource-group MyResourceGroup --name contoso.com --tags "project=demo" "env=test"
```

## <a name="get-a-dns-zone"></a>Abrufen einer DNS-Zone

Verwenden Sie zum Abrufen einer DNS-Zone `az network dns zone show`. Entsprechende Hilfeinformationen finden Sie unter `az network dns zone show --help`.

Das folgende Beispiel gibt die DNS-Zone *contoso.com* und die zugehörigen Daten aus der Ressourcengruppe *MyResourceGroup* zurück. 

```azurecli
az network dns zone show --resource-group myresourcegroup --name contoso.com
```

Das folgende Beispiel ist die Antwort.

```json
{
  "etag": "00000002-0000-0000-3d4d-64aa3689d201",
  "id": "/subscriptions/147a22e9-2356-4e56-b3de-1f5842ae4a3b/resourceGroups/myresourcegroup/providers/Microsoft.Network/dnszones/contoso.com",
  "location": "global",
  "maxNumberOfRecordSets": 5000,
  "name": "contoso.com",
  "nameServers": [
    "ns1-04.azure-dns.com.",
    "ns2-04.azure-dns.net.",
    "ns3-04.azure-dns.org.",
    "ns4-04.azure-dns.info."
  ],
  "numberOfRecordSets": 4,
  "resourceGroup": "myresourcegroup",
  "tags": {},
  "type": "Microsoft.Network/dnszones"
}
```

Beachten Sie, dass von `az network dns zone show` keine DNS-Einträge zurückgegeben werden. Verwenden Sie zum Auflisten von DNS-Einträgen `az network dns record-set list`.


## <a name="list-dns-zones"></a>Auflisten von DNS-Zonen

Verwenden Sie zum Aufzählen von DNS-Zonen `az network dns zone list`. Entsprechende Hilfeinformationen finden Sie unter `az network dns zone list --help`.

Wenn Sie die Ressourcengruppe angeben, werden nur die Zonen innerhalb der Ressourcengruppe aufgelistet:

```azurecli
az network dns zone list --resource-group MyResourceGroup
```

Wenn Sie die Ressourcengruppe weglassen, werden alle Zonen im Abonnement aufgelistet:

```azurecli
az network dns zone list 
```

## <a name="update-a-dns-zone"></a>Aktualisieren einer DNS-Zone

Änderungen an einer DNS-Zonenressource können mithilfe von `az network dns zone update`vorgenommen werden. Entsprechende Hilfeinformationen finden Sie unter `az network dns zone update --help`.

Dieser Befehl aktualisiert keinen der DNS-Ressourceneintragssätze in der Zone (siehe [Verwalten von DNS-Ressourceneintragssätzen](dns-operations-recordsets-cli.md)). Es wird nur verwendet, um Eigenschaften der Zonenressource selbst zu aktualisieren. Diese Eigenschaften sind gegenwärtig auf die [Azure Resource Manager-„Tags“](dns-zones-records.md#tags) für die Zonenressource beschränkt.

Das folgende Beispiel zeigt, wie Sie die Tags einer DNS-Zone aktualisieren. Die vorhandenen Tags werden durch den angegebenen Wert ersetzt.

```azurecli
az network dns zone update --resource-group myresourcegroup --name contoso.com --set tags.team=support
```

## <a name="delete-a-dns-zone"></a>Löschen einer DNS-Zone

DNS-Zonen können mithilfe von `az network dns zone delete`gelöscht werden. Entsprechende Hilfeinformationen finden Sie unter `az network dns zone delete --help`.

> [!NOTE]
> Beim Löschen einer DNS-Zone werden auch alle DNS-Einträge in der Zone gelöscht. Dieser Vorgang kann nicht rückgängig gemacht werden. Wenn die DNS-Zone verwendet wird, tritt in Diensten, die die Zone verwenden, ein Fehler auf.
>
>Informationen dazu, wie Sie Zonen vor versehentlichem Löschen schützen, finden Sie unter [How to protect DNS zones and records](dns-protect-zones-recordsets.md) (Schützen von DNS-Zonen und -Einträgen).

Dieser Befehl fordert Sie zur Bestätigung auf. Der optionale Schalter `--yes` unterdrückt diese Aufforderung.

Das folgende Beispiel zeigt, wie die Zone *contoso.com* aus der Ressourcengruppe *MyResourceGroup* gelöscht wird.

```azurecli
az network dns zone delete --resource-group myresourcegroup --name contoso.com
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie [Ressourceneintragssätze und Einträge in Ihrer DNS-Zone verwalten](dns-getstarted-create-recordset-cli.md).

Erfahren Sie, wie Sie [Ihre Domäne an Azure DNS delegieren](dns-domain-delegation.md).



---
title: "Konfigurieren des gewünschten Zustands für Azure – Übersicht | Microsoft Docs"
description: "Übersicht über die Verwendung des Microsoft Azure-Erweiterungshandlers für PowerShell DSC. Mit Informationen zu Voraussetzungen, Architektur, Cmdlets..."
services: virtual-machines-windows
documentationcenter: 
author: zjalexander
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
keywords: 
ms.assetid: bbacbc93-1e7b-4611-a3ec-e3320641f9ba
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: na
ms.date: 01/09/2017
ms.author: zachal
translationtype: Human Translation
ms.sourcegitcommit: c2ce603e80243584fdc302c545e520b4503f5555
ms.openlocfilehash: ca2d8d4b277f48ec46156293f73b18b6c2967c51


---
# <a name="introduction-to-the-azure-desired-state-configuration-extension-handler"></a>Einführung in den Handler der Azure-Erweiterung zum Konfigurieren des gewünschten Zustands
[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-both-include.md)]

Der Azure VM-Agent und die dazugehörigen Erweiterungen sind Teil der Microsoft Azure-Infrastrukturdienste. VM-Erweiterungen sind Softwarekomponenten, die die VM-Funktionalität erweitern und verschiedene Verwaltungsvorgänge für virtuelle Computer vereinfachen. Die VMAccess-Erweiterung kann z.B. verwendet werden, um ein Administratorkennwort zurückzusetzen, oder die benutzerdefinierte Skripterweiterung kann zur Ausführung eines Skripts auf dem virtuellen Computer verwendet werden.

In diesem Artikel wird die PowerShell-Erweiterung zum Konfigurieren des gewünschten Zustands (Desired State Configuration, DSC) für virtuelle Azure-Computer als Teil des Azure PowerShell SDKs vorgestellt. Sie können neue Cmdlets verwenden, um eine PowerShell DSC-Konfiguration auf einen virtuellen Azure-Computer, der über die PowerShell DSC-Erweiterung verfügt, hochzuladen und anzuwenden. Die PowerShell DSC-Erweiterung richtet einen Aufruf an PowerShell DSC, um die empfangene DSC-Konfiguration auf dem virtuellen Computer in Kraft zu setzen. Diese Funktion steht auch über das Azure-Portal zur Verfügung.

## <a name="prerequisites"></a>Voraussetzungen
**Lokaler Computer** Für die Interaktion mit der Erweiterung für virtuelle Azure-Computer müssen Sie entweder das Azure-Portal oder das Azure PowerShell SDK verwenden. 

**Gast-Agent:** Der virtuelle Azure-Computer, der mit der DSC-Konfiguration konfiguriert wird, muss über ein Betriebssystem verfügen, das Version 4.0 oder 5.0 von Windows Management Framework (WMF) unterstützt. Die vollständige Liste der unterstützten Betriebssystemversionen finden Sie im [Versionsverlauf der DSC-Erweiterung](https://blogs.msdn.microsoft.com/powershell/2014/11/20/release-history-for-the-azure-dsc-extension/).

## <a name="terms-and-concepts"></a>Begriffe und Konzepte
Für dieses Handbuch müssen Sie mit folgenden Konzepten vertraut sein:

Konfiguration: Ein DSC-Konfigurationsdokument. 

Knoten: Ein Ziel für die DSC-Konfiguration. In diesem Dokument bezieht sich „Knoten“ immer auf einen virtuellen Azure-Computer.

Konfigurationsdaten: Eine PSD1-Datei mit Umgebungsdaten für eine Konfiguration.

## <a name="architectural-overview"></a>Übersicht über die Architektur
Die Azure DSC-Erweiterung nutzt das VM-Agent-Framework von Azure zur Übermittlung und Inkraftsetzung von DSC-Konfigurationen auf virtuellen Azure-Computern sowie zur Erstellung entsprechender Berichte. Die DSC-Erweiterung erwartet eine ZIP-Datei mit mindestens einem Konfigurationsdokument sowie einen Parametersatz, der entweder über das Azure PowerShell SDK oder über das Azure-Portal bereitgestellt wird.

Wenn die Erweiterung zum ersten Mal aufgerufen wird, wird ein Installationsvorgang ausgeführt. Dieser Prozess verwendet die folgende Logik, um eine Version von Windows Management Framework (WMF) zu installieren:

1. Bei einem virtuellen Azure-Computer unter Windows Server 2016 wird keine Aktion ausgeführt. Unter Windows Server 2016 ist bereits die neueste Version von PowerShell installiert.
2. Bei Angabe der Eigenschaft `wmfVersion` wird die entsprechende WMF-Version installiert (es sei denn, sie ist nicht mit dem Betriebssystem des virtuellen Computers kompatibel).
3. Ohne Angabe der Eigenschaft `wmfVersion` wird die neueste geeignete WMF-Version installiert.

Die WMF-Installation erfordert einen Neustart. Nach dem Neustart lädt die Erweiterung die in der Eigenschaft `modulesUrl` angegebene ZIP-Datei herunter. Befindet sich der Speicherort im Azure-Blobspeicher, kann in der Eigenschaft `sasToken` ein SAS-Token für den Dateizugriff angegeben werden. Nachdem die ZIP-Datei heruntergeladen und entpackt wurde, wird durch Ausführen der in `configurationFunction` definierten Konfigurationsfunktion die MOF-Datei generiert. Anschließend führt die Erweiterung `Start-DscConfiguration -Force` für die generierte MOF-Datei aus. Die Erweiterung erfasst die Ausgabe und schreibt sie in den Azure-Statuskanal. Ab diesem Punkt behandelt der DSC-LCM die Überwachung und Korrektur wie gewohnt. 

## <a name="powershell-cmdlets"></a>PowerShell-Cmdlets
PowerShell-Cmdlets können mit Azure Resource Manager oder dem klassischen Bereitstellungsmodell zum Verpacken, Veröffentlichen und Überwachen der Bereitstellung von DSC-Erweiterungen verwendet werden. Bei den im Anschluss aufgeführten Cmdlets handelt es sich um Module für die klassische Bereitstellung, „Azure“ kann jedoch durch „AzureRm“ ersetzt werden, um das Azure Resource Manager-Modell zu verwenden. Beispielsweise verwendet `Publish-AzureVMDscConfiguration` das klassische Bereitstellungsmodell, während `Publish-AzureRmVMDscConfiguration` Azure Resource Manager verwendet. 

`Publish-AzureVMDscConfiguration` durchsucht eine Konfigurationsdatei nach abhängigen DSC-Ressourcen und erstellt eine ZIP-Datei mit der Konfiguration und erforderlichen DSC-Ressourcen für deren Inkraftsetzung. Mithilfe des Parameters `-ConfigurationArchivePath` kann das Paket auch lokal erstellt werden. Andernfalls wird die ZIP-Datei im Azure-Blobspeicher veröffentlicht und durch ein SAS-Token geschützt.

Bei der durch dieses Cmdlet erstellten ZIP-Datei befindet sich das PS1-Skript im Stammverzeichnis des Archivordners. Der Modulordner für Ressourcen wird im Archivordner platziert. 

`Set-AzureVMDscExtension` fügt die erforderlichen Einstellungen für die PowerShell-DSC-Erweiterung in ein VM-Konfigurationsobjekt ein. Im klassischen Bereitstellungsmodell müssen die VM-Änderungen mit `Update-AzureVM` auf eine Azure-VM angewendet werden. 

`Get-AzureVMDscExtension` ruft den DSC-Erweiterungsstatus eines bestimmten virtuellen Computers ab. 

`Get-AzureVMDscExtensionStatus` ruft den Status der DSC-Konfiguration ab, die durch den DSC-Erweiterungs-Handler in Kraft gesetzt wurde. Diese Aktion kann auf einem einzelnen virtuellen Computer oder einer Gruppe virtueller Computer ausgeführt werden.

`Remove-AzureVMDscExtension` entfernt den Erweiterungs-Handler von einem bestimmten virtuellen Computer. Dieses Cmdlet bewirkt **nicht** die Entfernung der Konfiguration, die Deinstallation von WMF oder eine Änderung der angewendeten Einstellungen auf dem virtuellen Computer. Es wird lediglich der Erweiterungshandler entfernt. 

**Hauptunterschiede in ASM- und Azure Resource Manager-Cmdlets**

* Azure Resource Manager-Cmdlets sind synchron. ASM-Cmdlets sind asynchron.
* ResourceGroupName, VMName, ArchiveStorageAccountName, Version und Location sind erforderliche Parameter in Azure Resource Manager.
* ArchiveResourceGroupName ist ein neuer optionaler Parameter für Azure Resource Manager. Diesen Parameter können Sie angeben, wenn Ihr Speicherkonto nicht der Ressourcengruppe angehört, in der der virtuelle Computer erstellt wird.
* ConfigurationArchive heißt bei Azure Resource Manager ArchiveBlobName.
* ContainerName heißt bei Azure Resource Manager ArchiveContainerName.
* StorageEndpointSuffix heißt bei Azure Resource Manager ArchiveStorageEndpointSuffix.
* Für Azure Resource Manager wurde der Switch „AutoUpdate“ hinzugefügt, um die automatische Aktualisierung des Erweiterungshandlers auf die neueste Version zu ermöglichen, sobald diese verfügbar ist. Hierdurch wird der virtuelle Computer unter Umständen neu gestartet, wenn eine neue WMF-Version veröffentlicht wird. 

## <a name="azure-portal-functionality"></a>Funktionen des Azure-Portals
Navigieren Sie zu einem virtuellen Computer. Klicken Sie unter „Einstellungen“ > „Allgemein“ auf „Erweiterungen“. Ein neuer Bereich wird erstellt. Klicken Sie auf „Hinzufügen“, und wählen Sie „PowerShell DSC“ aus.

Das Portal benötigt Eingabe.
**Konfigurationsmodule oder -skript**: Dieses Feld ist obligatorisch. Erfordert eine PS1-Datei mit einem Konfigurationsskript oder eine ZIP-Datei mit einem PS1-Konfigurationsskript am Verzeichnisstamm und allen abhängigen Ressourcen in Modulordnern innerhalb der ZIP. Die Datei kann mithilfe des Cmdlets `Publish-AzureVMDscConfiguration -ConfigurationArchivePath` aus dem Azure PowerShell SDK erstellt werden. Die ZIP-Datei wird in Ihren Benutzer-Blobspeicher hochgeladen und durch ein SAS-Token geschützt. 

**PSD1-Konfigurationsdatendatei**: Dieses Feld ist optional. Wenn Ihre Konfiguration eine Konfigurationsdatendatei in PSD1 erfordert, wählen Sie mithilfe dieses Felds eine solche Datei aus, und laden Sie sie in Ihren Benutzer-Blobspeicher hoch, wo sie durch ein SAS-Token geschützt wird. 

**Modulspezifischer Konfigurationsname**: PS1-Dateien können mehrere Konfigurationsfunktionen besitzen. Geben Sie den Namen des PS1-Konfigurationsskripts ein – gefolgt von \' und dem Namen der Konfigurationsfunktion. Wenn der Name des PS1-Skripts z.B. „configuration.ps1“ und die Konfiguration „IisInstall“ lautet, geben Sie Folgendes ein: `configuration.ps1\IisInstall`

**Konfigurationsargumente**: Falls die Konfigurationsfunktion Argumente akzeptiert, geben Sie diese hier im folgenden Format ein: `argumentName1=value1,argumentName2=value2`. Hierbei handelt es sich um ein anderes Format als bei den Konfigurationsargumenten, die von PowerShell-Cmdlets oder Resource Manager-Vorlagen akzeptiert werden. 

## <a name="getting-started"></a>Erste Schritte
Die Azure DSC-Erweiterung akzeptiert DSC-Konfigurationsdokumente und setzt sie auf virtuellen Azure-Computern in Kraft. Im Anschluss sehen Sie ein einfaches Beispiel für eine Konfiguration. Speichern Sie es lokal als „IisInstall.ps1“:

```powershell
configuration IISInstall 
{ 
    node "localhost"
    { 
        WindowsFeature IIS 
        { 
            Ensure = "Present" 
            Name = "Web-Server"                       
        } 
    } 
}
```

In den folgenden Schritten wird das Skript „IisInstall.ps1“ auf dem angegebenen virtuellen Computer platziert, die Konfiguration ausgeführt und der Status gemeldet.
###<a name="classic-model"></a>Klassisches Modell
```powershell
#Azure PowerShell cmdlets are required
Import-Module Azure

#Use an existing Azure Virtual Machine, 'DscDemo1'
$demoVM = Get-AzureVM DscDemo1

#Publish the configuration script into user storage.
Publish-AzureVMDscConfiguration -ConfigurationPath ".\IisInstall.ps1" -StorageContext $storageContext -Verbose -Force

#Set the VM to run the DSC configuration
Set-AzureVMDscExtension -VM $demoVM -ConfigurationArchive "IisInstall.ps1.zip" -StorageContext $storageContext -ConfigurationName "IisInstall" -Verbose

#Update the configuration of an Azure Virtual Machine
$demoVM | Update-AzureVM -Verbose

#check on status
Get-AzureVMDscExtensionStatus -VM $demovm -Verbose
```
###<a name="azure-resource-manager-model"></a>Azure Resource Manager-Modell

```powershell
$resourceGroup = "dscVmDemo"
$location = "westus"
$vmName = "myVM"
$storageName = "demostorage"
#Publish the configuration script into user storage
Publish-AzureRmVMDscConfiguration -ConfigurationPath .\iisInstall.ps1 -ResourceGroupName $resourceGroup -StorageAccountName $storageName -force
#Set the VM to run the DSC configuration
Set-AzureRmVmDscExtension -Version 2.21 -ResourceGroupName $resourceGroup -VMName $vmName -ArchiveStorageAccountName $storageName -ArchiveBlobName iisInstall.ps1.zip -AutoUpdate:$true -ConfigurationName "IISInstall"

```

## <a name="logging"></a>Protokollierung
Speicherort der Protokolle:

C:\WindowsAzure\Logs\Plugins\Microsoft.Powershell.DSC\[Versionsnummer]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu PowerShell DSC finden Sie im [PowerShell-Dokumentationscenter](https://msdn.microsoft.com/powershell/dsc/overview). 

Sehen Sie sich die [Azure Resource Manager-Vorlage für die DSC-Erweiterung](virtual-machines-windows-extensions-dsc-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)an. 

Weitere Funktionen, die Sie mit PowerShell DSC verwalten können, finden Sie, indem Sie den [PowerShell-Katalog](https://www.powershellgallery.com/packages?q=DscResource&x=0&y=0) nach weiteren DSC-Ressourcen durchsuchen.

Ausführliche Informationen zum Übergeben von sensiblen Parametern an Konfigurationen finden Sie unter [Übergeben von Anmeldeinformationen an den Azure DSC-Erweiterungshandler](virtual-machines-windows-extensions-dsc-credentials.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).




<!--HONumber=Jan17_HO2-->



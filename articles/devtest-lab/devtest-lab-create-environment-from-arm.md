---
title: Erstellen von Umgebungen mit mehreren virtuellen Computern mit Azure Resource Manager-Vorlagen | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie in Azure DevTest Labs auf der Grundlage einer Azure Resource Manager-Vorlage Umgebungen mit mehreren virtuellen Computern erstellen.
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: bd280fabf22d569f3c4dae9f00f44b08700c8fc4
ms.openlocfilehash: f034f028b0b1e0011e152b88d304dd54cfbc8320
ms.lasthandoff: 02/21/2017


---

# <a name="create-multi-vm-environments-with-azure-resource-manager-templates"></a>Erstellen von Umgebungen mit mehreren virtuellen Computern mit Azure Resource Manager-Vorlagen

Über das [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) können Sie ganz einfach [einen virtuellen Computer erstellen und einem Lab hinzufügen](./devtest-lab-add-vm-with-artifacts.md). Für einzelne virtuelle Computer funktioniert das sehr gut. Wenn die Umgebung allerdings mehrere virtuelle Computer enthält, muss jeder virtuelle Computer einzeln erstellt werden. Für Szenarien wie etwa eine Web-App mit mehreren Ebenen oder eine SharePoint-Farm ist ein Mechanismus erforderlich, der die gleichzeitige Erstellung mehrerer virtueller Computer ermöglicht. Mit Azure Resource Manager-Vorlagen können Sie jetzt die Infrastruktur und Konfiguration Ihrer Azure-Lösung definieren und wiederholt mehrere virtuelle Computer in einem konsistenten Zustand bereitstellen. Dieses Feature hat folgende Vorteile:

- Azure Resource Manager-Vorlagen werden direkt über Ihr Quellcodeverwaltungs-Repository (GitHub oder Team Services Git) geladen.
- Nach der Konfiguration können Ihre Benutzer eine Umgebung erstellen, indem sie genau wie bei anderen Arten von [VM-Grundlagen](./devtest-lab-comparing-vm-base-image-types.md) eine Azure Resource Manager-Vorlage über das Azure-Portal auswählen.
- Azure PaaS-Ressourcen können in einer Umgebung zusätzlich zu virtuellen IaaS-Computern über eine Azure Resource Manager-Vorlage bereitgestellt werden.
- Die Umgebungskosten können im Lab zusätzlich zu den einzelnen virtuellen Computern nachverfolgt werden, die unter Verwendung anderer Grundlagen erstellt wurden.
- Den Benutzern steht für Umgebungen die gleiche VM Richtliniensteuerung zur Verfügung wie für virtuelle Computer mit einem einzelnen Lab.

## <a name="configure-azure-resource-manager-template-repositories"></a>Konfigurieren von Azure Resource Manager-Vorlagenrepositorys

Bei Infrastruktur als Code und Konfiguration als Code empfiehlt es sich, Umgebungsvorlagen in der Quellcodeverwaltung zu verwalten. Azure DevTest Labs wendet diese Vorgehensweise an und lädt alle Azure Resource Manager-Vorlagen direkt aus Ihrem GitHub- oder VSTS Git-Repository. Beim Organisieren von Azure Resource Manager-Vorlagen in einem Repository sind folgende Regeln zu beachten:

- Der Name der Mastervorlagendatei muss `azuredeploy.json` lauten. 

    ![Zentrale Azure Resource Manager-Vorlagendateien](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Wenn Sie in einer Parameterdatei definierte Parameterwerte verwenden möchten, muss der Name der Parameterdatei `azuredeploy.parameters.json` lauten.
- Sie können Metadaten definieren, um den Anzeigenamen und die Beschreibung der Vorlage anzugeben. Diese Metadaten müssen sich in einer Datei namens `metadata.json` befinden. Die folgende Metadaten-Beispieldatei veranschaulicht das Angeben von Anzeigename und Beschreibung: 

```json
{
 
"itemDisplayName": "<your template name>",
 
"description": "<description of the template>"
 
}
```

Mit den folgenden Schritten können Sie Ihrem Lab über das Azure-Portal ein Repository hinzufügen: 

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.   
1. Wählen Sie auf dem Blatt des Labs die Option **Configuration and Policies** (Konfiguration und Richtlinien) aus.

    ![Konfiguration und Richtlinien](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Wählen Sie in der Einstellungsliste **Configuration and Policies** (Konfiguration und Richtlinien) die Option **Repositorys** aus. Auf dem Blatt **Repositorys** werden die Repositorys aufgelistet, die dem Lab hinzugefügt wurden. Für alle Labs wird automatisch ein Repository namens `Public Repo` generiert und mit dem [DevTest Labs-GitHub-Repository](https://github.com/Azure/azure-devtestlab) verbunden, das mehrere verwendbare VM-Artefakte enthält.

    ![Öffentliches Repository](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Wählen Sie **Hinzufügen+** aus, um Ihr Azure Resource Manager-Vorlagenrepository hinzuzufügen.
1. Wenn sich das zweite Blatt vom Typ **Repositorys** öffnet, geben Sie die erforderlichen Informationen ein:
    - **Name**: Geben Sie den im Lab verwendeten Repositorynamen ein.
    - **Git-Klon-URL:** Geben Sie die Git-HTTPS-Klon-URL aus GitHub oder Visual Studio Team Services ein.  
    - **Verzweigung**: Geben Sie den Verzweigungsnamen für den Zugriff auf Ihre Azure Resource Manager-Vorlagendefinitionen ein. 
    - **Persönliches Zugriffstoken**: Das persönliche Zugriffstoken ermöglicht den sicheren Zugriff auf Ihr Repository. Wählen Sie ** &lt;Ihr Name> > Mein Profil > Sicherheit > Öffentliches Zugriffstoken** aus, um Ihr Token aus Visual Studio Team Services abzurufen. Um Ihr Token aus GitHub abzurufen, wählen Sie Ihren Avatar und anschließend **Einstellungen > Öffentliches Zugriffstoken** aus. 
    - **Ordnerpfade**: Geben Sie über eines der beiden Eingabefelder den Ordnerpfad ein – entweder zu Ihren Artefaktdefinitionen (erstes Eingabefeld) oder zu Ihren Azure Resource Manager-Vorlagendefinitionen. Der Pfad beginnt mit einem Schrägstrich (/) und wird relativ zu Ihrem Git-Klon URI angegeben.   
    
        ![Öffentliches Repository](./media/devtest-lab-create-environment-from-arm/repo-values.png)

1. Wenn alle erforderlichen Felder ausgefüllt wurden und bei der Überprüfung der Angaben keine Fehler aufgetreten sind, wählen Sie **Speichern** aus.

Im nächsten Abschnitt erfahren Sie, wie Sie Umgebungen auf der Grundlage einer Azure Resource Manager-Vorlage erstellen.

## <a name="create-an-environment-from-an-azure-resource-manager-template"></a>Erstellen einer Umgebung auf der Grundlage einer Azure Resource Manager-Vorlage

Nachdem im Lab ein Azure Resource Manager-Vorlagenrepository konfiguriert wurde, können Ihre Labbenutzer mit folgenden Schritten eine Umgebung über das Azure-Portal erstellen:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
1. Wählen Sie **Weitere Dienste** und dann in der Liste **DevTest Labs** aus.
1. Wählen Sie in der Liste der Labs das gewünschte Lab aus.   
1. Wählen Sie auf dem Blatt des Labs die Option **Hinzufügen+** aus.
1. Auf dem Blatt **Basisdesign auswählen** werden die Basisimages, die Sie mit Ihren Azure Resource Manager-Vorlagen verwenden können, zuerst aufgeführt. Wählen Sie die gewünschte Azure Resource Manager-Vorlage aus.

    ![Auswählen einer Grundlage](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. Geben Sie auf dem Blatt **Hinzufügen** den Wert für **Umgebungsname** ein. Der Umgebungsname ist der Name, der Ihren Benutzern im Lab angezeigt wird. Die restlichen Eingabefelder sind in der Azure Resource Manager-Vorlage definiert. Falls in der Vorlage Standardwerte definiert sind oder die Datei `azuredeploy.parameter.json` vorhanden ist, werden in diesen Eingabefeldern Standardwerte angezeigt. Für Parameter vom Typ *Sichere Zeichenfolge* können Sie die Geheimnisse verwenden, die im [persönlichen geheimen Speicher](https://azure.microsoft.com/en-us/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store) des Labs gespeichert sind.

    ![Blatt „Hinzufügen“](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > Einige Parameterwerte werden als leere Werte angezeigt (auch wenn sie angegeben wurden). Wenn Benutzer diese Werte also Parametern in einer Azure Resource Manager-Vorlage zuweisen, zeigt DevTest Labs die Werte nicht an. Stattdessen werden leere Eingabefelder angezeigt, in die die Labbenutzer beim Erstellen der Umgebung einen Wert eingeben müssen.
    > 
    > - GEN-UNIQUE
    > - GEN-UNIQUE-[N]
    > - GEN-SSH-PUB-KEY
    > - GEN-PASSWORD 
 
1. Wählen Sie **Hinzufügen** aus, um die Umgebung zu erstellen. Die Bereitstellung der Umgebung beginnt umgehend, und der Status wird in der Liste **My virtual machines** (Meine virtuellen Computer) angezeigt. Vom Lab wird automatisch eine neue Ressourcengruppe erstellt, um alle in der Azure Resource Manager-Vorlage definierten Ressourcen bereitzustellen.
1. Wählen Sie die erstellte Umgebung in der Liste **My virtual machines** (Meine virtuellen Computer)aus, um das Ressourcengruppenblatt zu öffnen, und erkunden Sie die in der Umgebung bereitgestellten Ressourcen.
    
    ![Liste mit Ihren virtuellen Computern](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Klicken Sie auf eine der Umgebungen, um die verfügbaren Aktionen anzuzeigen. Hierzu zählt beispielsweise das Anwenden von Artefakten, das Anfügen von Datenträgern oder das Anpassen des Zeitpunkts für das automatische Herunterfahren.

    ![Umgebungsaktionen](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="next-steps"></a>Nächste Schritte
* Nach der Erstellung des virtuellen Computers können Sie auf dem Blatt des virtuellen Computers die Option **Verbinden** auswählen, um eine Verbindung mit dem virtuellen Computer herzustellen.
* Sehen Sie sich die [Azure Resource Manager-Vorlagen aus dem Katalog mit Azure-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates) an.


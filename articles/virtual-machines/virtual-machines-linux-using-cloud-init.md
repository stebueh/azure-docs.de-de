---
title: Verwenden von Cloud-Init zum Anpassen einer Linux-VM | Microsoft Docs
description: "Erfahren Sie, wie Sie Cloud-Init zum Anpassen einer Linux-VM während der Erstellung mithilfe von Azure CLI 2.0 (Vorschau) verwenden."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 195c22cd-4629-4582-9ee3-9749493f1d72
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/10/2017
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: 7d804c93933fd53b0a74696391e3ade228e66560
ms.openlocfilehash: f1c44718685cd522dcd79ac7e334e52a9488d123
ms.lasthandoff: 02/27/2017


---
# <a name="use-cloud-init-to-customize-a-linux-vm-during-creation"></a>Verwenden von Cloud-Init zum Anpassen einer Linux-VM während der Erstellung
In diesem Artikel wird gezeigt, wie Sie mithilfe von Azure CLI 2.0 ein Cloud-Init-Skript zum Festlegen des Hostnamens, zum Aktualisieren installierter Pakete und zum Verwalten von Benutzerkonten erstellen.  Diese Cloud-Init-Skripts werden dann während der Erstellung der virtuellen Computer über die Azure-Befehlszeilenschnittstelle aufgerufen.  Sie können diese Schritte auch mit [Azure CLI 1.0](virtual-machines-linux-using-cloud-init-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) ausführen.

## <a name="quick-commands"></a>Schnellbefehle
Erstellen Sie ein cloud-init.txt-Skript, das den Hostnamen festlegt, alle Pakete aktualisiert und Linux einen Sudo-Benutzer hinzufügt.

```sh
#cloud-config
hostname: myVMhostname
apt_upgrade: true
users:
  - name: myNewAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myVM
```

Erstellen einer Ressourcengruppe zum Starten von virtuellen Computern in mit [az group create](/cli/azure/group#create. Im folgenden Beispiel wird die Ressourcengruppe `myResourceGroup` erstellt:

```azurecli
az group create --name myResourceGroup --location westus
```

Erstellen Sie eine Linux-VM mit [az vm create](/cli/azure/vm#create) und Cloud-Init, die beim Start konfiguriert wird.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --custom-data cloud-init.txt
```

## <a name="detailed-walkthrough"></a>Ausführliche exemplarische Vorgehensweise
### <a name="introduction"></a>Einführung
Wenn Sie eine neue Linux-VM starten, erhalten Sie eine Standard-Linux-VM ohne Anpassung an Ihre Bedürfnisse. [Cloud-Init](https://cloudinit.readthedocs.org) ist eine Standardmethode, ein Skript oder Konfigurationseinstellungen in diese Linux-VM einzufügen, wenn sie zum ersten Mal gestartet wird.

Azure bietet drei verschiedene Möglichkeiten, bei Start oder Bereitstellung Änderungen an einer Linux-VM vorzunehmen.

* Einfügen von Skripts mit Cloud-Init.
* Einfügen von Skripts mithilfe der Azure- [VMAccess-Erweiterung](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
* Eine Azure-Vorlage, die Cloud-Init nutzt.
* Eine Azure-Vorlage, die [CustomScriptExtention](virtual-machines-linux-extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)nutzt.

So fügen Sie Skripts zu einem beliebigen Zeitpunkt nach dem Start ein:

* SSH zur direkten Befehlsausführung
* Fügt Skripts mithilfe der Azure- [VMAccess-Erweiterung](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)ein, imperativ oder in einer Azure-Vorlage.
* Konfigurationsverwaltungstools wie Ansible, Salt, Chef und Puppet.

> [!NOTE]
> : Die VMAccess-Erweiterung führt ein Skript in gleicher Weise als Root aus, wie es mit SSH möglich ist.  Allerdings aktiviert die Verwendung der VM-Erweiterung mehrere Funktionen von Azure, die abhängig von Ihrem Szenario hilfreich sein können.
> 
> 

## <a name="cloud-init-availability-on-azure-vm-quick-create-image-aliases"></a>Cloud-Init-Verfügbarkeit in Azure-VM-Schnellerstellungs-Imagealiasen:
| Alias | Herausgeber | Angebot | SKU | Version | Cloud-Init |
|:--- |:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |neueste |no |
| CoreOS |CoreOS |CoreOS |Stable |neueste |Ja |
| Debian |credativ |Debian |8 |neueste |no |
| openSUSE |SUSE |openSUSE |13.2 |neueste |no |
| RHEL |Redhat |RHEL |7.2 |neueste |no |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |neueste |Ja |

Microsoft arbeitet mit den Partnern zusammen, damit Cloud-Init einbezogen wird und in den Images arbeitet, die sie in Azure bereitstellen.

## <a name="add-a-cloud-init-script-to-the-vm-creation-with-the-azure-cli"></a>Hinzufügen eines Cloud-Init-Skripts zu der VM-Erstellung mit der Azure-Befehlszeilenschnittstelle
Um beim Erstellen eines virtuellen Computers in Azure ein Cloud-Init-Skript zu starten, geben Sie die Cloud-Init-Datei mit der Option `--custom-data` der Azure-Befehlszeilenschnittstelle an.

Erstellen Sie eine Ressourcengruppe, mit der virtuelle Computer gestartet werden können.

Erstellen einer Ressourcengruppe zum Starten von virtuellen Computern in mit [az group create](/cli/azure/group#create. Im folgenden Beispiel wird die Ressourcengruppe `myResourceGroup` erstellt:

```azurecli
az group create --name myResourceGroup --location westus
```

Erstellen Sie eine Linux-VM mit [az vm create](/cli/azure/vm#create) und Cloud-Init, die beim Start konfiguriert wird.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --custom-data cloud-init.txt
```

## <a name="create-a-cloud-init-script-to-set-the-hostname-of-a-linux-vm"></a>Erstellen eines Cloud-Init-Skripts zum Festlegen des Hostnamens einer Linux-VM
Eine der einfachsten und wichtigsten Einstellungen für jede Linux-VM ist die des Hostnamens. Wir können ihn mit diesem Skript leicht mit Cloud-Init festlegen.  

### <a name="example-cloud-init-script-named-cloudconfighostnametxt"></a>Beispiel: Cloud-Init-Skript namens `cloud_config_hostname.txt`.
```sh
#cloud-config
hostname: myservername
```

Während des ersten Starts des virtuellen Computers legt dieses Cloud-Init-Skript den Hostnamen auf `myservername` fest. Erstellen Sie eine Linux-VM mit [az vm create](/cli/azure/vm#create) und Cloud-Init, die beim Start konfiguriert wird.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --custom-data cloud-init.txt
```

Melden Sie sich an und überprüfen Sie den Hostnamen des neuen virtuellen Computers.

```bash
ssh myVM
hostname
myservername
```

## <a name="create-a-cloud-init-script-to-update-linux"></a>Erstellen eines Cloud-Init-Skripts zum Aktualisieren von Linux
Aus Sicherheitsgründen soll Ihre Ubuntu-VM beim ersten Start aktualisiert werden.  Bei Verwendung von Cloud-Init ist dies abhängig von der Linux-Distribution, die Sie verwenden, mit dem folgenden Skript möglich.

### <a name="example-cloud-init-script-cloudconfigaptupgradetxt-for-the-debian-family"></a>Beispiel: Cloud-Init-Skript `cloud_config_apt_upgrade.txt` für die Debian-Familie
```sh
#cloud-config
apt_upgrade: true
```

Nachdem Linux gestartet wurde, werden alle installierten Pakete über `apt-get`aktualisiert. Erstellen Sie eine Linux-VM mit [az vm create](/cli/azure/vm#create) und Cloud-Init, die beim Start konfiguriert wird.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --custom-data cloud_config_apt_upgrade.txt
```

Melden Sie sich an,und überprüfen Sie, ob alle Pakete aktualisiert sind.

```bash
ssh myUbuntuVM
sudo apt-get upgrade
Reading package lists... Done
Building dependency tree
Reading state information... Done
Calculating upgrade... Done
The following packages have been kept back:
  linux-generic linux-headers-generic linux-image-generic
0 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
```

## <a name="create-a-cloud-init-script-to-add-a-user-to-linux"></a>Erstellen eines Cloud-Init-Skripts zum Hinzufügen eines Benutzers zu Linux
Eine der ersten Aufgaben auf jeder neuen Linux-VM ist, das Sie einen Benutzer für sich selbst hinzufügen, um die Verwendung von `root`zu vermeiden. SSH-Schlüssel sind eine bewährte Methode für die Sicherheit und Verwendbarkeit und werden der `~/.ssh/authorized_keys` -Datei mit diesem Cloud-Init-Skript hinzugefügt.

### <a name="example-cloud-init-script-cloudconfigadduserstxt-for-debian-family"></a>Beispiel: Cloud-Init-Skript `cloud_config_add_users.txt` für die Debian-Familie
```sh
#cloud-config
users:
  - name: myCloudInitAddedAdminUser
    groups: sudo
    shell: /bin/bash
    sudo: ['ALL=(ALL) NOPASSWD:ALL']
    ssh-authorized-keys:
      - ssh-rsa AAAAB3<snip>==myAdminUser@myUbuntuVM
```

Nachdem Linux gestartet wurde, werden alle aufgeführten Benutzer erstellt und der Sudo-Gruppe hinzugefügt. Erstellen Sie eine Linux-VM mit [az vm create](/cli/azure/vm#create) und Cloud-Init, die beim Start konfiguriert wird.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image UbuntuLTS \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub \
    --custom-data cloud_config_add_users.txt
```

Melden Sie sich an und überprüfen Sie den neu erstellten Benutzer.

```bash
ssh myVM
cat /etc/group
```

Ausgabe

```bash
root:x:0:
<snip />
sudo:x:27:myCloudInitAddedAdminUser
<snip />
myCloudInitAddedAdminUser:x:1000:
```

## <a name="next-steps"></a>Nächste Schritte
Cloud-Init wird eine standardmäßige Möglichkeit, Ihre Linux-VM beim Start zu ändern. Azure verfügt außerdem über VM-Erweiterungen, mit denen Sie Ihre LinuxVM beim Start oder während der Ausführung ändern können. Sie können z.B. mit der Azure-VMAccess-Erweiterung SSH oder Benutzerinformationen zurücksetzen, während die VM ausgeführt wird. Bei Cloud-Init müssten Sie einen Neustart durchführen, um das Kennwort zurückzusetzen.

[Informationen zu Erweiterungen und Features für virtuelle Computer](virtual-machines-linux-extensions-features.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[Verwalten von Benutzern, SSH und Überprüfen oder Reparieren von Datenträgern auf Azure-Linux-VMs mit der VMAccess-Erweiterung](virtual-machines-linux-using-vmaccess-extension.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)



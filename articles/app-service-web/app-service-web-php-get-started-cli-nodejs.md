---
title: Erstellen, Konfigurieren und Bereitstellen einer PHP-Web-App in Azure | Microsoft-Dokumentation
description: "In diesem Tutorial wird gezeigt, wie Sie eine PHP-Web-App (Laravel) in Azure App Service ausführen. Es wird beschrieben, wie Sie Azure App Service so konfigurieren, dass die Anforderungen des von Ihnen gewählten PHP-Frameworks erfüllt werden."
services: app-service\web
documentationcenter: php
author: cephalin
manager: erikre
editor: 
tags: mysql
ms.assetid: cb73859d-48aa-470a-b486-d984746d6d26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: PHP
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
translationtype: Human Translation
ms.sourcegitcommit: 0921b01bc930f633f39aba07b7899ad60bd6a234
ms.openlocfilehash: 7877662f43b83d6d60c3ef836d14b291245d8d42
ms.lasthandoff: 03/01/2017


---
# <a name="create-configure-and-deploy-a-php-web-app-to-azure"></a>Erstellen, Konfigurieren und Bereitstellen einer PHP-Web-App in Azure
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In diesem Tutorial wird gezeigt, wie Sie eine PHP-Web-App für Azure erstellen, konfigurieren und bereitstellen und Azure App Service so konfigurieren, dass die Anforderungen Ihrer PHP-Web-App erfüllt werden. Am Ende des Tutorials verfügen Sie über eine funktionierende [Laravel](https://www.laravel.com/)-Web-App, die unter [Azure App Service](../app-service/app-service-value-prop-what-is.md) live ausgeführt wird.

Als PHP-Entwickler können Sie Ihr bevorzugtes PHP-Framework in Azure einbinden. In diesem Tutorial wird Laravel einfach als konkretes App-Beispiel verwendet. Sie erhalten Informationen zu folgenden Themen: 

* Bereitstellen mit Git
* Festlegen der PHP-Version
* Verwenden einer Startdatei, die sich nicht im Stammverzeichnis der Anwendung befindet
* Zugreifen auf umgebungsspezifische Variablen
* Aktualisieren der App in Azure

Sie können die hier erlernten Dinge auf andere PHP-Web-Apps anwenden, die Sie unter Azure bereitstellen.

[!INCLUDE [app-service-linux](../../includes/app-service-linux.md)]

## <a name="cli-versions-to-complete-the-task"></a>CLI-Versionen zum Durchführen dieser Aufgabe

Führen Sie die Aufgabe mit einer der folgenden CLI-Versionen durch:

- [Azure CLI 1.0:](app-service-web-php-get-started-cli-nodejs.md) Unsere CLI für das klassische Bereitstellungsmodell und das Resource Manager-Bereitstellungsmodell
- [Azure CLI 2.0:](app-service-web-php-get-started.md) Unsere Befehlszeilenschnittstelle der nächsten Generation für das Resource Manager-Bereitstellungsmodell

## <a name="prerequisites"></a>Voraussetzungen
* [PHP 5.6.29](http://php.net/downloads.php)
* [Composer](https://getcomposer.org/download/)
* [Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md)
* [Git](http://www.git-scm.com/downloads)
* Ein Microsoft Azure-Konto. Falls Sie noch kein Konto haben, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder [Ihre Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

> [!NOTE]
> Sehen Sie sich eine Web-App in Aktion an: [Probieren Sie App Service gleich aus](https://azure.microsoft.com/try/app-service/), und erstellen Sie eine kurzzeitige Start-App – ohne Kreditkarte oder weitere Verpflichtungen.
> 
> 

## <a name="create-a-php-laravel-app-on-your-dev-machine"></a>Erstellen einer PHP-App (Laravel) auf dem Entwicklungscomputer
1. Öffnen Sie eine neue Windows-Eingabeaufforderung, ein PowerShell-Fenster, eine Linux-Shell oder ein OS X-Terminal. Führen Sie die folgenden Befehle aus, um sicherzustellen, dass die erforderlichen Tools richtig auf dem Computer installiert sind. 
   
        php --version
        composer --version
        azure --version
        git --version
   
    Entsprechende Downloadlinks finden Sie bei Bedarf unter [Voraussetzungen](#Prerequisites) .

2. Installieren Sie Laravel wie folgt:
   
        composer global require "laravel/installer"
3. Wechseln Sie mit `CD` in ein Arbeitsverzeichnis, und erstellen Sie wie folgt eine neue Laravel-Anwendung:
   
        cd <working_directory>
        laravel new <app_name>
4. Wechseln Sie mit `CD` in das neu erstellte Verzeichnis `<app_name>`, und testen Sie die App:
   
        cd <app_name>
        php artisan serve
   
    Sie sollten jetzt in einem Browser zu „ http://localhost:8000 “ navigieren und den Laravel-Begrüßungsbildschirm sehen können.
   
    ![Lokales Testen der PHP-App (Laravel) vor der Bereitstellung in Azure](./media/app-service-web-php-get-started/laravel-splash-screen.png)

Dies ist bisher nur der übliche Laravel-Workflow, und das Ziel besteht hier ja auch nicht darin, <a href="https://laravel.com/docs/5.3" rel="nofollow">Laravel zu erlernen</a>. Wir fahren also fort.

## <a name="create-an-azure-web-app-and-set-up-git-deployment"></a>Erstellen einer Azure-Web-App und Einrichten der Git-Bereitstellung
> [!NOTE]
> „Einen Augenblick! Wie gehe ich vor, wenn ich die Bereitstellung per FTP durchführen möchte?“ Für diese Vorgehensweise ist ein [FTP-Tutorial](web-sites-php-mysql-deploy-use-ftp.md) vorhanden. 
> 
> 

Mit der Azure-Befehlszeilenschnittstelle können Sie mit nur einer Codezeile eine Web-App in Azure App Service erstellen und für die Git-Bereitstellung einrichten. Dies werden wir jetzt tun.

1. Wechseln Sie in den ASM-Modus, und melden Sie sich bei Azure an:
   
        azure config mode asm
        azure login
   
    Folgen Sie der Hilfemeldung, um den Anmeldeprozess fortzusetzen.
   
    ![Anmelden an Azure zum Bereitstellen der PHP-App (Laravel) in Azure](./media/app-service-web-php-get-started/log-in-to-azure-cli.png)

3. Legen Sie den Bereitstellungsbenutzer für App Service fest. Sie werden später unter Verwendung der Anmeldeinformationen Code bereitstellen.
   
        azure site deployment user set --username <username> --pass <password>

2. Führen Sie den Befehl zum Erstellen der Azure-Web-App mit der Git-Bereitstellung aus. Geben Sie die Zahl der gewünschten Region ein, wenn Sie dazu aufgefordert werden.
   
        azure site create --git <app_name>
   
    ![Erstellen der Azure-Ressource für Ihre PHP-App (Laravel) in Azure](./media/app-service-web-php-get-started/create-site-cli.png)
   
    Mit diesem Befehl wird im aktuellen Verzeichnis ein neues Git-Repository erstellt (mit `git init`) und mit dem Repository in Azure als Git-Remoterepository verbunden (mit `git remote add`).

<a name="configure"/>

## <a name="configure-the-azure-web-app"></a>Konfigurieren der Azure-Web-App
Damit die Laravel-App in Azure funktioniert, müssen Sie auf einige Dinge achten. Sie führen diese Übung in ähnlicher Weise für Ihr bevorzugtes PHP-Framework durch.

* Konfigurieren Sie PHP 5.6.4 oder höher. Die vollständige Liste mit den Serveranforderungen finden Sie unter den [aktuellen Laravel 5.3-Serveranforderungen](https://laravel.com/docs/5.3#server-requirements). Bei den restlichen Einträgen der Liste handelt es sich um Erweiterungen, die von den PHP-Installationen von Azure bereits aktiviert wurden. 
* Legen Sie die Umgebungsvariablen gemäß den Anforderungen Ihrer App fest. Laravel nutzt die Datei `.env` zum einfachen Festlegen der Umgebungsvariablen. Da aber kein Commit in die Quellcodeverwaltung vorgesehen ist (siehe [Laravel-Umgebungskonfiguration](https://laravel.com/docs/5.3/configuration#environment-configuration)), legen Sie stattdessen die App-Einstellungen Ihrer Azure-Web-App fest.
* Stellen Sie sicher, dass der Einstiegspunkt der Laravel-App ( `public/index.php`) zuerst geladen wird. Weitere Informationen finden Sie unter [Laravel Lifecycle Overview](https://laravel.com/docs/5.3/lifecycle#lifecycle-overview)(Laravel – Übersicht über den Lebenszyklus). Anders ausgedrückt: Sie müssen die Stamm-URL der Web-App so festlegen, dass sie auf das Verzeichnis `public` zeigt.
* Aktivieren Sie die Composer-Erweiterung in Azure, da Sie über die Datei „composer.json“ verfügen. Sie können das Abrufen der erforderlichen Pakete dann Composer überlassen, wenn Sie die Bereitstellung mit `git push`durchführen. Dies ist lediglich eine Frage der Benutzerfreundlichkeit. 
  Wenn Sie die Composer-Automation nicht aktivieren, müssen Sie nur `/vendor` aus der Datei `.gitignore` entfernen, damit Git beim Übernehmen und Bereitstellen von Code alle Elemente im Verzeichnis `vendor` einschließt (Aufhebung von „Ignorieren“).

Wir werden diese Aufgaben nun nacheinander konfigurieren.

1. Legen Sie die PHP-Version fest, die für die Laravel-App erforderlich ist.
   
        azure site set --php-version 5.6
   
    Sie haben das Festlegen der PHP-Version hiermit abgeschlossen. 
2. Generieren Sie ein neues `APP_KEY` -Element für Ihre Azure-Web-App, und legen Sie es als App-Einstellung für die Azure-Web-App fest.
   
        php artisan key:generate --show
        azure site appsetting add APP_KEY="<output_of_php_artisan_key:generate_--show>"
3. Aktivieren Sie auch das Laravel-Debuggen, um dem Anzeigen der kryptischen Seite `Whoops, looks like something went wrong.` vorzubeugen.
   
        azure site appsetting add APP_DEBUG=true
   
    Sie haben das Festlegen von Umgebungsvariablen abgeschlossen.
   
   > [!NOTE]
   > Wir gehen aber erst einmal langsam vor und beschreiben, welche Abläufe hier in Laravel und in Azure stattfinden. Laravel verwendet die Datei `.env` im Stammverzeichnis, um Umgebungsvariablen für die App bereitzustellen. Darin ist die Zeile `APP_DEBUG=true` enthalten (und die Zeile `APP_KEY=...`). Auf diese Variable wird in `config/app.php` mit dem Code `'debug' => env('APP_DEBUG', false),` zugegriffen. [env()](https://laravel.com/docs/5.3/helpers#method-env) ist eine Laravel-Hilfsmethode, die das PHP-Element [getenv()](http://php.net/manual/en/function.getenv.php) im Hintergrund einsetzt.
   > 
   > `.env` wird von Git aber ignoriert, da die Datei `.gitignore` im Stammverzeichnis verwendet wird. Einfach ausgedrückt: `.env` im lokalen Git-Repository wird nicht zusammen mit den restlichen Dateien an Azure übertragen. Sie können diese Zeile natürlich einfach aus `.gitignore`entfernen, aber es wurde bereits erwähnt, dass die Übernahme dieser Datei in die Quellcodeverwaltung nicht zu empfehlen ist. Trotzdem benötigen Sie eine Möglichkeit zum Angeben dieser Umgebungsvariablen in Azure. 
   > 
   > Die gute Nachricht ist, dass App-Einstellungen in Azure App Service [getenv()](http://php.net/manual/en/function.getenv.php) in PHP unterstützen. Sie können zwar FTP oder andere Hilfsmittel zum manuellen Hochladen einer `.env`-Datei in Azure verwenden, aber Sie können die gewünschten Variablen – wie eben durchgeführt – auch einfach als Azure-App-Einstellungen ohne `.env` in Azure angeben. Wenn eine Variable sowohl in einer `.env` -Datei als auch in Azure-App-Einstellungen enthalten ist, hat die Azure-App-Einstellung Vorrang.     
   > 
   > 
4. Für die letzten beiden Aufgaben (Festlegen des virtuellen Verzeichnisses und Aktivieren von Composer) ist das [Azure-Portal](https://portal.azure.com) erforderlich. Melden Sie sich also mit Ihrem Azure-Konto beim [Portal](https://portal.azure.com) an.
5. Klicken Sie im linken Menü auf **App Services** > **&lt;App-Name>** > **Erweiterungen**.
   
    ![Aktivieren von Composer für die PHP-App (Laravel) in Azure](./media/app-service-web-php-get-started/configure-composer-tools.png)
   
6. Klicken Sie auf **Hinzufügen**, um eine Erweiterung hinzuzufügen.
7. Wählen Sie auf dem [Blatt](../azure-resource-manager/resource-group-portal.md#manage-resources) **Erweiterung wählen** die Option **Composer**. (Ein *Blatt* ist eine Portalseite, die horizontal geöffnet wird.)
8. Klicken Sie auf dem [Blatt](../azure-resource-manager/resource-group-portal.md#manage-resources) **Rechtliche Bedingungen akzeptieren** auf **OK**. 
9. Klicken Sie auf dem [Blatt](../azure-resource-manager/resource-group-portal.md#manage-resources) **Erweiterung hinzufügen** auf **OK**.
   
    Nachdem die Erweiterung in Azure hinzugefügt wurde, wird in der Ecke eine benutzerfreundliche Popupmeldung angezeigt, und auf dem [Blatt](../azure-resource-manager/resource-group-portal.md#manage-resources) **Erweiterungen** ist der Eintrag  **Composer** enthalten.
   
    ![Blatt „Erweiterungen“ nach dem Aktivieren von Composer für die PHP-App (Laravel) in Azure](./media/app-service-web-php-get-started/configure-composer-end.png)
   
    Sie haben die Aktivierung von Composer abgeschlossen.
10. Klicken Sie auf dem [Ressourcenblatt](../azure-resource-manager/resource-group-portal.md#manage-resources) Ihrer Web-App auf **Anwendungseinstellungen**.
    
     ![Blatt „Zugriffseinstellungen“ zum Festlegen des virtuellen Verzeichnisses für die PHP-App (Laravel) in Azure](./media/app-service-web-php-get-started/configure-virtual-dir-settings.png)
    
     Beachten Sie im Blatt **Anwendungseinstellungen** die PHP-Version, die Sie weiter oben festgelegt haben:
    
     ![PHP-Version im Blatt „Einstellungen“ für die PHP-App (Laravel) in Azure](./media/app-service-web-php-get-started/configure-virtual-dir-settings-a.png)
    
     Beachten Sie auch die App-Einstellungen, die Sie hinzugefügt haben:
    
     ![App-Einstellungen im Blatt „Einstellungen“ für die PHP-App (Laravel) in Azure](./media/app-service-web-php-get-started/configure-virtual-dir-settings-b.png)
11. Scrollen Sie zum unteren Rand des [Blatts](../azure-resource-manager/resource-group-portal.md#manage-resources), und ändern Sie das virtuelle Stammverzeichnis so, dass es nicht auf **site\wwwroot**, sondern auf **site\wwwroot\public** verweist.
    
     ![Festlegen des virtuellen Verzeichnisses für die PHP-App (Laravel) in Azure](./media/app-service-web-php-get-started/configure-virtual-dir-public.png)
12. Klicken Sie oben auf dem [Blatt](../azure-resource-manager/resource-group-portal.md#manage-resources) auf **Speichern**.
    
     Sie haben das Festlegen des virtuellen Verzeichnisses abgeschlossen. 

## <a name="deploy-your-web-app-with-git-and-setting-environment-variables"></a>Bereitstellen der Web-App mit Git (und Festlegen von Umgebungsvariablen)
Es ist jetzt alles für die Bereitstellung des Codes vorbereitet. Hierfür verwenden Sie die Befehlseingabeaufforderung oder das Terminal.

1. Übernehmen Sie alle Änderungen, und stellen Sie Ihren Code für die Azure-Web-App so bereit, wie Sie dies auch für jedes Git-Repository durchführen würden:
   
        git add .
        git commit -m "Hurray! My first commit for my Azure app!"
        git push azure master 
   
    Melden Sie sich mithilfe der zuvor erstellten Benutzeranmeldeinformationen an, wenn Sie dazu aufgefordert werden. 

2. Wir verwenden für die Ausführung im Browser den folgenden Befehl:
   
        azure site browse
   
    Im Browser sollte nun der Laravel-Begrüßungsbildschirm angezeigt werden.
   
    ![Laravel-Begrüßungsbildschirm nach dem Bereitstellen der Web-App in Azure](./media/app-service-web-php-get-started/laravel-azure-splash-screen.png)
   
    Glückwunsch! Sie führen eine Laravel-Web-App in Azure aus.

## <a name="troubleshoot-common-errors"></a>Problembehandlung für häufige Fehler
Hier sind einige Fehler aufgeführt, die beim Durcharbeiten des Tutorials auftreten können:

* [Azure-Befehlszeilenschnittstelle zeigt „‚site‘ is not an azure command“ an](#clierror)
* [Web-App zeigt HTTP 403-Fehler an](#http403)
* [Web-App zeigt „Whoops, looks like something went wrong.“ an](#whoops)
* [Web-App zeigt „Keine unterstützte Verschlüsselung gefunden.“ an](#encryptor)

<a name="clierror"></a>

### <a name="azure-cli-shows-site-is-not-an-azure-command"></a>Azure-Befehlszeilenschnittstelle zeigt „‚site‘ is not an azure command“ an
Beim Ausführen von `azure site *` im Befehlszeilenterminal wird der folgende Fehler angezeigt: `error:   'site' is not an azure command. See 'azure help'.` 

Dies ist normalerweise das Ergebnis eines Wechsels in den Modus „ARM“ (Azure Resource Manager). Wechseln Sie zum Beheben des Fehlers zurück in den Modus „ASM“ (Azure Service Management), indem Sie `azure config mode asm`ausführen.

<a name="http403"></a>

### <a name="web-app-shows-http-403-error"></a>Web-App zeigt HTTP 403-Fehler an
Sie haben Ihre Web-App erfolgreich unter Azure bereitgestellt, aber wenn Sie zur Azure-Web-App navigieren, wird Folgendes angezeigt: `HTTP 403` oder `You do not have permission to view this directory or page.`

Der wahrscheinlichste Grund ist, dass die Web-App den Einstiegspunkt der Laravel-App nicht finden kann. Vergewissern Sie sich, dass Sie das virtuelle Stammverzeichnis so geändert haben, dass es auf `site\wwwroot\public` mit der Datei `index.php` von Laravel zeigt (siehe [Konfigurieren der Azure-Web-App](#configure)).

<a name="whoops"></a>

### <a name="web-app-shows-whoops-looks-like-something-went-wrong"></a>Web-App zeigt „Whoops, looks like something went wrong.“ an
Sie haben Ihre Web-App erfolgreich unter Azure bereitgestellt, aber wenn Sie zur Azure-Web-App navigieren, wird die folgende kryptische Meldung angezeigt: `Whoops, looks like something went wrong.`

Um eine aussagekräftigere Fehlermeldung zu erhalten, aktivieren Sie das Laravel-Debugging, indem Sie die Umgebungsvariable `APP_DEBUG` auf `true` festlegen (siehe [Konfigurieren der Azure-Web-App](#configure)).

<a name="encryptor"></a>

### <a name="web-app-shows-no-supported-encryptor-found"></a>Web-App zeigt „Keine unterstützte Verschlüsselung gefunden.“ an
Sie haben Ihre Web-App erfolgreich unter Azure bereitgestellt, aber wenn Sie zur Azure-Web-App navigieren, wird die folgende Fehlermeldung angezeigt:

![APP_KEY fehlt in der PHP-App (Laravel) in Azure](./media/app-service-web-php-get-started/laravel-error-APP_KEY.png)

Das ist ein relativ unangenehmer Fehler, aber wenigstens ist er nicht kryptisch, da Sie das Laravel-Debugging aktiviert haben. Eine erste Suche nach der Fehlerzeichenfolge in den Laravel-Foren führt zu der Ursache, dass der „APP_KEY“ in der `.env`-Datei nicht festgelegt wurde (bzw. in diesem Fall das vollständige Fehlen der `.env`-Datei in Azure). Sie können dies beheben, indem Sie die Einstellung `APP_KEY` als Azure-App-Einstellung hinzufügen (siehe [Konfigurieren der Azure-Web-App](#configure)).

## <a name="next-steps"></a>Nächste Schritte
Informieren Sie sich darüber, wie Sie Ihrer App Daten hinzufügen, indem Sie [in Azure eine MySQL-Datenbank erstellen](../store-php-create-mysql-database.md). Verwenden Sie auch die folgenden Links, die für PHP in Azure hilfreicher sind:

* [PHP Developer Center](/develop/php/)
* [Erstellen einer Web-App aus dem Azure Marketplace](app-service-web-create-web-app-from-marketplace.md)
* [Konfigurieren von PHP in Azure App Service-Web-Apps](web-sites-php-configure.md)
* [Konvertieren von WordPress in Multisite in Azure App Service](web-sites-php-convert-wordpress-multisite.md)
* [Leistungsstarkes WordPress in Azure App Service](web-sites-php-enterprise-wordpress.md)



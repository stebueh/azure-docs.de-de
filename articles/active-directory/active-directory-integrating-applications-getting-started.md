---
title: Erste Schritte bei der Integration von Apps in Azure AD | Microsoft-Dokumentation
description: "In diesem Artikel sind die ersten Schritte für die Integration von lokalen Anwendungen und Cloudanwendungen in Azure Active Directory (AD)  aufgeführt."
services: active-directory
documentationcenter: 
author: ihenkel
manager: femila
editor: 
ms.assetid: db6d210d-c970-49e9-bd20-36d984bcd1c3
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 01/23/2017
ms.author: inhenk
translationtype: Human Translation
ms.sourcegitcommit: 54e6bf0953135855d4dda2bc83c8dc06212c6eb5
ms.openlocfilehash: 6b3f6889c747e72ee1c592b1b0dfb1d03d700ad3


---
# <a name="integrating-azure-active-directory-with-applications-getting-started-guide"></a>Erste Schritte zur Integration von Anwendungen in Azure Active Directory
## <a name="overview"></a>Übersicht
Dieses Thema bietet Ihnen eine Roadmap für die Integration von Anwendungen in Azure Active Directory (AD). Jeder der folgenden Abschnitte enthält eine kurze Zusammenfassung eines ausführlicheren Themas, damit Sie feststellen können, welche Teile dieses Erste-Schritte-Leitfadens für Sie relevant sind.  Folgen Sie den Links, um eingehendere Informationen zu den einzelnen Themen zu erhalten.

## <a name="before-you-begin-take-inventory"></a>Voraussetzungen – Bestandsaufnahme
Vor der Integration von Anwendungen in Azure AD sollten Sie unbedingt den aktuellen Stand ermitteln und festlegen, was Sie erreichen möchten.  Die folgenden Fragen helfen Ihnen bei Ihren Überlegungen zu Ihrem Azure AD-Projekt zur Anwendungsintegration.

### <a name="application-inventory"></a>Anwendungsbestand
* Wo befinden sich all Ihre Anwendungen? Wer ist der Eigentümer?
* Welche Art der Authentifizierung ist für Ihre Anwendungen erforderlich?
* Wer benötigt Zugriff auf welche Anwendungen?
* Möchten Sie eine neue Anwendung bereitstellen?
  * Wird diese innerbetrieblich erstellt und auf einer Azure Compute-Instanz bereitgestellt?
  * Möchten Sie eine Anwendung verwenden, die im Azure-Anwendungskatalog verfügbar ist?

### <a name="user-and-group-inventory"></a>Benutzer- und Gruppenbestand
* Wo befinden sich Ihre Benutzerkonten?
  * Lokales Active Directory
  * Azure AD
  * In einer separaten Anwendungsdatenbank, die Sie besitzen
  * In ungenehmigten Anwendungen
  * Alle oben genannten Möglichkeiten
* Welche Berechtigungen und Rollenzuweisungen gelten derzeit für die einzelnen Benutzer? Möchten Sie den Zugriff überprüfen, oder sind Sie sicher, dass die aktuellen Zugriffs- und Rollenzuweisungen angemessen sind?
* Sind in Ihrem lokalen Active Directory bereits Gruppen eingerichtet?
  * Wie sind Ihre Gruppen organisiert?
  * Wer sind die Mitglieder der Gruppen?
  * Welche Berechtigungen/Rollenzuweisungen gelten derzeit für die Gruppen?
* Müssen Sie vor der Integration Benutzer-/Gruppendatenbanken bereinigen?  (Dies ist eine ziemlich wichtige Frage. Bei einer fehlerhaften Ausgangslage lässt sich kein optimales Ergebnis erzielen – „Garbage In, Garbage Out“.)

### <a name="access-management-inventory"></a>Bestand der Zugriffsverwaltung
* Wie verwalten Sie derzeit den Benutzerzugriff auf Anwendungen? Muss daran etwas geändert werden?  Haben Sie andere Methoden zum Verwalten des Zugriffs in Erwägung gezogen, beispielsweise [RBAC](role-based-access-control-configure.md)?
* Wer muss worauf zugreifen?

Vielleicht können Sie nicht alle Fragen im Voraus beantworten, aber das ist kein Problem.  Dieser Leitfaden hilft Ihnen, einige dieser Fragen zu beantworten und einige fundierte Entscheidungen zu treffen.

## <a name="prerequisites"></a>Voraussetzungen
* Ein Azure-Abonnement und ein Azure Active Directory-Verzeichnis.  Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Azure für 30 Tage kostenlos testen. [Probieren Sie es aus!](https://azure.microsoft.com/trial/get-started-active-directory/)

## <a name="application-integration-with-azure-ad"></a>Anwendungsintegration in Azure AD
### <a name="finding-unsanctioned-cloud-applications-with-cloud-app-discovery"></a>Suchen nach nicht genehmigten Cloudanwendungen per Cloud App Discovery
Wie oben erwähnt sind möglicherweise Anwendungen im Einsatz, die bis jetzt von Ihrer Organisation noch nicht verwaltet wurden.  Im Rahmen der Bestandsaufnahme können Sie nicht genehmigte Cloudanwendungen finden. Weitere Informationen hierzu finden Sie unter [Finden nicht genehmigter Cloudanwendungen mit Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).

### <a name="authentication-types"></a>Authentifizierungstypen
Für jede Ihrer Anwendungen gelten möglicherweise unterschiedliche Authentifizierungsanforderungen. Bei Azure AD können Signaturzertifikate mit Anwendungen verwendet werden, die die Protokolle SAML 2.0, WS-Verbund oder OpenID Connect sowie das einmalige Anmelden per Kennwort verwenden. Weitere Informationen zu Anwendungsauthentifizierungstypen, die mit Azure AD verwendet werden können, finden Sie unter [Verwalten von Zertifikaten für die einmalige Verbundanmeldung in Azure Active Directory](active-directory-sso-certs.md) und unter [Kennwortbasierte einmalige Anmeldung](active-directory-appssoaccess-whatis.md).

### <a name="enabling-sso-with-azure-ad-app-proxy"></a>Aktivieren von SSO mit Azure AD-App-Proxy
Mit dem Microsoft Azure AD-Anwendungsproxy können Sie sicheren Zugriff von jedem Ort und mit jedem Gerät auf Anwendungen bereitstellen, die sich in Ihrem privaten Netzwerk befinden. Nachdem Sie einen Anwendungsproxy-Connector in Ihrer Umgebung installiert haben, können Sie ihn mit Azure AD leicht konfigurieren.

### <a name="integrating-applications-with-azure-ad"></a>Integrieren von Anwendungen in Azure Active Directory
In den folgenden Artikeln werden die verschiedenen Methoden zur Integration von Anwendungen in Azure AD erläutert und Anleitungen bereitgestellt.

* [Bestimmen des zu verwendenden Active Directory-Verzeichnisses](active-directory-administer.md)
* [Verwenden von Anwendungen im Azure-Anwendungskatalog](active-directory-appssoaccess-whatis.md)
* [Liste der Tutorials zur Integration von SaaS-Anwendungen](active-directory-saas-tutorial-list.md)

## <a name="managing-access-to-applications"></a>Verwalten des Zugriffs auf Anwendungen
In den folgenden Artikeln werden Möglichkeiten zur Verwaltung des Zugriffs auf Anwendungen beschrieben, nachdem sie mithilfe von Azure AD-Connectors und Azure AD in Azure AD integriert wurden.

* [Verwalten des Zugriffs auf Apps mit Azure AD](active-directory-managing-access-to-apps.md)
* [Automatisieren mit Azure AD-Connectors](active-directory-saas-app-provisioning.md)
* [Zuweisen von Benutzern zu einer Anwendung](active-directory-applications-guiding-developers-assigning-users.md)
* [Zuweisen von Gruppen zu einer Anwendung](active-directory-applications-guiding-developers-assigning-groups.md)
* [Gemeinsames Verwenden von Konten](active-directory-sharing-accounts.md)

## <a name="integrating-custom-applications"></a>Integrieren benutzerdefinierter Anwendungen
Wenn Sie eine neue Anwendung schreiben und Entwickler beim Nutzen von Azure AD unterstützen möchten, finden Sie unter [Leitfaden für Entwickler](active-directory-applications-guiding-developers-for-lob-applications.md)weitere Informationen.

Wenn Sie Ihre benutzerdefinierte Anwendung dem Azure-Anwendungskatalog hinzufügen möchten, finden Sie weitere Informationen unter [“Bring your own app” with Azure AD Self-Service SAML configuration](http://blogs.technet.com/b/ad/archive/2015/06/17/bring-your-own-app-with-azure-ad-self-service-saml-configuration-gt-now-in-preview.aspx)(„Bring your own app“ mit Azure AD-Konfiguration für Self-Service SAML).

## <a name="see-also"></a>Weitere Informationen
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)




<!--HONumber=Feb17_HO1-->



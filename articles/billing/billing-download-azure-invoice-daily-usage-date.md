---
title: "Abrufen von Azure-Rechnungen und täglichen Nutzungsdaten | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Azure-Rechnungen und tägliche Nutzungsdaten abrufen."
services: 
documentationcenter: 
author: genlin
manager: ruchic
editor: 
tags: billing
ms.assetid: 6d568d1d-3bd6-4348-97d0-1098b5fe0661
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: genli
translationtype: Human Translation
ms.sourcegitcommit: 4d974590ee7677a4f3fcebd9e2474c99a974f663
ms.openlocfilehash: 60ac57fd22f237b7fe62773486fe46913aca8dd0


---
# <a name="how-to-get-your-azure-billing-invoice-and-daily-usage-data"></a>Abrufen von Azure-Rechnungen und täglichen Nutzungsdaten
Sie können die entsprechende Option aktivieren und zusätzliche Empfänger konfigurieren, die Ihre Azure-Rechnung in einer E-Mail erhalten. Sie können Ihre Rechnung auch aus dem [Azure-Portal](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) herunterladen. Darüber hinaus können Sie eine CSV-Datei mit Nutzungsdaten aus dem [Azure-Kontocenter](https://account.windowsazure.com) herunterladen. Nur der Kontoadministrator hat die Berechtigung für die Abrechnung und Nutzungsinformationen. Den Kontoadministrator des Abonnements finden Sie unter [Übertragen eines Azure-Abonnements – Häufig gestellte Fragen](billing-subscription-transfer.md#faq).

## <a name="get-your-invoice-over-email-pdf"></a>Empfangen Ihrer Rechnung per E-Mail (PDF)
1. Wählen Sie auf dem [Blatt „Abonnements“](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) Ihr Abonnement aus. Sie müssen die Option für jedes Abonnement aktivieren, das Sie besitzen. Klicken Sie auf **Rechnungen** und dann auf **Rechnung per E-Mail**. Wenn Sie nicht der Kontoadministrator sind, wird diese Option möglicherweise nicht angezeigt.

    ![Screenshot mit dem Ablauf der Aktivierung](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)
    
2. Klicken Sie auf **Aktivieren**, und stimmen Sie den Bedingungen zu:

    ![Screenshot mit dem Ablauf der Aktivierung](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)
 
3. Nachdem Sie die Vereinbarung akzeptiert haben, können Sie zusätzliche Empfänger konfigurieren:

    ![Screenshot mit dem Ablauf der Aktivierung](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)
    
Sie können auch in der E-Mail mit der Benachrichtigung über den monatlichen Kontoauszug auf den Deep-Link dieses Blatts zugreifen.

### <a name="i-cant-access-the-email-settings-blade"></a>Der Zugriff auf das Blatt mit den E-Mail-Einstellungen ist nicht möglich
* Sie müssen der Kontoadministrator sein, um diese Einstellung zu konfigurieren. Sie sind nicht sicher, was dies bedeutet? [Hier erhalten Sie weitere Informationen](billing-add-change-azure-subscription-administrator.md).
* Wenn Sie eine monatliche Rechnung haben, aber keine E-Mail empfangen, stellen Sie sicher, dass die [E-Mail für die Kommunikation ordnungsgemäß festgelegt wurde](https://account.windowsazure.com/profile).
* Diese Funktion ist für bestimmte Abonnements möglicherweise nicht verfügbar, z.B. für Supportangebote, Enterprise Agreements oder Azure in Open.

## <a name="download-invoice-from-azure-portal-pdf"></a>Herunterladen der Rechnung aus dem Azure-Portal (PDF)

1. Melden Sie sich als Kontoadministrator beim [Azure-Portal](https://portal.azure.com) an. 
2. Wählen Sie im Menü „Hub“ die Option **Abonnements** aus. 

    ![Screenshot mit der Abonnementoption](./media/billing-download-azure-invoice-daily-usage-date/submenu.png) 

3. Wählen Sie auf dem Blatt **Abonnements** das anzuzeigende Abonnement aus, und wählen Sie dann **Abrechnung und Nutzung**. 

    ![Screenshot mit der Option „Abrechnung und Nutzung“](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png) 

4. Klicken Sie auf dem Blatt **Abrechnung und Nutzung** auf **Rechnung herunterladen**, um eine Kopie Ihrer PDF-Rechnung anzuzeigen. 

    ![Screenshot mit Abrechnungszeiträumen, der Option „Herunterladen“ und den Gesamtgebühren für jeden Abrechnungszeitraum](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

5. Sie können Ihre Daten zur tägliche Nutzung durch Klicken auf den Abrechnungszeitraum anzeigen. 

Weitere Informationen über Ihre Rechnung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](billing-understand-your-bill.md).

## <a name="download-usage-from-the-account-center-csv"></a>Herunterladen der Nutzungsdaten aus dem Kontocenter (CSV)
1. Melden Sie sich als Kontoadministrator im [Azure-Kontocenter](https://account.windowsazure.com/subscriptions) an.
2. Wählen Sie das Abonnement, für das Sie die Rechnung und Nutzungsinformationen abrufen möchten.
3. Klicken Sie auf **ABRECHNUNGSVERLAUF**. 

    ![Screenshot mit Abrechnungsverlaufsoption](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. Sie können Ihre Aufstellungen für die letzten sechs Abrechnungszeiträume sowie den aktuellen, nicht berechneten Zeitraum anzeigen. 

    ![Screenshot, der Abrechnungszeiträume, Optionen zum Herunterladen von Rechnungen und Daten zur täglichen Nutzung sowie Gesamtgebühren für jeden Abrechnungszeitraum anzeigt](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. Wählen Sie **Aktuelle Abrechnung anzeigen** , um eine Schätzung der Kosten zum Zeitpunkt der Erstellung der Schätzung anzuzeigen. Diese Informationen werden nur einmal täglich aktualisiert und umfassen möglicherweise nicht die gesamte Nutzung. Ihre monatliche Abrechnung kann von dieser Schätzung abweichen.

    ![Screenshot mit der Option „Aktuelle Erklärung anzeigen“](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![Screenshot mit der Schätzung der aktuellen Gebühren](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. Wählen Sie **Nutzung herunterladen**, um die täglichen Nutzungsdaten als CSV-Datei herunterzuladen. Wenn zwei Versionen verfügbar sind, laden Sie Version 2 herunter.

    ![Screenshot mit der Option „Nutzung herunterladen“](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

Weitere Informationen über Ihre Daten zur täglichen Nutzung finden Sie unter [Erläuterungen zur Rechnung für Microsoft Azure](billing-understand-your-bill.md).

## <a name="a-namenoinvoicea-why-dont-i-see-an-invoice-for-the-last-billing-period"></a><a name="noinvoice"></a> Warum sehe ich keine Rechnung für den letzten Abrechnungszeitraum?
Mehrere Gründe können dafür ausschlaggebend sein, dass Sie keine Rechnung sehen:
- Sie haben einen monatlichen Guthabenbetrag in Ihrem Abonnement, den Sie nicht überschritten haben, oder eine kostenlose Testversion. Eine Rechnung wird nur generiert, wenn Sie Geld schuldig sind.
- Es sind weniger als 30 Tage seit dem Abschluss Ihres Azure-Abonnements vergangen.
- Die Rechnung ist nicht noch erstellt. Warten Sie bis zum Ende des Abrechnungszeitraums.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
Bei weiteren Fragen [wenden Sie sich an den Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um das Problem schnell zu lösen.




<!--HONumber=Feb17_HO3-->



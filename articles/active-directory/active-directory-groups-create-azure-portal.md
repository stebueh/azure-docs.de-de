---
title: Erstellen einer neuen Gruppe in der Azure Active Directory-Vorschau | Microsoft Docs
description: "Erstellen einer Gruppe in Azure Active Directory und Hinzufügen von Benutzern (Mitgliedern) zur Gruppe"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/13/2017
ms.author: curtand
translationtype: Human Translation
ms.sourcegitcommit: 24a6ae1b259a1524ec3a8542dcbf55ff0450421e
ms.openlocfilehash: ab74ff51f6dd1af91c4ec4d5de570ca30fb963bd


---
# <a name="create-a-new-group-in-azure-active-directory-preview"></a>Erstellen einer neuen Gruppe in der Azure Active Directory-Vorschau
> [!div class="op_single_selector"]
> * [Azure-Portal](active-directory-groups-create-azure-portal.md)
> * [Klassisches Azure-Portal](active-directory-accessmanagement-manage-groups.md)
> * [PowerShell](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

In diesem Artikel wird erläutert, wie Sie eine neue Gruppe in der Azure Active Directory-Vorschau (Azure AD) erstellen und auffüllen. [Was bietet die Vorschauversion?](active-directory-preview-explainer.md) Sie verwenden eine Gruppe zum Durchführen von Verwaltungsaufgaben, z.B. für das Zuweisen von Lizenzen oder Berechtigungen für mehrere Benutzer oder Geräte in einem Arbeitsschritt.

## <a name="how-do-i-create-a-group"></a>Wie erstelle ich eine Gruppe?
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com) über ein Konto an, das als globaler Administrator für das Verzeichnis konfiguriert ist.
2. Wählen Sie **Weitere Dienste** aus, geben Sie **Benutzer und Gruppen** in das Textfeld ein, und drücken Sie die EINGABETASTE****.

   ![Öffnen der Benutzerverwaltung](./media/active-directory-groups-create-azure-portal/search-user-management.png)
3. Wählen Sie auf dem Blatt **Benutzer und Gruppen** die Option **Alle Gruppen** aus.

   ![Öffnen des Blatts „Gruppen“](./media/active-directory-groups-create-azure-portal/view-groups-blade.png)
4. Wählen Sie auf dem Blatt **Benutzer und Gruppen** den Befehl **Hinzufügen** aus.

   ![Auswählen des Befehls „Hinzufügen“](./media/active-directory-groups-create-azure-portal/add-group-command.png)
5. Fügen Sie auf dem Blatt **Gruppe** einen Namen und eine Beschreibung für die Gruppe hinzu.
6. Um der Gruppe Mitglieder hinzuzufügen, wählen Sie im Textfeld **Mitgliedschaftstyp** die Einstellung **Zugewiesen** aus und klicken dann auf **Mitglieder**. Weitere Informationen zum dynamischen Verwalten der Mitgliedschaft finden Sie unter [Verwenden von Attributen zum Erstellen erweiterter Regeln für Gruppenmitgliedschaften](active-directory-groups-dynamic-membership-azure-portal.md).

   ![Auswählen der hinzuzufügenden Mitglieder](./media/active-directory-groups-create-azure-portal/select-members.png)
7. Wählen Sie auf dem Blatt **Mitglieder** mindestens einen Benutzer oder ein Gerät aus, den/das Sie der Gruppe hinzufügen möchten, und klicken Sie auf die Schaltfläche **Auswählen** im unteren Bereich des Blatts, um das Element zur Gruppe hinzuzufügen. Das Feld **Benutzer** filtert die Anzeige basierend auf einem Abgleich Ihrer Eingabe mit einem beliebigen Teil eines Benutzer- oder Gerätenamens. In diesem Feld können keine Platzhalterzeichen verwendet werden.
8. Wenn Sie alle gewünschten Mitglieder zur Gruppe hinzugefügt haben, wählen Sie auf dem Blatt **Gruppe** die Option **Erstellen** aus.    

   ![Bestätigen der Gruppenerstellung](./media/active-directory-groups-create-azure-portal/create-group-confirmation.png)

## <a name="next-steps"></a>Nächste Schritte
Diese Artikel enthalten zusätzliche Informationen zu Azure Active Directory.

* [Anzeigen vorhandener Gruppen](active-directory-groups-view-azure-portal.md)
* [Verwalten der Einstellungen einer Gruppe](active-directory-groups-settings-azure-portal.md)
* [Verwalten der Mitglieder einer Gruppe](active-directory-groups-members-azure-portal.md)
* [Verwalten der Mitgliedschaften einer Gruppe](active-directory-groups-membership-azure-portal.md)
* [Verwalten dynamischer Regeln für Benutzer in einer Gruppe](active-directory-groups-dynamic-membership-azure-portal.md)



<!--HONumber=Feb17_HO2-->



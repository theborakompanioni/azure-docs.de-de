---
title: "Anmeldung beim Azure-Abonnement nicht möglich | Microsoft-Dokumentation"
description: "Es wird beschrieben, wie Sie die Problembehandlung für einige gängige Probleme bei der Anmeldung am Azure-Abonnement durchführen können."
services: 
documentationcenter: 
author: genlin
manager: jlian
editor: 
tags: billing
ms.assetid: d1545298-99db-4941-8e97-f24a06bb7cb6
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/2/2016
ms.author: genli
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 030b41eaa405ff70081a4c15cabded08a1b0b07a


---
# <a name="i-cant-sign-in-to-manage-my-azure-subscription"></a>Ich kann mich nicht anmelden, um mein Azure-Abonnement zu verwalten
Dieser Artikel führt Sie durch einige der am häufigsten verwendeten Methoden, um Anmeldungsprobleme zu beheben.

## <a name="page-hangs-in-the-loading-status"></a>Seite hängt im Ladestatus
Wenn Ihr Internetbrowser hängt, können Sie nacheinander die folgenden Schritte ausprobieren, bis Sie auf das [Azure-Portal](https://portal.azure.com) zugreifen können.

* Aktualisieren Sie die Seite.
* Verwenden Sie einen anderen Internetbrowser.
* Wenn Sie Microsoft Internet Explorer verwenden, navigieren Sie im InPrivate-Browsermodus zum Azure-Portal. 
  
  A:    Klicken Sie auf **Extras** ![Schaltfläche „Extras“](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Sicherheit** > **InPrivate-Browsen**.
  
  B.    Wechseln Sie zum [Azure-Portal](https://portal.azure.com), und melden Sie sich beim Portal an.

## <a name="error-message-no-subscriptions-found"></a>Fehlermeldung „Keine Abonnements gefunden“
Wenn Ihr Konto nicht über ausreichende Berechtigungen verfügt, wird ggf. die Fehlermeldung **Keine Abonnements gefunden** angezeigt. Stellen Sie sicher, dass Sie als der richtige Administrator angemeldet sind. Ein Kontoadministrator hat nur Zugriff auf das [Kontocenter](https://account.windowsazure.com/Subscriptions). Dienstadministratoren und Co-Administratoren haben hingegen nur Zugriff auf das [Azure-Portal](https://portal.azure.com) oder das klassische Azure-Portal.

**Szenario 1: Fehlermeldung wird im [Azure-Portal](https://portal.azure.com)** empfangen

Zum Beheben dieses Problems [fügen Sie die Co-Administrator- oder Besitzerrolle](billing-add-change-azure-subscription-administrator.md) für das Konto hinzu.

**Szenario 2: Fehlermeldung wird im [Azure-Kontocenter](https://account.windowsazure.com/Subscriptions)** empfangen

Überprüfen Sie, ob das Konto, das Sie verwendet haben, das des Kontoadministrators ist. Gehen Sie folgendermaßen vor, um zu überprüfen, wer der Kontoadministrator ist:

1. Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com)an.
2. Wählen Sie im Menü „Hub“ die Option **Abonnement**aus.
3. Wählen Sie das zu überprüfende Abonnement und dann **Einstellungen**aus.
4. Wählen Sie **Eigenschaften**aus. Der Kontoadministrator des Abonnements wird im Feld **Kontoadministrator** angezeigt.

## <a name="you-are-automatically-signed-in-as-a-different-user"></a>Sie werden automatisch als ein anderer Benutzer angemeldet
Dieses Problem kann auftreten, wenn Sie mehr als ein Benutzerkonto in einem Internetbrowser verwenden.

Um dieses Problem zu lösen, probieren Sie eine der folgenden Methoden aus:

* Löschen Sie Cache und Internetcookies. Klicken Sie in Internet Explorer auf **Extras** ![Schaltfläche „Extras“](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Internetoptionen** > **Löschen**. Stellen Sie sicher, dass die Kontrollkästchen für temporäre Dateien, Cookies, Kennwort und Verlauf aktiviert sind, und klicken Sie dann auf „Löschen“.
* Setzen Sie die Internet Explorer-Einstellungen zurück, um alle persönlichen Einstellungen wiederherstellen, die Sie vorgenommen haben. Klicken Sie hierzu auf **Extras** ![Schaltfläche „Extras“](./media/billing-cannot-login-subscription/Toolsbutton.png)> **Internetoptionen** > **Erweitert**. Wählen Sie das Feld **Persönliche Einstellungen löschen** und dann **Zurücksetzen**.
* Navigieren Sie im Modus InPrivate-Browsen zum Azure-Portal. Klicken Sie auf **Extras** ![Schaltfläche „Extras“](./media/billing-cannot-login-subscription/Toolsbutton.png) > **Sicherheit** > **InPrivate-Browsen**.

## <a name="need-help-contact-support"></a>Sie brauchen Hilfe? Wenden Sie sich an den Support.
[Wenden Sie sich an den Support](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409), falls Sie weitere Hilfe benötigen, um das Problem schnell beheben zu lassen. 




<!--HONumber=Dec16_HO2-->



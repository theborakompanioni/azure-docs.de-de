---
title: 'Tutorial: Azure Active Directory-Integration mit Workplace by Facebook | Microsoft-Dokumentation'
description: Hier erfahren Sie, wie Sie das einmalige Anmelden zwischen Azure Active Directory und Workplace by Facebook konfigurieren.
services: active-directory
documentationcenter: 
author: asmalser-msft
manager: femila
editor: 
ms.assetid: 30f2ee64-95d3-44ef-b832-8a0a27e2967c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/29/2016
ms.author: asmalser
translationtype: Human Translation
ms.sourcegitcommit: efd29518fc207eb3e554f1389a6109ae4f1f3205
ms.openlocfilehash: 252e7b3c198a3433ecd21aef2222b2ca14b7ac04


---
# <a name="tutorial-azure-active-directory-integration-with-workplace-by-facebook"></a>Tutorial: Azure Active Directory-Integration mit Workplace by Facebook
Dieses Tutorial soll Ihnen zeigen, wie Sie Workplace by Facebook in Azure Active Directory (Azure AD) integrieren können.

Die Integration von Workplace by Facebook in Azure AD bietet die folgenden Vorteile: 

* Sie können in Azure AD den Zugriff auf Workplace by Facebook steuern. 
* Sie können automatisch Konten für Benutzer bereitstellen, denen Zugriff auf Workplace by Facebook erteilt wurde.
* Sie können es Benutzern ermöglichen, sich mit ihren Azure AD-Konten automatisch bei Workplace by Facebook anzumelden (einmaliges Anmelden).
* Sie können Ihre Konten an einem zentralen Ort verwalten. 

Weitere Informationen zur Integration von SaaS-Apps in Azure AD finden Sie unter [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md).

## <a name="prerequisites"></a>Voraussetzungen
Um die Azure AD-Integration mit CS Stars konfigurieren zu können, benötigen Sie Folgendes:

* Ein Azure AD-Abonnement
* Workplace by Facebook mit aktivierter Funktion für einmaliges Anmelden

Um die Schritte in diesem Tutorial zu testen, sollten Sie folgende Empfehlungen beachten:

* Sie sollten keine Produktionsumgebung verwenden, sofern dies nicht erforderlich ist.
* Wenn Sie keine Azure AD-Testumgebung haben, können Sie [hier](https://azure.microsoft.com/pricing/free-trial/)eine einmonatige Testversion anfordern. 

## <a name="adding-workplace-by-facebook-from-the-gallery"></a>Hinzufügen von Workplace by Facebook aus dem Katalog
Zum Konfigurieren der Integration von Workplace by Facebook in Azure AD müssen Sie Workplace by Facebook aus dem Katalog der Liste mit den verwalteten SaaS-Apps hinzufügen.

**Um Workplace by Facebook aus dem Katalog hinzuzufügen, führen Sie die folgenden Schritte aus:**

1. Klicken Sie im linken Navigationsbereich des **klassischen Azure-Portals** auf **Active Directory**. 
   
    ![Active Directory][1]
2. Wählen Sie in der Liste **Verzeichnis** das Verzeichnis aus, für das Sie die Verzeichnisintegration aktivieren möchten.
3. Klicken Sie zum Öffnen der Anwendungsansicht in der oberen Menüleiste der Verzeichnisansicht auf **Anwendungen** .
   
    ![Anwendungen][2]
4. Klicken Sie unten auf der Seite auf **Hinzufügen** .
   
    ![Anwendungen][3]
5. Klicken Sie im Dialogfeld **Was möchten Sie tun?** auf **Anwendung aus dem Katalog hinzufügen**.
   
    ![Anwendungen][4]
6. Geben Sie im Suchfeld den Suchbegriff **Workplace by Facebook** ein.
7. Wählen Sie im Ergebnisbereich **Workplace by Facebook** aus, und klicken Sie dann auf **Abschließen**, um die Anwendung hinzuzufügen.

### <a name="configuring-azure-ad-single-sign-on"></a>Konfigurieren des einmaligen Anmeldens von Azure AD
In diesem Abschnitt wird erläutert, wie Sie es Benutzern mithilfe einer Verbundanmeldung auf Basis des SAML-Protokolls ermöglichen, sich mit ihrem Konto in Azure Active Directory bei Workplace by Facebook zu authentifizieren.

**Führen Sie zum Konfigurieren des einmaligen Anmeldens von Azure AD bei Workplace by Facebook die folgenden Schritte aus:**

1. Klicken Sie nach dem Hinzufügen von Workplace by Facebook im klassischen Azure-Portal auf **Einmaliges Anmelden konfigurieren**.
2. Geben Sie auf dem Bildschirm **App-URL konfigurieren** die URL ein, unter der sich Benutzer bei der Anwendung Workplace by Facebook anmelden. Hierbei handelt es sich um die URL Ihres Workplace by Facebook-Mandanten (Beispiel: https://example.facebook.com/). Klicken Sie anschließend auf **Weiter**.
3. Melden Sie sich in einem anderen Webbrowserfenster bei der Workplace by Facebook-Unternehmenswebsite als Administrator an.
4. Führen Sie die Schritte unter der folgenden URL aus, um Workplace by Facebook für die Verwendung von Azure AD als Identitätsanbieter zu konfigurieren: [https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/authentication/cloud-providers)
5. Kehren Sie anschließend zum Browserfenster mit dem klassischen Azure-Portal zurück, und aktivieren Sie das Kontrollkästchen, um zu bestätigen, dass Sie den Vorgang abgeschlossen haben. Klicken Sie dann auf **Weiter** und **Abschließen**.


## <a name="automatically-provisioning-users-to-workplace-by-facebook"></a>Automatisches Bereitstellen von Benutzern für Workplace by Facebook
Azure AD unterstützt die Möglichkeit, die Kontodetails zugewiesener Benutzer mit Workplace by Facebook zu synchronisieren. Dank dieser automatischen Synchronisierung kann Workplace by Facebook die erforderlichen Daten für die Autorisierung von Benutzern für den Zugriff abzurufen, bevor sie sich zum ersten Mal anmelden. Sie hebt zudem die Bereitstellung von Benutzern in Workplace by Facebook auf, wenn der Zugriff in Azure AD widerrufen wurde.

Die automatische Bereitstellung kann durch Klicken auf **Kontobereitstellung konfigurieren** im Fenster des klassischen Azure-Portals eingerichtet werden.

Weitere Informationen zum Konfigurieren der automatischen Bereitstellung finden Sie unter [https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers](https://developers.facebook.com/docs/facebook-at-work/provisioning/cloud-providers)

## <a name="assigning-users-to-workplace-by-facebook"></a>Zuweisen von Benutzern zu Workplace by Facebook
Damit bereitgestellten AAD-Benutzern Workplace by Facebook im Zugriffsbereich angezeigt wird, muss ihnen im klassischen Azure-Portal der Zugriff zugewiesen werden.

**Gehen Sie wie folgt vor, um Benutzer Workplace by Facebook zuzuweisen:**

1. Klicken Sie auf der Startseite für Workplace by Facebook im klassischen Azure-Portal auf **Konten zuweisen**.
2. Wählen Sie im Menü **Einblenden** aus, ob Sie einen Benutzer oder eine Gruppe Workplace by Facebook zuweisen möchten, und klicken Sie auf die Schaltfläche mit dem Häkchen.
3. Wählen Sie in der angezeigten Liste die Benutzer oder Gruppen aus, denen Sie Workplace by Facebook zuweisen möchten.
4. Klicken Sie in der Fußzeile auf die Schaltfläche **Zuweisen** .

## <a name="additional-resources"></a>Weitere Ressourcen
* [Liste der Tutorials zur Integration von SaaS-Apps in Azure Active Directory](active-directory-saas-tutorial-list.md)
* [Was bedeuten Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory?](active-directory-appssoaccess-whatis.md)

<!--Image references-->
[1]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_01.png
[2]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_02.png
[3]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_03.png
[4]: ./media/active-directory-saas-facebook-at-work-tutorial/tutorial_general_04.png







<!--HONumber=Nov16_HO5-->



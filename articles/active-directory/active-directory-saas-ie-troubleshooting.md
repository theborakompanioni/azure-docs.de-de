---
title: "Behandeln von Problemen bei der Azure-Zugriffsbereichserweiterung für IE | Microsoft-Dokumentation"
description: "So stellen Sie das Internet Explorer-Add-On für das Portal &quot;Meine Apps&quot; mithilfe von Gruppenrichtlinien bereit"
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: f56b3230-26fd-42ec-9e3d-2c12daf15479
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/16/2017
ms.author: markvi
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: f593ecca4f4c82dcbd07e6f57ad120537f7d2134
ms.openlocfilehash: 4c3f7f95b1052512547a9996019803b6185a3405
ms.contentlocale: de-de
ms.lasthandoff: 03/01/2017


---
# <a name="troubleshooting-the-access-panel-extension-for-internet-explorer"></a>Problembehandlung in der Zugriffsbereichserweiterung für Internet Explorer
Dieser Artikel hilft Ihnen bei der Behandlung der folgenden Probleme:

* Während der Verwendung von Internet Explorer ist nicht möglich, über das Portal "Meine Apps" auf Ihre Apps zuzugreifen.
* Die Meldung „Software installieren“ wird angezeigt, obwohl Sie die Software bereits installiert haben.

Als Administrator finden Sie weitere Informationen unter: [How to Deploy the Access Panel Extension for Internet Explorer using Group Policy](active-directory-saas-ie-group-policy.md)

## <a name="run-the-diagnostic-tool"></a>Ausführen des Diagnoseprogramms
Sie können Probleme bei der Installation mithilfe der Zugriffsbereichserweiterung diagnostizieren, indem Sie das Diagnoseprogramm "Zugriffsbereich" herunterladen und ausführen:

1. [Klicken Sie hier, um das Diagnoseprogramm herunterzuladen.](https://account.activedirectory.windowsazure.com/applications/AccessPanelExtensionDiagnosticTool/AccessPanelExtensionDiagnosticTool.zip)
2. Öffnen Sie die Datei, und klicken Sie auf die Schaltfläche **Alle extrahieren** .
   
    ![Auf "Alle extrahieren" klicken](./media/active-directory-saas-ie-troubleshooting/extract1.png)
3. Klicken Sie dann zum Fortfahren auf die Schaltfläche **Extrahieren** .
   
    ![Auf "Extrahieren" klicken](./media/active-directory-saas-ie-troubleshooting/extract2.png)
4. Um das Programm auszuführen, klicken Sie mit der rechten Maustaste auf die Datei mit dem Namen **AccessPanelExtensionDiagnosticTool**, und wählen Sie dann **Öffnen mit > Microsoft Windows Script Host** aus.
   
    ![Öffnen mit > Microsoft Windows Script Host](./media/active-directory-saas-ie-troubleshooting/open_tool.png)
5. Anschließend wird das folgende Diagnosefenster angezeigt, in dem die möglichen Probleme bei der Installation angezeigt werden.
   
    ![Ein Beispiel für das Diagnosefenster](./media/active-directory-saas-ie-troubleshooting/tool_preview.png)
6. Klicken Sie auf "**Ja**", wenn das Programm die gefundenen Probleme beheben soll.
7. Um diese Änderungen zu speichern, schließen Sie alle Internet Explorer-Fenster, und öffnen Sie Internet Explorer anschließend erneut.<br />Wenn Sie weiterhin nicht auf Ihre Apps zugreifen können, führen Sie die folgenden Schritte aus.

## <a name="check-that-the-access-panel-extension-is-enabled"></a>Überprüfen, ob die Zugriffsbereichserweiterung aktiviert ist
So überprüfen Sie, ob die Zugriffsbereichserweiterung in Internet Explorer aktiviert ist:

1. Klicken Sie in Internet Explorer auf das **Zahnradsymbol** in der oberen rechten Ecke des Fensters. Wählen Sie dann **Internetoptionen** aus.<br />(In älteren Versionen von Internet Explorer finden Sie diese Option unter **Extras > Internetoptionen**.
   
    ![Auf "Extras" > "Internetoptionen" klicken](./media/active-directory-saas-ie-troubleshooting/internetoptions.png)
2. Klicken Sie auf die Registerkarte **Programme** und dann auf die Schaltfläche **Add-Ons verwalten**.
   
    ![Auf "Add-Ons verwalten" klicken](./media/active-directory-saas-ie-troubleshooting/internetoptions_programs.png)
3. Wählen Sie in diesem Dialogfeld die **Zugriffsbereichserweiterung** aus, und klicken Sie dann auf die Schaltfläche **Aktivieren**.
   
    ![Auf "Aktivieren" klicken](./media/active-directory-saas-ie-troubleshooting/enableaddon.png)
4. Um diese Änderungen zu speichern, schließen Sie alle Internet Explorer-Fenster, und öffnen Sie Internet Explorer dann erneut.

## <a name="enable-extensions-for-inprivate-browsing"></a>Aktivieren von Erweiterungen für InPrivate-Browsen
Gehen Sie wie folgt vor, wenn Sie den Modus "InPrivate-Browsen" verwenden:

1. Klicken Sie in Internet Explorer auf das **Zahnradsymbol** in der oberen rechten Ecke des Fensters. Wählen Sie dann **Internetoptionen** aus.<br />(In älteren Versionen von Internet Explorer finden Sie diese Option unter **Extras > Internetoptionen**.
   
    ![Ein Beispiel für das Diagnosefenster](./media/active-directory-saas-ie-troubleshooting/inprivateoptions.png)
2. Klicken Sie auf die Registerkarte **Datenschutz**, und **deaktivieren** Sie dann das Kontrollkästchen **Symbolleisten und Erweiterungen beim Starten des InPrivate-Browsens deaktivieren**.</p>
   
    !["Symbolleisten und Erweiterungen beim Starten des InPrivate-Browsens deaktivieren" deaktivieren](./media/active-directory-saas-ie-troubleshooting/enabletoolbars.png)
3. Um diese Änderungen zu speichern, schließen Sie alle Internet Explorer-Fenster, und öffnen Sie Internet Explorer dann erneut.

## <a name="uninstall-the-access-panel-extension"></a>Deinstallieren der Zugriffsbereichserweiterung
So deinstallieren Sie die Zugriffsbereichserweiterung auf Ihrem Computer:

1. Drücken Sie auf der Tastatur die **Windows-Taste** , um das Startmenü zu öffnen. Wenn das Menü geöffnet ist, können Sie beliebige Begriffe für die Suche eingeben. Geben Sie "Systemsteuerung" ein, und öffnen Sie dann die **Systemsteuerung** , wenn diese in den Suchergebnissen angezeigt wird.
   
    ![Systemsteuerung suchen](./media/active-directory-saas-ie-troubleshooting/search_sm.png)
2. Ändern Sie in der rechten oberen Ecke der Systemsteuerung die Option **Anzeige** in **Große Symbole**. Suchen Sie dann die Schaltfläche **Programme und Funktionen**, und klicken Sie darauf.
   
    ![Ansicht in große Symbole ändern](./media/active-directory-saas-ie-troubleshooting/control_panel.png)
3. Wählen Sie in der Liste den Eintrag **Zugriffsbereichserweiterung** aus, und klicken Sie dann auf die Schaltfläche **Deinstallieren**.
   
    ![Auf "Deinstallieren" klicken](./media/active-directory-saas-ie-troubleshooting/uninstall.png)
4. Sie können dann versuchen, die Erweiterung erneut zu installieren, um festzustellen, ob das Problem behoben wurde.

Wenn beim Deinstallieren der Erweiterung Probleme auftreten, können Sie diese auch mithilfe des Tools [Microsoft Fix it](https://go.microsoft.com/?linkid=9779673) entfernen.

## <a name="related-articles"></a>Verwandte Artikel
* [Artikelindex für die Anwendungsverwaltung in Azure Active Directory](active-directory-apps-index.md)
* [Anwendungszugriff und einmaliges Anmelden mit Azure Active Directory](active-directory-appssoaccess-whatis.md)
* [How to Deploy the Access Panel Extension for Internet Explorer using Group Policy (auf Englisch)](active-directory-saas-ie-group-policy.md)



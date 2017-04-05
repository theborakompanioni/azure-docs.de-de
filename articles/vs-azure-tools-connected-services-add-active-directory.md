---
title: "Hinzufügen von Azure Active Directory mithilfe von verbundenen Diensten in Visual Studio | Microsoft Docs"
description: "Fügen Sie Azure Active Directory mithilfe des Dialogfelds &quot;Verbundene Dienste hinzufügen&quot; in Visual Studio hinzu"
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: 
ms.assetid: f599de6b-e369-436f-9cdc-48a0165684cb
ms.service: active-directory
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/01/2017
ms.author: tarcher
translationtype: Human Translation
ms.sourcegitcommit: 356de369ec5409e8e6e51a286a20af70a9420193
ms.openlocfilehash: 5ff7c4ee612fd1261d85870be4d6fcbd66b64735
ms.lasthandoff: 03/27/2017


---
# <a name="adding-an-azure-active-directory-by-using-connected-services-in-visual-studio"></a>Hinzufügen von Azure Active Directory mithilfe von verbundenen Diensten in Visual Studio
Mithilfe von Azure Active Directory (Azure AD) können Sie das einmalige Anmelden (Single Sign-On, SSO) für ASP.NET MVC-Webanwendungen oder Active Directory-Authentifizierung in Web-API-Diensten unterstützen. Mit der Azure Active Directory-Authentifizierung können Ihre Benutzer ihre Konten in Azure Active Directory verwenden, um eine Verbindung mit Ihren Webanwendungen herzustellen. Die Azure Active Directory-Authentifizierung mit Web-API bietet neben weiteren Vorteilen mehr Datensicherheit, wenn eine API über eine Webanwendung verfügbar gemacht wird. Mit Azure AD benötigen Sie kein separates Authentifizierungssystem mit eigenem Konto und Benutzerverwaltung.

## <a name="prerequisites"></a>Voraussetzungen
- Azure-Konto: Wenn Sie noch nicht über ein Azure-Konto verfügen, können Sie sich [für eine kostenlose Testversion registrieren](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F) oder Ihre [Visual Studio-Abonnentenvorteile aktivieren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F).

### <a name="connect-to-azure-active-directory-using-the-connected-services-dialog"></a>Herstellen einer Verbindung mit Azure Active Directory über das Dialogfeld „Verbundene Dienste“
1. Erstellen oder öffnen Sie in Visual Studio ein ASP.NET MVC-Projekt oder ein ASP.NET-Web-API-Projekt.

1. Klicken Sie im Projektmappen-Explorer mit der rechten Maustaste auf den Knoten **Verbundene Dienste**, und wählen Sie im Kontextmenü die Option **Verbundene Dienste hinzufügen** aus.

1. Wählen Sie auf der Seite **Verbundene Dienste** die Option **Authentifizierung mit Azure Active Directory**.
   
    ![Seite „Verbundene Dienste“](./media/vs-azure-tools-connected-services-add-active-directory/connected-services-add-active-directory.png)

1. Wählen Sie auf der Seite **Einführung** des Assistenten **Azure AD-Authentifizierung konfigurieren** die Option **Weiter** aus.
   
    ![Seite „Einführung“](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-1.png)

1. Wählen Sie auf der Seite **Einmaliges Anmelden** des Assistenten **Azure AD-Authentifizierung konfigurieren** eine Domäne aus der Dropdownliste **Domäne** aus. Die Liste der Domänen enthält alle Domänen, auf die mit den im Dialogfeld "Kontoeinstellungen" aufgeführten Konten zugegriffen werden kann. Alternativ können Sie einen Domänennamen eingeben, wenn Sie den gewünschten nicht finden, beispielsweise `mydomain.onmicrosoft.com`. Sie können die Option zum Erstellen einer neuen Azure Active Directory-App auswählen oder die Einstellungen einer vorhandenen Azure Active Directory-App verwenden. Wählen Sie abschließend die Option **Weiter** aus.
   
    ![Seite „Einmaliges Anmelden“](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-2.png)

1. Stellen Sie auf der Seite **Verzeichniszugriff** des Assistenten **Azure AD-Authentifizierung konfigurieren** sicher, dass die Option **Verzeichnisdaten lesen** aktiviert ist. 
   
    ![Seite „Verzeichniszugriff“](./media/vs-azure-tools-connected-services-add-active-directory/configure-azure-ad-wizard-3.png)

1. Wählen Sie **Fertig stellen** aus, um den erforderlichen Konfigurationscode und Verweise hinzuzufügen, damit das Projekt für die Azure AD-Authentifizierung aktiviert wird. Die Active Directory-Domäne wird im [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)angezeigt.

1. Visual Studio zeigt einen [Artikel](#how-your-project-is-modified) an, der Ihnen erläutert, wie Ihr Projekt geändert wurde. Wenn Sie überprüfen möchten, ob alles funktioniert hat, öffnen Sie eine der geänderten Konfigurationsdateien, und stellen Sie sicher, dass die Einstellungen, die in dem Artikel genannt werden, vorhanden sind. 

## <a name="how-your-project-is-modified"></a>Änderungen am Projekt
Beim Ausführen des Assistenten fügt Visual Studio Ihrem Projekt Azure Active Directory und entsprechende Verweise hinzu. Zudem werden Konfigurationsdateien und Codedateien im Projekt geändert, um Unterstützung für Azure AD hinzuzufügen. Die genauen Änderungen, die von Visual Studio vorgenommen werden, hängen vom Projekttyp ab. Ausführliche Informationen dazu, wie ASP.NET MVC-Projekte geändert werden, finden Sie unter [Was ist passiert – MVC-Projekte](http://go.microsoft.com/fwlink/p/?LinkID=513809). Informationen zu Web-API-Projekten finden Sie unter [Was ist passiert – Web-API-Projekte](http://go.microsoft.com/fwlink/p/?LinkId=513810).

## <a name="next-steps"></a>Nächste Schritte
* [MSDN-Forum für Azure Active Directory](https://social.msdn.microsoft.com/forums/azure/home?forum=WindowsAzureAD)
* [Azure Active Directory-Dokumentation](https://azure.microsoft.com/documentation/services/active-directory/)
* [Blogbeitrag: Intro to Azure Active Directory](http://blogs.msdn.com/b/brunoterkaly/archive/2014/03/03/introduction-to-windows-azure-active-directory.aspx)



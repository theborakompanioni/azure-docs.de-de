---
title: "Erstellen Ihrer ersten Java-Web-App in Azure in fünf Minuten | Microsoft-Dokumentation"
description: "Hier erfahren Sie anhand der Bereitstellung einer einfachen Java-Anwendung, wie leicht die Ausführung von Web-Apps in App Service ist."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 8bacfe3e-7f0b-4394-959a-a88618cb31e1
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: java
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: cephalin;robmcm
translationtype: Human Translation
ms.sourcegitcommit: 2c33e75a7d2cb28f8dc6b314e663a530b7b7fdb4
ms.openlocfilehash: 2673a9c0d91510756a97b2dba3801d2925905c9a
ms.lasthandoff: 04/21/2017


---
# <a name="create-your-first-java-web-app-in-azure-in-five-minutes"></a>Erstellen Ihrer ersten Java-Web-App in Azure in fünf Minuten

[!INCLUDE [app-service-web-selector-get-started](../../includes/app-service-web-selector-get-started.md)] 

Dieser Schnellstart hilft Ihnen bei der Bereitstellung Ihrer ersten Java-Web-App in [Azure App Service](../app-service/app-service-value-prop-what-is.md) in nur wenigen Minuten. Wenn Sie das Tutorial durchgearbeitet haben, verfügen Sie über eine einfache Java-basierte Web-App, die in der Cloud ausgeführt wird.

![Navigieren zur Web-App](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="before-you-begin"></a>Voraussetzungen

In diesem Tutorial wird veranschaulicht, wie Sie die Eclipse-IDE für Java EE-Entwickler verwenden, um eine Java-Web-App zu erstellen und in Azure bereitzustellen. Wenn Sie Eclipse noch nicht installiert haben, können Sie die Anwendung kostenlos von „http://www.eclipse.org/“ herunterladen.

In den Schritten dieses Tutorials wird das [Azure-Toolkit für Eclipse](/azure/azure-toolkit-for-eclipse) verwendet, um den Veröffentlichungsprozess für Java-Web-Apps in Azure zu vereinfachen. Eine Anleitung zum Installieren des Toolkits finden Sie unter [Installieren des Azure-Toolkits für Eclipse](/azure/azure-toolkit-for-eclipse-installation).

> [!NOTE]
>
> Sie können auch [IntelliJ IDEA](https://www.jetbrains.com/idea/) von JetBrains verwenden, um die Schritte in diesem Tutorial auszuführen. Einige Schritte weichen für diese Entwicklungsumgebung unter Umständen etwas ab, aber es ist auch ein [Azure-Toolkit für IntelliJ](/azure/azure-toolkit-for-intellij) verfügbar, das Sie zum Vereinfachen des Veröffentlichungsprozesses für diese IDE verwenden können.
>

Außerdem benötigen Sie ein Azure-Abonnement, um die Schritte in diesem Tutorial auszuführen. Wenn Sie noch kein Azure-Abonnement besitzen, können Sie Ihre [MSDN-Abonnentenvorteile](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) aktivieren oder sich für ein [kostenloses Azure-Konto](https://azure.microsoft.com/pricing/free-trial/) registrieren.

## <a name="create-a-dynamic-web-project-in-eclipse"></a>Erstellen eines dynamischen Webprojekts in Eclipse

Klicken Sie in der Eclipse-IDE nacheinander auf **File** (Datei), **New** (Neu) und **Dynamic Web Project** (Dynamisches Webprojekt).

![Neues dynamisches Webprojekt](./media/app-service-web-get-started-java/file-new-dynamic-web-project-menu.png)

Wenn das Dialogfeld „Dynamic Web Project“ (Dynamisches Webprojekt) angezeigt wird, geben Sie der Anwendung den Namen **MyFirstJavaOnAzureWebApp**, und klicken Sie anschließend auf **Finish** (Fertig stellen).
   
![Dialogfeld „Dynamic Web Project“ (Dynamisches Webprojekt)](./media/app-service-web-get-started-java/new-dynamic-web-project-dialog-box.png)

> [!NOTE]
>
> Wenn Sie eine lokale Laufzeitumgebung installiert haben, z.B. [Apache Tomcat](https://tomcat.apache.org/) können Sie dies im Feld **Target runtime** (Zielruntime) angeben.
>

Fügen Sie nach der Erstellung Ihres dynamischen Webprojekts eine neue JSP-Seite hinzu, indem Sie Ihr Projekt im Projektexplorer erweitern, mit der rechten Maustaste auf den Ordner **WebContent** klicken und dann auf **New** (Neu) und **JSP File** (JSP-Datei) klicken.

![Menü „New“ > „JSP File“ („Neu“ > „JSP-Datei“)](./media/app-service-web-get-started-java/new-jsp-file-menu.png)

Geben Sie im angezeigten Dialogfeld „New JSP File“ (Neue JSP-Datei) den Namen **index.jsp** für die Datei ein, behalten Sie den übergeordneten Ordner **MyFirstJavaOnAzureWebApp/WebContent** bei, und klicken Sie auf **Next** (Weiter).

![Dialogfeld „New JSP File“ (Neue JSP-Datei)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-1.png)

Geben Sie auf der zweiten Seite des Dialogfelds „New JSP File“ (Neue JSP-Datei) der Datei den Namen **index.jsp**, behalten Sie den übergeordneten Ordner **MyFirstJavaOnAzureWebApp/WebContent** bei, und klicken Sie auf **Finish** (Fertig stellen).

![Dialogfeld „New JSP File“ (Neue JSP-Datei)](./media/app-service-web-get-started-java/new-jsp-file-dialog-box-page-2.png)

Ersetzen Sie auf der neuen Seite, die in Eclipse geöffnet wird, den vorhandenen Abschnitt `<body></body>` durch den folgenden Code:

```jsp
<body>
<h1><% out.println("Java on Azure!"); %></h1>
</body>
```

Speichern Sie Ihre Änderungen, die Sie auf der Seite vorgenommen haben.

![Bearbeiten von JSP-Code](./media/app-service-web-get-started-java/creating-index-jsp-page.png)

## <a name="publish-your-web-app-to-azure"></a>Veröffentlichen Ihrer Web-App in Azure

Für die Bereitstellung Ihrer Web-App in Azure nutzen Sie mehrere Features, die über das Azure-Toolkit für Eclipse bereitgestellt werden.

Verwenden Sie eine der folgenden Methoden, um mit dem Veröffentlichungsprozess zu beginnen:

* Klicken Sie mit der rechten Maustaste im **Projektexplorer** von Eclipse auf Ihr Projekt, und klicken Sie anschließend auf **Azure** und auf **Publish as Azure Web App** (Als Azure-Web-App veröffentlichen).

   ![Kontextmenü „Publish as Azure Web App“ (Als Azure-Web-App veröffentlichen)](./media/app-service-web-get-started-java/publish-as-azure-web-app-context-menu.png)

* Klicken Sie auf der Eclipse-Symbolleiste auf **Publish** (Veröffentlichen) und dann auf **Publish as Azure Web App** (Als Azure-Web-App veröffentlichen).

   ![Dropdownmenü „Publish as Azure Web App“ (Als Azure-Web-App veröffentlichen)](./media/app-service-web-get-started-java/publish-as-azure-web-app-drop-down-menu.png)

Wenn Sie sich noch nicht an Ihrem Azure-Konto angemeldet haben, erhalten Sie eine entsprechende Aufforderung. Führen Sie dazu die folgenden Schritte aus:

1. Sie haben zwei Möglichkeiten, um sich an Ihrem Azure-Konto anzumelden: Wählen Sie für dieses Tutorial die Option **Interactive** (Interaktiv).

   ![Dialogfeld „Azure Sign In“ (Azure-Anmeldung)](./media/app-service-web-get-started-java/azure-signin-dialog-box.png)

1. Geben Sie Ihre Azure-Anmeldeinformationen ein, und klicken Sie auf **Sign in** (Anmelden).

   ![Dialogfeld für Azure-Anmeldung](./media/app-service-web-get-started-java/azure-login-dialog-box.png)

1. Wählen Sie Ihre Azure-Abonnements aus, und klicken Sie dann auf **Select** (Auswählen).

   ![Dialogfeld für Azure-Anmeldung](./media/app-service-web-get-started-java/select-azure-subscriptions-dialog-box.png)

> [!NOTE]
>
> Eine ausführliche Anleitung zu den Anmeldearten **Interactive** (Interaktiv) und **Automated** (Automatisiert) finden Sie im Artikel [Azure Sign In Instructions for the Azure Toolkit for Eclipse](https://go.microsoft.com/fwlink/?linkid=846174) (Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse).
>

Nachdem Sie sich an Ihrem Azure-Konto angemeldet haben, wird das Dialogfeld **Deploy Web App** (Web-App bereitstellen) angezeigt. Es sollte keine App Services-Instanz aufgeführt sein, wenn Sie zum ersten Mal eine Web-App in Azure veröffentlichen. Wenn dies der Fall ist oder wenn Sie eine neue App Service-Instanz erstellen möchten, ist der nächste Schritt die Erstellung der neuen App Service-Instanz. Klicken Sie hierfür auf **Create** (Erstellen).

![Dialogfeld „Deploy Web App“ (Web-App bereitstellen)](./media/app-service-web-get-started-java/deploy-web-app-dialog-box.png)

Wenn das Dialogfeld **Create App Service** (App Service erstellen) angezeigt wird, müssen Sie als Erstes die folgenden Daten angeben:

* Einen eindeutigen Namen für Ihre Web-App, der zur DNS-Adresse für Ihre Web-App wird. Beispiel: **MyJavaWebApp** wird zu *myjavawebapp.azurewebsites.net*.

* Den Webcontainer für Ihre Web-App. Beispiel: **Newest Tomcat 8.5**.

* Ihr Azure-Abonnement.

   ![Dialogfeld „Create App Service“ (App Service erstellen)](./media/app-service-web-get-started-java/create-app-service-dialog-box.png)

Wenn Sie keine vorhandenen App Service-Pläne haben oder wenn Sie einen neuen Dienstplan erstellen möchten, müssen Sie die folgenden Informationen angeben:

* Einen eindeutigen Namen für Ihren neuen Dienstplan. Dieser Name wird angezeigt, wenn Sie später Web-Apps mit dem Azure-Toolkit veröffentlichen, und er wird im [Azure-Portal](https://portal.azure.com) beim Verwalten Ihres Kontos angegeben.

* Den geografischen Standort, an dem Ihr Dienstplan erstellt wird.

* Den Tarif für Ihren Dienstplan.

   ![Erstellen des App Service-Plans](./media/app-service-web-get-started-java/create-app-service-plan.png)

Klicken Sie als Nächstes auf die Registerkarte **Resource group** (Ressourcengruppe). Wenn keine Ressourcengruppen vorhanden sind oder wenn Sie eine neue erstellen möchten, müssen Sie einen eindeutigen Namen für die neue Ressourcengruppe angeben. Wählen Sie andernfalls im Dropdownmenü eine vorhandene Ressourcengruppe.

![Erstellen des App Service-Plans](./media/app-service-web-get-started-java/create-app-service-resource-group.png)

Klicken Sie abschließend auf die Registerkarte **JDK**. Es sind mehrere Optionen angegeben, mit denen Entwickler JDKs (Java Developer Kits) von Drittanbietern oder benutzerdefinierte JDKs angeben können. Wählen Sie für dieses Tutorial die Option **Default** (Standard), und klicken Sie auf **Create** (Erstellen).

![Erstellen des App Service-Plans](./media/app-service-web-get-started-java/create-app-service-specify-jdk.png)

Das Azure-Toolkit beginnt mit der Erstellung Ihrer neuen App Service-Instanz, und während der Verarbeitung wird ein Statusdialogfeld angezeigt.

![Statusleiste für App Service-Erstellung](./media/app-service-web-get-started-java/create-app-service-progress-bar.png)

Nachdem die neue App Service-Instanz erstellt wurde, müssen Sie nur noch angeben, ob die Web-App im Stamm der neuen Website bereitgestellt werden soll. Wenn Sie beispielsweise eine App Service-Instanz unter *wingtiptoys.azurewebsites.net* verwenden und die Bereitstellung im Stamm nicht auswählen, wird Ihre Web-App mit dem Namen **MyFirstJavaOnAzureWebApp** unter *wingtiptoys.azurewebsites.net/MyFirstJavaOnAzureWebApp* bereitgestellt.

![Bereitstellen der Web-App im Stamm](./media/app-service-web-get-started-java/deploy-web-app-to-root.png)

Klicken Sie nach Abschluss aller obigen Schritte auf **Deploy** (Bereitstellen), um die Web-App in Azure zu veröffentlichen.

![Bereitstellen der Web-App in Azure](./media/app-service-web-get-started-java/deploy-web-app-to-azure.png)

Glückwunsch! Sie haben Ihre Web-App erfolgreich in Azure bereitgestellt! Sie können eine Vorschau der Web-App auf der Azure-Website anzeigen:

![Navigieren zur Web-App](./media/app-service-web-get-started-java/browse-web-app-1.png)

## <a name="updating-your-web-app"></a>Aktualisieren Ihrer Web-App

Nachdem Sie Ihre Web-App in Azure veröffentlicht haben, ist die Aktualisierung der Web-App ein deutlich einfacherer Prozess. In den folgenden Schritten wird beschrieben, wie Sie Änderungen für Ihre Web-App veröffentlichen.

Ändern Sie als Erstes den JSP-Beispielcode, damit der Titel durch das heutige Datum ersetzt wird:

```jsp
<%@ page
    language="java"
    contentType="text/html; charset=ISO-8859-1"
    pageEncoding="ISO-8859-1"
    import="java.text.SimpleDateFormat"
    import="java.util.Date" %>
<!DOCTYPE html PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
<head>
<meta http-equiv="Content-Type" content="text/html; charset=ISO-8859-1">
<% SimpleDateFormat date = new SimpleDateFormat("yyyy/MM/dd"); %>
<title><% out.println(date.format(new Date())); %></title>
</head>
<body>
<h1><% out.println("Java on Azure!"); %></h1>
</body>
</html>
```

![Aktualisieren des JSP-Codes](./media/app-service-web-get-started-java/updating-index-jsp-page.png)

Klicken Sie nach dem Speichern der Änderungen für die Seite im **Projektexplorer** von Eclipse mit der rechten Maustaste auf Ihr Projekt, und klicken Sie dann auf **Azure** und **Publish as Azure Web App** (Als Azure-Web-App veröffentlichen).

![Veröffentlichen der aktualisierten Web-App](./media/app-service-web-get-started-java/publish-updated-web-app-context-menu.png)

Wenn das Dialogfeld **Deploy Web App** (Web-App bereitstellen) angezeigt wird, wird die obige App Service-Instanz aufgeführt. Zum Aktualisieren der Web-App müssen Sie lediglich Ihre App Service-Instanz markieren und auf **Deploy** (Bereitstellen) klicken, um Ihre Änderungen zu veröffentlichen.

![Bereitstellen der Web-App in Azure](./media/app-service-web-get-started-java/deploy-web-app-to-azure.png)

> [!NOTE]
>
> Wenn Sie Ihre Web-App im Stamm der App Service-Instanz bereitstellen, müssen Sie die Option **Deploy to root** (Im Stamm bereitstellen) bei jedem Veröffentlichungsvorgang Ihrer Änderungen erneut aktivieren.
>

Nachdem Sie die Änderungen veröffentlicht haben, sehen Sie, dass der Seitentitel im Browser geändert wurde und das heutige Datum angezeigt wird.

![Navigieren zur Web-App](./media/app-service-web-get-started-java/browse-web-app-2.png)

## <a name="deleting-your-web-app"></a>Löschen Ihrer Web-App

Zum Löschen einer Web-App können Sie den **Azure Explorer** verwenden, der Teil des Azure-Toolkits ist. Falls die Ansicht **Azure Explorer** in Eclipse nicht bereits angezeigt wird, können Sie die folgenden Schritte ausführen:

1. Klicken Sie nacheinander auf **Window** (Fenster), **Show View** (Ansicht anzeigen) und **Other** (Andere).

   ![Menü „Show View“ (Ansicht anzeigen)](./media/app-service-web-get-started-java/show-azure-explorer-view-1.png)

2. Wählen Sie im angezeigten Dialogfeld **Show View** (Ansicht anzeigen) die Option **Azure Explorer**, und klicken Sie auf **OK**.

   ![Dialogfeld „Show View“ (Ansicht anzeigen)](./media/app-service-web-get-started-java/show-azure-explorer-view-2.png)

Zum Löschen Ihrer Web-App aus dem Azure Explorer erweitern Sie den Knoten **Web Apps** (Web-Apps), klicken mit der rechten Maustaste auf Ihre Web-App und wählen die Option **Delete** (Löschen).

![Löschen der Web-App](./media/app-service-web-get-started-java/delete-web-app-context-menu.png)

Klicken Sie auf **OK**, wenn die Aufforderung zum Löschen der Web-App angezeigt wird.

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu den Azure-Toolkits für Java-IDEs finden Sie unter den folgenden Links:

* [Azure-Toolkit für Eclipse (dieser Artikel)](../azure-toolkit-for-eclipse.md)
  * [Neuerungen im Azure-Toolkit für Eclipse](../azure-toolkit-for-eclipse-whats-new.md)
  * [Installieren des Azure-Toolkits für Eclipse](../azure-toolkit-for-eclipse-installation.md)
  * [Sign In Instructions for the Azure Toolkit for Eclipse](https://go.microsoft.com/fwlink/?linkid=846174) (Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse)
* [Azure Toolkit für IntelliJ](../azure-toolkit-for-intellij.md)
  * [Neuerungen im Azure-Toolkit für IntelliJ](../azure-toolkit-for-intellij-whats-new.md)
  * [Installieren des Azure Toolkit für IntelliJ](../azure-toolkit-for-intellij-installation.md)
  * [Azure Sign In Instructions for the Azure Toolkit for IntelliJ](https://go.microsoft.com/fwlink/?linkid=846179) (Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ)

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center](https://azure.microsoft.com/develop/java/) und in den [Java-Tools für Visual Studio Team Services](https://java.visualstudio.com/).


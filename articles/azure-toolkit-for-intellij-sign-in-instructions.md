---
title: "Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie sich bei Microsoft Azure mit dem Azure-Toolkit für IntelliJ anmelden."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: d1be0432a54ed300bc135345b660b90f3efb3b39
ms.lasthandoff: 04/22/2017


---

# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-intellij"></a>Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ

Das Azure-Toolkit für IntelliJ bietet zwei Methoden für die Anmeldung bei Ihrem Azure-Konto:

  * **Interaktiv**: Bei Wahl dieser Methode geben Sie bei jeder Anmeldung bei Ihrem Azure-Konto Ihre Azure-Anmeldeinformationen ein.
  * **Automatisch**: Für diese Methode erstellen Sie eine Datei mit Anmeldeinformationen, die Ihre Dienstprinzipaldaten enthält. Anschließend können Sie sich mithilfe dieser Datei automatisch bei Ihrem Azure-Konto anmelden.

In den folgenden Abschnitte werden die für die beiden Methoden auszuführenden Schritte beschrieben.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a>Interaktive Anmeldung bei Ihrem Azure-Konto

Nachstehend wird erläutert, wie Sie sich bei Azure durch manuelles Eingeben Ihrer Azure-Anmeldeinformationen anmelden.

1. Öffnen Sie das Projekt in IntelliJ IDEA.

1. Klicken Sie auf **Tools**, dann auf **Azure** und danach auf **Azure Sign In**.

   ![IntelliJ-Menü für die Anmeldung bei Azure][I01]

1. Wählen Sie im angezeigten Dialogfeld **Azure Sign In** die Option **Interactive** aus, und klicken Sie dann auf **Sign In**.

   ![Anmeldedialogfeld][I02]

1. Wenn das Dialogfeld **Azure Log In** angezeigt wird, geben Sie Ihre Azure-Anmeldeinformationen ein, und klicken Sie dann auf **Sign In**.

   ![Dialogfeld für Azure-Anmeldung][I03]

1. Wenn das Dialogfeld **Select Subscriptions** angezeigt wird, wählen Sie die Abonnements aus, die Sie verwenden möchten, und klicken Sie dann auf **OK**.

   ![Dialogfeld zum Auswählen von Abonnements][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a>Abmelden von Ihrem Azure-Konto bei interaktiver Anmeldung

Nachdem Sie die Schritte im vorherigen Abschnitt konfiguriert haben, werden Sie bei jedem Neustart von IntelliJ IDEA automatisch von Ihrem Azure-Konto abgemeldet. Wenn Sie sich jedoch von Ihrem Azure-Konto abmelden möchten, ohne IntelliJ IDEE neu zu starten, führen Sie die folgenden Schritte aus.

1. Klicken Sie in IntelliJ IDEA auf **Tools**, dann auf **Azure** und danach auf **Azure Sign Out**.

   ![IntelliJ-Menü für die Abmeldung von Azure][L01]

1. Wenn das Dialogfeld **Azure Sign Out** angezeigt wird, klicken Sie auf **Yes**.

   ![Dialogfeld zum Abmelden][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a>Automatisches Anmelden bei Ihrem Azure-Konto und Erstellen einer Datei mit Anmeldeinformationen für die künftige Nutzung

Nachstehend werden die Schritte zum Erstellen einer Datei mit Anmeldeinformationen erläutert, die Ihre Dienstprinzipaldaten enthält. Nachdem Sie diese Schritte abgeschlossen haben, verwendet IntelliJ IDEA automatisch die Datei mit Anmeldeinformationen, damit Sie beim Öffnen Ihres Projekts automatisch bei Azure angemeldet werden.

1. Öffnen Sie das Projekt in IntelliJ IDEA.

1. Klicken Sie auf **Tools**, dann auf **Azure** und danach auf **Azure Sign In**.

   ![IntelliJ-Menü für die Anmeldung bei Azure][A01]

1. Wählen Sie im angezeigten Dialogfeld **Azure Sign In** die Option **Automated** aus, und klicken Sie dann auf **New**.

   ![Anmeldedialogfeld][A02]

1. Wenn das Dialogfeld **Azure Log In** angezeigt wird, geben Sie Ihre Azure-Anmeldeinformationen ein, und klicken Sie dann auf **Sign In**.

   ![Dialogfeld für Azure-Anmeldung][A03]

1. Wenn das Dialogfeld **Create authentication files** angezeigt wird, wählen Sie die gewünschten Abonnements aus. Wählen Sie Ihr Zielverzeichnis, und klicken Sie dann auf **Start**.

   ![Dialogfeld für Azure-Anmeldung][A04]

1. Das Dialogfeld **Service Principal Creatation Status** wird angezeigt. Nachdem Ihre Dateien erfolgreich erstellt wurden, klicken Sie auf **OK**.

   ![Dialogfeld mit dem Status der Dienstprinzipalerstellung][A05]

1. Wenn das Dialogfeld **Azure Sign In** angezeigt wird, klicken Sie auf **Sign In**.

   ![Dialogfeld für Azure-Anmeldung][A06]

1. Wenn das Dialogfeld **Select Subscriptions** angezeigt wird, wählen Sie die Abonnements aus, die Sie verwenden möchten, und klicken Sie dann auf **OK**.

   ![Dialogfeld zum Auswählen von Abonnements][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a>Automatisches Abmelden von Ihrem Azure-Konto bei automatischer Anmeldung

Nachdem Sie die Schritte im vorherigen Abschnitt konfiguriert haben, werden Sie vom Azure-Toolkit bei jedem Neustart von IntelliJ IDEA automatisch bei Ihrem Azure-Konto angemeldet. Um sich jedoch von Ihrem Azure-Konto abzumelden und zu verhindern, dass das Azure-Toolkit Sie automatisch anmeldet, führen Sie die folgenden Schritte aus.

1. Klicken Sie in IntelliJ IDEA auf **Tools**, dann auf **Azure** und danach auf **Azure Sign Out**.

   ![IntelliJ-Menü für die Abmeldung von Azure][L01]

1. Wenn das Dialogfeld **Azure Sign Out** angezeigt wird, klicken Sie auf **Yes**.

   ![Dialogfeld zum Abmelden][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a>Automatisches Anmelden bei Ihrem Azure-Konto mithilfe einer bereits erstellten Datei mit Anmeldeinformationen

Wenn Sie sich bei Verwenden von IntelliJ IDEE von Azure abmelden, müssen Sie das Azure-Toolkit für IntelliJ so neu konfigurieren, dass eine Datei mit Anmeldeinformationen verwendet wird, die Sie zuvor erstellt haben, ehe Sie sich automatisch bei Ihrem Azure-Konto anmelden können. Die folgenden Schritte begleiten Sie durch die Konfiguration des Azure-Toolkits, um eine vorhandene Datei mit Anmeldeinformationen zu verwenden.

1. Öffnen Sie das Projekt in IntelliJ IDEA.

1. Klicken Sie auf **Tools**, dann auf **Azure** und danach auf **Azure Sign In**.

   ![IntelliJ-Menü für die Anmeldung bei Azure][A01]

1. Wählen Sie im angezeigten Dialogfeld **Azure Sign In** die Option **Automated** aus, und klicken Sie dann auf **Browse**.

   ![Anmeldedialogfeld][A02]

1. Wenn das Dialogfeld **Select Authentication File** angezeigt wird, wählen Sie eine Datei mit Anmeldeinformationen aus, die Sie zuvor erstellt haben, und klicken Sie dann auf **Select**.

   ![Anmeldedialogfeld][A08]

1. Wenn das Dialogfeld **Azure Sign In** angezeigt wird, klicken Sie auf **Sign In**.

   ![Dialogfeld für Azure-Anmeldung][A06]

1. Wenn das Dialogfeld **Select Subscriptions** angezeigt wird, wählen Sie die Abonnements aus, die Sie verwenden möchten, und klicken Sie dann auf **OK**.

   ![Dialogfeld zum Auswählen von Abonnements][A07]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu den Azure-Toolkits für Java-IDEs finden Sie unter den folgenden Links:

* [Azure-Toolkit für Eclipse]
  * [Neuerungen im Azure-Toolkit für Eclipse]
  * [Installieren des Azure-Toolkits für Eclipse]
  * [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse]
  * [Erstellen einer „Hello World“-Web-App für Azure in Eclipse]
* [Azure Toolkit für IntelliJ]
  * [Neuerungen im Azure-Toolkit für IntelliJ]
  * [Installieren des Azure Toolkit für IntelliJ]
  * *Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ (dieser Artikel)*
  * [Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center] und in den [Java-Tools für Visual Studio Team Services].

<!-- URL List -->

[Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij.md
[Erstellen einer „Hello World“-Web-App für Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installieren des Azure-Toolkits für Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installieren des Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Neuerungen im Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Neuerungen im Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-Tools für Visual Studio Team Services]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L03.png


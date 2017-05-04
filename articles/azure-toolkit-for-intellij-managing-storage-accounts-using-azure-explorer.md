---
title: "Verwalten von Speicherkonten mit Azure-Explorer für IntelliJ | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ihre Azure-Speicherkonten mit Azure-Explorer für IntelliJ verwalten."
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
ms.openlocfilehash: 22930bf1d37d7b6039bd02792cdd70ec321d6fa7
ms.lasthandoff: 04/22/2017


---

# <a name="managing-storage-accounts-using-the-azure-explorer-for-intellij"></a>Verwalten von Speicherkonten mit dem Azure-Explorer für IntelliJ

Der Azure-Explorer, der zum Azure-Toolkit für IntelliJ gehört, bietet Java-Entwicklern eine einfache Lösung zum Verwalten von Speicherkonten in ihrem Azure-Konto innerhalb der IDE von IntelliJ.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-intellij-show-azure-explorer](../includes/azure-toolkit-for-intellij-show-azure-explorer.md)]

## <a name="creating-a-storage-account-in-intellij"></a>Erstellen eines Speicherkontos in IntelliJ

Die folgenden Schritte begleiten Sie durch die Schritte zum Erstellen eines Speicherkontos mit dem Azure-Explorer.

1. Melden Sie sich beim Azure-Konto gemäß den Anweisungen im Artikel [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse] an.

1. Erweitern Sie im **Azure-Explorer** im Toolfenster den Knoten **Azure**. Klicken Sie mit der rechten Maustaste auf **Storage Accounts** und dann auf **Create Storage Account**.
   ![Menü „Create Storage Account“][CS01]

1. Wenn das Dialogfeld **Create Storage Account** angezeigt wird, geben Sie die folgenden Optionen an: ![Dialogfeld „Create Storage Account“][CS02]

   a. **Name**: Gibt den Namen des neuen Speicherkontos an.

   b. **Account kind**: Gibt den Typ des zu erstellenden Speicherkontos an, z.B. Blob Storage. (Weitere Informationen finden Sie unter [Informationen zu Azure-Speicherkonten].)

   c. **Performance**: Gibt an, welches Speicherkontoangebot für den ausgewählten Herausgeber verwendet werden soll, z.B. Premium. (Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage].)

   d. **Replication**: Gibt die Replikation des Speicherkontos an, z.B. zonenredundant. (Weitere Informationen finden Sie unter [Azure Storage-Replikation].)

   e. **Subscription**: Wählen Sie das Azure-Abonnement aus, das Sie für das neue Speicherkonto verwenden möchten.

   f. **Location**: Gibt den Speicherort an, an dem Ihr Speicherkonto erstellt wird, z.B. USA, Westen.

   g. **Resource Group**: Gibt die Ressourcengruppe für den virtuellen Computer an. Sie müssen eine der folgenden Optionen wählen:
      * **Create New**: Gibt an, dass Sie eine neue Ressourcengruppe erstellen möchten.
      * **Use Existing**: Gibt an, dass Sie in einer Liste von Ressourcengruppen, die Ihrem Azure-Konto zugeordnet sind, eine Wahl treffen.

1. Nachdem Sie alle oben genannten Optionen angegeben haben, klicken Sie auf **OK**.

## <a name="creating-a-storage-container-in-intellij"></a>Erstellen eines Speichercontainers in IntelliJ

Die folgenden Schritte begleiten Sie durch die Schritte zum Erstellen eines Speichercontainers mit dem Azure-Explorer.

1. Klicken Sie im Azure-Explorer mit der rechten Maustaste auf das Speicherkonto, in dem Sie einen Container erstellen möchten, und klicken Sie dann auf **Create blob container**.
   ![Menü zum Erstellen eines Speichercontainers][CC01]

1. Wenn das Dialogfeld **Create Blob Container** angezeigt wird, geben Sie den Namen für Ihren Container ein, und klicken Sie dann auf **OK**. Ausführliche Informationen zum Benennen von Containern und Blobs finden Sie unter [Naming and Referencing Containers, Blobs, and Metadata] (Benennen und Referenzieren von Containern, Blobs und Metadaten). ![Dialogfeld zum Erstellen eines Speichercontainers][CC02]

## <a name="deleting-a-storage-container-in-intellij"></a>Löschen eines Speichercontainers in IntelliJ

Um mit dem Azure-Explorer einen Speichercontainer zu löschen, führen Sie die folgenden Schritte aus:

1. Klicken Sie im Azure-Explorer mit der rechten Maustaste auf den Speichercontainer und dann auf **Delete**.
   ![Menü zum Löschen eines Speichercontainers][DC01]

1. Klicken Sie auf **Ja**, wenn Sie zum Löschen des Speichercontainers aufgefordert werden.
   ![Dialogfeld zum Löschen eines Speichercontainers][DC02]

## <a name="deleting-a-storage-account-in-intellij"></a>Löschen eines Speicherkontos in IntelliJ

Um mit dem Azure-Explorer ein Speicherkonto zu löschen, führen Sie die folgenden Schritte aus:

1. Klicken Sie im **Azure-Explorer** im Toolfenster mit der rechten Maustaste auf das Speicherkonto, und wählen Sie **Delete**.
   ![Menü zum Löschen eines Speicherkontos][DS01]

1. Klicken Sie auf **Ja**, wenn Sie zum Löschen des Speicherkontos aufgefordert werden.
   ![Dialogfeld zum Löschen eines Speicherkontos][DS02]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Azure-Speicherkonten sowie ihren Größen und Preisen finden Sie unter den folgenden Links:

* [Einführung in Microsoft Azure Storage]
* [Informationen zu Azure-Speicherkonten]
* Größen von Azure-Speicherkonten
   * [Größen für Windows-Speicherkonten in Azure]
   * [Größen für Linux-Speicherkonten in Azure]
* Preise von Azure-Speicherkonten
   * [Preise von Windows-Speicherkonten]
   * [Preise von Linux-Speicherkonten]

Weitere Informationen zu den Azure-Toolkits für Java-IDEs finden Sie unter den folgenden Links:

* [Azure-Toolkit für Eclipse]
  * [Neuerungen im Azure-Toolkit für Eclipse]
  * [Installieren des Azure-Toolkits für Eclipse]
  * [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse]
  * [Erstellen einer „Hello World“-Web-App für Azure in Eclipse]
* [Azure Toolkit für IntelliJ]
  * [Neuerungen im Azure-Toolkit für IntelliJ]
  * [Installieren des Azure Toolkit für IntelliJ]
  * [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ]
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
[Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Neuerungen im Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Neuerungen im Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-Tools für Visual Studio Team Services]: https://java.visualstudio.com/

[Einführung in Microsoft Azure Storage]: /azure/storage/storage-introduction
[Informationen zu Azure-Speicherkonten]: /azure/storage/storage-create-storage-account
[Azure Storage-Replikation]: /azure/storage/storage-redundancy
[Skalierbarkeits- und Leistungsziele für Azure Storage]: /azure/storage/storage-scalability-targets
[Naming and Referencing Containers, Blobs, and Metadata]: http://go.microsoft.com/fwlink/?LinkId=255555

[Größen für Windows-Speicherkonten in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Größen für Linux-Speicherkonten in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Preise von Windows-Speicherkonten]: /pricing/details/virtual-machines/windows/
[Preise von Linux-Speicherkonten]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-intellij-managing-storage-accounts-using-azure-explorer/DC02.png


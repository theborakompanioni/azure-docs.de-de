---
title: "Verwalten von Speicherkonten mit Azure-Explorer für Eclipse | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Ihre Azure Storage-Konten mit Azure-Explorer für Eclipse verwalten."
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
ms.openlocfilehash: 3c95ddde2909cd8dee055724ba01f22647627bd3
ms.lasthandoff: 04/22/2017


---

# <a name="managing-storage-accounts-using-the-azure-explorer-for-eclipse"></a>Verwalten von Speicherkonten mit Azure-Explorer für Eclipse

Der Azure-Explorer gehört zum Azure-Toolkit für Eclipse und bietet Java-Entwicklern eine einfach zu bedienende Lösung für das Verwalten von Speicherkonten in ihrem Azure-Konto innerhalb der IDE von Eclipse.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

[!INCLUDE [azure-toolkit-for-eclipse-show-azure-explorer](../includes/azure-toolkit-for-eclipse-show-azure-explorer.md)]

## <a name="creating-a-storage-account-in-eclipse"></a>Erstellen eines Speicherkontos in Eclipse

Die folgenden Schritte begleiten Sie durch die Schritte zum Erstellen eines Speicherkontos mit dem Azure-Explorer.

1. Melden Sie sich beim Azure-Konto gemäß den Anweisungen im Artikel [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse] an.

1. Erweitern Sie in der Ansicht des **Azure-Explorers** den Knoten **Azure**. Klicken Sie mit der rechten Maustaste auf **Speicherkonten**, und klicken Sie dann auf **Speicherkonto erstellen**.
   ![Menü „Speicherkonto erstellen“][CS01]

1. Wenn das Dialogfeld **Speicherkonto erstellen** angezeigt wird, geben Sie die folgenden Optionen an: ![Dialogfeld „Speicherkonto erstellen“][CS02]

   a. **Name:** Geben Sie den Namen des neuen Speicherkontos an.

   b. **Abonnement:** Wählen Sie das Azure-Abonnement aus, das Sie für das neue Speicherkonto verwenden möchten.

   c. **Ressourcengruppe:** Geben Sie die Ressourcengruppe für den virtuellen Computer an. Sie müssen eine der folgenden Optionen auswählen:
      * **Neu erstellen:** gibt an, dass Sie eine neue Ressourcengruppe erstellen möchten
      * **Vorhandene verwenden:** gibt an, dass Sie in einer Liste von Ressourcengruppen, die Ihrem Azure-Konto zugeordnet sind, eine Auswahl treffen möchten

   d. **Standort:** Geben Sie den Standort an, an dem Ihr Speicherkonto erstellt wird, z.B. „USA, Westen“.

   e. **Kontoart:** Geben Sie den Typ des zu erstellenden Speicherkontos an, z.B. „Blob Storage“. (Weitere Informationen finden Sie unter [Informationen zu Azure Storage-Konten].)

   f. **Leistung:** Geben Sie an, welches Speicherkontoangebot für den ausgewählten Herausgeber verwendet werden soll, z.B. „Premium“. (Weitere Informationen finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage].)

   g. **Replikation:** Geben Sie die Replikation des Speicherkontos an, z.B. „Zonenredundant“. (Weitere Informationen finden Sie unter [Azure Storage-Replikation].)

1. Nachdem Sie alle oben genannten Optionen angegeben haben, klicken Sie auf **Erstellen**.

## <a name="creating-a-storage-container-in-eclipse"></a>Erstellen eines Speichercontainers in Eclipse

Die folgenden Schritte begleiten Sie durch die Schritte zum Erstellen eines Speichercontainers mit dem Azure-Explorer.

1. Klicken Sie im Azure-Explorer mit der rechten Maustaste auf das Speicherkonto, in dem Sie einen Container erstellen möchten, und klicken Sie dann auf **Blobcontainer erstellen**.
   ![Menü „Blobcontainer erstellen“][CC01]

1. Wenn das Dialogfeld **Blobcontainer erstellen** angezeigt wird, geben Sie den Namen für Ihren Container ein, und klicken Sie dann auf **OK**. (Ausführliche Informationen zum Benennen von Containern und Blobs finden Sie unter [Benennen von Containern, Blobs und Metadaten und Verweisen auf diese].) ![Dialogfeld zum Erstellen eines Speichercontainers][CC02]

## <a name="deleting-a-storage-container-in-eclipse"></a>Löschen eines Speichercontainers in Eclipse

Um mit dem Azure-Explorer einen Speichercontainer zu löschen, führen Sie die folgenden Schritte aus:

1. Klicken Sie im Azure-Explorer mit der rechten Maustaste auf den Speichercontainer, und klicken Sie dann auf **Löschen**.
   ![Menü zum Löschen eines Speichercontainers][DC01]

1. Klicken Sie auf **OK**, wenn Sie gefragt werden, ob der Speichercontainer gelöscht werden soll.
   ![Dialogfeld zum Löschen eines Speichercontainers][DC02]

## <a name="deleting-a-storage-account-in-eclipse"></a>Löschen eines Speicherkontos in Eclipse

Um mit dem Azure-Explorer ein Speicherkonto zu löschen, führen Sie die folgenden Schritte aus:

1. Klicken Sie in der Ansicht des **Azure-Explorers** mit der rechten Maustaste auf das Speicherkonto, und wählen Sie **Löschen** aus.
   ![Menü zum Löschen eines Speicherkontos][DS01]

1. Klicken Sie auf **OK**, wenn Sie gefragt werden, ob das Speicherkonto gelöscht werden soll.
   ![Dialogfeld zum Löschen eines Speicherkontos][DS02]

## <a name="see-also"></a>Weitere Informationen
Weitere Informationen zu Azure Storage-Konten sowie Größen und Preisen finden Sie unter den folgenden Links:

* [Einführung in Microsoft Azure Storage]
* [Informationen zu Azure Storage-Konten]
* Größen für Azure Storage-Konten
  * [Größen für Windows-Speicherkonten in Azure]
  * [Größen für Linux-Speicherkonten in Azure]
* Preise von Azure Storage-Konten
  * [Preise von Windows-Speicherkonten]
  * [Preise von Linux-Speicherkonten]

Weitere Informationen zu den Azure-Toolkits für Java-IDEs finden Sie unter den folgenden Links:

* [Azure-Toolkit für Eclipse]
  * [Neuerungen im Azure-Toolkit für Eclipse]
  * [Installieren des Azure-Toolkits für Eclipse]
  * [Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse] (Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse)
  * [Erstellen einer „Hello World“-Web-App für Azure in Eclipse]
* [Azure Toolkit für IntelliJ]
  * [Neuerungen im Azure-Toolkit für IntelliJ]
  * [Installieren des Azure Toolkit für IntelliJ]
  * [Azure Sign In Instructions for the Azure Toolkit for IntelliJ] (Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ)
  * [Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center] und in den [Java-Tools für Visual Studio Team Services].

<!-- URL List -->

[Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse.md
[Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij.md
[Erstellen einer „Hello World“-Web-App für Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Erstellen einer „Hello World“-Web-App für Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installieren des Azure-Toolkits für Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Installieren des Azure Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md (Anleitung zur Azure-Anmeldung für das Azure-Toolkit für Eclipse)
[Azure Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md (Anleitung zur Azure-Anmeldung für das Azure-Toolkit für IntelliJ)
[Neuerungen im Azure-Toolkit für Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Neuerungen im Azure-Toolkit für IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/
[Java-Tools für Visual Studio Team Services]: https://java.visualstudio.com/

[Einführung in Microsoft Azure Storage]: /azure/storage/storage-introduction
[Informationen zu Azure Storage-Konten]: /azure/storage/storage-create-storage-account
[Azure Storage-Replikation]: /azure/storage/storage-redundancy
[Skalierbarkeits- und Leistungsziele für Azure Storage]: /azure/storage/storage-scalability-targets
[Benennen von Containern, Blobs und Metadaten und Verweisen auf diese]: http://go.microsoft.com/fwlink/?LinkId=255555

[Größen für Windows-Speicherkonten in Azure]: /azure/virtual-machines/virtual-machines-windows-sizes
[Größen für Linux-Speicherkonten in Azure]: /azure/virtual-machines/virtual-machines-linux-sizes
[Preise von Windows-Speicherkonten]: /pricing/details/virtual-machines/windows/
[Preise von Linux-Speicherkonten]: /pricing/details/virtual-machines/linux/

<!-- IMG List -->

[CS01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS01.png
[CS02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CS02.png
[CC01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC01.png
[CC02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/CC02.png

[DS01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS01.png
[DS02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DS02.png
[DC01]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC01.png
[DC02]: ./media/azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer/DC02.png


---
title: Azure-Speicherkontenliste
description: "Verwalten Ihrer Speicherkontoeinstellungen mit dem Azure-Toolkit für Eclipse"
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: bbacfcd8-dbf5-4265-a930-59f508de5325
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: ff60ebaddd3a7888cee612f387bd0c50799496ac
ms.openlocfilehash: 9ef430dfaa43f9bc4294593c8abbdaf457ef07c8
ms.lasthandoff: 01/05/2017


---
# <a name="azure-storage-account-list"></a>Azure-Speicherkontenliste
Azure-Speicherkonten bieten Downloadspeicherorte für Ihr JDK, den Anwendungsserver und beliebige Komponenten sowie zum Speichern von Zustandsinformationen für das Caching. Eclipse verwaltet eine Liste der bekannten Speicherkonten, die für Ihre Projekte im Eclipse-Arbeitsbereich verfügbar sind. Zum Öffnen des Dialogfelds **Storage Accounts**, mit dem diese Liste in Eclipse verwaltet wird, klicken Sie auf **Window** und dann auf **Preferences**. Erweitern Sie anschließend **Azure**, und klicken Sie dann auf **Storage Accounts**.

Im Folgenden ist das Dialogfeld **Storage Accounts** abgebildet.

![][ic719496]

Dieses Dialogfeld kann auch über einen der Links **Accounts** in Dialogfeldern geöffnet werden, in denen Speicherkonten verwendet werden, z. B.:

* Registerkarte **JDK** im Dialogfeld **Server Configuration**.
* Registerkarte **Server** im Dialogfeld **Server Configuration**.
* Dialogfeld **Add Component**.
* Eigenschaftendialogfeld **Caching**

## <a name="to-import-your-storage-accounts-using-a-publish-settings-file"></a>So importieren Sie Ihre Speicherkonten mithilfe einer Datei mit Veröffentlichungseinstellungen
1. Klicken Sie im Dialogfeld **Storage Accounts** auf **Import from PUBLISH-SETTINGS file**.

2. (Überspringen Sie diesen Schritt, wenn Sie bereits eine Datei mit Veröffentlichungseinstellungen auf Ihrem lokalen Computer gespeichert haben.) Klicken Sie im Dialogfeld **Import Subscription Information** auf **Download PUBLISH-SETTINGS File**. Wenn Sie noch nicht bei Ihrem Azure-Konto angemeldet sind, werden Sie zur Anmeldung aufgefordert. Anschließend werden Sie aufgefordert, eine Azure-Datei mit Veröffentlichungseinstellungen zu speichern. (Sie können die resultierenden Anweisungen auf den Anmeldeseiten ignorieren – diese werden vom Azure-Portal bereitgestellt und sind für Visual Studio-Benutzer gedacht.) Speichern Sie sie auf Ihrem lokalen Computer.

3. Klicken Sie im Dialogfeld **Import Subscription Information** auf die Schaltfläche **Browse**, wählen Sie die zuvor lokal gespeicherte Datei mit den Veröffentlichungseinstellungen aus, und klicken Sie dann auf **Open**.

4. Klicken Sie auf **OK**, um das Dialogfeld **Import Subscription Information** zu schließen.

## <a name="to-create-a-new-storage-account"></a>So erstellen Sie ein neues Speicherkonto
1. Klicken Sie im Dialogfeld **Storage Accounts** auf **Add**.

2. Klicken Sie im Dialogfeld **Add Storage Accounts** auf **New**.

3. Geben Sie im Dialogfeld **New Storage Account** Werte für folgende Informationen ein:

   * Name des Speicherkontos.

   * Speicherort des Speicherkontos.

   * Beschreibung des Speicherkontos.

   * Abonnement, zu dem das Speicherkonto gehört.

4. Klicken Sie auf **OK**, um das Dialogfeld **New Storage Account** zu schließen.

Es kann mehrere Minuten dauern, bis das Speicherkonto erstellt wurde. Nachdem es erstellt wurde, klicken Sie auf **OK**, um das Dialogfeld **Add Storage Account** zu schließen. Das neue Speicherkonto wird der Liste verfügbarer Speicherkonten hinzugefügt.

## <a name="to-add-an-existing-storage-account-to-the-list"></a>Hinzufügen eines vorhandenen Speicherkontos zu der Liste
1. Wenn Sie nicht bereits über ein Azure-Konto verfügen, erstellen Sie anhand der zuvor im Abschnitt **So erstellen Sie ein neues Speicherkonto** beschriebenen Schritte. (Alternativ können Sie im [Azure-Verwaltungsportal][Azure Management Portal] ein neues Speicherkonto erstellen.)

2. Klicken Sie im Dialogfeld **Storage Accounts** auf **Add**.

3. Geben Sie im Dialogfeld **Add Storage Account** Werte für **Name** und **Access Key** ein. Kontoname und Zugriffsschlüssel müssen ein vorhandenes Azure-Speicherkonto betreffen. Verwenden Sie den Abschnitt **Storage** im [Azure-Verwaltungsportal][Azure Management Portal], um die Speicherkontonamen und -schlüssel anzuzeigen. Das Dialogfeld **Add Storage Account** sieht etwa wie folgt aus.
   
   ![][ic719497]

4. Klicken Sie auf **OK**, um das Dialogfeld **Add Storage Account** zu schließen.

## <a name="to-modify-a-storage-account-to-use-a-new-access-key"></a>So ändern Sie ein Speicherkonto zur Verwendung eines neuen Zugriffsschlüssels
1. Klicken Sie im Dialogfeld **Storage Accounts** auf das Speicherkonto, das Sie bearbeiten möchten, und dann auf **Edit**.

2. Ändern Sie im Dialogfeld **Edit Storage Account Access Key** den Wert von **Access Key**.

3. Klicken Sie auf **OK**, um das Dialogfeld **Edit Storage Account Access Key** zu schließen.

## <a name="to-remove-a-storage-account-from-the-list-maintained-in-eclipse"></a>So entfernen Sie ein Speicherkonto aus der in Eclipse verwalteten Liste
1. Klicken Sie im Dialogfeld **Storage Accounts** auf das Speicherkonto, das Sie bearbeiten möchten, und dann auf **Remove**.

2. Klicken Sie auf **OK**, wenn Sie zum Entfernen des Speicherkontos aufgefordert werden.

> [!NOTE]
> Durch das Entfernen des Speicherkontos im Dialogfeld **Storage Accounts** wird das Konto nur aus der Liste der unter Eclipse sichtbaren Speicherkonten entfernt. Das Speicherkonto wird dabei nicht aus dem Azure-Abonnement entfernt. Darüber hinaus wird das Speicherkonto möglicherweise erneut in der Liste angezeigt, nachdem Eclipse die Daten Ihres Abonnements neu geladen hat.
> 
> 

## <a name="see-also"></a>Weitere Informationen
[Azure-Toolkit für Eclipse][Azure Toolkit for Eclipse]

[Installieren des Azure-Toolkits für Eclipse][Installing the Azure Toolkit for Eclipse] 

[Erstellen einer Hello World-Anwendung für Azure in Eclipse][Creating a Hello World Application for Azure in Eclipse]

Weitere Informationen zum Verwenden von Azure mit Java finden Sie im [Azure Java Developer Center][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic719496]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719496.png
[ic719497]: ./media/azure-toolkit-for-eclipse-azure-storage-account-list/ic719497.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/dn205108.aspx -->


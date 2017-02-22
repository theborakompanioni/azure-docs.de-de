---
title: "Hochladen eines Zertifikats für die Azure-Verwaltungs-API | Microsoft-Dokumentation"
description: "Erfahren Sie, wie ein API-Verwaltungszertifikat für das klassische Azure-Portal hochgeladen wird."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 1b813833-39c8-46be-8666-fd0960cfbf04
ms.service: na
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/18/2016
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 783390791f27a9bf8ea400403a8b0f866f8c5cee
ms.openlocfilehash: 0f1ae486340270cac5cfdb5d44485d05cb996388


---
# <a name="upload-an-azure-management-api-management-certificate"></a>Hochladen eines Verwaltungszertifikats für die Azure-Verwaltung-API
Mit Verwaltungszertifikaten können Sie die Authentifizierung mit der in Azure bereitgestellten Dienstverwaltungs-API durchführen. Diese Zertifikate werden in vielen Programmen und Tools (z. B. Visual Studio oder Azure SDK) zum Automatisieren der Konfiguration und Bereitstellung verschiedener Azure-Dienste verwendet. **Dies gilt nur für das klassische Azure-Portal.**

> [!WARNING]
> Vorsicht ist geboten! Alle Personen, die die Authentifizierung mit diesen Zertifikaten durchführen, können das zugeordnete Abonnement verwalten.
>
>

Weitere Informationen zu Azure-Zertifikaten (einschließlich der Erstellung eines selbstsignierten Zertifikats) sind bei Bedarf [verfügbar](cloud-services/cloud-services-certs-create.md#what-are-management-certificates) .

Sie können auch [Azure Active Directory](https://azure.microsoft.com/en-us/services/active-directory/) für die Authentifizierung von Clientcode zur Automatisierung verwenden.

## <a name="upload-a-management-certificate"></a>Hochladen eines Verwaltungszertifikats
Sobald Sie ein Verwaltungszertifikat erstellt haben (CER-Datei nur mit dem öffentlichen Schlüssel), können Sie es in das Portal hochladen. Wenn das Zertifikat im Portal verfügbar ist, kann jeder Benutzer mit einem entsprechenden Zertifikat (privater Schlüssel) über die Verwaltungs-API eine Verbindung herstellen und auf die Ressourcen für das zugehörige Abonnement zugreifen.

1. Melden Sie sich beim [klassischen Azure-Portal](http://manage.windowsazure.com)an.
2. Stellen Sie sicher, dass Sie das Abonnement auswählen, dem Sie ein Zertifikat zuordnen möchten. Wählen Sie rechts oben im Portal den Text **Abonnements** aus.

    ![Einstellungen](./media/azure-api-management-certs/subscription.png)
3. Nachdem Sie das gewünschte Abonnement ausgewählt haben, wählen Sie links im Portal **Einstellungen** aus (möglicherweise müssen Sie nach unten scrollen).

    ![Einstellungen](./media/azure-api-management-certs/settings.png)
4. Wählen Sie die Registerkarte **Verwaltungszertifikate** aus.

    ![Einstellungen](./media/azure-api-management-certs/certificates-tab.png)
5. Wählen Sie die Schaltfläche **Hochladen** aus.

    ![Einstellungen](./media/azure-api-management-certs/upload.png)
6. Füllen Sie das Dialogfeld aus, und wählen Sie das **Häkchen**aus.

    ![Einstellungen](./media/azure-api-management-certs/upload-dialog.png)

## <a name="next-steps"></a>Nächste Schritte
Da Sie jetzt einem Abonnement ein Verwaltungszertifikat zugeordnet haben, können Sie (nachdem Sie das entsprechende Zertifikat lokal installiert haben) programmgesteuert eine Verbindung mit der [Dienstverwaltungs-REST-API](https://msdn.microsoft.com/library/azure/mt420159.aspx) herstellen und die verschiedenen Azure-Ressourcen automatisieren, die auch diesem Abonnement zugeordnet sind.



<!--HONumber=Dec16_HO1-->



---
title: "Keine funktionierende Connectorgruppe für eine Anwendungsproxyanwendung gefunden | Microsoft-Dokumentation"
description: "Beheben von Problemen, die auftreten können, wenn kein funktionierender Connector in einer Connectorgruppe für die Anwendung mit dem Azure AD-Anwendungsproxy vorhanden ist"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 5b133afd82afecbfab250c2760404a4173086fa5
ms.lasthandoff: 04/17/2017


---

# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Keine funktionierende Connectorgruppe für eine Anwendungsproxyanwendung gefunden

Dieser Artikel unterstützt Sie beim Beheben häufiger Probleme, die auftreten können, wenn für eine Anwendungsproxyanwendung, die mit Azure Active Directory integriert wird, kein Connector erkannt wird.

## <a name="overview-of-steps"></a>Übersicht über die Schritte
Wenn für Ihre Anwendung kein funktionierender Connector in einer Connectorgruppe vorhanden ist, gibt es verschiedene Methoden, um das Problem zu beheben:

-   Wenn sich kein Connector in der Gruppe befindet, haben Sie die folgenden Möglichkeiten:

    -   Laden Sie einen neuen Connector auf den rechten lokalen Server herunter, und weisen Sie ihn dieser Gruppe zu.

    -   Verschieben Sie einen aktiven Connector in die Gruppe.

-   Wenn sich kein aktiver Connector in der Gruppe befindet, haben Sie die folgenden Möglichkeiten:

    -   Identifizieren Sie die Ursache für die Inaktivität des Connectors, und beheben Sie das Problem.

    -   Verschieben Sie einen aktiven Connector in die Gruppe.

Öffnen Sie das Menü „Anwendungsproxy“ in Ihrer Anwendung, und betrachten Sie die Warnmeldung der Connectorgruppe, um die Ursache für das Problem zu erfahren. Sie gibt entweder an, dass die Gruppe mindestens einen Connector benötigt (es befindet sich einer in der Gruppe), oder dass kein aktiver Connector vorhanden ist (daher liegen wahrscheinlich inaktive Connectors vor).

   ![Connectorgruppenauswahl im Azure-Portal](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Ausführliche Informationen zu den jeweiligen Optionen finden Sie unten im entsprechenden Abschnitt. Bei jeder dieser Optionen wird angenommen, dass Sie von der Connectorverwaltungsseite aus starten. Wenn Sie die oben aufgeführte Fehlermeldung betrachten, können Sie zu dieser Seite wechseln, indem Sie auf die Warnung klicken. Wechseln Sie andernfalls zu **Azure Active Directory**, klicken Sie auf **Unternehmens-Apps** und dann auf **Anwendungsproxy**, um zu dieser Seite zu wechseln.

   ![Connectorgruppenverwaltung im Azure-Portal](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Herunterladen eines neuen Connectors

Verwenden Sie die Schaltfläche „Connector herunterladen“ am Seitenanfang, um einen neuen Connector herunterzuladen.

Beachten Sie Folgendes: Der Connector muss auf einem Computer mit uneingeschränktem Zugriff auf die Back-End-Anwendung installiert werden, und befindet sich in der Regel auf dem gleichen Server wie die Anwendung. Nach dem Herunterladen sollte der Connector in diesem Menü angezeigt werden. Klicken Sie auf den Connector und verwenden die Dropdownliste „Connectorgruppe“, um sicherzustellen, dass er der richtigen Gruppe angehört. Speichern Sie die Änderungen.

   ![Herunterladen des Connectors aus dem Azure-Portal](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Verschieben eines aktiven Connectors

Wenn Sie über einen aktiven Connector verfügen, der dieser Gruppe angehören sollte und uneingeschränkten Zugriff auf die Back-End-Anwendung hat, können Sie den Connector in die zugewiesene Gruppe verschieben. Klicken Sie hierzu auf den Connector. Verwenden Sie im Feld „Connectorgruppe“ die Dropdownliste, um die richtige Gruppe auszuwählen, und klicken Sie dann auf „Speichern“.

## <a name="resolve-an-inactive-connector"></a>Beheben eines inaktiven Connectors

Wenn die einzigen Connectors in der Gruppe inaktiv sind, befinden sie sich voraussichtlich auf einem Computer, bei dem die Blockierung nicht für alle erforderlichen Ports aufgehoben wurde.

Weitere Informationen zur Untersuchung dieses Problems finden Sie im Dokument zur Problembehandlung bei Ports.

## <a name="next-steps"></a>Nächste Schritte
[Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md)




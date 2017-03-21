---
title: "Erstellen, Verknüpfen, Löschen oder Verschieben eines Integrationskontos in Azure Logic Apps | Microsoft-Dokumentation"
description: "Erstellen eines Integrationskontos und Verknüpfen eines Integrationskontos mit Logic Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: d3ad9e99-a9ee-477b-81bf-0881e11e632f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/23/2017
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: 8a531f70f0d9e173d6ea9fb72b9c997f73c23244
ms.openlocfilehash: 2a7ecec310f4e321417364eee854409b2f85e2d3
ms.lasthandoff: 03/10/2017


---

# <a name="what-is-an-integration-account"></a>Was ist ein Integrationskonto?

Mit einem Integrationskonto können Apps für die Unternehmensintegration Artefakte wie Schemas, Zuordnungen, Zertifikate, Partner und Vereinbarungen verwalten. Jede Integrations-App, die Sie erstellen, verwendet ein Integrationskonto, um auf diese Schemas, Zuordnungen, Zertifikate usw. zuzugreifen.

## <a name="create-an-integration-account"></a>Erstellen eines Integrationskontos

1.    Melden Sie sich beim [Azure-Portal](http://portal.azure.com "Azure-Portal") an. Wählen Sie im linken Menü die Option **Weitere Dienste** aus.

    ![„Weitere Dienste“ auswählen](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Geben Sie im Suchfeld den Begriff „integration“ als Filter ein. Wählen Sie in der Ergebnisliste die Option **Integrationskonten** aus.

    ![Nach „Integration“ filtern, „Integrationskonten“ auswählen](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. Wählen Sie im oberen Seitenbereich die Option **Hinzufügen** aus.

    ![„Hinzufügen“ auswählen](./media/logic-apps-enterprise-integration-accounts/account-3.png)

4. Benennen Sie Ihr Integrationskonto, und wählen Sie das Azure-Abonnement aus, das Sie verwenden möchten. Erstellen Sie eine neue **Ressourcengruppe**, oder wählen Sie eine vorhandene Ressourcengruppe aus. Wählen Sie anschließend einen **Standort** zum Hosten Ihres Integrationskontos sowie einen **Tarif** aus. 

    Wählen Sie abschließend **Erstellen** aus.

    ![Details für das Integrationskonto angeben](./media/logic-apps-enterprise-integration-accounts/account-4.png)

    Azure stellt Ihr Integrationskonto am ausgewählten Standort bereit. Dieser Vorgang dauert in der Regel maximal eine Minute.

5. Aktualisieren Sie die Seite. Das neue Integrationskonto wird nun aufgeführt.

    ![Das neue Integrationskonto erscheint.](./media/logic-apps-enterprise-integration-accounts/account-5.png) 

Verknüpfen Sie als Nächstes das erstellte Integrationskonto mit Ihrer Logik-App. 

## <a name="link-an-integration-account-to-a-logic-app"></a>Verknüpfen eines Integrationskontos mit einer Logik-App

Damit Ihre Logik-Apps auf Zuordnungen, Schemas, Vereinbarungen und andere Artefakte in Ihrem Integrationskonto zugreifen können, müssen Sie das Integrationskonto mit Ihrer Logik-App verknüpfen.

### <a name="prerequisites"></a>Voraussetzungen

* Ein Integrationskonto
* Eine Logik-App

> [!NOTE] 
> Bevor Sie beginnen, vergewissern Sie sich, dass sich Ihr Integrationskonto und Ihre Logik-App am *gleichen Azure-Standort* befinden.


1. Wählen Sie im Azure-Portal Ihre Logik-App aus, und überprüfen Sie ihren Standort.

    ![Logik-App auswählen und Standort überprüfen](./media/logic-apps-enterprise-integration-accounts/linkaccount-1.png)

2. Wählen Sie unter **Einstellungen** die Option **Integrationskonto** aus.

    ![„Integrationskonto“ auswählen](./media/logic-apps-enterprise-integration-accounts/linkaccount-2.png)

3. Wählen Sie in der Liste **Wählen Sie ein Integrationskonto aus.** das Integrationskonto aus, das Sie mit Ihrer Logik-App verknüpfen möchten. Wählen Sie zum Fertigstellen der Verknüpfung die Option **Speichern** aus.

    ![Auswählen Ihres Integrationskontos](./media/logic-apps-enterprise-integration-accounts/linkaccount-3.png)

    Sie erhalten eine Benachrichtigung mit dem Hinweis, dass Ihr Integrationskonto mit Ihrer Logik-App verknüpft ist und dass Ihrer Logik-App nun alle Artefakte in Ihrem Integrationskonto zur Verfügung stehen.

    ![Die Logik-App ist mit Ihrem Integrationskonto verknüpft.](./media/logic-apps-enterprise-integration-accounts/linkaccount-5.png)

Nachdem Ihr Integrationskonto nun mit Ihrer Logik-App verknüpft ist, können Sie in Ihren Logik-Apps die B2B-Connectors verwenden. Zu den gängigen B2B-Connectors gehören unter anderem die XML-Überprüfung und die Flatfilecodierung/-decodierung.  

## <a name="delete-your-integration-account"></a>Löschen Ihres Integrationskontos

1. Wählen Sie **Weitere Dienste**.

    ![„Weitere Dienste“ auswählen](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Geben Sie im Suchfeld den Begriff „integration“ als Filter ein. Wählen Sie in der Ergebnisliste die Option **Integrationskonten** aus.

    ![Nach „Integration“ filtern, „Integrationskonten“ auswählen](./media/logic-apps-enterprise-integration-accounts/account-2.png)  

3. Wählen Sie das Integrationskonto aus, das Sie löschen möchten.

    ![Zu löschendes Integrationskonto auswählen](./media/logic-apps-enterprise-integration-accounts/account-5.png)

4. Wählen Sie im Menü die Option **Löschen** aus.

    ![„Löschen“ auswählen](./media/logic-apps-enterprise-integration-accounts/delete.png)

5. Bestätigen Sie, dass Sie das Integrationskonto wirklich löschen möchten.

## <a name="move-your-integration-account"></a>Verschieben Ihres Integrationskontos

Gehen Sie wie folgt vor, wenn Sie ein Integrationskonto in ein anderes Azure-Abonnement oder in eine andere Ressourcengruppe verschieben möchten.

> [!IMPORTANT]
> Nach dem Verschieben eines Integrationskontos müssen alle Skripts mit den neuen Ressourcen-IDs aktualisiert werden.

1. Wählen Sie **Weitere Dienste**.

    ![„Weitere Dienste“ auswählen](./media/logic-apps-enterprise-integration-accounts/account-1.png)

2. Geben Sie im Suchfeld den Begriff „integration“ als Filter ein. Wählen Sie in der Ergebnisliste die Option **Integrationskonten** aus.

    ![Nach „Integration“ filtern, „Integrationskonten“ auswählen](./media/logic-apps-enterprise-integration-accounts/account-2.png)

3. Wählen Sie das Integrationskonto aus, das Sie verschieben möchten. Wählen Sie unter **Einstellungen** die Option **Eigenschaften** aus.

    ![Zu verschiebendes Integrationskonto auswählen. Unter „Einstellungen“ die Option „Eigenschaften“ auswählen.](./media/logic-apps-enterprise-integration-accounts/move.png)

5. Ändern Sie die mit Ihrem Integrationskonto verknüpfte Ressourcengruppe oder das mit Ihrem Integrationskonto verknüpfte Azure-Abonnement.

    ![„Ressourcengruppe ändern“ oder „Abonnement ändern“ auswählen](./media/logic-apps-enterprise-integration-accounts/move-2.png)

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zu Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  



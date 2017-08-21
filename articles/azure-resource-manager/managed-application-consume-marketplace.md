---
title: Nutzen einer verwalteten Azure-Anwendung im Marketplace | Microsoft-Dokumentation
description: "Beschreibt wie eine verwaltete Azure-Anwendung erstellt wird, die über den Marketplace verfügbar ist."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/11/2017
ms.author: gauravbh; tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 54454e98a2c37736407bdac953fdfe74e9e24d37
ms.openlocfilehash: baf456740bddd562391ed64d707f990c8921d710
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---

# <a name="consume-azure-managed-applications-in-the-marketplace"></a>Nutzen von verwalteten Azure-Anwendungen im Marketplace

Wie im Artikel [Übersicht über verwaltete Anwendungen](managed-application-overview.md) beschrieben, gibt es zwei End-to-End Szenarios. Bei einem steht der Herausgeber oder Hersteller im Mittelpunkt, der eine verwaltete Anwendung zur Verwendung durch Kunden erstellen möchte. Beim zweiten geht es um den Kunden oder den Verbraucher der verwalteten Anwendung. Dieser Artikel behandelt das zweite Szenario. Er beschreibt, wie eine verwaltete Anwendung aus dem Microsoft Azure Marketplace bereitgestellt werden kann.

## <a name="create-from-the-marketplace"></a>Erstellen im Marketplace

Das Bereitstellen einer verwalteten Anwendung im Marketplace ähnelt der Vorgehensweise zur Bereitstellung jeder Art von Ressource im Marketplace. 

Wählen Sie im Portal **+ neu**, und suchen Sie nach der Art der Lösung, die Sie bereitstellen möchten. Wählen Sie aus den verfügbaren Optionen jene aus, die Sie benötigen.

![Suchlösungen](./media/managed-application-consume-marketplace/search-apps.png)

Überprüfen Sie die Zusammenfassung der Anwendung, und wählen Sie **Erstellen**.

![Verwaltete Anwendung erstellen](./media/managed-application-consume-marketplace/create-marketplace-managed-app.png)

Wie jede andere Lösung werden Ihnen Felder angezeigt, in die der Wert eingegeben werden muss. Diese Felder variieren je nach Typ der verwalteten Anwendung, die Sie erstellen. 

## <a name="view-support-information"></a>Supportinformationen anzeigen

Nachdem Ihre verwaltete Anwendung bereitgestellt wurde, zeigen Sie die Supportinformation für die Anwendung an. Im Blatt zur verwalteten Anwendung werden die Supportinformationen aufgeführt.

![support](./media/managed-application-consume-marketplace/support.png)

## <a name="view-publisher-permissions"></a>Anzeigeberechtigungen für Verleger anzeigen

Dem Hersteller, der Ihre Anwendung verwaltet, wird Zugriff auf Ihre Ressourcen gewährt. Um diese Berechtigungen zu sehen, wählen Sie **Autorisierungen** aus.

![Autorisierungen](./media/managed-application-consume-marketplace/authorizations.png)

## <a name="next-steps"></a>Nächste Schritte

* Informationen zum Veröffentlichen einer verwalteten Anwendung im Marketplace finden Sie unter [Verwaltete Azure-Anwendungen im Marketplace](managed-application-author-marketplace.md).
* Um verwaltete Anwendungen zu Veröffentlichen, die nur Benutzern in Ihrer Organisation zur Verfügung stehen, gehen Sie zu [Erstellen und Veröffentlichen von verwalteten Dienstkataloganwendungen](managed-application-publishing.md).
* Um verwaltete Anwendungen zu nutzen, die nur Benutzern in Ihrer Organisation zur Verfügung stehen gehen Sie zu [Nutzen einer verwalteten Dienstkataloganwendung](managed-application-consumption.md).

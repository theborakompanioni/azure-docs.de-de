---
title: Erste Schritte mit Azure Active Directory Domain Services | Microsoft-Dokumentation
description: Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals (Vorschauversion)
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: ace1ed4a-bf7f-43c1-a64a-6b51a2202473
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/28/2017
ms.author: maheshu
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: d2d570bc2bab3bfd28282cd95b0a1be65e96743d
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a>Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals (Vorschauversion)
Dieser Artikel zeigt, wie Azure Active Directory Domain Services (Azure AD DS) mithilfe des Azure-Portals aktiviert werden kann.

Um den Assistenten zum **Aktivieren von Azure AD Domain Services** zu starten, führen Sie die folgenden Schritte aus:

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Klicken Sie im linken Bereich auf **Neu**.
3. Wenn Sie sich auf dem Blatt **Neu** befinden, geben Sie **Domain Services** in die Suchleiste ein.

    ![Suche nach Domänendiensten](./media/getting-started/search-domain-services.png)

4. Klicken Sie auf **Azure AD Domain Services** in der Liste der Suchvorschläge. Wenn Sie sich auf dem Blatt **Azure AD Domain Services** befinden, klicken Sie auf die Schaltfläche **Erstellen**.

    ![Blatt: Domänendienste](./media/getting-started/domain-services-blade.png)

5. Der Assistent zum **Aktivieren von Azure AD Domain Services** wird gestartet.


## <a name="task-1-configure-basic-settings"></a>Aufgabe 1: Grundeinstellungen konfigurieren
Auf der Seite **Grundlagen** des Assistenten können Sie den DNS-Domänennamen für die verwaltete Domäne angeben. Sie können ebenfalls die Ressourcengruppe und den Azure-Standort angeben, für die die verwaltete Domäne bereitgestellt werden soll.

![Konfigurieren von Grundlagen](./media/getting-started/domain-services-blade-basics.png)

1. Wählen Sie den **DNS-Domänennamen** für Ihre verwaltete Domäne.

   * Standardmäßig ist der Standarddomänenname des Verzeichnisses (mit dem Suffix **.onmicrosoft.com**) ausgewählt.

   * Sie können auch einen benutzerdefinierten Domänennamen eingeben. In diesem Beispiel lautet der benutzerdefinierte Domänenname *contoso100.com*.

     > [!WARNING]
     > Das Präfix des angegebenen Domänennamens (beispielsweise *contoso100* im Domänennamen *contoso100.com*) darf maximal 15 Zeichen lang sein. Sie können keine verwaltete Domäne mit einem Präfix, das länger als 15 Zeichen ist, erstellen.
     >
     >

2. Stellen Sie sicher, dass der DNS-Domänenname, den Sie für die verwaltete Domäne ausgewählt haben, nicht bereits im virtuellen Netzwerk vorhanden ist. Überprüfen Sie insbesondere, ob Folgendes zutrifft:

   * Im virtuellen Netzwerk ist bereits eine Domäne mit dem gleichen DNS-Domänennamen vorhanden.

   * Das virtuelle Netzwerk, in dem Sie die verwaltete Domäne aktivieren möchten, hat eine VPN-Verbindung mit Ihrem lokalen Netzwerk. In diesem Szenario stellen Sie sicher, dass Sie keine Domäne mit demselben DNS-Domänennamen in Ihrem lokalen Netzwerk haben.

   * Im virtuellen Netzwerk ist bereits ein Clouddienst mit diesem Namen vorhanden.

3. Wählen Sie den **Typ des virtuellen Netzwerks**. Standardmäßig wird der virtuelle Netzwerktyp **Resource Manager** ausgewählt. Sie sollten diesen virtuellen Netzwerktyp für alle neu erstellten verwalteten Domänen verwenden.

4. Wählen Sie das **Azure-Abonnement**, in dem Sie die verwaltete Domäne erstellen möchten.

5. Wählen Sie die **Ressourcengruppe**, zu der die verwaltete Domäne gehören soll. Wählen Sie entweder die Option **Create new** (Neu erstellen) oder **Use existing** (Vorhandene verwenden), um die Ressourcengruppe auszuwählen.

6. Wählen Sie den **Azure-Speicherort**, in dem die verwaltete Domäne erstellt werden soll. Auf der Seite **Netzwerk** des Assistenten sehen Sie nur virtuelle Netzwerke, die zu dem von Ihnen gewählten Standort gehören.

7. Wenn Sie fertig sind, klicken Sie auf **OK**, um zur Seite **Netzwerk** des Assistenten zu gelangen.


## <a name="next-step"></a>Nächster Schritt
[Aufgabe 2: Konfigurieren der Netzwerkeinstellungen](active-directory-ds-getting-started-network.md)


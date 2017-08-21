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
ms.date: 07/15/2017
ms.author: maheshu
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: 7f420d60862adf61e4f21e5abac2932a742bd55d
ms.contentlocale: de-de
ms.lasthandoff: 07/17/2017

---
# <a name="enable-azure-active-directory-domain-services-using-the-azure-portal-preview"></a>Aktivieren von Azure Active Directory Domain Services mithilfe des Azure-Portals (Vorschauversion)


## <a name="before-you-begin"></a>Voraussetzungen
Informationen finden Sie unter [Netzwerkaspekte für die Azure Active Directory Domain Services](active-directory-ds-networking.md).


## <a name="task-2-configure-network-settings"></a>Aufgabe 2: Konfigurieren der Netzwerkeinstellungen
Die nächste Konfigurationsaufgabe besteht in der Erstellung eines virtuellen Azure-Netzwerks und eines dedizierten Subnetzes. Azure Active Directory Domain Services wird dann in diesem Subnetz Ihres virtuellen Netzwerks aktiviert. Sie können auch ein bestehendes virtuelles Netzwerk auswählen, in dem Sie das dedizierte Subnetz erstellen.

1. Klicken Sie auf **Virtual Network** (Virtuelles Netzwerk), um ein virtuelles Netzwerk auszuwählen.
2. Auf dem Blatt **Choose virtual network** (Virtuelles Netzwerk auswählen) sehen Sie alle bestehenden virtuellen Netzwerke. Sie können nur die virtuellen Netzwerke sehen, die zu der Ressourcengruppe und dem Azure-Speicherort gehören, die Sie auf der Seite **Grundlagen** des Assistenten ausgewählt haben.

3. Wählen Sie dann das virtuelle Netzwerk aus, in dem Azure AD Domain Services aktiviert werden sollen. Klicken Sie auf **Create new** (Neu erstellen), wenn Sie ein neues virtuelles Netzwerk erstellen möchten. Es wird dringend empfohlen, ein dediziertes Subnetz für Azure AD Domain Services zu verwenden. Wenn Sie ein bestehendes virtuelles Netzwerk auswählen, [erstellen Sie ein dediziertes Subnetz mithilfe der Erweiterung für virtuelle Netzwerke](../virtual-network/virtual-networks-create-vnet-arm-pportal.md), und wählen Sie dieses Subnetz anschließend aus. 

    ![Auswählen eines virtuellen Netzwerks](./media/getting-started/domain-services-blade-network-pick-vnet.png)

4. Klicken Sie auf **Subnet** (Subnetz), um das dedizierte Subnetz in dem virtuellen Netzwerk auszuwählen, in dem Ihre neue verwaltete Domäne aktiviert werden soll. Geben Sie im Blatt **Create subnet** (Subnetz erstellen) einen Namen für das Subnetz an, und klicken Sie auf **OK**, wenn Sie fertig sind. Erstellen Sie zum Beispiel ein Subnetz mit dem Namen „DomainServices“, um es für andere Administratoren verständlich zu machen, was innerhalb des Subnetzes bereitgestellt wird.

    ![Auswählen eines Subnetzes innerhalb des virtuellen Netzwerks](./media/getting-started/domain-services-blade-network-pick-subnet.png)

  > [!NOTE]
  > **Richtlinien für das Auswählen eines Subnetzes**
  > 1. Verwenden Sie ein dediziertes Subnetz für Azure AD Domain Services. Stellen Sie für dieses Subnetz keine weiteren virtuellen Computer bereit. Diese Konfiguration ermöglicht es Ihnen, Netzwerksicherheitsgruppen (NSGs) für Ihre Workloads bzw. virtuellen Computer zu konfigurieren, ohne Ihre verwaltete Domäne zu unterbrechen. Einzelheiten dazu finden Sie unter [networking considerations for Azure Active Directory Domain Services (Netzwerkaspekte für Azure Active Directory Domain Services)](active-directory-ds-networking.md).
  2. Wählen Sie für die Bereitstellung von Azure AD Domain Services nicht das Gatewaysubnetz, weil es sich dabei um eine nicht unterstützte Konfiguration handelt.
  3. Stellen Sie sicher, dass das von Ihnen ausgewählte Subnetz über einen ausreichend verfügbaren Adressraum von mindestens 3-5 verfügbaren IP-Adressen verfügt.
  >

5. Wenn Sie fertig sind, klicken Sie auf **OK**, um zur Seite **Administrator group** (Administratorgruppe) des Assistenten zu gelangen.


## <a name="next-step"></a>Nächster Schritt
[Task 3: configure administrative group and enable Azure AD Domain Services (Aufgabe 3: Konfigurieren einer administrativen Gruppe und Aktivieren von Azure AD Domain Services)](active-directory-ds-getting-started-admingroup.md)


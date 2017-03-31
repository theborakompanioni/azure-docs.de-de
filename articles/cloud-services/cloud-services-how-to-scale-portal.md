---
title: Automatisches Skalieren eines Clouddiensts im (klassischen) Portal | Microsoft Docs
description: "Erfahren Sie, wie Sie mit dem Portal Regeln für die automatische Skalierung für eine Clouddienst-Webrolle oder -Workerrolle in Azure konfigurieren."
services: cloud-services
documentationcenter: 
author: Thraka
manager: timlt
editor: 
ms.assetid: 701d4404-5cc0-454b-999c-feb94c1685c0
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: adegeo
translationtype: Human Translation
ms.sourcegitcommit: 4f2230ea0cc5b3e258a1a26a39e99433b04ffe18
ms.openlocfilehash: 7891fbd326340ed102f885e84993ef987e044023
ms.lasthandoff: 03/25/2017

---

# <a name="how-to-configure-auto-scaling-for-a-cloud-service-in-the-portal"></a>Konfigurieren der automatischen Skalierung für einen Clouddienst im Portal
> [!div class="op_single_selector"]
> * [Azure-Portal](cloud-services-how-to-scale-portal.md)
> * [Klassisches Azure-Portal](cloud-services-how-to-scale.md)

Bedingungen können für eine Clouddienst-Workerrolle festgelegt werden, die ein horizontales Herunter- oder Hochskalieren auslösen. Die Bedingungen für die Rolle können auf der CPU, dem Datenträger oder der Netzwerklast der Rolle basieren. Sie können auch eine Bedingung basierend auf einer Nachrichtenwarteschlange oder der Metrik einer anderen mit Ihrem Abonnement verknüpften Azure-Ressource festlegen.

> [!NOTE]
> Dieser Artikel konzentriert sich auf Clouddienst-Webrollen und -Workerrollen. Wenn Sie einen virtuellen Computer (klassisch) direkt erstellen, wird er in einem Clouddienst gehostet. Sie können einen standardmäßigen virtuellen Computer skalieren, indem Sie ihn einer [Verfügbarkeitsgruppe](../virtual-machines/windows/classic/configure-availability.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json) zuordnen und manuell aktivieren oder deaktivieren.

## <a name="considerations"></a>Überlegungen
Folgendes ist allerdings zu beachten, bevor die Skalierung einer Anwendung konfiguriert wird:

* Die Skalierung ist abhängig von der Kernspeichernutzung.

    Größere Rolleninstanzen verwenden mehr Kerne. Sie können eine Anwendung nur innerhalb der für Ihr Abonnement geltenden Kerngrenzwerte skalieren. Nehmen Sie z.B. an, Ihr Abonnement sei auf 20 Kerne begrenzt. Wenn Sie eine Anwendung mit zwei mittelgroßen Clouddiensten ausführen (insgesamt vier Kerne), stehen für das zentrale Hochskalieren anderer Clouddienstbereitstellungen in Ihrem Abonnement nur noch sechzehn Kerne zur Verfügung. Weitere Informationen zu Größen finden Sie unter [Größen für Clouddienste](cloud-services-sizes-specs.md).

* Sie können basierend auf einem Schwellenwert für die Nachrichtenwarteschlange skalieren. Weitere Informationen über die Verwendung von Warteschlangen finden Sie unter [Erste Schritte mit Azure Queue Storage mit .NET](../storage/storage-dotnet-how-to-use-queues.md).

* Sie können auch andere Ressourcen skalieren, die Ihrem Abonnement zugeordnet sind.

* Um die Hochverfügbarkeit Ihrer Anwendung zu gewährleisten, sollte diese mit mindestens zwei Rolleninstanzen bereitgestellt werden. Weitere Informationen finden Sie unter [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/).

> [!WARNING]
> Automatische Skalierung funktioniert nur mit klassischen Azure Storage-Konten. Sie funktioniert nicht mit Azure Resource Manager-Speicherkonten.


## <a name="where-scale-is-located"></a>Aufrufen der Funktion für die Skalierung
Nachdem Sie Ihren Clouddienst ausgewählt haben, sollte das Blatt für den Clouddienst angezeigt werden.

1. Wählen Sie auf dem Blatt für den Clouddienst auf der Kachel **Rollen und Instanzen** den Namen des Clouddiensts aus.   
   **WICHTIG**: Sie müssen auf die Clouddienstrolle klicken, nicht auf die Rolleninstanz unterhalb der Rolle.
   
    ![](./media/cloud-services-how-to-scale-portal/roles-instances.png)
2. Wählen Sie die Kachel **Skalieren** aus.
   
    ![](./media/cloud-services-how-to-scale-portal/scale-tile.png)

## <a name="automatic-scale"></a>Automatisches Skalieren
Sie können die Skalierungseinstellungen für eine Rolle **manuell** oder **automatisch** konfigurieren. Bei der manuellen Konfiguration legen Sie die absolute Anzahl von Instanzen fest. Bei der automatischen Konfiguration können Sie jedoch Regeln festlegen, die steuern, wie und in welchem Umfang skaliert werden soll.

Legen Sie die Option **Skalieren nach** auf **Zeitplan und Leistungsregeln** fest.

![Clouddienst-Skalierungseinstellungen mit Profil und Regel](./media/cloud-services-how-to-scale-portal/schedule-basics.png)

1. Ein vorhandenes Profil.
2. Fügen Sie eine Regel für das übergeordnete Profil hinzu.
3. Fügen Sie ein weiteres Profil hinzu.

Wählen Sie **Profil hinzufügen**aus. Das Profil bestimmt, welchen Modus Sie für die Skalierung verwenden möchten: **immer**, **Wiederholung**, **festes Datum**.

Nachdem Sie das Profil und die Regeln konfiguriert haben, wählen Sie oben das Symbol **Speichern** aus.

#### <a name="profile"></a>Profil
Das Profil legt die Mindest- und Höchstanzahl von Instanzen für die Skalierung fest und bestimmt zudem, wann dieser Skalierungsbereich aktiv ist.

* **Always**
  
    Dieser Bereich von Instanzen bleibt immer verfügbar.  
  
    ![Clouddienst, der immer skaliert wird](./media/cloud-services-how-to-scale-portal/select-always.png)
* **Serie**
  
    Wählen Sie eine Reihe von Wochentagen zum Skalieren aus.
  
    ![Clouddienst, der mit einem Wiederholungsplan skaliert wird](./media/cloud-services-how-to-scale-portal/select-recurrence.png)
* **Festes Datum**
  
    Ein fester Datumsbereich zum Skalieren der Rolle.
  
    ![CLouddienst, der mit einem festen Datum skaliert wird](./media/cloud-services-how-to-scale-portal/select-fixed.png)

Nachdem Sie das Profil konfiguriert haben, wählen Sie unten auf dem Profilblatt die Schaltfläche **OK** aus.

#### <a name="rule"></a>Regel
Regeln werden einem Profil hinzugefügt und stellen eine Bedingung dar, die die Skalierung auslöst. 

Der Trigger der Regel basiert auf einer Metrik des Clouddiensts (CPU-Auslastung, Datenträgeraktivität oder Netzwerkaktivität), der Sie einen bedingten Wert hinzufügen können. Darüber hinaus kann der Trigger auch auf einer Nachrichtenwarteschlange oder der Metrik einer anderen mit Ihrem Abonnement verknüpften Azure-Ressource basieren.

![](./media/cloud-services-how-to-scale-portal/rule-settings.png)

Nachdem Sie die Regel konfiguriert haben, wählen Sie unten auf dem Regelblatt die Schaltfläche **OK** aus.

## <a name="back-to-manual-scale"></a>Zurück zum manuellen Skalieren
Navigieren Sie zu [Skalierungseinstellungen](#where-scale-is-located), und legen Sie die Option **Skalieren nach** auf **Eine Anzahl von Instanzen, die ich manuell festlege** fest.

![Clouddienst-Skalierungseinstellungen mit Profil und Regel](./media/cloud-services-how-to-scale-portal/manual-basics.png)

Durch diese Einstellung wird die automatische Skalierung von der Rolle entfernt. Danach können Sie die Anzahl der Instanzen direkt angeben. 

1. Die Skalierungsoption (manuell oder automatisch).
2. Eine Schieberegler für die Rolleninstanz, um die Instanzen festzulegen, auf die skaliert werden soll.
3. Instanzen der Rolle, auf die skaliert werden soll.

Nachdem Sie die Skalierungseinstellungen konfiguriert haben, wählen Sie oben das Symbol **Speichern** aus.



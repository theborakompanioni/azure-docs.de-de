---
title: "Verwalten von Access Control-Datensätzen für das StorSimple Virtual Array | Microsoft Docs"
description: "Es wird beschrieben, wie Sie mit Access Control-Datensätzen (Access Control Records, ACRs) bestimmen, welche Hosts Verbindungen mit einem Volume auf dem StorSimple Virtual Array herstellen können."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 11252938-5b97-4178-8c37-f58eaa3d00b1
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/03/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 037da7a5647f5b3d2b0cf1364fab050406be3b62


---
# <a name="use-the-storsimple-manager-service-to-manage-access-control-records-for-the-storsimple-virtual-array"></a>Verwenden des StorSimple Manager-Diensts zum Verwalten von Access Control-Datensätzen für das StorSimple Virtual Array
## <a name="overview"></a>Übersicht
Mit Access Control-Datensätzen (Access Control Records, ACRs) können Sie angeben, welche Hosts eine Verbindung mit einem Volume im StorSimple Virtual Array (auch als lokales virtuelles StorSimple-Gerät bezeichnet) herstellen können. ACRs sind auf ein bestimmtes Volume festgelegt. Sie enthalten die qualifizierten iSCSI-Namen (IQN) der Hosts. Wenn ein Host versucht, eine Verbindung mit einem Volume herzustellen, überprüft das Gerät den ACR zu diesem Volume anhand des IQN-Namens. Bei einer Übereinstimmung wird eine Verbindung hergestellt. Im Abschnitt mit den **Zugriffssteuerungsdatensätzen** auf der Seite **Konfigurieren** werden alle Zugriffssteuerungsdatensätze zu den IQNs der Hosts angezeigt.

In diesem Tutorial werden die folgenden häufig durchgeführten ACR-bezogenen Aufgaben erläutert:

* Abrufen des IQN
* Hinzufügen von Zugriffssteuerungsdatensätzen 
* Bearbeiten von Zugriffssteuerungsdatensätzen 
* Löschen von Zugriffssteuerungsdatensätzen 

> [!IMPORTANT]
> * Achten Sie beim Zuweisen eines ACR zu einem Volume darauf, dass dabei nicht gleichzeitig von mehreren nicht gruppierten Hosts auf das Volume zugegriffen wird, da dadurch das Volume beschädigt werden könnte. 
> * Stellen Sie beim Löschen eines ACR von einem Volume sicher, dass der entsprechende Host nicht auf das Volume zugreift, da das Löschen zu einer Unterbrechung des Lese-/ Schreibzugriff führen kann.
> 
> 

## <a name="get-the-iqn"></a>Abrufen des IQN
Führen Sie die folgenden Schritte aus, um den IQN eines Windows-Hosts abzurufen, auf dem Windows Server 2012 ausgeführt wird.

[!INCLUDE [storsimple-get-iqn](../../includes/storsimple-get-iqn.md)]

## <a name="add-an-acr"></a>Hinzufügen eines ACR
ACRs werden auf der Seite **Konfiguration** des StorSimple Manager-Diensts hinzugefügt. In der Regel ordnen Sie einem Volume einen ACR zu.

Informationen zum Zuordnen eines ACR zu einem Volume finden Sie unter [Hinzufügen eines Volumes](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume).

> [!IMPORTANT]
> Achten Sie beim Zuweisen eines ACR zu einem Volume darauf, dass dabei nicht gleichzeitig von mehreren nicht gruppierten Hosts auf das Volume zugegriffen wird, da dadurch das Volume beschädigt werden könnte.
> 
> 

Führen Sie die folgenden Schritte aus, um einen ACR hinzuzufügen:

#### <a name="to-add-an-acr"></a>So fügen Sie einen ACR hinzu
1. Wählen Sie auf der Startseite für Dienste Ihren Dienst aus, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfiguration** .
   
    ![Registerkarte „Konfiguration“](./media/storsimple-ova-manage-acrs/acr1.png)
2. Geben Sie in der Tabelle unter **Zugriffssteuerungsdatensätze** einen **Namen** für Ihren ACR ein.
3. Geben Sie unter **iSCSI-Initiatorname**den IQN des Windows-Hosts an. 
4. Klicken Sie unten auf der Seite auf **Speichern** , um den neu erstellten ACR zu speichern. Die folgende Bestätigungsmeldung wird angezeigt.
   
    ![Bestätigungsmeldung](./media/storsimple-ova-manage-acrs/acr2.png)
5. Klicken Sie auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-ova-manage-acrs/check-icon.png). Die tabellarische Auflistung wird mit dem hinzugefügten Eintrag aktualisiert.

## <a name="edit-an-acr"></a>Bearbeiten eines ACR
Navigieren Sie im klassischen Azure-Portal zur Seite **Konfiguration** , um ACRs zu bearbeiten. 

> [!NOTE]
> Sie sollten nur die ACRs ändern, die derzeit nicht verwendet werden. Zum Bearbeiten eines ACR, der einem momentan verwendeten Volume zugeordnet ist, sollten Sie zunächst das Volume offline schalten.
> 
> 

Führen Sie die folgenden Schritte aus, um einen ACR zu bearbeiten:

#### <a name="to-edit-an-acr"></a>So bearbeiten Sie einen ACR
1. Wählen Sie auf der Startseite für Dienste Ihren Dienst aus, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfiguration** .
2. Zeigen Sie in der tabellarischen Auflistung der Zugriffssteuerungsdatensätze auf den zu ändernden ACR.
3. Geben Sie einen neuen Namen und/oder IQN für den ACR an.
4. Klicken Sie unten auf der Seite auf **Speichern** , um den geänderten ACR zu speichern. Es wird eine Bestätigungsmeldung angezeigt. 
5. Klicken Sie auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-ova-manage-acrs/check-icon.png). Die tabellarische Auflistung wird den Änderungen entsprechend aktualisiert.

## <a name="delete-an-access-control-record"></a>Löschen von Zugriffssteuerungsdatensätzen
Navigieren Sie im klassischen Azure-Portal zur Seite **Konfiguration** , um ACRs zu löschen. 

> [!NOTE]
> * Sie sollten nur die ACRs löschen, die derzeit nicht verwendet werden. Zum Löschen eines ACR, der einem momentan verwendeten Volume zugeordnet ist, sollten Sie zunächst das Volume offline schalten.
> * Stellen Sie beim Löschen eines ACR von einem Volume sicher, dass der entsprechende Host nicht auf das Volume zugreift, da das Löschen zu einer Unterbrechung des Lese-/ Schreibzugriff führen kann.
> 
> 

Führen Sie die folgenden Schritte aus, um einen Zugriffssteuerungsdatensatz zu löschen.

#### <a name="to-delete-an-access-control-record"></a>So löschen Sie einen Zugriffssteuerungsdatensatz
1. Wählen Sie auf der Startseite für Dienste Ihren Dienst aus, doppelklicken Sie auf den Namen, und klicken Sie dann auf die Registerkarte **Konfiguration** .
2. Zeigen Sie in der tabellarischen Auflistung der Zugriffssteuerungsdatensätze (ACRs) auf den zu löschenden ACR.
3. Ein Löschsymbol (**x**) wird in der äußersten rechten Spalte für den ausgewählten ACR angezeigt. Klicken Sie auf das Symbol **x** , um den ACR zu löschen. Die folgende Bestätigungsmeldung wird angezeigt.
   
    ![Bestätigungsmeldung](./media/storsimple-ova-manage-acrs/acr3.png)
4. Klicken Sie auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-ova-manage-acrs/check-icon.png). Die tabellarische Auflistung wird mit dem gelöschten Eintrag aktualisiert.

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie sich die weiteren Informationen zum [Hinzufügen von Volumes und Konfigurieren von ACRs](storsimple-ova-deploy3-iscsi-setup.md#step-3-add-a-volume)durch.




<!--HONumber=Nov16_HO3-->



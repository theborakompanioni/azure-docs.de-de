---
title: StorSimple Snapshot Manager-Sicherungsrichtlinien | Microsoft Docs
description: Beschreibt, wie das StorSimple Snapshot Manager-MMC-Snap-in zum Erstellen und Verwalten von Sicherungsrichtlinien verwendet wird, die geplante Sicherungen steuern.
services: storsimple
documentationcenter: NA
author: SharS
manager: carmonm
editor: 
ms.assetid: 04415d0b-42f0-4737-8afa-257fb2dbe5d0
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 05/12/2016
ms.author: v-sharos
translationtype: Human Translation
ms.sourcegitcommit: 9cf39b922e51dd60b5b2c51cb7030436c60232a5
ms.openlocfilehash: 88fc29b81c9502374a25cb34175ad1ee7c25423f


---
# <a name="use-storsimple-snapshot-manager-to-create-and-manage-backup-policies"></a>Verwenden des StorSimple Snapshot Managers zum Erstellen und Verwalten von Sicherungsrichtlinien
## <a name="overview"></a>Übersicht
Eine Sicherungsrichtlinie erstellt einen Zeitplan für die Sicherung von Volumedaten lokal oder in der Cloud. Wenn Sie eine Sicherungsrichtlinie erstellen, können Sie auch eine Aufbewahrungsrichtlinie angeben. (Sie können bis zu 64 Momentaufnahmen beibehalten.) Weitere Informationen über Sicherungsrichtlinien finden Sie im Artikel [StorSimple 8000-Serie: eine Hybridcloud-Speicherlösung](storsimple-overview.md) unter [Sicherungstypen](storsimple-what-is-snapshot-manager.md#backup-types-and-backup-policies).

In diesem Tutorial werden folgende Punkte erläutert:

* Erstellen einer Sicherungsrichtlinie
* Bearbeiten einer Sicherungsrichtlinie
* Löschen einer Sicherungsrichtlinie

## <a name="create-a-backup-policy"></a>Erstellen einer Sicherungsrichtlinie
Verwenden Sie das folgende Verfahren, um eine neue Sicherungsrichtlinie zu erstellen.

#### <a name="to-create-a-backup-policy"></a>So erstellen Sie eine Sicherungsrichtlinie
1. Klicken Sie auf das Desktopsymbol, um den StorSimple Snapshot Manager zu starten.
2. Klicken Sie im Fenster **Bereich** mit der rechten Maustaste auf **Sicherungsrichtlinien**, und klicken Sie dann auf **Sicherungsrichtlinie erstellen**.

    ![Erstellen einer Sicherungsrichtlinie](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_BU_policy.png)

    Das Dialogfeld **Richtlinie erstellen** wird angezeigt.

    ![Erstellen einer Richtlinie – Registerkarte "Allgemein"](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_general.png)
3. Vervollständigen Sie auf der Registerkarte **Allgemein** die folgenden Informationen:

   1. Geben Sie im Textfeld **Name** einen Namen für die Richtlinie ein.
   2. Geben Sie im Textfeld **Volumegruppe** den Namen der Volumegruppe ein, die der Richtlinie zugeordnet ist.
   3. Wählen Sie entweder **Lokale Momentaufnahme** oder **Cloudmomentaufnahme**.
   4. Wählen Sie die Anzahl der Momentaufnahmen, die beibehalten werden sollen. Wenn Sie die Option **Alle**auswählen, werden 64 Momentaufnahmen (die maximale Anzahl) beibehalten.
4. Klicken Sie auf die Registerkarte **Zeitplan** .

    ![Erstellen einer Richtlinie – Registerkarte "Zeitplan"](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Create_policy_schedule.png)
5. Vervollständigen Sie auf der Registerkarte **Zeitplan** die folgenden Informationen:

   1. Klicken Sie auf das Kontrollkästchen **Aktivieren** , um die nächste Sicherung zu planen.
   2. Wählen Sie unter **Einstellungen** entweder **Einmalig**, **Täglich**, **Wöchentlich** oder **Monatlich** aus.
   3. Klicken Sie im Textfeld **Start** auf das Kalendersymbol, und wählen Sie ein Startdatum.
   4. Unter **Erweiterte Einstellungen**können Sie optional Wiederholungszeitpläne und ein Enddatum festlegen.
   5. Klicken Sie auf **OK**.

Nachdem Sie eine Sicherungsrichtlinie erstellt haben, werden die folgenden Informationen im Bereich **Ergebnisse** angezeigt:

* **Name** – der Name der Sicherungsrichtlinie.
* **Typ** – lokale oder cloudbasierte Momentaufnahme.
* **Volumegruppe** – die der Richtlinie zugeordnete Volumegruppe.
* **Aufbewahrung** – die Anzahl der beibehaltenen Momentaufnahmen, maximal 64.
* **Erstellt** – das Datum, an dem diese Richtlinie erstellt wurde.
* **Aktiviert**: Gibt an, ob die Richtlinie derzeit angewendet wird. Bei **True** ist die Richtline aktiviert, bei **False** ist die Richtlinie nicht aktiviert.

## <a name="edit-a-backup-policy"></a>Bearbeiten einer Sicherungsrichtlinie
Verwenden Sie das folgende Verfahren, um eine vorhandene Sicherungsrichtlinie bearbeiten.

#### <a name="to-edit-a-backup-policy"></a>So bearbeiten Sie eine Sicherungsrichtlinie
1. Klicken Sie auf das Desktopsymbol, um den StorSimple Snapshot Manager zu starten.
2. Klicken Sie im Fenster **Bereich** auf den Knoten **Sicherungsrichtlinien**. Alle Sicherungsrichtlinien werden im Bereich **Ergebnisse** angezeigt.
3. Klicken Sie mit der rechten Maustaste auf die Richtlinie, die Sie bearbeiten möchten, und klicken Sie dann auf **Bearbeiten**.

    ![Bearbeiten einer Sicherungsrichtlinie](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Edit_BU_policy.png)
4. Wenn das Fenster **Richtlinie erstellen** angezeigt wird, geben Sie Ihre Änderungen ein, und klicken Sie dann auf **OK**.

## <a name="delete-a-backup-policy"></a>Löschen einer Sicherungsrichtlinie
Verwenden Sie das folgende Verfahren, um eine Sicherungsrichtlinie zu löschen.

#### <a name="to-delete-a-backup-policy"></a>So löschen Sie eine Sicherungsrichtlinie
1. Klicken Sie auf das Desktopsymbol, um den StorSimple Snapshot Manager zu starten.
2. Klicken Sie im Fenster **Bereich** auf den Knoten **Sicherungsrichtlinien**. Alle Sicherungsrichtlinien werden im Bereich **Ergebnisse** angezeigt.
3. Klicken Sie mit der rechten Maustaste auf die Sicherungsrichtlinie, die Sie löschen möchten, und klicken Sie dann auf **Löschen**.
4. Wenn die Bestätigungsmeldung angezeigt wird, klicken Sie auf **Ja**.

    ![Bestätigung der gelöschten Sicherungsrichtlinie](./media/storsimple-snapshot-manager-manage-backup-policies/HCS_SSM_Delete_BU_policy.png)

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum [Verwenden von StorSimple Snapshot Manager zum Verwalten der StorSimple-Lösung](storsimple-snapshot-manager-admin.md).
* Weitere Informationen zum [Verwenden von StorSimple Snapshot Manager zum Anzeigen und Verwalten von Sicherungsaufträgen](storsimple-snapshot-manager-manage-backup-jobs.md).



<!--HONumber=Nov16_HO3-->



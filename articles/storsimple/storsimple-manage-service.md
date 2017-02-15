---
title: Bereitstellen des StorSimple Manager-Diensts | Microsoft Docs
description: "Erläutert das Erstellen und Löschen des StorSimple Manager-Diensts im klassischen Azure-Portal und beschreibt die Verwaltung des Dienstregistrierungsschlüssels."
services: storsimple
documentationcenter: 
author: SharS
manager: carmonm
editor: 
ms.assetid: bc1d5650-275c-42ed-bc77-cdb596f85943
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/24/2016
ms.author: v-sharos
translationtype: Human Translation
ms.sourcegitcommit: 9cf39b922e51dd60b5b2c51cb7030436c60232a5
ms.openlocfilehash: 51c414bacb825dd51bcd76a06e1f9e0f0c001b93


---
# <a name="deploy-the-storsimple-manager-service"></a>Bereitstellen des StorSimple Manager-Diensts
## <a name="overview"></a>Übersicht
Der StorSimple Manager-Dienst wird in Microsoft Azure ausgeführt. Er stellt Verbindungen mit mehreren StorSimple-Geräten her. Nachdem Sie den Dienst erstellt haben, können Sie ihn zum Verwalten der Geräte über das in einem Browser ausgeführte klassische Microsoft Azure-Portal verwenden. So können Sie alle Geräte, die mit den StorSimple Manager-Dienst verbunden sind, von einem zentralen Standort aus überwachen und damit den Verwaltungsaufwand minimieren.

Auf der Startseite von StorSimple Manager werden alle StorSimple Manager-Dienste aufgeführt, mit denen Sie Ihre StorSimple-Speichergeräte verwalten können. Für jeden StorSimple Manager-Dienst werden die folgende Informationen auf der Seite von StorSimple Manager angezeigt:

* **Name:** der Name, der dem StorSimple Manager-Dienst bei der Erstellung zugewiesen wurde. Der Dienstname kann nicht mehr geändert werden, nachdem der Dienst erstellt wurde.
* **Status**: der Status des Diensts mit den möglichen Werten **Aktiv**, **Wird erstellt** oder **Online**.
* **Standort:** der geografische Standort, an dem das StorSimple-Gerät bereitgestellt wird.
* **Abonnement:** das Abonnement für die Abrechnung, das mit Ihrem Dienst verbunden ist.

Folgende häufig anfallenden Aufgaben können über die StorSimple Manager-Seite durchgeführt werden:

* Erstellen von Diensten
* Löschen von Diensten
* Abrufen des Dienstregistrierungsschlüssels
* Neugenerieren des Dienstregistrierungsschlüssels

In diesem Lernprogramm wird beschrieben, wie diese Aufgaben durchgeführt werden.

## <a name="create-a-service"></a>Erstellen von Diensten
Erstellen Sie mithilfe der Option **Schnellerfassung** einen StorSimple Manager-Dienst, wenn Sie Ihr StorSimple-Gerät bereitstellen möchten. Für das Erstellen eines Diensts ist Folgendes erforderlich:

* Ein Abonnement mit einem Enterprise Agreement
* Ein aktives Microsoft Azure-Speicherkonto
* Die Abrechnungsinformationen für die Zugriffsverwaltung

Sie können beim Erstellen des Diensts auch ein Standardspeicherkonto generieren.

Mit einem Dienst können mehrere Geräte verwaltet werden. Ein Gerät kann jedoch nicht mehrere Dienste umfassen. Große Unternehmen können mit mehreren Dienstinstanzen mit verschiedenen Abonnements, Organisationen oder sogar Bereitstellungsstandorten arbeiten. Beachten Sie, dass Sie separate Instanzen des StorSimple Manager-Diensts benötigen, um StorSimple-Geräte der Serie 8000 und StorSimple Virtual Arrays zu verwalten.

Führen Sie die folgenden Schritte aus, um einen Dienst zu erstellen.

[!INCLUDE [storsimple-create-new-service](../../includes/storsimple-create-new-service.md)]

## <a name="delete-a-service"></a>Löschen von Diensten
Bevor Sie einen Dienst löschen, stellen Sie sicher, dass er von keinen verbundenen Geräten verwendet wird. Wenn der Dienst verwendet wird, deaktivieren Sie die verbundenen Geräte. Der deaktivierte Vorgang trennt die Verbindung zwischen dem Gerät und dem Dienst, behält aber die Gerätedaten in der Cloud bei.

[AZURE.IMPORTANT] Das Löschen eines Diensts kann nicht rückgängig gemacht werden. Jedes Gerät, das den Dienst verwendet hat, muss auf die Werkseinstellungen zurückgesetzt werden, bevor es mit einem anderen Dienst verwendet werden kann. In diesem Szenario gehen sowohl die lokalen Daten auf dem Gerät als auch die Konfiguration verloren.

Führen Sie die folgenden Schritte aus, um einen Dienst zu löschen.

### <a name="to-delete-a-service"></a>So löschen Sie einen Dienst
1. Klicken Sie auf der Seite des **StorSimple Manager-Diensts** auf den zu löschenden Dienst.
2. Klicken Sie unten auf der Seite auf **Löschen** .
3. Klicken Sie in der Bestätigungsbenachrichtigung auf **Ja** . Es dauert einige Minuten, bis der Dienst gelöscht wird.

## <a name="get-the-service-registration-key"></a>Abrufen des Dienstregistrierungsschlüssels
Nachdem Sie einen Dienst erstellt haben, müssen Sie Ihr StorSimple-Gerät bei dem Dienst registrieren. Um Ihr erstes StorSimple-Gerät zu registrieren, benötigen Sie den Dienstregistrierungsschlüssel. Um zusätzliche Geräte bei einem vorhandenen StorSimple-Dienst zu registrieren, benötigen Sie den Registrierungsschlüssel und den Verschlüsselungsschlüssel für Dienstdaten (die während der Registrierung auf dem ersten Gerät generiert werden). Weitere Informationen zum Verschlüsselungsschlüssel für Dienstdaten finden Sie unter [StorSimple-Sicherheit](storsimple-security.md). Sie rufen den Registrierungsschlüssel ab, indem Sie auf der Seite **Registrierungsschlüssel** on the **Registrierungsschlüssel** zugreifen.

Führen Sie die folgenden Schritte durch, um den Dienstregistrierungsschlüssel abzurufen.

[!INCLUDE [storsimple-get-service-registration-key](../../includes/storsimple-get-service-registration-key.md)]

Bewahren Sie den Dienstregistrierungsschlüssel an einem sicheren Ort auf. Sie benötigen diesen Schlüssel sowie den Verschlüsselungsschlüssel für Dienstdaten, um zusätzliche Geräte bei diesem Dienst zu registrieren. Nach dem Abrufen des Dienstregistrierungsschlüssels müssen Sie das Gerät mithilfe von Windows PowerShell für StorSimple konfigurieren.

Einzelheiten zum Verwenden des Registrierungsschlüssels finden Sie unter [Schritt 3: Konfigurieren und Registrieren des Geräts über Windows PowerShell für StorSimple](storsimple-deployment-walkthrough.md#step-3-configure-and-register-the-device-through-windows-powershell-for-storsimple).

## <a name="regenerate-the-service-registration-key"></a>Neugenerieren des Dienstregistrierungsschlüssels
Sie müssen den Dienstregistrierungsschlüssel neu generieren, wenn Sie die Schlüsselrotation durchführen müssen oder wenn sich die Liste der Dienstadministratoren geändert hat. Wenn Sie den Schlüssel neu generieren, wird der neue Schlüssel nur für die Registrierung nachfolgender Geräte verwendet. Bereits registrierte Geräte sind von diesem Vorgang nicht betroffen.

Führen Sie die folgenden Schritte durch, um den Dienstregistrierungsschlüssel neu zu generieren.

### <a name="to-regenerate-the-service-registration-key"></a>So generieren Sie den Dienstregistrierungsschlüssel neu
1. Klicken Sie auf der Seite des **StorSimple Manager-Diensts** auf **Registrierungsschlüssel**.
2. Klicken Sie im Dialogfeld **Dienstregistrierungsschlüssel** auf **Neu generieren**.
3. Es wird eine Bestätigungsmeldung angezeigt. Klicken Sie auf **OK** , um die Neugenerierung fortzusetzen.
4. Ein neuer Dienstregistrierungsschlüssel wird angezeigt.
5. Kopieren Sie diesen Schlüssel, und speichern Sie ihn für die Registrierung neuer Geräte bei diesem Dienst.
6. Klicken Sie auf das Häkchensymbol  ![Häkchensymbol](./media/storsimple-manage-service/HCS_CheckIcon.png) , um das Dialogfeld zu schließen.

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zum [StorSimple-Bereitstellungsprozess](storsimple-deployment-walkthrough.md)
* Weitere Informationen über das [Verwalten des StorSimple-Speicherkontos](storsimple-manage-storage-accounts.md)
* Weitere Informationen über das [Verwalten Ihres StorSimple-Geräts mithilfe des StorSimple Manager-Diensts](storsimple-manager-service-administration.md).



<!--HONumber=Nov16_HO3-->



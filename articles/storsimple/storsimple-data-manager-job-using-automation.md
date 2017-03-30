---
title: "Verwenden von Azure Automation, um einen Auftrag auszulösen | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Automation verwenden, um StorSimple Data Manager-Aufträge auszulösen (private Vorschau)."
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
translationtype: Human Translation
ms.sourcegitcommit: 0d8472cb3b0d891d2b184621d62830d1ccd5e2e7
ms.openlocfilehash: 9691408bcd80afb6eba534f26749b76dd3bfe315
ms.lasthandoff: 03/21/2017

---

# <a name="use-azure-automation-to-trigger-a-job-private-preview"></a>Verwenden von Azure Automation, um einen Auftrag auszulösen (private Vorschau)

In diesem Artikel wird beschrieben, wie Azure Automation verwendet werden kann, um einen StorSimple Data Manager-Auftrag auszulösen.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie beginnen stellen Sie sicher, dass Sie über Folgendes verfügen:

*    Azure PowerShell ist installiert. [Azure PowerShell herunterladen](https://azure.microsoft.com/documentation/articles/powershell-install-configure/).
*    Konfigurationseinstellungen für das Initiieren des Datentransformationsauftrags (in diesem Dokument finden Sie Anweisungen, wie Sie diese Einstellungen erhalten).
*    Eine Auftragsdefinition, die korrekt in einer Ressource für Hybriddaten innerhalb einer Ressourcengruppe konfiguriert wurde.
*    Laden Sie die [zip](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/raw/master/Azure%20Automation%20For%20Data%20Manager/DataTransformationApp.zip)-Datei `DataTransformationApp.zip` aus dem GitHub-Repository herunter.
*    Laden Sie die [Skript](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Get-ConfigurationParams.ps1)-Datei `Get-ConfigurationParams.ps1` aus dem GitHub-Repository herunter.
*    Laden Sie die [Skript](https://github.com/Azure-Samples/storsimple-dotnet-data-manager-get-started/blob/master/Azure%20Automation%20For%20Data%20Manager/Trigger-DataTransformation-Job.ps1)-Datei `Trigger-DataTransformation-Job.ps1` aus dem GitHub-Repository herunter.

## <a name="step-by-step"></a>Schrittweise Anleitung

### <a name="get-azure-active-directory-permissions-for-the-automation-job-to-run-the-job-definition"></a>Abrufen von Azure Active Directory-Berechtigungen für den Automation-Auftrag, um die Auftragsdefinition auszuführen

1. Führen Sie die folgenden Schritte aus, um die Konfigurationsparameter für Active Directory abzurufen:

    1. Öffnen Sie Windows PowerShell auf Ihrem lokalen Computer. Vergewissern Sie sich, dass [Azure PowerShell](https://azure.microsoft.com/downloads/) installiert ist.
    1. Führen Sie das Skript `Get-ConfigurationParams.ps1` aus (in dem Ordner, in den Sie es heruntergeladen haben). Geben Sie den folgenden Befehl im PowerShell-Fenster ein:

        ```
        .\Get-ConfigurationParams.ps1 -SubscriptionName "AzureSubscriptionName" -ActiveDirectoryKey "AnyRandomPassword" -AppName "ApplicationName"
         ```

        „ActiveDirectoryKey“ ist ein Kennwort, das Sie später verwenden. Geben Sie ein entsprechendes Kennwort ein. „AppName“ kann eine beliebige Zeichenfolge sein.

2. Dieses Skript gibt die folgenden Werte aus, die verwendet werden müssen, wenn das Automation-Runbook ausgelöst wird. Notieren Sie sich diese Werte.

    - Client-ID
    - Mandanten-ID
    - Active Directory-Schlüssel (identisch mit der Eingabe für „ActiveDirectoryKey“)
    - Abonnement-ID

### <a name="set-up-the-automation-account"></a>Einrichten des Automation-Kontos

1. Melden Sie sich bei Azure an, und öffnen Sie Ihr Automation-Konto.
2. Klicken Sie auf die Kachel **Objekte**, um die Liste mit den Objekten zu öffnen.
3. Klicken Sie auf die Kachel **Module**, um die Liste mit den Modulen zu öffnen.
4. Klicken Sie auf die Schaltfläche **+ Modul hinzufügen**. Das Blatt „Modul hinzufügen“" wird gestartet.

    ![Automation-Konto-Einstellungen](./media/storsimple-data-manager-job-using-automation/add-module1m.png)

5. Nachdem Sie die Datei `DataTransformationApp.zip` auf Ihrem lokalen Computer ausgewählt haben, klicken Sie auf **OK**, um das Modul zu importieren.

   Wenn Azure Automation ein Modul in Ihr Konto importiert, werden Metadaten zu dem Modul extrahiert. Dieser Vorgang kann einige Minuten dauern.

   ![Automation-Konto-Einstellungen](./media/storsimple-data-manager-job-using-automation/add-module2m.png)

   

6. Sie erhalten jeweils eine Benachrichtigung, wenn das Modul bereitgestellt wird und wenn der Vorgang abgeschlossen ist.  Sie können den Status auch auf der Kachel **Module** überprüfen.

### <a name="to-import-the-runbook-that-triggers-the-job-definition"></a>So importieren Sie das Runbook, das die Auftragsdefinition auslöst

1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
2. Klicken Sie auf die Kachel **Runbooks**, um die Liste mit den Runbooks zu öffnen.
3. Klicken Sie auf **+ Runbook hinzufügen** und dann auf **Vorhandenes Runbook importieren**.

   ![Importieren eines vorhandenen Runbooks](./media/storsimple-data-manager-job-using-automation/import-a-runbook.png)

4. Klicken Sie auf **Runbook-Datei**, und wählen Sie die zu importierende Datei `Trigger-DataTransformation-Job.ps1` aus.
5. Klicken Sie auf **Erstellen**, um das Modul zu importieren. Das neue Runbook wird in der Liste der Runbooks für das Automation-Konto angezeigt.
7. Klicken Sie auf das Runbook **Trigger-DataTransformation-Job**, und klicken Sie dann auf **Bearbeiten**.
8. Klicken Sie auf **Veröffentlichen**, und klicken Sie dann auf **Ja**, wenn Sie zur Bestätigung aufgefordert werden.


### <a name="to-run-the-runbook"></a>So führen Sie das Runbook aus
1. Öffnen Sie im Azure-Portal Ihr Automation-Konto.
2. Klicken Sie auf die Kachel **Runbooks** , um die Liste mit den Runbooks zu öffnen.
3. Klicken Sie auf **Trigger-DataTransformation-Job**.
4. Klicken Sie auf **Starten** , um das Runbook zu starten.

   ![Runbook starten](./media/storsimple-data-manager-job-using-automation/run-runbook1m.png)

5. Geben Sie auf dem Blatt **Runbook starten** alle Parameter ein. Klicken Sie auf **OK**, um den Datentransformationsauftrag zu senden.

   ![Runbook starten](./media/storsimple-data-manager-job-using-automation/run-runbook2m.png)


## <a name="next-steps"></a>Nächste Schritte

[Use StorSimple Data Manager UI to transform your data (Verwenden der StorSimple Data Manager-UI zum Transformieren von Daten) ](storsimple-data-manager-ui.md).

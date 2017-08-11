---
title: Integration der Azure Automation-Quellcodeverwaltung mit GitHub Enterprise | Microsoft-Dokumentation
description: Beschreibt die Details zum Konfigurieren der Integration der Quellcodeverwaltung von Automation-Runbooks mit GitHub Enterprise.
services: automation
documentationCenter: 
authors: mgoedtel
manager: jwhit
editor: 
ms.assetid: e01d817c-7d38-421c-adf5-647a4b526eb4
ms.service: automation
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: magoedte
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: 62793dcdbbf4c83161e95d1c165d5c231245f7c6
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---

# <a name="azure-automation-scenario---automation-source-control-integration-with-github-enterprise"></a>Azure Automation-Szenario – Integration der Automation-Quellcodeverwaltung mit GitHub Enterprise

Automation unterstützt aktuell die Integration der Quellcodeverwaltung, wodurch Sie Runbooks in Ihrem Automation-Konto einem GitHub-Repository für die Quellcodeverwaltung zuordnen können.  Allerdings möchten Kunden, die [GitHub Enterprise](https://enterprise.github.com/home) zur Unterstützung ihrer DevOps-Methoden bereitgestellt haben, dieses auch verwenden, um den Lebenszyklus von zur Automatisierung von Geschäftsprozessen und Dienstverwaltungsvorgängen entwickelten Runbooks zu verwalten.  

In diesem Szenario konfigurieren Sie in Ihrem Rechenzentrum einen Windows-Computer als Hybrid Runbook Worker, auf dem Azure Resource Manager-Module und Git-Tools installiert sind.  Der Hybrid Worker-Computer verfügt über einen Klon des lokalen Git-Repositorys.  Beim Ausführen des Runbooks auf dem Hybrid Worker wird das Git-Verzeichnis synchronisiert und der Inhalt der Runbookdatei in das Automation-Konto importiert.

In diesem Artikel wird die Einrichtung dieser Konfiguration in Ihrer Azure-Automation-Umgebung beschrieben. Zunächst wird Automation mit den Sicherheitsanmeldeinformationen, den zur Unterstützung dieses Szenarios erforderlichen Runbooks und der Bereitstellung eines Hybrid Runbook Workers in Ihrem Rechenzentrum konfiguriert, um die Runbooks auszuführen, auf Ihr GitHub Enterprise-Repository zuzugreifen und Runbooks mit Ihrem Automation-Konto zu synchronisieren.  


## <a name="getting-the-scenario"></a>Abrufen des Szenarios

Dieses Szenario basiert auf zwei PowerShell-Runbooks, die Sie direkt aus dem [Runbookkatalog](automation-runbook-gallery.md) des Azure-Portals importieren oder aus dem [PowerShell-Katalog](https://www.powershellgallery.com) herunterladen können.

### <a name="runbooks"></a>Runbooks

Runbook | Beschreibung| 
--------|------------|
Export-RunAsCertificateToHybridWorker | Das Runbook exportiert ein RunAs-Zertifikat aus einem Automation-Konto in einen Hybrid Worker, damit Runbooks auf dem Worker mit Azure authentifiziert werden können, um sie in das Automation-Konto zu importieren.| 
Sync-LocalGitFolderToAutomationAccount | Das Runbook synchronisiert den lokalen Git-Ordner auf dem Hybridcomputer und importiert anschließend die Runbookdateien (*.ps1) in das Automation-Konto.|

### <a name="credentials"></a>Anmeldeinformationen

Anmeldeinformation | Beschreibung|
-----------|------------|
GitHRWCredential | Anmeldeinformationsobjekt, das Sie erstellen und das den Benutzernamen und das Kennwort für einen Benutzer mit Berechtigungen für den Hybrid Worker enthält.|

## <a name="installing-and-configuring-this-scenario"></a>Installieren und Konfigurieren dieses Szenarios

### <a name="prerequisites"></a>Voraussetzungen

1. Das Runbook „Sync-LocalGitFolderToAutomationAccount“ authentifiziert sich mit dem [ausführenden Azure-Konto](automation-sec-configure-azure-runas-account.md). 

2. Ein Microsoft OMS-Arbeitsbereich (Operations Management Suite), in dem die Azure Automation-Lösung aktiviert und konfiguriert ist, ist ebenfalls erforderlich.  Wenn Sie nicht über einen entsprechenden Arbeitsbereich verfügen, der dem zum Installieren und Konfigurieren dieses Szenarios verwendeten Automation-Konto zugeordnet ist, wird er beim Ausführen des Skripts **New-OnPremiseHybridWorker.ps1** über den Hybrid Runbook Worker erstellt und konfiguriert.        

    > [!NOTE]
    > Derzeit wird die Automation-Integration mit OMS nur für die folgenden Regionen unterstützt: **Australien, Südosten**, **USA, Osten 2**, **Asien, Südosten** und **Europa, Westen**. 

3. Ein Computer, der als dedizierter Hybrid Runbook Worker dienen kann, auf dem außerdem die GitHub-Software gehostet wird und die Runbookdateien (*runbook*.ps1) in einem Quellverzeichnis im Dateisystem für die Synchronisierung zwischen GitHub und Ihrem Automation-Konto verwaltet werden.

### <a name="import-and-publish-the-runbooks"></a>Importieren und Veröffentlichen der Runbooks

Befolgen Sie zum Importieren der Runbooks *Export-RunAsCertificateToHybridWorker* und *Sync-LocalGitFolderToAutomationAccount* aus dem Runbookkatalog Ihres Automation-Kontos im Azure-Portal die Verfahren unter [Importieren eines Runbooks aus dem Runbookkatalog](automation-runbook-gallery.md#to-import-a-runbook-from-the-runbook-gallery-with-the-azure-portal). Veröffentlichen Sie die Runbooks, nachdem sie erfolgreich in Ihr Automation-Konto importiert wurden.

### <a name="deploy-and-configure-hybrid-runbook-worker"></a>Bereitstellen und Konfigurieren von Hybrid Runbook Worker

Wenn Sie in Ihrem Rechenzentrum noch keinen Hybrid Runbook Worker bereitgestellt haben, lesen Sie die Anforderungen, und führen Sie die automatischen Installationsschritte mithilfe des Verfahrens unter [Azure Automation Hybrid Runbook Worker – Automatisieren der Installation und Konfiguration](automation-hybrid-runbook-worker.md#automated-deployment) aus.  Sobald Sie den Hybrid Worker erfolgreich auf einem Computer installiert haben, führen Sie die folgenden Schritte aus, um dessen Konfiguration zur Unterstützung dieses Szenarios fertig zu stellen.

1. Melden Sie sich auf dem Computer, der die Hybrid Runbook Worker-Rolle hostet, mit einem Konto mit lokalen Administratorrechten an, und erstellen Sie ein Verzeichnis, in dem die Git-Runbookdateien gespeichert werden.  Klonen Sie das interne Git-Repository in das Verzeichnis.
2. Wenn Sie noch kein RunAs-Konto erstellt haben oder wenn Sie ein neues RunAs-Konto für diesen Zweck erstellen möchten, navigieren Sie aus dem Azure-Portal zu den Automation-Konten, wählen Sie Ihr Automation-Konto aus, und erstellen Sie ein [Anmeldeinformationsobjekt](automation-credentials.md), das den Benutzernamen und das Kennwort für einen Benutzer mit Berechtigungen für den Hybrid Worker enthält.  
3. [Bearbeiten Sie das Runbook](automation-edit-textual-runbook.md) **Export RunAsCertificateToHybridWorker** in Ihrem Automation-Konto, und ändern Sie den Wert für die Variable *$Password* in ein sicheres Kennwort.  Nachdem Sie den Wert geändert haben, klicken Sie auf **Veröffentlichen**, um die Entwurfsversion des Runbooks veröffentlichen zu lassen. 
5. Starten Sie das Runbook **Export-RunAsCertificateToHybridWorker**, und wählen Sie im Blatt **Runbook starten** unter der Option **Laufzeiteinstellungen** die Option **Hybrid Worker** aus. Wählen Sie anschließend aus der Dropdownliste die zuvor für dieses Szenario erstellte Hybrid Worker-Gruppe aus.  

    Hierdurch wird ein Zertifikat in den Hybrid Worker exportiert, damit Runbooks auf dem Worker sich mithilfe der ausführenden Verbindung bei Azure authentifizieren können, um Azure-Ressourcen zu verwalten (speziell für dieses Szenario: um Runbooks in das Automation-Konto zu importieren).

4. Wählen Sie aus Ihrem Automation-Konto die zuvor erstellte Hybrid Worker-Gruppe aus, [geben Sie ein RunAs-Konto](automation-hrw-run-runbooks.md#runas-account) für die Hybrid Worker-Gruppe an, und wählen Sie das Anmeldeinformationsobjekt aus, das Sie soeben oder zuvor erstellt haben.  Dadurch wird sichergestellt, dass das Synchronisierungsrunbook Git-Befehle ausführen kann. 
5. Starten Sie das Runbook **Sync-LocalGitFolderToAutomationAccount**, geben Sie die folgenden erforderlichen Eingabeparameterwerte ein, und wählen Sie im Blatt **Runbook starten** unter der Option **Laufzeiteinstellungen** die Option **Hybrid Worker** aus. Wählen Sie anschließend aus der Dropdownliste die zuvor für dieses Szenario erstellte Hybrid Worker-Gruppe aus:
    * *ResourceGroup*: der Name der Ressourcengruppe, die Ihrem Automation-Konto zugeordnet ist
    * *AutomationAccountName*: der Name Ihres Automation-Kontos
    * *GitPath*: der lokale Ordner oder die lokale Datei auf dem Hybrid Runbook Worker, in den bzw. in die die neuesten Änderungen per Pull von Git gespeichert werden

    Dadurch wird zunächst der lokale Git-Ordner auf dem Hybrid Worker-Computer synchronisiert, und anschließend werden die PS1-Dateien aus dem Quellverzeichnis in das Automation-Konto importiert.

    ![Runbook „Start Sync-LocalGitFolderToAutomationAccount“](media/automation-scenario-source-control-integration-with-github-ent/start-runbook-synclocalgitfoldertoautoacct.png)<br>

7. Zeigen Sie eine Auftragszusammenfassung für das Runbook an, indem Sie in Ihrem Automation-Konto zuerst auf dem Blatt **Runbooks** das Runbook und dann die Kachel **Aufträge** auswählen.  Vergewissern Sie sich, dass der Vorgang erfolgreich abgeschlossen wurde, indem Sie die Kachel **Alle Protokolle** auswählen und den detaillierten Protokolldatenstrom überprüfen.  

## <a name="next-steps"></a>Nächste Schritte

-  Weitere Informationen zu den verschiedenen Runbooktypen sowie zu ihren Vorteilen und Einschränkungen finden Sie unter [Azure Automation-Runbooktypen](automation-runbook-types.md)
-  Weitere Informationen zur PowerShell-Skriptunterstützung finden Sie unter [Native PowerShell Script Support in Azure Automation (Native PowerShell-Skriptunterstützung in Azure Automation)](https://azure.microsoft.com/blog/announcing-powershell-script-support-azure-automation-2/)


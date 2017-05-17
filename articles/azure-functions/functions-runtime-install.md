---
title: Azure Functions Runtime-Installation | Microsoft-Dokumentation
description: Installieren von Azure Functions Runtime
services: functions
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 05/08/2017
ms.author: anwestg
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 70740f0caf31d34f354b1d13558d9f960d7dfc4f
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---

# <a name="install-the-azure-functions-runtime-preview"></a>Installieren von Azure Functions Runtime Preview

Wenn Sie Azure Functions Runtime Preview installieren möchten, müssen Sie diese Schritte ausführen:

1. Stellen Sie sicher, dass Ihr Computer die Mindestanforderungen erfüllt.
1. Laden Sie das [Installationsprogramm für Azure Functions Runtime Preview](https://aka.ms/azafr) herunter. 
1. Installieren Sie Azure Functions Runtime Preview.
1. Schließen Sie die Konfiguration von Azure Functions Runtime Preview ab.

## <a name="prerequisites"></a>Voraussetzungen

Bevor Sie Azure Functions Runtime Preview installieren, benötigen Sie Folgendes:

1. Einen Computer unter Microsoft Windows Server 2016 oder Microsoft Windows 10 Creators Update (Professional oder Enterprise Edition).
1. Eine SQL Server-Instanz, die in Ihrem Netzwerk ausgeführt wird.  Sie müssen mindestens SQL Server Express verwenden.

## <a name="install-the-azure-functions-runtime-preview"></a>Installieren von Azure Functions Runtime Preview

Das Installationsprogramm für Azure Functions Runtime Preview führt Sie durch die Installation der Verwaltungs- und Workerrollen von Azure Functions Runtime Preview.  Es ist möglich, die Verwaltungs- und Workerrolle auf einem Computer zu installieren.  Wenn Sie allerdings weitere Funktionen hinzufügen, müssen Sie weitere Workerrollen auf zusätzlichen Computern bereitstellen, um die Funktionen auf mehrere Worker skalieren zu können.

## <a name="install-the-management-and-worker-role-on-the-same-machine"></a>Installieren der Verwaltungs- und Workerrolle auf einem Computer

1. Führen Sie das Installationsprogramm für Azure Functions Runtime Preview aus.

    ![Installationsprogramm für Azure Functions Runtime Preview][1]

1. Klicken Sie auf **Weiter**, um auf die nächste Seite des Installationsprogramms zu gelangen.
1. Nachdem Sie die **Lizenzbedingungen** gelesen haben, **aktivieren Sie das Kontrollkästchen** zum Akzeptieren der Lizenzbedingungen, und klicken Sie auf **Weiter**, um fortzufahren.
1. Wählen Sie jetzt die Rollen aus, die Sie auf diesem Computer installieren möchten: **Functions-Verwaltungsrolle** und/oder **Functions-Workerrolle**. Klicken Sie dann auf **Weiter**.

    ![Installationsprogramm für Azure Functions Runtime Preview – Rollenauswahl][3]

    > [!NOTE]
    > Sie können die **Functions-Workerrolle** auf vielen anderen Computern installieren. Befolgen Sie dazu diese Anweisungen, und wählen Sie nur **Functions-Workerrolle** im Installationsprogramm aus.

1. Klicken Sie auf **Weiter**, damit das **Installationsprogramm für Azure Functions Runtime** die Installation auf Ihrem Computer durchführt.
1. Nach Abschluss startet das Installationsprogramm das **Konfigurationstool für Azure Functions Runtime**.

    ![Abschluss des Installationsprogramms für Azure Functions Runtime Preview][5]

    > [!NOTE]
    > Bei einer Installation unter **Windows 10** und ohne vorherige Aktivierung des Features **Container** fordert das Installationsprogramm für **Azure Functions Runtime** Sie auf, den Computer zum Abschließen der Installation neu starten.

## <a name="configure-the-azure-functions-runtime"></a>Konfigurieren von Azure Functions Runtime

Zum Abschließen der Azure Functions Runtime-Installation müssen Sie die Konfiguration abschließen.

1. Das **Konfigurationstool für Azure Functions Runtime** zeigt an, welche Rollen auf dem Computer installiert sind.

    ![Konfigurationstool für Azure Functions Runtime Preview][6]

1. Klicken Sie auf die Registerkarte **Datenbank**, geben Sie die **Verbindungsdetails für die SQL Server-Instanz** ein, und klicken Sie auf **Übernehmen**.  Dies ist erforderlich, damit Azure Functions Runtime eine Datenbank zur Unterstützung der Runtime erstellt.
    
    ![Datenbankkonfiguration für Azure Functions Runtime Preview][7]

1. Klicken Sie auf die Registerkarte **Anmeldeinformationen**.  Auf diesem Bildschirm müssen Sie zwei neue Anmeldeinformationen für die Verwendung mit einer Dateifreigabe erstellen, die alle Ihre Azure Functions-Instanzen hostet.  Geben Sie Kombinationen aus **Benutzername und Kennwort** für den **Besitzer der Dateifreigabe** und für den **Benutzer der Dateifreigabe** an, und klicken Sie auf **Übernehmen**.

    ![Anmeldeinformationen für Azure Functions Runtime Preview][8]

1. Klicken Sie auf die Registerkarte **Dateifreigabe**.  Auf diesem Bildschirm geben Sie die Details der **Dateifreigabe** an.  Diese kann für Sie erstellt werden, oder Sie können eine vorhandene Dateifreigabe verwenden. Klicken Sie dann auf **Übernehmen**.  Wenn Sie eine neue Dateifreigabe auswählen, müssen Sie ein Verzeichnis angeben, das von Azure Functions Runtime verwendet werden soll.
    
    ![Dateifreigabe für Azure Functions Runtime Preview][9]

1. Klicken Sie auf die Registerkarte **IIS**.  Diese Registerkarte zeigt die Details der Websites in IIS an, die durch die Azure Functions Runtime-Installation erstellt werden.  Klicken Sie zum Abschließen auf **Übernehmen**.

    ![IIS für Azure Functions Runtime Preview][10]

1. Klicken Sie auf die Registerkarte **Dienste**.  Diese Registerkarte zeigt den Status der Dienste in Ihrer Azure Functions Runtime-Installation an.  Wenn nach der Erstkonfiguration der **Hostaktivierungsdienst für Azure Functions** nicht ausgeführt wird, klicken Sie auf **Dienst starten**.

    ![Abgeschlossene Konfiguration für Azure Functions Runtime Preview][11]

1. Navigieren Sie schließlich zum **Portal für Azure Functions Runtime** unter `https://<machinename>/`.

    ![Portal für Azure Functions Runtime Preview][12]


<!--Image references-->
[1]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer1.png
[2]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer2-EULA.png
[3]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer3-ChooseRoles.png
[4]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer4-Install.png
[5]: ./media/functions-runtime-install/AzureFunctionsRuntime_Installer5-InstallComplete.png
[6]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration1.png
[7]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration2_SQL.png
[8]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration3_Credentials.png
[9]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration4_Fileshare.png
[10]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration5_IIS.png
[11]: ./media/functions-runtime-install/AzureFunctionsRuntime_Configuration6_Services.png
[12]: ./media/functions-runtime-install/AzureFunctionsRuntime_Portal.png

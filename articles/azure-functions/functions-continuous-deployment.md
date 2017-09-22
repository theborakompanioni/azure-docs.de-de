---
title: "Continuous Deployment für Azure Functions | Microsoft Docs"
description: "Verwenden Sie Continuous Deployment-Funktionen von Azure App Service, um Ihre Azure-Funktionen zu veröffentlichen."
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 361daf37-598c-4703-8d78-c77dbef91643
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/25/2016
ms.author: glenga
ms.translationtype: HT
ms.sourcegitcommit: 8f9234fe1f33625685b66e1d0e0024469f54f95c
ms.openlocfilehash: 3eaa0535222a4a91c36abfff79dd4c134fa8352e
ms.contentlocale: de-de
ms.lasthandoff: 09/20/2017

---
# <a name="continuous-deployment-for-azure-functions"></a>Continuous Deployment für Azure Functions
Mit Azure Functions können Sie Ihre Funktionen-App unter Verwendung von App Service-Continuous Integration problemlos bereitstellen. Functions kann in Bitbucket, Dropbox, GitHub und Visual Studio Team Services (VSTS) integriert werden. Dies ermöglicht einen Workflow, bei dem Funktionscodeaktualisierungen, die durch einen dieser integrierten Dienste erfolgt sind, die Bereitstellung in Azure auslösen. Sollten Sie noch nicht mit Azure Functions vertraut sein, sehen Sie sich zuerst die [Übersicht zu Azure Functions](functions-overview.md)an.

Die kontinuierliche Bereitstellung ist hervorragend für Projekte geeignet, bei denen häufig zahlreiche Beiträge integriert werden. Außerdem behalten Sie die Kontrolle über den Quellcode Ihrer Funktionen. Aktuell werden folgende Bereitstellungsquellen unterstützt:

* [Bitbucket](https://bitbucket.org/)
* [Dropbox](https://www.dropbox.com/)
* Externes Repository (Git oder Mercurial)
* [Lokales Git-Repository](../app-service/app-service-deploy-local-git.md)
* [GitHub](https://github.com)
* [OneDrive](https://onedrive.live.com/)
* [Visual Studio Team Services](https://www.visualstudio.com/team-services/)

Bereitstellungen werden pro Funktionen-App konfiguriert. Nach Aktivierung von Continuous Deployment wird der Zugriff auf den Code im Portal auf *schreibgeschützt*festgelegt.

## <a name="continuous-deployment-requirements"></a>Anforderungen von Continuous Deployment

Die Bereitstellungsquelle und der darin enthaltene Funktionscode müssen vor der Einrichtung von Continuous Deployment konfiguriert worden sein. In jeder Bereitstellung einer Funktionen-App befinden sich die Funktionen jeweils in einem nach der Funktion benannten Unterverzeichnis.  

[!INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="set-up-continuous-deployment"></a>Einrichten der fortlaufenden Bereitstellung
Gehen Sie wie folgt vor, um Continuous Deployment für eine vorhandene Funktionen-App zu konfigurieren. Die folgenden Schritte beziehen sich auf die Integration mit einem GitHub-Repository. Für Visual Studio Team Services oder andere Bereitstellungsdienste gelten jedoch ähnliche Schritte.

1. Klicken Sie in der Funktionen-App im [Azure-Portal](https://portal.azure.com) auf **Plattformfeatures** und auf **Bereitstellungsoptionen**. 
   
    ![Continuous Deployment einrichten](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Klicken Sie dann auf dem Blatt **Bereitstellungen** auf **Setup**.
 
    ![Continuous Deployment einrichten](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. Klicken Sie auf dem Blatt **Bereitstellungsquelle** auf **Quelle auswählen**, geben Sie die Informationen für die gewünschte Bereitstellungsquelle ein, und klicken Sie auf **OK**.
   
    ![Bereitstellungsquelle auswählen](./media/functions-continuous-deployment/choose-deployment-source.png)

Nach dem Konfigurieren von Continuous Deployment werden alle Dateiänderungen in Ihrer Bereitstellungsquelle in die Funktionen-App kopiert, und es wird eine vollständige Websitebereitstellung ausgelöst. Wenn Dateien in der Quelle aktualisiert werden, wird die Website neu bereitgestellt.

## <a name="deployment-options"></a>Bereitstellungsoptionen

Im Anschluss finden Sie einige gängige Bereitstellungsszenarien:

- [Erstellen einer Stagingbereitstellung](#staging)
- [Verschieben vorhandener Funktionen in Continuous Deployment](#existing)

<a name="staging"></a>
### <a name="create-a-staging-deployment"></a>Erstellen einer Stagingbereitstellung

Funktionen-Apps unterstützen noch keine Bereitstellungsslots. Separate Staging- und Produktionsbereitstellungen können jedoch per Continuous Integration verwaltet werden.

Der Prozess zum Konfigurieren und Verwenden einer Stagingbereitstellung sieht im Grunde wie folgt aus:

1. Erstellen Sie in Ihrem Abonnement zwei Funktionen-Apps – eine für den Produktionscode und eine für das Staging. 

2. Erstellen Sie eine Bereitstellungsquelle, sofern noch nicht vorhanden. In diesem Beispiel wird [GitHub]verwendet.

3. Führen Sie für Ihre für die Produktionsumgebung vorgesehene Funktionen-App die vorherigen unter **Einrichten von Continuous Deployment** angegebenen Schritte aus, und legen Sie die Bereitstellungsverzweigung auf die Hauptverzweigung Ihres GitHub-Repositorys fest.
   
    ![Bereitstellungsverzweigung auswählen](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. Wiederholen Sie diesen Schritt für Ihre für die Stagingumgebung vorgesehene Funktionen-App, wählen aber stattdessen in Ihrem GitHub-Repository die Stagingverzweigung aus. Falls Ihre Bereitstellungsquelle keine Verzweigung unterstützt, verwenden Sie einen anderen Ordner.
    
5. Aktualisieren Sie Ihren Code in der Stagingverzweigung oder im Stagingordner, und vergewissern Sie sich anschließend, dass die Änderungen in der Stagingbereitstellung berücksichtigt werden.

6. Führen Sie nach dem Testen die Änderungen aus der Stagingverzweigung in der Hauptverzweigung zusammen. Dadurch wird die Bereitstellung für die Funktionen-App in der Produktionsumgebung ausgelöst. Falls Ihre Bereitstellungsquelle keine Verzweigungen unterstützt, überschreiben Sie die Dateien im Produktionsordner mit den Dateien aus dem Stagingordner.

<a name="existing"></a>
### <a name="move-existing-functions-to-continuous-deployment"></a>Verschieben vorhandener Funktionen in Continuous Deployment
Wenn Sie über Funktionen verfügen, die Sie im Portal erstellt und verwaltet haben, müssen Sie die vorhandenen Funktionscodedateien per FTP oder aus dem lokalen Git-Repository herunterladen, um Continuous Deployment wie oben beschrieben einrichten zu können. Dies ist über die App Service-Einstellungen für Ihre Funktionen-App möglich. Die heruntergeladenen Dateien können Sie an Ihre gewünschte Continuous Deployment-Quelle hochladen.

> [!NOTE]
> Nach dem Konfigurieren von Continuous Integration können die Quelldateien nicht mehr über das Functions-Portal bearbeitet werden.

- [Gewusst wie: Konfigurieren von Anmeldeinformationen für die Bereitstellung](#credentials)
- [Gewusst wie: Herunterladen von Dateien per FTP](#downftp)
- [Herunterladen von Dateien mithilfe des lokalen Git-Repositorys](#downgit)

<a name="credentials"></a>
#### <a name="how-to-configure-deployment-credentials"></a>Gewusst wie: Konfigurieren von Anmeldeinformationen für die Bereitstellung
Bevor Sie über FTP oder ein lokales Git-Repository Dateien aus Ihrer Funktionen-App herunterladen können, müssen Sie Ihre Anmeldeinformationen für den Websitezugriff angeben. Anmeldeinformationen werden auf der Ebene der Funktionen-App festgelegt. Legen Sie die Anmeldeinformationen für die Bereitstellung im Azure-Portal mit den folgenden Schritten fest:

1. Klicken Sie in der Funktionen-App im [Azure-Portal](https://portal.azure.com) auf **Plattformfeatures** und auf **Anmeldeinformationen für Bereitstellung**.
   
    ![Lokale Anmeldeinformationen für die Bereitstellung festlegen](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. Geben Sie einen Benutzernamen und ein Kennwort ein, und klicken Sie anschließend auf **Speichern**. Nun können Sie unter Verwendung dieser Anmeldeinformationen per FTP oder über das integrierte Git-Repository auf Ihre Funktionen-App zugreifen.

<a name="downftp"></a>
#### <a name="how-to-download-files-using-ftp"></a>Gewusst wie: Herunterladen von Dateien per FTP

1. Klicken Sie in der Funktionen-App im [Azure-Portal](https://portal.azure.com) auf **Plattformfeatures** und auf **Eigenschaften**. Kopieren Sie dann die Werte für **FTP/Bereitstellungsbenutzer**, **FTP-Hostname** und **FTPS-Hostname**.  

    **FTP/Bereitstellungsbenutzer** muss wie im Portal angezeigt eingegeben werden (einschließlich App-Name), um den richtigen Kontext für den FTP-Server bereitzustellen.
   
    ![Bereitstellungsinformationen abrufen](./media/functions-continuous-deployment/get-deployment-credentials.png)

2. Verwenden Sie im FTP-Client die gesammelten Verbindungsinformationen, um eine Verbindung mit Ihrer App herzustellen und die Quelldateien für Ihre Funktionen herunterzuladen.

<a name="downgit"></a>
#### <a name="how-to-download-files-using-a-local-git-repository"></a>Herunterladen von Dateien mithilfe des lokalen Git-Repositorys

1. Klicken Sie in der Funktionen-App im [Azure-Portal](https://portal.azure.com) auf **Plattformfeatures** und auf **Bereitstellungsoptionen**. 
   
    ![Continuous Deployment einrichten](./media/functions-continuous-deployment/setup-deployment.png)
 
2. Klicken Sie dann auf dem Blatt **Bereitstellungen** auf **Setup**.
 
    ![Continuous Deployment einrichten](./media/functions-continuous-deployment/setup-deployment-1.png)
   
2. Klicken Sie auf dem Blatt **Bereitstellungsquelle** auf **Lokales Git-Repository** und anschließend auf **OK**.

3. Klicken Sie in **Plattformfeatures** auf **Eigenschaften**, und notieren Sie sich den Wert der Git-URL. 
   
    ![Continuous Deployment einrichten](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. Klonen Sie das Repository auf Ihrem lokalen Computer mithilfe einer Git-fähigen Befehlszeile oder mit Ihrem bevorzugten Git-Tool. Der Befehl zum Klonen von Git sieht wie folgt aus:
   
        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. Rufen Sie Dateien aus Ihrer Funktionen-App in den Klon auf Ihrem lokalen Computer ab, wie im folgenden Beispiel zu sehen:
   
        git pull origin master
   
    Geben Sie bei eventueller Aufforderung Ihre [konfigurierten Anmeldeinformationen für die Bereitstellung](#credentials) an.  

[GitHub]: https://github.com/


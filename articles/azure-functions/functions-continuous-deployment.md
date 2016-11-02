<properties
   pageTitle="Continuous Deployment für Azure Functions | Microsoft Azure"
   description="Verwenden Sie Continuous Deployment-Funktionen von Azure App Service, um Ihre Azure-Funktionen zu veröffentlichen."
   services="functions"
   documentationCenter="na"
   authors="ggailey777"
   manager="erikre"
   editor=""
   tags=""
   />

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/25/2016"
   ms.author="glenga"/>


# <a name="continuous-deployment-for-azure-functions"></a>Continuous Deployment für Azure Functions 

Mit Azure Functions können Sie problemlos Continuous Deployment für Ihre Funktionen-App konfigurieren. Functions macht sich die Integration von Azure App Service in BitBucket, Dropbox, GitHub und Visual Studio Team Services (VSTS) zunutze und ermöglicht einen Continuous Deployment-Workflow, bei dem Azure Funktionscodeaktualisierungen abruft, wenn diese für einen der genannten Dienste veröffentlicht werden. Sollten Sie noch nicht mit Azure Functions vertraut sein, sehen Sie sich zuerst die [Übersicht zu Azure Functions](functions-overview.md)an.

Die kontinuierliche Bereitstellung ist hervorragend für Projekte geeignet, bei denen häufig zahlreiche Beiträge integriert werden. Außerdem behalten Sie die Kontrolle über den Quellcode Ihrer Funktionen. Aktuell werden folgende Bereitstellungsquellen unterstützt:

+ [Bitbucket](https://bitbucket.org/)
+ [Dropbox](https://bitbucket.org/)
+ [Lokales Git-Repository](../app-service-web/app-service-deploy-local-git.md)
+ Externes Git-Repository
+ [GitHub]
+ Externes Mercurial-Repository
+ [OneDrive](https://onedrive.live.com/)
+ Visual Studio Team Services

Bereitstellungen werden pro Funktionen-App konfiguriert. Nach Aktivierung von Continuous Deployment wird der Zugriff auf den Code im Portal auf *schreibgeschützt*festgelegt.

## <a name="continuous-deployment-requirements"></a>Anforderungen von Continuous Deployment

Die Bereitstellungsquelle und der darin enthaltene Funktionscode müssen vor der Einrichtung von Continuous Development konfiguriert worden sein. In jeder Bereitstellung einer Funktionen-App befinden sich die Funktionen jeweils in einem nach der Funktion benannten Unterverzeichnis. Diese Ordnerstruktur stellt im Grunde Ihren Websitecode dar. 

[AZURE.INCLUDE [functions-folder-structure](../../includes/functions-folder-structure.md)]

## <a name="setting-up-continuous-deployment"></a>Einrichten von Continuous Deployment

Gehen Sie wie folgt vor, um Continuous Deployment für eine vorhandene Funktionen-App zu konfigurieren:

1. Klicken Sie in Ihrer Funktionen-App im [Azure Functions-Portal](https://functions.azure.com/signin) auf **Funktionen-App-Einstellungen** > ** Continuous Integration konfigurieren** > **Setup**.

    ![Continuous Deployment einrichten](./media/functions-continuous-deployment/setup-deployment.png)
    
    ![Continuous Deployment einrichten](./media/functions-continuous-deployment/setup-deployment-1.png)
    
    Sie können auch zum Blatt „Bereitstellungen“ navigieren, indem Sie im Schnellstartbereich von Functions auf **Start from source control**(Über Quellcodeverwaltung starten) klicken.

2. Klicken Sie auf dem Blatt „Bereitstellungen“ auf **Quelle auswählen**, geben Sie die Informationen für die gewünschte Bereitstellungsquelle ein, und klicken Sie anschließend auf **OK**.

    ![Bereitstellungsquelle auswählen](./media/functions-continuous-deployment/choose-deployment-source.png)

Nach dem Konfigurieren von Continuous Deployment werden alle geänderten Dateien in Ihrer Bereitstellungsquelle in die Funktionen-App kopiert, und eine vollständige Websitebereitstellung wird ausgelöst. Wenn Dateien in der Quelle aktualisiert werden, wird die Website neu bereitgestellt.


##<a name="deployment-options"></a>Bereitstellungsoptionen

Im Anschluss finden Sie einige gängige Bereitstellungsszenarien:

+ 

###<a name="create-a-staging-deployment"></a>Erstellen einer Stagingbereitstellung

Funktionen-Apps unterstützen noch keine Bereitstellungsslots. Separate Staging- und Produktionsbereitstellungen können jedoch per Continuous Integration verwaltet werden.

Der Prozess zum Konfigurieren und Verwenden einer Stagingbereitstellung sieht im Grunde wie folgt aus:

1. Erstellen Sie in Ihrem Abonnement zwei Funktionen-Apps – eine für den Produktionscode und eine für das Staging. 

2. Erstellen Sie eine Bereitstellungsquelle, sofern noch nicht vorhanden. Wir verwenden [GitHub].
 
3. Führen Sie für Ihre für die Produktionsumgebung vorgesehene Funktionen-App die weiter oben unter **Einrichten von Continuous Deployment** angegebenen Schritte aus, und legen Sie die Bereitstellungsverzweigung auf die Hauptverzweigung des GitHub-Repositorys fest.

    ![Bereitstellungsverzweigung auswählen](./media/functions-continuous-deployment/choose-deployment-branch.png)

4. Wiederholen Sie diesen Schritt für Ihre für die Stagingumgebung vorgesehene Funktionen-App, wählen Sie diesmal aber in Ihrem GitHub-Repository die Stagingverzweigung aus. Falls Ihre Bereitstellungsquelle keine Verzweigung unterstützt, verwenden Sie einen anderen Ordner.
 
5. Aktualisieren Sie Ihren Code in der Stagingverzweigung oder im Stagingordner, und vergewissern Sie sich anschließend, dass die Änderungen in der Stagingbereitstellung berücksichtigt werden.

6. Führen Sie nach dem Testen die Änderungen aus der Stagingverzweigung in der Hauptverzweigung zusammen. Dadurch wird die Bereitstellung für die Funktionen-App in der Produktionsumgebung ausgelöst. Falls Ihre Bereitstellungsquelle keine Verzweigungen unterstützt, überschreiben Sie die Dateien im Produktionsordner mit den Dateien aus dem Stagingordner.

###<a name="move-existing-functions-to-continuous-deployment"></a>Verschieben vorhandener Funktionen in Continuous Deployment

Wenn Sie über Funktionen verfügen, die Sie im Portal erstellt und verwaltet haben, müssen Sie die vorhandenen Funktionscodedateien per FTP oder mithilfe des lokalen Git-Repositorys herunterladen, um Continuous Deployment wie oben beschrieben einrichten zu können. Dies ist über die App Service-Einstellungen für Ihre Funktionen-App möglich. Die heruntergeladenen Dateien können Sie an Ihre gewünschte Continuous Deployment-Quelle hochladen.

>[AZURE.NOTE]Nach dem Konfigurieren von Continuous Integration können die Quelldateien nicht mehr über das Functions-Portal bearbeitet werden.

####<a name="how-to:-configure-deployment-credentials"></a>Gewusst wie: Konfigurieren von Anmeldeinformationen für die Bereitstellung
Damit Sie überhaupt Dateien aus Ihrer Funktionen-App herunterladen können, müssen Sie Ihre Anmeldeinformationen für den Websitezugriff angeben. Dies ist über das Portal möglich. Anmeldeinformationen werden auf der Ebene der Funktionen-App festgelegt.

1. Klicken Sie in Ihrer Funktionen-App im [Azure Functions-Portal](https://functions.azure.com/signin) auf **Funktionen-App-Einstellungen** > **Zu App Service-Einstellungen wechseln** > **Anmeldeinformationen für Bereitstellung**.

    ![Lokale Anmeldeinformationen für die Bereitstellung festlegen](./media/functions-continuous-deployment/setup-deployment-credentials.png)

2. Geben Sie einen Benutzernamen und ein Kennwort ein, und klicken Sie anschließend auf **Speichern**. Nun können Sie unter Verwendung dieser Anmeldeinformationen per FTP oder über das integrierte Git-Repository auf Ihre Funktionen-App zugreifen.

####<a name="how-to:-download-files-using-ftp"></a>Gewusst wie: Herunterladen von Dateien per FTP

1. Klicken Sie in Ihrer Funktionen-App im [Azure Functions-Portal](https://functions.azure.com/signin) auf **Funktionen-App-Einstellungen** > **Zu App Service-Einstellungen wechseln** > **Eigenschaften**, und kopieren Sie die Werte für **FTP/Bereitstellungsbenutzer**, **FTP-Hostname** und **FTPS-Hostname**.  
**FTP/Bereitstellungsbenutzer** muss wie im Portal angezeigt eingegeben werden (einschließlich App-Name), um den richtigen Kontext für den FTP-Server bereitzustellen.

    ![Bereitstellungsinformationen abrufen](./media/functions-continuous-deployment/get-deployment-credentials.png)
    
2. Verwenden Sie im FTP-Client die gesammelten Verbindungsinformationen, um eine Verbindung mit Ihrer App herzustellen und die Quelldateien für Ihre Funktionen herunterzuladen.

####<a name="how-to:-download-files-using-the-local-git-repository"></a>Gewusst wie: Herunterladen von Dateien mithilfe des lokalen Git-Repositorys

1. Klicken Sie in Ihrer Funktionen-App im [Azure Functions-Portal](https://functions.azure.com/signin) auf **Funktionen-App-Einstellungen** > ** Continuous Integration konfigurieren** > **Setup**.

2. Klicken Sie auf dem Blatt „Bereitstellungen“ auf **Quelle auswählen** > **Lokales Git-Repository** und anschließend auf **OK**.
 
3. Klicken Sie auf **Zu App Service-Einstellungen wechseln** > **Eigenschaften**, und notieren Sie sich die Git-URL. 
    
    ![Continuous Deployment einrichten](./media/functions-continuous-deployment/get-local-git-deployment-url.png)

4. Klonen Sie das Repository auf Ihrem lokalen Computer mithilfe einer Git-fähigen Befehlszeile oder mit Ihrem bevorzugten Git-Tool. Der Befehl zum Klonen von Git sieht wie folgt aus:

        git clone https://username@my-function-app.scm.azurewebsites.net:443/my-function-app.git

5. Rufen Sie Dateien aus Ihrer Funktionen-App in den Klon auf Ihrem lokalen Computer ab, wie im folgenden Beispiel zu sehen:

        git pull origin master

    Geben Sie bei Bedarf den Benutzernamen und das Kennwort für Ihre Funktionen-App-Bereitstellung an.  


[GitHub]: https://github.com/



<!--HONumber=Oct16_HO2-->



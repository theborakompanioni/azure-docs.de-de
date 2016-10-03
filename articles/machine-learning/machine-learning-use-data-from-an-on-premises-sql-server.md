<properties
pageTitle="Verwenden von Daten aus einer lokalen SQL Server-Datenbank in Machine Learning | Azure"
description="Verwenden Sie Daten aus einer lokalen SQL Server-Datenbank, um erweiterte Analysen mit Azure Machine Learning durchzuführen."
services="machine-learning"
documentationCenter=""
authors="garyericson"
manager="jhubbard"
editor="cgronlun"/>

<tags
ms.service="machine-learning"
ms.workload="data-services"
ms.tgt_pltfrm="na"
ms.devlang="na"
ms.topic="article"
ms.date="09/16/2016"
ms.author="garye;krishnan"/>

# Durchführen der erweiterten Analyse mit Azure Machine Learning mit Daten aus einer lokalen SQL Server-Datenbank


Unternehmen, die mit lokalen Daten arbeiten, möchten häufig die Vorteile der Skalierung und Flexibilität der Cloud für ihre Machine Learning-Workloads nutzen. Sie möchten jedoch nicht ihre aktuellen Geschäftsprozesse und Workflows durch Verschieben ihrer lokalen Daten in die Cloud unterbrechen. Azure Machine Learning unterstützt jetzt das Lesen von Daten aus einer lokalen SQL Server-Datenbank und anschließendes Trainieren und Bewerten von Modellen mit diesen Daten. Sie müssen die Daten zwischen der Cloud und dem lokalen Server nicht mehr manuell kopieren und synchronisieren. Stattdessen kann das **Import Data**-Modul in Azure Machine Learning Studio jetzt direkt aus einer lokalen SQL Server-Datenbank für Ihre Trainings- und Bewertungsaufträge lesen.

Dieser Artikel bietet einen Überblick über die Vorgehensweise beim Eingang lokaler SQL Server-Daten in Azure Machine Learning. Hierbei wird vorausgesetzt, dass Sie mit Azure Machine Learning-Konzepten wie Arbeitsbereichen, Modulen, Datasets, Experimenten *usw.* vertraut sind.

> [AZURE.NOTE] Dieses Feature ist nicht für kostenlose Arbeitsbereiche verfügbar. Weitere Informationen zu Machine Learning-Preisen und -Ebenen finden Sie unter [Azure Machine Learning Pricing](https://azure.microsoft.com/pricing/details/machine-learning/).

<!-- --> 

[AZURE.INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


## Installieren des Microsoft-Datenverwaltungsgateways

Um in Azure Machine Learning auf eine lokale SQL Server-Datenbank zuzugreifen, müssen Sie das Microsoft-Datenverwaltungsgateway herunterladen und installieren. Beim Konfigurieren der Gatewayverbindung in Machine Learning Studio haben Sie die Möglichkeit, das Gateway im unten beschriebenen Dialogfeld **Datengateway herunterladen und registrieren** herunterzuladen und zu installieren.

Sie können das Datenverwaltungsgateway auch vorab installieren, indem Sie das MSI-Setuppaket aus dem [Microsoft Download Center](https://www.microsoft.com/download/details.aspx?id=39717) herunterladen und ausführen. Wählen Sie die neueste Version, indem Sie Ihrem Computer entsprechend entweder 32-Bit oder 64-Bit auswählen. MSI kann auch zum Aktualisieren eines vorhandenen Datenverwaltungsgateway auf die neueste Version verwendet werden, wobei alle Einstellungen beibehalten werden.

Für das Gateway ist Folgendes erforderlich:

- Die unterstützten Windows-Betriebssystemversionen sind Windows 7, Windows 8/8.1, Windows 10, Windows Server 2008 R2, Windows Server 2012 und Windows Server 2012 R2.
- Die empfohlene Konfiguration für den Gatewaycomputer lautet: mindestens 2 GHz, 4 Kerne, 8 GB RAM und 80 GB Festplattenspeicher.
- Wenn der Hostcomputer in den Ruhezustand wechselt, kann das Gateway nicht auf Datenanforderungen reagieren. Aus diesem Grund konfigurieren Sie vor der Installation des Gateways einen entsprechenden Energiesparplan auf dem Computer. Bei der Gatewayinstallation wird eine Meldung angezeigt, wenn der Computer für den Ruhezustand konfiguriert ist.
- Weil die Kopieraktivität mit einer bestimmten Häufigkeit ausgeführt wird, folgt auch die Ressourcenverwendung (CPU, Arbeitsspeicher) auf dem Computer dem gleichen Muster mit Spitzen- und Leerlaufzeiten. Die Ressourcenverwendung hängt auch stark von der Datenmenge ab, die verschoben wird. Wenn mehrere Kopieraufträge in Bearbeitung sind, werden Sie feststellen, dass die Ressourcenverwendung während der Spitzenzeiten ansteigt. Zwar ist die oben angegebene minimale Konfiguration technisch ausreichend, doch eine Konfiguration mit mehr Ressourcen ist immer vorteilhaft. Dabei ist Ihre spezifische Auslastung für Datenverschiebungen relevant.

Sie sollten beim Einrichten und Verwenden eines Datenverwaltungsgateways Folgendes berücksichtigen:

- Es darf nur eine Instanz des Datenverwaltungsgateways auf einem Computer installiert sein.
- Sie können ein einzelnes Gateway für mehrere lokale Datenquellen verwenden.
- Sie können über mehrere Gateways auf verschiedenen Computern verfügen, die eine Verbindung mit der gleichen lokalen Datenquelle herstellen.
- Sie können ein Gateway nur für jeweils einen Arbeitsbereich konfigurieren. Gateways können zurzeit nicht Arbeitsbereiche übergreifend freigegeben werden.
- Sie können für einen einzelnen Arbeitsbereich mehrere Gateways konfigurieren. Sie möchten z.B. ein Gateway verwenden, das während der Entwicklung mit Ihren Testdatenquellen verbunden ist, und ein Produktionsgateway, wenn Sie bereit sind, den Betrieb aufzunehmen.
- Das Gateway muss sich nicht auf dem gleichen Computer wie die Datenquelle befinden, aber die Nähe zur Datenquelle verkürzt die Zeit, die das Gateway benötigt, um sich mit der Datenquelle zu verbinden. Es wird empfohlen, das Gateway auf einem anderen Computer als dem Computer zu installieren, auf dem die lokale Datenquelle gehostet wird. So konkurriert das Gateway nicht mit der Datenquelle um Ressourcen.
- Wenn Sie bereits ein Gateway auf dem Computer installiert haben, das Power BI- oder Azure Data Factory-Szenarien unterstützt, installieren Sie auf einem anderen Computer ein separates Gateway für Azure Machine Learning.

    > [AZURE.NOTE] Sie können Datenverwaltungsgateway und Power BI-Gateway nicht auf dem gleichen Computer ausführen.

- Sie müssen das Datenverwaltungsgateway auch dann für Azure Machine Learning verwenden, wenn Sie Azure ExpressRoute für andere Daten verwenden. Auch wenn Sie ExpressRoute einsetzen und mit dem Datenverwaltungsgateway eine Verbindung zwischen Machine Learning und der Datenquelle herstellen, sollten Sie Ihre Datenquelle wie eine lokale Datenquelle behandeln (die sich hinter einer Firewall befindet).

Ausführliche Informationen zu Installationsvoraussetzungen, Installationsschritten und Tipps zur Problembehandlung finden Sie im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) beginnend mit dem Abschnitt [Überlegungen zur Verwendung des Datenverwaltungsgateways](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway).

## <span id="using-the-data-gateway-step-by-step-walk" class="anchor"><span id="_Toc450838866" class="anchor"></span></span>Eingang von Daten aus einer lokalen SQL Server-Datenbank in Azure Machine Learning


In dieser exemplarischen Vorgehensweise richten Sie ein Datenverwaltungsgateway in einem Azure Machine Learning-Arbeitsbereich ein, konfigurieren es und lesen dann Daten aus einer lokalen SQL Server-Datenbank.

> [AZURE.TIP] Bevor Sie beginnen, deaktivieren Sie den Popupblocker Ihres Browsers für `studio.azureml.net`. Wenn Sie den Browser Google Chrome verwenden, laden Sie eines der Plug-Ins herunter, die im Google Chrome WebStore unter [ClickOnce-App-Erweiterungen](https://chrome.google.com/webstore/search/clickonce?_category=extensions) verfügbar sind, und installieren Sie es.

### Schritt 1: Erstellen eines Gateways

Der erste Schritt ist das Erstellen und Einrichten des Gateways für den Zugriff auf die lokale SQL-Datenbank.

1. Melden Sie sich bei [Azure Machine Learning Studio](https://studio.azureml.net/Home/) an, und wählen Sie den Arbeitsbereich, in dem Sie arbeiten möchten.

2. Klicken Sie links auf das Blatt **SETTINGS** und dann oben auf die Registerkarte **DATA GATEWAYS**.

3. Klicken Sie am unteren Bildschirmrand auf **NEW DATA GATEWAY**.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-button.png)

4. Geben Sie im Dialogfeld **New data gateway** den **Gateway Name** ein, und fügen Sie mit **Description** optional eine Beschreibung hinzu. Klicken Sie auf den Pfeil in der unteren rechten Ecke, um mit dem nächsten Schritt der Konfiguration fortzufahren.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/new-data-gateway-dialog-enter-name.png)

5. Kopieren Sie im Dialogfeld „Download and register data gateway“ den GATEWAY REGISTRATION KEY in die Zwischenablage.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/download-and-register-data-gateway.png)

6. <span id="note-1" class="anchor"></span>Wenn Sie das Microsoft-Datenverwaltungsgateway noch nicht heruntergeladen und installiert haben, klicken Sie auf **Download data management gateway**. So gelangen Sie zum Microsoft Download Center, in dem Sie die benötigte Gatewayversion auswählen können, um sie herunterzuladen und zu installieren. Ausführliche Informationen zu Installationsvoraussetzungen, Installationsschritten und Tipps zur Problembehandlung finden Sie in den Anfangsabschnitten des Artikels [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

7. Nachdem das Gateway installiert ist, wird der Konfigurations-Manager des Datenverwaltungsgateways geöffnet und das Dialogfeld **Gateway registrieren** angezeigt. Fügen Sie den **Gatewayregistrierungsschlüssel** ein, den Sie in die Zwischenablage kopiert haben, und klicken Sie auf **Registrieren**.

8. Wenn Sie bereits ein Gateway installiert haben, führen Sie den Konfigurations-Manager des Datenverwaltungsgateways aus, klicken Sie auf **Schlüssel ändern**, fügen Sie den **Gatewayregistrierungsschlüssel** ein, den Sie in die Zwischenablage kopiert haben, und klicken Sie auf **OK**.

9. Wenn die Installation abgeschlossen ist, wird das Dialogfeld **Gateway registrieren** für den Konfigurations-Manager des Microsoft-Datenverwaltungsgateways angezeigt. Fügen Sie den GATEWAYREGISTRIERUNGSSCHLÜSSEL ein, den Sie oben in die Zwischenablage kopiert haben, und klicken Sie auf **Registrieren**.

    ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-register-gateway.png)

10. Die Gatewaykonfiguration ist abgeschlossen, wenn die folgenden Werte auf der Registerkarte **Startseite** des Konfigurations-Managers des Microsoft-Datenverwaltungsgateways festgelegt wurden:

    - **Gatewayname** und **Instanzname** sind auf den Namen des Gateways festgelegt.

    - **Registrierung** wird auf **Registriert** festgelegt.

    - **Status** wird auf **Gestartet** festgelegt.

    - Die Statusleiste im unteren Bereich zeigt **Connected to Data Management Gateway Cloud Service** sowie ein grünes Häkchen an.

     ![](media/machine-learning-use-data-from-an-on-premises-sql-server/data-gateway-configuration-manager-registered.png)

     Azure Machine Learning Studio wird bei erfolgreicher Registrierung auch aktualisiert.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-registered.png)

11. Klicken Sie im Dialogfeld **Datengateway herunterladen und registrieren** auf das Häkchen, um das Setup abzuschließen. Auf der Seite **Einstellungen** wird der Gatewaystatus „Online“ angezeigt. Im rechten Bereich finden Sie Status- und andere nützliche Informationen.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\gateway-status.png)

12. Wechseln Sie im Konfigurations-Managers des Microsoft-Datenverwaltungsgateways zur Registerkarte **Zertifikat**. Das auf dieser Registerkarte angegebene Zertifikat wird zum Verschlüsseln/Entschlüsseln von Anmeldeinformationen für den lokalen Datenspeicher verwendet, den Sie im Portal angeben. Dies ist das generierte Standardzertifikat. Microsoft empfiehlt Ihnen, dies in Ihr persönliches Zertifikat zu ändern, das Sie in Ihrem Zertifikatsverwaltungssystem sichern. Klicken Sie auf **Ändern**, um Ihr eigenes Zertifikat zu verwenden.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-certificate.png)

13. (Optional) Wenn Sie die ausführliche Protokollierung aktivieren möchten, um Probleme mit dem Gateway behandeln zu können, wechseln Sie im Konfigurations-Manager des Microsoft-Datenverwaltungsgateways zur Registerkarte **Diagnose**, und aktivieren Sie die Option **Ausführliche Protokollierung für die Problembehandlung aktivieren**. Die Protokollinformationen finden Sie in der Windows-Ereignisanzeige unter **Anwendungs- und Dienstprotokolle** -&gt; Knoten **Datenverwaltungsgateway**. Sie können die Verbindung mit einer lokalen Datenquelle über das Gateway auch über die Registerkarte **Diagnose** testen.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\data-gateway-configuration-manager-verbose-logging.png)

Damit ist die Einrichtung des Gateways in Azure Machine Learning abgeschlossen. Nun können Sie Ihre lokalen Daten nutzen.

Sie können in Studio mehrere Gateways für jeden Arbeitsbereich erstellen und einrichten. Nehmen Sie an, Sie haben ein Gateway, das Sie während der Entwicklung mit den Testdatenquellen verbinden möchten, und ein anderes Gateway für Ihre Produktionsdatenquellen. Azure Machine Learning bietet Ihnen die Flexibilität, abhängig von Ihrer Unternehmensumgebung mehrere Gateways einzurichten. Derzeit kann ein Gateway nicht in verschiedenen Arbeitsbereichen genutzt werden, und auf einem einzelnen Computer kann nur ein einziges Gateway installiert werden. Weitere Punkte, die Sie bei der Installation des Gateways berücksichtigen müssen, finden Sie unter [Überlegungen zur Verwendung des Datenverwaltungsgateways](../data-factory/data-factory-move-data-between-onprem-and-cloud.md#considerations-for-using-data-management-gateway) im Artikel [Verschieben von Daten zwischen lokalen Quellen und der Cloud mit dem Datenverwaltungsgateway](../data-factory/data-factory-move-data-between-onprem-and-cloud.md).

### Schritt 2: Verwenden des Gateways zum Lesen von Daten aus einer lokalen Datenquelle

Nachdem Sie das Gateway eingerichtet haben, können Sie ein **Import Data**-Modul einem Experiment hinzufügen, das die Daten aus einer lokalen SQL Server-Datenbank eingibt.

1.  Wählen Sie in Machine Learning Studio die Registerkarte **EXPERIMENTS**, klicken Sie in der linken unteren Ecke auf **+NEW**, und wählen Sie **Blank Experiment** (oder eines von mehreren verfügbaren Beispielexperimenten).

2.  Suchen Sie das Modul **Import Data**, und ziehen Sie es in den Experimentbereich.

3.  Klicken Sie unter dem Bereich auf **Save as**. Geben Sie „Azure Machine Learning On-Premises SQL Server Tutorial“ als Namen des Experiments ein, wählen Sie den Arbeitsbereich, und klicken Sie auf das **OK**-Häkchen.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\experiment-save-as.png)

4.  Klicken Sie auf das **Import Data**-Modul, um es auszuwählen, wählen Sie dann rechts neben dem Bereich in **Properties** in der Einblendliste **Data source** „On-Premises SQL Database“ aus.

5.  Wählen Sie das **Data gateway**, das sie installiert und registriert haben. Sie können ein anderes Gateway einrichten, indem Sie „(add new Data Gateway…)“ auswählen.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-select-on-premises-data-source.png)

6.  Geben Sie **Database server name** und **Database name** der SQL-Datenbank ein, zusammen mit der SQL-**Database query**, die Sie ausführen möchten.

7.  Klicken Sie unter **User name and password** auf **Enter values**, und geben Sie Ihre Datenbank-Anmeldeinformationen ein. Je nach Ihrer lokalen SQL Server-Konfiguration können Sie die Integrierte Windows-Authentifizierung oder SQL Server-Authentifizierung verwenden.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\database-credentials.png)
    
    Die Meldung „values required“ ändert sich in „values set“ mit einem grünen Häkchen. Sie müssen die Anmeldeinformationen nur einmal eingeben, solange sich die Datenbankinformationen oder das Kennwort nicht ändern. Azure Machine Learning verwendet das Zertifikat, das Sie bei der Installation des Gateways zum Verschlüsseln der Anmeldeinformationen in der Cloud bereitgestellt haben. Azure speichert nie unverschlüsselt lokale Anmeldeinformationen.

    ![](media\machine-learning-use-data-from-an-on-premises-sql-server\import-data-properties-entered.png)

8.  Klicken Sie auf **RUN**, um das Experiment auszuführen.

Nach Abschluss der Ausführung des Experiments können Sie die Daten, die Sie aus der Datenbank importiert haben, durch Klicken auf den Ausgabeport des **Import Data**-Moduls und Auswählen von **Visualize** visuell darstellen.

Wenn Sie die Entwicklung des Experiments abgeschlossen haben, können Sie Ihr Modell bereitstellen und in Betrieb nehmen. Mit dem Batchausführungsdienst werden Daten aus der im **Import Data**-Modul konfigurierten lokalen SQL Server-Datenbank gelesen und für die Bewertung verwendet. Sie können zwar den Request Response Service zur Bewertung lokaler Daten verwenden, aber Microsoft empfiehlt stattdessen die Verwendung des [Excel-Add-Ins](machine-learning-excel-add-in-for-web-services.md). Derzeit wird das Schreiben in eine lokale SQL Server-Datenbank über **Export Data** weder in Ihren Experimenten noch in veröffentlichten Webdiensten unterstützt.

<!---HONumber=AcomDC_0921_2016-->
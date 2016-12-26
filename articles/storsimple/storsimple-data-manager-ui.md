---
title: "Microsoft Azure StorSimple Data Manager Benutzeroberfläche| Microsoft Docs"
description: "Beschreibt, wie Sie die Benutzeroberfläche des StorSimple Data Manager-Diensts verwenden (private Vorschau)"
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
ms.date: 11/22/2016
ms.author: vidarmsft
translationtype: Human Translation
ms.sourcegitcommit: 692f00bf342e2cd6d3d76754d18751c773aeedc5
ms.openlocfilehash: 53a8599df2c647613122cd791b680e2e658586b0

---

# <a name="manage-using-the-storsimple-data-manager-service-ui-private-preview"></a>Verwalten der Benutzeroberfläche des StorSimple Data Manager-Diensts (private Vorschau)

In diesem Artikel wird erläutert, wie Sie die Benutzeroberfläche von StorSimple Data Manager verwenden, um Datentransformation von Daten auf den Geräten der StorSimple 8000-Serie auszuführen. Die transformierten Daten können von anderen Azure-Diensten, wie z.B. Azure Media Services, Azure HDInsight, Azure Machine Learning und Azure Search, genutzt werden. 


## <a name="use-storsimple-data-transformation"></a>Verwenden der StorSimple-Datentransformation

StorSimple Data Manager ist die Ressource, in der die Datentransformation instanziiert werden kann. Der Datentransformationsdienst erlaubt es Ihnen, Daten von Ihrem lokalen StorSimple-Gerät in Blobs in Azure Storage zu verschieben. Daher müssen Sie im Workflow die Details zu Ihrem StorSimple-Gerät sowie die für Sie interessanten Daten angeben, die Sie in das Speicherkonto verschieben möchten.

### <a name="create-a-storsimple-data-manager-service"></a>Erstellen eines StorSimple Data Manager-Diensts

Führen Sie die folgenden Schritte aus, um einen StorSimple Data Manager-Dienst zu erstellen.

1. Wechseln Sie zum Erstellen eines StorSimple Data Manager-Diensts zu [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager).

2. Klicken Sie auf das Symbol **+**, und suchen Sie nach StorSimple Data Manager. Klicken Sie auf Ihren StorSimple Data Manager-Dienst, und klicken Sie dann auf **Erstellen**.

3. Wenn Ihr Abonnement für die Erstellung dieses Diensts aktiviert ist, wird das folgende Blatt angezeigt.

    ![Erstellen einer StorSimple Data Manager-Ressource](./media/storsimple-data-manager-ui/create-new-data-manager-service.png)

4. Nehmen Sie die Eingaben vor, und klicken Sie auf **Erstellen**. Der angegebene Standort sollte dem entsprechen, der Ihre Speicherkonten und Ihren StorSimple Manager-Dienst enthält. Derzeit werden nur die Regionen USA, Westen und Europa, Westen unterstützt. Daher sollten sich Ihr StorSimple Manager-Dienst, Data Manager-Dienst und die zugeordneten Speicherkonten alle in den oben genannten unterstützten Regionen befinden. Es dauert ungefähr eine Minute, den Dienst zu erstellen.

### <a name="create-a-data-transformation-job-definition"></a>Erstellen einer Auftragsdefinition für die Datentransformation

Sie müssen innerhalb eines StorSimple Data Manager-Diensts eine Auftragsdefinition für die Datentransformation erstellen. Eine Auftragsdefinition gibt Details zu den Daten im nativen Format an, die Sie in ein Speicherkonto verschieben möchten. 

Führen Sie die folgenden Schritte aus, um eine Auftragsdefinition für eine neue Datentransformation zu erstellen.

1.  Navigieren Sie zu dem Dienst, den Sie erstellt haben. Klicken Sie auf **+ Auftragsdefinition**.

    ![Auf +Auftragsdefinition klicken](./media/storsimple-data-manager-ui/click-add-job-definition.png)

2. Das Blatt der neuen Auftragsdefinition wird geöffnet. Benennen Sie die Auftragsdefinition, und klicken Sie auf **Quelle**. Geben Sie auf dem Blatt **Datenquelle konfigurieren** die Details des StorSimple-Geräts und der für Sie interessanten Daten an.

    ![Auftragsdefinition erstellen](./media/storsimple-data-manager-ui//create-new-job-deifnition.png)

3. Da es sich um einen neuen Data Manager-Dienst handelt, werden keine Datenrepositorys konfiguriert. Klicken Sie in der Dropdownliste im Datenrepository auf **Neu hinzufügen**, und anschließend auf **Add Data Repository** (Datenrepository hinzufügen), um Ihren StorSimple Manager als Datenrepository hinzuzufügen.

4. Wählen Sie **StorSimple 8000 series Manager (Manager der StorSimple 8000-Serie)** als Typ des Repositorys aus, und geben Sie die Eigenschaften Ihres **StorSimple Managers** ein. Im Feld **Ressourcen-ID** müssen Sie die Nummer vor **:** im Registrierungsschlüssel Ihres StorSimple Managers eingeben.

    ![Erstellen der Datenquelle](./media/storsimple-data-manager-ui/create-new-data-source.png)

5.  Klicken Sie nach Abschluss des Vorgangs auf **OK**. Dadurch wird Ihr Datenrepository gespeichert, und dieser StorSimple Manager kann in anderen Auftragsdefinitionen wiederverwendet werden, ohne diese Parameter erneut einzugeben. Nachdem Sie auf **OK** geklickt haben, dauert es ein paar Sekunden bevor der StorSimple Manager in der Dropdownliste angezeigt wird.

6.  Geben Sie auf dem Blatt **Datenquelle konfigurieren** den Gerätenamen und den Namen des Volume an, das die für Sie interessanten Daten enthält.

7.  Geben Sie im Unterabschnitt **Filter** das Stammverzeichnis ein, das die für Sie interessanten Daten enthält (das Feld sollte mit einem `\` beginnen). Sie können hier auch Dateifilter hinzufügen.

8.  Der Datentransformationsdienst funktioniert mit den Daten, die über Momentaufnahmen an die Azure-Cloud übertragen werden. Bei der Ausführung dieses Auftrags können Sie entscheiden, bei jeder Ausführung dieses Auftrags eine Sicherung vorzunehmen (um aktuellste Daten zu bearbeiten), oder die letzte vorhandene Sicherung in der Cloud zu verwenden (wenn Sie archivierte Daten bearbeiten).

    ![Details der neuen Datenquelle](./media/storsimple-data-manager-ui/new-data-source-details.png)

9. Als Nächstes müssen die Zieleinstellungen konfiguriert werden. Es gibt 2 Typen von unterstützten Zielen – Azure-Speicherkonten und Azure Media Services-Konten. Wählen Sie Speicherkonten aus, wenn Sie Dateien in Blobs in diesem Konto ablegen möchten. Wählen Sie Media Services-Konten aus, wenn Sie Dateien in Objekten in diesem Konto ablegen möchten. Auch hier brauchen wir wieder ein Repository. Wählen Sie in der Dropdownliste **Neu hinzufügen** aus, und dann **Konfigurieren von Einstellungen**.

    ![Erstellen der Datensenke](./media/storsimple-data-manager-ui/create-new-data-sink.png)

10. Hier können Sie den Typ des Repositorys auswählen, das Sie hinzufügen möchten, und die anderen Parameter, die dem Repository zugeordnet werden sollen. In beiden Fällen wird bei der Ausführung des Auftrags eine Speicherwarteschlange erstellt. Diese Warteschlange wird mit Nachrichten über transformierte Blobs aufgefüllt, wenn diese bereit sind. Der Name dieser Warteschlange entspricht dem Namen der Auftragsdefinition. Wenn Sie für den Typ des Repositorys **Media Services** auswählen, können Sie auch Anmeldeinformationen des Speicherkontos eingeben, wenn die Warteschlange erstellt wird.

    ![Details der neuen Daten-Senke](./media/storsimple-data-manager-ui/new-data-sink-details.png)

11. Nach Hinzufügen des Datenrepository (dies kann ein paar Sekunden dauern) erscheint das Repository in der Dropdownliste in **Target account name** (Zielkontoname).  Wählen Sie das Ziel aus, das Sie benötigen.

12. Klicken Sie auf **OK**, um die Auftragsdefinition zu erstellen. Die Auftragsdefinition wird jetzt eingerichtet. Sie können diese Auftragsdefinition mehrmals über die Benutzeroberfläche verwenden.

    ![Neue Auftragsdefinition hinzufügen](./media/storsimple-data-manager-ui/add-new-job-definition.png)

### <a name="run-the-job-definition"></a>Ausführen der Auftragsdefinition

Wenn Sie Daten aus StorSimple in das Speicherkonto verschieben möchten, das Sie in der Auftragsdefinition angegeben haben, müssen Sie ihn aufrufen. Die Parameter können jedes Mal, wenn Sie den Auftrag aufrufen, flexibel geändert werden. Die Schritte lauten wie folgt:

1. Wählen Sie Ihren StorSimple Data Manager-Dienst aus, und klicken Sie dann auf **Überwachung**. Klicken Sie auf **Jetzt ausführen**.

    ![Auftragsdefinition auslösen](./media/storsimple-data-manager-ui/run-now.png)

2. Wählen Sie die Auftragsdefinition aus, die Sie ausführen möchten. Klicken Sie auf **Ausführungseinstellungen**, um Einstellungen zu modifizieren, die Sie möglicherweise bei dieser Ausführung des Auftrags ändern möchten.

    ![Ausführungseinstellungen des Auftrags](./media/storsimple-data-manager-ui/run-settings.png)

3. Klicken Sie auf **OK**, und klicken Sie dann auf **Ausführen**, um Ihren Auftrag auszuführen. Gehen Sie zur Seite **Aufträge** in Ihrem StorSimple Data Manager, um diesen Auftrag zu überwachen.

    ![Liste der Aufträge und Status](./media/storsimple-data-manager-ui/jobs-list-and-status.png)

4. Zusätzlich zur Überwachung können Sie auf dem Blatt **Aufträge** außerdem die Speicherwarteschlange abhören. Hier wird jedes Mal eine Nachricht hinzugefügt, wenn eine Datei aus StorSimple in das Speicherkonto verschoben wird.


## <a name="next-steps"></a>Nächste Schritte

[Use .NET SDK to launch StorSimple Data Manager jobs (Verwenden von .NET SDK zum ausführen von StorSimple Data Manager-Aufträgen)](storsimple-data-manager-dotnet-jobs.md).


<!--HONumber=Nov16_HO4-->



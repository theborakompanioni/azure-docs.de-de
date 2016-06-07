<properties 
   pageTitle="Installieren von Update 0.1 auf Ihrem StorSimple Virtual Array | Microsoft Azure"
   description="Hier wird beschrieben, wie Sie über die Webbenutzeroberfläche von StorSimple Virtual Array Update 0.1 über das Portal und die Hotfix-Methode anwenden."
   services="storsimple"
   documentationCenter="NA"
   authors="alkohli"
   manager="carmonm"
   editor="" />
<tags 
   ms.service="storsimple"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="TBD"
   ms.date="05/24/2016"
   ms.author="alkohli" />

# Installieren Sie Update 0.1 auf Ihrem StorSimple Virtual Array

## Übersicht

Dieser Artikel beschreibt die erforderlichen Schritte zum Installieren von Update 0.1 auf Ihrem StorSimple Virtual Array. Er erklärt die Updateverfahren über das klassische Azure-Portal und über die lokale Webbenutzeroberfläche. Beide Updateverfahren dauern jeweils weniger als 2 Minuten.

In der Regel müssen Sie Softwareupdates oder Hotfixes installieren, um StorSimple Virtual Array auf dem neuesten Stand zu halten. Im Allgemeinen wird empfohlen, Updates über das klassische Azure-Portal zu installieren. Wenn das Portal nicht verfügbar ist, können Sie zum Anwenden von Hotfixes oder Updates jedoch auch die lokale Webbenutzeroberfläche verwenden.

Beachten Sie, dass das Gerät bei der Update- oder Hotfixinstallation neu gestartet wird. Da es sich bei StorSimple Virtual Array um ein Gerät mit einem Einzelknoten handelt, werden alle aktuellen IOs unterbrochen, was zu einer gewissen Ausfallzeit führt.

Außerdem wird empfohlen, die Volumes oder Freigaben vor dem Anwenden eines Updates auf dem Host und dann auf dem Gerät offline zu nehmen. So wird das Risiko einer Datenbeschädigung minimiert.

## Verwenden des klassischen Azure-Portals

Wir empfehlen Ihnen, Updates über das klassische Azure-Portal zu installieren. Beim Verfahren über das Portal muss der Benutzer die Updates suchen, herunterladen und anschließend installieren. Führen Sie die folgenden Schritte aus, um Updates oder Hotfixes zu installieren.

#### So installieren Sie Updates über das klassische Azure-Portal

1. Wählen Sie auf der Seite **Geräte** das Gerät aus, für das Sie Updates installieren möchten.

2. Navigieren Sie zu **Geräte** > **Wartung** > **Softwareupdates**.

3. Wenn Softwareupdates verfügbar sind, wird eine Meldung angezeigt. Klicken Sie am unteren Rand der Seite auf **Updates scannen**, um nach Updates zu suchen.

	![Updates scannen](./media/storsimple-ova-install-update-01/aupdate1m.png)

4. Klicken Sie unten auf der Seite auf **Updates herunterladen**. Der Benutzer wird mit einem Dialogfeld darauf hingewiesen, dass das Update zu einer Unterbrechung führt. Da es sich bei StorSimple Virtual Array um ein Gerät mit einem Einzelknoten handelt, wird das Gerät nach dem Update neu gestartet. Dadurch werden alle ausgeführt IOs unterbrochen. Klicken Sie auf das Häkchensymbol, um einen Auftrag zum Herunterladen der verfügbaren Updates zu starten.

	![Herunterladen von Updates bestätigen](./media/storsimple-ova-install-update-01/aupdate3m.png)

	Sie werden benachrichtigt, nachdem die Updates heruntergeladen wurden.

	![Updates herunterladen](./media/storsimple-ova-install-update-01/aupdate5m.png)

5. Klicken Sie unten auf der Seite auf **Updates installieren**, um mit dem Update des Geräts zu beginnen. Das Dialogfeld erscheint erneut. Klicken Sie auf das Häkchensymbol, um einen Auftrag zum Installieren der Updates zu starten.
 
 	![Installieren von Updates bestätigen](./media/storsimple-ova-install-update-01/aupdate6m.png)
 
 
	Sie werden benachrichtigt, wenn der Auftrag erstellt wurde.

	![Installieren von Updates](./media/storsimple-ova-install-update-01/aupdate8m.png)
	
6. Klicken Sie auf den Link **Auftrag anzeigen**, und wechseln Sie zur Seite **Aufträge**, um den Installationsstatus zu prüfen. Klicken Sie auf **Details**, um detaillierte Informationen zum Updateauftrag zu erhalten.

	![Updates überwachen](./media/storsimple-ova-install-update-01/aupdate9m.png)

6. Wechseln Sie nach Abschluss der Installation (wenn der Auftragsstatus bei 100 % ist) zu **Geräte** > **Wartung** > **Softwareupdates**. Die angezeigte Softwareversion sollte **10.0.10279.0** lauten.

	![Updates überprüfen](./media/storsimple-ova-install-update-01/aupdate13m.png)

## Verwenden der lokalen Webbenutzeroberfläche 

Verwenden Sie die lokale Webbenutzeroberfläche, um Updates anzuwenden, wenn das klassische Azure-Portal nicht verfügbar ist. Die Verwendung der lokalen Webbenutzeroberfläche umfasst zwei Schritte:

- Herunterladen des Updates oder Hotfixes
- Installieren des Updates oder Hotfixes

### Herunterladen des Updates oder Hotfixes

Führen Sie die folgenden Schritte aus, um das Softwareupdate aus dem Microsoft Update-Katalog herunterzuladen.

#### So laden sie das Update oder den Hotfix herunter

1. Starten Sie Internet Explorer, und navigieren Sie zu [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Falls Sie den Microsoft Update-Katalog zum ersten Mal auf diesem Computer verwenden, klicken Sie auf **Installieren**, wenn Sie zum Installieren des Add-Ons für den Microsoft Update-Katalog aufgefordert werden.
   
	![Katalog installieren](./media/storsimple-ova-install-update-01/install-catalog.png)

3. Geben Sie im Suchfeld des Microsoft Update-Katalogs die KB-Nummer des Hotfixes ein, den Sie herunterladen möchten (beispielsweise **3160441**), und klicken Sie anschließend auf **Suchen**.

    Die Hotfix-Liste wird angezeigt, z.B. **Microsoft Azure StorSimple Virtual Array Update 0.1**.

    ![Katalog durchsuchen](./media/storsimple-ova-install-update-01/download1.png)

4. Klicken Sie auf **Hinzufügen**. Das Update wird dem Warenkorb hinzugefügt.

5. Klicken Sie auf **Warenkorb anzeigen**.

6. Klicken Sie auf **Download**. Geben Sie einen lokalen Speicherort an, oder klicken Sie auf **Durchsuchen**, und navigieren Sie zum gewünschten lokalen Speicherort für den Download. Die Updates werden heruntergeladen und am angegebenen Speicherort in einem nach dem Update benannten Unterordner gespeichert. Der Ordner kann auch in eine Netzwerkfreigabe kopiert werden, auf die vom Gerät aus zugegriffen werden kann.


### Installieren des Updates oder Hotfixes

Stellen Sie vor der Update- oder Hotfixinstallation sicher, dass das Update oder der Hotfix entweder lokal auf Ihren Host heruntergeladen wurde bzw. dass auf das Update oder den Hotfix über eine Netzwerkfreigabe zugegriffen werden kann. Führen Sie die folgenden Schritte aus, um Updates oder Hotfixes zu installieren.

#### So installieren Sie das Update oder den Hotfix

1. Wechseln Sie auf der lokalen Webbenutzeroberfläche zu **Wartung** > **Softwareupdate**.

2. Geben Sie unter **Updatedateipfad** den Dateinamen für das Update oder den Hotfix ein. Sie können auch zur Installationsdatei des Updates oder Hotfixes navigieren, sofern sie auf einer Netzwerkfreigabe abgelegt wurde. Klicken Sie auf **Übernehmen**.

	![Gerät aktualisieren](./media/storsimple-ova-install-update-01/update1m.png)

3.  Eine Warnung wird angezeigt. Da es sich hierbei um ein Gerät mit einem Einzelknoten handelt, wird das Gerät neu gestartet, und es kommt zu einer gewissen Ausfallzeit. Klicken Sie auf das Häkchensymbol.

	![Gerät aktualisieren](./media/storsimple-ova-install-update-01/update4m.png)

4. Das Update wird gestartet. Nachdem das Gerät erfolgreich aktualisiert wurde, wird es neu gestartet. Auf die lokale Benutzeroberfläche kann währenddessen nicht zugegriffen werden.

    ![Gerät aktualisieren](./media/storsimple-ova-install-update-01/update6m.png)

4. Nach dem Neustart wird die Anmeldeseite angezeigt. Sie können anschließend die Version der Software prüfen. Wechseln Sie zu **Wartung** > **Softwareupdate**. Die Buildnummer sollte **10.0.0.0.10279** sein.

	> [AZURE.NOTE] Die Softwareversionen werden in der lokalen Webbenutzeroberfläche und im klassischen Azure-Portal etwas unterschiedlich angegeben. Die lokale Webbenutzeroberfläche gibt **10.0.0.0.10279** an, und das klassische Azure-Portal **10.0.10279.0**, obwohl die Version die gleiche ist.

	![Gerät aktualisieren](./media/storsimple-ova-install-update-01/update9m.png)

## Nächste Schritte

Weitere Informationen zur [Verwaltung des StorSimple Virtual Array](storsimple-ova-web-ui-admin.md).

<!---HONumber=AcomDC_0525_2016-->
<properties 
   pageTitle="Installieren von Updates auf einem StorSimple Virtual Array | Microsoft Azure"
   description="Hier wird beschrieben, wie Sie die Webbenutzeroberfläche des StorSimple Virtual Arrays verwenden, um Updates und Hotfixes über das Portal anzuwenden."
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
   ms.date="06/16/2016"
   ms.author="alkohli" />

# Installieren von Updates auf einem StorSimple Virtual Array

## Übersicht

Dieser Artikel beschreibt die erforderlichen Schritte zum Installieren von Updates auf Ihrem StorSimple Virtual Array über die lokale Webbenutzeroberfläche. Dieses Verfahren dauert weniger als zwei Minuten. Sie müssen Softwareupdates oder -hotfixes installieren, um Ihr StorSimple Virtual Array auf dem neuesten Stand zu halten.

Beachten Sie, dass das Gerät bei der Installation des Updates oder Hotfixes neu gestartet wird. Da es sich bei einem StorSimple Virtual Array um ein Gerät mit einem Einzelknoten handelt, werden alle aktuellen E/A-Vorgänge unterbrochen, was zu einer gewissen Ausfallzeit führt.

Außerdem wird empfohlen, die Volumes oder Freigaben vor dem Anwenden eines Updates auf dem Host und dann auf dem Gerät offline zu nehmen. So wird das Risiko einer Datenbeschädigung minimiert.

## Verwenden der lokalen Webbenutzeroberfläche 
Sie müssen die lokale Webbenutzeroberfläche verwenden, um Hotfixes oder Updates zu installieren (zurzeit können Sie Updates nicht über das klassische Azure-Portal installieren).

Die Verwendung der lokalen Webbenutzeroberfläche umfasst zwei Schritte:

- Herunterladen des Updates oder Hotfixes
- Installieren des Updates oder Hotfixes

### Herunterladen des Updates oder Hotfixes

Führen Sie die folgenden Schritte aus, um das Softwareupdate aus dem Microsoft Update-Katalog herunterzuladen.

#### So laden sie das Update oder den Hotfix herunter

1. Starten Sie Internet Explorer, und navigieren Sie zu [http://catalog.update.microsoft.com](http://catalog.update.microsoft.com).

2. Falls Sie den Microsoft Update-Katalog zum ersten Mal auf diesem Computer verwenden, klicken Sie auf **Installieren**, wenn Sie zum Installieren des Add-Ons für den Microsoft Update-Katalog aufgefordert werden.
   
	![Katalog installieren](./media/storsimple-ova-install-update-01/install-catalog.png)

3. Geben Sie im Suchfeld des Microsoft Update-Katalogs die KB-Nummer (Knowledge Base) des Hotfixes ein, den Sie herunterladen möchten. Geben Sie **3160441** für Update 0.2 bzw. **3160441** für Update 0.1 ein, und klicken Sie anschließend auf **Suchen**.

    Die Hotfixliste wird angezeigt, z.B. **StorSimple Virtual Array Update 0.1**.

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

4. Nach dem Neustart wird die Anmeldeseite angezeigt. Sie können anschließend die Version der Software prüfen. Wechseln Sie zu **Wartung** > **Softwareupdate**. Die angezeigte Softwareversion sollte **10.0.10280.0** für Update 0.2 bzw. **10.0.10279.0** für Update 0.1 lauten.

	> [AZURE.NOTE] Die Softwareversionen werden in der lokalen Webbenutzeroberfläche und im klassischen Azure-Portal etwas unterschiedlich angegeben. Beispiel: Die lokale Webbenutzeroberfläche gibt **10.0.0.0.10279** an, und das klassische Azure-Portal **10.0.10279.0**, obwohl die Version die gleiche ist.

	![Gerät aktualisieren](./media/storsimple-ova-install-update-01/update9m.png)

## Nächste Schritte

Erfahren Sie, wie Sie [Ihr StorSimple Virtual Array verwalten](storsimple-ova-web-ui-admin.md).

<!---HONumber=AcomDC_0622_2016-->
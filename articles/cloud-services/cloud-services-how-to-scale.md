<properties
	pageTitle="Automatisches Skalieren eines Clouddiensts im Portal | Microsoft Azure"
	description="(Klassisch) Erfahren Sie, wie Sie mit dem klassischen Portal Regeln für die automatische Skalierung für eine Clouddienst-Webrolle oder -Workerrolle in Azure konfigurieren."
	services="cloud-services"
	documentationCenter=""
	authors="Thraka"
	manager="timlt"
	editor=""/>

<tags
	ms.service="cloud-services"
	ms.workload="tbd"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="04/06/2016"
	ms.author="adegeo"/>


# Automatisches Skalieren eines Clouddiensts

> [AZURE.SELECTOR]
- [Azure-Portal](cloud-services-how-to-scale-portal.md)
- [Klassisches Azure-Portal](cloud-services-how-to-scale.md)

Auf der Seite „Skalieren“ des klassischen Azure-Portals können Sie manuell Ihre Webrolle oder Workerrolle skalieren, oder Sie können die automatische Skalierung basierend auf CPU-Auslastung oder einer Nachrichtenwarteschlange aktivieren.

>[AZURE.NOTE] Dieser Artikel konzentriert sich auf Clouddienst-Webrollen und -Workerrollen. Wenn Sie einen virtuellen Computer (klassisch) direkt erstellen, wird er in einem Clouddienst gehostet. Einige dieser Informationen gelten für diese Typen virtueller Computer. Das Skalieren einer Verfügbarkeitsgruppe virtueller Computer besteht eigentlich nur darin, dass Sie sie entsprechend der Skalierungsregeln, die Sie konfigurieren, ein- und ausschalten beendet. Weitere Informationen zu virtuellen Computern und Verfügbarkeitsgruppen finden Sie unter [Konfigurieren einer Verfügbarkeitsgruppe für virtuelle Computer im klassischen Bereitstellungsmodell](../virtual-machines/virtual-machines-windows-classic-configure-availability.md).

Folgendes ist allerdings zu beachten, bevor die Skalierung einer Anwendung konfiguriert wird:

- Die Skalierung ist abhängig von der Kernspeichernutzung. Größere Rolleninstanzen verwenden mehr Kerne. Sie können eine Anwendung nur innerhalb der für Ihr Abonnement geltenden Kerngrenzwerte skalieren. Wenn als Grenzwert für Ihr Abonnement beispielsweise zwanzig Kerne festgelegt sind, und Sie eine Anwendung mit zwei mittelgroßen Clouddiensten ausführen (insgesamt vier Kerne), stehen für das zentrale Hochskalieren anderer Clouddienstbereitstellungen in Ihrem Abonnement nur noch sechzehn Kerne zur Verfügung. Weitere Informationen zu Größen finden Sie unter [Größen für Clouddienste](cloud-services-sizes-specs.md).

- Sie müssen eine Warteschlange anlegen und dieser einer Rolle zuweisen, bevor Sie eine Anwendung auf Basis eines Nachrichtenschwellwerts skalieren können. Weitere Informationen finden Sie unter [Verwenden des Warteschlangenspeicherdiensts](../storage/storage-dotnet-how-to-use-queues.md).

- Sie können Ressourcen skalieren, die mit Ihrem Clouddienst verknüpft sind. Weitere Informationen zum Verknüpfen von Ressourcen finden Sie unter [Verknüpfen einer Ressource mit einem Clouddienst](cloud-services-how-to-manage.md#how-to-link-a-resource-to-a-cloud-service).

- Um die Hochverfügbarkeit Ihrer Anwendung zu gewährleisten, sollte diese mit mindestens zwei Rolleninstanzen bereitgestellt werden. Weitere Informationen finden Sie unter [Vereinbarungen zum Servicelevel](https://azure.microsoft.com/support/legal/sla/).



## Zeitplan (Skalierung)

In der Standardeinstellung folgen alle Rollen keinem bestimmten Zeitplan. Daher gelten alle geänderten Einstellungen für alle Uhrzeiten und alle Tage im Laufe des Jahres. Wenn Sie möchten, können Sie manuelle oder automatische Skalierung einrichten:

- Wochentage
- Wochenenden
- Woche nachts
- Woche morgens
- Bestimmte Datumsangaben
- Bestimmte Datumsbereiche

Dies wird im [klassischen Azure-Portal](https://manage.windowsazure.com/) auf der Seite **Clouddienste** > **[Ihr Clouddienst]** > **Skalieren** > **[Produktion oder Staging]** konfiguriert.

Klicken Sie für jede Rolle, die Sie ändern möchten, auf die Schaltfläche **Zeiten für Zeitplan einrichten**.

![Automatisches Skalieren von Clouddiensten auf Basis eines Zeitplans][scale_schedules]



## Manuelles Skalieren

Auf der Seite **Skalieren** können Sie die Anzahl der in einem Clouddienst ausgeführten Instanzen manuell erhöhen oder verringern. Dies wird für jeden Zeitplan konfiguriert, den Sie erstellt haben, oder für alle Zeiten, wenn Sie keinen Zeitplan erstellt haben.

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) auf **Cloud Services** und dann auf den Namen des Clouddiensts, um das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Sie Ihren Clouddienst nicht sehen können, müssen Sie möglicherweise von **Produktion** zu **Staging** wechseln oder umgekehrt.

2. Klicken Sie auf **Skalieren**.

3. Wählen Sie den Zeitplan, für den Sie die Skalierungsoptionen ändern möchten. Der Standard ist *Keine geplanten Zeiten*, wenn Sie keine Zeitpläne definiert haben.

4. Suchen Sie den Abschnitt **Nach Metrik skalieren**, und wählen Sie **KEINE** aus. Dies ist die Standardeinstellung für alle Rollen.

5. Jede Rolle in einem Clouddienst hat einen Regler, über den die Anzahl der verwendbaren Instanzen geändert werden kann.

    ![Manuelles Skalieren einer Clouddienstrolle][manual_scale]

    Wenn Sie mehr Instanzen benötigen, müssen Sie möglicherweise die [Größe des Clouddiensts für den virtuellen Computer](cloud-services-sizes-specs.md) ändern.

6. Klicken Sie auf **Speichern**. Je nach Reglerstellung werden Rolleninstanzen hinzugefügt oder entfernt.

>[AZURE.TIP] Wenn Sie ![][tip_icon] sehen, bewegen Sie die Maus dorthin, und Sie können Hilfe zu den Aktionen einer bestimmten Einstellung erhalten.


## Automatisches Skalieren – CPU

Hiermit skalieren Sie, ob die durchschnittliche prozentuale CPU-Nutzung bestimmte Schwellenwerte über- oder unterschreitet; Rolleninstanzen werden erstellt oder gelöscht.

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) auf **Cloud Services** und dann auf den Namen des Clouddiensts, um das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Sie Ihren Clouddienst nicht sehen können, müssen Sie möglicherweise von **Produktion** zu **Staging** wechseln oder umgekehrt.

2. Klicken Sie auf **Skalieren**.

3. Wählen Sie den Zeitplan, für den Sie die Skalierungsoptionen ändern möchten. Der Standard ist *Keine geplanten Zeiten*, wenn Sie keine Zeitpläne definiert haben.

4. Suchen Sie den Abschnitt **Nach Metrik skalieren**, und wählen Sie **CPU** aus.

5. Jetzt können Sie einen minimalen und maximalen Bereich von Rolleninstanzen, die Ziel-CPU-Auslastung (um zentrales Hochskalieren auszulösen) und die Anzahl der Instanzen, um die zentral hoch- und herunterskaliert werden soll, konfigurieren.

![Skalieren einer Clouddienstrolle nach CPU-Auslastung][cpu_scale]

>[AZURE.TIP] Wenn Sie ![][tip_icon] sehen, bewegen Sie die Maus dorthin, und Sie können Hilfe zu den Aktionen einer bestimmten Einstellung erhalten.





## Automatisches Skalieren – Warteschlange

Dabei wird automatisch skaliert, ob die Anzahl der Nachrichten in einer Warteschlange einen angegebenen Schwellenwert über- oder unterschreitet; Rolleninstanzen werden erstellt oder gelöscht.

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) auf **Cloud Services** und dann auf den Namen des Clouddiensts, um das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Sie Ihren Clouddienst nicht sehen können, müssen Sie möglicherweise von **Produktion** zu **Staging** wechseln oder umgekehrt.

2. Klicken Sie auf **Skalieren**.

3. Suchen Sie den Abschnitt **Nach Metrik skalieren**, und wählen Sie **CPU** aus.

4. Jetzt können Sie einen minimalen und maximalen Bereich von Rolleninstanzen, die Warteschlange und die Anzahl der Warteschlangennachrichten, die für jede Instanz verarbeitet werden sollen, und die Anzahl der Instanzen, um die zentral hoch- und herunterskaliert werden soll, konfigurieren.

![Skalieren einer Clouddienstrolle nach Nachrichtenwarteschlange][queue_scale]

>[AZURE.TIP] Wenn Sie ![][tip_icon] sehen, bewegen Sie die Maus dorthin, und Sie können Hilfe zu den Aktionen einer bestimmten Einstellung erhalten.


## Skalieren verknüpfter Ressourcen

Häufig empfiehlt es sich, beim Skalieren einer Rolle auch die von der Anwendung verwendete Datenbank zu skalieren. Wenn Sie die Datenbank mit dem Clouddienst verknüpfen, können Sie auf die Skalierungseinstellungen für diese Ressource zugreifen, indem Sie auf den entsprechenden Link klicken.

1. Klicken Sie im [klassischen Azure-Portal](https://manage.windowsazure.com/) auf **Cloud Services** und dann auf den Namen des Clouddiensts, um das Dashboard zu öffnen.

    > [AZURE.TIP] Wenn Sie Ihren Clouddienst nicht sehen können, müssen Sie möglicherweise von **Produktion** zu **Staging** wechseln oder umgekehrt.

2. Klicken Sie auf **Skalieren**.

3. Suchen Sie die **verknüpften Ressourcen**, und klicken Sie auf **Skalierung für diese Datenbank verwalten**.

    > [AZURE.NOTE] Wenn Sie keine **verknüpften Ressourcen** sehen, besitzen Sie wahrscheinlich keine verknüpften Ressourcen.

![][linked_resource]


[manual_scale]: ./media/cloud-services-how-to-scale/manual-scale.png
[queue_scale]: ./media/cloud-services-how-to-scale/queue-scale.png
[cpu_scale]: ./media/cloud-services-how-to-scale/cpu-scale.png
[tip_icon]: ./media/cloud-services-how-to-scale/tip.png
[scale_schedules]: ./media/cloud-services-how-to-scale/schedules.png
[scale_popup]: ./media/cloud-services-how-to-scale/schedules-dialog.png
[linked_resource]: ./media/cloud-services-how-to-scale/linked-resources.png

<!---HONumber=AcomDC_0608_2016-->
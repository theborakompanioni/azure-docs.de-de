<properties
	pageTitle="Problembehandlung bei der automatischen Skalierung von VM-Skalierungsgruppen | Microsoft Azure"
	description="Beheben Sie Probleme bei der automatischen Skalierung von VM-Skalierungsgruppen. Sie erhalten grundlegende Informationen zu typischen Problemen sowie zu ihrer Behebung."
	services="virtual-machine-scale-sets"
	documentationCenter=""
	authors="gbowerman"
	manager="timlt"
	editor=""
	tags="azure-resource-manager"/>

<tags
	ms.service="virtual-machine-scale-sets"
	ms.workload="na"
	ms.tgt_pltfrm="windows"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/28/2016"
	ms.author="guybo"/>
    
# Beheben von Problemen bei der automatischen Skalierung von VM-Skalierungsgruppen

**Problem:** Sie haben im Azure Resource Manager mithilfe von VM-Skalierungsgruppen eine Infrastruktur mit automatischer Skalierung erstellt, beispielsweise durch Bereitstellung der folgenden Vorlage: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-lapstack-autoscale. Sie haben die Skalierungsregeln festgelegt, und alles funktioniert einwandfrei, aber auch bei hoher Auslastung der virtuellen Computer erfolgt keine automatische Skalierung.

## Schritte zur Problembehandlung

Folgende Punkte sollten berücksichtigt werden:

- Über wie viele Kerne verfügt jeder virtuelle Computer, und laden Sie jeden Kern? Das oben genannte Beispiel einer Azure-Schnellstartvorlage enthält das Skript „do\_work.php“, das einen einzelnen Kern lädt. Wenn Sie einen größeren virtuellen Computer als Standard\_A1 verwenden, müssten Sie diese Last mehrmals ausführen. Überprüfen Sie die Anzahl der Kerne Ihrer virtuellen Computer. Lesen Sie dazu die Informationen unter [Größen für virtuelle Windows-Computer in Azure](../virtual-machines/virtual-machines-windows-sizes.md).

- Wie viele virtuelle Computer sind in der VM-Skalierungsgruppe enthalten? Wird auf jedem virtuellen Computer eine Last ausgeführt?

    Das horizontale Hochskalieren erfolgt nur, wenn die durchschnittliche CPU auf **allen** virtuellen Computer in einer Skalierungsgruppe den Schwellenwert in dem Zeitintervall überschreitet, das in den Regeln für die automatische Skalierung festgelegt ist.

- Haben Sie Skalierungsereignisse verpasst?

    Überprüfen Sie die Überwachungsprotokolle im Azure-Portal auf Skalierungsereignisse. Vielleicht wurde ein Vorgang zum zentralen Hochskalieren oder zum zentralen Herunterskalieren übersehen. Sie können nach „Scale“ (Skalierung) filtern.

	![Überwachungsprotokolle][audit]

- Liegen die Schwellenwerte für das horizontale Herunterskalieren und das horizontale Hochskalieren weit genug auseinander?

    Angenommen, Sie legen eine Regel mit folgenden Bedingungen fest: Wenn die durchschnittliche CPU-Auslastung mindestens fünf Minuten lang bei über 50 Prozent liegt, wird horizontal hochskaliert, bei einer durchschnittlichen CPU-Auslastung unter 50 Prozent wird horizontal herunterskaliert. Dies würde zu einem Fluktuationsproblem führen, wenn sich die CPU-Nutzung in der Nähe dieses Schwellenwerts bewegt, und die Gruppe würde durch die Skalierungsaktionen fortwährend vergrößert und verkleinert. Aus diesem Grund versucht der Dienst für die automatische Skalierung die Fluktuation zu verhindern, was dazu führen kann, dass keine Skalierung stattfindet. Stellen Sie daher sicher, dass die Schwellenwerte für das horizontale Hochskalieren und das horizontale Herunterskalieren weit genug auseinander liegen, um einen gewissen Skalierungsspielraum zu erhalten.

- Haben Sie Ihre eigene JSON-Vorlage geschrieben?

    Fehler können ganz leicht passieren. Beginnen Sie daher mit einer Vorlage wie der obigen, die sich bewährt hat, und nehmen Sie nach und nach kleine Änderungen vor. Die Vorlage muss ein Speicherkonto für die Diagnoseerweiterung, die Skalierungsgruppe und die Microsoft.Insights-Ressource korrelieren und korrekt auf den Namen der Leistungsdatenmetriken verweisen, der sich bei Windows und Linux unterscheidet.

- Können Sie manuell horizontal herunter- und hochskalieren?

    Versuchen Sie, die VM-Skalierungsgruppenressource mit einer anderen Kapazitätseinstellung erneut bereitzustellen, um die Anzahl der virtuellen Computer manuell zu ändern. Hier finden Sie eine Beispielvorlage dafür: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing. Sie müssen die Vorlage ggf. bearbeiten, damit sie die gleiche Computergröße wie die Skalierungsgruppe verwendet. Wenn Sie die Anzahl der virtuellen Computer manuell ändern können, wissen Sie, dass das Problem nur im Zusammenhang mit der automatischen Skalierung besteht.

- Überprüfen Sie im [Azure-Ressourcen-Explorer](https://resources.azure.com/) Ihre Angabe für „Microsoft.Compute/virtualMachineScaleSet“ sowie die Microsoft.Insights-Ressourcen.

    Dies ist ein unverzichtbares Tool für die Problembehandlung, das Aufschluss über den Zustand Ihrer Azure Resource Manager-Ressourcen gibt. Klicken Sie auf Ihr Abonnement, und sehen Sie sich die Ressourcengruppe an, für die Sie die Problembehandlung ausführen. Sehen Sie unter dem Compute-Ressourcenanbieter die von Ihnen erstellte VM-Skalierungsgruppe an, und überprüfen Sie die Instanzansicht, in der der Zustand einer Bereitstellung angezeigt wird. Überprüfen Sie außerdem die Instanzansicht der virtuellen Computer in der VM-Skalierungsgruppe. Wechseln Sie anschließend zum Microsoft.Insights-Ressourcenanbieter, und überprüfen Sie die Regeln für die automatische Skalierung auf ihre Richtigkeit.

- Funktioniert die Diagnoseerweiterung, und gibt sie Leistungsdaten aus?
 
    Die automatische Skalierung im Azure Resource Manager wird mithilfe der VM-Erweiterung „Diagnoseerweiterung“ (unterteilt in die Linux- und die Windows-Diagnoseerweiterung) ausgeführt. Sie gibt Leistungsdaten an ein Speicherkonto aus, das Sie in der Vorlage definieren. Diese Daten werden dann vom Azure Insights-Dienst aggregiert.

    Wenn der Insights-Dienst Daten aus den virtuellen Computern nicht lesen kann (etwa bei einem Ausfall der virtuellen Computer), sollten Sie eine E-Mail erhalten. Überprüfen Sie daher Ihren Posteingang (für die E-Mail-Adresse, die Sie beim Erstellen des Azure-Kontos angegeben haben).

    Sie können sich die Daten auch selbst ansehen. Sehen Sie sich das Azure-Speicherkonto in einem Cloud-Explorer an. Beispiel: Verwenden Sie den [Cloud-Explorer von Visual Studio](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), melden Sie sich an, und wählen Sie das verwendete Azure-Abonnement sowie den Namen des Diagnosespeicherkontos aus, auf den in der Diagnoseerweiterungsdefinition in Ihrer Bereitstellungsvorlage verwiesen wird.

	![Cloud-Explorer][explorer]

    Hier sehen Sie eine Reihe von Tabellen, in denen die Daten der einzelnen virtuellen Computer gespeichert werden. Sehen Sie sich beispielsweise die aktuellen Zeilen mit Linux- und CPU-Metriken an. Der Cloud-Explorer von Visual Studio unterstützt eine Abfragesprache. Sie können daher eine Abfrage wie etwa „Timestamp gt datetime’2016-02-02T21:20:00Z’“ ausführen, um sicherzustellen, dass die aktuellen Ereignisse abgerufen werden (hier wird davon ausgegangen, dass die Zeitzone UTC verwendet wird). Entsprechen die dort angezeigten Daten den von Ihnen eingerichteten Skalierungsregeln? Im folgenden Beispiel ist die CPU-Auslastung für Computer 20 in den letzten fünf Minuten auf 100 Prozent gestiegen.

	![Speichertabellen][tables]

    Wenn keine Daten vorhanden sind, impliziert dies, dass ein Problem mit der Diagnoseerweiterung auf den virtuellen Computern vorliegt. Sind Daten vorhanden, weist dies darauf hin, dass entweder ein Problem mit den Skalierungsregeln oder mit dem Insights-Dienst vorliegt. Überprüfen Sie den [Azure-Status](https://azure.microsoft.com/status/).

    Nachdem Sie diese Schritte ausgeführt haben, können Sie Informationen in den Foren auf [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp) oder [Stack Overflow](http://stackoverflow.com/questions/tagged/azure) suchen oder an den Support wenden. Stellen Sie bei Aufforderung die Vorlage und eine Ansicht der Leistungsdaten bereit.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png

<!---HONumber=AcomDC_0427_2016-->
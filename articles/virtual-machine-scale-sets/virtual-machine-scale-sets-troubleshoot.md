---
title: Problembehandlung bei der automatischen Skalierung von VM-Skalierungsgruppen | Microsoft Docs
description: Beheben Sie Probleme bei der automatischen Skalierung von VM-Skalierungsgruppen. Sie erhalten grundlegende Informationen zu typischen Problemen sowie zu ihrer Behebung.
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: windows
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: guybo
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: bd45a0fb99a77851aa7b91d23bd4b830b6f5cc7b
ms.lasthandoff: 03/31/2017


---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Beheben von Problemen bei der automatischen Skalierung von VM-Skalierungsgruppen
**Problem**: Sie haben im Azure Resource Manager mithilfe von VM-Skalierungsgruppen eine Infrastruktur mit automatischer Skalierung erstellt, beispielsweise durch Bereitstellung der folgenden Vorlage: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. Sie haben die Skalierungsregeln festgelegt, und alles funktioniert einwandfrei, aber auch bei hoher Auslastung der virtuellen Computer erfolgt keine automatische Skalierung.

## <a name="troubleshooting-steps"></a>Schritte zur Problembehandlung
Folgende Punkte sollten berücksichtigt werden:

* Über wie viele Kerne verfügt jeder virtuelle Computer, und laden Sie jeden Kern?
  Das oben genannte Beispiel einer Azure-Schnellstartvorlage enthält das Skript „do_work.php“, das einen einzelnen Kern lädt. Wenn Sie einen größeren virtuellen Computer als eine Größe mit einem Kern wie Standard_A1 oder D1 verwenden, müssten Sie diese Last mehrmals ausführen. Überprüfen Sie die Anzahl der Kerne Ihrer virtuellen Computer. Lesen Sie dazu die Informationen unter [Größen für virtuelle Windows-Computer in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Wie viele virtuelle Computer sind in der VM-Skalierungsgruppe enthalten? Wird auf jedem virtuellen Computer eine Last ausgeführt?
  
    Das horizontale Hochskalieren erfolgt nur, wenn die durchschnittliche CPU auf **allen** virtuellen Computer in einer Skalierungsgruppe den Schwellenwert in dem Zeitintervall überschreitet, das in den Regeln für die automatische Skalierung festgelegt ist.
* Haben Sie Skalierungsereignisse verpasst?
  
    Überprüfen Sie die Überwachungsprotokolle im Azure-Portal auf Skalierungsereignisse. Vielleicht wurde ein Vorgang zum zentralen Hochskalieren oder zum zentralen Herunterskalieren übersehen. Sie können nach „Scale“ (Skalierung) filtern.
  
    ![Überwachungsprotokolle][audit]
* Liegen die Schwellenwerte für das horizontale Herunterskalieren und das horizontale Hochskalieren weit genug auseinander?
  
    Angenommen, Sie legen eine Regel mit folgenden Bedingungen fest: Wenn die durchschnittliche CPU-Auslastung mindestens fünf Minuten lang bei über 50 Prozent liegt, wird horizontal hochskaliert, bei einer durchschnittlichen CPU-Auslastung unter 50 Prozent wird horizontal herunterskaliert. Dies würde zu einem Fluktuationsproblem führen, wenn sich die CPU-Nutzung in der Nähe dieses Schwellenwerts bewegt, und die Gruppe würde durch die Skalierungsaktionen fortwährend vergrößert und verkleinert. Aus diesem Grund versucht der Dienst für die automatische Skalierung die Fluktuation zu verhindern, was dazu führen kann, dass keine Skalierung stattfindet. Stellen Sie daher sicher, dass die Schwellenwerte für das horizontale Hochskalieren und das horizontale Herunterskalieren weit genug auseinander liegen, um einen gewissen Skalierungsspielraum zu erhalten.
* Haben Sie Ihre eigene JSON-Vorlage geschrieben?
  
    Fehler können ganz leicht passieren. Beginnen Sie daher mit einer Vorlage wie der obigen, die sich bewährt hat, und nehmen Sie nach und nach kleine Änderungen vor. 
* Können Sie manuell horizontal herunter- und hochskalieren?
  
    Versuchen Sie, die VM-Skalierungsgruppenressource mit einer anderen Kapazitätseinstellung erneut bereitzustellen, um die Anzahl der virtuellen Computer manuell zu ändern. Hier finden Sie eine Beispielvorlage dafür: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing. Sie müssen die Vorlage ggf. bearbeiten, damit sie die gleiche Computergröße wie die Skalierungsgruppe verwendet. Wenn Sie die Anzahl der virtuellen Computer manuell ändern können, wissen Sie, dass das Problem nur im Zusammenhang mit der automatischen Skalierung besteht.
* Überprüfen Sie im [Azure-Ressourcen-Explorer](https://resources.azure.com/)
  
    Dies ist ein unverzichtbares Tool für die Problembehandlung, das Aufschluss über den Zustand Ihrer Azure Resource Manager-Ressourcen gibt. Klicken Sie auf Ihr Abonnement, und sehen Sie sich die Ressourcengruppe an, für die Sie die Problembehandlung ausführen. Sehen Sie unter dem Compute-Ressourcenanbieter die von Ihnen erstellte VM-Skalierungsgruppe an, und überprüfen Sie die Instanzansicht, in der der Zustand einer Bereitstellung angezeigt wird. Überprüfen Sie außerdem die Instanzansicht der virtuellen Computer in der VM-Skalierungsgruppe. Wechseln Sie anschließend zum Microsoft.Insights-Ressourcenanbieter, und überprüfen Sie die Regeln für die automatische Skalierung auf ihre Richtigkeit.
* Funktioniert die Diagnoseerweiterung, und gibt sie Leistungsdaten aus?
  
    **Update**: Die automatische Skalierung in Azure wurde erweitert und verwendet jetzt eine hostbasierte Metrikpipeline, für die keine Diagnoseerweiterung mehr installiert werden muss. Das bedeutet, dass die nächsten Abschnitte nicht mehr zutreffen, wenn Sie mithilfe der neuen Pipeline eine Anwendung mit automatischer Skalierung erstellen. Ein Beispiel für Azure-Vorlagen, die konvertiert wurden, um die Hostpipeline zu verwenden: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Die Verwendung hostbasierter Metriken für die automatische Skalierung ist aus folgenden Gründen besser geeignet:
  
  * Es sind weniger Elemente zu berücksichtigen, da keine Diagnoseerweiterungen installiert werden müssen.
  * Die Vorlagen sind einfacher. Fügen Sie einfach einer vorhandenen Vorlage für Skalierungsgruppen Insights-Regeln für die automatische Skalierung hinzu.
  * Zuverlässigere Berichterstellung und schnellerer Start neuer virtueller Computer.
    
    Der einzige Grund, aus dem Sie möglicherweise weiterhin eine Diagnoseerweiterung verwenden möchten, liegt vor, wenn Sie Berichterstellungs-/Skalierungsfunktionen für Arbeitsspeicherdiagnosen benötigen. Hostbasierte Metriken erstellen keine Berichte zum Arbeitsspeicher.
    
    Sie müssen den Rest des Artikels nur lesen, wenn Sie weiteren Diagnoseerweiterungen für die automatische Skalierung verwenden.
    
    Die automatische Skalierung in Azure Resource Manager kann über eine VM-Erweiterung namens Diagnoseerweiterung funktionieren (dies muss aber nicht mehr der Fall sein). Sie gibt Leistungsdaten an ein Speicherkonto aus, das Sie in der Vorlage definieren. Diese Daten werden dann vom Azure Monitor-Dienst aggregiert.
    
    Wenn der Insights-Dienst Daten aus den virtuellen Computern nicht lesen kann (etwa bei einem Ausfall der virtuellen Computer), sollten Sie eine E-Mail erhalten. Überprüfen Sie daher Ihren Posteingang (für die E-Mail-Adresse, die Sie beim Erstellen des Azure-Kontos angegeben haben).
    
    Sie können sich die Daten auch selbst ansehen. Sehen Sie sich das Azure-Speicherkonto in einem Cloud-Explorer an. Beispiel: Verwenden Sie den [Cloud-Explorer von Visual Studio](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), melden Sie sich an, und wählen Sie das verwendete Azure-Abonnement sowie den Namen des Diagnosespeicherkontos aus, auf den in der Diagnoseerweiterungsdefinition in Ihrer Bereitstellungsvorlage verwiesen wird.
    
    ![Cloud-Explorer][explorer]
    
    Hier sehen Sie eine Reihe von Tabellen, in denen die Daten der einzelnen virtuellen Computer gespeichert werden. Sehen Sie sich beispielsweise die aktuellen Zeilen mit Linux- und CPU-Metriken an. Der Cloud-Explorer von Visual Studio unterstützt eine Abfragesprache. Sie können daher eine Abfrage wie etwa „Timestamp gt datetime’2016-02-02T21:20:00Z’“ ausführen, um sicherzustellen, dass die aktuellen Ereignisse abgerufen werden (hier wird davon ausgegangen, dass die Zeitzone UTC verwendet wird). Entsprechen die dort angezeigten Daten den von Ihnen eingerichteten Skalierungsregeln? Im folgenden Beispiel ist die CPU-Auslastung für Computer 20 in den letzten fünf Minuten auf 100 Prozent gestiegen.
    
    ![Speichertabellen][tables]
    
    Wenn keine Daten vorhanden sind, impliziert dies, dass ein Problem mit der Diagnoseerweiterung auf den virtuellen Computern vorliegt. Sind Daten vorhanden, weist dies darauf hin, dass entweder ein Problem mit den Skalierungsregeln oder mit dem Insights-Dienst vorliegt. Überprüfen Sie den [Azure-Status](https://azure.microsoft.com/status/).
    
    Wenn Sie diese Schritte ausgeführt haben und weiterhin Probleme mit der automatischen Skalierung auftreten, können Sie Informationen in den Foren auf [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp) oder [Stack Overflow](http://stackoverflow.com/questions/tagged/azure) suchen oder sich an den Support wenden. Stellen Sie bei Aufforderung die Vorlage und eine Ansicht der Leistungsdaten bereit.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png


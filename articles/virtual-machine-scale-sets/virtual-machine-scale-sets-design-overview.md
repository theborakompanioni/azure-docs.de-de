<properties
	pageTitle="Entwerfen von Skalierungsgruppen virtueller Computer für die Skalierung | Microsoft Azure"
	description="Erfahren Sie mehr über das Entwerfen von Skalierungsgruppen virtueller Computer für die Skalierung"
	keywords="virtueller Linux-Computer, Skalierungsgruppen für virtuelle Computer" 
	services="virtual-machine-scale-sets"
	documentationCenter=""
	authors="gatneil"
	manager="madhana"
	editor="tysonn"
	tags="azure-resource-manager" />

<tags
	ms.service="virtual-machine-scale-sets"
	ms.workload="na"
	ms.tgt_pltfrm="vm-linux"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/28/2016"
	ms.author="gatneil"/>

# Entwerfen von VM-Skalierungsgruppen für die Skalierung

In diesem Thema werden Überlegungen zum Entwurf von Skalierungsgruppen für virtuelle Computer erörtert. Informationen darüber, was Skalierungsgruppen für virtuelle Computer sind, finden Sie unter [Übersicht über VM-Skalierungsgruppen](virtual-machine-scale-sets-overview.md).


## Speicher

Eine Skalierungsgruppe verwendet Speicherkonten zum Speichern der Betriebssystem-Datenträger der virtuellen Computer in der Gruppe. Sie sollten ein Verhältnis von 20 VMs pro Speicherkonto oder weniger einsetzen. Außerdem sollten Sie für die Anfangszeichen der Speicherkontonamen verschiedene Buchstaben verwenden. Auf diese Weise lässt sich die Last leichter auf verschiedene interne Systeme verteilen. In der folgenden Vorlage verwenden wir z.B. die Resource Manager-Vorlagenfunktion uniqueString zum Generieren von Präfixhashwerten, die Speicherkontonamen vorangestellt werden: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## Überbereitstellung

Ab der API-Version „2016-03-30“ erfolgt die „Überbereitstellung“ virtueller Computer bei VM-Skalierungsgruppen standardmäßig. Wenn die Überbereitstellung aktiviert ist, richtet die Skalierungsgruppe tatsächlich mehr virtuelle Computer ein, als Sie angefordert haben, und löscht dann die zusätzlichen virtuellen Computer, die zuletzt erstellt wurden. Die Überbereitstellung verbessert die Erfolgsquoten bei der Bereitstellung. Diese zusätzlichen virtuellen Computer werden Ihnen nicht berechnet und nicht auf Ihre Kontingentgrenzen angerechnet.

Während die Überbereitstellung die Erfolgsquoten bei der Bereitstellung verbessert, kann sie ein verwirrendes Verhalten bei Anwendungen verursachen, die nicht dafür konzipiert sind, mit dem unangekündigten Verschwinden von virtuellen Computern umzugehen. Um die Überbereitstellung zu deaktivieren, stellen Sie sicher, dass Ihre Vorlage folgende Zeichenfolge enthält: "overprovision": "false". Weitere Einzelheiten finden Sie in der [REST-API-Dokumentation für VM-Skalierungsgruppen](https://msdn.microsoft.com/library/azure/mt589035.aspx).

Wenn Sie die Überbereitstellung deaktivieren, erhalten Sie ein größeres Verhältnis von VMs pro Speicherkonto, aber Sie sollten nicht über 40 gehen.


## Grenzen
Eine Skalierungsgruppe, die auf der Grundlage eines (von Ihnen erstellten) benutzerdefinierten Images erstellt wird, muss alle Betriebssystem-Datenträger-VHDs in einem einzelnen Speicherkonto erstellen. Folglich ist 20 die maximal empfohlene Anzahl von virtuellen Computern in einer Skalierungsgruppe, die auf einem benutzerdefinierten Image basiert. Wenn Sie die Überbereitstellung deaktivieren, können Sie bis zu 40 gehen.

Eine Skalierungsgruppe, die auf der Grundlage eines Plattformimages erstellt wird, ist derzeit auf 100 virtuelle Computer beschränkt. (Wir empfehlen fünf Speicherkonten für diese Skalierung.)

Wenn die Anzahl benötigter virtueller Computer über diese Grenzwerte hinausgeht, müssen Sie mehrere Skalierungsgruppen bereitstellen, wie in dieser [Vorlage](https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale) gezeigt.

<!---HONumber=AcomDC_0817_2016-->
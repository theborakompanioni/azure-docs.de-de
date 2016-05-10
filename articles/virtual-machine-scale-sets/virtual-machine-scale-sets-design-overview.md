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
	ms.date="04/01/2016"
	ms.author="gatneil"/>

# Entwerfen von VM-Skalierungsgruppen für die Skalierung

In diesem Thema werden Überlegungen zum Entwurf von Skalierungsgruppen für virtuelle Computer erörtert. Informationen darüber, was Skalierungsgruppen für virtuelle Computer sind, finden Sie unter [Virtual Machine Scale Sets Overview](virtual-machine-scale-sets-overview.md) (Übersicht über Skalierungsgruppen für virtuelle Computer).


## Speicher

Eine Skalierungsgruppe verwendet Speicherkonten zum Speichern der Betriebssystem-Datenträger der virtuellen Computer in der Gruppe. Sie sollten ein Verhältnis von 20 VMs pro Speicherkonto oder weniger einsetzen. Außerdem sollten Sie für die Anfangszeichen der Speicherkontonamen verschiedene Buchstaben verwenden. Dann lässt sich die Last leichter über unterschiedliche interne Systeme verteilen. In der folgenden Vorlage verwenden wir z.B. die uniqueString-ARM-Vorlagenfunktion zum Generieren von Präfixhashwerten, die Speicherkontonamen vorangestellt werden: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat).


## Überbereitstellung

Ab der 2016-03-30-API-Version ist die „Überbereitstellung“ virtueller Computer bei VM-Skalierungsgruppen standardmäßig. Dies bedeutet, dass die Skalierungsgruppe tatsächlich mehr virtuelle Computer bereitstellt, als Sie angefordert haben, statt unnötige VMs zu löschen. Dies verbessert die Erfolgsraten bei der Bereitstellung, da die gesamte Bereitstellung von Azure Resource Manager sogar dann als „fehlgeschlagen“ betrachtet wird, wenn ein virtueller Computer nicht erfolgreich bereitgestellt wird. Diese zusätzlichen virtuellen Computer werden Ihnen nicht berechnet und nicht auf Ihre Kontingentgrenzen angerechnet.

Während dies die Erfolgsraten bei der Bereitstellung verbessert, kann es ein verwirrendes Verhalten bei Anwendungen verursachen, die nicht dafür konzipiert sind, mit dem unangekündigten Verschwinden von VMs umzugehen. Um die Überbereitstellung zu deaktivieren, stellen Sie bitte sicher, dass Ihre Vorlage folgende Zeichenfolge enthält: „overprovision“: false.

Wenn Sie die Überbereitstellung deaktivieren, erhalten Sie ein größeres Verhältnis von VMs pro Speicherkonto, aber Sie sollten nicht über 40 gehen.


## Grenzen (Plattform im Vergleich zu benutzerdefiniert)
Eine Skalierungsgruppe, die auf einem Plattformimage erstellt wird, ist auf 100 virtuelle Computer beschränkt, und wir empfehlen 5 Speicherkonten für diese Skalierung. Aber eine Skalierungsgruppe, die auf einem (von Ihnen erstellten) benutzerdefinierten Image erstellt wird, muss alle Betriebssystem-Datenträger-VHDs in einem Speicherkonto erstellen. Folglich ist 20 die maximale Anzahl von virtuellen Computern in einer Skalierungsgruppe, die auf einem benutzerdefinierten Image erstellt wird. Wenn Sie die Überbereitstellung deaktivieren, können Sie bis zu 40 gehen.


## Großer Maßstab

Eine Skalierungsgruppe ist auf 100 virtuelle Computer für Plattformimages, 20 für benutzerdefinierte Images (40 ohne Überbereitstellung) beschränkt. Wenn Sie weitere wünschen, müssen Sie mehrere Skalierungsgruppen bereitstellen. Ein Beispiel hierzu finden Sie in dieser Vorlage: https://github.com/Azure/azure-quickstart-templates/tree/master/301-custom-images-at-scale.

<!---HONumber=AcomDC_0427_2016-->
---
title: Zugreifen auf virtuelle Azure-Computer über den Server-Explorer | Microsoft Docs
description: In diesem Artikel erhalten Sie einen Überblick darüber, wie virtuelle Azure-Computer im Server-Explorer von Visual Studio angezeigt, erstellt und verwaltet werden können.
services: visual-studio-online
documentationcenter: na
author: TomArcher
manager: douge
editor: ''

ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 08/15/2016
ms.author: tarcher

---
# Zugreifen auf virtuelle Computer in Azure über den Server-Explorer
Mit dem Server-Explorer in Visual Studio können Sie Informationen über die von Azure gehosteten virtuellen Computer anzeigen.

## Zugreifen auf virtuelle Computer im Server-Explorer
Wenn Sie in Azure gehostete virtuelle Computer verwenden, können Sie über den Server-Explorer auf diese Computer zugreifen. Sie müssen sich zunächst bei Ihrem Azure-Abonnement anmelden, um die mobilen Dienste anzeigen zu können. Öffnen Sie das Kontextmenü für den Azure-Knoten, und wählen Sie **Verbindung mit Microsoft Azure-Abonnement herstellen** aus, um sich anzumelden.

### So rufen Sie Informationen zu Ihren virtuellen Computern ab
1. Wählen Sie im Server-Explorer einen virtuellen Computer aus, und drücken Sie anschließend F4, um das Eigenschaftenfenster für den Computer anzuzeigen.
   
    Die folgende Tabelle zeigt, welche Eigenschaften verfügbar sind – diese jedoch alle schreibgeschützt. Sie können sie über das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) ändern.
   
   | Eigenschaft | Beschreibung |
   | --- | --- |
   | DNS-Name |Die URL mit der Internetadresse des virtuellen Computers. |
   | Umgebung |Für einen virtuellen Computer lautet der Wert dieser Eigenschaft immer "Produktion". |
   | Name |Der Name des virtuellen Computers. |
   | Größe |Die Größe des virtuellen Computers. Diese Eigenschaft gibt die Menge an verfügbarem Arbeitsspeicher und Festplattenspeicher an. Weitere Informationen finden Sie unter "Konfigurieren von Größen für Clouddienste". |
   | Status |Mögliche Werte lauten "Wird gestartet", "Gestartet", "Wird angehalten", "Angehalten" und "Status wird abgerufen". Wird "Status wird abgerufen" angezeigt, ist der aktuelle Status unbekannt. Die Werte für diese Eigenschaft unterscheiden sich von den Werten, die im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) verwendet werden. |
   | SubscriptionID |Die Abonnement-ID für Ihr Azure-Konto. Sie können diese Informationen im [klassischen Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885) anzeigen, indem Sie die Eigenschaften für ein Abonnement öffnen. |
2. Wählen Sie einen Endpunktknoten aus, und zeigen Sie dann das Fenster **Eigenschaften** an.
3. Die folgende Tabelle beschreibt die verfügbaren Endpunkteigenschaften. Alle Eigenschaften sind schreibgeschützt. Verwenden Sie zum Hinzufügen oder Bearbeiten der Endpunkte für einen virtuellen Computer das [klassische Azure-Portal](http://go.microsoft.com/fwlink/?LinkID=213885).
   
   | Eigenschaft | Beschreibung |
   | --- | --- |
   | Name |Ein Bezeichner für den Endpunkt. |
   | Privater Port |Der Port für den internen Netzwerkzugriff auf Ihre Anwendung. |
   | Protocol |Das Protokoll, das von der Transportschicht für diesen Endpunkt verwendet wird – entweder TCP oder UDP. |
   | Öffentlicher Port |Der Port, der für den öffentlichen Zugang auf Ihrer Anwendung verwendet wird. |

## Nächste Schritte
Weitere Informationen zum Verwenden von Azure-Rollen in Visual Studio finden Sie unter [Verwenden von Remotedesktop mit Azure-Rollen](vs-azure-tools-remote-desktop-roles.md).

<!---HONumber=AcomDC_0817_2016-->
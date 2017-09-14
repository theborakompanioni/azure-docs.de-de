---
title: "Zugreifen auf virtuelle Azure-Computer über den Server-Explorer | Microsoft Docs"
description: "In diesem Artikel erhalten Sie einen Überblick darüber, wie virtuelle Azure-Computer im Server-Explorer von Visual Studio angezeigt, erstellt und verwaltet werden können."
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: eb3afde6-ba90-4308-9ac1-3cc29da4ede0
ms.service: multiple
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: multiple
ms.date: 8/31/2017
ms.author: kraigb
ms.translationtype: HT
ms.sourcegitcommit: 3eb68cba15e89c455d7d33be1ec0bf596df5f3b7
ms.openlocfilehash: 75dcc603327b50718b279f3ce055663ec0bc2596
ms.contentlocale: de-de
ms.lasthandoff: 09/01/2017

---
# <a name="accessing-azure-virtual-machines-from-server-explorer"></a>Zugreifen auf virtuelle Computer in Azure über den Server-Explorer
Mit dem Server-Explorer in Visual Studio können Sie Informationen über die von Azure gehosteten virtuellen Computer anzeigen.

## <a name="accessing-virtual-machines-in-server-explorer"></a>Zugreifen auf virtuelle Computer im Server-Explorer
Wenn Sie in Azure gehostete virtuelle Computer verwenden, können Sie über den Server-Explorer auf diese Computer zugreifen. Sie müssen sich zunächst bei Ihrem Azure-Abonnement anmelden, um die mobilen Dienste anzeigen zu können. Öffnen Sie das Kontextmenü für den Azure-Knoten, und wählen Sie **Verbindung mit Microsoft Azure-Abonnement herstellen**aus, um sich anzumelden.

### <a name="to-get-information-about-your-virtual-machines"></a>So rufen Sie Informationen zu Ihren virtuellen Computern ab
1. Wählen Sie im Cloud-Explorer einen virtuellen Computer aus, und drücken Sie anschließend die F4-Taste, um das Eigenschaftenfenster für den Computer anzuzeigen.
   
    Die folgende Tabelle zeigt, welche Eigenschaften verfügbar sind – diese jedoch alle schreibgeschützt. Sie können sie über das [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) ändern.
   
   | Eigenschaft | Beschreibung |
   | --- | --- |
   | DNS-Name |Die URL mit der Internetadresse des virtuellen Computers. |
   | Umgebung |Für einen virtuellen Computer lautet der Wert dieser Eigenschaft immer "Produktion". |
   | Name |Der Name des virtuellen Computers. |
   | Größe |Die Größe des virtuellen Computers. Diese Eigenschaft gibt die Menge an verfügbarem Arbeitsspeicher und Festplattenspeicher an. Weitere Informationen finden Sie unter [Größen virtueller Computer](https://docs.microsoft.com/azure/cloud-services/cloud-services-sizes-specs). |
   | Status |Mögliche Werte lauten "Wird gestartet", "Gestartet", "Wird angehalten", "Angehalten" und "Status wird abgerufen". Wird "Status wird abgerufen" angezeigt, ist der aktuelle Status unbekannt. Die Werte für diese Eigenschaft unterscheiden sich von den Werten, die im [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) verwendet werden. |
   | SubscriptionID |Die Abonnement-ID für Ihr Azure-Konto. Sie können diese Informationen im [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) anzeigen, indem Sie die Eigenschaften für ein Abonnement öffnen. |
2. Wählen Sie einen Endpunktknoten aus, und zeigen Sie dann das Fenster **Eigenschaften** an.
3. Die folgende Tabelle beschreibt die verfügbaren Endpunkteigenschaften. Alle Eigenschaften sind schreibgeschützt. Verwenden Sie zum Hinzufügen oder Bearbeiten der Endpunkte für einen virtuellen Computer das [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040). 
   
   | Eigenschaft | Beschreibung |
   | --- | --- |
   | Name |Ein Bezeichner für den Endpunkt. |
   | Privater Port |Der Port für den internen Netzwerkzugriff auf Ihre Anwendung. |
   | Protocol |Das Protokoll, das von der Transportschicht für diesen Endpunkt verwendet wird – entweder TCP oder UDP. |
   | Öffentlicher Port |Der Port, der für den öffentlichen Zugang auf Ihrer Anwendung verwendet wird. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Verwenden von Azure-Rollen in Visual Studio finden Sie unter [Verwenden von Remotedesktop mit Azure-Rollen](vs-azure-tools-remote-desktop-roles.md).



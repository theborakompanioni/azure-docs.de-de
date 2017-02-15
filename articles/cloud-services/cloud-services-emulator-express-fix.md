---
title: Einrichten von Emulator Express zum Debuggen von Cloud Services-Anwendungen in Visual Studio | Microsoft Docs
description: Es wird beschrieben, wie Sie C++ Redistributable installieren, um Emulator Express in Visual Studio zu aktivieren.
services: cloud-services
documentationcenter: 
author: cawa
manager: paulyuk
editor: 
ms.assetid: 22b20f7a-23f4-4f7f-b536-3bf1e01adcd1
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 11/02/2016
ms.author: cawa
translationtype: Human Translation
ms.sourcegitcommit: 165ab7363efaf90eaab41098f71e2f1b846c346e
ms.openlocfilehash: 05d672dcb1335c617bb8d8cae43947bcd5e9ab3d

---
# <a name="use-emulator-express-to-debug-cloud-services-application-in-vs-2017"></a>Verwenden von Emulator Express zum Debuggen der Cloud Services-Anwendung in VS 2017
In diesem Artikel wird beschrieben, wie Sie Emulator Express zum Debuggen von Cloud Services-Anwendungen in VS 2017 verwenden.

## <a name="background-context"></a>Hintergrundkontext
Emulator Express wird standardmäßig zum Debuggen von Cloud Services-Web- und -Workerrollen in Visual Studio verwendet. Die Einstellung wird auf der Seite mit den Cloud Services-Projekteigenschaften angegeben.

![Öffnen der Projekteigenschaften][0]

![Emulator Express ist standardmäßig ausgewählt.][1]

[Visual C++ Redistributable][Visual C++ Redistributable] für Visual Studio ist für Emulator Express erforderlich. Derzeit wird diese Komponente nicht zusammen mit der Azure-Workload installiert. Bei Verwendung von F5 zum Debuggen einer Cloud Services-Anwendung wird in Visual Studio eine Aufforderung zum Installieren dieser Komponente angezeigt und der Debugvorgang fortgesetzt.

![Aufforderung zum Installieren von C++ Redistributable][2]

Klicken Sie auf „Ja“, um C++ Redistributable zu installieren.

![Installieren von C++ Redistributable][3]

Drücken Sie erneut F5, um Debugsitzungen zu starten.

![Starten des Debugvorgangs][4]

![Debuggen erfolgreich][5]

> Hinweis: Visual C++ Redistributable muss nur einmal installiert werden. Wenn Sie ein Upgrade von einer älteren Version des Azure SDK durchführen und Emulator Express installiert haben, tritt dieses Problem nicht auf.
> 
> 

## <a name="manual-workaround"></a>Manuelle Problemumgehung
Sie können [Visual C++ Redistributable][Visual C++ Redistributable] manuell installieren, um den gleichen Effekt zu erzielen, als wenn die Installation von Visual Studio auf Ihrem System durchgeführt worden wäre.

[vcredist_x86.exe][vcredist_x86.exe]

[vcredist_x64.exe][vcredist_x64.exe]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung des Azure-Computeremulators zum Debuggen Ihrer Cloud Services-Anwendungen in Visual Studio finden Sie unter [Verwenden von Emulator Express zum Ausführen und Debuggen eines Clouddiensts auf einem lokalen Computer][Verwenden von Emulator Express zum Ausführen und Debuggen eines Clouddiensts auf einem lokalen Computer].

[Visual C++ Redistributable]:https://www.microsoft.com/en-us/download/details.aspx?id=30679
[vcredist_x86.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x86.exe
[vcredist_x64.exe]:https://download.microsoft.com/download/1/6/B/16B06F60-3B20-4FF2-B699-5E9B7962F9AE/VSU_4/vcredist_x64.exe
[Verwenden von Emulator Express zum Ausführen und Debuggen eines Clouddiensts auf einem lokalen Computer]:https://azure.microsoft.com/en-us/documentation/articles/vs-azure-tools-emulator-express-debug-run/

[0]: ./media/cloud-services-emulator-express-fix/vs-05.png
[1]: ./media/cloud-services-emulator-express-fix/vs-06.png
[2]: ./media/cloud-services-emulator-express-fix/vs-01.png
[3]: ./media/cloud-services-emulator-express-fix/vs-02.png
[4]: ./media/cloud-services-emulator-express-fix/vs-03.png
[5]: ./media/cloud-services-emulator-express-fix/vs-04.png



<!--HONumber=Nov16_HO3-->



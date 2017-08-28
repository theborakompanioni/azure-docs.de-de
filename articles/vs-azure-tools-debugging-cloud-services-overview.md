---
title: Optionen zum Debuggen von Azure-Clouddiensten | Microsoft Docs
description: Debuggen von Azure-Clouddiensten
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 80755da7-8350-4f5c-97ce-2962beabb36d
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/18/2017
ms.author: kraigb
ms.translationtype: Human Translation
ms.sourcegitcommit: 424d8654a047a28ef6e32b73952cf98d28547f4f
ms.openlocfilehash: e79aa1db6c89ad1e382c7a70017f868fe186aa74
ms.contentlocale: de-de
ms.lasthandoff: 03/22/2017

---
# <a name="learn-the-various-ways-to-debug-an-azure-cloud-service"></a>Lernen Sie die verschiedenen Möglichkeiten zum Debuggen eines Azure-Clouddiensts kennen.
Dieser Artikel enthält Links zu verschiedenen Methoden zum Debuggen eines Azure-Clouddiensts. 

## <a name="debugging-an-azure-cloud-service-in-visual-studio"></a>Debuggen eines Azure-Clouddiensts in Visual Studio
Sie können Zeit und Geld sparen, wenn Sie den Azure-Compute-Emulator zum Debuggen Ihres Clouddiensts auf einem lokalen Computer verwenden. Durch lokales Debuggen eines Diensts vor der Bereitstellung können Sie die Zuverlässigkeit und Leistung verbessern, ohne für die Computezeit bezahlen zu müssen. Allerdings können einige Fehler nur auftreten, wenn Sie einen Clouddienst in Azure ausführen. Fehler, die nur beim Ausführen eines Clouddiensts in Azure auftreten, lassen sich debuggen, indem das Remotedebuggen beim Veröffentlichen des Diensts aktiviert und der Debugger dann an eine Rolleninstanz angefügt wird. Weitere Informationen finden Sie unter [Debuggen des Clouddiensts auf dem lokalen Computer](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-your-cloud-service-on-your-local-computer).

## <a name="using-azure-diagnostics"></a>Verwenden von Azure-Diagnose 
Sie können die Azure-Diagnose verwenden, um ausführliche Informationen zu Code, der in Rollen ausgeführt wird, zu protokollieren, unabhängig davon, ob die Rollen in der Entwicklungsumgebung oder in Azure ausgeführt werden. Weitere Informationen finden Sie unter [Aktivieren der Azure-Diagnose in Azure Cloud Services](http://go.microsoft.com/fwlink/p/?LinkId=400450).

## <a name="using-intellitrace"></a>Verwenden von IntelliTrace 
Wenn Sie Visual Studio Enterprise zum Schreiben von Rollen verwenden, die auf .NET Framework 4.5 ausgerichtet sind, können Sie IntelliTrace aktivieren, sobald Sie einen Clouddienst über Visual Studio bereitstellen. IntelliTrace bietet ein Protokoll, das Sie mit Visual Studio verwenden können, um die Anwendung so zu debuggen, als ob sie in Azure ausgeführt wird. Weitere Informationen finden Sie unter [Debuggen eines veröffentlichten Clouddiensts mit IntelliTrace und Visual Studio](http://go.microsoft.com/fwlink/p/?LinkId=623016).

## <a name="remote-debugging"></a>Remotedebuggen 
Sie können das Remotedebuggen für Ihre Clouddienste zu dem Zeitpunkt aktivieren, zu dem Sie den Clouddienst über Visual Studio bereitstellen. Wenn Sie das Remotedebuggen für eine Bereitstellung aktivieren möchten, werden Dienste zum Remotedebuggen auf den virtuellen Computern installiert, auf denen jede Rolleninstanz ausgeführt wird. Diese Dienste, z.B. `msvsmon.exe`, haben keine Auswirkungen auf die Leistung und führen nicht zu zusätzlichen Kosten. Weitere Informationen finden Sie unter [Debuggen eines Clouddiensts in Azure](vs-azure-tools-debug-cloud-services-virtual-machines.md#debug-a-cloud-service-in-azure).

## <a name="next-steps"></a>Nächste Schritte
- [Debuggen eines Azure-Clouddiensts oder virtuellen Computers in Visual Studio:](./vs-azure-tools-debug-cloud-services-virtual-machines.md) Erfahren Sie Näheres zum Debuggen von Azure-Clouddiensten.

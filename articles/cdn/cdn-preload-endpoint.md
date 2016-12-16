---
title: Vorabladen von Assets auf einen Azure CDN-Endpunkt | Microsoft Docs
description: Erfahren Sie, wie zwischengespeicherten Inhalt auf einen CDN-Endpunkt vorab laden.
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 5ea3eba5-1335-413e-9af3-3918ce608a83
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2016
ms.author: casoper
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e54ab1d1e1d363f92c05772a5367b28c46fe4038


---
# <a name="pre-load-assets-on-an-azure-cdn-endpoint"></a>Vorabladen von Assets auf einen Azure CDN-Endpunkt
[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

Standardmäßig werden Assets erst zwischengespeichert, nachdem sie angefordert wurden. Das heißt, dass die erste Anforderung aus den einzelnen Regionen länger dauern kann, da der Inhalt nicht auf den Edgeservern zwischengespeichert ist, die deshalb Anforderungen an den Ursprungsserver weiterleiten müssen. Durch Vorabladen von Inhalten kann die Latenz beim ersten Treffer vermieden werden.

Abgesehen vom Ermöglichen einer besseren Kundenerfahrung kann durch das Vorabladen zwischengespeicherter Inhalte auch der Netzwerkdatenverkehr auf dem Ursprungsserver reduziert werden.

> [!NOTE]
> Das Vorabladen von Assets ist nützlich für große Ereignisse oder Inhalte, die für eine große Anzahl von Benutzern gleichzeitig zur Verfügung gestellt werden, wie z.B. bei Veröffentlichung eines neuen Films oder Softwareupdates.
> 
> 

In diesem Tutorial wird Schritt für Schritt erläutert, wie Sie zwischengespeicherten Inhalt auf alle Azure CDN-Edgeknoten vorab laden.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
1. Navigieren Sie im [Azure-Portal](https://portal.azure.com)zum CDN-Profil mit dem Endpunkt, auf den Sie Inhalt vorab laden möchten.  Das Profilblatt wird angezeigt.
2. Klicken Sie in der Liste auf den Endpunkt.  Das Blatt für den Endpunkt wird angezeigt.
3. Klicken Sie im Blatt für den CDN-Endpunkt auf die Schaltfläche „Laden“.
   
    ![Blatt für CDN-Endpunkt](./media/cdn-preload-endpoint/cdn-endpoint-blade.png)
   
    Das Blatt „Laden“ wird angezeigt.
   
    ![Blatt „Laden“ für CDN](./media/cdn-preload-endpoint/cdn-load-blade.png)
4. Geben Sie in das Textfeld **Pfad** den vollständigen Pfad jedes Assets ein, das Sie laden möchten (z.B. `/pictures/kitten.png`).
   
   > [!TIP]
   > Nachdem Sie Text eingegeben haben, werden weitere **Pfad** -Textfelder angezeigt, damit Sie eine Liste mit mehreren Assets erstellen können.  Sie können Assets aus der Liste löschen, indem Sie auf die Schaltfläche mit den Auslassungspunkten (...) klicken.
   > 
   > Pfade müssen eine relative URL enthalten, die dem folgenden [regulären Ausdruck](https://msdn.microsoft.com/library/az24scfc.aspx) entspricht: `^(?:\/[a-zA-Z0-9-_.\u0020]+)+$`.  Jedes Objekt muss einen eigenen Pfad haben.  Platzhalter werden beim Vorabladen von Assets nicht unterstützt.
   > 
   > 
   
    ![Schaltfläche „Laden“](./media/cdn-preload-endpoint/cdn-load-paths.png)
5. Klicken Sie auf die Schaltfläche **Laden** .
   
    ![Schaltfläche „Laden“](./media/cdn-preload-endpoint/cdn-load-button.png)

> [!NOTE]
> Es gilt eine Einschränkung von 10 Anforderungen zum Laden pro Minute pro CDN-Profil.
> 
> 

## <a name="see-also"></a>Weitere Informationen
* [Löschen eines Azure CDN-Endpunkts](cdn-purge-endpoint.md)
* [Azure CDN-REST-API-Referenz – Löschen oder Vorabladen eines Endpunkts](https://msdn.microsoft.com/library/mt634451.aspx)




<!--HONumber=Nov16_HO3-->



---
title: "Löschen eines Azure CDN-Endpunkts | Microsoft Docs"
description: "Erfahren Sie, wie alle zwischengespeicherten Inhalte aus einem CDN-Endpunkt gelöscht werden."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 0b50230b-fe82-4740-90aa-95d4dde8bd4f
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/28/2016
ms.author: casoper
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: bacc40df27e01ac33a032720efc27e68c35ab8d0


---
# <a name="purge-an-azure-cdn-endpoint"></a>Löschen eines Azure CDN-Endpunkts
## <a name="overview"></a>Übersicht
Azure CDN-Edgeknoten speichern Assets zwischen, bis die Gültigkeitsdauer (Time-to-live, TTL) des Assets abläuft.  Wenn ein Client nach Ablauf der TTL des Assets das Asset aus dem Edgeknoten anfordert, ruft der Edgeknoten eine neue aktualisierte Kopie des Assets ab, um die Clientanforderung zu erfüllen und den Cache zu aktualisieren.

Manchmal möchten Sie möglicherweise zwischengespeicherten Inhalt aus allen Edgeknoten löschen und sie zwingen, neue aktualisierte Assets abzurufen.  Als Gründe hierfür kommen z. B. Updates Ihrer Webanwendung oder schnelle Aktualisierung von Assets, die falsche Informationen enthalten, infrage.

> [!TIP]
> Beachten Sie, dass das Löschen nur den zwischengespeicherten Inhalt auf den CDN-Edgeservern entfernt.  Alle Downstreamcaches, z.B. Proxyserver und lokale Browsercaches, können weiterhin eine zwischengespeicherte Kopie der Datei enthalten.  Denken Sie daran, wenn Sie die Gültigkeitsdauer einer Datei festlegen.  Sie können erzwingen, dass ein Downstreamclient die neueste Version der Datei anfordert, indem Sie ihr bei jeder Aktualisierung einen eindeutigen Namen geben, oder indem Sie das [Zwischenspeichern von Zeichenfolgen](cdn-query-string.md) nutzen.  
> 
> 

Dieses Lernprogramm führt Sie durch das Löschen von Assets aus allen Edgeknoten eines Endpunkts.

## <a name="walkthrough"></a>Exemplarische Vorgehensweise
1. Navigieren Sie im [Azure-Portal](https://portal.azure.com)zu dem CDN-Profil mit dem Endpunkt, den Sie löschen möchten.
2. Klicken Sie auf dem CDN-Profilblatt auf die Schaltfläche zum Löschen.
   
    ![CDN-Profilblatt](./media/cdn-purge-endpoint/cdn-profile-blade.png)
   
    Das Blatt „Löschen“ wird geöffnet.
   
    ![CDN-Löschblatt](./media/cdn-purge-endpoint/cdn-purge-blade.png)
3. Wählen Sie auf dem Blatt „Löschen“ die Dienstadresse, die Sie in der URL-Dropdownliste löschen möchten.
   
    ![Löschformular](./media/cdn-purge-endpoint/cdn-purge-form.png)
   
   > [!NOTE]
   > Um das Blatt „Löschen“ aufzurufen, können Sie auch auf dem Blatt des CDN-Endpunkts auf die Schaltfläche **Löschen** klicken.  In diesem Fall ist im **URL** -Feld die Adresse des Diensts dieses Endpunkts vorgegeben.
   > 
   > 
4. Wählen Sie, welche Assets Sie aus dem Edgeknoten löschen möchten.  Wenn Sie alle Assets löschen möchten, klicken Sie auf das Kontrollkästchen **Alles löschen**.  Geben Sie andernfalls den vollständigen Pfad jedes Assets, das Sie löschen möchten (z.B. `/pictures/kitten.png`), in das Textfeld **Pfad** ein.
   
   > [!TIP]
   > Nachdem Sie Text eingegeben haben, werden weitere **Pfad** -Textfelder angezeigt, damit Sie eine Liste mit mehreren Assets erstellen können.  Sie können Assets aus der Liste löschen, indem Sie auf die Schaltfläche mit den Auslassungspunkten (...) klicken.
   > 
   > Pfade müssen eine relative URL enthalten, die dem folgenden [regulären Ausdruck](https://msdn.microsoft.com/library/az24scfc.aspx) entspricht: `^\/(?:[a-zA-Z0-9-_.\u0020]+\/)*\*$";`.  Für **Azure CDN von Verizon** (Standard oder Premium) kann das Sternchen (\*) als Platzhalter verwendet werden (z.B. `/music/*`).  Platzhalter und **Alles löschen** sind bei **Azure CDN von Akamai** nicht erlaubt.
   > 
   > 
5. Klicken Sie auf die Schaltfläche **Löschen** .
   
    ![Löschschaltfläche](./media/cdn-purge-endpoint/cdn-purge-button.png)

> [!IMPORTANT]
> Die Bearbeitung von Löschanforderungen dauert mit **Azure CDN von Verizon** (Standard oder Premium) etwa 2 bis 3 Minuten und mit **Azure CDN von Akamai** ca. 7 Minuten.  Die Anzahl gleichzeitiger Löschanforderungen ist bei Azure CDN auf 50 begrenzt. 
> 
> 

## <a name="see-also"></a>Siehe auch
* [Vorabladen von Assets auf einen Azure CDN-Endpunkt](cdn-preload-endpoint.md)
* [Azure CDN-REST-API-Referenz – Löschen oder Vorabladen eines Endpunkts](https://msdn.microsoft.com/library/mt634451.aspx)




<!--HONumber=Nov16_HO3-->



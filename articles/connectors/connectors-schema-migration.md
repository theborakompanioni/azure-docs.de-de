---
title: Migrieren von Logik-Apps zur Schemaversion 2015-08-01-preview | Microsoft Docs
description: "Sie können Logik-Apps einfach zur neuesten Schemaversion migrieren: Führen Sie einfach die folgenden Schritte aus:"
services: logic-apps
documentationcenter: 
author: MSFTMAN
manager: erikre
editor: 
tags: connectors
ms.assetid: 3e177e49-fd69-43e9-9b9b-218abb250c31
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/23/2016
ms.author: deonhe
translationtype: Human Translation
ms.sourcegitcommit: 30abc823a72431dfd12f1051434191ea904533f6
ms.openlocfilehash: a5a73a9f124e5339b61dbc49021444a208a471f0


---
# <a name="how-to-migrate-logic-apps-to-schema-version-2015-08-01-preview"></a>Gewusst wie: Migrieren von Logik-Apps zur Schemaversion 2015-08-01-preview
Um Ihre vorhandenen Logik-Apps in das neue Schema zu verschieben, führen Sie folgende Schritte aus:  

1. Öffnen Sie Ihre Logik-App im Azure-Portal.  
2. Klicken Sie auf „Schema aktualisieren“.
   
   ![API-Symbol][step1]   
   Die Seite „Schema aktualisieren“ wird angezeigt. Sie enthält einen Link zu einem Dokument, das Details zu den Verbesserungen im neuen Schema bereitstellt: ![API-Symbol][step2].

> [!NOTE]
> Wenn Sie die Option **Schema aktualisieren** auswählen, führen wir automatisch die Migrationsschritte aus und stellen die Codeausgabe für Sie bereit. Hiermit können Sie Ihre Definition aktualisieren. Halten Sie sich bei der Codeerstellung jedoch an bewährte Verfahren, wie sie beispielsweise weiter unten im Abschnitt **Bewährte Methoden** beschrieben werden.
> 
> 

## <a name="best-practices-when-migrating-your-logic-apps-to-the-latest-schema-version"></a>Bewährte Methoden beim Migrieren von Logik-Apps zur neuesten Schemaversion:
* Kopieren Sie das migrierte Skript in eine neue Logik-App – Überschreiben Sie nicht die alte App, bis Sie Ihre Tests abgeschlossen und bestätigt haben, dass die migrierte App wie erwartet funktioniert.
* Testen Sie Ihre Logik-App, **bevor** sie in Produktion geht.
* Beginnen Sie nach Abschluss der Migration mit der Aktualisierung Ihrer Logik-Apps, um nach Möglichkeit die [verwalteten APIs](apis-list.md) zu nutzen. Beispielsweise können Sie Dropbox v2 dort verwenden, wo Sie bisher DropBox v1 verwendet haben.

## <a name="whats-next"></a>Nächste Schritte
* [Informationen zum manuellen Migrieren von Logik-Apps](../logic-apps/logic-apps-schema-2015-08-01.md)

<!--Icon references-->
[step1]: ./media/connectors-schema-migration/migrateschema1.png
[step2]: ./media/connectors-schema-migration/migrateschema2.png









<!--HONumber=Jan17_HO3-->



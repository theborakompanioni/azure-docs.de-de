---
title: Sicherheit auf Zeilenebene in Power BI Embedded
description: Details zur Sicherheit auf Zeilenebene in Power BI Embedded
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: 7936ade5-2c75-435b-8314-ea7ca815867a
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 10/04/2016
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: a98185bf44af2271f5ded04c05d3134321db536a


---
# <a name="row-level-security-with-power-bi-embedded"></a>Sicherheit auf Zeilenebene in Power BI Embedded
Sicherheit auf Zeilenebene (Row-Level Security, RLS) kann verwendet werden, um den Benutzerzugriff auf bestimmte Daten in einem Bericht oder Dataset einzuschränken. So können mehrere unterschiedliche Benutzer denselben Bericht verwenden, sehen dabei aber unterschiedliche Daten. Power BI Embedded unterstützt jetzt Datasets, die mit Sicherheit auf Zeilenebene konfiguriert sind.

![](media/power-bi-embedded-rls/pbi-embedded-rls-flow-1.png)

Um RLS nutzen zu können, ist es wichtig, dass Sie drei Hauptbegriffe verstanden haben: Benutzer, Rollen und Regeln. Wir sehen uns dies hier genauer an:

**Benutzer**: Dies sind die eigentlichen Endbenutzer, die Berichte anzeigen. In Power BI Embedded werden Benutzer anhand der username-Eigenschaft in einem App-Token identifiziert.

**Rollen** : Benutzer sind Rollen zugeordnet. Eine Rolle ist ein Container für Regeln und kann einen Namen wie „Verkaufsleiter“ oder „Vertriebsmitarbeiter“ haben. In Power BI Embedded werden Benutzer anhand der roles-Eigenschaft in einem App-Token identifiziert.

**Regeln** : Rollen verfügen über Regeln, bei denen es sich um die eigentlichen Filter handelt, die auf die Daten angewendet werden. Dies kann ein einfacher Filter wie „Country = USA“ oder auch ein dynamischerer Filter sein.

### <a name="example"></a>Beispiel
Für den restlichen Teil dieses Artikels geben wir ein Beispiel für die Erstellung von RLS und die anschließende Verwendung in einer eingebetteten Anwendung an. In unserem Beispiel wird die PBIX-Datei [Retail Analysis Sample](http://go.microsoft.com/fwlink/?LinkID=780547) verwendet.

![](media/power-bi-embedded-rls/pbi-embedded-rls-scenario-2.png)

In „Retail Analysis Sample“ sind die Umsätze für alle Geschäfte einer Einzelhandelskette aufgeführt. Ohne RLS werden unabhängig davon, welcher Bereichsleiter sich anmeldet und den Bericht anzeigt, immer dieselben Daten angezeigt. Die Geschäftsleitung hat nun die Entscheidung getroffen, dass jeder Bereichsleiter nur die Möglichkeit haben soll, die Umsätze für die eigenen Geschäfte anzuzeigen. Um dies zu erreichen, können wir RLS verwenden.

RLS wird in Power BI Desktop erstellt. Wenn das Dataset und der Bericht geöffnet werden, können wir in die Diagrammansicht wechseln, um das Schema anzuzeigen:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-3.png)

Für dieses Schema sind einige Punkte zu beachten:

* Alle Kennzahlen, z.B. **Total Sales** (Gesamtumsatz), sind in der Faktentabelle **Sales** (Umsatz) gespeichert.
* Hierzu gehören vier weitere Dimensionstabellen: **Item** (Artikel), **Time** (Uhrzeit), **Store** (Geschäft) und **District** (Bereich).
* Die Pfeile auf die Beziehungslinien zeigen an, in welcher Richtung Filter aus einer Tabelle in eine andere Tabelle verlaufen können. Wenn ein Filter beispielsweise auf **Time[Date]** angewendet wird, wird damit unter dem aktuellen Schema nur nach Werten in der Tabelle **Sales** gefiltert. Von diesem Filter sind keine anderen Tabellen betroffen, da alle Pfeile auf den Beziehungslinien auf die Tabelle „Sales“ zeigen, und nicht davon weg.
* Die Tabelle **District** gibt an, wer der Leiter des Bereichs ist:
  
  ![](media/power-bi-embedded-rls/pbi-embedded-rls-district-table-4.png)

Wenn wir basierend auf diesem Schema einen Filter auf die Spalte **District Manager** in der Tabelle „District“ anwenden und wenn dieser Filter eine Übereinstimmung mit dem Benutzer ergibt, der den Bericht anzeigt, filtert dieser Filter auch nach den Tabellen **Store** und **Sales**, um nur Daten für den jeweiligen Bereichsleiter anzuzeigen.

So geht‘s:

1. Klicken Sie auf der Registerkarte „Modellierung“ auf **Rollen verwalten**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-modeling-tab-5.png)
2. Erstellen Sie eine neue Rolle mit dem Namen **Manager**.  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-6.png)
3. Geben Sie in der Tabelle **District** den folgenden DAX-Ausdruck ein: **[District Manager] = USERNAME()**  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-manager-role-7.png)
4. Um sicherzustellen, dass die Regeln funktionieren, klicken Sie auf der Registerkarte **Modellierung** auf **Als Rollen anzeigen** und geben Folgendes ein:  
   ![](media/power-bi-embedded-rls/pbi-embedded-rls-view-as-roles-8.png)
   
   In den Berichten werden jetzt die Daten angezeigt, die für die Anmeldung von **Andrew Ma**gelten.

Mit dem hier angewendeten Filter wird nach allen Datensätzen in den Tabellen **District**, **Store** und **Sales** gefiltert. Aufgrund der Filterrichtung der Beziehungen zwischen **Sales** und **Time**, **Sales** und **Item** und **Item** und **Time** wird aber nicht nach den Tabellen gefiltert.

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-9.png)

Dies ist für diese Anforderung ggf. kein Problem. Wenn Bereichsleitern aber keine Artikel angezeigt werden sollen, für die sie nicht über Umsatz verfügen, können wir die bidirektionale Kreuzfilterung für die Beziehung aktivieren und den Sicherheitsfilter in beiden Richtungen anwenden. Dies erreichen Sie, indem Sie die Beziehung zwischen **Sales** und **Item** wie folgt bearbeiten:

![](media/power-bi-embedded-rls/pbi-embedded-rls-edit-relationship-10.png)

Jetzt können Filter auch von der Tabelle „Sales“ zur Tabelle **Item** fließen:

![](media/power-bi-embedded-rls/pbi-embedded-rls-diagram-view-11.png)

**Hinweis** Wenn Sie den DirectQuery-Modus für Ihre Daten verwenden, müssen Sie die bidirektionale Kreuzfilterung aktivieren, indem Sie diese beiden Optionen auswählen:

1. **Datei** -> **Optionen und Einstellungen** -> **Vorschaufeatures** -> **Kreuzfilterung in beide Richtungen für DirectQuery aktivieren**.
2. **Datei** -> **Optionen und Einstellungen** -> **DirectQuery** -> **Unbeschränkte Measures im DirectQuery-Modus zulassen**.

Weitere Informationen zur bidirektionalen Kreuzfilterung erhalten Sie, indem Sie das Whitepaper [Bidirectional cross-filtering in SQL Server Analysis Services 2016 and Power BI Desktop](http://download.microsoft.com/download/2/7/8/2782DF95-3E0D-40CD-BFC8-749A2882E109/Bidirectional cross-filtering in Analysis Services 2016 and Power BI.docx) (Bidirektionale Kreuzfilterung in SQL Server Analysis Services 2016 und Power BI Desktop) herunterladen.

Dies sind die Schritte, die in Power BI Desktop ausgeführt werden müssen. Es ist aber noch ein weiterer Schritt erforderlich, damit die definierten RLS-Regeln in Power BI Embedded funktionieren. Benutzer werden von Ihrer Anwendung authentifiziert und autorisiert, und App-Token werden verwendet, um den Benutzern Zugriff auf einen bestimmten Power BI Embedded-Bericht zu gewähren. Power BI Embedded verfügt nicht über spezifische Informationen darüber, wer der Benutzer ist. Damit RLS funktioniert, müssen Sie im Rahmen Ihres App-Tokens weiteren Kontext übergeben:

* **username** (optional): Bei Verwendung mit RLS ist dies eine Zeichenfolge, die zum Identifizieren des Benutzers verwendet werden kann, wenn RLS-Regeln angewendet werden. Weitere Informationen finden Sie unter „Sicherheit auf Zeilenebene in Power BI Embedded“.
* **roles** : Mit den in dieser Zeichenfolge enthaltenen Rollen wird ausgewählt, wann die Regeln der Sicherheit auf Zeilenebene (RLS) angewendet werden sollen. Wenn mehr als eine Rolle übergeben wird, sollten sie als Zeichenfolgenarray übergeben werden.

Falls die username-Eigenschaft vorhanden ist, müssen Sie in den Rollen auch mindestens einen Wert übergeben.

Das vollständige App-Token sieht ungefähr wie folgt aus:

![](media/power-bi-embedded-rls/pbi-embedded-rls-app-token-string-12.png)

Wenn sich nach Abschluss all dieser Schritte nun eine Person an unserer Anwendung anmeldet, um diesen Bericht anzuzeigen, sieht sie nur die Daten, für die der Zugriff gemäß unseren RLS-Regeln gewährt wurde.

![](media/power-bi-embedded-rls/pbi-embedded-rls-dashboard-13.png)

## <a name="see-also"></a>Siehe auch
[Row-level security (RLS) with Power BI](https://powerbi.microsoft.com/en-us/documentation/powerbi-admin-rls/) (Sicherheit auf Zeilenebene)




<!--HONumber=Nov16_HO3-->



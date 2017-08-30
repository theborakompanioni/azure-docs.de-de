---
title: "Parallelitäts- und Workloadverwaltung in SQL Data Warehouse | Microsoft Docs"
description: "Grundlagen der Parallelitäts- und Workloadverwaltung in Azure SQL Data Warehouse zum Entwickeln von Lösungen"
services: sql-data-warehouse
documentationcenter: NA
author: sqlmojo
manager: jhubbard
editor: 
ms.assetid: ef170f39-ae24-4b04-af76-53bb4c4d16d3
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 08/23/2017
ms.author: joeyong;barbkess;kavithaj
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: b1ab2a8253684c62be650eed2ea5f69c62188a22
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="concurrency-and-workload-management-in-sql-data-warehouse"></a>Parallelitäts- und Workloadverwaltung in SQL Data Warehouse
Mit Microsoft Azure SQL Data Warehouse können Sie die Parallelitätsebenen und Ressourcenzuordnungen steuern, z.B. die Priorisierung von Arbeitsspeicher und CPU, um für eine vorhersagbare und skalierbare Leistung zu sorgen. In diesem Artikel werden die Konzepte der Parallelitäts- und Workloadverwaltung vorgestellt. Es wird beschrieben, wie beide Features implementiert wurden und wie Sie diese in Ihrem Data Warehouse steuern können. Die Workloadverwaltung von SQL Data Warehouse ist für die Unterstützung von Mehrbenutzerumgebungen ausgelegt. Sie ist nicht für Workloads mit mehreren Mandanten ausgelegt.

## <a name="concurrency-limits"></a>Parallelitätslimits
In SQL Data Warehouse sind bis zu 1.024 gleichzeitige Verbindungen zulässig. Alle 1.024 Verbindungen können gleichzeitig Abfragen übermitteln. Um den Durchsatz zu optimieren, reiht SQL Data Warehouse einige Abfragen unter Umständen in die Warteschlange ein, um sicherzustellen, dass für jede Abfrage ein Mindestmaß an Arbeitsspeicher gewährt wird. Queuing erfolgt während der Abfrageausführung. Beim Erreichen von Parallelitätslimits kann SQL Data Warehouse den Gesamtdurchsatz erhöhen, indem Abfragen in Warteschlangen eingereiht werden und so sichergestellt wird, dass aktive Abfragen Zugriff auf dringend benötigte Arbeitsspeicherressourcen erhalten.  

Für Parallelitätslimits gelten zwei Konzepte: *gleichzeitige Abfragen* und *Parallelitätsslots*. Damit eine Abfrage ausgeführt wird, muss dies sowohl innerhalb des Abfrageparallelitätslimits als auch innerhalb der Parallelitätsslotzuordnung erfolgen.

* Bei gleichzeitigen Abfragen geht es um die Anzahl von gleichzeitig ausgeführten Abfragen. SQL Data Warehouse unterstützt bis zu 32 gleichzeitige Abfragen für höhere DWU-Größen.
* Parallelitätsslots werden basierend auf DWUs zugeordnet. Für jeweils 100 DWUs werden vier Parallelitätsslots bereitgestellt. DW100 beispielsweise ordnet vier Parallelitätsslots zu, DW1000 dagegen 40. Jede Abfrage nutzt mindestens einen Parallelitätsslot, je nach [Ressourcenklasse](#resource-classes) der Abfrage. Abfragen, die in der Ressourcenklasse „smallrc“ ausgeführt werden, nutzen einen Parallelitätsslot. Abfragen, die in einer höheren Ressourcenklasse ausgeführt werden, benötigen mehr Parallelitätsslots.

In der folgenden Tabelle werden die Limits für gleichzeitige Abfragen und Parallelitätsslots der verschiedenen DWU-Größen beschrieben.

### <a name="concurrency-limits"></a>Parallelitätslimits
| DWU | Max. Anzahl gleichzeitiger Abfragen | Zugeordnete Parallelitätsslots |
|:--- |:---:|:---:|
| DW100 |4 |4 |
| DW200 |8 |8 |
| DW300 |12 |12 |
| DW400 |16 |16 |
| DW500 |20 |20 |
| DW600 |24 |24 |
| DW1000 |32 |40 |
| DW1200 |32 |48 |
| DW1500 |32 |60 |
| DW2000 |32 |80 |
| DW3000 |32 |120 |
| DW6000 |32 |240 |

Wenn einer dieser Grenzwerte erreicht wird, werden neue Abfragen in die Warteschlange eingereiht und nach dem Prinzip „First In, First Out“ ausgeführt.  Wenn eine Abfrage abgeschlossen wird und die Anzahl von Abfragen und Slots unter die Grenzwerte sinkt, werden Abfragen in der Warteschlange freigegeben. 

> [!NOTE]  
> *SELECT* -Abfragen, die exklusiv in dynamischen Verwaltungssichten (DMVs) oder Katalogsichten ausgeführt werden, werden nicht von den Parallelitätslimits gesteuert. Sie können das System unabhängig von der Anzahl der darin ausgeführten Abfragen überwachen.
> 
> 

## <a name="resource-classes"></a>Ressourcenklassen
Mithilfe von Ressourcenklassen steuern Sie die Speicherzuweisung und die zugewiesenen CPU-Zyklen einer Abfrage. Sie können einem Benutzer zwei Typen von Ressourcenklassen in Form von Datenbankrollen zuweisen. Dabei handelt es sich um die folgenden beiden Ressourcenklassentypen:
1. Dynamische Ressourcenklassen (**smallrc, mediumrc, largerc, xlargerc**) weisen je nach der aktuellen DWU eine variable Menge an Arbeitsspeicher zu. Dies bewirkt, dass Ihre Abfragen automatisch mehr Arbeitsspeicher erhalten, wenn Sie die DWU nach oben skalieren. 
2. Statische Ressourcenklassen (**staticrc10, staticrc20, staticrc30, staticrc40, staticrc50, staticrc60, staticrc70, staticrc80**) weisen unabhängig von der aktuellen DWU die gleiche Menge an Arbeitsspeicher zu (vorausgesetzt, dass die DWU selbst über genügend Arbeitsspeicher verfügt). Dies bedeutet, dass Sie auf größeren DWUs mehr gleichzeitige Abfragen in jeder Ressourcenklasse ausführen können.

Benutzer in **smallrc** und **staticrc10** erhalten eine geringere Menge an Arbeitsspeicher und können von höherer Parallelität profitieren. Im Gegensatz dazu wird Benutzern, die **xlargerc** oder **staticrc80** zugewiesen sind, eine große Menge an Arbeitsspeicher gewährt. Dies bedeutet, dass ein geringerer Anteil ihrer Abfragen gleichzeitig ausgeführt werden kann.

Standardmäßig ist jeder Benutzer Mitglied der kleinen Ressourcenklasse **smallrc**. Die Prozedur `sp_addrolemember` wird verwendet, um die Ressourcenklasse heraufzusetzen, und `sp_droprolemember` wird verwendet, um die Ressourcenklasse herabzusetzen. Mit dem folgenden Befehl wird die Ressourcenklasse beispielsweise von „loaduser“ auf **largerc** heraufgesetzt:

```sql
EXEC sp_addrolemember 'largerc', 'loaduser'
```


### <a name="queries-that-do-not-honor-resource-classes"></a>Abfragen, die Ressourcenklassen nicht berücksichtigen

Es gibt einige Arten von Abfragen, die nicht von einem größeren Arbeitsspeicher profitieren. Das System ignoriert die Ressourcenklassenzuweisung dieser Abfragen und führt sie stattdessen immer in der kleinen Ressourcenklasse aus. Wenn diese Abfragen immer in der kleinen Ressourcenklasse ausgeführt werden, können sie auch dann ausgeführt werden, wenn nicht viele Parallelitätsslots zur Verfügung stehen, und sie verbrauchen nicht mehr Slots als notwendig. Weitere Informationen finden Sie unter [Ressourcenklassenausnahmen](#query-exceptions-to-concurrency-limits) .

## <a name="details-on-resource-class-assignment"></a>Details zur Ressourcenklassenzuweisung


Weitere Details zu Ressourcenklassen:

* *Rolle ändern* ist erforderlich, um die Ressourcenklasse eines Benutzers zu ändern.
* Obwohl Sie mindestens einer der höheren Ressourcenklassen einen Benutzer hinzufügen können, haben dynamische Ressourcenklassen Vorrang vor statischen Ressourcenklassen. Wenn ein Benutzer sowohl **mediumrc**(dynamisch) und **staticrc80**(statisch) zugewiesen ist, wird also die Ressourcenklasse **mediumrc** berücksichtigt.
 * Wenn ein Benutzer mehreren Ressourcenklassen eines bestimmten Typs (mehr als einer dynamischen Ressourcenklasse oder mehr als einer statischen Ressourcenklasse) zugewiesen ist, wird die höchste Ressourcenklasse berücksichtigt. Ist ein Benutzer also sowohl „mediumrc“ als auch „largerc“ zugewiesen ist, wird die höhere Ressourcenklasse (largerc) berücksichtigt. Und wenn ein Benutzer sowohl **staticrc20** und **statirc80** zugewiesen ist,wird **staticrc80** berücksichtigt.
* Die Ressourcenklasse des Systemadministratorbenutzers kann nicht geändert werden.

Ein ausführliches Beispiel finden Sie unter [Beispiel: Ändern der Ressourcenklasse eines Benutzers](#changing-user-resource-class-example).

## <a name="memory-allocation"></a>Speicherzuweisung
Das Erhöhen der Ressourcenklasse eines Benutzers hat Vor- und Nachteile. Durch das Erhöhen einer Ressourcenklasse für einen Benutzer ist für die Abfragen mehr Arbeitsspeicher zugänglich. Das kann bedeuten, dass Abfragen schneller ausgeführt werden.  Durch höhere Ressourcenklassen verringert sich jedoch auch die Anzahl von gleichzeitigen Abfragen, die ausgeführt werden können. Dies ist der Kompromiss zwischen dem Zuweisen großer Mengen von Arbeitsspeicher für eine einzelne Abfrage und dem Zulassen der gleichzeitigen Ausführung anderer Abfragen, denen ebenfalls Arbeitsspeicher zugewiesen werden muss. Wenn einem Benutzer eine große Menge Arbeitsspeicher für eine Abfrage zugewiesen wurde, können andere Benutzer nicht auf diesen Arbeitsspeicher zugreifen, um eine Abfrage auszuführen.

In der folgenden Tabelle ist der Arbeitsspeicher angegeben, der jeder Verteilung nach DWU und Ressourcenklasse zugeordnet wird.

### <a name="memory-allocations-per-distribution-for-dynamic-resource-classes-mb"></a>Speicherbelegungen pro Verteilung für dynamische Ressourcenklassen (MB)
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| DW100 |100 |100 |200 |400 |
| DW200 |100 |200 |400 |800 |
| DW300 |100 |200 |400 |800 |
| DW400 |100 |400 |800 |1.600 |
| DW500 |100 |400 |800 |1.600 |
| DW600 |100 |400 |800 |1.600 |
| DW1000 |100 |800 |1.600 |3.200 |
| DW1200 |100 |800 |1.600 |3.200 |
| DW1500 |100 |800 |1.600 |3.200 |
| DW2000 |100 |1.600 |3.200 |6.400 |
| DW3000 |100 |1.600 |3.200 |6.400 |
| DW6000 |100 |3.200 |6.400 |12.800 |

In der folgenden Tabelle ist der Arbeitsspeicher angegeben, der jeder Verteilung nach DWU und statischer Ressourcenklasse zugeordnet wird. Beachten Sie, dass der Arbeitsspeicher der höheren Ressourcenklassen reduziert wird, um die globalen DWU-Grenzwerte zu berücksichtigen.

### <a name="memory-allocations-per-distribution-for-static-resource-classes-mb"></a>Speicherbelegungen pro Verteilung für statische Ressourcenklassen (MB)
| DWU | staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |100 |200 |400 |400 |400 |400 |400 |400 |
| DW200 |100 |200 |400 |800 |800 |800 |800 |800 |
| DW300 |100 |200 |400 |800 |800 |800 |800 |800 |
| DW400 |100 |200 |400 |800 |1.600 |1.600 |1.600 |1.600 |
| DW500 |100 |200 |400 |800 |1.600 |1.600 |1.600 |1.600 |
| DW600 |100 |200 |400 |800 |1.600 |1.600 |1.600 |1.600 |
| DW1000 |100 |200 |400 |800 |1.600 |3.200 |3.200 |3.200 |
| DW1200 |100 |200 |400 |800 |1.600 |3.200 |3.200 |3.200 |
| DW1500 |100 |200 |400 |800 |1.600 |3.200 |3.200 |3.200 |
| DW2000 |100 |200 |400 |800 |1.600 |3.200 |6.400 |6.400 |
| DW3000 |100 |200 |400 |800 |1.600 |3.200 |6.400 |6.400 |
| DW6000 |100 |200 |400 |800 |1.600 |3.200 |6.400 |12.800 |

In der obigen Tabelle sehen Sie, dass eine Abfrage, die in einem DW2000 in der Ressourcenklasse **xlargerc** ausgeführt wird, jeweils Zugriff auf 6.400 MB Arbeitsspeicher in jeder der 60 verteilten Datenbanken hat.  In SQL Data Warehouse gibt es 60 Verteilungen. Daher sollten zur Berechnung der gesamten Arbeitsspeicherbelegung für eine Abfrage in einer bestimmten Ressourcenklasse die oben genannten Werte mit 60 multipliziert werden.

### <a name="memory-allocations-system-wide-gb"></a>Systemweite Speicherbelegungen (GB)
| DWU | smallrc | mediumrc | largerc | xlargerc |
|:--- |:---:|:---:|:---:|:---:|
| DW100 |6 |6 |12 |23 |
| DW200 |6 |12 |23 |47 |
| DW300 |6 |12 |23 |47 |
| DW400 |6 |23 |47 |94 |
| DW500 |6 |23 |47 |94 |
| DW600 |6 |23 |47 |94 |
| DW1000 |6 |47 |94 |188 |
| DW1200 |6 |47 |94 |188 |
| DW1500 |6 |47 |94 |188 |
| DW2000 |6 |94 |188 |375 |
| DW3000 |6 |94 |188 |375 |
| DW6000 |6 |188 |375 |750 |

In dieser Tabelle mit systemweiten Speicherbelegungen sehen Sie, dass einer Abfrage, die in einem DW2000 in der Ressourcenklasse „xlargerc“ ausgeführt wird, im gesamten SQL Data Warehouse insgesamt 375 GB Arbeitsspeicher zugewiesen werden (6.400 MB × 60 Verteilungen ÷ 1.024 zur Umrechnung in GB).

Die gleiche Berechnung gilt für statische Ressourcenklassen.
 
## <a name="concurrency-slot-consumption"></a>Verbrauch von Parallelitätsslots  
SQL Data Warehouse weist Abfragen, die in höheren Ressourcenklassen ausgeführt werden, mehr Arbeitsspeicher zu. Der Arbeitsspeicher ist eine feststehende Ressource.  Daher gilt Folgendes: Je mehr Arbeitsspeicher pro Abfrage zugeordnet ist, desto weniger parallele Abfragen können unterstützt werden. Die folgende Tabelle gibt in einer einzigen Ansicht alle oben stehenden Konzepte wieder – sowohl die Anzahl verfügbarer Parallelitätsslots pro DWU als auch die Slots, die von den einzelnen Ressourcenklassen verbraucht werden.  

### <a name="allocation-and-consumption-of-concurrency-slots"></a>Zuordnung und Verbrauch von Parallelitätsslots  
| DWU | Maximale Anzahl gleichzeitiger Abfragen | Zugeordnete Parallelitätsslots | Durch „smallrc“ verwendete Slots | Durch „mediumrc“ verwendete Slots | Durch „largerc“ verwendete Slots | Durch „xlargerc“ verwendete Slots |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |4 |4 |1 |1 |2 |4 |
| DW200 |8 |8 |1 |2 |4 |8 |
| DW300 |12 |12 |1 |2 |4 |8 |
| DW400 |16 |16 |1 |4 |8 |16 |
| DW500 |20 |20 |1 |4 |8 |16 |
| DW600 |24 |24 |1 |4 |8 |16 |
| DW1000 |32 |40 |1 |8 |16 |32 |
| DW1200 |32 |48 |1 |8 |16 |32 |
| DW1500 |32 |60 |1 |8 |16 |32 |
| DW2000 |32 |80 |1 |16 |32 |64 |
| DW3000 |32 |120 |1 |16 |32 |64 |
| DW6000 |32 |240 |1 |32 |64 |128 |

### <a name="allocation-and-consumption-of-concurrency-slots-for-static-resource-classes"></a>Zuordnung und Verbrauch von Parallelitätsslots für statische Ressourcenklassen
| DWU | Maximale Anzahl gleichzeitiger Abfragen | Zugeordnete Parallelitätsslots |staticrc10 | staticrc20 | staticrc30 | staticrc40 | staticrc50 | staticrc60 | staticrc70 | staticrc80 |
|:--- |:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
| DW100 |4 |4 |1 |2 |4 |4 |4 |4 |4 |4 |
| DW200 |8 |8 |1 |2 |4 |8 |8 |8 |8 |8 |
| DW300 |12 |12 |1 |2 |4 |8 |8 |8 |8 |8 |
| DW400 |16 |16 |1 |2 |4 |8 |16 |16 |16 |16 |
| DW500 | 20| 20| 1| 2| 4| 8| 16| 16| 16| 16|
| DW600 | 24| 24| 1| 2| 4| 8| 16| 16| 16| 16|
| DW1000 | 32| 40| 1| 2| 4| 8| 16| 32| 32| 32|
| DW1200 | 32| 48| 1| 2| 4| 8| 16| 32| 32| 32|
| DW1500 | 32| 60| 1| 2| 4| 8| 16| 32| 32| 32|
| DW2000 | 32| 80| 1| 2| 4| 8| 16| 32| 64| 64|
| DW3000 | 32| 120| 1| 2| 4| 8| 16| 32| 64| 64|
| DW6000 | 32| 240| 1| 2| 4| 8| 16| 32| 64| 128|

In diesen Tabellen sehen Sie, dass ein als DW1000 ausgeführtes SQL Data Warehouse maximal 32 gleichzeitige Abfragen und insgesamt 40 Parallelitätsslots zuordnet. Wenn alle Benutzer Abfragen in „smallrc“ ausführen, sind 32 gleichzeitige Abfragen zulässig, da jede Abfrage 1 Parallelitätsslot belegt. Wenn alle Benutzer in einem DW1000 Abfragen in „mediumrc“ ausführen, werden jeder Abfrage 800 MB pro Verteilung zugewiesen, was eine Gesamtzuweisung an Arbeitsspeicher von 47 GB ergibt. Die Parallelität ist in diesem Fall auf 5 Benutzer begrenzt (40 Parallelitätsslots / 8 Slots pro mediumrc-Benutzer).

## <a name="selecting-proper-resource-class"></a>Auswählen der richtigen Ressourcenklasse  
Eine gute Vorgehensweise besteht darin, Benutzer dauerhaft zu einer Ressourcenklasse zuzuweisen, anstatt die Ressourcenklasse der Benutzer zu ändern. Für Ladevorgänge in gruppierte Columnstore-Tabellen werden beispielsweise Indizes von höherer Qualität erstellt, wenn mehr Arbeitsspeicher zugeordnet wird. Erstellen Sie einen Benutzer speziell für das Laden von Daten, und weisen Sie diesen Benutzer dauerhaft einer höheren Ressourcenklasse zu, um sicherzustellen, dass bei Ladevorgängen Zugriff auf eine höhere Arbeitsspeicherebene besteht.
Dies sind einige bewährte Methoden, die angewendet werden können: Wie bereits erwähnt, unterstützt SQL DW Ressourcenklassentypen: statische Ressourcenklassen und dynamische Ressourcenklassen.
### <a name="loading-best-practices"></a>Bewährte Methoden für Lasten
1.  Wenn Lasten mit einer regelmäßigen Datenmenge erwartet werden, ist eine statische Ressourcenklasse eine gute Wahl. Wenn später zentral hochskaliert wird, um mehr Rechenleistung abzurufen, kann das Data Warehouse standardmäßig mehr gleichzeitige Abfragen ausführen, da der Lastbenutzer nicht mehr Arbeitsspeicher belegt.
2.  Wenn in einigen Fällen größere Lasten erwartet werden, ist eine dynamische Ressourcenklasse eine gute Wahl. Wenn später zentral hochskaliert wird, um mehr Rechenleistung abzurufen, erhält der Lastbenutzer standardmäßig mehr Arbeitsspeicher, sodass der Ladevorgang schneller ausgeführt wird.

Der zum effizienten Verarbeiten von Lasten erforderliche Arbeitsspeicher richtet sich nach der Art der geladenen Tabelle und der Menge der verarbeiteten Daten. Das Laden von Daten in CCI-Tabellen erfordert z.B. eine bestimmte Menge an Arbeitsspeicher, damit CCI-Zeilengruppen optimiert werden können. Weitere Informationen finden Sie unter „Columnstore-Indizes – Leitfaden zum Datenladevorgang“.

Als bewährte Methode empfehlen wir Ihnen, mindestens 200 MB des Arbeitsspeichers für Lasten zu verwenden.

### <a name="querying-best-practices"></a>Bewährte Methoden für Abfragen
Abfragen haben je nach ihrer Komplexität unterschiedliche Anforderungen. Das Erhöhen des Arbeitsspeichers pro Abfrage oder der Parallelität eignen sich gleichermaßen, um den Gesamtdurchsatz je nach den Anforderungen der Abfrage zu steigern.
1.  Wenn regelmäßige, komplexe Abfragen erwartet werden (z.B. um tägliche und wöchentliche Berichte zu generieren) und keine Parallelität genutzt werden muss, ist eine dynamische Ressourcenklasse eine gute Wahl. Wenn das System mehr Daten verarbeitet, wird dem Benutzer, die der Abfrage ausführt, durch das zentrale Hochskalieren des Data Warehouse daher automatisch mehr Arbeitsspeicher bereitgestellt.
2.  Wenn variable oder tageszyklische Parallelitätsmuster erwartet werden (z.B. wenn die Datenbank mithilfe einer allgemein zugänglichen Webbenutzeroberfläche abgefragt wird), ist eine statische Ressourcenklasse eine gute Wahl. Wenn später auf ein Data Warehouse hochskaliert wird, kann der Benutzer, welcher der statischen Ressourcenklasse zugeordnet ist, automatisch mehr gleichzeitige Abfragen ausführen.

Das Auswählen der richtigen Arbeitsspeicherzuweisung je nach Anforderung der Abfrage ist eine anspruchsvolle Aufgabe, da es von zahlreichen Faktoren abhängt, z.B. der Menge der abgefragten Daten, der Art der Tabellenschemas sowie von verschiedenen Verknüpfungs-, Auswahl- und Gruppenprädikaten. Generell ermöglicht das Zuweisen von mehr Arbeitsspeicher ein schnelleres Durchführen von Abfragen, verringert jedoch die allgemeine Parallelität. Wenn Parallelität keine Rolle spielt, hat eine übermäßige Arbeitsspeicherzuweisung keine negativen Auswirkungen. Zur Durchsatzoptimierung müssen u.U. verschiedene Arten von Ressourcenklassen ausprobiert werden.

Mithilfe der folgenden gespeicherten Prozedur können Sie Parallelitäts- und Arbeitsspeicherzuweisung pro Ressourcenklasse für ein bestimmtes Servicelevelziel sowie die naheliegend beste Ressourcenklasse für speicherintensive CCI-in einer nicht partitionierten CCI-Tabelle für eine bestimmte Ressourcenklasse ermitteln:

#### <a name="description"></a>Beschreibung:  
Hier wird der Zweck dieser gespeicherten Prozedur beschrieben:  
1. Der Benutzer soll beim Ermitteln der Parallelitäts- und Arbeitsspeicherzuweisung pro Ressourcenklasse für ein bestimmtes Servicelevelziel unterstützt werden. Der Benutzer muss dazu wie im folgenden Beispiel gezeigt NULL als Schema und Tabellenname angeben.  
2. Der Benutzer soll beim Ermitteln der naheliegend beste Ressourcenklasse für speicherintensive CCI-Vorgänge (Laden, Kopieren einer Tabelle, Indexneuerstellung usw.) in einer nicht partitionierten CCI-Tabelle für eine bestimmte Ressourcenklasse unterstützt werden. Die gespeicherte Prozedur verwendet ein Tabellenschema, um die erforderliche Arbeitsspeicherzuweisung dafür zu ermitteln.

#### <a name="dependencies--restrictions"></a>Abhängigkeiten und Einschränkungen:
- Diese gespeicherte Prozedur dient nicht zum Berechnen des Arbeitsspeicherbedarfs für die Tabelle „partitioned-cci“.    
- Diese gespeicherte Prozedur berücksichtigt keine Arbeitsspeicheranforderungen für den SELECT-Teil von CTAS/INSERT-SELECT und geht davon aus, dass es sich um eine einfache SELECT-Anweisung handelt.
- Diese gespeicherte Prozedur verwendet eine temporäre Tabelle, damit diese in der Sitzung verwendet werden kann, in der diese gespeicherte Prozedur erstellt wurde.    
- Diese gespeicherte Prozedur hängt von den aktuellen Angeboten (z.B. Hardwarekonfiguration, DMS-Konfiguration) an und funktioniert nicht ordnungsgemäß, wenn eines davon geändert wird.  
- Diese gespeicherte Prozedur hängt von der vorhandenen angebotenen Parallelitätsbegrenzung ab und funktioniert nicht ordnungsgemäß, wenn diese geändert wird.  
- Diese gespeicherte Prozedur hängt von vorhandenen Ressourcenklassenangeboten ab und funktioniert nicht ordnungsgemäß, wenn diese geändert werden.  

>  [!NOTE]  
>  Wenn Sie nach der Ausführung der gespeicherten Prozedur mit den bereitgestellten Parametern keine Ausgabe erhalten, kann dies zwei Gründe haben. <br />1. Der DW-Parameter enthält einen ungültigen SLO-Wert, <br />2. ODER es sind keine übereinstimmenden Ressourcenklasse für den CCI-Vorgang vorhanden, wenn ein Tabellenname angegeben wurde. <br />Beispielsweise beträgt bei DW100 die höchste verfügbare Arbeitsspeicherzuweisung 400 MB, auch wenn das Tabellenschema breit genug ist, um die Anforderung von 400 MB zu überschreiten.
      
#### <a name="usage-example"></a>Verwendungsbeispiel:
Syntax:  
`EXEC dbo.prc_workload_management_by_DWU @DWU VARCHAR(7), @SCHEMA_NAME VARCHAR(128), @TABLE_NAME VARCHAR(128)`  
1. @DWU:: Gibt entweder einen NULL-Parameter zum Extrahieren der aktuellen DWU aus der DW DB oder eine beliebige unterstützte DWU im Format „DW100“ an.
2. @SCHEMA_NAME:: Gibt einen Schemanamen der Tabelle an.
3. @TABLE_NAME:: Gibt einen relevanten Tabellennamen an.

Beispiele für Ausführung dieser gespeicherte Prozedur:  
```sql  
EXEC dbo.prc_workload_management_by_DWU 'DW2000', 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU NULL, 'dbo', 'Table1';  
EXEC dbo.prc_workload_management_by_DWU 'DW6000', NULL, NULL;  
EXEC dbo.prc_workload_management_by_DWU NULL, NULL, NULL;  
```

Die in den obigen Beispielen verwendete Tabelle1 kann wie folgt erstellt werden:  
`CREATE TABLE Table1 (a int, b varchar(50), c decimal (18,10), d char(10), e varbinary(15), f float, g datetime, h date);`

#### <a name="heres-the-stored-procedure-definition"></a>Definition der gespeicherten Prozedur:
```sql  
-------------------------------------------------------------------------------
-- Dropping prc_workload_management_by_DWU procedure if it exists.
-------------------------------------------------------------------------------
IF EXISTS (SELECT * FROM sys.objects WHERE type = 'P' AND name = 'prc_workload_management_by_DWU')
DROP PROCEDURE dbo.prc_workload_management_by_DWU
GO

-------------------------------------------------------------------------------
-- Creating prc_workload_management_by_DWU.
-------------------------------------------------------------------------------
CREATE PROCEDURE dbo.prc_workload_management_by_DWU
(   @DWU VARCHAR(7),
      @SCHEMA_NAME VARCHAR(128),
       @TABLE_NAME VARCHAR(128)
)
AS
IF @DWU IS NULL
BEGIN
-- Selecting proper DWU for the current DB if not specified.
SET @DWU = (
  SELECT 'DW'+CAST(COUNT(*)*100 AS VARCHAR(10))
  FROM sys.dm_pdw_nodes
  WHERE type = 'COMPUTE')
END

DECLARE @DWU_NUM INT
SET @DWU_NUM = CAST (SUBSTRING(@DWU, 3, LEN(@DWU)-2) AS INT)

-- Raise error if either schema name or table name is supplied but not both them supplied
--IF ((@SCHEMA_NAME IS NOT NULL AND @TABLE_NAME IS NULL) OR (@TABLE_NAME IS NULL AND @SCHEMA_NAME IS NOT NULL))
--     RAISEERROR('User need to supply either both Schema Name and Table Name or none of them')
       
-- Dropping temp table if exists.
IF OBJECT_ID('tempdb..#ref') IS NOT NULL
BEGIN
  DROP TABLE #ref;
END

-- Creating ref. temptable (CTAS) to hold mapping info.
-- CREATE TABLE #ref
CREATE TABLE #ref
WITH (DISTRIBUTION = ROUND_ROBIN)
AS 
WITH
-- Creating concurrency slots mapping for various DWUs.
alloc
AS
(
  SELECT 'DW100' AS DWU, 4 AS max_queries, 4 AS max_slots, 1 AS slots_used_smallrc, 1 AS slots_used_mediumrc,
        2 AS slots_used_largerc, 4 AS slots_used_xlargerc, 1 AS slots_used_staticrc10, 2 AS slots_used_staticrc20,
        4 AS slots_used_staticrc30, 4 AS slots_used_staticrc40, 4 AS slots_used_staticrc50,
        4 AS slots_used_staticrc60, 4 AS slots_used_staticrc70, 4 AS slots_used_staticrc80
  UNION ALL
    SELECT 'DW200', 8, 8, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW300', 12, 12, 1, 2, 4, 8, 1, 2, 4, 8, 8, 8, 8, 8
  UNION ALL
    SELECT 'DW400', 16, 16, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
     SELECT 'DW500', 20, 20, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW600', 24, 24, 1, 4, 8, 16, 1, 2, 4, 8, 16, 16, 16, 16
  UNION ALL
    SELECT 'DW1000', 32, 40, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1200', 32, 48, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW1500', 32, 60, 1, 8, 16, 32, 1, 2, 4, 8, 16, 32, 32, 32
  UNION ALL
    SELECT 'DW2000', 32, 80, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
   SELECT 'DW3000', 32, 120, 1, 16, 32, 64, 1, 2, 4, 8, 16, 32, 64, 64
  UNION ALL
    SELECT 'DW6000', 32, 240, 1, 32, 64, 128, 1, 2, 4, 8, 16, 32, 64, 128
)
-- Creating workload mapping to their corresponding slot consumption and default memory grant.
,map
AS
(
  SELECT 'SloDWGroupC00' AS wg_name,1 AS slots_used,100 AS tgt_mem_grant_MB
  UNION ALL
    SELECT 'SloDWGroupC01',2,200
  UNION ALL
    SELECT 'SloDWGroupC02',4,400
  UNION ALL
    SELECT 'SloDWGroupC03',8,800
  UNION ALL
    SELECT 'SloDWGroupC04',16,1600
  UNION ALL
    SELECT 'SloDWGroupC05',32,3200
  UNION ALL
    SELECT 'SloDWGroupC06',64,6400
  UNION ALL
    SELECT 'SloDWGroupC07',128,12800
)
-- Creating ref based on current / asked DWU.
, ref
AS
(
  SELECT  a1.*
  ,       m1.wg_name          AS wg_name_smallrc
  ,       m1.tgt_mem_grant_MB AS tgt_mem_grant_MB_smallrc
  ,       m2.wg_name          AS wg_name_mediumrc
  ,       m2.tgt_mem_grant_MB AS tgt_mem_grant_MB_mediumrc
  ,       m3.wg_name          AS wg_name_largerc
  ,       m3.tgt_mem_grant_MB AS tgt_mem_grant_MB_largerc
  ,       m4.wg_name          AS wg_name_xlargerc
  ,       m4.tgt_mem_grant_MB AS tgt_mem_grant_MB_xlargerc
  ,       m5.wg_name          AS wg_name_staticrc10
  ,       m5.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc10
  ,       m6.wg_name          AS wg_name_staticrc20
  ,       m6.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc20
  ,       m7.wg_name          AS wg_name_staticrc30
  ,       m7.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc30
  ,       m8.wg_name          AS wg_name_staticrc40
  ,       m8.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc40
  ,       m9.wg_name          AS wg_name_staticrc50
  ,       m9.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc50
  ,       m10.wg_name          AS wg_name_staticrc60
  ,       m10.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc60
  ,       m11.wg_name          AS wg_name_staticrc70
  ,       m11.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc70
  ,       m12.wg_name          AS wg_name_staticrc80
  ,       m12.tgt_mem_grant_MB AS tgt_mem_grant_MB_staticrc80
  FROM alloc a1
  JOIN map   m1  ON a1.slots_used_smallrc     = m1.slots_used
  JOIN map   m2  ON a1.slots_used_mediumrc    = m2.slots_used
  JOIN map   m3  ON a1.slots_used_largerc     = m3.slots_used
  JOIN map   m4  ON a1.slots_used_xlargerc    = m4.slots_used
  JOIN map   m5  ON a1.slots_used_staticrc10    = m5.slots_used
  JOIN map   m6  ON a1.slots_used_staticrc20    = m6.slots_used
  JOIN map   m7  ON a1.slots_used_staticrc30    = m7.slots_used
  JOIN map   m8  ON a1.slots_used_staticrc40    = m8.slots_used
  JOIN map   m9  ON a1.slots_used_staticrc50    = m9.slots_used
  JOIN map   m10  ON a1.slots_used_staticrc60    = m10.slots_used
  JOIN map   m11  ON a1.slots_used_staticrc70    = m11.slots_used
  JOIN map   m12  ON a1.slots_used_staticrc80    = m12.slots_used
-- WHERE   a1.DWU = @DWU
  WHERE   a1.DWU = UPPER(@DWU)
)
SELECT  DWU
,       max_queries
,       max_slots
,       slots_used
,       wg_name
,       tgt_mem_grant_MB
,       up1 as rc
,       (ROW_NUMBER() OVER(PARTITION BY DWU ORDER BY DWU)) as rc_id
FROM
(
    SELECT  DWU
    ,       max_queries
    ,       max_slots
    ,       slots_used
    ,       wg_name
    ,       tgt_mem_grant_MB
    ,       REVERSE(SUBSTRING(REVERSE(wg_names),1,CHARINDEX('_',REVERSE(wg_names),1)-1)) as up1
    ,       REVERSE(SUBSTRING(REVERSE(tgt_mem_grant_MBs),1,CHARINDEX('_',REVERSE(tgt_mem_grant_MBs),1)-1)) as up2
    ,       REVERSE(SUBSTRING(REVERSE(slots_used_all),1,CHARINDEX('_',REVERSE(slots_used_all),1)-1)) as up3
    FROM    ref AS r1
    UNPIVOT
    (
        wg_name FOR wg_names IN (wg_name_smallrc,wg_name_mediumrc,wg_name_largerc,wg_name_xlargerc,
        wg_name_staticrc10, wg_name_staticrc20, wg_name_staticrc30, wg_name_staticrc40, wg_name_staticrc50,
        wg_name_staticrc60, wg_name_staticrc70, wg_name_staticrc80)
    ) AS r2
    UNPIVOT
    (
        tgt_mem_grant_MB FOR tgt_mem_grant_MBs IN (tgt_mem_grant_MB_smallrc,tgt_mem_grant_MB_mediumrc,
        tgt_mem_grant_MB_largerc,tgt_mem_grant_MB_xlargerc, tgt_mem_grant_MB_staticrc10, tgt_mem_grant_MB_staticrc20,
        tgt_mem_grant_MB_staticrc30, tgt_mem_grant_MB_staticrc40, tgt_mem_grant_MB_staticrc50,
        tgt_mem_grant_MB_staticrc60, tgt_mem_grant_MB_staticrc70, tgt_mem_grant_MB_staticrc80)
    ) AS r3
    UNPIVOT
    (
        slots_used FOR slots_used_all IN (slots_used_smallrc,slots_used_mediumrc,slots_used_largerc,
        slots_used_xlargerc, slots_used_staticrc10, slots_used_staticrc20, slots_used_staticrc30,
        slots_used_staticrc40, slots_used_staticrc50, slots_used_staticrc60, slots_used_staticrc70,
        slots_used_staticrc80)
    ) AS r4
) a
WHERE   up1 = up2
AND     up1 = up3
;
-- Getting current info about workload groups.
WITH  
dmv  
AS  
(
  SELECT
          rp.name                                           AS rp_name
  ,       rp.max_memory_kb*1.0/1048576                      AS rp_max_mem_GB
  ,       (rp.max_memory_kb*1.0/1024)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_MB
  ,       (rp.max_memory_kb*1.0/1048576)
          *(request_max_memory_grant_percent/100)           AS max_memory_grant_GB
  ,       wg.name                                           AS wg_name
  ,       wg.importance                                     AS importance
  ,       wg.request_max_memory_grant_percent               AS request_max_memory_grant_percent
  FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
  JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    ON  wg.pdw_node_id  = rp.pdw_node_id
                                                                  AND wg.pool_id      = rp.pool_id
  WHERE   rp.name = 'SloDWPool'
  GROUP BY
          rp.name
  ,       rp.max_memory_kb
  ,       wg.name
  ,       wg.importance
  ,       wg.request_max_memory_grant_percent
)
-- Creating resource class name mapping.
,names
AS
(
  SELECT 'smallrc' as resource_class, 1 as rc_id
  UNION ALL
    SELECT 'mediumrc', 2
  UNION ALL
    SELECT 'largerc', 3
  UNION ALL
    SELECT 'xlargerc', 4
  UNION ALL
    SELECT 'staticrc10', 5
  UNION ALL
    SELECT 'staticrc20', 6
  UNION ALL
    SELECT 'staticrc30', 7
  UNION ALL
    SELECT 'staticrc40', 8
  UNION ALL
    SELECT 'staticrc50', 9
  UNION ALL
    SELECT 'staticrc60', 10
  UNION ALL
    SELECT 'staticrc70', 11
  UNION ALL
    SELECT 'staticrc80', 12
)
,base AS
(   SELECT  schema_name
    ,       table_name
    ,       SUM(column_count)                   AS column_count
    ,       ISNULL(SUM(short_string_column_count),0)   AS short_string_column_count
    ,       ISNULL(SUM(long_string_column_count),0)    AS long_string_column_count
    FROM    (   SELECT  sm.name                                             AS schema_name
                ,       tb.name                                             AS table_name
                ,       COUNT(co.column_id)                                 AS column_count
                           ,       CASE    WHEN co.system_type_id IN (36,43,106,108,165,167,173,175,231,239) 
                                AND  co.max_length <= 32 
                                THEN COUNT(co.column_id) 
                        END                                                 AS short_string_column_count
                ,       CASE    WHEN co.system_type_id IN (165,167,173,175,231,239) 
                                AND  co.max_length > 32 and co.max_length <=8000
                                THEN COUNT(co.column_id) 
                        END                                                 AS long_string_column_count
                FROM    sys.schemas AS sm
                JOIN    sys.tables  AS tb   on sm.[schema_id] = tb.[schema_id]
                JOIN    sys.columns AS co   ON tb.[object_id] = co.[object_id]
                           WHERE tb.name = @TABLE_NAME AND sm.name = @SCHEMA_NAME
                GROUP BY sm.name
                ,        tb.name
                ,        co.system_type_id
                ,        co.max_length            ) a
GROUP BY schema_name
,        table_name
)
, size AS
(
SELECT  schema_name
,       table_name
,       75497472                                            AS table_overhead

,       column_count*1048576*8                              AS column_size
,       short_string_column_count*1048576*32                       AS short_string_size,       (long_string_column_count*16777216) AS long_string_size
FROM    base
UNION
SELECT CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as schema_name
         ,CASE WHEN COUNT(*) = 0 THEN 'EMPTY' END as table_name
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as table_overhead
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as column_size
         ,CASE WHEN COUNT(*) = 0 THEN 0 END as short_string_size

,CASE WHEN COUNT(*) = 0 THEN 0 END as long_string_size
FROM   base
)
, load_multiplier as 
(
SELECT  CASE 
                     WHEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) > 0 THEN FLOOR(8 * (CAST (@DWU_NUM AS FLOAT)/6000)) 
                     ELSE 1 
              END AS multipliplication_factor
) 
       SELECT  r1.DWU
       , schema_name
       , table_name
       , rc.resource_class as closest_rc_in_increasing_order
       , max_queries_at_this_rc = CASE
             WHEN (r1.max_slots / r1.slots_used > r1.max_queries)
                  THEN r1.max_queries
             ELSE r1.max_slots / r1.slots_used
                  END
       , r1.max_slots as max_concurrency_slots
       , r1.slots_used as required_slots_for_the_rc
       , r1.tgt_mem_grant_MB  as rc_mem_grant_MB
       , CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) AS est_mem_grant_required_for_cci_operation_MB       
       FROM    size, load_multiplier, #ref r1, names  rc
       WHERE r1.rc_id=rc.rc_id
                     AND CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) < r1.tgt_mem_grant_MB
       ORDER BY ABS(CAST((table_overhead*1.0+column_size+short_string_size+long_string_size)*multipliplication_factor/1048576    AS DECIMAL(18,2)) - r1.tgt_mem_grant_MB)
GO
```

## <a name="query-importance"></a>Wichtigkeit von Abfragen
SQL Data Warehouse implementiert Ressourcenklassen anhand von Workloadgruppen. Es gibt insgesamt acht Workloadgruppen, die das Verhalten der Ressourcenklassen über die verschiedenen DWU-Größen hinweg steuern. In jeder DWU verwendet SQL Data Warehouse nur vier der acht Workloadgruppen. Dies ergibt Sinn, da jede Workloadgruppe einer der vier Ressourcenklassen „smallrc“, „mediumrc“, „largerc“ oder „xlargerc“ zugewiesen ist. Sie müssen diese Workloadgruppen verstehen, da für einige dieser Workloadgruppen eine höhere *Wichtigkeit*festgelegt ist. Die Wichtigkeit wird für die CPU-Zeitplanung verwendet. Abfragen, die mit hoher Wichtigkeit ausgeführt werden, erhalten dreimal mehr CPU-Zyklen als Abfragen mit mittlerer Wichtigkeit. Daher wird anhand von Parallelitätsslotzuordnungen auch die CPU-Priorität bestimmt. Wenn für eine Abfrage 16 oder mehr Slots verwendet werden, ist die Wichtigkeit hoch.

In der folgenden Tabelle sind die Wichtigkeitszuordnungen für die einzelnen Workloadgruppen angegeben.

### <a name="workload-group-mappings-to-concurrency-slots-and-importance"></a>Zuordnungen von Workloadgruppen zu Parallelitätsslots und Wichtigkeit
| Workloadgruppen | Zuordnung von Parallelitätsslots | MB/Verteilung | Wichtigkeitszuordnung |
|:--- |:---:|:---:|:--- |
| SloDWGroupC00 |1 |100 |Mittel |
| SloDWGroupC01 |2 |200 |Mittel |
| SloDWGroupC02 |4 |400 |Mittel |
| SloDWGroupC03 |8 |800 |Mittel |
| SloDWGroupC04 |16 |1.600 |Hoch |
| SloDWGroupC05 |32 |3.200 |Hoch |
| SloDWGroupC06 |64 |6.400 |Hoch |
| SloDWGroupC07 |128 |12.800 |Hoch |

In der Tabelle **Zuordnung und Verbrauch von Parallelitätsslots** sehen Sie, dass ein DW500 jeweils 1, 4, 8 bzw. 16 Parallelitätsslots für smallrc-, mediumrc-, largerc- und xlargerc-Ressourcenklassen verwendet. Schlagen Sie diese Werte in der vorherigen Tabelle nach, um die Wichtigkeit für jede Ressourcenklasse zu ermitteln.

### <a name="dw500-mapping-of-resource-classes-to-importance"></a>DW500-Zuordnung zwischen Ressourcenklassen und Wichtigkeit
| Ressourcenklasse | Workloadgruppe | Verwendete Parallelitätsslots | MB/Verteilung | Wichtigkeit |
|:--- |:--- |:---:|:---:|:--- |
| smallrc |SloDWGroupC00 |1 |100 |Mittel |
| mediumrc |SloDWGroupC02 |4 |400 |Mittel |
| largerc |SloDWGroupC03 |8 |800 |Mittel |
| xlargerc |SloDWGroupC04 |16 |1.600 |Hoch |
| staticrc10 |SloDWGroupC00 |1 |100 |Mittel |
| staticrc20 |SloDWGroupC01 |2 |200 |Mittel |
| staticrc30 |SloDWGroupC02 |4 |400 |Mittel |
| staticrc40 |SloDWGroupC03 |8 |800 |Mittel |
| staticrc50 |SloDWGroupC03 |16 |1.600 |Hoch |
| staticrc60 |SloDWGroupC03 |16 |1.600 |Hoch |
| staticrc70 |SloDWGroupC03 |16 |1.600 |Hoch |
| staticrc80 |SloDWGroupC03 |16 |1.600 |Hoch |

Sie können die folgende DMV-Abfrage verwenden, um die Unterschiede bei der Arbeitsspeicherressourcen-Zuweisung detailliert aus Sicht des Resource Governors anzuzeigen. Außerdem können Sie die derzeitige und zurückliegende Nutzung der Workloadgruppen bei der Problembehandlung analysieren.

```sql
WITH rg
AS
(   SELECT  
     pn.name                        AS node_name
    ,pn.[type]                        AS node_type
    ,pn.pdw_node_id                    AS node_id
    ,rp.name                        AS pool_name
    ,rp.max_memory_kb*1.0/1024                AS pool_max_mem_MB
    ,wg.name                        AS group_name
    ,wg.importance                    AS group_importance
    ,wg.request_max_memory_grant_percent        AS group_request_max_memory_grant_pcnt
    ,wg.max_dop                        AS group_max_dop
    ,wg.effective_max_dop                AS group_effective_max_dop
    ,wg.total_request_count                AS group_total_request_count
    ,wg.total_queued_request_count            AS group_total_queued_request_count
    ,wg.active_request_count                AS group_active_request_count
    ,wg.queued_request_count                AS group_queued_request_count
    FROM    sys.dm_pdw_nodes_resource_governor_workload_groups wg
    JOIN    sys.dm_pdw_nodes_resource_governor_resource_pools rp    
            ON  wg.pdw_node_id  = rp.pdw_node_id
            AND wg.pool_id      = rp.pool_id
    JOIN    sys.dm_pdw_nodes pn
            ON    wg.pdw_node_id    = pn.pdw_node_id
    WHERE   wg.name like 'SloDWGroup%'
        AND     rp.name = 'SloDWPool'
)
SELECT    pool_name
,        pool_max_mem_MB
,        group_name
,        group_importance
,        (pool_max_mem_MB/100)*group_request_max_memory_grant_pcnt AS max_memory_grant_MB
,        node_name
,        node_type
,       group_total_request_count
,       group_total_queued_request_count
,       group_active_request_count
,       group_queued_request_count
FROM    rg
ORDER BY
    node_name
,    group_request_max_memory_grant_pcnt
,    group_importance
;
```

## <a name="queries-that-honor-concurrency-limits"></a>Abfragen mit Berücksichtigung von Parallelitätslimits
Die meisten Abfragen werden über Ressourcenklassen gesteuert. Diese Abfragen müssen die Schwellenwerte sowohl für gleichzeitige Abfragen als auch für Parallelität einhalten. Ein Benutzer kann eine Abfrage nicht aus dem Parallelitätsslotmodell ausschließen.

Um es noch einmal zu wiederholen: Die folgenden Anweisungen berücksichtigen Ressourcenklassen:

* INSERT-SELECT
* UPDATE
* DELETE
* SELECT (wenn Benutzertabellen abgefragt werden)
* ALTER INDEX REBUILD
* ALTER INDEX REORGANIZE
* ALTER TABLE REBUILD
* CREATE INDEX
* CREATE CLUSTERED COLUMNSTORE INDEX
* CREATE TABLE AS SELECT (CTAS)
* Laden von Daten
* Datenverschiebungsvorgänge, die vom DMS (Data Movement Service) ausgeführt werden

## <a name="query-exceptions-to-concurrency-limits"></a>Ausnahmen von Parallelitätslimits für Abfragen
Einige Abfragen berücksichtigen die Ressourcenklasse nicht, der der Benutzer zugeordnet ist. Diese Ausnahmen von den Parallelitätslimits werden zugelassen, wenn für einen bestimmten Befehl nicht viele Arbeitsspeicherressourcen zur Verfügung stehen. Dies ist häufig der Fall, wenn es sich bei dem Befehl um einen Metadatenvorgang handelt. Ziel dieser Ausnahmen ist es, zu vermeiden, dass Abfragen eine größere Menge an Arbeitsspeicher zugewiesen wird, die diese niemals benötigen. In diesen Fällen wird immer die Standardressourcenklasse (die kleine Ressourcenklasse, „smallrc“) verwendet, unabhängig von der Ressourcenklasse, die dem Benutzer eigentlich zugewiesen ist. Beispielsweise wird `CREATE LOGIN` immer in „smallrc“ ausgeführt. Da zum Ausführen dieses Vorgangs nur sehr wenig Ressourcen erforderlich sind, ist es nicht sinnvoll, die Abfrage in das Parallelitätsslotmodell aufzunehmen.  Diese Abfragen sind auch nicht durch das Parallelitätslimit von 32 Benutzern beschränkt. Eine unbegrenzte Anzahl dieser Abfragen kann bis zum Sitzungslimit von 1.024 Sitzungen ausgeführt werden.

Die folgenden Anweisungen berücksichtigen Ressourcenklassen nicht:

* CREATE oder DROP TABLE
* ALTER TABLE ... SWITCH, SPLIT oder MERGE PARTITION
* ALTER INDEX DISABLE
* DROP INDEX
* CREATE, UPDATE oder DROP STATISTICS
* TRUNCATE TABLE
* ALTER AUTHORIZATION
* CREATE LOGIN
* CREATE, ALTER oder DROP USER
* CREATE, ALTER oder DROP PROCEDURE
* CREATE oder DROP VIEW
* INSERT VALUES
* SELECT aus Systemsichten und DMVs
* EXPLAIN
* DBCC

<!--
Removed as these two are not confirmed / supported under SQLDW
- CREATE REMOTE TABLE AS SELECT
- CREATE EXTERNAL TABLE AS SELECT
- REDISTRIBUTE
-->

##  <a name="changing-user-resource-class-example"></a> Beispiel: Ändern der Ressourcenklasse eines Benutzers
1. **Anmeldung erstellen**: Öffnen Sie eine Verbindung mit Ihrer **master**-Datenbank auf der SQL Server-Instanz, die Ihre SQL Data Warehouse-Datenbank hostet, und führen Sie die folgenden Befehle aus.
   
    ```sql
    CREATE LOGIN newperson WITH PASSWORD = 'mypassword';
    CREATE USER newperson for LOGIN newperson;
    ```
   
   > [!NOTE]
   > Es ist eine gute Idee, einen Benutzer in der Masterdatenbank für Azure SQL Data Warehouse-Benutzer zu erstellen. Das Erstellen eines Benutzers in der Masterdatenbank ermöglicht einem Benutzer, sich mit Tools wie SSMS ohne Angabe eines Datenbanknamens anzumelden.  Außerdem kann er mit dem Objekt-Explorer alle Datenbanken auf einer SQL Server-Instanz anzeigen.  Weitere Informationen zum Erstellen und Verwalten von Benutzern finden Sie unter [Sichern einer Datenbank in SQL Data Warehouse][Secure a database in SQL Data Warehouse].
   > 
   > 
2. **SQL Data Warehouse-Benutzer erstellen**: Öffnen Sie eine Verbindung mit der **SQL Data Warehouse**-Datenbank, und führen Sie den folgenden Befehl aus.
   
    ```sql
    CREATE USER newperson FOR LOGIN newperson;
    ```
3. **Berechtigungen erteilen**: Das folgende Beispiel gewährt die Berechtigung `CONTROL` für die **SQL Data Warehouse**-Datenbank. `CONTROL` auf Datenbankebene entspricht „db_owner“ in SQL Server.
   
    ```sql
    GRANT CONTROL ON DATABASE::MySQLDW to newperson;
    ```
4. **Ressourcenklasse heraufsetzen:** Verwenden Sie die folgende Abfrage, um einen Benutzer zu einer Rolle mit höheren Berechtigungen für die Workloadverwaltung hinzuzufügen.
   
    ```sql
    EXEC sp_addrolemember 'largerc', 'newperson'
    ```
5. **Ressourcenklasse herabsetzen:** Verwenden Sie die folgende Abfrage, um einen Benutzer aus einer Workloadverwaltungsrolle zu entfernen.
   
    ```sql
    EXEC sp_droprolemember 'largerc', 'newperson';
    ```
   
   > [!NOTE]
   > Es ist nicht möglich, einen Benutzer aus „smallrc“ zu entfernen.
   > 
   > 

## <a name="queued-query-detection-and-other-dmvs"></a>Erkennung von Abfragen in der Warteschlange und andere DMVs
Sie können die DMV `sys.dm_pdw_exec_requests` verwenden, um Abfragen zu identifizieren, die in eine Parallelitätswarteschlange eingereiht wurden. Abfragen, die auf einen Parallelitätsslot warten, weisen den Status **Angehalten**auf.

```sql
SELECT      r.[request_id]                 AS Request_ID
        ,r.[status]                 AS Request_Status
        ,r.[submit_time]             AS Request_SubmitTime
        ,r.[start_time]                 AS Request_StartTime
        ,DATEDIFF(ms,[submit_time],[start_time]) AS Request_InitiateDuration_ms
        ,r.resource_class                         AS Request_resource_class
FROM    sys.dm_pdw_exec_requests r;
```

Workloadverwaltungsrollen können mit `sys.database_principals`angezeigt werden.

```sql
SELECT  ro.[name]           AS [db_role_name]
FROM    sys.database_principals ro
WHERE   ro.[type_desc]      = 'DATABASE_ROLE'
AND     ro.[is_fixed_role]  = 0;
```

Mit der folgenden Abfrage wird angezeigt, welcher Rolle die einzelnen Benutzer zugewiesen sind.

```sql
SELECT     r.name AS role_principal_name
        ,m.name AS member_principal_name
FROM    sys.database_role_members rm
JOIN    sys.database_principals AS r            ON rm.role_principal_id        = r.principal_id
JOIN    sys.database_principals AS m            ON rm.member_principal_id    = m.principal_id
WHERE    r.name IN ('mediumrc','largerc', 'xlargerc');
```

SQL Data Warehouse verfügt über die folgenden Wartetypen:

* **LocalQueriesConcurrencyResourceType**: Bezieht sich auf Abfragen, die sich außerhalb des Parallelitätsslot-Frameworks befinden. DMV-Abfragen und Systemfunktionen wie `SELECT @@VERSION` sind Beispiele für lokale Abfragen.
* **UserConcurrencyResourceType**: Bezieht sich auf Abfragen, die sich innerhalb des Parallelitätsslot-Frameworks befinden. Abfragen für Endbenutzertabellen sind ein Beispiel für die Verwendung dieses Ressourcentyps.
* **DmsConcurrencyResourceType**: Bezieht sich auf Wartezeiten, die sich aufgrund von Datenverschiebungen ergeben.
* **BackupConcurrencyResourceType**: Diese Wartezeit gibt an, dass eine Datenbank gesichert wird. Der maximale Wert für diesen Ressourcentyp ist 1. Wenn mehrere Sicherungen gleichzeitig angefordert wurden, werden die restlichen Sicherungen in die Warteschlange eingereiht.

Mit der DMV `sys.dm_pdw_waits` kann angezeigt werden, auf welche Ressourcen eine Abfrage wartet.

```sql
SELECT  w.[wait_id]
,       w.[session_id]
,       w.[type]            AS Wait_type
,       w.[object_type]
,       w.[object_name]
,       w.[request_id]
,       w.[request_time]
,       w.[acquire_time]
,       w.[state]
,       w.[priority]
,    SESSION_ID()            AS Current_session
,    s.[status]            AS Session_status
,    s.[login_name]
,    s.[query_count]
,    s.[client_id]
,    s.[sql_spid]
,    r.[command]            AS Request_command
,    r.[label]
,    r.[status]            AS Request_status
,    r.[submit_time]
,    r.[start_time]
,    r.[end_compile_time]
,    r.[end_time]
,    DATEDIFF(ms,r.[submit_time],r.[start_time])        AS Request_queue_time_ms
,    DATEDIFF(ms,r.[start_time],r.[end_compile_time])    AS Request_compile_time_ms
,    DATEDIFF(ms,r.[end_compile_time],r.[end_time])        AS Request_execution_time_ms
,    r.[total_elapsed_time]
FROM    sys.dm_pdw_waits w
JOIN    sys.dm_pdw_exec_sessions s  ON w.[session_id] = s.[session_id]
JOIN    sys.dm_pdw_exec_requests r  ON w.[request_id] = r.[request_id]
WHERE    w.[session_id] <> SESSION_ID();
```

Mit der DMV `sys.dm_pdw_resource_waits` werden nur die Ressourcenwartezeiten einer bestimmten Abfrage angezeigt. Mit der Ressourcenwartezeit wird nur gemessen, wie lange auf die Bereitstellung von Ressourcen gewartet wird. Bei der Signalwartezeit (Signal Wait Time) wird dagegen gemessen, wie lange die zugrunde liegenden SQL Server-Instanzen zum Planen der Abfrage in der CPU benötigen.

```sql
SELECT  [session_id]
,       [type]
,       [object_type]
,       [object_name]
,       [request_id]
,       [request_time]
,       [acquire_time]
,       DATEDIFF(ms,[request_time],[acquire_time])  AS acquire_duration_ms
,       [concurrency_slots_used]                    AS concurrency_slots_reserved
,       [resource_class]
,       [wait_id]                                   AS queue_position
FROM    sys.dm_pdw_resource_waits
WHERE    [session_id] <> SESSION_ID();
```

Die DMV `sys.dm_pdw_wait_stats` kann für Verlaufsanalysen von Wartezeiten verwendet werden.

```sql
SELECT    w.[pdw_node_id]
,        w.[wait_name]
,        w.[max_wait_time]
,        w.[request_count]
,        w.[signal_time]
,        w.[completed_count]
,        w.[wait_time]
FROM    sys.dm_pdw_wait_stats w;
```

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Verwalten von Datenbankbenutzern und der Sicherheit finden Sie unter [Sichern einer Datenbank in SQL Data Warehouse][Secure a database in SQL Data Warehouse]. Weitere Informationen dazu, wie größere Ressourcenklassen die Qualität von gruppierten Columnstore-Indizes verbessern können, finden Sie unter [Neuerstellen von Indizes zur Verbesserung der Segmentqualität].

<!--Image references-->

<!--Article references-->
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md
[Neuerstellen von Indizes zur Verbesserung der Segmentqualität]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Secure a database in SQL Data Warehouse]: ./sql-data-warehouse-overview-manage-security.md

<!--MSDN references-->
[Managing Databases and Logins in Azure SQL Database]:https://msdn.microsoft.com/library/azure/ee336235.aspx

<!--Other Web references-->


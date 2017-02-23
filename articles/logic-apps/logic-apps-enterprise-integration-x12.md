---
title: Erstellen einer X12-Vereinbarung in Azure-Logik-Apps | Microsoft-Dokumentation
description: "Informationen zum Erstellen einer X12-Vereinbarung für das Enterprise Integration Pack | Azure-Logik-Apps"
services: logic-apps
documentationcenter: .net,nodejs,java
author: padmavc
manager: anneta
editor: cgronlun
ms.assetid: 7422d2d5-b1c7-4a11-8c9b-0d8cfa463164
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: 1a982309fe771c0c244c691ae648bd5f4844a825
ms.openlocfilehash: f7b61b85cadfabde6637e46f0e65108a29901198


---
# <a name="enterprise-integration-with-x12"></a>Unternehmensintegration mit X12

## <a name="prereqs"></a>Voraussetzungen
Bevor Sie X12-Nachrichten austauschen können, müssen Sie eine X12-Vereinbarung erstellen und in Ihrem Integrationskonto speichern. In den folgenden Schritten werden Sie durch das Erstellen einer X12-Vereinbarung begleitet.

* Ein in Ihrem Azure-Abonnement definiertes [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-accounts.md)  
* Mindestens zwei [Partner](../logic-apps/logic-apps-enterprise-integration-partners.md), die mit EDI X12-Qualifizierer unter Geschäftsidentitäten konfiguriert wurden   
* Ein erforderliches [Schema](../logic-apps/logic-apps-enterprise-integration-schemas.md), das in das [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-accounts.md) hochgeladen werden muss

## <a name="create-an-x12-agreement"></a>Erstellen einer X12-Vereinbarung
Nach der Anmeldung beim [Azure-Portal](http://portal.azure.com "Azure-Portal"):  

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com "Azure-Portal") an.
2. Wählen Sie **Weitere Dienste** aus, und geben Sie im Filtersuchfeld den Begriff **Integration** ein. Wählen Sie in der Ergebnisliste **Integrationskonten** aus:    
![](./media/logic-apps-enterprise-integration-agreements/overview-1.png)    
3. Wählen Sie das Integrationskonto aus, dem Sie das Zertifikat hinzufügen möchten:    
![](./media/logic-apps-enterprise-integration-overview/overview-3.png)   
4. Wählen Sie die Kachel **Vereinbarungen** aus. Wenn die Kachel „Vereinbarungen“ nicht angezeigt wird, fügen Sie sie hinzu:     
![](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)   
5. Wählen Sie auf dem geöffneten Blatt „Vereinbarungen“ die Schaltfläche **Hinzufügen** aus.   
![](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)     
6. Geben Sie einen **Namen** für die Vereinbarung ein. Wählen Sie dann auf dem geöffneten Blatt „Vereinbarungen“ den **Vereinbarungstyp**, den **Hostpartner**, die **Hostidentität**, den **Gastpartner** und die **Gastidentität** aus.    
![](./media/logic-apps-enterprise-integration-x12/x12-1.png)  

| Eigenschaft | Beschreibung |
| --- | --- |
| Name |Name der Vereinbarung |
| Vereinbarungstyp | Muss X12 sein |
| Hostpartner |Eine Vereinbarung benötigt einen Host- und einen Gastpartner. Der Hostpartner stellt die Organisation dar, die die Vereinbarung konfiguriert. |
| Hostidentität |Ein Bezeichner für den Hostpartner. |
| Gastpartner |Eine Vereinbarung benötigt einen Host- und einen Gastpartner. Der Gastpartner stellt die Organisation dar, die Geschäfte mit dem Hostpartner tätigt. |
| Gastidentität |Ein Bezeichner für den Gastpartner. |
| Empfangseinstellungen |Diese Eigenschaften gelten für alle Nachrichten, die von einer Vereinbarung empfangen werden. |
| Sendeeinstellungen |Diese Eigenschaften gelten für alle Nachrichten, die von einer Vereinbarung gesendet werden. |  

> [!NOTE]
> Die Auflösung der X12-Vereinbarung hängt von der Übereinstimmung des Absenderqualifizierers und Bezeichners sowie des Empfängerqualifizierers und Bezeichners ab, die beim Partner und in der eingehenden Nachricht definiert sind.  Wenn es bei diesen Werten beim Partner Änderungen gibt, bearbeiten Sie auch die Vereinbarung.
> 
> 

## <a name="receive-settings"></a>Empfangseinstellungen

1. Wählen Sie **Empfangseinstellungen** aus, um zu konfigurieren, wie Nachrichten, die über diese Vereinbarung empfangen werden, behandelt werden.  
2. Das Steuerelement „Empfangseinstellungen“ ist in die folgenden Abschnitte unterteilt, einschließlich Bezeichnern, Bestätigung, Schemas, Umschläge, Kontrollnummern, Überprüfungen und interne Einstellungen. Konfigurieren Sie diese Eigenschaften basierend auf Ihrer Vereinbarung mit dem Partner, mit dem Sie Nachrichten austauschen. Es folgt eine Übersicht über dieser Steuerelemente. Konfigurieren Sie sie so, wie diese Vereinbarung eingehende Nachrichten identifizieren und verarbeiten soll.  

### <a name="identifiers"></a>Bezeichner

![](./media/logic-apps-enterprise-integration-x12/x12-2.png)  

| Eigenschaft | Beschreibung |
| --- | --- |
| ISA1 (Autorisierungsqualifizierer) |Wählen Sie den Wert für den Autorisierungsqualifizierer aus der Dropdownliste. |
| ISA2 |Optional. Geben Sie den Wert für den Autorisierungsqualifizierer ein. Wenn Sie für ISA1 einen anderen Wert als 00 eingegeben haben, geben Sie mindestens ein alphanumerisches Zeichen und maximal 10 ein. |
| ISA3 (Sicherheitsqualifizierer) |Wählen Sie den Wert für den Sicherheitsqualifizierer aus der Dropdownliste. |
| ISA4 |Optional. Geben Sie den Wert für Sicherheitsinformationen ein. Wenn Sie für ISA3 einen anderen Wert als 00 eingegeben haben, geben Sie mindestens ein alphanumerisches Zeichen und maximal 10 ein. |

### <a name="acknowledgments"></a>Danksagungen

![](./media/logic-apps-enterprise-integration-x12/x12-3.png) 

| Eigenschaft | Beschreibung |
| --- | --- |
| TA1 erwartet |Gibt eine technische Bestätigung an den Absender des Austauschs zurück. |
| FA erwartet |Gibt eine Funktionsbestätigung an den Absender des Austauschs zurück. Wählen Sie dann basierend auf den Schemaversionen, mit denen Sie arbeiten, ob Sie die Bestätigungen des Typs 997 oder 999 wünschen. |
| AK2/IK2-Schleife einschließen |Aktiviert die Generierung von AK2-Schleifen in Funktionsbestätigungen für akzeptierte Transaktionssätze |


### <a name="schemas"></a>Schemas
Wählen Sie ein Schema für jeden Transaktionstyp (ST1) und jede Absenderanwendung (GS2). Die Empfangspipeline disassembliert die eingehende Nachricht durch Abstimmen der Werte für ST1 und GS2 in der eingehenden Nachricht mit den hier festgelegten Werten und des Schemas der eingehenden Nachricht mit dem Schema, das Sie hier festlegen.

![](./media/logic-apps-enterprise-integration-x12/x12-33.png) 

| Eigenschaft | Beschreibung |
| --- | --- |
| Version |Wählen Sie die X12-Version. |
| Transaktionstyp (ST01) |Wählen Sie den Transaktionstyp. |
| Absenderanwendung (GS02) |Wählen Sie die Absenderanwendung. |
| Schema |Wählen Sie die Schemadatei, die Sie verwenden möchten. Schemas werden mit Ihrem Integrationskonto hinzugefügt. |

> [!NOTE]
> Konfigurieren Sie das erforderliche [Schema](../logic-apps/logic-apps-enterprise-integration-schemas.md), das in das [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-accounts.md) hochgeladen wird.
> 
> 

### <a name="envelopes"></a>Umschläge

![](./media/logic-apps-enterprise-integration-x12/x12-34.png) 

| Eigenschaft | Beschreibung |
| --- | --- |
| ISA11-Verwendung |Verwenden Sie dieses Feld, um das Trennzeichen in einem Transaktionssatz anzugeben:</br></br>Wählen Sie das Wiederholungstrennzeichen, um das Trennzeichen für wiederholte Vorkommen eines einfachen Datenelements oder einer wiederholten Datenstruktur anzugeben. anstelle der Dezimalschreibweise des eingehenden Dokuments in der EDI-Empfangspipeline zu verwenden.</br></br>Wählen Sie das Wiederholungstrennzeichen, um das Trennzeichen für wiederholte Vorkommen eines einfachen Datenelements oder einer wiederholten Datenstruktur anzugeben. (^) wird z.B. normalerweise als Wiederholungstrennzeichen verwendet. Für HIPAA-Schemas können Sie nur (^) verwenden. |

### <a name="control-numbers"></a>Kontrollnummern

![](./media/logic-apps-enterprise-integration-x12/x12-35.png) 

| Eigenschaft | Beschreibung |
| --- | --- |
| Doppelte Austauschkontrollnummern nicht zulassen |Blockiert doppelte Austauschvorgänge. Vergleicht die Austauschkontrollnummer (ISA13) mit der empfangenen Austauschkontrollnummer. Wenn eine Übereinstimmung gefunden wird, verarbeitet die Empfangspipeline den Austausch nicht. Sie können die Anzahl der Tage angeben, an denen diese Überprüfung durchgeführt wird, indem Sie den entsprechenden Wert für *Innerhalb der folgenden Anzahl von Tagen auf ISA13-Duplikate prüfen* angeben. |
| Doppelte Gruppenkontrollnummern nicht zulassen |Blockiert Austauschvorgänge mit doppelten Gruppenkontrollnummern |
| Doppelte Transaktionssatz-Kontrollnummern nicht zulassen |Blockiert Austauschvorgänge mit doppelten Transaktionssatz-Kontrollnummern |

### <a name="validations"></a>Überprüfungen

![](./media/logic-apps-enterprise-integration-x12/x12-36.png) 

| Eigenschaft | Beschreibung |
| --- | --- |
| Nachrichtentyp |EDI-Nachrichtentypen, z.B. „850-Bestellung“ bzw. „999-Implementierungsbestätigung“. |
| EDI-Überprüfung |Führt EDI-Überprüfung für Datentypen gemäß den EDI-Eigenschaften Schema, Längeneinschränkungen, leere Datenelemente und nachfolgende Trennzeichen vor. |
| Erweiterte Überprüfung |Wenn der Datentyp nicht EDI ist, werden Datenelementanforderung und zulässige Wiederholung, Aufzählungen und Datenelementlänge (Min./Max.) überprüft. |
| Führende/nachfolgende Nullen zulassen |Zusätzliche führende oder nachfolgende Leer- und Nullzeichen werden beibehalten. Sie werden nicht entfernt. |
| Richtlinie für nachfolgende Trennzeichen |Generiert nachfolgende Trennzeichen für den empfangenen Austausch. Die Optionen umfassen „Nicht zulässig“, „Optional“ und „Erforderlich“. |

### <a name="internal-settings"></a>Interne Einstellungen

![](./media/logic-apps-enterprise-integration-x12/x12-37.png) 

| Eigenschaft | Beschreibung |
| --- | --- |
| Implizite Dezimalzahlen im Nn-Format in Zehnerlogarithmuswerte einer Zahl konvertieren |Konvertiert einen im Nn-Format angegebenen EDI-Wert in einen Zehnerlogarithmuswert. |
| Leere XML-Tags erstellen, wenn nachfolgende Trennzeichen zulässig sind |Aktivieren Sie dieses Kontrollkästchen, damit der Austauschabsender leere XML-Tags für nachfolgende Trennzeichen einbezieht. |
| Austausch in Transaktionssätze trennen – Transaktionssätze bei Fehler anhalten|Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument, indem der geeignete Umschlag auf den Transaktionssatz angewendet wird. Hält nur die Transaktion an, bei denen die Überprüfung fehlschlägt. |
| Austausch in Transaktionssätze trennen – Austausch bei Fehler anhalten|Analysiert jeden Transaktionssatz in einem Austausch in ein separates XML-Dokument, indem der geeignete Umschlag angewendet wird. Hält den gesamten Austausch an, wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht. | 
| Austausch beibehalten – Transaktionssätze bei Fehler anhalten |Behält den Austausch bei und erstellt ein XML-Dokument für den gesamten Batchaustausch. Hält nur die Transaktionssätze an, die die Überprüfung nicht bestehen, während alle weiteren Transaktionssätze verarbeitet werden. |
| Austausch beibehalten – Austausch bei Fehler anhalten |Behält den Austausch bei und erstellt ein XML-Dokument für den gesamten Batchaustausch.  Hält den gesamten Austausch an, wenn mindestens ein Transaktionssatz im Austausch die Überprüfung nicht besteht. |

Nachdem Sie die Eigenschaften der Empfangseinstellungen festgelegt haben, wählen Sie die Schaltfläche **OK**.  
   
Ihre Vereinbarung ist bereit, eingehende Nachrichten zu verarbeiten, die dem Schema entsprechen, das Sie ausgewählt haben.

## <a name="send-settings"></a>Sendeeinstellungen

1. Wählen Sie **Sendeeinstellungen** aus, um zu konfigurieren, wie Nachrichten, die über diese Vereinbarung gesendet werden, behandelt werden.  

2. Das Steuerelement „Sendeeinstellungen“ ist in die folgenden Abschnitte unterteilt, einschließlich Bezeichnern, Bestätigung, Schemas, Umschläge, Kontrollnummern, Zeichensätze und Trennzeichen sowie Überprüfung.  Konfigurieren Sie diese Eigenschaften basierend auf Ihrer Vereinbarung mit dem Partner, mit dem Sie Nachrichten austauschen. Hier ist eine Ansicht dieser Steuerelemente angegeben. Konfigurieren Sie sie so, wie diese Vereinbarung ausgehende Nachrichten identifizieren und verarbeiten soll:


### <a name="identifiers"></a>Bezeichner

![](./media/logic-apps-enterprise-integration-x12/x12-4.png)  

| Eigenschaft | Beschreibung |
| --- | --- |
| Autorisierungsqualifizierer (ISA1) |Wählen Sie den Wert für den Autorisierungsqualifizierer aus der Dropdownliste. |
| ISA2 |Geben Sie den Wert für den Autorisierungsqualifizierer ein. Wenn dieser Wert nicht 00 ist, geben Sie mindestens ein alphanumerisches Zeichen und maximal 10 ein. |
| Sicherheitsqualifizierer (ISA3) |Wählen Sie den Wert für den Sicherheitsqualifizierer aus der Dropdownliste. |
| ISA4 |Geben Sie den Wert für Sicherheitsinformationen ein. Wenn dieser Wert nicht 00 ist, geben Sie für den Wert (ISA4) im Textfeld mindestens ein alphanumerisches Zeichen und maximal 10 ein. |

### <a name="acknowledgment"></a>Bestätigung

![](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Eigenschaft | Beschreibung |
| --- | --- |
| TA1 erwartet |Wählen Sie dieses Kontrollkästchen, um eine technische Bestätigung (TA1) an den Austauschabsender zurückzugeben. Diese Einstellung gibt an, dass der Hostpartner, der die Nachricht sendet, eine Bestätigung vom Gastpartner in der Vereinbarung anfordert. Diese Bestätigungen werden vom Hostpartner basierend auf den Empfangseinstellungen der Vereinbarung erwartet. |
| FA erwartet |Aktivieren Sie dieses Kontrollkästchen, um eine funktionale Bestätigung (FA) an den Absender des Austauschs zu senden, und wählen Sie dann, basierend auf den Schemaversionen, mit denen Sie arbeiten, ob Sie die 997- oder 999-Bestätigungen senden möchten. Diese Bestätigungen werden vom Hostpartner basierend auf den Empfangseinstellungen der Vereinbarung erwartet. |
| FA-Version |Wählen Sie die FA-Version. |

### <a name="schemas"></a>Schemas

![](./media/logic-apps-enterprise-integration-x12/x12-5.png)  

| Eigenschaft | Beschreibung |
| --- | --- |
| Version |Wählen Sie die X12-Version. |
| Transaktionstyp (ST01) |Wählen Sie den Transaktionstyp. |
| Schema |Wählen Sie das zu verwendende Schema aus. Schemas befinden sich in Ihrem Integrationskonto. Wenn Sie das Schema zuerst ausgewählt haben, konfiguriert es automatisch die Version und den Transaktionstyp.  |

> [!NOTE]
> Konfigurieren Sie das erforderliche [Schema](../logic-apps/logic-apps-enterprise-integration-schemas.md), das in das [Integrationskonto](../logic-apps/logic-apps-enterprise-integration-accounts.md) hochgeladen wird.
> 
> 

### <a name="envelopes"></a>Umschläge

![](./media/logic-apps-enterprise-integration-x12/x12-6.png) 

| Eigenschaft | Beschreibung |
| --- | --- |
| ISA11-Verwendung |Verwenden Sie dieses Feld, um das Trennzeichen in einer Transaktion anzugeben |
| Standardbezeichner |Wählen Sie das Wiederholungstrennzeichen, um das Trennzeichen für wiederholte Vorkommen eines einfachen Datenelements oder einer wiederholten Datenstruktur anzugeben. |
| Wiederholungstrennzeichen |Wählen Sie das Wiederholungstrennzeichen, um das Trennzeichen für wiederholte Vorkommen eines einfachen Datenelements oder einer wiederholten Datenstruktur anzugeben. (^) wird z.B. normalerweise als Wiederholungstrennzeichen verwendet. Für HIPAA-Schemas können Sie nur (^) verwenden. |

### <a name="control-numbers"></a>Kontrollnummern

![](./media/logic-apps-enterprise-integration-x12/x12-8.png) 

| Eigenschaft | Beschreibung |
| --- | --- |
| Kontrollversionsnummer (ISA12) |Wählen der Version des X12-Standards |
| Verwendungsindikator (ISA15) |Wählen des Kontexts eines Austauschs.  Die Werte sind Informationen, Produktionsdaten oder Testdaten. |
| SCHEMA |Generiert die GS- und ST-Segmente für einen X12-codierten Austausch, der an die Sendepipeline gesendet wird. |
| GS1 |Optional, einen Wert für den Funktionscode in der Dropdownliste wählen |
| GS2 |Optional, Anwendungsabsender |
| GS3 |Optional, Anwendungsempfänger |
| GS4 |CCYYMMDD oder YYMMDD wählen |
| GS5 |Optional, HHMM, HHMMSS oder HHMMSSdd wählen |
| GS7 |Optional, einen Wert für die zuständige Agentur in der Dropdownliste wählen |
| GS8 |Optional, Version des Dokuments |
| Austauschkontrollnummer (ISA13) |Erforderlich, einen Wertebereich für die Austauschkontrollnummer eingeben. Geben Sie einen numerischen Wert von mindestens 1 und maximal 999999999 ein. |
| Gruppenkontrollnummer (GS06) |Erforderlich, einen Wertebereich für die Gruppenkontrollnummer eingeben. Geben Sie einen numerischen Wert von mindestens 1 und maximal 999999999 ein. |
| Transaktionssatz-Kontrollnummer (ST02) |Erforderlich, einen Wertebereich für die Transaktionssatz-Kontrollnummer eingeben. Geben Sie einen numerischen Wertebereich von mindestens 1 und maximal 999999999 ein. |
| Präfix |Optional, vorgesehen für den Bereich von Transaktionssatz-Kontrollnummern, die in der Bestätigung verwendet werden. Geben Sie einen numerischen Wert für die beiden mittleren Felder und einen alphanumerischen Wert (falls gewünscht) für die Felder Präfix und Suffix ein. Die mittleren Felder sind erforderlich und enthalten die minimalen und maximalen Werte für die Kontrollnummer. |
| Suffix |Optional, vorgesehen für den Bereich von Transaktionssatz-Kontrollnummern, die in einer Bestätigung verwendet werden. Geben Sie einen numerischen Wert für die beiden mittleren Felder und einen alphanumerischen Wert (falls gewünscht) für die Felder mit dem Präfix und Suffix ein. Die mittleren Felder sind erforderlich und enthalten die minimalen und maximalen Werte für die Kontrollnummer. |

### <a name="character-sets-and-separators"></a>Zeichensätze und Trennzeichen

Anders als beim Zeichensatz können Sie für jeden Nachrichtentyp einen anderen Satz zu verwendender Trennzeichen eingeben. Wenn ein Zeichensatz nicht für ein bestimmtes Nachrichtenschema angegeben ist, wird der Standardzeichensatz verwendet.

![](./media/logic-apps-enterprise-integration-x12/x12-9.png) 


| Eigenschaft | Beschreibung |
| --- | --- |
| Zu verwendender Zeichensatz |Wählen Sie den X12-Zeichensatz zum Überprüfen der Eigenschaften.  Die Optionen sind „Basic“, „Extended“ und „UTF8“. |
| SCHEMA |Wählen Sie in der Dropdownliste ein Schema aus. Wählen Sie für das ausgewählte Schema die zu verwendenden Trennzeichen |
| Eingabetyp |Wählen Sie in der Dropdownliste einen Eingabetyp aus. |
| Trennzeichen für zusammengesetzte Elemente |Geben Sie ein einzelnes Zeichen zum Trennen zusammengesetzter Datenelemente an. |
| Datenelementtrennzeichen |Geben Sie ein einzelnes Zeichen zum Trennen einfacher Datenelemente in zusammengesetzten Datenelementen an. |
| Ersetzungszeichen |Geben Sie ein Ersetzungszeichen an. Beim Generieren der ausgehenden X12-Nachricht werden alle Vorkommen von Trennzeichen in den Nutzlastdaten durch das angegebene Zeichen ersetzt. |
| Segmentabschlusszeichen |Geben Sie ein einzelnes Zeichen ein, um das Ende eines EDI-Segments anzuzeigen. |
| Suffix |Wählen Sie das Zeichen, das mit dem Segmentbezeichner verwendet wird. Wenn Sie ein Suffix angeben, kann das Segmentabschlusszeichen-Datenelement leer sein. Wenn das Segmentabschlusszeichen leer gelassen wird, müssen Sie ein Suffix angeben. |

### <a name="validation"></a>Überprüfen

![](./media/logic-apps-enterprise-integration-x12/x12-10.png) 

| Eigenschaft | Beschreibung |
| --- | --- |
| Nachrichtentyp |Wählen Sie den Nachrichtentyp in der Liste aus. |
| EDI-Überprüfung |Durch Auswahl dieser Option wird die Überprüfung auf dem Austauschempfänger aktiviert. Diese Überprüfung führt die EDI-Überprüfung für Transaktionssatz-Datenelemente durch, wobei Datentypen, Längeneinschränkungen und leere Datenelemente sowie nachfolgende Trennzeichen überprüft werden. |
| Erweiterte Überprüfung |Diese Option ermöglicht die erweiterte Überprüfung von Austauschvorgängen, die vom Austauschabsender empfangen wurden. Dazu gehören zusätzlich zur Überprüfung des XSD-Datentyps die Überprüfung von Feldlänge, Optionalität und Wiederholungsanzahl. Sie können die Erweiterungsüberprüfung aktivieren, ohne die EDI-Überprüfung zu aktivieren, und umgekehrt. |
| Führende/nachfolgende Nullen zulassen |Diese Option gibt an, dass ein von der Partei empfangener EDI-Austausch die Überprüfung auch dann besteht, wenn ein Datenelement in einem EDI-Austausch aufgrund nachfolgender Leerzeichen nicht der Längenanforderung entspricht, jedoch nach Entfernen der Leerzeichen der Längenanforderung entspricht. |
| Nachfolgendes Trennzeichen |Diese Option gibt an, dass ein von der Partei empfangener EDI-Austausch die Überprüfung auch dann besteht, wenn ein Datenelement in einem EDI-Austausch aufgrund führender (oder nachfolgender) Nullen oder Leerzeichen nicht der Längenanforderung entspricht, jedoch nach deren Entfernung der Längenanforderung entspricht.</br></br>Wählen Sie „Nicht zulässig“, wenn Sie keine nachfolgenden Trennzeichen in einem vom Austauschabsender empfangenen Austausch zulassen möchten. Wenn der Austausch nachfolgende Trennzeichen enthält, wird er für ungültig erklärt.</br></br>Wählen Sie „Optional“, um Austauschvorgänge mit oder ohne nachgestellte Trennzeichen zu akzeptieren.</br></br>Wählen Sie „Erforderlich“, wenn der empfangene Austausch nachfolgende Trennzeichen enthalten muss. |

Nachdem Sie die Eigenschaften der Sendeeinstellungen festgelegt haben, klicken Sie auf **OK**.  Ihre Vereinbarung ist bereit, ausgehende Nachrichten zu verarbeiten, die dem Schema entsprechen, das Sie ausgewählt haben.

Klicken Sie auf „OK“, um die Vereinbarung zu erstellen.

Wählen Sie auf dem Blatt „Integrationskonto“ die Kachel **Vereinbarungen** aus, auf dem die neu hinzugefügte Vereinbarung aufgeführt ist.  
![](./media/logic-apps-enterprise-integration-x12/x12-7.png)   

## <a name="learn-more"></a>Weitere Informationen
* [Weitere Informationen zum Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  




<!--HONumber=Feb17_HO1-->



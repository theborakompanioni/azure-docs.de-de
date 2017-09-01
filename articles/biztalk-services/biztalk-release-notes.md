---
title: "Anmerkungen zu dieser Version für Azure BizTalk Services | Microsoft Docs"
description: Beschreibt bekannte Probleme im Zusammenhang mit Azure BizTalk Services
services: biztalk-services
documentationcenter: 
author: msftman
manager: erikre
editor: 
ms.assetid: f4906fdc-4cd9-4a57-a007-a88c2e51a18f
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.translationtype: HT
ms.sourcegitcommit: f76de4efe3d4328a37f86f986287092c808ea537
ms.openlocfilehash: 18ed891a9bba2b4011d3492722a2366d96fb3c01
ms.contentlocale: de-de
ms.lasthandoff: 07/10/2017


---
# <a name="release-notes-for-azure-biztalk-services"></a>Versionshinweise für Azure BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Die Versionshinweise für Microsoft Azure BizTalk Services enthalten die bekannten Probleme dieser Version.

## <a name="whats-new-in-the-november-update-of-biztalk-services"></a>Neuerungen im November-Update von BizTalk Services
* Die Verschlüsselung ruhender Daten kann im BizTalk Services-Portal aktiviert werden. Finden Sie unter [Aktivieren ruhender Verschlüsselung im BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/dn874052.aspx).

## <a name="update-history"></a>Updateverlauf
### <a name="october-update"></a>Update vom Oktober
* Unterstützung von Organisationskonten:  
  * **Szenario**: Sie haben eine BizTalk Service-Bereitstellung mit einem Microsoft-Konto registriert (z.B. user@live.com). In diesem Szenario können nur Microsoft-Kontobenutzer den BizTalk Service im BizTalk Services-Portal verwalten. Ein Organisationskonto kann nicht verwendet werden.  
  * **Szenario**: Sie haben eine BizTalk Service-Bereitstellung, mithilfe eines Organisationskontos in Azure Active Directory registriert (z.B. user@fabrikam.com oder user@contoso.com). In diesem Szenario können nur Azure Active Directory-Benutzer innerhalb derselben Organisation den BizTalk Service im BizTalk Services-Portal verwalten. Ein Microsoft-Konto kann nicht verwendet werden.  
* Wenn Sie einen BizTalk Service im klassischen Azure-Portal erstellen, werden Sie automatisch beim BizTalk Services-Portal registriert.
  * **Szenario**: Sie melden sich beim klassischen Azure-Portal an, erstellen einen BizTalk Service und wählen dann zum ersten Mal **Verwalten** aus. Beim Öffnen des BizTalk Services-Portals wird der BizTalk Service automatisch registriert und steht für Ihre Bereitstellungen zur Verfügung.  
    Weitere Informationen finden Sie unter [Registrieren und Aktualisieren einer BizTalk Service-Bereitstellung im BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/hh689837.aspx).  

### <a name="august-14-update"></a>Update vom 14. August
* Entkopplung von Vereinbarungen und Bridges – Handelspartnervereinbarungen und -Bridges sind nun im BizTalk Services-Portal entkoppelt. Vereinbarungen und Bridges werden nun separat erstellt, und Bridges werden zur Laufzeit anhand der Werte in der EDI-Nachricht zu einer Vereinbarung aufgelöst. Weitere Informationen finden Sie unter [Erstellen von Vereinbarungen in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689908.aspx), [Erstellen einer EDI-Bridge mithilfe des BizTalk Services-Portals](https://msdn.microsoft.com/library/azure/dn793986.aspx), [Erstellen einer AS2-Bridge mithilfe des BizTalk Services-Portals](https://msdn.microsoft.com/library/azure/dn793993.aspx) sowie unter [Wie werden Bridges zur Laufzeit in Vereinbarungen aufgelöst?](https://msdn.microsoft.com/library/azure/dn794001.aspx).  
* Die Option zum Erstellen von Vorlagen für Vereinbarungen wurde entfernt.  
* Für die sendeseitige Vereinbarung können Sie nun unterschiedliche Trennzeichensätze pro Schema angeben. Diese Konfiguration wird unter den Protokolleinstellungen für die sendeseitige Vereinbarung angegeben. Weitere Informationen finden Sie unter [Erstellen einer X12-Vereinbarung in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx) sowie unter [Erstellen einer EDIFACT-Vereinbarung in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/dn606267.aspx). Für den gleichen Zweck wurden auch zwei neue Entitäten zur TPM OM-API hinzugefügt. Weitere Informationen finden Sie unter [X12DelimiterOverrides](https://msdn.microsoft.com/library/azure/dn798749.aspx) sowie unter [EDIFACTDelimiterOverride](https://msdn.microsoft.com/library/azure/dn798748.aspx).  
* Standard-XSD-Konstrukte werden nun unterstützt (inklusive abgeleiteter Typen). Weitere Informationen finden Sie unter [Verwenden von Standard-XSD-Konstrukten in Ihren Zuordnungen](https://msdn.microsoft.com/library/azure/dn793987.aspx) sowie unter [Abgeleitete Typenzuordnung – Szenarios und Beispiele](https://msdn.microsoft.com/library/azure/dn793997.aspx).  
* AS2 unterstützt neue MIC-Algorithmen für die Nachrichtensignierung und neue Verschlüsselungsalgorithmen. Weitere Informationen finden Sie unter [Erstellen einer AS2-Vereinbarung in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689890.aspx).  
  ## <a name="know-issues"></a>Bekannte Probleme

### <a name="connectivity-issues-after-biztalk-services-portal-update"></a>Konnektivitätsprobleme nach der Aktualisierung des BizTalk Services-Portals
  Wenn das BizTalk Services-Portal geöffnet ist, während BizTalk Services zur Implementierung von Dienständerungen aktualisiert wird, treten im BizTalk Services-Portal unter Umständen Konnektivitätsprobleme auf.  
  Als Problemumgehung können Sie den Browser neu starten, den Browsercache löschen oder das Portal im privaten Modus starten.  

### <a name="visual-studio-ide-cannot-locate-the-artifact-if-you-click-an-error-or-warning-in-a-biztalk-services-project"></a>Visual Studio IDE kann das Artefakt nicht finden, wenn Sie in einem BizTalk Services-Projekt auf einen Fehler oder eine Warnung klicken
Installieren Sie Visual Studio 2012 Update 3 RC 1, um das Problem zu beheben.  

### <a name="custom-binding-project-reference"></a>Verweis auf ein benutzerdefiniertes Bindungsprojekt
Betrachten Sie die folgenden Situationen mit einem BizTalk Services-Projekt in einer Visual Studio-Projektmappe:  

* Eine Visual Studio-Projektmappe enthält sowohl ein BizTalk Services-Projekt als auch ein benutzerdefiniertes Bindungsprojekt. Das BizTalk Service-Projekt enthält einen Verweis auf die benutzerdefinierte Bindungsprojektdatei.
* Das BizTalk Service-Projekt enthält einen Verweis auf eine DLL mit einer benutzerdefinierten Bindung und benutzerdefiniertem Verhalten.

Die Projektmappe wird in Visual Studio erfolgreich erstellt. Anschließend erstellen Sie die Projektmappe neu oder bereinigen sie. Bei der wiederholten Neuerstellung oder Bereinigung der Projektmappe tritt der folgende Fehler auf:  
  Die Datei „<Path to DLL>“ konnte nicht nach „bin\Debug\FileName.dll“ kopiert werden. Der Prozess kann nicht auf die Datei „bin\Debug\FileName.dll“ zugreifen, da sie bereits von einem anderen Prozess verwendet wird.  

#### <a name="workaround"></a>Problemumgehung
* Wenn [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) installiert ist, haben Sie folgende Möglichkeiten:
  
  * Starten Sie Visual Studio neu. Oder:
  * Starten Sie die Projektmappe neu. Führen Sie dann lediglich einen Buildvorgang für die Projektmappe aus.  
* Wenn [Visual Studio 2012 Update 3](https://www.microsoft.com/download/details.aspx?id=39305) nicht installiert ist, öffnen Sie den Task-Manager, klicken Sie auf die Registerkarte „Prozesse“, klicken Sie auf den Prozess „MSBuild.exe“, und klicken Sie anschließend auf die Schaltfläche „Prozess beenden“.  

### <a name="routing-to-basichttprelay-endpoints-is-not-supported-from-bridges-and-biztalk-services-portal-if-non-printable-characters-are-promoted-as-http-headers"></a>Weiterleitung an BasicHttpRelay-Endpunkte wird von Bridges und vom BizTalk Services-Portal nicht unterstützt, wenn nicht druckbare Zeichen als HTTP-Header höher gestuft werden
Wenn Sie nicht druckbare Zeichen als Bestandteil höher gestufter Eigenschaften für Nachrichten verwenden, können diese Nachrichten nicht an Relayziele mit BasicHttpRelay-Bindung weitergeleitet werden. Zudem sind die im Rahmen der Nachverfolgung verfügbaren höher gestuften Eigenschaften für Blobs URL-codiert und für Ziele nicht codiert.  

### <a name="mdn-is-sent-asynchronously-even-if-the-send-asynchronous-mdn-option-is-unchecked"></a>MDN wird asynchron gesendet, obwohl die Option „Asynchrone MDN senden“ deaktiviert ist
Szenario: Wenn Sie das Kontrollkästchen **Asynchrone MDN senden** aktivieren, eine URL angeben, an die die asynchrone MDN gesendet werden soll, und anschließend das Kontrollkästchen **Asynchrone MDN senden** wieder deaktivieren, wird die MDN weiterhin an die angegebene URL gesendet, obwohl die Option zum Senden asynchroner MDNs nicht aktiviert ist.  
Löschen Sie zur Umgehung des Problems vor dem Deaktivieren des Kontrollkästchens **Asynchrone MDN senden** die angegebene URL, und stellen Sie dann die AS2-Vereinbarung bereit.  

### <a name="whitespace-characters-beyond-a-valid-interchange-cause-an-empty-message-to-be-sent-to-the-suspend-endpoint"></a>Über den gültigen Austauschbereich hinausgehende Leerzeichen führen dazu, dass eine leere Nachricht an den angehaltenen Endpunkt gesendet wird
Wenn Leerzeichen über ein IEA-Segment hinausgehen, behandelt der Disassembler dies als Ende des aktuellen Austauschs und interpretiert die nächste Gruppe von Leerzeichen als nächste Nachricht. Da es sich hierbei nicht um einen gültigen Austausch handelt, wird möglicherweise eine erfolgreiche Nachricht an das Weiterleitungsziel und eine leere Nachricht an den angehaltenen Endpunkt gesendet.  

### <a name="tracking-in-biztalk-services-portal"></a>Nachverfolgung im BizTalk Services-Portal
Nachverfolgungsereignisse werden bis zur EDI-Nachrichtenverarbeitung und jeglicher Korrelation erfasst. Wenn außerhalb der Protokollphase ein Fehler für eine Nachricht auftritt, zeigt die Nachverfolgung die Nachricht dennoch als erfolgreich verarbeitet an. Suchen Sie in diesem Fall im Abschnitt zur **Nachverfolgung** des Protokollabschnitts in der Spalte **Details** nach entsprechenden Fehlerdetails.
Information zur Protokollphase finden Sie in den X12-Empfangs- und Sendeeinstellungen (siehe[Erstellen einer X12-Vereinbarung in Azure BizTalk Services](https://msdn.microsoft.com/library/azure/hh689847.aspx)).  

### <a name="update-agreement"></a>Vereinbarungsupdate
Im BizTalk Services-Portal können Sie beim Konfigurieren einer Vereinbarung den Qualifizierer einer Identität ändern. Dies kann zu inkonsistenten Eigenschaften führen. Angenommen, es ist eine Vereinbarung mit „ZZ:1234567“ und „ZZ:7654321“ als Qualifizierer vorhanden. In den Profileinstellungen des BizTalk Services-Portals ändern Sie „ZZ:1234567“ in „01:GeänderterWert“. Wenn Sie nun die Vereinbarung öffnen, wird „01:GeänderterWert“ anstelle von „ZZ:1234567“ angezeigt.
Wenn Sie den Qualifizierer einer Identität ändern möchten, löschen Sie die Vereinbarung, aktualisieren Sie **Identitäten** im Partnerprofil, und erstellen Sie die Vereinbarung dann erneut.  

> AZURE.WARNING Dieses Verhalten wirkt sich auf X12 und AS2 aus.  
> 
> 

### <a name="as2-attachments"></a>AS2-Anlagen
Anlagen für AS2-Nachrichten werden beim Senden oder Empfangen nicht unterstützt. Anlagen werden vielmehr ignoriert, und der Nachrichtentext wird als normale AS2-Nachricht verarbeitet.  

### <a name="resources-remembering-path"></a>Ressourcen: Speichern des Pfads
Beim Hinzufügen von **Ressourcen** wird der Pfad, der zuvor zum Hinzufügen einer Ressource verwendet wurde, unter Umständen nicht im Dialogfeld gespeichert. Fügen Sie die Website des BizTalk Services-Portals in Internet Explorer den **vertrauenswürdigen Sites** hinzu, damit der zuvor verwendete Pfad gespeichert wird.  

### <a name="if-you-rename-the-entity-name-of-a-bridge-and-close-the-project-without-saving-changes-opening-the-entity-again-results-in-an-error"></a>Wenn Sie den Entitätsnamen einer Bridge ändern und das Projekt ohne Speichern der Änderungen schließen, tritt beim erneuten Öffnen der Entität ein Fehler auf
Szenario mit Schritten in der angegebenen Reihenfolge:  

* Sie fügen einem BizTalk Service-Projekt eine Bridge (beispielsweise eine unidirektionale XML-Bridge) hinzu.  
* Sie benennen die Bridge um, indem Sie einen Wert für die Eigenschaft „Entitätsname“ angeben. Dadurch wird die zugeordnete BRIDGECONFIG-Datei in den von Ihnen angegebenen Namen umbenannt.  
* Sie schließen die BCS-Datei (durch Schließen der Registerkarte in Visual Studio), ohne die Änderungen zu speichern.  
* Sie öffnen die BCS-Datei im Projektmappen-Explorer.  
  Dabei werden Sie feststellen, dass die zugeordnete BRIDGECONFIG-Datei den neuen Namen enthält, auf der Entwurfsoberfläche aber weiterhin der alte Entitätsname angezeigt wird. Beim Versuch, die Bridgekonfiguration durch Doppelklicken auf die Bridgekomponente zu öffnen, erhalten Sie die folgende Fehlermeldung:  
  `‘<old name>’ Entity’s associated file ‘<old name>.bridgeconfig’ does not exist` Speichern Sie daher nach der Umbenennung der Entitäten in einem BizTalk Service-Projekt unbedingt Ihre Änderungen, um diese Situation zu vermeiden.  
  
### <a name="biztalk-service-project-builds-successfully-even-if-an-artifact-has-been-excluded-from-a-visual-studio-project"></a>Das BizTalk Service-Projekt wird erfolgreich erstellt, obwohl ein Artefakt aus einem Visual Studio-Projekt ausgeschlossen wurde
Szenario: Sie fügen einem BizTalk Service-Projekt ein Artefakt (beispielsweise eine XSD-Datei) hinzu, schließen dieses Artefakt in die Bridgekonfiguration ein (etwa durch Angabe als Anforderungsnachrichtentyp) und schließen es dann aus dem Visual Studio-Projekt aus. In diesem Fall wird beim Erstellen des Projekts kein Fehler angezeigt, solange das gelöschte Artefakt auf dem Datenträger im gleichen Verzeichnis verfügbar ist, aus dem es in das Visual Studio-Projekt eingeschlossen wurde.
  
### <a name="the-biztalk-service-project-does-not-check-for-schema-availability-while-configuring-the-bridges"></a>Beim Konfigurieren der Bridges wird die Schemaverfügbarkeit vom BizTalk Service-Projekt nicht überprüft
Wenn in einem BizTalk Service-Projekt ein dem Projekt hinzugefügtes Schema ein anderes Schema importiert, überprüft das BizTalk Service-Projekt nicht, ob das importierte Schema dem Projekt hinzugefügt wird. Wenn Sie versuchen, ein derartiges Projekt zu erstellen, werden keine Buildfehler angezeigt.
  
### <a name="the-response-message-for-a-xml-request-reply-bridge-is-always-of-charset-utf-8"></a>Die Antwortnachricht für eine XML-Anforderungs-Antwort-Bridge entspricht immer dem UTF-8-Zeichensatz
In dieser Version ist der Zeichensatz der Antwortnachricht einer XML-Anforderungs-Antwort-Bridge immer auf UTF-8 festgelegt.
  
### <a name="user-defined-datatypes"></a>Benutzerdefinierte Datentypen
Die BizTalk Adapter Pack-Adapter des BizTalk Adapter Service-Features können benutzerdefinierte Datentypen für Adaptervorgänge verwenden.
Kopieren Sie bei Verwendung benutzerdefinierter Datentypen die DLL-Dateien in das Verzeichnis „<Laufwerk:>\Programme\Microsoft BizTalk Adapter Service\BAServiceRuntime\bin\“ oder in den globalen Assemblycache (GAC) auf dem Server, auf dem der BizTalk Adapter Service gehostet wird. Andernfalls kann auf dem Client der folgende Fehler auftreten:  
```
<s:Fault xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
<faultcode>s:Client</faultcode>
<faultstring xml:lang="en-US">The UDT with FullName "File, FileUDT, Version=Value, Culture=Value, PublicKeyToken=Value" could not be loaded. Try placing the assembly containing the UDT definition in the Global Assembly Cache.</faultstring>
<detail>
  <AFConnectRuntimeFault xmlns="http://Microsoft.ApplicationServer.Integration.AFConnect/2011" xmlns:i="http://www.w3.org/2001/XMLSchema-instance">
    <ExceptionCode>ERROR_IN_SENDING_MESSAGE</ExceptionCode>
  </AFConnectRuntimeFault>
</detail>
</s:Fault>
```  
  
> [!IMPORTANT]
> Für die Installation einer Datei im globalen Assemblycache wird die Verwendung von „GACUtil.exe“ empfohlen. Die Verwendungsmöglichkeiten des Tools und die verfügbaren Visual Studio-Befehlszeilenoptionen sind in „GACUtil.exe“ dokumentiert.  
> 
> 

### <a name="restarting-the-biztalk-adapter-service-web-site"></a>Neustarten der BizTalk Adapter Service-Website
Bei der Installation der **BizTalk Adapter Service-Runtime*** wird in IIS die Website **BizTalk Adapter Service** mit der Anwendung **BAService** erstellt. Die Anwendung **BAService** verwendet intern eine Relaybindung, um die Reichweite des lokalen Dienstendpunkts auf die Cloud auszudehnen. Für einen lokal gehosteten Dienst wird der entsprechende Relayendpunkt nur beim Service Bus registriert, wenn der lokale Dienst gestartet wird.  

Beim Beenden und Starten einer Anwendung wird die Konfiguration für das automatische Starten einer Anwendung nicht berücksichtigt. Wenn **BAService** beendet wird, müssen Sie also immer die Website **BizTalk Adapter Service** (BizTalk-Adapterdienst) neu starten. Daher sollten Sie die Anwendung **BAService** nicht starten oder beenden.

### <a name="special-characters-should-not-be-used-for-address-and-entity-names-of-lob-components"></a>In Adress- und Entitätsnamen von LOB-Komponenten dürfen keine Sonderzeichen verwendet werden
Verwenden Sie in Adress- und Entitätsnamen von LOB-Komponenten keine Sonderzeichen. Andernfalls tritt beim Bereitstellen des BizTalk Service-Projekts ein Fehler auf. Bei Verwendung einiger Zeichen (beispielsweise „%“) wird die BizTalk Adapter Service-Website möglicherweise beendet und muss manuell gestartet werden.

### <a name="test-map-with-get-context-property"></a>Testen der Zuordnung mit „Kontexteigenschaft abrufen“
Wenn eine Transformation einen Zuordnungsvorgang vom Typ **Kontexteigenschaft abrufen** enthält, kann **Zuordnung testen** nicht ausgeführt werden. Ersetzen Sie als vorübergehende Problemumgehung den Zuordnungsvorgang **Kontexteigenschaft abrufen** durch einen Zuordnungsvorgang vom Typ „Zeichenfolge verketten“ mit Platzhalterdaten. Dadurch wird das Zielschema aufgefüllt, und Sie können andere Transformationsfunktionen testen.

### <a name="test-map-property-does-not-display"></a>Die Eigenschaft für „Zuordnung testen“ wird nicht angezeigt
Die Eigenschaften für **Zuordnung testen** werden in Visual Studio nicht angezeigt. Dieser Fall kann eintreten, wenn die Fenster **Eigenschaften** und **Projektmappen-Explorer** nicht gleichzeitig angedockt sind. Docken Sie zum Beheben dieses Problems die Fenster **Eigenschaften** und **Projektmappen-Explorer** an.  

### <a name="datetime-reformat-drop-down-is-grayed-out"></a>Das Dropdown-Steuerelement „DateTime neu formatieren“ ist abgeblendet
Wenn ein Zuordnungsvorgang vom Typ „DateTime neu formatieren“ der Entwurfsoberfläche hinzugefügt und konfiguriert wird, ist die Dropdownliste „Format“ möglicherweise abgeblendet. Dieser Fall kann eintreten, wenn die Computeranzeige auf **Mittel – 125%** oder **Größer – 150%** festgelegt ist. Legen Sie zur Lösung des Problems die Anzeige wie nachstehend beschrieben auf **Kleiner – 100 % (Standard)** fest:  

1. Öffnen Sie die **Systemsteuerung**, und klicken Sie auf **Darstellung und Anpassung**.
2. Klicken Sie auf **Anzeige**.
3. Klicken Sie auf **Kleiner – 100% (Standard)** und anschließend auf **Übernehmen**.

Die Dropdownliste **Format** sollte jetzt wie erwartet funktionieren.

### <a name="duplicate-agreements-in-the-biztalk-services-portal"></a>Doppelte Vereinbarungen im BizTalk Services-Portal
Stellen Sie sich folgendes Szenario vor:

1. Sie erstellen eine Vereinbarung mithilfe der TPM OM-API.
2. Sie öffnen die Vereinbarung im BizTalk Services-Portal auf zwei unterschiedlichen Registerkarten.
3. Sie stellen die Vereinbarung über beide Registerkarten bereit.
4. Dadurch werden beide Vereinbarungen bereitgestellt, was im BizTalk Services-Portal zu doppelten Einträgen führt.

**Problemumgehung**. Öffnen Sie im BizTalk Services-Portal eine der doppelt vorhandenen Vereinbarungen, und heben Sie die Bereitstellung auf.  

### <a name="bridges-do-not-use-updated-certificate-even-after-a-certificate-has-been-updated-in-the-artifact-store"></a>Bridges verwenden auch nach der Aktualisierung eines Zertifikats im Artefaktspeicher nicht das aktualisierte Zertifikat
Stellen Sie sich die folgenden Szenarien vor:  

**Szenario 1: Verwenden fingerabdruckbasierter Verhaltensmuster zum Identifizieren von Zertifikaten für die sichere Nachrichtenübertragung von einer Bridge an einen Dienstendpunkt**  
Stellen Sie sich ein Szenario vor, in dem Sie in Ihrem BizTalk Service-Projekt fingerabdruckbasierte Verhaltensmuster zum Identifizieren von Zertifikaten verwenden. Sie aktualisieren das Zertifikat im BizTalk Services-Portal mit dem gleichen Namen, aber mit einem anderen Fingerabdruck. Das BizTalk Service-Projekt aktualisieren Sie jedoch nicht entsprechend. In einem solchen Szenario werden die Nachrichten möglicherweise weiterhin von der Bridge verarbeitet, da sich die älteren Zertifikatdaten noch im Kanalcache befinden. Danach kann die Nachrichtenverarbeitung nicht mehr ausgeführt werden.  

**Problemumgehung**: Aktualisieren Sie das Zertifikat im BizTalk Service-Projekt, und stellen Sie das Projekt erneut bereit.  

**Szenario 2: Verwenden namensbasierter Verhaltensmuster zum Identifizieren von Zertifikaten für die sichere Nachrichtenübertragung von einer Bridge an einen Dienstendpunkt**

Stellen Sie sich ein Szenario vor, in dem Sie in Ihrem BizTalk Service-Projekt namensbasierte Verhaltensmuster zum Identifizieren von Zertifikaten verwenden. Sie aktualisieren das Zertifikat im BizTalk Services-Portal, aktualisieren aber das BizTalk Service-Projekt nicht entsprechend. In einem solchen Szenario werden die Nachrichten möglicherweise weiterhin von der Bridge verarbeitet, da sich die älteren Zertifikatdaten noch im Kanalcache befinden. Danach kann die Nachrichtenverarbeitung nicht mehr ausgeführt werden.  

**Problemumgehung**: Aktualisieren Sie das Zertifikat im BizTalk Service-Projekt, und stellen Sie das Projekt erneut bereit.  

### <a name="bridges-continue-to-process-messages-even-when-the-sql-database-is-offline"></a>Bridges verarbeiten Nachrichten weiterhin, auch wenn die SQL-Datenbank offline ist
Die BizTalk Services-Bridges setzen die Verarbeitung von Nachrichten eine Zeit lang fort, obwohl die Microsoft Azure SQL-Datenbank (in der die Ausführungsinformationen wie bereitgestellte Artefakte und Pipelines gespeichert werden) offline ist. Der Grund: BizTalk Services verwendet die zwischengespeicherten Artefakte und die zwischengespeicherte Bridgekonfiguration.
Falls die Bridges keine Nachrichten verarbeiten sollen, wenn die SQL-Datenbank offline ist, können Sie den BizTalk Service mithilfe der PowerShell-Cmdlets für BizTalk Services beenden oder anhalten. Informationen zu den Windows PowerShell-Cmdlets für die Vorgangsverwaltung finden Sie unter [Azure BizTalk Service-Verwaltung – Beispiel](http://go.microsoft.com/fwlink/p/?LinkID=329019) .  

### <a name="reading-the-xml-message-within-a-bridges-custom-code-component-includes-an-extra-bom-character"></a>Beim Lesen der XML-Nachricht in der benutzerdefinierten Codekomponente einer Bridge ist ein zusätzliches BOM-Zeichen enthalten
Szenario: Sie möchten eine XML-Nachricht im benutzerdefinierten Code einer Bridge lesen. Bei Verwendung der .NET-API „System.Text.Encoding.UTF8.GetString(bytes)“ ist in der Ausgabe am Anfang der Nachricht ein zusätzliches BOM-Zeichen enthalten. Wenn die Ausgabe kein zusätzliches BOM-Zeichen enthalten soll, müssen Sie ```System.IO.StreamReader().ReadToEnd()```verwenden.

### <a name="sending-messages-to-a-bridge-using-wcf-does-not-scale"></a>Beim Senden von Nachrichten an eine Bridge mit WCF erfolgt keine Skalierung
Wenn Nachrichten mithilfe von WCF an eine Bridge gesendet werden, erfolgt keine Skalierung. Verwenden Sie „HttpWebRequest“, wenn Sie einen skalierbaren Client benötigen.

### <a name="upgrade-token-provider-error-after-upgrading-from-biztalk-services-preview-to-general-availability-ga"></a>UPGRADE: Tokenanbieterfehler nach dem Upgrade von BizTalk Services Preview auf die allgemein verfügbare Version
Es gibt eine EDI- oder AS2-Vereinbarung mit aktiven Batches. Wenn der BizTalk Service von der Vorschauversion auf die allgemein verfügbare Version aktualisiert wird, kann Folgendes geschehen:

* Fehler: Der Tokenanbieter konnte kein Sicherheitstoken bereitstellen. Vom Tokenanbieter zurückgegebene Meldung: Der Remotename konnte nicht aufgelöst werden.
* Stapelverarbeitungsaufgaben werden abgebrochen.

**Problemumgehung**: Stellen Sie die Vereinbarung nach dem BizTalk Service-Upgrade auf die allgemein verfügbare Version erneut bereit.  

### <a name="upgrade-toolbox-shows-the-old-bridge-icons-after-upgrading-the-biztalk-services-sdk"></a>UPGRADE: Toolbox zeigt nach dem Upgraden des BizTalk Services-SDKs die alten Bridgesymbole an
Nach dem Upgraden einer älteren Version des BizTalk Services-SDKs (mit alten Symbolen zur Darstellung der Bridges) werden in der Toolbox weiterhin die alten Symbole für die Bridges angezeigt. Wenn Sie jedoch der BizTalk Service-Projekt-Entwurfsoberfläche eine Bridge hinzufügen, wird auf der Oberfläche das neue Symbol angezeigt.  

**Problemumgehung**. Sie können dieses Problem umgehen, indem Sie die TBD-Dateien aus dem Verzeichnis „<system drive>:\Benutzer\<Benutzer>\AppData\Local\Microsoft\VisualStudio\11.0“ löschen.  

### <a name="upgrade-biztalk-portal-update-from-preview-to-ga-might-show-an-error-indicating-that-the-edi-capability-is-not-available"></a>UPGRADE: Nach der Aktualisierung des BizTalk-Portals von der Vorschauversion auf die allgemein verfügbare Version wird möglicherweise ein Fehler mit dem Hinweis angezeigt, dass die EDI-Funktion nicht verfügbar ist
Wenn Sie beim BizTalk Services-Portal angemeldet sind, während das BizTalk Services-Upgrade von der Vorschauversion auf die allgemein verfügbare Version durchgeführt wird, wird im Portal unter Umständen eine Fehlermeldung wie die folgende angezeigt:  

Diese Funktion ist in dieser Edition von Microsoft Azure BizTalk Services nicht verfügbar. Wechseln Sie zur Verwendung dieser Funktionen zu einer geeigneten Edition.  

**Lösung**: Melden Sie sich vom Portal ab, schließen Sie den Browser, öffnen Sie ihn wieder, und melden Sie sich dann erneut beim Portal an.  

### <a name="upgrade-new-tracking-data-does-not-show-up-after-biztalk-services-is-upgraded-to-ga"></a>UPGRADE: Nach dem Upgraden von BizTalk Services auf die allgemein verfügbare Version werden keine neuen Nachverfolgungsdaten angezeigt
Szenario: Sie haben im Abonnement der Vorschauversion von BizTalk Services eine XML-Bridge bereitgestellt. Sie senden Nachrichten an die Bridge, und die entsprechenden Nachverfolgungsdaten sind im BizTalk Services-Portal verfügbar. Das BizTalk Services-Portal und die BizTalk Services-Laufzeitkomponenten werden auf die allgemein verfügbare Version aktualisiert, und Sie senden eine Nachricht an den gleichen (zuvor bereitgestellten) Bridgeendpunkt.  

### <a name="pipelines-versus-bridges"></a>Pipelines im Vergleich zu Bridges
In diesem Dokument werden die Begriffe „Pipelines“ und „Bridges“ synonym verwendet. Beiden Begriffe bezeichnen im Wesentlichen eine in BizTalk Services bereitgestellte Nachrichtenverarbeitungseinheit.  

### <a name="concepts"></a>Konzepte
[BizTalk Services](https://msdn.microsoft.com/library/azure/hh689864.aspx)   



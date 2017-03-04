---
title: "Überwachung und Protokollierung – Microsoft Threat Modeling Tool – Azure | Microsoft-Dokumentation"
description: "Gegenmaßnahmen für durch das Threat Modeling Tool offengelegte Gefahren"
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: rodsan
translationtype: Human Translation
ms.sourcegitcommit: 8251f44200c11d3efcec04b7ac99857232b2f9ed
ms.openlocfilehash: e6a369ce9970b27709f5a1a1fa6c52b100b3b925
ms.lasthandoff: 02/15/2017


---

# <a name="security-frame-auditing-and-logging--mitigations"></a>Sicherheitsrahmen: Überwachung und Protokollierung | Gegenmaßnahmen 
| Produkt/Dienst | Artikel |
| --------------- | ------- |
| Dynamics CRM    | <ul><li>[Identifizieren von vertraulichen Entitäten in der Lösung und Implementieren der Änderungsüberwachung](#sensitive-entities)</li></ul> |
| Webanwendung. | <ul><li>[Sicherstellen, dass Überwachung und Protokollierung für die Anwendung erzwungen werden](#auditing)</li><li>[Sicherstellen, dass die Protokollrotation und -trennung vorhanden sind](#log-rotation)</li><li>[Sicherstellen, dass die Anwendung keine sensiblen Benutzerdaten protokolliert](#log-sensitive-data)</li><li>[Sicherstellen, dass für die Überwachungs- und Protokolldateien der Zugriff eingeschränkt ist](#log-restricted-access)</li><li>[Sicherstellen, dass Ereignisse der Benutzerverwaltung protokolliert werden](#user-management)</li><li>[Sicherstellen, dass das System über integrierte Schutzmaßnahmen gegen missbräuchliche Nutzung verfügt](#inbuilt-defenses)</li><li>[Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service](#diagnostics-logging)</li></ul> |
| Datenbank | <ul><li>[Sicherstellen, dass die Anmeldungsüberwachung in SQL Server aktiviert ist](#identify-sensitive-entities)</li><li>[Aktivieren der Bedrohungserkennung in Azure SQL](#threat-detection)</li></ul> |
| Azure Storage | <ul><li>[Verwenden der Azure-Speicheranalyse zum Überwachen des Zugriffs auf Azure Storage](#analytics)</li></ul> |
| WCF | <ul><li>[Implementieren einer ausreichenden Protokollierung](#sufficient-logging)</li><li>[Implementieren einer ausreichenden Behandlung von Überwachungsfehlern](#audit-failure-handling)</li></ul> |
| Web-API | <ul><li>[Sicherstellen, dass Überwachung und Protokollierung für die Web-API erzwungen werden](#logging-web-api)</li></ul> |
| IoT-Bereichsgateway | <ul><li>[Sicherstellen, dass geeignete Überwachung und Protokollierung auf dem Bereichsgateway erzwungen werden](#logging-field-gateway)</li></ul> |
| IoT-Cloudgateway | <ul><li>[Sicherstellen, dass geeignete Überwachung und Protokollierung auf dem Cloudgateway erzwungen werden](#logging-cloud-gateway)</li></ul> |

## <a name="a-idsensitive-entitiesaidentify-sensitive-entities-in-your-solution-and-implement-change-auditing"></a><a id="sensitive-entities"></a>Identifizieren von vertraulichen Entitäten in der Lösung und Implementieren der Änderungsüberwachung

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Dynamics CRM | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | N/V  |
| Referenzen              | N/V  |
| Schritte                   | Identifizieren Sie in der Lösung Entitäten mit sensiblen Daten, und implementieren Sie eine Änderungsüberwachung für diese Entitäten und Felder. |

## <a name="a-idauditingaensure-that-auditing-and-logging-is-enforced-on-the-application"></a><a id="auditing"></a>Sicherstellen, dass Überwachung und Protokollierung für die Anwendung erzwungen werden

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Webanwendung. | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | N/V  |
| Referenzen              | N/V  |
| Schritte                   | Aktivieren Sie die Überwachung und Protokollierung für alle Komponenten. Überwachungsprotokolle sollten den Benutzerkontext erfassen. Identifizieren Sie alle wichtigen Ereignissen, und protokollieren Sie diese Ereignisse. Implementieren Sie eine zentralisierte Protokollierung. |

## <a name="a-idlog-rotationaensure-that-log-rotation-and-separation-are-in-place"></a><a id="log-rotation"></a>Sicherstellen, dass die Protokollrotation und -trennung vorhanden sind

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Webanwendung. | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | –  |
| Referenzen              | N/V  |
| Schritte                   | <p>Protokollrotation ist ein automatisierter Prozess, der bei der Systemadministration verwendet wird und bei dem mit einem Datum versehene Protokolldateien archiviert werden. Server, die große Anwendungen ausführen, protokollieren häufig jede Anforderung: Bei umfangreichen Protokollen ist die Protokollrotation eine Methode, die Gesamtgröße der Protokolle zu beschränken, während nach wie vor die Analyse von aktuellen Ereignissen möglich bleibt. </p><p>Protokolltrennung bedeutet im Grunde, dass Sie die Protokolldateien auf einer anderen Partition speichern müssen als die, auf der das Betriebssystem/die Anwendung ausgeführt wird, um einen DoS-Angriff oder die Verschlechterung der Leistung Ihrer Anwendung abzuwenden.</p>|

## <a name="a-idlog-sensitive-dataaensure-that-the-application-does-not-log-sensitive-user-data"></a><a id="log-sensitive-data"></a>Sicherstellen, dass die Anwendung keine sensiblen Benutzerdaten protokolliert

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Webanwendung. | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | N/V  |
| Referenzen              | N/V  |
| Schritte                   | <p>Stellen Sie sicher, dass Sie keine sensiblen Daten protokollieren, die ein Benutzer an Ihre Website übermittelt. Achten Sie auf eine beabsichtigte Protokollierung und auf Nebeneffekte, die auf Entwurfsprobleme zurückzuführen sind. Beispiele für sensible Daten:</p><ul><li>Benutzeranmeldeinformationen</li><li>Sozialversicherungsnummer oder andere identifizierende Informationen</li><li>Kreditkartennummern oder andere finanzielle Informationen</li><li>Informationen zur Gesundheit</li><li>Private Schlüssel oder anderen Daten, die verwendet werden könnten, um verschlüsselte Informationen zu entschlüsseln</li><li>System- oder Anwendungsinformationen, die für effektivere Angriffe auf die Anwendung verwendet werden können</li></ul>|

## <a name="a-idlog-restricted-accessaensure-that-audit-and-log-files-have-restricted-access"></a><a id="log-restricted-access"></a>Sicherstellen, dass für die Überwachungs- und Protokolldateien der Zugriff eingeschränkt ist

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Webanwendung. | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | –  |
| Referenzen              | N/V  |
| Schritte                   | <p>Stellen Sie sicher, dass die Zugriffsrechte für die Protokolldateien in geeigneter Weise festgelegt werden. Anwendungskonten sollten lesegeschützten Zugriff und Operatoren und Supportmitarbeiter sollten bei Bedarf schreibgeschützten Zugriff erhalten.</p><p>Administratorkonten sind die einzige Konten, die vollständigen Zugriff haben sollten. Überprüfen Sie die Windows-ACL auf Protokolldateien, um sicherzustellen, dass sie ordnungsgemäß beschränkt sind:</p><ul><li>Anwendungskonten sollten lesegeschützten Zugriff haben.</li><li>Operatoren und Supportmitarbeiter sollten bei Bedarf schreibgeschützten Zugriff haben.</li><li>Nur Administratorkonten sollten vollständigen Zugriff haben.</li></ul>|

## <a name="a-iduser-managementaensure-that-user-management-events-are-logged"></a><a id="user-management"></a>Sicherstellen, dass Ereignisse der Benutzerverwaltung protokolliert werden

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Webanwendung. | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | N/V  |
| Referenzen              | –  |
| Schritte                   | <p>Stellen Sie sicher, dass die Anwendung Ereignisse der Benutzerverwaltung überwacht, z.B. erfolgreiche und fehlgeschlagene Benutzeranmeldungen, Kennwortzurücksetzungen, Kennwortänderungen, Kontosperrungen, Benutzerregistrierungen. Auf diese Weise kann potenziell verdächtiges Verhalten erkannt und darauf reagiert werden. So wird auch das Sammeln von Betriebsdaten ermöglicht, um z.B. nachzuverfolgen, wer auf die Anwendung zugreift.</p>|

## <a name="a-idinbuilt-defensesaensure-that-the-system-has-inbuilt-defenses-against-misuse"></a><a id="inbuilt-defenses"></a>Sicherstellen, dass das System über integrierte Schutzmaßnahmen gegen missbräuchliche Nutzung verfügt

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Webanwendung. | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | –  |
| Referenzen              | N/V  |
| Schritte                   | <p>Es sollten Kontrollen vorhanden sein, die bei einer missbräuchlichen Anwendungsnutzung eine Sicherheitsausnahme auslösen. Beispiel: Wenn eine Überprüfung von Eingaben vorhanden ist und ein Angreifer versucht, schädlichen Code einzufügen, der nicht mit dem regulären Ausdruck übereinstimmt, kann eine Sicherheitsausnahme ausgelöst werden, die ein Hinweis auf die missbräuchliche Nutzung des Systems sein könnte.</p><p>Beispielsweise wird empfohlen, für die folgenden Probleme Sicherheitsausnahmen zu protokollieren und Maßnahmen zu ergreifen:</p><ul><li>Eingabeüberprüfung</li><li>CSRF-Verstöße</li><li>Brute-Force (obere Grenze für die Anzahl von Anforderungen pro Benutzer und Ressource)</li><li>Verstöße bei Dateiuploads</li><ul>|

## <a name="a-iddiagnostics-loggingaenable-diagnostics-logging-for-web-apps-in-azure-app-service"></a><a id="diagnostics-logging"></a>Aktivieren der Diagnoseprotokollierung für Web-Apps in Azure App Service

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Webanwendung. | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | EnvironmentType: Azure |
| Referenzen              | N/V  |
| Schritte | <p>Azure bietet integrierte Diagnosefunktionen zur Unterstützung beim Debuggen einer App Service-Web-App. Dies gilt auch für API-Apps und mobile Apps. App Service-Web-Apps bieten Diagnosefunktionen zum Protokollieren von Informationen sowohl über den Webserver als auch über die Webanwendung.</p><p>Diese sind logisch in Webserverdiagnose und Anwendungsdiagnose unterteilt.</p>|

## <a name="a-ididentify-sensitive-entitiesaensure-that-login-auditing-is-enabled-on-sql-server"></a><a id="identify-sensitive-entities"></a>Sicherstellen, dass die Anmeldungsüberwachung in SQL Server aktiviert ist

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Datenbank | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | –  |
| Referenzen              | [Konfigurieren der Anmeldungsüberwachung](https://msdn.microsoft.com/library/ms175850.aspx) |
| Schritte | <p>Die Anmeldungsüberwachung für Datenbankserver muss aktiviert sein, um Angriffe zu erkennen/bestätigen, bei denen das Kennwort erraten wird. Es ist wichtig, fehlgeschlagene Anmeldeversuche zu erfassen. Das Erfassen erfolgreicher und fehlgeschlagener Anmeldeversuche bietet zusätzliche Vorteile während der forensischen Untersuchung.</p>|

## <a name="a-idthreat-detectionaenable-threat-detection-on-azure-sql"></a><a id="threat-detection"></a>Aktivieren der Bedrohungserkennung in Azure SQL

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Datenbank | 
| SDL-Phase               | Entwickeln |  
| Technologien | SQL Azure |
| Attribute              | SQL-Version: V12 |
| Referenzen              | [Erste Schritte mit der Bedrohungserkennung von SQL-Datenbank](https://azure.microsoft.com/documentation/articles/sql-database-threat-detection-get-started/)|
| Schritte |<p>Die Bedrohungserkennung erkennt anormale Datenbankaktivitäten, die auf potenzielle Sicherheitsrisiken für die Datenbank hindeuten. Sie bietet eine neue Sicherheitsebene und ermöglicht es den Kunden, auf erkannte potenzielle Bedrohungen zu reagieren. Zu diesem Zweck werden Sicherheitswarnungen zu anormalen Aktivitäten bereitgestellt.</p><p>Die Benutzer können die verdächtigen Ereignisse mithilfe der Azure SQL-Datenbanküberwachung untersuchen, um zu ermitteln, ob sie auf einen Zugriffsversuch zurückzuführen sind oder die Verletzung der Datensicherheit oder den Missbrauch von Daten zum Ziel haben.</p><p>Die Bedrohungserkennung vereinfacht den Umgang mit potenziellen Bedrohungen für die Datenbank, ohne das Fachwissen eines Sicherheitsexperten besitzen oder komplexe Sicherheitsüberwachungssysteme verwalten zu müssen.</p>|

## <a name="a-idanalyticsause-azure-storage-analytics-to-audit-access-of-azure-storage"></a><a id="analytics"></a>Verwenden der Azure-Speicheranalyse zum Überwachen des Zugriffs auf Azure Storage

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Azure Storage | 
| SDL-Phase               | Bereitstellung |  
| Technologien | Allgemein |
| Attribute              | N/V |
| Referenzen              | [Verwenden der Speicheranalyse zur Überwachung des Autorisierungstyps](https://azure.microsoft.com/documentation/articles/storage-security-guide/#storage-analytics) |
| Schritte | <p>Für jedes Speicherkonto kann die Azure-Speicheranalyse zur Protokollierung und Speicherung von Metrikdaten aktiviert werden. Bei der Speicheranalyse werden wichtige Informationen bereitgestellt, z.B. die Authentifizierungsmethode, die von einem Benutzer verwendet wird, wenn er auf den Speicher zugreift.</p><p>Dies kann sehr hilfreich sein, wenn Sie den Zugriff auf den Speicher sehr genau beobachten. In Blob Storage können Sie z. B. alle Container als privat festlegen und die Verwendung eines SAS-Diensts in allen Anwendungen implementieren. Anschließend können Sie die Protokolle regelmäßig überprüfen, um festzustellen, ob der Zugriff auf Ihre Blobs über die Schlüssel des Speicherkontos erfolgt, was auf eine Sicherheitslücke hinweisen könnte, oder ob die Blobs öffentlich sind, obwohl sie es nicht sein sollten.</p>|

## <a name="a-idsufficient-loggingaimplement-sufficient-logging"></a><a id="sufficient-logging"></a>Implementieren einer ausreichenden Protokollierung

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | WCF | 
| SDL-Phase               | Entwickeln |  
| Technologien | .NET Framework |
| Attribute              | N/V  |
| Referenzen              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Kingdom](https://vulncat.fortify.com/en/vulncat/index.html) |
| Schritte | <p>Das Fehlen eines richtigen Audit-Trails nach einem Sicherheitsincident kann die forensische Untersuchung behindern. Windows Communication Foundation (WCF) bietet die Möglichkeit, erfolgreiche und/oder fehlgeschlagene Authentifizierungsversuche zu protokollieren.</p><p>Die Protokollierung fehlgeschlagener Authentifizierungsversuche kann Administratoren vor Brute-Force-Angriffen warnen. Die Protokollierung von Ereignissen für eine erfolgreiche Authentifizierung können einen nützlichen Audit-Trail bereitstellen, wenn ein berechtigtes Konto gefährdet wird. Aktivieren Sie die Funktion für die Sicherheitsüberwachung von Diensten in WCF. |

### <a name="example"></a>Beispiel
Im Folgenden wird eine Beispielkonfiguration mit aktivierter Überwachung veranschaulicht.
```
<system.serviceModel>
    <behaviors>
        <serviceBehaviors>
            <behavior name=""NewBehavior"">
                <serviceSecurityAudit auditLogLocation=""Default""
                suppressAuditFailure=""false"" 
                serviceAuthorizationAuditLevel=""SuccessAndFailure""
                messageAuthenticationAuditLevel=""SuccessAndFailure"" />
                ...
            </behavior>
        </servicebehaviors>
    </behaviors>
</system.serviceModel>
```

## <a name="a-idaudit-failure-handlingaimplement-sufficient-audit-failure-handling"></a><a id="audit-failure-handling"></a>Implementieren einer ausreichenden Behandlung von Überwachungsfehlern

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | WCF | 
| SDL-Phase               | Entwickeln |  
| Technologien | .NET Framework |
| Attribute              | N/V  |
| Referenzen              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Kingdom](https://vulncat.fortify.com/en/vulncat/index.html) |
| Schritte | <p>Die entwickelte Lösung wird so konfiguriert, dass keine Ausnahme generiert wird, wenn nicht in ein Überwachungsprotokoll geschrieben werden kann. Wenn WCF so konfiguriert ist, dass keine Ausnahme ausgelöst wird, wenn nicht in ein Überwachungsprotokoll geschrieben werden kann, wird das Programm nicht über den Fehler benachrichtigt, und es findet möglicherweise keine Überwachung auf kritische Sicherheitsereignisse statt.</p>|

### <a name="example"></a>Beispiel
Das `<behavior/>`-Element der folgenden WCF-Konfigurationsdatei weist WCF an, die Anwendung nicht zu benachrichtigen, wenn WCF nicht in ein Überwachungsprotokoll schreiben kann.
````
<behaviors>
    <serviceBehaviors>
        <behavior name="NewBehavior">
            <serviceSecurityAudit auditLogLocation="Application"
            suppressAuditFailure="true"
            serviceAuthorizationAuditLevel="Success"
            messageAuthenticationAuditLevel="Success" />
        </behavior>
    </serviceBehaviors>
</behaviors>
````
Konfigurieren Sie WCF so, dass das Programm benachrichtigt wird, wenn nicht in ein Überwachungsprotokoll geschrieben werden kann. Im Programm sollte ein alternatives Benachrichtigungsschema vorhanden sein, um die Organisation zu informieren, dass Audit-Trails nicht verwaltet werden. 

## <a name="a-idlogging-web-apiaensure-that-auditing-and-logging-is-enforced-on-web-api"></a><a id="logging-web-api"></a>Sicherstellen, dass Überwachung und Protokollierung für die Web-API erzwungen werden

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | Web-API | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | –  |
| Referenzen              | –  |
| Schritte | Aktivieren Sie die Überwachung und Protokollierung für Web-APIs. Überwachungsprotokolle sollten den Benutzerkontext erfassen. Identifizieren Sie alle wichtigen Ereignissen, und protokollieren Sie diese Ereignisse. Implementieren Sie eine zentralisierte Protokollierung. |

## <a name="a-idlogging-field-gatewayaensure-that-appropriate-auditing-and-logging-is-enforced-on-field-gateway"></a><a id="logging-field-gateway"></a>Sicherstellen, dass geeignete Überwachung und Protokollierung auf dem Bereichsgateway erzwungen werden

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | IoT-Bereichsgateway | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | N/V  |
| Referenzen              | N/V  |
| Schritte | <p>Wenn mehrere Geräte zu einem Bereichsgateway verbunden werden, stellen Sie sicher, dass Verbindungsversuche und der Authentifizierungsstatus (Erfolg oder Fehler) für die einzelnen Geräte protokolliert und auf dem Bereichsgateway verwaltet werden.</p><p>In Fällen, in denen das Bereichsgateway die IoT Hub-Anmeldeinformationen für die einzelnen Geräte verwaltet, stellen Sie zudem sicher, dass die Überwachung ausgeführt wird, wenn diese Anmeldeinformationen abgerufen werden. Entwickeln Sie einen Prozess, um die Protokolle in regelmäßigen Abständen in Azure IoT Hub/Speicher für die langfristige Aufbewahrung hochzuladen.</p> |

## <a name="a-idlogging-cloud-gatewayaensure-that-appropriate-auditing-and-logging-is-enforced-on-cloud-gateway"></a><a id="logging-cloud-gateway"></a>Sicherstellen, dass geeignete Überwachung und Protokollierung auf dem Cloudgateway erzwungen werden

| Titel                   | Details      |
| ----------------------- | ------------ |
| Komponente               | IoT-Cloudgateway | 
| SDL-Phase               | Entwickeln |  
| Technologien | Allgemein |
| Attribute              | N/V  |
| Referenzen              | [Einführung in die IoT Hub-Vorgangsüberwachung](https://azure.microsoft.com/documentation/articles/iot-hub-operations-monitoring/) |
| Schritte | <p>Berücksichtigen Sie im Entwurf das Sammeln und Speichern von Überwachungsdaten, die durch die IoT Hub-Vorgangsüberwachung erfasst werden. Aktivieren Sie die folgenden Kategorien für die Überwachung:</p><ul><li>Geräte-Identitätsvorgänge</li><li>Gerät-zu-Cloud-Kommunikation</li><li>Cloud-zu-Gerät-Kommunikation</li><li>Verbindungen</li><li>Dateiuploads</li></ul>|

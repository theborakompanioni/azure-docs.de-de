---
title: "Azure Machine Learning-REST-API – Fehlercodes | Microsoft-Dokumentation"
description: "Diese Fehlercodes können für den Vorgang eines Azure Machine Learning-Webdiensts zurückgegeben werden."
keywords: 
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 0923074b-3728-439d-a1b8-8a7245e39be4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/16/2016
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: e8a6db7963203d747b1f506d0cfae8b3e98d58d3
ms.openlocfilehash: 4b5c9e4c62fbcf548aad1dbe242e2df0f2f5d41f


---
 
# <a name="machine-learning-rest-api-error-codes"></a>Machine Learning-REST-API – Fehlercodes
 
Die folgenden Fehlercodes können für den Vorgang eines Azure Machine Learning-Webdiensts zurückgegeben werden.
 
## <a name="badargument-http-status-code-400"></a>BadArgument (HTTP-Statuscode 400)
 
Ungültiges Argument angegeben.
 
Diese Klasse von Fehlern bedeutet, dass ein angegebenes Argument ungültig war. Dies können Anmeldeinformationen oder der Speicherort von Azure-Speicher für Daten sein, die an den Webdienst übergeben werden. Das Feld mit dem „Code“ des Fehlers im Abschnitt „Details“ enthält Informationen dazu, welches Argument ungültig war.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| BadParameterValue | Der angegebene Parameterwert erfüllt die Vorgaben der Parameterregel im Parameter nicht. |
| BadSubscriptionId | Die Abonnement-ID, die zum Bewerten verwendet wird, ist nicht die ID, die in der Ressource vorhanden ist. |
| BadVersionCall | Während des API-Aufrufs wurde ein ungültiger Versionsparameter übergeben: {0}. Ermitteln Sie auf der API-Hilfeseite die richtige Version, und wiederholen Sie den Vorgang. |
| BatchJobInputsNotSpecified | Die folgenden erforderlichen Eingaben wurden nicht mit der Anforderung angegeben: {0}. Stellen Sie sicher, dass alle Eingabedaten angegeben sind, und wiederholen Sie den Vorgang. |
| BatchJobInputsTooManySpecified | In der Anforderung wurden mehr Eingaben angegeben, als im Dienst definiert sind. Liste mit akzeptierten Eingaben: {0}. Stellen Sie sicher, dass alle Eingabedaten richtig angegeben wurden, und wiederholen Sie den Vorgang. |
| BlobNameTooLong | Der Azure-Blobspeicherpfad für die Diagnoseausgabe ist zu lang: {0}. Kürzen Sie den Pfad, und wiederholen Sie den Vorgang. |
| BlobNotFound | Auf das angegebene Azure-Blob kann nicht zugegriffen werden – {0}.  Azure-Fehlermeldung: {1}. |
| ContainerIsEmpty | Es wurde kein Azure-Speichercontainername angegeben. Geben Sie einen gültigen Containernamen an, und wiederholen Sie den Vorgang. |
| ContainerSegmentInvalid | Der Containername ist ungültig. Geben Sie einen gültigen Containernamen an, und wiederholen Sie den Vorgang. |
| ContainerValidationFailed | Bei der Überprüfung des Blobcontainers ist dieser Fehler aufgetreten: {0}. |
| DataTypeNotSupported | Es wurde ein nicht unterstützter Datentyp angegeben. Stellen Sie gültige Datentypen bereit, und wiederholen Sie den Vorgang. |
| DuplicateInputInBatchCall | Die Batchanforderung ist ungültig. Einfach- und Mehrfacheingaben können nicht gleichzeitig angegeben werden. Entfernen Sie eines dieser Elemente aus der Anforderung, und wiederholen Sie den Vorgang. |
| ExpiryTimeInThePast | Die angegebene Ablaufzeit liegt in der Vergangenheit: {0}. Geben Sie eine Ablaufzeit (in UTC) an, die in der Zukunft liegt, und wiederholen Sie den Vorgang. Legen Sie die Ablaufzeit auf NULL fest, wenn der Zeitraum niemals ablaufen soll. |
| IncompleteSettings | Die Diagnoseeinstellungen sind unvollständig. |
| InputBlobRelativeLocationInvalid | Es wurde kein Azure-Speicherblobname angegeben. Geben Sie einen gültigen Blobnamen an, und wiederholen Sie den Vorgang. |
| InvalidBlob | Ungültige Blobspezifikation für Blob: {0}. Vergewissern Sie sich, dass die Verbindungszeichenfolge bzw. der relative Pfad oder die SAS-Tokenspezifikation richtig ist, und wiederholen Sie den Vorgang. |
| InvalidBlobConnectionString | Die Verbindungszeichenfolge, die für ein Eingabe-/Ausgabeblob angegeben wurde, ist ungültig: {0}. Beheben Sie dies, und wiederholen Sie den Vorgang. |
| InvalidBlobExtension | Der Blobverweis {0} verfügt über eine ungültige oder fehlende Dateierweiterung. Unterstützte Dateierweiterungen für diesen Ausgabetyp sind: „{1}“. |
| InvalidInputNames | In der Anforderung wurden ungültige Diensteingabenamen angegeben: {0}. Ordnen Sie die Eingabedaten den richtigen Diensteingaben zu, und wiederholen Sie den Vorgang. |
| InvalidOutputOverrideName | Ungültiger Ausgabeüberschreibungsname: {0}. Der Dienst weist keinen Ausgabeknoten mit diesem Namen auf. Übergeben Sie einen korrekten Ausgabeknotennamen für die Überschreibung (Groß-/Kleinschreibung wird berücksichtigt). |
| InvalidQueryParameter | Ungültiger Abfrageparameter „{0}“. {1} |
| MissingInputBlobInformation | Fehlende Informationen für das Azure-Speicherblob. Geben Sie eine gültige Verbindungszeichenfolge und einen relativen Pfad oder einen URI ein, und wiederholen Sie den Vorgang. |
| MissingJobId | Es wurde keine Auftrags-ID angegeben. Eine Auftrags-ID wird zurückgegeben, wenn ein Auftrag zum ersten Mal übermittelt wurde. Stellen Sie sicher, dass die Auftrags-ID korrekt ist, und wiederholen Sie den Vorgang. |
| MissingKeys | Es wurden keine Schlüssel oder kein Primär- oder Sekundärschlüssel angegeben. |
| MissingModelPackage | Es wurde keine Modellpaket-ID oder kein Modellpaket angegeben. Geben Sie eine gültige Modellpaket-ID oder ein Modellpaket an, und wiederholen Sie den Vorgang. |
| MissingOutputOverrideSpecification | Für die Anforderung fehlt die Blobspezifikation für die Ausgabeüberschreibung {0}. Geben Sie einen gültigen Blobspeicherort für die Anforderung an, oder entfernen Sie die Ausgabespezifikation, wenn keine Speicherortüberschreibung gewünscht wird. |
| MissingRequestInput | Der Webdienst erwartet eine Eingabe, aber es ist keine Eingabe erfolgt. Stellen Sie sicher, dass basierend auf den veröffentlichten Eingabeports im Modell gültige Eingaben bereitgestellt werden, und wiederholen Sie den Vorgang. |
| MissingRequiredGlobalParameters | Es wurden nicht alle erforderlichen Webdienstparameter angegeben. Vergewissern Sie sich, dass die für die Module erwarteten Parameter korrekt sind, und wiederholen Sie den Vorgang. |
| MissingRequiredOutputOverrides | Beim Aufrufen eines verschlüsselten Dienstendpunkts ist es obligatorisch, Ausgabeüberschreibungen für alle Ausgaben des Diensts zu übergeben. Derzeit fehlende Überschreibungen für diese Ausgaben: {0} |
| MissingWebServiceGroupId | Keine Webdienstgruppen-ID angegeben. Geben Sie eine gültige Webdienstgruppen-ID ein, und wiederholen Sie den Vorgang. |
| MissingWebServiceId | Keine Webdienst-ID angegeben. Geben Sie eine gültige Webdienst-ID an, und wiederholen Sie den Vorgang. |
| MissingWebServicePackage | Kein Webdienstpaket angegeben. Geben Sie ein gültiges Webdienstpaket an, und wiederholen Sie den Vorgang. |
| MissingWorkspaceId | Keine Arbeitsbereich-ID angegeben. Geben Sie eine gültige Arbeitsbereich-ID an, und wiederholen Sie den Vorgang. |
| ModelConfigurationInvalid | Ungültige Modellkonfiguration im Modellpaket. Stellen Sie sicher, dass die Modellkonfiguration eine Ausgabeendpunkt-Definition, einen stderror-Endpunkt und einen stdout-Endpunkt enthält, und wiederholen Sie den Vorgang. |
| ModelPackageIdInvalid | Ungültige Modellpaket-ID. Stellen Sie sicher, dass die Modellpaket-ID korrekt ist, und wiederholen Sie den Vorgang. |
| RequestBodyInvalid | Kein Anforderungstext angegeben, oder Fehler beim Deserialisieren des Anforderungstexts. |
| RequestIsEmpty | Keine Anforderung angegeben. Geben Sie eine gültige Anforderung an, und wiederholen Sie den Vorgang. |
| UnexpectedParameter | Unerwartete Parameter angegeben. Vergewissern Sie sich, dass alle Parameternamen richtig geschrieben sind und nur erwartete Parameter übergeben werden, und wiederholen Sie den Vorgang. |
| UnknownError | Unbekannter Fehler. |
| UserParameterInvalid | {0} |
| WebServiceConcurrentRequestRequirementInvalid | Die Vorgaben für gleichzeitige Anforderungen für den Webdienst {0} können nicht geändert werden. |
| WebServiceIdInvalid | Ungültige Webdienst-ID angegeben. Die Webdienst-ID muss eine gültige GUID sein. |
| WebServiceTooManyConcurrentRequestRequirement | Für gleichzeitige Anforderungen kann die Vorgabe nicht auf einen höheren Wert als {0} festgelegt werden. |
| WebServiceTypeInvalid | Ungültigen Webdiensttyp bereitgestellt. Vergewissern Sie sich, dass der Webdiensttyp korrekt ist, und wiederholen Sie den Vorgang. Gültige Webdiensttypen: {0}. |
 
## <a name="baduserargument-http-status-code-400"></a>BadUserArgument (HTTP-Statuscode 400)
 
Ungültiges Benutzerargument angegeben.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| InputMismatchError | Die Eingabedaten stimmen nicht mit dem Eingabeportschema überein. |
| InputParseError | Fehler beim Analysieren des Eingabevektors.  Stellen Sie sicher, dass der Eingabevektor über die richtige Anzahl von Spalten und Datentypen verfügt.  Weitere Details: {0}. |
| MissingRequiredGlobalParameters | Die vom Webdienst erwarteten Parameter fehlen. Stellen Sie sicher, dass alle erforderlichen Parameter, die vom Webdienst erwartet werden, korrekt sind, und wiederholen Sie den Vorgang. |
| UnexpectedParameter | Stellen Sie sicher, dass nur die erforderlichen Parameter, die vom Webdienst erwartet werden, übergeben werden, und wiederholen Sie den Vorgang. |
| UserParameterInvalid | {0} |
 
## <a name="invalidoperation-http-status-code-400"></a>InvalidOperation (HTTP-Statuscode 400)
 
Die Anforderung ist im aktuellen Kontext ungültig.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| CannotStartJob | Der Auftrag kann nicht gestartet werden, da er sich im Status {0} befindet. |
| IncompatibleModel | Das Modell ist mit der Anforderungsversion inkompatibel. Die Anforderungsversion unterstützt nur einzelne DataTable-Ausgabemodelle. |
| MultipleInputsNotAllowed | Für das Modell sind mehrere Eingaben unzulässig. |
 
## <a name="libraryexecutionerror-http-status-code-400"></a>LibraryExecutionError (HTTP-Statuscode 400)
 
Bei der Modulausführung ist ein interner Bibliotheksfehler aufgetreten.
 
 
## <a name="moduleexecutionerror-http-status-code-400"></a>ModuleExecutionError (HTTP-Statuscode 400)
 
Bei der Modulausführung ist ein Fehler aufgetreten.
 
 
## <a name="webservicepackageerror-http-status-code-400"></a>WebServicePackageError (HTTP-Statuscode 400)
 
Ungültiges Webdienstpaket. Stellen Sie sicher, dass das angegebene Webdienstpaket korrekt ist, und wiederholen Sie den Vorgang.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| FormatError | Das Webdienstpaket ist nicht wohlgeformt. Details: {0} |
| RuntimesError | Der Webdienstpaket-Graph ist ungültig. Details: {0} |
| ValidationError | Der Webdienstpaket-Graph ist ungültig. Details: {0} |
 
## <a name="unauthorized-http-status-code-401"></a>Unauthorized (HTTP-Statuscode 401)
 
Die Anforderung ist nicht berechtigt, auf die Ressource zuzugreifen.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| AdminRequestUnauthorized | Nicht autorisiert |
| ManagementRequestUnauthorized | Nicht autorisiert |
| ScoreRequestUnauthorized | Es wurden ungültige Anmeldeinformationen angegeben. |
 
## <a name="notfound-http-status-code-404"></a>NotFound (HTTP-Statuscode 404)
 
Ressource nicht gefunden.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| ModelPackageNotFound | Modellpaket nicht gefunden. Stellen Sie sicher, dass die Modellpaket-ID korrekt ist, und wiederholen Sie den Vorgang. |
| WebServiceIdNotFoundInWorkspace | Der Webdienst wurde unter diesem Arbeitsbereich nicht gefunden. Es besteht ein Konflikt zwischen webServiceId und workspaceId. Stellen Sie sicher, dass der angegebene Webdienst Teil des Arbeitsbereichs ist, und wiederholen Sie den Vorgang. |
| WebServiceNotFound | Webdienst nicht gefunden. Stellen Sie sicher, dass die Webdienst-ID korrekt ist, und wiederholen Sie den Vorgang. |
| WorkspaceNotFound | Arbeitsbereich nicht gefunden. Stellen Sie sicher, dass die Arbeitsbereich-ID korrekt ist, und wiederholen Sie den Vorgang. |
 
## <a name="requesttimeout-http-status-code-408"></a>RequestTimeout (HTTP-Statuscode 408)
 
Der Vorgang konnte nicht innerhalb der zulässigen Zeit abgeschlossen werden.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| RequestCanceled | Die Anforderung wurde vom Client abgebrochen. |
| ScoreRequestTimeout | Zeitüberschreitung für die Ausführung der Anforderung. |
 
## <a name="conflict-http-status-code-409"></a>Conflict (HTTP-Statuscode 409)
 
Ressource ist bereits vorhanden.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| ModelOutputMetadataMismatch | Ungültiger Ausgabeparametername. Versuchen Sie, das Metadaten-Editor-Modul zu verwenden, um Spalten umzubenennen, und wiederholen Sie den Vorgang. |
 
## <a name="memoryquotaviolation-http-status-code-413"></a>MemoryQuotaViolation (HTTP-Statuscode 413)
 
Für das Modell wurde das zugewiesene Arbeitsspeicherkontingent überschritten.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| OutOfMemoryLimit | Das Modell hat mehr Arbeitsspeicher als vorgesehen verbraucht. Der maximal zulässige Arbeitsspeicher für das Modell beträgt {0} MB. Überprüfen Sie das Modell auf Probleme. |
 
## <a name="internalerror-http-status-code-500"></a>InternalError (HTTP-Statuscode 500)
 
Bei der Ausführung ist ein interner Fehler aufgetreten.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| AdminAuthenticationFailed |  |
| BackendArgumentError |  |
| BackendBadRequest |  |
| ClusterConfigBlobMisconfigured |  |
| ContainerProcessTerminatedWithSystemError | Der Containerprozess ist mit einem Systemfehler abgestürzt. |
| ContainerProcessTerminatedWithUnknownError | Der Containerprozess ist mit einem unbekannten Fehler abgestürzt. |
| ContainerValidationFailed | Bei der Überprüfung des Blobcontainers ist dieser Fehler aufgetreten: {0}. |
| DeleteWebServiceResourceFailed |  |
| ExceptionDeserializationError |  |
| FailedGettingApiDocument |  |
| FailedStoringWebService |  |
| InvalidMemoryConfiguration | InvalidMemoryConfiguration, ConfigValue: {0} |
| InvalidResourceCacheConfiguration |  |
| InvalidResourceDownloadConfiguration |  |
| InvalidWebServiceResources |  |
| MissingTaskInstance | Keine Argumente angegeben. Vergewissern Sie sich, dass gültige Argumente übergeben werden, und wiederholen Sie den Vorgang. |
| ModelPackageInvalid |  |
| ModuleExecutionFailed |  |
| ModuleLoadFailed |  |
| ModuleObjectCloneFailed |  |
| OutputConversionFailed |  |
| PortDataTypeNotSupported | Port id={0} weist einen nicht unterstützten Datentyp auf: {1}. |
| ResourceDownload |  |
| ResourceLoadFailed |  |
| ServiceUrisNotFound |  |
| SwaggerGeneration | Fehler bei Swagger-Generierung, Details: {0} |
| UnexpectedScoreStatus |  |
| UnknownBackendErrorResponse |  |
| UnknownError |  |
| UnknownJobStatusCode | Unbekannter Auftragsstatuscode {0}. |
| UnknownModuleError |  |
| UpdateWebServiceResourceFailed |  |
| WebServiceGroupNotFound |  |
| WebServicePackageInvalid | InvalidWebServicePackage, Details: {0} |
| WorkerAuthorizationFailed |  |
| WorkerUnreachable |  |
 
## <a name="internalerrorsystemlowonmemory-http-status-code-500"></a>InternalErrorSystemLowOnMemory (HTTP-Statuscode 500)
 
Bei der Ausführung ist ein interner Fehler aufgetreten. Das System verfügt nicht über genügend Arbeitsspeicher. Wiederholen Sie den Vorgang.
 
 
## <a name="modelpackageformaterror-http-status-code-500"></a>ModelPackageFormatError (HTTP-Statuscode 500)
 
Ungültiges Modellpaket. Stellen Sie sicher, dass das angegebene Modellpaket korrekt ist, und wiederholen Sie den Vorgang.
 
 
## <a name="webservicepackageinternalerror-http-status-code-500"></a>WebServicePackageInternalError (HTTP-Statuscode 500)
 
Ungültiges Webdienstpaket. Stellen Sie sicher, dass das angegebene Webpaket korrekt ist, und wiederholen Sie den Vorgang.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| ModuleError | Der Webdienstpaket-Graph ist ungültig. Details: {0} |
 
## <a name="initializingcontainers-http-status-code-503"></a>InitializingContainers (HTTP-Statuscode 503)
 
Die Anforderung kann nicht ausgeführt werden, da die Container initialisiert werden.
 
 
## <a name="serviceunavailable-http-status-code-503"></a>ServiceUnavailable (HTTP-Statuscode 503)
 
Der Dienst ist vorübergehend nicht verfügbar.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| NoMoreResources | Keine Ressourcen für Anforderung verfügbar. |
| RequestThrottled | Anforderung wurde für Endpunkt {0} gedrosselt. Die maximale Parallelität für den Endpunkt beträgt {1}. |
| TooManyConcurrentRequests | Zu hohe Zahl von gleichzeitig gesendeten Anforderungen. |
| TooManyHostsBeingInitialized | Zu viele Hosts werden zur gleichen Zeit initialisiert. Erwägen Sie eine Drosselung oder eine Wiederholung des Vorgangs. |
| TooManyHostsBeingInitializedPerModel | Zu viele Hosts werden zur gleichen Zeit initialisiert. Erwägen Sie eine Drosselung oder eine Wiederholung des Vorgangs. |
 
## <a name="gatewaytimeout-http-status-code-504"></a>GatewayTimeout (HTTP-Statuscode 504)
 
Der Vorgang konnte nicht innerhalb der zulässigen Zeit abgeschlossen werden.
 
| Fehlercode | Meldung für den Benutzer |
| ---------- |--------------|
| BackendInitializationTimeout | Die Webdienstinitialisierung konnte nicht innerhalb der zulässigen Dauer abgeschlossen werden. |
| BackendScoreTimeout | Die Ausführung der Webdienstanforderung konnte nicht innerhalb der zulässigen Dauer abgeschlossen werden. |
 



<!--HONumber=Nov16_HO3-->



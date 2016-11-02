<properties
   pageTitle="Allgemeine FabricClient-Ausnahmen | Microsoft Azure"
   description="Beschreibt die allgemeinen Ausnahmen und Fehler, die von den FabricClient-APIs beim Ausführen der Anwendungs- und Clusterverwaltungsvorgänge ausgelöst werden können."
   services="service-fabric"
   documentationCenter=".net"
   authors="rwike77"
   manager="timlt"
   editor=""/>

<tags
   ms.service="service-fabric"
   ms.devlang="dotnet"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="NA"
   ms.date="08/25/2016"
   ms.author="ryanwi"/>

# Allgemeine Ausnahmen und Fehler bei der Arbeit mit FabricClient-APIs
Mit [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx)-APIs können Cluster- und Anwendungsadministratoren administrative Aufgaben für eine Service Fabric-Anwendung, einen Dienst oder Cluster ausführen. Dazu zählen z. B. Anwendungsbereitstellung, Upgrade und Entfernung, Überprüfung der Integrität eines Clusters oder Testen eines Diensts. Anwendungsentwickler und Clusteradministratoren können die FabricClient-APIs verwenden, um Tools zum Verwalten der Service Fabric-Cluster und -Anwendungen zu entwickeln.

Viele verschiedene Vorgänge können mit FabricClient ausgeführt werden. Jede Methode kann Ausnahmen für Fehler aufgrund von falschen Eingaben, Laufzeitfehlern oder vorübergehenden Infrastrukturproblemen auslösen. Bitte entnehmen Sie der API-Referenzdokumentation, welche Ausnahmen von bestimmten Methoden ausgelöst werden. Einige Ausnahmen können jedoch von verschiedenen [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx)-APIs ausgelöst werden. Die folgende Tabelle enthält die Ausnahmen, die für alle FabricClient-APIs gelten.

|Ausnahme| Grund für Auslösung|
|---------|:-----------|
|[System.Fabric.FabricObjectClosedException](https://msdn.microsoft.com/library/system.fabric.fabricobjectclosedexception.aspx)|Das [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx)-Objekt befindet sich im geschlossenen Status. Löschen Sie das [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx)-Objekt, das Sie verwenden, und instanziieren Sie ein neues [FabricClient](https://msdn.microsoft.com/library/system.fabric.fabricclient.aspx)-Objekt. |
|[System.TimeoutException](https://msdn.microsoft.com/library/system.timeoutexception.aspx)|Beim Vorgang ist ein Timeout aufgetreten. [OperationTimedOut](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx) wird zurückgegeben, wenn die Ausführung des Vorgangs länger dauert, als in „MaxOperationTimeout“ vorgegeben.|
|[System.UnauthorizedAccessException](https://msdn.microsoft.com/de-DE/library/system.unauthorizedaccessexception.aspx)|Die Zugriffsüberprüfung für den Vorgang ist fehlgeschlagen. „E\_ACCESSDENIED“ wird zurückgegeben.|
|[System.Fabric.FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx)|Beim Ausführen des Vorgangs ist ein Laufzeitfehler aufgetreten. Eine der FabricClient-Methoden kann potenziell [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx) auslösen. Die [ErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricexception.errorcode.aspx)-Eigenschaft gibt die genaue Ursache der Ausnahme an. Fehlercodes werden in der [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx)-Enumeration definiert.|
|[System.Fabric.FabricTransientException](https://msdn.microsoft.com/library/system.fabric.fabrictransientexception.aspx)|Der Vorgang ist aufgrund eines vorübergehenden Fehlerzustands beliebiger Art fehlgeschlagen. Beispielsweise kann ein Vorgang fehlschlagen, da ein Quorum von Replikaten vorübergehend nicht erreichbar ist. Vorübergehende Ausnahmen entsprechen fehlgeschlagenen Vorgängen, die wiederholt werden können.|

Einige häufige [FabricErrorCode](https://msdn.microsoft.com/library/system.fabric.fabricerrorcode.aspx)-Fehler, die in einer [FabricException](https://msdn.microsoft.com/library/system.fabric.fabricexception.aspx) zurückgegeben werden können:

|Error| Bedingung|
|---------|:-----------|
|CommunicationError|Ein Kommunikationsfehler verursachte das Fehlschlagen des Vorgangs – wiederholen Sie den Vorgang.|
|InvalidCredentialType|Der Anmeldeinformationstyp ist ungültig.|
|InvalidX509FindType|Der X509FindType ist ungültig.|
|InvalidX509StoreLocation|Der X509-Speicherort ist ungültig.|
|InvalidX509StoreName|Die X509-Speichername ist ungültig.|
|InvalidX509Thumbprint|Die X509-Zertifikatfingerabdruck-Zeichenfolge ist ungültig.|
|InvalidProtectionLevel|Die Schutzebene ist ungültig.|
|InvalidX509Store|Der X509-Zertifikatspeicher kann nicht geöffnet werden.|
|InvalidSubjectName|Der Antragstellername ist ungültig.|
|InvalidAllowedCommonNameList|Das Format der Zeichenfolge der Liste allgemeiner Namen ist ungültig. Es sollte eine durch Trennzeichen getrennte Liste sein.|

<!---HONumber=AcomDC_0831_2016-->
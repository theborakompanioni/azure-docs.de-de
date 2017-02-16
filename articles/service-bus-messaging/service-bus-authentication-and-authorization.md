---
title: Service Bus-Authentifizierung und -Autorisierung | Microsoft Docs
description: "Übersicht über die SAS-Authentifizierung (Shared Access Signature)."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 18bad0ed-1cee-4a5c-a377-facc4785c8c9
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/13/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2c1a1f3b73466526b13bcfeb4580335390506c23
ms.openlocfilehash: 14dfe58f6296a4ec516845bace456ffd59fa608a


---
# <a name="service-bus-authentication-and-authorization"></a>Service Bus-Authentifizierung und -Autorisierung
Anwendungen können sich bei Azure Service Bus mithilfe der SAS-Authentifizierung (Shared Access Signature) oder über Azure Active Directory Access Control (auch Access Control Service oder ACS genannt) authentifizieren. Die SAS-Authentifizierung ermöglicht Anwendungen die Authentifizierung bei Service Bus mithilfe eines Zugriffsschlüssels, der für den Namespace oder für die Entität konfiguriert wird, welcher bestimmte Rechte zugeordnet sind. Sie können diesen Schlüssel zum Generieren eines SAS-Tokens verwenden, das Clients für die Authentifizierung bei Service Bus verwenden können.

> [!IMPORTANT]
> Von diesen beiden Möglichkeiten wird SAS empfohlen, da diese Authentifizierung ein einfaches, flexibles und komfortables Authentifizierungsschema für Service Bus bereitstellt. Anwendungen können SAS in Szenarios verwenden, in denen kein autorisierter „Benutzer“ verwaltet werden muss. 

## <a name="shared-access-signature-authentication"></a>SAS-Authentifizierung (Shared Access Signature)
Mit der [SAS-Authentifizierung](service-bus-sas-overview.md) können Sie einem Benutzer Zugriff auf Service Bus-Ressourcen mit spezifischen Rechten gewähren. Die SAS-Authentifizierung in Service Bus umfasst die Konfiguration eines kryptografischen Schlüssels mit den zugehörigen Rechten für eine Service Bus-Ressource. Clients können Zugriff auf diese Ressource erlangen, indem sie ein SAS-Token bereitstellen. Dieses setzt sich aus dem Ressourcen-URI, auf den zugegriffen wird, und einer Ablaufangabe zusammen, die mit dem konfigurierten Schlüssel signiert wird.

Sie können Schlüssel für SAS für einen Service Bus-Namespace konfigurieren. Der betreffende Schlüssel gilt für alle Messagingentitäten in dem jeweiligen Namespace. Sie können auch Schlüssel für Service Bus-Warteschlangen und -Themen konfigurieren. SAS wird auch für Service Bus Relays unterstützt.

Wenn Sie SAS verwenden möchten, können Sie ein [SharedAccessAuthorizationRule](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) -Objekt für einen Namespace, eine Warteschlange oder ein Thema konfigurieren, das aus den folgenden Elementen besteht:

* *KeyName* , der die Regel identifiziert.
* *PrimaryKey* – einem kryptografischen Schlüssel, der zum Signieren/Überprüfen von SAS-Token verwendet wird.
* *SecondaryKey* – einem kryptografischen Schlüssel, der zum Signieren/Überprüfen von SAS-Token verwendet wird.
* *Rechten* , die die Auflistung der erteilten Lausch-, Sende- oder Verwaltungsrechte (Listen, Send, Manage) darstellen.

Autorisierungsregeln, die auf Namespace-Ebene konfiguriert werden, können Zugriff auf alle Entitäten in einem Namespace für Clients mit Token erteilen, die mithilfe des entsprechenden Schlüssels signiert wurden. Bis zu zwölf solcher Autorisierungsregeln können für einen Service Bus-Namespace, eine Service Bus-Warteschlange oder ein Service Bus-Thema konfiguriert werden. Standardmäßig wird für jeden Namespace bei der ersten Bereitstellung ein [SharedAccessAuthorizationRule](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) -Objekt mit allen Rechten konfiguriert.

Für den Zugriff auf eine Entität erfordert der Client ein SAS-Token, das mithilfe einer bestimmten [SharedAccessAuthorizationRule](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) generiert wurde. Das SAS-Token wird anhand des HMAC-SHA256-Codes einer Ressourcenzeichenfolge generiert. Diese  besteht aus dem Ressourcen-URI, auf den der Zugriff beansprucht wird, sowie aus einer Ablaufangabe mit einem kryptografischen Schlüssel, der der Autorisierungsregel zugeordnet ist.

Die Unterstützung der SAS-Authentifizierung für Service Bus ist im Azure .NET SDK, Version 2.0 oder höher, enthalten. SAS umfasst Unterstützung für eine [SharedAccessAuthorizationRule](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Alle APIs, die eine Verbindungszeichenfolge als Parameter akzeptieren, enthalten Unterstützung für SAS-Verbindungszeichenfolgen.

## <a name="acs-authentication"></a>ACS-Authentifizierung
Die Service Bus-Authentifizierung über ACS wird über den begleitenden ACS-Namespace „-sb“ verwaltet. Wenn Sie einen begleitenden ACS-Namespace für einen Service Bus-Namespace benötigen, können Sie den Service Bus-Namespace nicht über das klassische Azure-Portal erstellen. Sie müssen den Namespace stattdessen mit dem PowerShell-Cmdlet [New-AzureSBNamespace](https://docs.microsoft.com/powershell/servicemanagement/azure.compute/v1.6.1/New-AzureSBNamespace) erstellen. Beispiel:

```powershell
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $true
```

Verwenden Sie den folgenden Befehl, um zu vermeiden, dass ein ACS-Namespace erstellt wird:

```powershell
New-AzureSBNamespace <namespaceName> "<Region>” -CreateACSNamespace $false
```

Wenn Sie z.B. einen Service Bus-Namespace namens **contoso.servicebus.windows.net** erstellen, wird automatisch ein begleitender ACS-Namespace namens **contoso-sb.accesscontrol.windows.net** bereitgestellt. Für alle Namespaces, die vor August 2014 erstellt wurden, wurde ein begleitender ACS-Namespace erstellt.

Ein „Standardbesitzer“ der Dienstidentität (mit allen Rechten) wird standardmäßig in diesem begleitenden ACS-Namespace bereitgestellt. Sie können über ACS eine differenzierte Steuerung jeder Service Bus-Entität erzielen, indem Sie die geeigneten Vertrauensstellungen konfigurieren. Sie können zusätzliche Dienstidentitäten zum Verwalten des Zugriffs auf Service Bus-Entitäten konfigurieren.

Für den Zugriff auf eine Entität fordert der Client ein SWT-Token von ACS mit den entsprechenden Ansprüchen an, indem er seine Anmeldeinformationen bereitstellt. Das SWT-Token muss dann als Teil der Anforderung an Service Bus gesendet werden, um die Autorisierung des Clients für den Zugriff auf die Entität zu aktivieren.

Die Unterstützung der ACS-Authentifizierung für Service Bus ist im Azure .NET SDK, Version 2.0 oder höher, enthalten. Diese Authentifizierung umfasst Unterstützung für einen [SharedSecretTokenProvider](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.sharedsecrettokenprovider). Alle APIs, die eine Verbindungszeichenfolge als Parameter akzeptieren, enthalten Unterstützung für ACS-Verbindungszeichenfolgen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zu SAS finden Sie unter [SAS-Authentifizierung (Shared Access Signature) bei Service Bus](service-bus-shared-access-signature-authentication.md) .

Eine allgemeine Übersicht über SAS in Service Bus finden Sie unter [SAS (Shared Access Signatures)](service-bus-sas-overview.md).



<!--HONumber=Jan17_HO3-->



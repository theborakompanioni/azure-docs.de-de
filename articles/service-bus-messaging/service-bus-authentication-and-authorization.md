---
title: Azure Service Bus-Authentifizierung und -Autorisierung | Microsoft-Dokumentation
description: Authentifizieren Sie Service Bus-Apps mit Shared Access Signature-Authentifizierung (SAS).
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
ms.date: 08/09/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 9569f94d736049f8a0bb61beef0734050ecf2738
ms.openlocfilehash: b4b9d5d272bdb172f1d40db379a519a4f617550a
ms.contentlocale: de-de
ms.lasthandoff: 08/31/2017

---
# <a name="service-bus-authentication-and-authorization"></a>Service Bus-Authentifizierung und -Autorisierung

Anwendungen können mithilfe der SAS-Tokenauthentifizierung (Shared Access Signature) Zugriff auf Azure Service Bus-Funktionen erhalten. Mit SAS legen Anwendungen Service Bus ein Token vor, das mit einem symmetrischen Schlüssel signiert wurde, der sowohl dem Tokenaussteller als auch Service Bus bekannt ist („shared“), und dieser Schlüssel ist direkt einer Regel zugeordnet, die bestimmte Zugriffsrechte erteilt, z.B. die Berechtigung zum Empfangen von/Lauschen auf oder Senden von Nachrichten. SAS-Regeln werden entweder auf dem Namespace oder direkt auf Entitäten wie einer Warteschlange oder einem Thema konfiguriert, sodass eine differenzierte Zugriffssteuerung möglich ist.

SAS-Token können entweder direkt von einem Service Bus-Client oder einem dazwischen liegenden Tokenausstellungs-Endpunkt generiert werden, mit dem der Client interagiert. Ein System könnte z.B. voraussetzen, dass der Client einen durch Active Directory-Autorisierung geschützten Webdienst-Endpunkt aufruft, um seine Identität und seine Systemzugriffsrechte nachzuweisen, und der Webdienst würde dann das entsprechende Service Bus-Token zurückgeben. Dieses SAS-Token kann problemlos mit dem Service Bus-Tokenanbieter generiert werden, der im SDK enthalten ist. 

> [!IMPORTANT]
> Wenn Sie die Azure Active Directory-Zugriffssteuerung (auch als Access Control Service oder ACS bezeichnet) in Verbindung mit Service Bus verwenden, beachten Sie, dass die Unterstützung dieser Methode jetzt beschränkt ist und Sie Ihre Anwendung zur Verwendung von SAS migrieren sollten. Weitere Informationen finden Sie in [diesem Blogbeitrag](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/).

## <a name="shared-access-signature-authentication"></a>SAS-Authentifizierung (Shared Access Signature)

Mit der [SAS-Authentifizierung](service-bus-sas.md) können Sie einem Benutzer Zugriff auf Service Bus-Ressourcen mit spezifischen Rechten gewähren. Die SAS-Authentifizierung in Service Bus umfasst die Konfiguration eines kryptografischen Schlüssels mit den zugehörigen Rechten für eine Service Bus-Ressource. Clients können Zugriff auf diese Ressource erlangen, indem sie ein SAS-Token bereitstellen. Dieses setzt sich aus dem Ressourcen-URI, auf den zugegriffen wird, und einer Ablaufangabe zusammen, die mit dem konfigurierten Schlüssel signiert wird.

Sie können Schlüssel für SAS für einen Service Bus-Namespace konfigurieren. Der betreffende Schlüssel gilt für alle Messagingentitäten in dem jeweiligen Namespace. Sie können auch Schlüssel für Service Bus-Warteschlangen und -Themen konfigurieren. SAS wird in [Azure Relay](../service-bus-relay/relay-authentication-and-authorization.md) ebenfalls unterstützt.

Wenn Sie SAS verwenden möchten, können Sie ein [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule)-Objekt für einen Namespace, eine Warteschlange oder ein Thema konfigurieren. Diese Regel besteht aus den folgenden Elementen:

* *KeyName* , der die Regel identifiziert.
* *PrimaryKey* – einem kryptografischen Schlüssel, der zum Signieren/Überprüfen von SAS-Token verwendet wird.
* *SecondaryKey* – einem kryptografischen Schlüssel, der zum Signieren/Überprüfen von SAS-Token verwendet wird.
* *Rechten* , die die Auflistung der erteilten Lausch-, Sende- oder Verwaltungsrechte (Listen, Send, Manage) darstellen.

Autorisierungsregeln, die auf Namespace-Ebene konfiguriert werden, können Zugriff auf alle Entitäten in einem Namespace für Clients mit Token erteilen, die mithilfe des entsprechenden Schlüssels signiert wurden. Bis zu zwölf solcher Autorisierungsregeln können für einen Service Bus-Namespace, eine Service Bus-Warteschlange oder ein Service Bus-Thema konfiguriert werden. Standardmäßig wird für jeden Namespace bei der ersten Bereitstellung ein [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) -Objekt mit allen Rechten konfiguriert.

Für den Zugriff auf eine Entität erfordert der Client ein SAS-Token, das mithilfe einer bestimmten [SharedAccessAuthorizationRule](/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule) generiert wurde. Das SAS-Token wird anhand des HMAC-SHA256-Codes einer Ressourcenzeichenfolge generiert. Diese  besteht aus dem Ressourcen-URI, auf den der Zugriff beansprucht wird, sowie aus einer Ablaufangabe mit einem kryptografischen Schlüssel, der der Autorisierungsregel zugeordnet ist.

Die Unterstützung der SAS-Authentifizierung für Service Bus ist im Azure .NET SDK, Version 2.0 oder höher, enthalten. SAS umfasst Unterstützung für eine [SharedAccessAuthorizationRule](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.sharedaccessauthorizationrule). Alle APIs, die eine Verbindungszeichenfolge als Parameter akzeptieren, enthalten Unterstützung für SAS-Verbindungszeichenfolgen.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu SAS finden Sie unter [SAS-Authentifizierung (Shared Access Signature) bei Service Bus](service-bus-sas.md) .
- [Änderungen an Namespaces mit ACS-Unterstützung](https://blogs.msdn.microsoft.com/servicebus/2017/06/01/upcoming-changes-to-acs-enabled-namespaces/)
- Die entsprechenden Informationen zur Azure Relay-Authentifizierung und -Autorisierung finden Sie unter [Azure Relay-Authentifizierung und -Autorisierung](../service-bus-relay/relay-authentication-and-authorization.md). 



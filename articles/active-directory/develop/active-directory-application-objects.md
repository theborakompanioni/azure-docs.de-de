---
title: Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory | Microsoft-Dokumentation
description: "Enthält eine Beschreibung der Beziehung zwischen Anwendungsobjekten und Dienstprinzipalobjekten in Azure Active Directory."
documentationcenter: dev-center-name
author: bryanla
manager: mbaldwin
services: active-directory
editor: 
ms.assetid: adfc0569-dc91-48fe-92c3-b5b4833703de
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/29/2016
ms.author: bryanla;mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 1e75c89498d96712c63a5a992c6de13e74300c50
ms.openlocfilehash: 6d2b0d0c28963693f8fa5607200e73572ea748a0


---
# <a name="application-and-service-principal-objects-in-azure-active-directory"></a>Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory
Manchmal kann die Bedeutung des Azure Active Directory-Begriffs (Azure AD) „Anwendung“ einfach missverstanden werden, insbesondere wenn nicht ausreichend Kontext vorliegt. Das Ziel dieses Artikels besteht darin, dies zu verdeutlichen. Hierzu werden die konzeptuellen und konkreten Aspekte der Azure AD-Anwendungsintegration veranschaulicht, gefolgt von einer Erläuterung der Registrierung und Zustimmung zu einer [mehrinstanzenfähigen Anwendung](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Übersicht
Eine Azure AD-Anwendung ist nicht nur eine einfache Software. Es ist ein konzeptueller Begriff, der sich nicht nur auf die Anwendungssoftware bezieht, sondern auch auf die Registrierung (also: Identitätskonfiguration) bei Azure AD, um die Teilnahme an „Gesprächen“ zur Authentifizierung und Autorisierung zur Laufzeit zu ermöglichen. Der Definition nach kann eine Anwendung in einer [Client](active-directory-dev-glossary.md#client-application)rolle (Nutzung einer Ressource), einer [Ressourcenserver](active-directory-dev-glossary.md#resource-server)rolle (APIs werden für Clients verfügbar gemacht) oder beiden Rollen funktionieren. Das Konversationsprotokoll ist durch einen [Ablauf zur Erteilung einer OAuth 2.0-Autorisierung](active-directory-dev-glossary.md#authorization-grant) definiert und hat das Ziel, dem Client bzw. der Ressource das Zugreifen auf bzw. das Schützen der Daten einer Ressource zu ermöglichen. Nun gehen wir eine Ebene tiefer und sehen uns an, wie das Azure AD-Anwendungsmodell eine Anwendung intern darstellt. 

## <a name="application-registration"></a>Anwendungsregistrierung
Wenn Sie eine Anwendung im [klassischen Azure-Portal][AZURE-Classic-Portal] registrieren, werden im Azure AD-Mandanten zwei Objekte erstellt: ein Anwendungsobjekt und ein Dienstprinzipalobjekt.

#### <a name="application-object"></a>Anwendungsobjekt
Eine Azure AD-Anwendung wird durch ihr alleiniges Anwendungsobjekt *definiert*. Es befindet sich im Azure AD-Mandanten, in dem die Anwendung registriert ist und der als „Home“-Mandant der Anwendung bezeichnet wird. Das Anwendungsobjekt enthält identitätsbezogene Informationen für eine Anwendung und ist die Vorlage, von der die entsprechenden Dienstprinzipalobjekte für die Verwendung zur Laufzeit *abgeleitet* werden. 

Stellen Sie sich die Anwendung als *globale* Darstellung Ihrer Anwendung (zur Verwendung über alle Mandanten hinweg) und den Dienstprinzipal als *lokale* Darstellung (zur Verwendung in einem bestimmten Mandanten) vor. Die [Application-Entität][AAD-Graph-App-Entity] von Azure AD Graph definiert das Schema für ein Anwendungsobjekt. Für ein Anwendungsobjekt besteht daher eine 1:1-Beziehung mit der Softwareanwendung und eine 1:*n*-Beziehung mit den entsprechenden *n* Dienstprinzipalobjekten.

#### <a name="service-principal-object"></a>Dienstprinzipalobjekt
Das Dienstprinzipalobjekt definiert die Richtlinie und Berechtigungen für eine Anwendung und stellt die Basis für einen Sicherheitsprinzipal bereit, der die Anwendung repräsentiert, wenn zur Laufzeit auf Ressourcen zugegriffen wird. Die [ServicePrincipal-Entität][AAD-Graph-Sp-Entity] von Azure AD Graph definiert das Schema für ein Dienstprinzipalobjekt. 

Bevor ein Azure AD-Mandant einer Anwendung den Zugriff auf die von ihm gesicherten Ressourcen erlaubt, muss ein Dienstprinzipal im entsprechenden Mandanten erstellt werden. Auf der Grundlage des Dienstprinzipals kann Azure AD den Zugriff der Anwendung auf Ressourcen schützen, die sich im Besitz von Benutzern in diesem Mandanten befinden. Eine Anwendung mit nur einem Mandanten besitzt nur einen Dienstprinzipal (im eigenen Home-Mandanten). Eine mehrinstanzenfähige [Webanwendung](active-directory-dev-glossary.md#web-client) verfügt ebenfalls über einen Dienstprinzipal in jedem Mandanten, in dem ein Administrator oder mindestens ein Benutzer dieses Mandanten ihre Zustimmung gegeben haben, dass die Anwendung auf die Ressourcen des Mandanten zugreifen darf. Nach der Erteilung der Zustimmung wird das Dienstprinzipalobjekt bei zukünftigen Autorisierungsanforderungen herangezogen. 

> [!NOTE]
> Alle Änderungen, die Sie an Ihrem Anwendungsobjekt vornehmen, werden auch in seinem Dienstprinzipalobjekt ausschließlich im Home-Mandanten der Anwendung widergespiegelt (der Mandant, für den die Registrierung durchgeführt wurde). Bei mehrinstanzenfähigen Anwendungen werden Änderungen am Anwendungsobjekt erst dann in den Dienstprinzipalobjekten von Endkundenmandanten widergespiegelt, wenn der Endkundenmandant den Zugriff entfernt und wieder gewährt.
> 
> 

## <a name="example"></a>Beispiel
Das folgende Diagramm veranschaulicht die Beziehung zwischen einem Anwendungsobjekt der Anwendung und den entsprechenden Dienstprinzipalobjekten im Kontext einer mehrinstanzenfähigen Beispielanwendung mit dem Namen **HR App**. In diesem Szenario werden drei Azure AD-Mandanten verwendet: 

* **Adatum**: Der Mandant, der von dem Unternehmen verwendet wird, das die **HR App** entwickelt hat
* **Contoso**: Der Mandant, der von der Organisation Contoso (einem Endkunden der **HR App**) verwendet wird
* **Fabrikam**: Der von der Organisation Fabrikam (ebenfalls ein Endkunde der **HR App**) verwendete Mandant

![Beziehung zwischen einem Anwendungsobjekt und einem Dienstprinzipalobjekt](./media/active-directory-application-objects/application-objects-relationship.png)

Im obigen Diagramm ist Schritt 1 der Prozess zur Erstellung der Anwendungs- und Dienstprinzipalobjekte im Home-Mandanten der Anwendung.

Nachdem die Administratoren von Contoso und Fabrikam die Zustimmung erteilt haben, wird in Schritt 2 im Azure AD-Mandanten des Unternehmens ein Dienstprinzipalobjekt erstellt und mit den Berechtigungen versehen, die vom Administrator gewährt wurden. Beachten Sie auch, dass die HR App so konfiguriert bzw. entworfen werden kann, dass die Zustimmung für die individuelle Nutzung durch die Benutzer erteilt wird.

In Schritt 3 erhalten die Endkundenmandanten der HR-Anwendung (Contoso und Fabrikam) jeweils ein eigenes Dienstprinzipalobjekt. Jedes Objekt repräsentiert die Verwendung einer Instanz der Anwendung zur Laufzeit, gesteuert durch die Berechtigungen, die vom jeweiligen Administrator per Zustimmung erteilt wurden.

## <a name="next-steps"></a>Nächste Schritte
Sie können auf das Anwendungsobjekt einer Anwendung über die Azure AD Graph-API zugreifen, wie von der OData-[Application-Entität][AAD-Graph-App-Entity] dargestellt.

Sie können auf das Dienstprinzipalobjekt einer Anwendung über die Azure AD Graph-API zugreifen, wie von der OData-[ServicePrincipal-Entität][AAD-Graph-Sp-Entity] dargestellt.

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Classic-Portal]: https://manage.windowsazure.com



<!--HONumber=Dec16_HO4-->



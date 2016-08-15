<properties
pageTitle="Azure Active Directory-Anwendungs- und -Dienstprinzipalobjekte | Microsoft Azure"
description="Enthält eine Beschreibung der Beziehung zwischen Anwendungsobjekten und Dienstprinzipalobjekten in Azure Active Directory."
documentationCenter="dev-center-name"
authors="bryanla"
manager="mbaldwin"
services="active-directory"
editor=""/>

<tags
ms.service="active-directory"
ms.devlang="na"
ms.topic="article"
ms.tgt_pltfrm="na"
ms.workload="identity"
ms.date="07/14/2016"
ms.author="bryanla;mbaldwin"/>

# Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory
Wenn Sie Azure Active Directory-„Anwendung“ (AD) lesen, ist vielleicht nicht immer völlig klar, was der Autor damit genau meint. Das Ziel dieses Artikels besteht darin, dies besser zu erklären. Hierzu werden die konzeptuellen und konkreten Aspekte der Azure AD-Anwendungsintegration definiert, gefolgt von einem Beispiel für die Registrierung und Zustimmung zu einer [mehrinstanzenfähigen Anwendung](active-directory-dev-glossary.md#multi-tenant-application).

## Übersicht
Eine Azure AD-Anwendung ist nicht nur eine einfache Software. Es ist ein konzeptueller Begriff, der sich nicht nur auf die Anwendungssoftware bezieht, sondern auch auf die Registrierung (also: Identitätskonfiguration) bei Azure AD, um die Teilnahme an „Gesprächen“ zur Authentifizierung und Autorisierung zur Laufzeit zu ermöglichen. Der Definition nach kann eine Anwendung in einer [Client](active-directory-dev-glossary.md#client-application)rolle (Nutzung einer Ressource), einer [Ressourcenserver](active-directory-dev-glossary.md#resource-server)rolle (APIs werden für Clients verfügbar gemacht) oder beiden Rollen funktionieren. Das Konversationsprotokoll ist durch einen [Ablauf zur Erteilung einer OAuth 2.0-Autorisierung](active-directory-dev-glossary.md#authorization-grant) definiert und hat das Ziel, dem Client bzw. der Ressource das Zugreifen auf bzw. das Schützen der Daten einer Ressource zu ermöglichen. Nun gehen wir eine Ebene tiefer und sehen uns an, wie das Azure AD-Anwendungsmodell eine Anwendung intern darstellt.

## Anwendungsregistrierung
Wenn Sie eine Anwendung im [klassischen Azure-Portal](https://manage.windowsazure.com) registrieren, werden im Azure AD-Mandanten zwei Objekte erstellt: ein Anwendungsobjekt und ein Dienstprinzipalobjekt.

#### Anwendungsobjekt
Eine Azure AD-Anwendung wird durch ihr alleiniges Anwendungsobjekt *definiert*. Es befindet sich im Azure AD-Mandanten, in dem die Anwendung registriert ist und der als „Home“-Mandant der Anwendung bezeichnet wird. Das Anwendungsobjekt enthält identitätsbezogene Informationen für eine Anwendung und ist die Vorlage, von der die entsprechenden Dienstprinzipalobjekte für die Verwendung zur Laufzeit *abgeleitet* werden.

Sie können sich die Anwendung als *globale* Darstellung Ihrer Anwendung (zur Verwendung über alle Mandanten hinweg) und den Dienstprinzipal als *lokale* Darstellung (zur Verwendung in einem bestimmten Mandanten) vorstellen. Die [Application-Entität][AAD-Graph-App-Entity] von Azure AD Graph definiert das Schema für ein Anwendungsobjekt. Für ein Anwendungsobjekt besteht daher eine 1:1-Beziehung mit der Softwareanwendung und eine 1:*n*-Beziehung mit den entsprechenden *n* Dienstprinzipalobjekten.

#### Dienstprinzipalobjekt
Das Dienstprinzipalobjekt definiert die Richtlinie und Berechtigungen für eine Anwendung und liefert die Grundlage für einen Dienstprinzipal zum Darstellen der Anwendung, wenn zur Laufzeit auf Ressourcen zugegriffen wird. Die [ServicePrincipal-Entität][AAD-Graph-Sp-Entity] von Azure AD Graph definiert das Schema für ein Dienstprinzipalobjekt.

Ein Dienstprinzipalobjekt ist in jedem Mandanten erforderlich, für den eine Instanz der Anwendungsnutzung dargestellt werden muss. So wird der sichere Zugriff von diesem Mandanten auf die Ressourcen ermöglicht, die sich im Besitz von Benutzerkonten befinden. Eine Anwendung mit nur einem Mandanten hat nur einen Dienstprinzipal (im Home-Mandanten). Eine [Webanwendung](active-directory-dev-glossary.md#web-client) mit mehreren Mandanten hat ebenfalls diesen Dienstprinzipal und zusätzlich einen Dienstprinzipal in jedem Mandanten, für den der Anwendung die Zustimmung für den Zugriff auf die Ressourcen von einem Administrator oder Benutzern des Mandanten erteilt wurde. Nach der Erteilung der Zustimmung wird das Dienstprinzipalobjekt bei zukünftigen Autorisierungsanforderungen herangezogen.

> [AZURE.NOTE] Alle Änderungen, die Sie an Ihrem Anwendungsobjekt vornehmen, werden auch in seinem Dienstprinzipalobjekt ausschließlich im Home-Mandanten der Anwendung widergespiegelt (der Mandant, für den die Registrierung durchgeführt wurde). Wenn Ihre Anwendung für den mehrinstanzenfähigen Zugriff konfiguriert ist, werden Änderungen am Anwendungsobjekt erst dann in den Dienstprinzipalobjekten von Endkundenmandanten widergespiegelt, wenn der Endkundenmandant den Zugriff entfernt und wieder gewährt.

## Beispiel
Im folgenden Diagramm ist die Beziehung zwischen einem Anwendungsobjekt der Anwendung und den entsprechenden Dienstprinzipalobjekten im Kontext einer mehrinstanzenfähigen Beispielanwendung mit dem Namen **HR App** dargestellt. In diesem Szenario werden drei Azure AD-Mandanten verwendet:

- **Adatum**: Der Mandant, der von dem Unternehmen verwendet wird, das die **HR App** entwickelt hat.
- **Contoso**: Der Mandant, der von der Organisation Contoso (einem Endkunden der **HR App**) verwendet wird.
- **Fabrikam**: Der von der Organisation Fabrikam (ebenfalls ein Endkunde der **HR App**) verwendete Mandant.

![Beziehung zwischen einem Anwendungsobjekt und einem Dienstprinzipalobjekt](./media/active-directory-application-objects/application-objects-relationship.png)

Im obigen Diagramm ist Schritt 1 der Prozess zur Erstellung der Anwendungs- und Dienstprinzipalobjekte im Home-Mandanten der Anwendung.

Nachdem die Administratoren von Contoso und Fabrikam die Zustimmung erteilt und den Zugriff auf die Anwendung gewährt haben, wird in Schritt 2 im Azure AD-Mandanten des Unternehmens ein Dienstprinzipalobjekt erstellt und mit den Berechtigungen versehen, die vom Administrator des Unternehmens gewährt wurden. Beachten Sie auch, dass die HR App so konfiguriert bzw. entworfen werden kann, dass die Zustimmung für die individuelle Nutzung durch die Benutzer erteilt wird.

In Schritt 3 verfügen die Endkundenmandanten der HR App (z.B. Contoso und Fabrikam) jeweils über ein eigenes Dienstprinzipalobjekt, das deren Nutzung einer Instanz der Anwendung zur Laufzeit darstellt. Dies wird über die Berechtigungen gesteuert, die vom Administrator erteilt wurden.

## Nächste Schritte
Sie können auf das Anwendungsobjekt einer Anwendung über die Azure AD Graph-API zugreifen (wie von der OData-[Application-Entität](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference?branch=master#ApplicationEntity) dargestellt).

Sie können auf das Dienstprinzipalobjekt einer Anwendung über die Azure AD Graph-API zugreifen (wie von der OData-[ServicePrincipal-Entität](https://msdn.microsoft.com/library/azure/ad/graph/api/entity-and-complex-type-reference?branch=master#ServicePrincipalEntity) dargestellt).



<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#ApplicationEntity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipalentity
[AZURE-classic-portal]: https://manage.windowsazure.com

<!---HONumber=AcomDC_0803_2016-->
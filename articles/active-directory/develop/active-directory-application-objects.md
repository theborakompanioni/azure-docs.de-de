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
ms.date: 04/28/2016
ms.author: bryanla;mbaldwin
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: e155891ff8dc736e2f7de1b95f07ff7b2d5d4e1b
ms.openlocfilehash: 6ee0c0b5e606b1fd9fc9eecc877d2718b7079ecb
ms.contentlocale: de-de
ms.lasthandoff: 05/02/2017


---
# <a name="application-and-service-principal-objects-in-azure-active-directory-azure-ad"></a>Anwendungs- und Dienstprinzipalobjekte in Azure Active Directory (Azure AD)
In einigen Fällen kann der Begriff „Anwendung“ im Kontext von Azure AD falsch verstanden werden. Das Ziel dieses Artikels besteht darin, dies zu verdeutlichen. Hierzu werden die konzeptuellen und konkreten Aspekte der Azure AD-Anwendungsintegration veranschaulicht, gefolgt von einer Erläuterung der Registrierung und Zustimmung zu einer [mehrinstanzenfähigen Anwendung](active-directory-dev-glossary.md#multi-tenant-application).

## <a name="overview"></a>Übersicht
Eine Anwendung, die in Azure AD integriert ist, hat Auswirkungen über die Software hinaus. „Anwendung“ wird häufig als konzeptioneller Begriff verwendet und bezieht sich nicht nur auf die Anwendungssoftware, sondern auch auf die Azure AD-Registrierung und die Rolle bei „Konversationen“ zur Authentifizierung/Autorisierung zur Laufzeit. Der Definition nach kann eine Anwendung in einer [Client](active-directory-dev-glossary.md#client-application)rolle (Nutzung einer Ressource), einer [Ressourcenserver](active-directory-dev-glossary.md#resource-server)rolle (APIs werden für Clients verfügbar gemacht) oder beiden Rollen funktionieren. Das Konversationsprotokoll ist durch einen [Ablauf zur Erteilung einer OAuth 2.0-Autorisierung](active-directory-dev-glossary.md#authorization-grant) definiert und ermöglicht dem Client bzw. der Ressource das Zugreifen auf bzw. das Schützen von Daten einer Ressource. Nun gehen wir eine Ebene tiefer und sehen uns an, wie das Azure AD-Anwendungsmodell eine Anwendung beim Entwurf und zur Laufzeit darstellt. 

## <a name="application-registration"></a>Anwendungsregistrierung
Wenn Sie eine Azure AD-Anwendung im [Azure-Portal][AZURE-Portal] registrieren, werden im Azure AD-Mandanten zwei Objekte erstellt: ein Anwendungsobjekt und ein Dienstprinzipalobjekt.

#### <a name="application-object"></a>Anwendungsobjekt
Eine Azure AD-Anwendung ist durch ein eindeutiges Anwendungsobjekt definiert. Es befindet sich im Azure AD-Mandanten, in dem die Anwendung registriert ist und der als „Heimmandant“ der Anwendung bezeichnet wird. Die [Application-Entität][AAD-Graph-App-Entity] von Azure AD Graph definiert das Schema für die Eigenschaften eines Anwendungsobjekts. 

#### <a name="service-principal-object"></a>Dienstprinzipalobjekt
Das Dienstprinzipalobjekt definiert die Richtlinie und Berechtigungen für den Einsatz einer Anwendung in einem bestimmten Mandanten und stellt die Basis für einen Sicherheitsprinzipal bereit, der die Anwendung zur Laufzeit repräsentiert. Die [ServicePrincipal-Entität][AAD-Graph-Sp-Entity] von Azure AD Graph definiert das Schema für die Objekteigenschaften eines Dienstprinzipalobjekts. 

#### <a name="application-and-service-principal-relationship"></a>Beziehung zwischen Anwendung und Dienstprinzipal
Stellen Sie sich das Anwendungsobjekt als *globale* Darstellung Ihrer Anwendung zur Verwendung über alle Mandanten hinweg und den Dienstprinzipal als *lokale* Darstellung zur Verwendung in einem bestimmten Mandanten vor. Das Anwendungsobjekt fungiert als Vorlage, aus der allgemeine und Standardeigenschaften zur Verwendung im entsprechenden Dienstprinzipal *abgeleitet* werden. Für ein Anwendungsobjekt bestehen daher eine 1:1-Beziehung mit der Softwareanwendung und eine 1:viele-Beziehung mit den entsprechenden Dienstprinzipalobjekten.

Ein Dienstprinzipal muss in jedem Mandanten erstellt werden, in dem die Anwendung verwendet wird. Er ermöglicht, eine Identität für die Anmeldung und/oder den Zugriff auf vom Mandanten gesicherte Ressourcen einzurichten. Eine Einzelmandantenanwendung weist nur einen Dienstprinzipal (im eigenen Heimmandanten) auf, der normalerweise während der Anwendungsregistrierung erstellt und für die Verwendung autorisiert wird. Für eine Webanwendung/API mit mehreren Mandanten muss außerdem in jedem Mandanten ein Dienstprinzipal erstellt werden, in dem ein Benutzer aus diesem Mandanten der Verwendung zugestimmt hat.  

> [!NOTE]
> Alle Änderungen, die Sie an Ihrem Anwendungsobjekt vornehmen, werden auch in seinem Dienstprinzipalobjekt ausschließlich im Home-Mandanten der Anwendung widergespiegelt (der Mandant, für den die Registrierung durchgeführt wurde). Bei mehrinstanzenfähigen Anwendungen werden Änderungen am Anwendungsobjekt erst dann in den Dienstprinzipalobjekten von Endkundenmandanten übernommen, wenn der Zugriff über den [Anwendungszugriffsbereich](https://myapps.microsoft.com) entfernt und wieder gewährt wird.
><br>  
> Beachten Sie außerdem, dass native Anwendungen standardmäßig als mehrinstanzenfähig registriert werden.
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
Sie können auf das Anwendungsobjekt einer Anwendung über die Azure AD-Graph-API, den Anwendungsmanifest-Editor im [Azure-Portal][AZURE-Portal] oder [Azure AD-PowerShell-Cmdlets](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0) zugreifen, wie von der OData-[Application-Entität][AAD-Graph-App-Entity] dargestellt.

Sie können auf das Dienstprinzipalobjekt einer Anwendung über die Azure AD-Graph-API oder [Azure AD-PowerShell-Cmdlets](https://docs.microsoft.com/powershell/azure/overview?view=azureadps-2.0) zugreifen, wie von der OData-[ServicePrincipal-Entität][AAD-Graph-Sp-Entity] dargestellt.

Der [Azure AD-Graph-Tester](https://graphexplorer.azurewebsites.net/) eignet sich zum Abfragen der Anwendungs- und Dienstprinzipalobjekte.

<!--Image references-->

<!--Reference style links -->
[AAD-Graph-App-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#application-entity
[AAD-Graph-Sp-Entity]: https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/entity-and-complex-type-reference#serviceprincipal-entity
[AZURE-Portal]: https://portal.azure.com


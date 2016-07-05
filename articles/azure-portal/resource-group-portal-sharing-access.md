<properties
	pageTitle="Freigabe und Zugriffskontrolle für Azure-Dashboards | Microsoft Azure"
	description="Erfahren Sie, wie Sie die Freigabe und Zugriffskontrolle für Azure-Dashboards verwalten."
	services="azure-resource-manager,azure-portal"
    keywords="Portal, Freigabe, Zugriff"
	documentationCenter=""
	authors="adamabdelhamed"
	manager="dend"
	editor="dend"/>

<tags
	ms.service="azure-resource-manager"
	ms.workload="multiple"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="03/28/2016"
	ms.author="adamab"/>
    
# Freigabe und Zugriffskontrolle für Azure-Dashboards

Der Zugriff auf die Informationen, die von den meisten Kacheln im Portal angezeigt werden, wird in Azure über die [rollenbasierte Zugriffssteuerung](./active-directory/role-based-access-control-configure.md) geregelt. Um Dashboards nahtlos in das Ökosystem zu integrieren, werden alle veröffentlichten Dashboards als Azure-Ressourcen implementiert. Aus Sicht der Zugriffssteuerung unterscheiden sich Dashboards nicht von einem virtuellen Computer oder Speicherkonto.

Beispiel: Angenommen, Sie verfügen über ein Azure-Abonnement, und verschiedenen Mitgliedern Ihres Teams wurden die Rollen **Besitzer**, **Mitwirkender** oder**Leser** für das Abonnement zugewiesen. Benutzer, die Besitzer oder Mitwirkende sind, können Dashboards im Abonnement auflisten, anzeigen, erstellen, ändern oder löschen. Benutzer, die als Leser festgelegt wurden, können Dashboards auflisten und anzeigen, aber sie können diese nicht ändern oder löschen. Benutzer mit Leserzugriff können lokale Änderungen an einem veröffentlichten Dashboard vornehmen (z.B. beim Behandeln eines Problems), aber sie haben nicht die Möglichkeit, diese Änderungen zurück auf den Server zu veröffentlichen. Diese Benutzer können eine private Kopie des Dashboards zur eigenen Nutzung erstellen.

Beachten Sie, dass die einzelnen Kacheln im Dashboard jeweils eigene Anforderungen an die Zugriffssteuerung durchsetzen. Diese basieren auf den Ressourcen, für die Daten angezeigt werden. Dies bedeutet, dass Sie ein Dashboard entwerfen können, das umfassender genutzt werden kann, während die Daten auf den einzelnen Kacheln trotzdem geschützt sind.

<!---HONumber=AcomDC_0622_2016-->
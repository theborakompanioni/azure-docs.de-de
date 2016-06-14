<properties
	pageTitle="Übersicht über das Azure AD .NET-Protokoll | Microsoft Azure"
	description="Es wird beschrieben, wie Sie HTTP-Nachrichten zum Autorisieren des Zugriffs auf Webanwendungen und Web-APIs in Ihrem Mandanten mit Azure AD verwenden."
	services="active-directory"
	documentationCenter=".net"
	authors="priyamohanram"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="dotnet"
	ms.topic="article"
	ms.date="01/21/2016"
	ms.author="priyamo"/>

<!--TODO: Introduction -->

## Registrieren der Anwendung beim AD-Mandanten

Zuerst müssen Sie Ihre Anwendung beim Active Directory-Mandanten registrieren. Hierbei erhalten Sie eine Client-ID für die Anwendung, und die Aktivierung für den Empfang von Token wird durchgeführt.

- Melden Sie sich beim Azure-Verwaltungsportal an.

- Klicken Sie im linken Navigationsbereich auf **Active Directory**.

- Wählen Sie den Mandanten aus, unter dem die Anwendung registriert werden soll.

- Klicken Sie auf die Registerkarte **Anwendungen** und dann unten auf **Hinzufügen**.

- Folgen Sie der Anleitung, und erstellen Sie eine neue Anwendung. Es spielt bei diesem Tutorial keine Rolle, ob es sich um eine Webanwendung oder eine native Anwendung handelt. Falls Sie spezielle Beispiele für Webanwendungen oder native Anwendungen vorziehen, können Sie sich [hier](../articles/active-directory/active-directory-developers-guide.md) unsere „Schnellstarts“ ansehen.

- Geben Sie für Webanwendungen die **Anmelde-URL** an. Dies ist die Basis-URL Ihrer App, unter der sich Benutzer anmelden können, z.B. `http://localhost:12345`. Die **App-ID-URI** ist eine eindeutige Kennung für die Anwendung. Üblicherweise wird `https://<tenant-domain>/<app-name>` verwendet, zum Beispiel: `https://contoso.onmicrosoft.com/my-first-aad-app`

- Geben Sie für native Anwendungen einen **Umleitungs-URI** an, der von Azure AD zum Zurückgeben von Tokenantworten verwendet wird. Geben Sie einen für Ihre Anwendung spezifischen Wert ein, z.B. `http://MyFirstAADApp`.

- Nach Abschluss der Registrierung weist AAD Ihrer Anwendung eine eindeutige Client-ID zu. Sie benötigen diesen Wert in den nächsten Abschnitten. Kopieren Sie ihn daher in der Anwendung auf der Registerkarte **Konfigurieren**.

<!---HONumber=AcomDC_0601_2016-->
<properties
	pageTitle="Azure Active Directory B2C: Registrierung einer Anwendung | Microsoft Azure"
	description="Registrieren Ihrer Anwendung bei Azure Active Directory B2C"
	services="active-directory-b2c"
	documentationCenter=""
	authors="swkrish"
	manager="mbaldwin"
	editor="bryanla"/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="get-started-article"
	ms.date="07/22/2016"
	ms.author="swkrish"/>


# Azure Active Directory B2C: Registrieren der Anwendung

## Voraussetzung

Zum Erstellen einer Anwendung, die Registrierungen und Anmeldungen von Kunden akzeptiert, müssen Sie die Anwendung zunächst bei einem Azure Active Directory B2C-Mandanten registrieren. Erstellen Sie einen eigenen Mandanten mithilfe der unter [Erstellen eines Azure AD B2C-Mandanten](active-directory-b2c-get-started.md) beschriebenen Schritte. Nachdem Sie alle Schritte in diesem Artikel ausgeführt haben, ist das Blatt „B2C-Funktionen“ an Ihr Startmenü angeheftet.

[AZURE.INCLUDE [active-directory-b2c-devquickstarts-v2-apps](../../includes/active-directory-b2c-devquickstarts-v2-apps.md)]

## Navigieren zum Blatt „B2C-Funktionen“

Wenn das Blatt „B2C-Funktionen“ an Ihr Startmenü angeheftet ist, sehen Sie das Blatt, sobald Sie sich als globaler Administrator des B2C-Mandanten beim [Azure-Portal](https://portal.azure.com/) anmelden.

Sie können auf das Blatt auch zugreifen, indem Sie im [Azure-Portal](https://portal.azure.com/) im linken Navigationsbereich auf **Durchsuchen** und dann auf **Azure AD B2C** klicken.

> [AZURE.IMPORTANT] Sie müssen als globaler Administrator des B2C-Mandanten festgelegt sein, um auf das Blade mit den B2C-Features zugreifen zu können. Globale Administratoren anderer Mandanten oder Benutzer von Mandanten haben keinen Zugriff. Mit dem Mandantenumschalter in der oberen rechten Ecke des Azure-Portals können Sie zu Ihrem B2C-Mandanten wechseln.

## Registrieren einer Anwendung

1. Klicken Sie auf dem Blatt „B2C-Funktionen“ im Azure-Portal auf **Anwendungen**.
2. Klicken Sie oben auf dem Blatt auf **+Hinzufügen**.
3. Geben Sie einen Namen für die Anwendung ein, der die Funktion der Anwendung für Kunden beschreibt. Sie könnten z. B. „Contoso B2C-App“ eingeben.
4. Wenn Sie eine webbasierte Anwendung entwickeln, stellen Sie den Schalter **Web-App/Web-API einschließen** auf **Ja**. Die **Antwort-URLs** sind Endpunkte, an denen Azure AD B2C von Ihrer Anwendung angeforderte Token zurückgibt. Geben Sie z. B. Folgendes ein: `https://localhost:44321/`. Falls Ihre Webanwendung auch eine durch Azure AD B2C geschützte Web-API aufruft, wird darüber hinaus die Erstellung eines **geheimen Anwendungsschlüssels** empfohlen. Klicken Sie dazu auf die Schaltfläche **Schlüssel generieren**.
5. Wenn Sie eine mobile Anwendung entwickeln, stellen Sie den Schalter **Systemeigenen Client einschließen** auf **Ja**. Notieren Sie sich den standardmäßigen **Umleitungs-URI**, der automatisch für Sie erstellt wurde.
6. Klicken Sie auf **Erstellen**, um Ihre Anwendung zu registrieren.
7. Klicken Sie auf die soeben erstellte Anwendung, und notieren Sie sich die global eindeutige **Anwendungsclient-ID**, die Sie später in Ihrem Code verwenden.

> [AZURE.NOTE] **Geheime Anwendungsschlüssel** sind wichtige Sicherheitsanmeldeinformationen, die entsprechend geschützt werden sollten.

## Erstellen einer Schnellstart-App

Nachdem Sie nun über eine bei Azure AD B2C registrierte Anwendung verfügen, können Sie zum Einstieg eines der Schnellstarttutorials ausführen. Hier sind einige Vorschläge:

[AZURE.INCLUDE [active-directory-v2-quickstart-table](../../includes/active-directory-b2c-quickstart-table.md)]

<!---HONumber=AcomDC_0727_2016-->
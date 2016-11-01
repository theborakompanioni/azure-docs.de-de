<properties
	pageTitle="Azure Active Directory B2C: Erstellen eines Azure Active Directory B2C-Mandanten | Microsoft Azure"
	description="Thema zum Erstellen eines Azure Active Directory B2C-Mandanten"
	services="active-directory-b2c"
	documentationCenter=""
	authors="swkrish"
	manager="msmbaldwin"
	editor="bryanla"/>

<tags
	ms.service="active-directory-b2c"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.topic="article"
    ms.devlang="na"
	ms.date="08/30/2016"
	ms.author="swkrish"/>

# Azure Active Directory B2C: Erstellen eines Azure AD B2C-Mandanten

Zum Einstieg in Microsoft Azure Active Directory (Azure AD) B2C führen Sie die drei Schritte aus, die in diesem Artikel beschrieben werden.

## Schritt 1: Registrieren für ein Azure-Abonnement

Wenn Sie bereits über ein Azure-Abonnement verfügen, überspringen Sie diesen Schritt. Andernfalls registrieren Sie sich für ein [Azure-Abonnement](../active-directory/sign-up-organization.md), um Zugriff auf Azure AD B2C zu erhalten.

## Schritt 2: Erstellen eines Azure AD B2C-Mandanten

Führen Sie die folgenden Schritte aus, um einen neuen Azure AD B2C-Mandanten zu erstellen. Derzeit können B2C-Features in Ihren vorhandenen Mandanten noch nicht aktiviert werden.

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com/) als Abonnementadministrator an. Dies ist dasselbe Geschäfts-, Schul- oder Unikonto bzw. dasselbe Microsoft-Konto, mit dem Sie sich bei Azure registriert haben.
2. Klicken Sie auf **Neu** > **App Services** > **Active Directory** > **Verzeichnis** > **Benutzerdefiniert erstellen**.

    ![Screenshot der ersten Schritte zum Erstellen eines Mandanten](./media/active-directory-b2c-get-started/new-directory.png)

3. Wählen Sie **Name**, **Domänenname** und **Land oder Region** für den Mandanten aus.
4. Aktivieren Sie die Option **Hierbei handelt es sich um ein B2C-Verzeichnis**.
5. Klicken Sie auf das Häkchen, um die Aktion abzuschließen.

    ![Screenshot des Kontrollkästchens zum Erstellen eines B2C-Verzeichnisses](./media/active-directory-b2c-get-started/create-b2c-directory.png)

6. Der Mandant wird jetzt erstellt und in der Active Directory-Erweiterung aufgeführt. Außerdem werden Sie als globaler Administrator des Mandanten festgelegt. Sie können bei Bedarf weitere globale Administratoren hinzufügen.

    > [AZURE.IMPORTANT]
	Wenn Sie einen B2C-Mandanten für eine Produktions-App verwenden möchten, lesen Sie den Artikel zum [Vergleich zwischen Produktionsmandanten und B2C-Vorschaumandanten](active-directory-b2c-reference-tenant-type.md). Beachten Sie, dass beim Löschen eines vorhandenen B2C-Mandanten und erneuten Erstellen mit demselben Domänennamen bekannte Probleme auftreten. Sie müssen einen B2C-Mandanten mit einem anderen Domänennamen erstellen.

## Schritt 3: Navigieren zum Blatt „B2C-Funktionen“ im Azure-Portal

1. Navigieren Sie auf der Navigationsleiste links zur Active Directory-Erweiterung.
2. Suchen Sie Ihren Mandanten auf der Registerkarte **Verzeichnis**, und klicken Sie darauf.
3. Klicken Sie auf die Registerkarte **Konfigurieren**.
4. Klicken Sie im Abschnitt **B2C-Verwaltung** auf den Link **B2C-Einstellungen verwalten**.

    ![Screenshot der Verzeichniskonfiguration für B2C](./media/active-directory-b2c-get-started/b2c-directory-configure-tab.png)

5. Das Azure-Portal mit dem Blatt „B2C-Funktionen“ wird auf einer neuen Registerkarte oder in einem neuen Fenster des Browsers geöffnet.

    > [AZURE.IMPORTANT]
    Sie können im Azure-Portal erst nach etwa zwei bis drei Minuten auf den Mandanten zugreifen. Sollte ein Problem auftreten, warten Sie etwas, und führen Sie die Schritte erneut aus. Wenn dies nicht der Fall ist, wenden Sie sich an den Support.

6. Heften Sie dieses Blatt an Ihr Startmenü an, um auf einfache Weise darauf zugreifen zu können. (Das Tool zum Anheften ist oben rechts auf dem Featureblatt rot gekennzeichnet.)

    ![Screenshot des Blatts „B2C-Funktionen“](./media/active-directory-b2c-get-started/b2c-features-blade.png)

    > [AZURE.NOTE]
    Im [klassischen Azure-Portal](https://manage.windowsazure.com/) können Sie Benutzer und Gruppen, die Konfiguration für die Self-Service-Kennwortzurücksetzung und die Features des Unternehmensbrandings für Ihren Mandanten verwalten.

## Einfacher Zugriff auf das Blatt „B2C-Funktionen“ im Azure-Portal

Um die Auffindbarkeit zu verbessern, haben wir eine Verknüpfung zum Blatt „B2C-Funktionen“ im Azure-Portal hinzugefügt.

1. Melden Sie sich als globaler Administrator Ihres B2C-Mandanten im Azure-Portal an. Wenn Sie bereits bei einem anderen Mandanten angemeldet sind, wechseln Sie die Mandanten (in der oberen rechten Ecke).
2. Klicken Sie im linken Navigationsbereich auf **Durchsuchen**.
3. Klicken Sie auf **Azure AD B2C**, um auf das Blatt „B2C-Funktionen“ zuzugreifen.

    ![Screenshot des Blatts „Navigieren zu B2C-Funktionen“](./media/active-directory-b2c-get-started/b2c-browse.png)

## Nächste Schritte

Informationen zum Registrieren einer Anwendung in Azure AD B2C und zum Erstellen einer Schnellstartanwendung finden Sie unter [Azure Active Directory B2C: Registrieren der Anwendung](active-directory-b2c-app-registration.md).

<!---HONumber=AcomDC_0831_2016-->
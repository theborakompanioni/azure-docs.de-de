<properties
	pageTitle="Ich kann mich nicht anmelden, um mein Azure-Abonnement zu verwalten | Microsoft Azure"
	description="Informationen zur Problembehandlung bei einigen häufig auftretenden Anmeldeproblemen bei Azure-Abonnements"
	services=""
	documentationCenter=""
	authors="genlin"
	manager="msmbaldwin"
	editor=""
	tags="billing"
	/>

<tags
	ms.service="billing"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/17/2016"
	ms.author="genli"/>

# Ich kann mich nicht anmelden, um mein Azure-Abonnement zu verwalten

> [AZURE.NOTE] Wenn Sie bei irgendeinem Verfahren in diesem Artikel weitere Hilfe benötigen, [wenden Sie sich an den Support](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409), um das Problem schnell zu lösen.

Dieser Artikel hilft Ihnen bei der Problembehandlung einiger häufiger Ursachen bei Anmeldeproblemen.

## Auf welches Portal möchten Sie zugreifen?

Ein Kontoadministrator kann nur auf das [Kontocenter](https://account.windowsazure.com/) zugreifen, während Dienstadministratoren und Co-Administratoren nur Zugriff auf das [Azure-Portal](https://portal.azure.com) oder das [klassische Azure-Portal](https://manage.windowsazure.com/) haben.

Weitere Informationen zu den Azure-Administratorrollen finden Sie unter [Hinzufügen oder Ändern von Azure-Administratorrollen](billing-add-change-azure-subscription-administrator.md).

## Ist Ihr Abonnement mit einem Microsoft-Konto oder Organisationskonto verknüpft?

Ihr Microsoft-Konto ist die E-Mail-Adresse, die Sie zusammen mit Ihrem Kennwort verwenden, um sich bei Windows Live-Programmen oder -Diensten anzumelden, z. B. bei Outlook, Hotmail, MSN oder OneDrive. Sie können ein Microsoft-Konto mit einer beliebigen E-Mail-Adresse einrichten, z. B. auch mit Ihrer Firmen-E-Mail-Adresse. Weitere Informationen finden Sie unter [www.microsoft.com/account](http://www.microsoft.com/account).

Wenn Ihr Konto mit einem Organisationskonto verknüpft ist, wählen Sie die richtige Anmeldeoption aus (siehe Abbildung unten). Weitere Informationen zur Verwendung eines Organisationskontos finden Sie unter [Als Unternehmen für Azure registrieren](./active-directory/sign-up-organization.md).

![Anmeldeseite](./media/billing-cannot-login-subscription/signin.png)

## Co-Administrator: Verwenden Sie den richtigen Kontotyp zum Verwalten anderer Konten?

- Wenn Sie mit einem Microsoft-Konto angemeldet sind, können Sie nur andere Microsoft-Konten als Co-Administratoren hinzufügen. Dies ist eine Sicherheitsanforderung und verhindert, dass nicht zu einer Organisation gehörende Konten erkennen, ob bestimmte Konten (z. B. janedoe@contoso.com) gültig sind.
- Wenn Sie mit einem Organisationskonto angemeldet sind, können Sie andere Organisationskonten in Ihrer Organisation als Co-Administratoren hinzufügen. Beispielsweise kann abby@contoso.com das Konto bob@contoso.com als Dienstadministrator oder Co-Administrator hinzufügen, nicht jedoch john@notcontoso.com. Mit einem Organisationskonto angemeldete Benutzer können zudem Benutzer mit Microsoft-Konten als Dienstadministrator oder Co-Administrator hinzufügen.

Nachdem es jetzt möglich ist, sich bei Azure mit einem Organisationskonto anzumelden, ändern sich die folgenden Kontoanforderungen für Dienstadministratoren und Co-Administratoren:

| Anmeldemethode| Microsoft-Konto als Co-Administrator oder Dienstadministrator hinzufügen? |Organisationskonto in der gleichen Organisation als Co-Administrator oder Dienstadministrator hinzufügen? |Organisationskonto in einer anderen Organisation als Co-Administrator oder Dienstadministrator hinzufügen?
| ------------- | ------------- |---------------|---------------|
|Microsoft Account |Ja|Nein|Nein|
|Organisationskonto|Ja|Ja|Nein|

## Gibt es ein Problem mit der Internetbrowser?

Versuchen Sie, den Cache und Cookies zu löschen, den Internet Explorer-Modus „InPrivate-Browsen“ zu verwenden und einen anderen Browser zu nutzen.

> [AZURE.NOTE] Wenn Sie weitere Fragen haben, [wenden Sie sich an den Support](http://go.microsoft.com/fwlink/?linkid=544831&clcid=0x409), um das Problem schnell zu lösen.

<!---HONumber=AcomDC_0824_2016-->
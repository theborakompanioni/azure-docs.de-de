<properties
	pageTitle="Azure AD Connect: Synchronisierungsdienstinstanzen | Microsoft Azure"
	description="Diese Seite beschreibt besondere Überlegungen für Azure AD-Instanzen."
	services="active-directory"
	documentationCenter=""
	authors="andkjell"
	manager="stevenpo"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="06/27/2016"
	ms.author="andkjell"/>

# Azure AD Connect: Besondere Überlegungen zu Instanzen
Azure AD Connect wird am häufigsten mit der weltweiten Instanz von Azure AD und Office 365 verwendet. Es gibt jedoch noch weitere Instanzen, und für diese gelten andere Anforderungen an URLs sowie weitere besondere Überlegungen.

## Microsoft Cloud Deutschland
Die [Microsoft Cloud Deutschland](http://www.microsoft.de/cloud-deutschland) ist eine unabhängige Cloud, die von einem deutschen Datentreuhänder betrieben wird.

Diese Cloud befindet sich derzeit in der Vorschauphase. Viele Tätigkeiten, die Sie normalerweise selbst durchführen können, wie beispielsweise das Überprüfen von Domänen, müssen vom Betreiber ausgeführt werden. Wenden Sie sich an Ihren lokalen Microsoft-Vertriebsmitarbeiter, um Informationen zur Teilnahme an der Vorschau zu erhalten.

URLs, die im Proxyserver geöffnet werden müssen |
--- |
*.microsoftonline.de |
*.windows.net |
\+Zertifikatsperrlisten |

Wenn Sie sich bei Ihrem Azure AD-Verzeichnis anmelden, müssen Sie ein Konto in der Domäne onmicrosoft.com verwenden.

In der Microsoft Cloud Deutschland derzeit nicht enthaltene Features:

- Azure AD Connect Health ist nicht verfügbar.
- Automatische Updates sind nicht verfügbar.
- Kennwortrückschreiben ist nicht verfügbar.

## Microsoft Azure Government-Cloud
Die [Microsoft Azure Government-Cloud](https://azure.microsoft.com/features/gov/) ist eine Cloud für US-Regierungsbehörden.

Diese Cloud wurde von früheren Versionen von DirSync unterstützt. Ab Build 1.1.180 von Azure AD Connect wird die nächste Generation der Cloud unterstützt. Diese Generation verwendet ausschließlich in den USA basierte Endpunkte und weist eine andere Liste von URLs auf, die Sie in Ihrem Proxyserver öffnen müssen.

URLs, die im Proxyserver geöffnet werden müssen |
--- |
*.microsoftonline.com |
*.gov.us.microsoftonline.com |
\+Zertifikatsperrlisten |

Azure AD Connect kann nicht automatisch erkennen, dass sich Ihr Azure AD-Verzeichnis in der Government-Cloud befindet. Sie müssen daher folgende Aktionen ausführen, wenn Sie Azure AD Connect installieren.

1. Starten Sie die Azure AD Connect-Installation.
2. Wenn die erste Seite angezeigt wird, auf der Sie aufgefordert werden, den Endbenutzer-Lizenzvertrag zu akzeptieren, fahren Sie nicht fort, sondern lassen den Installations-Assistenten offen.
3. Starten Sie den Registrierungs-Editor, und ändern Sie den Registrierungsschlüssel `HKLM\SOFTWARE\Microsoft\Azure AD Connect\AzureInstance` in den Wert `2`.
4. Wechseln Sie zurück zum Azure AD Connect-Installations-Assistenten, akzeptieren Sie den Endbenutzer-Lizenzvertrag, und fahren Sie fort. Stellen Sie sicher, dass bei der Installation der Installationspfad zur **benutzerdefinierten Konfiguration** verwendet wird (nicht die Expressinstallation). Setzen Sie die Installation dann wie gewohnt fort.

In der Microsoft Azure Government-Cloud derzeit nicht enthaltene Features:

- Azure AD Connect Health ist nicht verfügbar.
- Automatische Updates sind nicht verfügbar.
- Kennwortrückschreiben ist nicht verfügbar.

## Nächste Schritte
Weitere Informationen zum [Integrieren lokaler Identitäten in Azure Active Directory](active-directory-aadconnect.md).

<!---HONumber=AcomDC_0629_2016-->
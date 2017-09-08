---
title: "Azure MFA-Versionen und Verbrauchspläne | Microsoft-Dokumentation"
description: "Informationen über den Client für die Multi-Factor Authentication sowie die verschiedenen verfügbaren Methoden und Versionen Details zu jedem Verbrauchsplan"
keywords: 
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2017
ms.author: kgremban
ms.translationtype: HT
ms.sourcegitcommit: a0b98d400db31e9bb85611b3029616cc7b2b4b3f
ms.openlocfilehash: c47ca818bd77291338de6e90e5dd27465475e868
ms.contentlocale: de-de
ms.lasthandoff: 08/29/2017

---
# <a name="how-to-get-azure-multi-factor-authentication"></a>Beziehen von Azure Multi-Factor Authentication

Zum Schutz Ihrer Konten sollte die Überprüfung in zwei Schritten in Ihrem Unternehmen Standard sein. Dieses Feature ist insbesondere für Administratorkonten wichtig, die privilegierten Zugriff auf Ressourcen haben. Aus diesem Grund bietet Microsoft grundlegende Features zur Überprüfung in zwei Schritten für Office 365- und Azure-Administratoren ohne Zusatzkosten. Wenn Sie für die Features für Ihre Administratoren ein Upgrade durchführen oder die Überprüfung in zwei Schritten auf den Rest Ihrer Benutzer ausweiten möchten, können Sie Azure Multi-Factor Authentication erwerben. 

Dieser Artikel erläutert den Unterschied zwischen den Versionen für Administratoren und der vollständigen Azure MFA-Version. Wenn Sie das vollständige Azure MFA-Angebot bereitstellen möchten, finden Sie in den späteren Abschnitten Informationen dazu, welche Implementierungsoptionen es gibt und wie Microsoft den Verbrauch berechnet.

>[!IMPORTANT]
>Dieser Artikel bietet einen Überblick über die verschiedenen Möglichkeiten zum Erwerb der Azure Multi-Factor Authentication. Ausführliche Informationen zur Preisgestaltung und Abrechnung finden Sie auf der Seite [Multi-Factor Authentication Preise ](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

## <a name="available-versions-of-azure-multi-factor-authentication"></a>Verfügbare Versionen von Azure Multi-Factor Authentication

Die folgende Tabelle beschreibt die Unterschiede zwischen den drei Versionen der Multi-Factor Authentication:

| Version | Beschreibung |
| --- | --- |
| Multi-Factor Authentication für Office 365 |Diese Version arbeitet ausschließlich mit Office 365-Anwendungen und wird über das Office 365-Portal verwaltet. Administratoren können [ihre Office 365-Ressourcen mit der Überprüfung in zwei Schritten sichern](https://support.office.com/article/Set-up-multi-factor-authentication-for-Office-365-users-8f0454b2-f51a-4d9c-bcde-2c48e41621c6). Diese Version ist Teil eines Office 365-Abonnements. |
| Multi-Factor Authentication für Azure-Administratoren | Globale Administratoren von Azure-Mandanten können die Überprüfung in zwei Schritten für ihre globalen Administratorkonten ohne zusätzliche Kosten aktivieren.|
| Azure Multi-Factor Authentication | Azure Multi-Factor Authentication wird häufig auch als „Vollversion“ bezeichnet und bietet von allen Versionen den größten Funktionsumfang. Über das [klassische Azure-Portal](https://manage.windowsazure.com) sind zusätzliche Konfigurationsoptionen verfügbar. Außerdem stehen erweiterte Funktionen für die Berichterstellung zur Verfügung, und es werden verschiedene lokale Anwendungen und Cloudanwendungen unterstützt. Azure Multi-Factor Authentication ist im Lieferumfang der Tarife [Azure Active Directory Premium](https://www.microsoft.com/cloud-platform/azure-active-directory-features) und [Enterprise Mobility + Security](https://www.microsoft.com/cloud-platform/enterprise-mobility-security-pricing) enthalten und kann in der Cloud oder lokal bereitgestellt werden. |

## <a name="feature-comparison-of-versions"></a>Funktionsvergleich der Versionen
In der folgenden Tabelle werden die Features aufgeführt, die in den verschiedenen Versionen von Azure Multi-Factor Authentication verfügbar sind:

> [!NOTE]
> In dieser Vergleichstabelle werden die Features erläutert, die in den einzelnen Versionen von Multi-Factor Authentication enthalten sind. Wenn Sie über die Vollversion des Multi-Factor Authentication-Diensts verfügen, sind einige Features möglicherweise nicht verfügbar (je nachdem, ob Sie [MFA in der Cloud oder lokal](multi-factor-authentication-get-started.md) verwenden).


| Funktion | Multi-Factor Authentication für Office 365 | Multi-Factor Authentication für Azure-Administratoren | Azure Multi-Factor Authentication |
| --- |:---:|:---:|:---:|
| Schutz von Administratorkonten mit MFA |● |● (Gilt nur für globale Administratorkonten) |● |
| Mobile App als zweiter Faktor |● |● |● |
| Telefonanruf als zweiter Faktor |● |● |● |
| SMS als zweiter Faktor |● |● |● |
| App-Kennwörter für Clients, die MFA nicht unterstützen |● |● |● |
| Administrative Kontrolle über Überprüfungsmethoden |● |● |● |
| PIN-Modus | | |● |
| Betrugswarnung | | |● |
| MFA-Berichte | | |● |
| Einmalumgehung | | |● |
| Benutzerdefinierte Begrüßungen für Telefonanrufe | | |● |
| Benutzerdefinierte Anrufer-ID für Telefonanrufe | | |● |
| Vertrauenswürdige IP-Adressen | | |● |
| Speichern der MFA für vertrauenswürdige Geräte |● |● |● |
| MFA-SDK | | |● (erfordert einen Multi-Factor Authentication-Anbieter und ein vollständiges Azure-Abonnement) |
| MFA für lokale Anwendungen | | |● |
| Richtlinien für bedingten Zugriff | | | ● |

## <a name="how-to-get-azure-multi-factor-authentication"></a>Beziehen von Azure Multi-Factor Authentication
Wenn Sie alle Funktionen von Azure Multi-Factor Authentication nutzen möchten, stehen mehrere Optionen zur Verfügung:

### <a name="option-1---mfa-licenses"></a>Option 1 – MFA-Lizenzen

Erwerben Sie Multi-Factor Authentication-Lizenzen, und weisen Sie diese Ihren Benutzern in Azure Active Directory zu. 

Erstellen Sie bei dieser Option nur dann einen Azure Multi-Factor Authentication-Anbieter, wenn Sie für einige Benutzer, die über keine Lizenzen verfügen, die Überprüfung in zwei Schritten bereitstellen müssen. Andernfalls erfolgt die Abrechnung unter Umständen zweimal.

### <a name="option-2---bundled-licenses-that-include-mfa"></a>Option 2 – Gebündelte Lizenzen, die MFA umfassen

Kaufen Sie Lizenzen, in denen Azure Multi-Factor Authentication enthalten ist, und weisen Sie sie Ihren Benutzern in Azure Active Directory zu. Beispiele wären etwa Azure Active Directory Premium oder Enterprise Mobility + Security. 

Erstellen Sie bei dieser Option nur dann einen Azure Multi-Factor Authentication-Anbieter, wenn Sie für einige Benutzer, die über keine Lizenzen verfügen, die Überprüfung in zwei Schritten bereitstellen müssen. Andernfalls erfolgt die Abrechnung unter Umständen zweimal. 

### <a name="option-3---mfa-consumption-based-model"></a>Option 3 – Verbrauchsbasiertes MFA-Modell

Erstellen Sie einen Azure Multi-Factor Authentication-Anbieter in einem Azure-Abonnement. Azure MFA-Anbieter sind Azure-Ressourcen, die wie alle anderen Azure-Ressourcen über Ihr Enterprise Agreement, Ihre Azure-Zahlungsverpflichtung oder Ihre Kreditkarte abgerechnet werden. Diese Anbieter können nur in vollständigen Azure-Abonnements erstellt werden und nicht in beschränkten Azure-Abonnements, für die ein Ausgabenlimit von 0 US-Dollar gilt. Beschränkte Abonnements werden beim Aktivieren von Lizenzen erstellt, wie etwa in den Optionen 1 und 2. 

Bei Verwendung eines Azure Multi-Factor Authentication-Anbieters stehen zwei Nutzungsmodelle zur Verfügung, die über Ihr Azure-Abonnement abgerechnet werden:  

1. **Pro Benutzer** – Diese Option ist für Unternehmen geeignet, die die Überprüfung in zwei Schritten für eine feste Anzahl von Mitarbeitern aktivieren möchten, welche die Authentifizierung regelmäßig benötigen. Die Abrechnung pro Benutzer basiert auf der Anzahl der Benutzer in Ihrem Azure AD-Mandanten und auf Ihrem Azure MFA-Server, für die MFA aktiviert ist. Wenn MFA für Benutzer sowohl in Azure AD als auch auf dem Azure MFA-Server aktiviert ist und die Domänensynchronisierung (Azure AD Connect) ebenfalls aktiviert ist, berechnen wir die größere Benutzergruppe. Wenn die Domänensynchronisierung nicht aktiviert ist, berechnen wir die Summe aller Benutzer, für die MFA in Azure AD und auf dem Azure MFA-Server aktiviert ist. Die Abrechnung erfolgt anteilsmäßig und wird täglich an das Commerce-System berichtet. 

  > [!NOTE]
  > Abrechnungsbeispiel 1: Für MFA sind heute 5.000 Benutzer aktiviert. Das MFA-System teilt diese Zahl durch 31 und meldet 161,29 Benutzer für diesen Tag. Morgen aktivieren Sie 15 weitere Benutzer, sodass das MFA-System für diesen Tag 161,77 Benutzer meldet. Am Ende des Abrechnungszyklus beträgt die Gesamtzahl der Benutzer, die im Rahmen Ihres Azure-Abonnements abgerechnet werden, 5.000. 
  >
  > Abrechnungsbeispiel 2: Sie haben sowohl Benutzer mit als auch ohne Lizenz. Für die Differenz verwenden Sie einen Azure MFA-Anbieter für eine Abrechnung pro Benutzer. In Ihrem Mandanten befinden sich 4.500 Enterprise Mobility + Security-Lizenzen, für MFA sind jedoch 5.000 Benutzer aktiviert. Für Ihr Azure-Abonnement werden 500 Benutzer abgerechnet, die täglich anteilsmäßig als 16,13 Benutzer gemeldet werden. 

2. **Pro Authentifizierung** – Diese Option ist für Unternehmen geeignet, die die Überprüfung in zwei Schritten für eine große Benutzergruppe aktivieren möchten, welche die Authentifizierung unregelmäßig benötigt. Die Abrechnung basiert auf der Anzahl der Anforderungen für die Überprüfung in zwei Schritten, unabhängig davon, ob diese Überprüfungen erfolgreich waren oder abgelehnt wurden. Diese Abrechnung erscheint auf Ihrer Azure-Nutzungsaufstellung in Paketen mit 10 Authentifizierungen und wird täglich gemeldet. 

  > [!NOTE]
  > Abrechnungsbeispiel 3: Der Azure MFA-Dienst hat heute 3.105 Anforderungen für die Überprüfung in zwei Schritten empfangen. Ihrem Azure-Abonnement werden heute 310,5 Authentifizierungspakete berechnet. 

Beachten Sie, dass Sie Azure MFA-Lizenzen haben können, die verbrauchsbasierte Konfiguration aber weiterhin berechnet wird. Wenn Sie einen Azure MFA-Anbieter für die Abrechnung pro Authentifizierung einrichten, wird jede Anforderung für die Überprüfung in zwei Schritten abgerechnet, auch für Benutzer, die über Lizenzen verfügen. Wenn Sie einen Azure MFA-Anbieter für die Abrechnung pro Benutzer einrichten, der nicht mit Ihrem Azure AD-Mandanten verknüpft ist, erfolgt die Abrechnung pro Benutzer, auch wenn Ihre Benutzer in Azure AD über Lizenzen verfügen. 

## <a name="next-steps"></a>Nächste Schritte

- Weitere Preisinformationen finden Sie unter [Preise für Azure MFA](https://azure.microsoft.com/pricing/details/multi-factor-authentication/).

- Wählen Sie, ob Azure MFA [in der Cloud oder lokal](multi-factor-authentication-get-started.md) bereitgestellt werden soll.

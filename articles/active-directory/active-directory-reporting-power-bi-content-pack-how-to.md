---
title: Verwenden des Azure Active Directory-Power BI-Inhaltspakets | Microsoft-Dokumentation
description: Es wird beschrieben, wie Sie das Azure Active Directory-Power BI-Inhaltspaket verwenden.
services: active-directory
author: MarkusVi
manager: femila
ms.assetid: addd60fe-d5ac-4b8b-983c-0736c80ace02
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/15/2017
ms.author: markvi
ms.reviewer: dhanyahk
ms.translationtype: Human Translation
ms.sourcegitcommit: 61fd58063063d69e891d294e627ae40cb878d65b
ms.openlocfilehash: ab95f61e2d88f82bf2b24aea786c7dbadfb9c0fe
ms.contentlocale: de-de
ms.lasthandoff: 06/23/2017

---
# Verwenden des Azure Active Directory-Power BI-Inhaltspakets
<a id="how-to-use-the-azure-active-directory-power-bi-content-pack" class="xliff"></a>

Das Verständnis, wie Ihre Benutzer Azure Active Directory-Funktionen annehmen und nutzen, ist für Sie als IT-Administrator sehr wichtig. Mit diesem Wissen können Sie Ihre IT-Infrastruktur und die Kommunikation planen, um die Nutzung zu erhöhen und in den Genuss möglichst vieler Vorteile der AAD-Funktionen zu kommen. Mit dem Power BI-Inhaltspaket für Azure Active Directory können Sie Ihre Daten weiter analysieren, um zu verstehen, wie Sie mithilfe dieser Daten umfassendere Einblicke in Azure Active Directory für die verschiedenen Funktionen gewinnen können, die für Sie wichtig sind.  Mit der Integration von Azure Active Directory-APIs in Power BI können Sie die vorab erstellten Inhaltspakete leicht herunterladen und Einblicke in alle Aktivitäten Ihrer Azure Active Directory-Instanz gewinnen, indem Sie die umfassende Visualisierungsoberfläche von Power BI nutzen. Sie können Ihr eigenes Dashboard erstellen und es leicht für alle anderen Benutzer Ihrer Organisation freigeben. 

Dieses Thema enthält die Schritt-für-Schritt-Anleitung zum Installieren und Verwenden des Inhaltspakets in Ihrer Umgebung.

## Installation
<a id="installation" class="xliff"></a>  

**Gehen Sie wie folgt vor, um das Power BI-Inhaltspaket zu installieren:**

1. Melden Sie sich mit Ihrem Power BI-Konto (gleiches Konto wie Ihr O365- oder Azure AD-Konto) an [Power BI](https://app.powerbi.com/groups/me/getdata/services) an.

2. Klicken Sie unten im linken Navigationsbereich auf **Daten abrufen**.

    ![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/01.png)
 
3. Klicken Sie im Feld **Services** (Dienste) auf **Get** (Abrufen).
   
    ![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/02.png)

4.  Suchen Sie nach **Azure Active Directory**.

    ![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/03.png)
 
5.  Geben Sie nach Aufforderung die ID Ihres Azure AD-Mandanten ein, und klicken Sie anschließend auf **Weiter**.

    > [!TIP] 
    > Eine schnelle Möglichkeit, die Mandanten-ID für Ihren Office 365/Azure AD-Mandanten abzurufen, ist die Anmeldung am Azure AD-Portal, der Drilldown in das Verzeichnis und das Kopieren der ID aus der folgenden URL: „https://manage.windowsazure.com/woodgroveonline.com#Workspaces/ActiveDirectoryExtension/Directory/<tenantid>/directoryQuickStart“.

    ![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/04.png) 

6.  Klicken Sie auf **Anmelden**. 
 
    ![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/05.png) 



7.  Geben Sie Ihren Benutzernamen und Ihr Kennwort ein, und klicken Sie dann auf **Anmelden**.
 
    ![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/06.png) 

8.  Klicken Sie im Dialogfeld für die App-Zustimmung auf **Akzeptieren**.
 
9.  Wenn das Dashboard für die Azure Active Directory-Aktivitätsprotokolle erstellt wurde, können Sie darauf klicken.
 
    ![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/08.png) 

## Welche Möglichkeiten habe ich mit diesem Inhaltspaket?
<a id="what-can-i-do-with-this-content-pack" class="xliff"></a>

Bevor beschrieben wird, was mit diesem Inhaltspaket möglich ist, erhalten Sie hier eine kurze Vorabübersicht über die verschiedenen Berichte im Inhaltspaket. Die Berichtsdaten gelten für die **letzten 30 Tage**.

### Berichte, die in dieser Version des Inhaltspakets für Azure Active Directory-Protokolle enthalten sind
<a id="reports-included-in-this-version-of-azure-active-directory-logs-content-pack" class="xliff"></a>

**Bericht zu App-Nutzung und Trends**: Sie erhalten Einblicke in die Apps, die in Ihrer Organisation verwendet werden bzw. die am häufigsten und zu den unterschiedlichen Zeiten genutzt werden. Sie können diesen Bericht einsetzen, um zu erfahren, wie eine App, die in Ihrer Organisation vor Kurzem eingeführt wurde, verwendet wird oder welche Apps beliebt sind. Auf diese Weise können Sie die Nutzung verbessern, falls Sie ermitteln, dass die App nicht genutzt wird.

**Anmeldungen nach Ort und Benutzer**: Informieren Sie sich über alle Anmeldungen, die per Azure-Identität durchgeführt werden, und über die Identität der Benutzer. Hierdurch können Sie einzelne Anmeldungen eingehender untersuchen und beispielsweise folgende Fragen beantworten:

- Von welchem Ort aus hat sich der Benutzer angemeldet?
- Welcher Benutzer hat sich am häufigsten angemeldet, und von wo aus erfolgen die Anmeldungen? 
- War die Anmeldung erfolgreich?  
 
Sie können Details anzeigen, indem Sie auf ein bestimmtes Datum oder einen Ort klicken.

**Eindeutige Benutzer pro App**: Verschaffen Sie sich einen Überblick über alle eindeutigen Benutzer, die eine bestimmte App verwenden. Dies betrifft nur Benutzer, die sich *erfolgreich* an einer Anwendung angemeldet haben.

**Geräteanmeldungen**: Informieren Sie sich über die Art von Betriebssystemen und Browsern, die von Benutzern in Ihrer Organisation verwendet werden, und erhalten Sie ausführliche Informationen zu den Benutzern, z.B.:

- Benutzername
- IP-Adresse
- Ort 
- Anmeldestatus 

Mit diesem Bericht können Sie die verschiedenen Geräteprofile verstehen, die in Ihrer Organisation verwendet werden, und basierend auf der Nutzung Geräterichtlinien bestimmen.

**SSPR-Trichter**: Entwickeln Sie ein Verständnis, wie die Zurücksetzung von Kennwörtern in Ihrer Organisation durchgeführt wird. Informieren Sie sich darüber, wie viele Kennwortzurücksetzungen mit dem SSPR-Tool durchgeführt wurden und wie viele davon erfolgreich waren. Tauchen Sie per SSPR-Trichter tiefer in Fehler bei der Kennwortzurücksetzung ein, damit Sie verstehen, warum bestimmte Fehler aufgetreten sind. Mit diesem Bericht entwickeln Sie ein besseres Verständnis der Nutzung des SSPR-Tools in Ihrer Organisation, damit Sie die richtigen Entscheidungen treffen können.

## Anpassen des Azure AD-Aktivitätspakets
<a id="customizing-azure-ad-activity-content-pack" class="xliff"></a>

**Visualisierung ändern**: Sie können die Visualisierung eines Berichts ändern, indem Sie auf **Bericht bearbeiten** klicken und die gewünschte Visualisierung auswählen.
 
![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/09.png) 
 
![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/10.png) 

**Zusätzliche Felder einfügen**: Sie können dem Bericht ein Feld hinzufügen oder es entfernen, indem Sie die Visualisierung auswählen, für die Sie das Feld hinzufügen oder entfernen möchten. Im folgenden Beispiel wird der Tabellenansicht ein Feld für den „Anmeldestatus“ hinzugefügt. 
 
![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/11.png) 

**Visualisierungen an Ihr Dashboard anheften**: Sie können Ihr Dashboard anpassen und Ihre eigenen Visualisierungen in den Bericht einfügen und im Dashboard anheften. Im folgenden Beispiel wird ein neuer Filter mit dem Namen „Sign-in Status“ (Anmeldestatus) hinzugefügt und in den Bericht eingebunden. Außerdem wurde die Visualisierung von einem Balkendiagramm in ein Liniendiagramm geändert, und die neue Visualisierung kann im Dashboard angeheftet werden.

![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/12.png) 

![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/13.png) 
 

 


**Dashboard freigeben**: Nachdem Sie den gewünschten Inhalt erstellt haben, können Sie das Dashboard für die Benutzer in Ihrer Organisation freigeben. Beachten Sie, dass Benutzer die von Ihnen ausgewählten Felder im Bericht sehen, nachdem Sie diesen freigegeben haben.
 
![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/14.png) 



## Planen einer täglichen Aktualisierung Ihres Power BI-Berichts
<a id="scheduling-a-daily-refresh-of-your-power-bi-report" class="xliff"></a>

Navigieren Sie zum Planen einer täglichen Aktualisierung Ihres Power BI-Berichts zu **Datasets > Einstellungen > Aktualisierung planen**, und legen Sie wie unten gezeigt Folgendes fest:
 
![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/15.png) 

## Aktualisieren auf eine neuere Version des Inhaltspakets
<a id="updating-to-newer-version-of-content-pack" class="xliff"></a>

Gehen Sie wie folgt vor, wenn Sie Ihr Inhaltspaket aktualisieren möchten, um eine neuere Version zu erhalten:

- Laden Sie das neue Inhaltspaket herunter, und richten Sie es gemäß der Anleitung in diesem Artikel ein.

- Navigieren Sie nach dem Einrichten zu **Datenquelle > Einstellungen > Anmeldeinformationen für die Datenquelle**, und geben Sie Ihre Anmeldeinformationen wie unten gezeigt erneut ein:

    ![Azure Active Directory-Power BI-Inhaltspaket](./media/active-directory-reporting-power-bi-content-pack-how-to/16.png) 

Sobald die neue Version des Inhaltspakets funktioniert, können Sie die alte Version bei Bedarf entfernen, indem Sie die zugrunde liegenden Berichte und Datasets löschen, die diesem Inhaltspaket zugeordnet sind.

## Es treten weiterhin Probleme auf?
<a id="still-having-issues" class="xliff"></a> 

Hilfreiche Informationen finden Sie in unserem [Handbuch zur Problembehandlung](active-directory-reporting-troubleshoot-content-pack.md). Allgemeine Hilfe zu Power BI finden Sie in diesen [Hilfeartikeln](https://powerbi.microsoft.com/en-us/documentation/powerbi-service-get-started/).
 




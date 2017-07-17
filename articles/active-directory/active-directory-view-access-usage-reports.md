---
title: Anzeigen Ihrer Zugriffs- und Nutzungsberichte | Microsoft Docs
description: "Erläutert das Anzeigen von Zugriffs- und Nutzungsberichten, um sich einen Einblick in die Integrität und Sicherheit des Verzeichnisses Ihrer Organisation zu verschaffen."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: a074bc4e-cf3f-4ad1-8cc6-4199d2e09ce4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/15/2017
ms.author: dhanyahk;markvi
ms.custom: oldportal
ms.translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 8b3835d354512645cd47ffa4541d6155c80e77ee
ms.contentlocale: de-de
ms.lasthandoff: 11/17/2016

---
# Anzeigen von Zugriffs- und Nutzungsberichten
<a id="view-your-access-and-usage-reports" class="xliff"></a>
*Diese Dokumentation ist Teil des Handbuchs für [Azure Active Directory Reporting](active-directory-reporting-guide.md).*

Sie können die Zugriffs- und Nutzungsberichte von Azure Active Directory verwenden, um sich einen Einblick in die Integrität und Sicherheit des Verzeichnisses Ihrer Organisation zu verschaffen. Mithilfe dieser Informationen kann ein Verzeichnisadministrator mögliche Sicherheitsrisiken besser bestimmen, um angemessen zu planen, wie diese Risiken eingedämmt werden können.

Im Azure-Verwaltungsportal werden Berichte auf folgende Weise kategorisiert:

* Anomalieberichte – Enthalten Anmeldeereignisse, die wir als anomal eingestuft haben. Unser Ziel ist, Sie auf solche Aktivitäten aufmerksam zu machen und Ihnen zu ermöglichen, eine Entscheidung zu treffen, ob ein Ereignis verdächtig ist.
* Integrierte Anwendungsberichte – Bieten Einblicke, wie Cloudanwendungen in Ihrer Organisation verwendet werden. Azure Active Directory ermöglicht die Integration in Tausende von Cloudanwendungen.
* Fehlerberichte – Enthalten Hinweis auf Fehler, die bei der Bereitstellung von Konten für externe Anwendungen auftreten können.
* Benutzerspezifische Berichte – Enthalten Geräte-/Anmeldeaktivitätsdaten für einen bestimmten Benutzer.
* Aktivitätsprotokolle – Enthalten eine Aufzeichnung aller überwachten Ereignisse in den letzten 24 Stunden, letzten 7 Tagen oder letzten 30 Tagen sowie von Änderungen an Gruppenaktivitäten sowie Kennwortzurücksetzungs- und Registrierungsaktivitäten.

> [!NOTE]
> * Einige erweiterte Anomalie- und Ressourcennutzungsberichte sind nur verfügbar, wenn Sie [Azure Active Directory Premium](active-directory-get-started-premium.md)aktivieren. Erweiterte Berichte unterstützen Sie bei der Optimierung der Zugriffssicherheit, Reaktion auf potenzielle Bedrohungen und dem Zugriff auf Analysen zur Geräte- und Anwendungsnutzung.
> * Die Azure Active Directory-Editionen Premium und Basic stehen für Kunden in China zur Verfügung, die mit der weltweit verfügbaren Instanz von Azure Active Directory arbeiten. Allerdings werden die Azure Active Directory-Editionen Premium und Basic derzeit durch den in China von 21Vianet betriebenen Microsoft Azure-Dienst nicht unterstützt. Wenn Sie weitere Informationen benötigen, kontaktieren Sie uns im [Azure Active Directory-Forum](https://feedback.azure.com/forums/169401-azure-active-directory/).
> 
> 

## Berichte
<a id="reports" class="xliff"></a>
| Bericht | Beschreibung |
| --- | --- |
| **Berichte zu anomalen Aktivitäten** | |
| [Anmeldungen aus unbekannten Quellen](active-directory-reporting-sign-ins-from-unknown-sources.md) |Zeigt möglicherweise einen Versuch einer Anmeldung ohne Ablaufverfolgung an. |
| [Anmeldungen nach mehreren Fehlern](active-directory-reporting-sign-ins-after-multiple-failures.md) |Zeigt möglicherweise einen erfolgreichen Brute-Force-Angriff an. |
| [Anmeldungen aus mehreren geografischen Regionen](active-directory-reporting-sign-ins-from-multiple-geographies.md) |Zeigt möglicherweise an, dass sich mehrere Benutzer mit dem gleichen Konto anmelden. |
| [Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md) |Zeigt möglicherweise eine erfolgreiche Anmeldung nach einem langfristigen Einbruchsversuch an. |
| [Anmeldungen von möglicherweise infizierten Geräten](active-directory-reporting-sign-ins-from-possibly-infected-devices.md) |Gibt einen Versuch an, sich über möglicherweise infizierte Geräte anzumelden. |
| [Irreguläre Anmeldeaktivitäten](active-directory-reporting-irregular-sign-in-activity.md) |Zeigt möglicherweise anomale Ereignisse bei Anmeldemustern von Benutzern an. |
| [Benutzer mit anomalen Anmeldeaktivitäten](active-directory-reporting-users-with-anomalous-sign-in-activity.md) |Gibt Benutzer an, deren Konten möglicherweise beschädigt sind. |
| Benutzer mit kompromittierten Anmeldeinformationen |Benutzer mit kompromittierten Anmeldeinformationen |
| **Aktivitätsprotokolle** | |
| Überwachungsbericht |Überwachte Ereignisse in Ihrem Verzeichnis |
| Kennwortzurücksetzungsaktivität |Bietet eine detaillierte Übersicht über das Zurücksetzen des Kennworts in Ihrer Organisation. |
| Aktivität "Registrierung für Zurücksetzen des Kennworts" |Bietet eine detaillierte Übersicht über Registrierungen für das Zurücksetzen des Kennworts in Ihrer Organisation. |
| Self-Service-Gruppenaktivität |Stellt ein Aktivitätsprotokoll aller Self-Service-Aktivitäten für Gruppen in Ihrem Verzeichnis bereit. |
| **Integrierte Anwendungen** | |
| Anwendungsnutzung |Stellt eine Nutzungszusammenfassung für alle in Ihr Verzeichnis integrierten SaaS-Anwendungen bereit. |
| Kontobereitstellungsaktivität |Stellt einen Verlauf der Kontobereitstellungsversuche für externe Anwendungen bereit. |
| Status des Kennwortrollovers |Stellt eine ausführliche Übersicht über den Status des automatischen Kennwortrollovers von SaaS-Anwendungen zur Verfügung. |
| Kontobereitstellungsfehler |Zeigt eine Auswirkung auf den Zugriff von Benutzern auf externe Anwendungen an. |
| **Rights Management** | |
| RMS-Nutzung |Enthält eine Übersicht zur Nutzung des Rights Management. |
| Aktivste RMS-Benutzer |Listet die Top 1000 aktiven Benutzer auf, die auf RMS-geschützte Dateien zugegriffen haben. |
| Nutzung von RMS-Geräten |Listet Geräte auf, die für den Zugriff auf RMS-geschützte Dateien verwendet werden. |
| Nutzung RMS-fähiger Anwendungen |Bietet die Nutzung von RMS-fähigen Anwendungen. |

## Berichte in den verschiedenen Editionen
<a id="report-editions" class="xliff"></a>
| Bericht | Free | Basic | Premium |
| --- | --- | --- | --- |
| **Berichte zu anomalen Aktivitäten** | | | |
| Anmeldungen aus unbekannten Quellen |✓ |✓  |✓ |
| Anmeldungen nach mehreren Fehlern |✓ |✓  |✓ |
| Anmeldungen aus mehreren geografischen Regionen |✓ |✓  |✓ |
| Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten | | |✓ |
| Anmeldungen von möglicherweise infizierten Geräten | | |✓ |
| Irreguläre Anmeldeaktivitäten | | |✓ |
| Benutzer mit anomalen Anmeldeaktivitäten | | |✓ |
| Benutzer mit kompromittierten Anmeldeinformationen | | |✓ |
| **Aktivitätsprotokolle** | | | |
| Überwachungsbericht |✓ |✓  |✓ |
| Kennwortzurücksetzungsaktivität | | |✓ |
| Aktivität "Registrierung für Zurücksetzen des Kennworts" | | |✓ |
| Self-Service-Gruppenaktivität | | |✓ |
| **Integrierte Anwendungen** | | | |
| Anwendungsnutzung | | |✓ |
| Kontobereitstellungsaktivität |✓ |✓  |✓ |
| Status des Kennwortrollovers | | |✓ |
| Kontobereitstellungsfehler |✓ |✓  |✓ |
| **Rechteverwaltung** | | | |
| RMS-Nutzung | | |Nur RMS |
| Aktivste RMS-Benutzer | | |Nur RMS |
| Nutzung von RMS-Geräten | | |Nur RMS |
| Nutzung RMS-fähiger Anwendungen | | |Nur RMS |

## Berichte zu anomalen Aktivitäten
<a id="anomalous-activity-reports" class="xliff"></a>
<p>Die Berichte zu anomalen Aktivitäten markieren verdächtige Anmeldeaktivitäten in Office365, im Azure-Verwaltungsportal, im Azure AD-Zugriffsbereich, in SharePoint Online, in Dynamics CRM Online und in anderen Microsoft-Onlinediensten.</p>

<p>All diese Berichte, mit Ausnahme des Berichts „Anmeldungen nach mehreren Fehlern“, markieren außerdem verdächtige <i>Verbundanmeldungen</i> bei den oben genannten Diensten, unabhängig vom Verbundanbieter. </p>

<p>Folgende Berichte sind verfügbar: </p><ul>

<li>[Anmeldungen aus unbekannten Quellen](active-directory-reporting-sign-ins-from-unknown-sources.md).</li>

<li>[Anmeldungen nach mehreren Fehlern](active-directory-reporting-sign-ins-after-multiple-failures.md).</li>

<li>[Anmeldungen aus mehreren geografischen Regionen](active-directory-reporting-sign-ins-from-multiple-geographies.md).</li>

<li>[Anmeldungen von IP-Adressen mit verdächtigen Aktivitäten](active-directory-reporting-sign-ins-from-ip-addresses-with-suspicious-activity.md).</li>

<li>[Irreguläre Anmeldeaktivitäten](active-directory-reporting-irregular-sign-in-activity.md).</li>

<li>[Anmeldungen von möglicherweise infizierten Geräten](active-directory-reporting-sign-ins-from-possibly-infected-devices.md).</li>

<li>[Benutzer mit anomalen Anmeldeaktivitäten](active-directory-reporting-users-with-anomalous-sign-in-activity.md).</li>

<li>Benutzer mit kompromittierten Anmeldeinformationen</li></ul>

## Aktivitätsprotokolle
<a id="activity-logs" class="xliff"></a>
### Überwachungsbericht
<a id="audit-report" class="xliff"></a>
| Beschreibung | Speicherort des Berichts |
|:--- |:--- |
| Zeigt eine Aufzeichnung aller überwachten Ereignisse in den letzten 24 Stunden, letzten 7 Tagen oder letzten 30 Tagen. <br /> Weitere Informationen finden Sie unter [Azure Active Directory-Überwachungsberichtsereignisse](active-directory-reporting-audit-events.md) |Verzeichnis > Registerkarte "Berichte" |

![Überwachungsbericht](./media/active-directory-view-access-usage-reports/auditReport.PNG)

### Kennwortzurücksetzungsaktivität
<a id="password-reset-activity" class="xliff"></a>
| Beschreibung | Speicherort des Berichts |
|:--- |:--- |
| Zeigt alle Versuche der Kennwortzurücksetzung, die in Ihrer Organisation erfolgt sind. |Verzeichnis > Registerkarte "Berichte" |

![Kennwortzurücksetzungsaktivität](./media/active-directory-view-access-usage-reports/passwordResetActivity.PNG)

### Aktivität "Registrierung für Zurücksetzen des Kennworts"
<a id="password-reset-registration-activity" class="xliff"></a>
| Beschreibung | Speicherort des Berichts |
|:--- |:--- |
| Zeigt alle Registrierungen für die Kennwortzurücksetzung, die in Ihrer Organisation erfolgt sind. |Verzeichnis > Registerkarte "Berichte" |

![Aktivität "Registrierung für Zurücksetzen des Kennworts"](./media/active-directory-view-access-usage-reports/passwordResetRegistrationActivity.PNG)

### Self-Service-Gruppenaktivität
<a id="self-service-groups-activity" class="xliff"></a>
| Beschreibung | Speicherort des Berichts |
|:--- |:--- |
| Zeigt alle Aktivitäten der per Self-Service verwalteten Gruppen in Ihrem Verzeichnis. |Verzeichnis > Benutzer > <i>Benutzer</i> > Registerkarte "Geräte" |

![Self-Service-Gruppenaktivität](./media/active-directory-view-access-usage-reports/selfServiceGroupsActivity.PNG)

## Integrierte Anwendungsberichte
<a id="integrated-applications-reports" class="xliff"></a>
### Anwendungsnutzung: Zusammenfassung
<a id="application-usage-summary" class="xliff"></a>
| Beschreibung | Speicherort des Berichts |
|:--- |:--- |
| In diesem Bericht können Sie die Nutzung aller SaaS-Anwendungen in Ihrem Verzeichnis ablesen. Dieser Bericht basiert darauf, wie oft Benutzer im Zugriffsbereich auf die Anwendung geklickt haben. |Verzeichnis > Registerkarte "Berichte" |

Dieser Bericht enthält Informationen zu Anmeldungen bei *allen* Anwendungen, auf die Ihr Verzeichnis Zugriff hat, einschließlich vorab integrierter Microsoft-Anwendungen.

Zu den bereits integrierten Microsoft-Anwendungen zählen Office 365, Sharepoint, das Azure-Verwaltungsportal und andere.

![Zusammenfassung Anwendungsnutzung](./media/active-directory-view-access-usage-reports/applicationUsage.PNG)

### Anwendungsnutzung: detailliert
<a id="application-usage-detailed" class="xliff"></a>
| Beschreibung | Speicherort des Berichts |
|:--- |:--- |
| In diesem Bericht können Sie die Nutzung einer bestimmten SaaS-Anwendung in Ihrem Verzeichnis ablesen. Dieser Bericht basiert darauf, wie oft Benutzer im Zugriffsbereich auf die Anwendung geklickt haben. |Verzeichnis > Registerkarte "Berichte" |

### Anwendungsdashboard
<a id="application-dashboard" class="xliff"></a>
| Beschreibung | Speicherort des Berichts |
|:--- |:--- |
| Dieser Bericht zeigt kumulativ die Anmeldungen an der Anwendung durch Benutzer in Ihrer Organisation über einen ausgewählten Zeitraum. Das Diagramm auf der Dashboardseite hilft Ihnen beim Ausmachen von Trends der gesamten Nutzung der Anwendung. |Verzeichnis > Anwendung > Registerkarte "Dashboard" |

## Fehlerberichte
<a id="error-reports" class="xliff"></a>
### Kontobereitstellungsfehler
<a id="account-provisioning-errors" class="xliff"></a>
| Beschreibung | Speicherort des Berichts |
|:--- |:--- |
| Dieser Bericht dient zum Überwachen von Fehlern, die während der Synchronisierung von Konten aus SaaS-Anwendungen mit Azure Active Directory auftreten. |Verzeichnis > Registerkarte "Berichte" |

![Kontobereitstellungsfehler](./media/active-directory-view-access-usage-reports/accountProvisioningErrors.PNG)

## Benutzerspezifische Berichte
<a id="user-specific-reports" class="xliff"></a>
### Geräte
<a id="devices" class="xliff"></a>
| Beschreibung | Speicherort des Berichts |
|:--- |:--- |
| In diesem Bericht finden Sie die IP-Adresse und den geografischen Standort der Geräte, die ein bestimmter Benutzer für Zugriff auf Azure Active Directory verwendet hat. |Verzeichnis > Benutzer > <i>Benutzer</i> > Registerkarte "Geräte" |

### Aktivität
<a id="activity" class="xliff"></a>
| Beschreibung | Speicherort des Berichts |
|:--- |:--- |
| Zeigt die Anmeldeaktivität eines Benutzers. Der Bericht enthält Informationen wie die Anwendung, bei der die Anmeldung erfolgt ist, das verwendete Gerät, die IP-Adresse und den Standort. Wir erfassen nicht den Verlauf von Benutzern, die sich mit einem Microsoft-Konto anmelden. |Verzeichnis > Benutzer > <i>Benutzer</i> > Registerkarte "Aktivität" |

#### Im Bericht "Benutzeraktivität" enthaltene Anmeldeereignisse
<a id="sign-in-events-included-in-the-user-activity-report" class="xliff"></a>
Nur bestimmte Arten von Anmeldeereignissen werden im Bericht "Benutzeraktivität" angezeigt.

| Ereignistypen | Enthalten? |
| --- | --- |
| Anmeldungen beim [Zugriffsbereich](http://myapps.microsoft.com/) |Ja |
| Anmeldungen beim [Azure-Verwaltungsportal](https://manage.windowsazure.com/) |Ja |
| Anmeldungen beim [Microsoft Azure-Portal](https://portal.azure.com/) |Ja |
| Anmeldungen beim [Office 365-Portal](http://portal.office.com/) |Ja |
| Anmeldungen bei einer systemeigenen Anwendung wie Outlook (siehe nachfolgende Ausnahme) |Ja |
| Anmeldungen bei einer Verbund-/bereitgestellten App über den Zugriffsbereich wie z. B. Salesforce |Ja |
| Anmeldungen bei einer kennwortbasierten App über den Zugriffsbereich wie z. B. Twitter |Ja |
| Anmeldungen bei einer benutzerdefinierten Geschäfts-App, die dem Verzeichnis hinzugefügt wurde |Nein (in Kürze verfügbar) |
| Anmeldungen bei einer Azure AD-Anwendungsproxy-App, die dem Verzeichnis hinzugefügt wurde |Nein (in Kürze verfügbar) |

> Hinweis: Um diese Bericht übersichtlich zu halten, werden Anmeldungen durch den [Microsoft Online Services-Anmeldeassistenten](http://community.office365.com/en-us/w/sso/534.aspx) nicht gezeigt.
> 
> 

## Zu beachtende Aspekte bei einer vermuteten Sicherheitsverletzung
<a id="things-to-consider-if-you-suspect-security-breach" class="xliff"></a>
Wenn Sie vermuten, dass ein Benutzerkonto ggf. gefährdet ist, oder von einer verdächtigen Benutzeraktivität ausgehen, die zu einer Sicherheitsverletzung Ihrer Verzeichnisdaten in der Cloud führen könnte, sollten Sie eine oder mehrere der folgenden Aktionen erwägen:

* Den Benutzer kontaktieren, um die Aktivität zu überprüfen
* Das Kennwort des Benutzers zurücksetzen
* [Multi-Factor Authentication](../multi-factor-authentication/multi-factor-authentication-get-started.md) zum Erhöhen der Sicherheit aktivieren

## Anzeigen oder Herunterladen eines Berichts
<a id="view-or-download-a-report" class="xliff"></a>
1. Klicken Sie im klassischen Azure-Portal auf **Active Directory**, anschließend auf den Namen des Verzeichnisses Ihrer Organisation und dann auf **Berichte**.
2. Klicken Sie auf der Seite "Berichte" auf den Bericht, den Sie anzeigen und/oder herunterladen möchten.
   
   > [!NOTE]
   > Wenn Sie die Berichtsfunktion von Azure Active Directory zum ersten Mal verwenden, wird eine Aktivierungsmeldung angezeigt. Wenn Sie zustimmen, klicken Sie auf das Häkchensymbol, um den Vorgang fortzusetzen.
   > 
   > 
3. Klicken Sie auf das Dropdownmenü neben "Intervall", und wählen Sie dann einen der folgenden Zeiträume aus, der zum Generieren des Berichts verwendet werden soll:
   
   * Letzte 24 Stunden
   * Letzte 7 Tage
   * Letzte 30 Tage
4. Klicken Sie auf das Häkchensymbol, um den Bericht auszuführen.
   * Im klassischen Azure-Portal werden bis zu 1000 Ereignisse angezeigt.
5. Falls zutreffend, klicken Sie auf **Herunterladen** , um den Bericht in einer komprimierten Datei im CSV-Format herunterzuladen, um ihn offline anzeigen oder zu archivieren.
   * In der heruntergeladenen Datei werden bis zu 75.000 Ereignisse aufgenommen.
   * Informationen zu weiteren Daten finden Sie im Artikel zur [Azure AD Reporting-API](active-directory-reporting-api-getting-started.md).

## Ignorieren von Ereignissen
<a id="ignore-an-event" class="xliff"></a>
Wenn Sie Anomalieberichte prüfen, stellen Sie möglicherweise fest, dass Sie verschiedene Ereignisse ignorieren können, die in verwandten Berichten enthalten sind. Wenn Sie ein Ereignis ignorieren möchten, markieren Sie einfach das Ereignis im Bericht und klicken dann auf **Ignorieren**. Die Schaltfläche **Ignorieren** dient zum dauerhaften Entfernen des markierten Ereignisses aus dem Bericht und kann nur von lizenzierten globalen Administratoren verwendet werden.

## Automatische E-Mail-Benachrichtigungen
<a id="automatic-email-notifications" class="xliff"></a>
Weitere Informationen zu den Benachrichtigungen für Berichte in Azure AD finden Sie unter [Benachrichtigungen zu Azure Active Directory-Berichten](active-directory-reporting-notifications.md).

## Nächste Schritte
<a id="whats-next" class="xliff"></a>
* [Erste Schritte mit Azure Active Directory Premium](active-directory-get-started-premium.md)
* [Hinzufügen Ihres Unternehmensbranding zur Anmelde- und Zugriffsbereichsseite](active-directory-add-company-branding.md)



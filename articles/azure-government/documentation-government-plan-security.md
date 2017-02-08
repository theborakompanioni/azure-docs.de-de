---
title: Azure Government-Sicherheit | Microsoft Docs
description: "Bietet eine Übersicht über die verfügbaren Dienste in Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: zakramer
manager: liki
ms.assetid: c3645bda-bf35-4232-a54d-7a0bfab2d594
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 11/14/2016
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: a9cc2bc044caddec63645932067f7a346945db78
ms.openlocfilehash: 11d0a67a4c368b50e2f2e8648c4fb2db8e0421c2


---
# <a name="security"></a>Sicherheit
## <a name="principles-for-securing-customer-data-in-azure-government"></a>Prinzipien für den Schutz von Kundendaten in Azure Government
Azure Government bietet eine Reihe von Features und Diensten zur Erstellung von Cloudlösungen, die Ihre Anforderungen in puncto regulierte/kontrollierte Daten erfüllen. Eine konforme Kundenlösung ist nichts anderes als die effektive Implementierung der standardmäßig verfügbaren Azure Government-Funktionen, kombiniert mit fundierter Datensicherheit.

Wenn Sie eine Lösung in Azure Government hosten, nimmt Ihnen Microsoft die Erfüllung zahlreicher Anforderungen auf der Cloudinfrastrukturebene ab.

Das folgende Diagramm zeigt das Defense-in-Depth-Modell von Azure. So bietet Microsoft etwa grundlegenden DDoS-Schutz für die Cloudinfrastruktur sowie Kundenfunktionen wie Sicherheitsappliances für kundenspezifische DDoS-Anforderungen für Anwendungen.

![alt text](./media/azure-government-Defenseindepth.png)

Diese Seite enthält Informationen zu den grundlegenden Prinzipien für den Schutz Ihrer Dienste und Anwendungen sowie Anleitungen und bewährte Methoden für die Umsetzung dieser Prinzipien. Kunden erfahren hier also, wie sie Azure Government optimal verwenden, um die Pflichten und Aufgaben im Zusammenhang mit einer Lösung zu erfüllen, die zur Verarbeitung von ITAR-Informationen verwendet wird.

 Übergreifende Prinzipien für den Schutz von Kundendaten:

* Schutz von Daten mittels Verschlüsselung
* Verwaltung geheimer Schlüssel
* Isolierung zur Einschränkung des Datenzugriffs

### <a name="protecting-customer-data-using-encryption"></a>Schutz von Kundendaten mittels Verschlüsselung
Die Eindämmung von Risiken und die Einhaltung gesetzlicher Vorschriften sind die treibenden Kräfte hinter dem wachsenden Fokus und der zunehmenden Bedeutung der Datenverschlüsselung. Verwenden Sie eine wirksame Verschlüsselungsimplementierung, um Ihre aktuellen Maßnahmen für Netzwerk- und Anwendungssicherheit zu verbessern und somit insgesamt das Risiko für Ihre Cloudumgebung zu senken.

#### <a name="encryption-at-rest"></a>Verschlüsselung ruhender Daten
Bei der Verschlüsselung ruhender Daten geht es um den Schutz von auf Datenträgern gespeicherten Kundeninhalten. Hierfür gibt es mehrere Möglichkeiten:

#### <a name="storage-service-encryption"></a>Storage Service Encryption
Azure Storage Service Encryption wird auf der Speicherkontoebene aktiviert, sodass Block- und Seitenblobs beim Schreiben in Azure Storage automatisch verschlüsselt werden. Wenn Sie die Daten aus Azure Storage lesen, werden sie vom Speicherdienst vor der Rückgabe entschlüsselt. Mit dieser Methode können Sie Ihre Daten ohne zusätzlichen Programmieraufwand schützen.

#### <a name="client-side-encryption"></a>Clientseitige Verschlüsselung
Die clientseitige Verschlüsselung ist in Java- und .NET-Speicherclientbibliotheken integriert. Diese können die Azure Key Vault-APIs verwenden, was eine unkomplizierte Implementierung ermöglicht. Verwenden Sie Azure Key Vault, um mithilfe von Azure Active Directory auf die geheimen Schlüssel bestimmter Benutzer in Azure Key Vault zuzugreifen.

#### <a name="encryption-in-transit"></a>Verschlüsselung während der Übertragung
Die für die Verbindung mit Azure Government verfügbare Basisverschlüsselung unterstützt das TLS 1.2-Protokoll (Transport Level Security) sowie X.509-Zertifikate. Für Infrastrukturnetzwerkverbindungen zwischen Azure Government-Datencentern werden auch FIPS 140-2-Kryptografiealgorithmen (Federal Information Processing Standard) der Stufe 1 verwendet.  Windows Server 2012 R2 und Windows 8 sowie virtuelle Computer und Azure-Dateifreigaben können SMB 3.0 für die Verschlüsselung zwischen dem virtuellen Computer und der Dateifreigabe verwenden. Verwenden Sie die clientseitige Verschlüsselung, um die Daten zu verschlüsseln, bevor sie in den Speicher einer Clientanwendung übertragen werden, und sie nach der Übertragung aus dem Speicher wieder zu entschlüsseln.

#### <a name="best-practices-for-encryption"></a>Best Practices für die Verschlüsselung
* Virtuelle IaaS-Computer: Verwenden Sie Azure Disk Encryption. Aktivieren Sie Storage Service Encryption, um die VHD-Dateien zu verschlüsseln, mit denen die Datenträger in Azure Storage gesichert werden. Dadurch werden jedoch nur neu geschriebene Daten verschlüsselt. Wenn Sie also einen virtuellen Computer erstellen und dann Storage Service Encryption für das Speicherkonto aktivieren, das die VHD-Datei enthält, werden nur die Änderungen verschlüsselt, nicht die ursprüngliche VHD-Datei.
* Clientseitige Verschlüsselung: Dies ist die sicherste Methode zum Verschlüsseln Ihrer Daten, da die Verschlüsselung vor der Übertragung stattfindet und die Daten als ruhende Daten verschlüsselt werden. Allerdings müssen Sie hierzu Ihren Anwendungen, die den Speicher nutzen, Code hinzuzufügen, was vielleicht nicht unbedingt erwünscht ist. In diesen Fällen können Sie bei der Übertragung HTTPS für Ihre Daten verwenden, und Storage Service Encryption zum Verschlüsseln ruhender Daten. Die clientseitige Verschlüsselung stellt außerdem eine zusätzliche Belastung für den Client dar, was wiederum bei der Skalierbarkeitsplanung berücksichtigt werden muss. Dies gilt insbesondere, wenn Sie große Datenmengen verschlüsseln und übertragen.

### <a name="protecting-customer-data-by-managing-secrets"></a>Schutz von Kundendaten durch die Verwaltung geheimer Schlüssel
Eine sichere Schlüsselverwaltung ist essenziell, um Daten in der Cloud zu schützen. Für Kunden empfiehlt es sich, die Schlüsselverwaltung möglichst einfach zu gestalten und die Kontrolle über die Schlüssel zu behalten, die von Cloudanwendungen und -diensten verwendet werden, um Daten zu verschlüsseln.

#### <a name="best-practices-for-managing-secrets"></a>Best Practices für die Verwaltung geheimer Schlüssel
* Verwenden Sie Key Vault, um das Risiko zu minimieren, dass geheime Schlüssel durch hartcodierte Konfigurationsdateien, durch Skripts oder im Quellcode offengelegt werden. Azure Key Vault verschlüsselt Schlüssel (etwa die Verschlüsselungsschlüssel für Azure Disk Encryption) und geheime Schlüssel (beispielsweise Kennwörter) mittels Speicherung in FIPS 140-2-Hardwaresicherheitsmodulen (HSMs) der Stufe 2. Zur Steigerung der Sicherheit können Sie Schlüssel in diese HSMs importieren oder in diesen generieren.
* Anwendungscode und Vorlagen dürfen nur URI-Verweise auf die geheimen Schlüssel enthalten, sodass die tatsächlichen geheimen Schlüssel nicht im Code, in der Konfiguration oder in Quellcoderepositorys enthalten sind. Dies verhindert umfassende Phishing-Angriffe auf interne oder externe Repositorys, z. B. "harvest-bots" auf GitHub.
* Verwenden Sie innerhalb von Key Vault eine sichere RBAC-Steuerung. Ein vertrauenswürdiger Mitarbeiter, der aus dem Unternehmen ausscheidet oder innerhalb des Unternehmens in eine andere Gruppe wechselt, darf keinen Zugriff mehr auf die geheimen Schlüssel haben.

Weitere Informationen finden Sie in der [öffentlichen Dokumentation zu Azure Key Vault](../key-vault/index.md).

### <a name="isolation-to-restrict-data-access"></a>Isolierung zur Einschränkung des Datenzugriffs
Bei der Isolierung geht es um die Einrichtung von Grenzen, Segmentierungen und Containern, um den Datenzugriff auf autorisierte Benutzer, Dienste und Anwendungen zu beschränken. So ist etwa die Trennung von Mandanten eine unverzichtbare Sicherheitsmaßnahme für mehrinstanzenfähigen Cloudplattformen wie Microsoft Azure. Eine logische Trennung trägt dazu bei, Konflikte zwischen Mandanten zu verhindern.

#### <a name="environment-isolation"></a>Isolierung der Umgebung
Bei der Azure Government-Umgebung handelt es sich um eine physische Instanz, die vom Rest des Microsoft-Netzwerks getrennt ist. Dies wird durch eine Reihe physischer und logischer Maßnahmen erreicht:

* Schutz durch physische Barrieren mit biometrischen Geräten und Kameras.
* Verwendung von spezifischen Anmeldeinformationen und mehrstufiger Authentifizierung durch Microsoft-Mitarbeiter, die logischen Zugriff auf die Produktionsumgebung benötigen.
* Die gesamte Dienstinfrastruktur für Azure Government befindet sich innerhalb der USA.

#### <a name="per-customer-isolation"></a>Kundenspezifische Isolierung
Azure implementiert Netzwerkzugriffssteuerung und -trennung durch VLAN-Isolierung, ACLs, Lastenausgleichsmodule und IP-Filter.

Kunden können ihre Ressourcen für Abonnements, Ressourcengruppen, virtuelle Netzwerke und Subnetze noch weiter isolieren.

## <a name="screening"></a>Sicherheitsüberprüfung
Dies betrifft die kürzlich bekannt gegebene Zulassung gemäß FedRAMP High und Impact Level 4 des US-Verteidigungsministeriums (DoD). Damit erhöhen sich die Anforderungen für die Sicherheit und Compliance in der gesamten Azure Government-Umgebung.

Wir führen nun eine NACLC-Sicherheitsüberprüfung (National Agency Check with Law and Credit) entsprechend der Definition in Abschnitt 5.6.2.2 des DoD Cloud Computing Security Requirements Guide (SRG, Handbuch für Sicherheitsanforderungen für Computing des US-Verteidigungsministeriums) bei allen unseren Betreibern durch.

> [!NOTE]
> Die für Mitarbeiter bei Clouddienstanbietern mit Zugriff auf Informationen der Ebene 4 und 5 erforderliche minimale Hintergrundüberprüfung basierend auf einer „nicht kritischen, sensiblen“ Position (z.B. DoD ADP-2) ist eine NACLC-Sicherheitsüberprüfung (National Agency Check with Law and Credit) (für „nicht kritische, sensible“ Auftragnehmer) oder eine MBI-Überprüfung (Moderate Risk Background Investigation) für eine Positionsbezeichnung mit „moderatem Risiko“.
> 
> 

In der folgenden Tabelle sind unsere aktuellen Sicherheitsüberprüfungen für Azure Government-Betreiber zusammengefasst:

| Sicherheits- und Hintergrundüberprüfungen für Azure Government | Beschreibung |
| --- | --- |
| US-Staatsbürgerschaft |Überprüfung der US-Staatsbürgerschaft. |
| Hintergrundüberprüfung für Microsoft Cloud (alle zwei Jahre) |Suche nach der US-Sozialversicherungsnummer, Vorstrafenüberprüfung, Liste des Office of Foreign Assets Control (OFAC, US-Amt zur Kontrolle von Auslandsvermögen), Liste des Bureau of Industry and Security (BIS, US-Amt für Industrie und Sicherheit), Sanktionsliste des Office of Defense Trade Controls des US-Außenministeriums. |
| NACLC-Sicherheitsüberprüfung (National Agency Check with Law and Credit) (alle fünf Jahre) |Erweiterung um Hintergrundüberprüfung anhand von Fingerabdruckdatensätzen in FBI-Datenbanken. Weitere Informationen finden Sie auf der <a href="https://www.opm.gov/investigations/background-investigations/federal-investigations-notices/1997/fin97-02/">Website des Office of Personnel Management</a>. |
| <a href="https://www.microsoft.com/en-us/TrustCenter/Compliance/CJIS"> Criminal Justice Information Services (CJIS) </a> |CJIS ist eine Sicherheitsüberprüfung auf bundesstaatlicher, kommunaler und nationaler Ebene (FBI) mit Verarbeitung von Fingerabdruckdatensätzen und Überprüfung des Vorstrafenregisters bei Betriebspersonal, das potenziell Zugriff auf kritische strafrechtliche Informationen (Criminal Justice Information, CJI) erhält.  In jedem US-Bundesstaat werden spezifische Hintergrundüberprüfungen aller Mitarbeiter mit potenziellem Zugriff auf CJI-Daten durchgeführt und anschließende Genehmigungen erteilt. |

Für das Betriebspersonal von Azure gelten die folgenden Prinzipien für den Zugriff:

* Aufgaben sind klar definiert und umfassen getrennte Zuständigkeiten für die Anforderung, Genehmigung und Bereitstellung von Änderungen.
* Der Zugriff erfolgt über definierte Schnittstellen mit spezifischer Funktionalität.
* Der Zugriff erfolgt Just-In-Time (JIT) und wird nur pro Einzelfall oder für ein bestimmtes Wartungsereignis und immer für eine begrenzte Dauer gewährt.
* Der Zugriff ist regelbasiert und umfasst definierte Rollen, denen ausschließlich die zur Problembehandlung erforderlichen Berechtigungen zugewiesen werden.

Zu den Standards der Sicherheitsüberprüfung gehört die Überprüfung der US-Staatsbürgerschaft aller Support- und Betriebsmitarbeiter von Microsoft, bevor ihnen Zugriff auf Azure Government-gehostete Systeme gewährt wird. Supportmitarbeiter, die Daten übertragen müssen, nutzen die sicheren Funktionen in Azure Government. Die sichere Datenübertragung erfordert für den Zugriff gesonderte Authentifizierungsanmeldeinformationen. Um beispielsweise auf Systemmetadaten zuzugreifen, verwenden Betriebsmitarbeiter spezifische webbasierte interne Verwaltungstools, schreibgeschützte APIs und JIT-Rechteerweiterungen.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.




<!--HONumber=Nov16_HO3-->



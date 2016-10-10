<properties
	pageTitle="Dokumentation zu Azure Government | Microsoft Azure"
	description="Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
	services="Azure-Government"
	cloud="gov" 
	documentationCenter=""
	authors="ryansoc"
	manager="zakramer"
	editor=""/>

<tags
	ms.service="multiple"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="azure-government"
	ms.date="08/25/2016"
	ms.author="ryansoc"/>


#  Dokumentationsübersicht für Azure Government

##  Einführung in die Dokumentation zu Azure Government

Diese Website beschreibt den Funktionsumfang der Dienste von [Microsoft Azure Government](https://azure.microsoft.com/features/gov/) und enthält allgemeine Informationen für alle Kunden. Machen Sie sich zunächst mit den Funktionen von Azure Government vertraut, und wenden Sie sich bei Fragen an Ihr Kontoteam, bevor Sie Ihrem Azure Government-Konto explizit regulierte Daten hinzufügen.

Auf der [Compliance-Seite von Microsoft Azure Trust Center](http://www.microsoft.com/de-DE/TrustCenter/Compliance/default.aspx) finden Sie aktuelle Informationen zu den Azure Government-Diensten, die spezifischen Akkreditierungen und Regulierungen unterliegen. Weitere ggf. verfügbare Microsoft-Dienste, die nicht von den Azure Government-Diensten abgedeckt werden, sind nicht Gegenstand dieses Dokuments. Azure Government-Dienste ermöglichen unter anderem die Verwendung einer Vielzahl zusätzlicher Ressourcen, Anwendungen oder Dienste, die von Drittanbietern (oder von Microsoft unter separaten Nutzungsbedingungen und Datenschutzrichtlinien) bereitgestellt werden. Diese sind ebenfalls nicht Gegenstand dieses Dokuments. Die Bedingungen solcher (etwa über den Marketplace angebotenen) Zusatzangebote müssen von Ihnen eigenverantwortlich geprüft werden, um sicherzustellen, dass sie Ihre Complianceanforderungen erfüllen.

Azure Government ist für juristische Personen verfügbar, die Daten verarbeiten, für die bestimmte gesetzliche Vorschriften und Auflagen wie etwa NIST 800.171 (DIB), ITAR, IRS 1075, DoD L4 und CJIS gelten, und die Azure Government zur Einhaltung von Vorschriften benötigen. Azure Government-Kunden müssen sich einer Eignungsprüfung unterziehen.

Fragen zur Eignung für Azure Government können an das Kontoteam gerichtet werden.

##  Prinzipien für den Schutz von Kundendaten in Azure Government

Azure Government bietet eine Reihe von Features und Diensten zur Erstellung von Cloudlösungen, die Ihre Anforderungen in puncto regulierte/kontrollierte Daten erfüllen. Eine konforme Kundenlösung ist nichts anderes als die effektive Implementierung der standardmäßig verfügbaren Azure Government-Funktionen, kombiniert mit fundierter Datensicherheit. Wenn Sie eine Lösung in Azure Government hosten, nimmt Ihnen Microsoft die Erfüllung zahlreicher Anforderungen auf der Cloudinfrastrukturebene ab.

Das folgende Diagramm zeigt das Defense-in-Depth-Modell von Azure. So bietet Microsoft etwa grundlegenden DDoS-Schutz für die Cloudinfrastruktur sowie Kundenfunktionen wie Sicherheitsappliances für kundenspezifische DDoS-Anforderungen für Anwendungen.

![alt text](./media/azure-government-Defenseindepth.png)

Diese Seite enthält Informationen zu den grundlegenden Prinzipien für den Schutz Ihrer Dienste und Anwendungen sowie Anleitungen und bewährte Methoden für die Umsetzung dieser Prinzipien. Kunden erfahren hier also, wie sie Azure Government optimal verwenden, um die Pflichten und Aufgaben im Zusammenhang mit einer Lösung zu erfüllen, die zur Verarbeitung von ITAR-Informationen verwendet wird.

Übergreifende Prinzipien für den Schutz von Kundendaten:
* Schutz von Daten mittels Verschlüsselung
* Verwaltung geheimer Schlüssel
* Isolierung zur Einschränkung des Datenzugriffs

##  Schutz von Kundendaten mittels Verschlüsselung

Die Eindämmung von Risiken und die Einhaltung gesetzlicher Vorschriften sind die treibenden Kräfte hinter dem wachsenden Fokus und der zunehmenden Bedeutung der Datenverschlüsselung. Verwenden Sie eine wirksame Verschlüsselungsimplementierung, um Ihre aktuellen Maßnahmen für Netzwerk- und Anwendungssicherheit zu verbessern und somit insgesamt das Risiko für Ihre Cloudumgebung zu senken.

### <a name="Overview"></a>Verschlüsselung ruhender Daten
Bei der Verschlüsselung ruhender Daten geht es um den Schutz von auf Datenträgern gespeicherten Kundeninhalten. Hierfür gibt es mehrere Möglichkeiten:

### <a name="Overview"></a>Storage Service Encryption

Azure Storage Service Encryption wird auf der Speicherkontoebene aktiviert, sodass Block- und Seitenblobs beim Schreiben in Azure Storage automatisch verschlüsselt werden. Wenn Sie die Daten aus Azure Storage lesen, werden sie vom Speicherdienst vor der Rückgabe entschlüsselt. Mit dieser Methode können Sie Ihre Daten ohne zusätzlichen Programmieraufwand schützen.

### <a name="Overview"></a>Azure Disk Encryption
Mit Azure Disk Encryption können Sie die Betriebssystemdatenträger und andere Datenträger verschlüsseln, die von einem virtuellen Azure-Computer verwendet werden. Die Integration in Azure Key Vault ermöglicht die Steuerung und Verwaltung der Schlüssel für die Datenträgerverschlüsselung.

### <a name="Overview"></a>Clientseitige Verschlüsselung
Die clientseitige Verschlüsselung ist in Java- und .NET-Speicherclientbibliotheken integriert. Diese können die Azure Key Vault-APIs verwenden, was eine unkomplizierte Implementierung ermöglicht. Verwenden Sie Azure Key Vault, um mithilfe von Azure Active Directory auf die geheimen Schlüssel bestimmter Benutzer in Azure Key Vault zuzugreifen.

### <a name="Overview"></a>Verschlüsselung während der Übertragung

Die für die Verbindung mit Azure Government verfügbare Basisverschlüsselung unterstützt das TLS 1.2-Protokoll (Transport Level Security) sowie X.509-Zertifikate. Für Infrastrukturnetzwerkverbindungen zwischen Azure Government-Datencentern werden auch FIPS 140-2-Kryptografiealgorithmen (Federal Information Processing Standard) der Stufe 1 verwendet. Windows Server 2012 R2 und Windows 8 sowie virtuelle Computer und Azure-Dateifreigaben können SMB 3.0 für die Verschlüsselung zwischen dem virtuellen Computer und der Dateifreigabe verwenden. Verwenden Sie die clientseitige Verschlüsselung, um die Daten zu verschlüsseln, bevor sie in den Speicher einer Clientanwendung übertragen werden, und sie nach der Übertragung aus dem Speicher wieder zu entschlüsseln.

### <a name="Overview"></a>Bewährte Methoden für die Verschlüsselung

* Virtuelle IaaS-Computer: Verwenden Sie Azure Disk Encryption. Aktivieren Sie Storage Service Encryption, um die VHD-Dateien zu verschlüsseln, mit denen die Datenträger in Azure Storage gesichert werden. Dadurch werden jedoch nur neu geschriebene Daten verschlüsselt. Wenn Sie also einen virtuellen Computer erstellen und dann Storage Service Encryption für das Speicherkonto aktivieren, das die VHD-Datei enthält, werden nur die Änderungen verschlüsselt, nicht die ursprüngliche VHD-Datei.
* Clientseitige Verschlüsselung: Dies ist die sicherste Methode zum Verschlüsseln Ihrer Daten, da die Verschlüsselung vor der Übertragung stattfindet und die Daten als ruhende Daten verschlüsselt werden. Allerdings müssen Sie hierzu Ihren Anwendungen, die den Speicher nutzen, Code hinzuzufügen, was vielleicht nicht unbedingt erwünscht ist. In diesen Fällen können Sie bei der Übertragung HTTPS für Ihre Daten verwenden, und Storage Service Encryption zum Verschlüsseln ruhender Daten. Die clientseitige Verschlüsselung stellt außerdem eine zusätzliche Belastung für den Client dar, was wiederum bei der Skalierbarkeitsplanung berücksichtigt werden muss. Dies gilt insbesondere, wenn Sie große Datenmengen verschlüsseln und übertragen.

Weitere Informationen zu den Verschlüsselungsoptionen in Azure finden Sie im [Azure Storage-Sicherheitsleitfaden](/storage-security-guide).

##  Schutz von Kundendaten durch die Verwaltung geheimer Schlüssel

Eine sichere Schlüsselverwaltung ist essenziell, um Daten in der Cloud zu schützen. Für Kunden empfiehlt es sich, die Schlüsselverwaltung möglichst einfach zu gestalten und die Kontrolle über die Schlüssel zu behalten, die von Cloudanwendungen und -diensten verwendet werden, um Daten zu verschlüsseln.

### <a name="Overview"></a>Bewährte Methoden für die Verwaltung geheimer Schlüssel

* Verwenden Sie Key Vault, um das Risiko zu minimieren, dass geheime Schlüssel durch hartcodierte Konfigurationsdateien, durch Skripts oder im Quellcode offengelegt werden. Azure Key Vault verschlüsselt Schlüssel (etwa die Verschlüsselungsschlüssel für Azure Disk Encryption) und geheime Schlüssel (beispielsweise Kennwörter) mittels Speicherung in FIPS 140-2-Hardwaresicherheitsmodulen (HSMs) der Stufe 2. Zur Steigerung der Sicherheit können Sie Schlüssel in diese HSMs importieren oder in diesen generieren.
* Anwendungscode und Vorlagen dürfen nur URI-Verweise auf die geheimen Schlüssel enthalten, sodass die tatsächlichen geheimen Schlüssel nicht im Code, in der Konfiguration oder in Quellcoderepositorys enthalten sind. Dies verhindert umfassende Phishing-Angriffe auf interne oder externe Repositorys, z. B. "harvest-bots" auf GitHub.
* Verwenden Sie innerhalb von Key Vault eine sichere RBAC-Steuerung. Ein vertrauenswürdiger Mitarbeiter, der aus dem Unternehmen ausscheidet oder innerhalb des Unternehmens in eine andere Gruppe wechselt, darf keinen Zugriff mehr auf die geheimen Schlüssel haben.

Weitere Informationen finden Sie unter [Key Vault for Azure Government](/azure-government/azure-government-tech-keyvault) (Schlüsseltresor für Azure Government).

##  Isolierung zur Einschränkung des Datenzugriffs

Bei der Isolierung geht es um die Einrichtung von Grenzen, Segmentierungen und Containern, um den Datenzugriff auf autorisierte Benutzer, Dienste und Anwendungen zu beschränken. So ist etwa die Trennung von Mandanten eine unverzichtbare Sicherheitsmaßnahme für mehrinstanzenfähigen Cloudplattformen wie Microsoft Azure. Eine logische Trennung trägt dazu bei, Konflikte zwischen Mandanten zu verhindern.

### <a name="Overview"></a>Isolierung der Umgebung
Bei der Azure Government-Umgebung handelt es sich um eine physische Instanz, die vom Rest des Microsoft-Netzwerks getrennt ist. Diese Trennung wird durch eine Reihe physischer und logischer Maßnahmen erreicht, so z.B. durch die Sicherung physischer Barrieren mithilfe von biometrischen Geräten und Kameras. Verwendung von spezifischen Anmeldeinformationen und mehrstufiger Authentifizierung durch Microsoft-Mitarbeiter, die logischen Zugriff auf die Produktionsumgebung benötigen. Die gesamte Dienstinfrastruktur für Azure Government befindet sich innerhalb der USA.

#### <a name="Overview"></a>Kundenspezifische Isolierung
Azure implementiert Netzwerkzugriffssteuerung und -trennung durch VLAN-Isolierung, ACLs, Lastenausgleichsmodule und IP-Filter.

Kunden können ihre Ressourcen für Abonnements, Ressourcengruppen, virtuelle Netzwerke und Subnetze noch weiter isolieren.

Weitere Informationen zur Isolierung in Microsoft Azure finden Sie im [Isolierungsabschnitt des Azure-Sicherheitsleitfadens](/azure-security-getting-started/#isolation).

Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.

<!---HONumber=AcomDC_0928_2016-->
<properties
   pageTitle="Empfohlene Vorgehensweisen für die Datensicherheit und die Verschlüsselung | Microsoft Azure"
   description="Dieser Artikel bietet eine Reihe von empfohlenen Vorgehensweisen für die Datensicherheit und Verschlüsselung unter Verwendung der integrierten Azure-Funktionen."
   services="security"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor="TomSh"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/23/2016"
   ms.author="yuridio"/>

#Empfohlene Vorgehensweisen für Datensicherheit und Verschlüsselung in Azure

Einer der Schlüssel zum Schutz von Daten in der Cloud ist die Berücksichtigung der möglichen Zustände, in denen Ihre Daten auftreten können. Außerdem sollten Sie die Steuerungsmöglichkeiten beachten, die für diesen Zustand verfügbar sind. Im Rahmen der empfohlenen Vorgehensweisen für Datensicherheit und Verschlüsselung in Azure befassen sich die Empfehlungen mit den folgenden Datenzuständen:

- Ruhende Daten: Dies umfasst alle Informationsspeicherobjekte, Container und Typen, die statisch in physischen Medien existieren, sei es auf Magnetplattenspeichern oder optischen Datenträgern.

- Während der Übertragung: Wenn Daten zwischen Komponenten, Speicherorten oder Programmen übertragen werden, beispielsweise über das Netzwerk, über einen Service Bus (von dem lokalen Computer in die Cloud und umgekehrt, inklusive Hybridverbindungen wie ExpressRoute) oder während eines Eingabe-/Ausgabevorganges, werden Sie als „in Bewegung befindlich“ betrachtet.

In diesem Artikel besprechen wir eine Reihe von empfohlenen Vorgehensweisen für Datensicherheit und Verschlüsselung in Azure. Diese empfohlenen Vorgehensweisen sind aus unseren Erfahrungen mit der Datensicherheit und der Verschlüsselung in Azure und den Erfahrungen von Kunden wie Ihnen abgeleitet.

Für jede empfohlene Vorgehensweise erklären wir:

- Wobei es bei der bewährten Methode geht
- Warum Sie die bewährte Methode nutzen sollten
- Was die Folge sein könnte, wenn Sie die bewährte Methode nicht aktivieren
- Mögliche Alternativen zur bewährten Methode
- Wie Sie erfahren können, wie Sie die empfohlenen Vorgehensweisen aktivieren

Dieser Artikel zu den empfohlenen Vorgehensweisen für die Datensicherheit und die Verschlüsselung in Azure basiert auf einer Konsensmeinung und den Fähigkeiten und Funktionssätzen der Azure-Plattform, wie sie zum Erstellungszeitpunkt dieses Artikels existieren. Meinungen und Technologien ändern sich im Laufe der Zeit. Dieser Artikel wird daher regelmäßig aktualisiert, um diese Änderungen widerzuspiegeln.

Die in diesem Artikel besprochenen empfohlenen Vorgehensweisen für Datensicherheit und Verschlüsselung in Azure umfassen:

- Erzwingen der Multi-Factor Authentication
- Verwenden der rollenbasierten Zugriffssteuerung (Role-Based Access Control; RBAC)
- Verschlüsseln virtueller Azure-Computer
- Verwenden von Hardware-Sicherheitsmodellen
- Verwalten mit sicheren Arbeitsstationen
- Aktivieren der SQL-Verschlüsselung
- Schützen der Daten während der Übertragung
- Erzwingen der Datenverschlüsselung auf Dateiebene


## Erzwingen der Multi-Factor Authentication

Der erste Schritt beim Datenzugriff und der Datensteuerung in Microsoft Azure ist die Authentifizierung des Benutzers. Die [Azure Multi-Factor Authentication (MFA)](./multi-factor-authentication/multi-factor-authentication.md) ist eine Methode zur Überprüfung der Identität eines Benutzers mithilfe einer weiteren Methode, als nur mittels Benutzername und Kennwort. Diese Authentifizierungsmethode schützt den Zugriff auf Daten und Anwendungen und erfüllt gleichzeitig die Anforderungen der Benutzer, die ein einfaches Anmeldeverfahren wünschen.

Durch die Aktivierung von Azure MFA für Ihre Benutzer fügen Sie eine zweite Sicherheitsebene für Benutzeranmeldungen und Transaktionen hinzu. In diesem Fall greift eine Transaktion möglicherweise auf ein Dokument zu, das sich auf einem Dateiserver oder in Ihrem SharePoint Online-Konto befindet. Azure MFA hilft der IT, die Wahrscheinlichkeit zu verringern, dass kompromittierte Anmeldeinformationen Zugriff auf die Daten einer Organisation erhalten.

Beispiel: Falls Sie Azure MFA für Ihre Benutzer erzwingen, und sie so konfigurieren, dass sie einen Telefonanruf oder eine SMS für die Überprüfung verwendet, ist es für den Angreifer im Falle der Kompromittierung von Benutzeranmeldeinformationen nicht möglich, auf Ressourcen zuzugreifen, da er keinen Zugriff auf das Handy des Benutzers hat. Organisationen, die diese zusätzliche Schicht zur Identitätssicherung nicht hinzufügen, sind anfälliger für Angriffe mit gestohlenen Anmeldeinformationen, was wiederum zu kompromittierten Daten führen kann.

Eine Alternative für Organisationen, die die Authentifizierung weiterhin lokal steuern möchten, ist die Verwendung des [Azure Multi-Factor Authentication-Server](./multi-factor-authentication/multi-factor-authentication-get-started-server.md), der auch als MFA lokal bezeichnet wird. Mithilfe dieser Methode können Sie weiterhin die Multi-Factor Authentication erzwingen und dabei den MFA-Server lokal halten.

Weitere Informationen über Azure Multi-Factor Authentication finden Sie unter [Erste Schritte mit Azure Multi-Factor Authentication in der Cloud](./multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## Verwenden der rollenbasierten Zugriffssteuerung (Role-Based Access Control; RBAC)
Schränken Sie den Zugriff auf Grundlage der Sicherheitsprinzipien [Need-to-know](https://en.wikipedia.org/wiki/Need_to_know)- und [least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege) (Prinzip der minimalen Berechtigung, Artikel in englischer Sprache) ein. Dies ist für Organisationen zwingend erforderlich, die Sicherheitsrichtlinien für den Datenzugriff durchsetzen möchten. Die rollenbasierte Zugriffssteuerung in Azure (RBAC) kann verwendet werden, um Benutzern, Gruppen und Anwendungen Berechtigungen für einen bestimmten Bereich zu erteilen. Der Bereich einer Rollenzuweisung kann ein Abonnement, eine Ressourcengruppe oder eine einzelne Ressource sein.

Sie können [integrierte RBAC-Rollen](./active-directory/role-based-access-built-in-roles.md) in Azure verwenden, um Benutzern Berechtigungen zuzuweisen. Ziehen Sie die Verwendung von *Speicherkontomitwirkender* für Cloudoperatoren in Betracht, die Speicherkonten verwalten müssen und die Rolle *Klassischer Speicherkontomitwirkender *, um klassische Speicherkonten zu verwalten. Überlegen Sie sich, ob Sie Cloudoperatoren, die VMs und Speicherkonten verwalten müssen, zur Rolle *Mitwirkender für virtuelle Computer* hinzufügen.

Organisationen, die keine Datenzugriffssteuerung mithilfe von Funktionen wie RBAC erzwingen, erteilen Ihren Benutzern möglicherweise mehr Berechtigungen als erforderlich. Dies kann zur Kompromittierung der Daten führen, da einige Benutzer Zugriff auf Daten erhalten, auf die sie von vorneherein keinen Zugriff haben sollten.

Lesen Sie den Artikel [Rollenbasierte Access Control in Azure](./active-directory/role-based-access-control-configure.md), um mehr über die Azure RBAC zu erfahren.

## Verschlüsseln virtueller Azure-Computer
Für viele Organisationen ist die Verschlüsselung von [ruhenden Daten](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) ein obligatorischer Schritt in Richtung Datenschutz, Compliance und Datenhoheit. Azure Disk Encryption ermöglicht IT-Administratoren, die Datenträger virtueller Windows- und Linux-IaaS-Computer (VMs) zu verschlüsseln. Bei der Azure Disk Encryption werden die Branchenstandardfunktion BitLocker von Windows und die Funktion DM-Crypt von Linux verwendet, um Volumeverschlüsselung für das Betriebssystem und die Datenträger bereitzustellen.

Sie können Azure Disk Encryption verwenden, um den Schutz Ihrer Daten zu verbessern und damit die Sicherheits- und Compliance-Anforderungen Ihrer Organisation zu erfüllen. Organisationen sollten auch die Verwendung von Verschlüsselungen in Betracht ziehen, um das Risiko des unberechtigten Datenzugriffs zu reduzieren. Es empfiehlt sich auch, Laufwerke zu verschlüsseln, bevor sie mit sensiblen Daten beschrieben werden.

Stellen Sie sicher, dass Sie die Datenvolumes Ihrer VM und das Startvolume verschlüsseln, um die ruhenden Daten in Ihrem Azure-Speicherkonto zu schützen. Schützen Sie die Verschlüsselungsschlüssel und geheimen Schlüssel durch die Verwendung von [Azure Key Vault](./key-vault/key-vault-whatis.md).

Berücksichtigen Sie für die lokalen Windows-Server folgende empfohlene Vorgehensweisen für die Verschlüsselung:

- Verwenden von [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) für die Datenverschlüsselung
- Speichern von Wiederherstellungsinformationen in AD DS.
- Falls es Grund zur Annahme gibt, dass die BitLocker-Schlüssel kompromittiert wurden, empfehlen wir Ihnen,das Laufwerk entweder neu zu formatieren, um alle BitLocker-Metadaten von dem Laufwerk zu entfernen, oder das gesamte Laufwerk zu ent- und anschließend erneut zu verschlüsseln.

Organisationen, die keine Datenverschlüsselung erzwingen, sehen sich eher Datenintegritätsproblemen gegenüber, beispielsweise, dass böswillige oder nicht autorisierte Benutzer Daten stehlen und dass kompromittierte Konten unautorisierte Zugriffe auf unverschlüsselte Daten erhalten. Neben den Risiken müssen Unternehmen den Branchenbestimmungen entsprechen und beweisen, dass sie gewissenhaft sind und die richtigen Sicherheitsprotokolle verwenden, um die Datensicherheit zu verbessern.

Erfahren Sie mehr über die Azure Disk Encryption, indem Sie den Artikel [Azure Disk Encryption für virtuelle Windows- und Linux-IaaS-Computer](azure-security-disk-encryption.md) lesen.

## Verwenden von Hardware-Sicherheitsmodellen

Branchenverschlüsselungslösungen verwenden für die Verschlüsselung von Daten geheime Schlüssel. Deshalb ist es sehr wichtig, dass diese Schlüssel sicher gespeichert werden. Das Verwalten von Schlüsseln wird zu einem integralen Bestandteil des Datenschutzes, da mithilfe der Schlüsselverwaltung geheime Schlüssel gespeichert werden, die wiederum zur Datenverschlüsselung dienen.

Azure Disk Encryption verwendet [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), um Ihnen bei der Steuerung und Verwaltung von Datenträger-Verschlüsselungsschlüsseln und geheimen Schlüsseln in Ihrem Schlüsseltresor-Abonnement zu helfen. Dabei wird gleichzeitig sichergestellt, dass alle ruhenden Daten auf den Laufwerken der virtuellen Computer in Ihrem Azure-Speicher verschlüsselt sind. Sie sollten Azure Key Vault verwenden, um die Verwendung der Schlüssel und der Richtlinien zu überwachen.

Es gibt viele inhärente Risiken im Zusammenhang mit dem Fehlen passender Sicherheitsprotokolle zum Schutz der geheimen Schlüssel, die für die Verschlüsselung Ihrer Daten verwendet wurden. Falls die Angreifer über Zugriff auf die geheimen Schlüssel verfügen, ist es ihnen möglich, die Daten zu entschlüsseln und potenziell auf die vertraulichen Informationen zuzugreifen.

Weitere Informationen zu allgemeinen Empfehlungen für die Zertifikatverwaltung in Azure erfahren Sie im Artikel [Certificate Management in Azure: Do‘s and Don’ts](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/) (Zertifikatverwaltung in Azure: Gebote und Verbote).

Weitere Informationen zu Azure Key Vault erhalten Sie unter [Erste Schritte mit dem Azure Key Vault](./key-vault/key-vault-get-started.md).

## Verwalten mit sicheren Arbeitsstationen

Da die große Mehrzahl der Angriffe auf den Endbenutzer zielt, ist der Endpunkt einer der Hauptangriffspunkte. Falls ein Angreifer den Endpunkt kompromittiert hat, kann er die Anmeldeinformationen des Benutzers verwenden, um Zugriff auf die Daten der Organisation zu erhalten. Die meisten Angriffe auf Endpunkte können sich zunutze machen, dass Endbenutzer Administratoren in ihren lokalen Arbeitsstationen sind.

Sie können das Risiko minimieren, indem Sie eine sichere Verwaltungsarbeitsstation verwenden. Wir empfehlen die Verwendung einer der [Privileged Access Workstations (PAW)](https://technet.microsoft.com/library/mt634654.aspx) (Arbeitsstationen mit privilegiertem Zugriff) zum Reduzieren der Angriffsfläche auf Arbeitsstationen. Diese sicheren Verwaltungsarbeitsstationen helfen Ihnen, einige dieser Angriffe zu minimieren und damit Ihre Daten sicherer zu machen. Stellen Sie sicher, dass Sie PAW verwenden, um Ihre Arbeitsstation zu sichern und zu sperren. Das ist ein wichtiger Schritt für die Gewährung von hohen Sicherheitszusagen für sensible Konten, Aufgaben und den Datenschutz.

Das Fehlen von Endpunktschutz gefährdet möglicherweise Ihre Daten. Erzwingen Sie daher die Einhaltung der Sicherheitsrichtlinien auf allen Geräten, die die Daten verwenden, unabhängig vom Speicherort der Daten (in der Cloud oder lokal).

Erfahren Sie mehr über die Arbeitsstationen mit privilegiertem Zugriff, indem Sie den Artikel [Securing Privileged Access](https://technet.microsoft.com/library/mt631194.aspx) (Sichern des privilegierten Zugriffs) lesen.

## Aktivieren der SQL-Verschlüsselung

[Azure SQL-Datenbank Transparent Data Encryption](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) bietet Schutz vor der Bedrohung durch böswillige Aktivitäten. Hierzu werden die Datenbank, die dazugehörigen Sicherungen und die Transaktionsprotokolldateien im Ruhezustand in Echtzeit ver- und entschlüsselt, ohne dass Änderungen der Anwendung erforderlich sind. TDE verschlüsselt die Speicherung einer gesamten Datenbank, indem ein symmetrischer Schlüssel verwendet wird, der als Datenbankverschlüsselungsschlüssel bezeichnet wird.

Auch wenn der gesamte Speicher verschlüsselt ist, ist es sehr wichtig, dass Sie Ihre Datenbank selbst ebenfalls verschlüsseln. Dies ist eine Implementierung des „Defense-in-Depth-Ansatzes“ (mehrschichtige Strategie) für den Datenschutz. Falls Sie die [Azure SQL-Datenbank](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) verwenden und sensible Daten wie beispielsweise Kreditkarteninformationen oder US-Sozialversicherungsnummern sichern möchten, können Sie Datenbanken mithilfe der FIPS 140-2-zertifizierten AES-256-Bit-Verschlüsselung verschlüsseln. Diese erfüllt die Anforderungen vieler Branchenstandards (z.B. HIPAA oder PCI).

Es ist wichtig zu verstehen, dass Dateien, die im Zusammenhang mit der [Pufferpoolerweiterung](https://msdn.microsoft.com/library/dn133176.aspx) (buffer pool extension; BPE) stehen, nicht verschlüsselt werden, wenn eine Datenbank mithilfe von TDE verschlüsselt wird. Sie müssen Dateisystemebenen-Verschlüsselungstools wie BitLocker oder das [Encrypting File System](https://technet.microsoft.com/library/cc700811.aspx) (verschlüsselnde Dateisystem; EFS) für mit der Pufferpoolerweiterung in Zusammenhang stehende Dateien verwenden.

Da ein autorisierter Benutzer, wie beispielsweise ein Sicherheits- oder Datenbankadministrator, auf die Daten zugreifen kann, selbst wenn Sie mit TDE verschlüsselt sind, sollten Sie auch die folgenden Empfehlungen befolgen:

- SQL-Authentifizierung auf Datenbankebene
- Azure AD-Authentifizierung mithilfe von RBAC-Rollen
- Benutzer und Anwendungen sollten verschiedenen Konten verwenden, um sich zu authentifizieren. Auf diese Weise können Sie die Anzahl von an Benutzer und Anwendungen verliehenen Berechtigungen beschränken und das Risiko böswilliger Aktivitäten reduzieren
- Implementieren Sie Sicherheitsmaßnahmen auf Datenbankebene, indem Sie feste Datenbankrollen (wie db\_datareader oder db\_datawriter) verwenden oder benutzerdefinierte Rollen für Ihre Anwendung erstellen, um explizite Berechtigungen für ausgewählte Datenbankobjekte zu gewähren.

Organisationen, die keine Verschlüsselung auf Datenbankebene verwenden, sind möglicherweise anfälliger für Angriffe, die in SQL-Datenbanken gespeicherte Daten kompromittieren.

Erfahren Sie mehr über die SQL-TDE-Verschlüsselung im Artikel [Azure SQL-Datenbank Transparent Data Encryption](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## Schützen der Daten während der Übertragung

Der Schutz von Daten während der Übertragung sollte ein wesentlicher Bestandteil Ihrer Datenschutzstrategie sein. Da die Daten zwischen vielen verschiedenen Speicherorten übertragen werden, empfiehlt es sich im Allgemeinen, immer SSL/TLS-Protokolle zu verwenden, um Daten zwischen verschiedenen Speicherorten auszutauschen. In einigen Fällen sollten Sie den gesamten Kommunikationskanal zwischen Ihrer lokalen und Ihrer Cloudinfrastruktur isolieren, indem Sie ein virtuelles privates Netzwerk (VPN) verwenden.

Für Daten, die sich zwischen Ihrer lokalen Infrastruktur und Azure bewegen, sollten Sie passende Sicherheitsmaßnahmen in Betracht ziehen, beispielsweise HTTPS oder VPN.

Verwenden Sie eine [Azure-Site-to-Site-VPN-Verbindung ](./vpn-gateway/vpn-gateway-site-to-site-create.md) für Organisationen, die den Zugriff von mehreren lokalen Arbeitsstationen an Azure sichern müssen.

Verwenden Sie eine [Punkt-zu-Standort-VPN-Verbindung](./vpn-gateway/vpn-gateway-point-to-site-create.md) für Organisationen, die den Zugriff von einer lokalen Arbeitsstation an Azure sichern müssen.

Größere Datasets können über dedizierte Hochgeschwindigkeits-WAN-Verbindungen wie [ExpressRoute](https://azure.microsoft.com/services/expressroute/) verschoben werden. Falls Sie sich für die Verwendung von ExpressRoute entscheiden, können Sie die Daten auch auf Anwendungsebene verschlüsseln, indem Sie [SSL/TLS](https://support.microsoft.com/kb/257591) oder andere Protokolle für den zusätzlicher Schutz verwenden.

Falls Sie mit Azure Storage über das Azure-Portal interagieren, werden alle Transaktionen über HTTPS durchgeführt. [Speicher-REST-API](https://msdn.microsoft.com/library/azure/dd179355.aspx) über HTTPS kann auch verwendet werden, um mit [Azure Storage](https://azure.microsoft.com/services/storage/) und [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/) zu interagieren.

Organisationen, die die Daten während der Übertragung nicht schützen, sind anfälliger für [Man-in-the-Middle-Angriffe](https://technet.microsoft.com/library/gg195821.aspx), [Abhöraktionen](https://technet.microsoft.com/library/gg195641.aspx) und Session Hijacking. Bei diesen Angriffen kann es sich um den ersten Schritt zur Zugriffsgewinnung auf vertrauliche Daten handeln.

Erfahren Sie mehr über die Azure-VPN-Option im Artikel [Planning and design for VPN Gateway](./vpn-gateway/vpn-gateway-plan-design.md) (Planen und Entwerfen eines VPN-Gateways).

## Erzwingen der Datenverschlüsselung auf Dateiebene

Eine weitere Schutzschicht, die die Sicherheit Ihrer Daten erhöhen kann, ist das Verschlüsseln der Datei selbst, unabhängig von ihrem Speicherort.

[Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) verwendet Verschlüsselungs-, Identitäts- und Autorisierungsrichtlinien, um Ihre Dateien und E-Mails zu schützen. Azure RMS funktioniert über mehrere Geräte (Handys, Tablets und PCs), indem Schutz sowohl innerhalb als auch außerhalb Ihrer Organisation geboten wird. Diese Funktion ist möglich, weil Azure RMS eine Schutzebene hinzufügt, die bei den Daten verbleibt, selbst wenn diese die Grenzen Ihrer Organisation verlassen.

Wenn Sie Azure RMS verwenden, um Ihre Dateien zu schützen, verwenden Sie eine Kryptographie, die dem Branchenstandard entspricht und [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html) voll unterstützt. Wenn Sie Azure RMS für den Datenschutz einsetzen, haben Sie die Garantie, dass die Datei geschützt bleibt, auch wenn diese in einen Speicher kopiert wird, der nicht von der IT kontrolliert wird, beispielsweise einem Cloudspeicherdienst. Dasselbe gilt für Dateien, die mittels E-Mail freigegeben werden. Die Datei ist als Anhang der E-Mail geschützt, und es wird eine Anleitung mitgeschickt, die erklärt, wie der geschützt Anhang geöffnet wird.

Bei der Planung der Einführung von Azure RMS empfehlen wir Folgendes:

- Installieren Sie die [RMS-Freigabe-App](https://technet.microsoft.com/library/dn339006.aspx). Diese App integriert sich mit Office-Anwendungen, indem sie ein Office-Add-In installiert, damit die Benutzer die Dateien einfach direkt schützen können.
- Konfigurieren Sie Ihre Anwendungen und Dienste für Azure RMS
- Erstellen Sie [benutzerdefinierte Vorlagen](https://technet.microsoft.com/library/dn642472.aspx), entsprechend Ihrer Geschäftsanforderungen. Beispiel: Eine Vorlage für streng geheime Daten, die für alle E-Mails im Zusammenhang mit streng geheimen Informationen verwendet werden soll.

Organisationen, die in Hinsicht auf die [Datenklassifizierung](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) (PDF-Dokument in englischer Sprache) und den Dateischutz Schwächen haben, sind möglicherweise anfälliger für Datenlecks. Ohne richtigen Dateischutz ist es Organisationen nicht möglich, Einblicke in das Unternehmen zu erhalten, eine Überwachung in Hinsicht auf Missbrauch durchzuführen und böswilligen Zugriff auf Dateien zu verhindern.

Erfahren Sie mehr über Azure RMS, indem Sie den Artikel [Erste Schritte mit Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx) lesen.

<!---HONumber=AcomDC_0525_2016-->
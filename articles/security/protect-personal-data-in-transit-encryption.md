---
title: "Schützen personenbezogener Daten während der Übertragung mithilfe von Verschlüsselung in Azure | Microsoft-Dokumentation"
description: "Verwenden der Verschlüsselung in Azure zum Schützen personenbezogener Daten"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 92c37c267afa27854f66b870f69b47195e388152
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a>Azure-Verschlüsselungstechnologien: Schützen personenbezogener Daten während der Übertragung mithilfe von Verschlüsselung

Dieser Artikel enthält grundlegende Informationen zur Verwendung von Azure-Verschlüsselungstechnologien zum Sichern von Daten während der Übertragung. 

Der Datenschutz personenbezogener Daten während der Übertragung im Netzwerk ist ein wesentlicher Teil einer mehrschichtigen Defense-in-Depth-Sicherheitsstrategie. Die Verschlüsselung während der Übertragung wurde entwickelt, um zu verhindern, dass Angreifer, die Übertragungen abfangen, die Daten anzeigen oder verwenden können.

## <a name="scenario"></a>Szenario

Ein großes Kreuzfahrtunternehmen mit Hauptsitz in den USA expandiert und bietet Routen im Mittelmeer, der Adria und der Ostsee sowie zu den Britischen Inseln an. Zu diesem Zweck hat das Unternehmen mehrere kleinere Kreuzfahrtunternehmen in Italien, Deutschland, Dänemark und Großbritannien erworben. 

Das Unternehmen verwendet Microsoft Azure zum Speichern von Unternehmensdaten in der Cloud. Dies umfasst personenbezogene Informationen, z.B. Namen, Adressen, Telefonnummern und Kreditkarteninformationen, in seiner globalen Kundendatenbank. Dies umfasst außerdem herkömmliche Informationen der Personalverwaltung wie Adressen, Telefonnummern, Steueridentifikationsnummern und andere Informationen zu Unternehmensmitarbeitern an allen Standorten. Das Kreuzfahrtunternehmen verwaltet zudem eine umfangreiche Datenbank für Mitglieder des Bonus- und Treueprogramms, die personenbezogene Informationen enthält, um Beziehungen zu aktuellen und früheren Kunden nachzuverfolgen.

Personenbezogene Daten von Kunden werden in den externen Geschäftsstellen des Unternehmens und von Reiseveranstaltern weltweit in die Datenbank eingegeben. Dokumente mit Kundeninformationen werden über das Netzwerk in Azure Storage übertragen.

## <a name="problem-statement"></a>Problembeschreibung

Das Unternehmen muss den Datenschutz der personenbezogenen Daten von Kunden und Mitarbeitern gewährleisten, während die Daten in und aus Azure-Diensten übertragen werden.

## <a name="company-goal"></a>Unternehmensziel

Unternehmensziel ist es, sicherzustellen, dass personenbezogene Daten während ihrer Übertragung verschlüsselt werden. Wenn nicht autorisierte Personen die personenbezogenen Daten während der Übertragung abfangen, müssen diese in einer nicht lesbaren Form vorliegen. Die Anwendung der Verschlüsselung muss einfach sein und vollständig im Hintergrund erfolgen – für die Benutzer und die Administratoren.

## <a name="solutions"></a>Lösungen

Azure-Dienste umfassen mehrere Tools und Technologien, mit denen Sie personenbezogene Daten während der Übertragung schützen können.

### <a name="azure-storage"></a>Azure Storage

Die in der Cloud gespeicherten Daten müssen vom Client, der sich an einem beliebigen physischen Standort auf der Welt befinden kann, in das Azure-Rechenzentrum übertragen werden. Wenn diese Daten von Benutzern abgerufen werden, werden sie wieder übertragen, in die entgegengesetzte Richtung. Daten, die über das öffentliche Internet übertragen werden, bergen immer das Risiko, von Angreifern abgefangen zu werden. Es ist daher wichtig, den Datenschutz personenbezogener Daten zu gewährleisten, indem diese bei der Übertragung zwischen Standorten mithilfe der Verschlüsselung auf Transportebene gesichert werden.

Das HTTPS-Protokoll bietet einen sicheren, verschlüsselten Kommunikationskanal im Internet. HTTPS sollte für den Zugriff auf Objekte in Azure Storage und beim Aufruf von REST-APIs verwendet werden. Sie können die Verwendung des HTTPS-Protokolls erzwingen, wenn Sie [Shared Access Signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) zum Delegieren des Zugriffs auf Azure Storage-Objekte verwenden. Es gibt zwei Arten von SAS: Dienst-SAS und Konto-SAS.

#### <a name="how-do-i-construct-a-service-sas"></a>Wie erstelle ich eine Dienst-SAS?

Eine Dienst-SAS delegiert den Zugriff auf eine Ressource in nur einem der Speicherdienste (Blob Storage, Queue Storage, Table Storage oder File Storage). Führen Sie folgende Schritte aus, um eine Dienst-SAS zu erstellen:

1. Angeben des signierten Versionsfelds

2. Angeben der signierten Ressource (nur Blob- und Dateidienst)

3. Angeben von Abfrageparametern zum Überschreiben von Antwortheadern (nur Blob- und Dateidienst)

4. Angeben des Tabellennamens (nur Tabellenspeicherdienst)

5. Angeben der Zugriffsrichtlinie

6. Angeben des Intervalls für die Signaturgültigkeit

8. Angeben von Berechtigungen

9. Angeben der IP-Adresse oder des IP-Bereichs

10. Angeben des HTTP-Protokolls

11. Angeben von Tabellenzugriffsbereichen

12. Angeben des signierten Bezeichners

13. Angeben der Signatur

Ausführlichere Anweisungen finden Sie unter [Erstellen einer Dienst-SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).

#### <a name="how-do-i-construct-an-account-sas"></a>Wie erstelle ich eine Konto-SAS?

Eine Konto-SAS delegiert den Zugriff auf Ressourcen in einem oder mehreren der Speicherdienste. Sie können auch den Zugriff auf Lese-, Schreib- und Löschvorgänge in Blob-Containern, Tabellen, Warteschlangen und Dateifreigaben delegieren, die mit einer Dienst-SAS nicht zulässig sind. Die Erstellung einer Konto-SAS ähnelt der einer Dienst-SAS. Ausführliche Anweisungen finden Sie unter [Erstellen einer Konto-SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN).

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a>Wie erzwinge ich HTTPS beim Aufruf von REST-APIs?

Um die Verwendung von HTTPS beim Aufruf von REST-APIs für den Zugriff auf Objekte in Speicherkonten zu erzwingen, können Sie die Option „Sichere Übertragung erforderlich“ für das Speicherkonto aktivieren. 

1. Wählen Sie im Azure-Portal die Option **Speicherkonto erstellen** oder für ein vorhandenes Speicherkonto die Option **Einstellungen** und dann **Konfiguration** aus.

2. Wählen Sie unter **Sichere Übertragung erforderlich** die Option **Aktiviert** aus.

![Erstellen eines Speicherkontos](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

Ausführlichere Anweisungen, einschließlich Informationen zur programmgesteuerten Aktivierung von „Sichere Übertragung erforderlich“ finden Sie unter [Vorschreiben einer sicheren Übertragung](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a>Wie verschlüssle ich Daten in Azure File Storage?

Zum Verschlüsseln von Daten während der Übertragung mit [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), können Sie SMB 3.x mit Windows 8, 8.1 und 10 sowie mit Windows Server 2012 R2 und Windows Server 2016 verwenden. Wenn Sie den Azure Files-Dienst verwenden, schlägt jede Verbindung ohne Verschlüsselung fehl, wenn „Sichere Übertragung erforderlich“ aktiviert ist. Hierzu zählen Szenarien, in denen SMB 2.1, SMB 3.0 ohne Verschlüsselung und einige Varianten des Linux SMB-Clients verwendet werden.

#### <a name="azure-client-side-encryption"></a>Clientseitige Verschlüsselung in Azure

Eine weitere Option zum Schützen personenbezogener Daten während der Übertragung zwischen einer Clientanwendung und Azure Storage ist die [clientseitige Verschlüsselung](https://docs.microsoft.com/azure/storage/storage-client-side-encryption). Die Daten werden verschlüsselt, bevor sie in Azure Storage übertragen werden. Wenn Sie die Daten aus Azure Storage abrufen, werden sie nach dem Empfang auf der Clientseite entschlüsselt.

### <a name="azure-site-to-site-vpn"></a>Azure-Site-to-Site-VPN

Eine effektive Methode zum Schutz personenbezogener Daten während der Übertragung zwischen einem Unternehmensnetzwerk oder Benutzer und dem virtuellen Azure-Netzwerk ist die Verwendung eines virtuellen privaten [Site-to-Site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)- oder [Point-to-Site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)-Netzwerks (VPN). Bei einer VPN-Verbindung wird ein sicherer verschlüsselter Tunnel im Internet erstellt.

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a>Wie erstelle ich eine Site-to-Site-VPN-Verbindung?

Über ein Site-to-Site-VPN werden mehrere Benutzer im Unternehmensnetzwerk mit Azure verbunden. Führen Sie folgende Schritte aus, um eine Standort-zu-Standort-Verbindung im Azure-Portal zu erstellen:

1. Erstellen Sie ein virtuelles Netzwerk.

2. Geben Sie einen DNS-Server an.

3. Erstellen Sie das Gatewaysubnetz.

4. Erstellen Sie das VPN-Gateway. 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. Erstellen Sie das Gateway des lokalen Netzwerks.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. Konfigurieren Sie das VPN-Gerät.

7. Erstellen Sie die VPN-Verbindung.

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. Überprüfen Sie die VPN-Verbindung.

Ausführlichere Anweisungen zum Erstellen einer Site-to-Site-Verbindung im Azure-Portal finden Sie unter [Erstellen einer Site-to-Site-Verbindung im Azure-Portal] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal).

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a>Wie erstelle ich eine Point-to-Site-VPN-Verbindung?

In einem Point-to-Site-VPN wird eine sichere Verbindung von einem einzelnen Clientcomputer mit einem virtuellen Netzwerk hergestellt. Dies ist nützlich, wenn Sie von einem Remotestandort, z.B. von zu Hause oder aus einem Hotel oder Konferenzzentrum, eine Verbindung mit Azure herstellen möchten. Führen Sie folgende Schritte aus, um eine Point-to-Site-Verbindung im Azure-Portal zu erstellen:

1. Erstellen Sie ein virtuelles Netzwerk.

2. Fügen Sie ein Gatewaysubnetz hinzu.

3. Geben Sie einen DNS-Server an. (Optional)

4. Erstellen Sie ein Gateway für das virtuelle Netzwerk.

5. Generieren Sie Zertifikate.

6. Fügen Sie den Clientadresspool hinzu.

7. Laden Sie die Daten des öffentlichen Zertifikats für das Stammzertifikat hoch.

8. Generieren und installieren Sie das Konfigurationspaket für VPN-Clients.

9. Installieren Sie ein exportiertes Clientzertifikat.

10. Herstellen einer Verbindung zu Azure.

11. Überprüfen Sie die Verbindung.

Ausführlichere Anweisungen finden Sie unter [Konfigurieren einer Point-to-Site-Verbindung mit einem VNET unter Verwendung der Zertifikatauthentifizierung: Azure-Portal](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal).

### <a name="ssltls"></a>SSL/TLS

Microsoft empfiehlt, dass Sie für den Austausch von Daten zwischen verschiedenen Standorten immer SSL/TLS-Protokolle verwenden. Organisationen, die sich für die Verwendung von [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) zum Verschieben umfangreicher DataSets über dedizierte Hochgeschwindigkeits-WAN-Verbindungen entscheiden, können die Daten auch auf Anwendungsebene verschlüsseln, indem sie SSL/TLS oder andere Protokolle für zusätzlichen Schutz verwenden.

### <a name="encryption-by-default"></a>Standardmäßige Verschlüsselung

Microsoft verwendet die Verschlüsselung zum Schutz von Daten während der Übertragung zwischen Kunden und Azure-Clouddiensten. Falls Sie mit Azure Storage über das Azure-Portal interagieren, werden alle Transaktionen über HTTPS durchgeführt.

[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) ist das Protokoll, das Microsoft-Rechenzentren mit Clientsystemen auszuhandeln versuchen, die eine Verbindung mit Microsoft-Clouddiensten herstellen. TLS bietet strenge Authentifizierung, Datenschutz von Nachrichten und Integrität (ermöglicht die Erkennung von Manipulation, Abfangen und Fälschung von Nachrichten), Interoperabilität, Algorithmusflexibilität sowie einfache Bereitstellung und Verwendung.

Zudem wird [Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) verwendet, sodass für die einzelnen Verbindungen zwischen den Clientsystemen von Kunden und den Clouddiensten von Microsoft eindeutige Schlüssel verwendet werden. Bei Verbindungen mit Microsoft-Clouddiensten werden zudem RSA-basierte Verschlüsselungsschlüssellängen von 2.048 Bit verwendet. Die Kombination aus TLS, RSA-basierten Schlüssellängen von 2.048 Bit und PFS erschwert das Abfangen und den Zugriff auf Daten während der Übertragung zwischen Microsoft-Clouddiensten und Kunden erheblich.

Daten während der Übertragung werden immer in [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview) verschlüsselt. Zusätzlich zur Verschlüsselung von Daten vor dem Speichern auf persistenten Medien werden Daten auch während der Übertragung immer über HTTPS geschützt. Für die Data Lake Store-REST-Schnittstellen wird ausschließlich das HTTPS-Protokoll unterstützt.

## <a name="summary"></a>Zusammenfassung

Das Unternehmen kann sein Ziel der Sicherung personenbezogener Daten und der Gewährleistung des Datenschutzes dieser Daten erreichen, indem HTTPS-Verbindungen mit Azure Storage mithilfe von Shared Access Signatures erzwungen werden und „Sichere Übertragung erforderlich“ für die Speicherkonten aktiviert wird. Das Unternehmen kann personenbezogene Daten zudem durch Verwenden von SMB 3.0-Verbindungen und Implementieren clientseitiger Verschlüsselung schützen. Mit Site-to-Site-VPN-Verbindungen vom Unternehmensnetzwerk mit dem virtuellen Azure-Netzwerk sowie mit Point-to-Site-VPN-Verbindungen von einzelnen Benutzern wird ein sicherer Tunnel erstellt, über den personenbezogene Daten sicher übertragen werden können. Ferner gewährleisten die Standardverschlüsselungsmethoden von Microsoft den Datenschutz personenbezogener Daten.

## <a name="next-steps"></a>Nächste Schritte

- [Empfohlene Vorgehensweisen für Datensicherheit und Verschlüsselung in Azure](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [Planung und Entwurf für VPN Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [Häufig gestellte Fragen zum VPN-Gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [Kaufen und Konfigurieren eines SSL-Zertifikats für Ihren Azure App Service](https://docs.microsoft.com/azure/app-service-web/web-sites-purchase-ssl-web-site)


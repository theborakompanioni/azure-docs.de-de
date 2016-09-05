<properties 
   pageTitle="Erstellen selbstsignierter Zertifikate für standortübergreifende Punkt-zu-Standort-Verbindungen mit einem virtuellen Netzwerk mithilfe von „makecert“ | Microsoft Azure"
   description="In diesem Artikel werden die Schritte erläutert, die zum Erstellen selbstsignierter Zertifikate unter Windows 10 mithilfe von „makecert“ erforderlich sind."
   services="vpn-gateway"
   documentationCenter="na"
   authors="cherylmc"
   manager="carmonm"
   editor=""
   tags="azure-resource-manager"/>
<tags 
   ms.service="vpn-gateway"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="08/15/2016"
   ms.author="cherylmc" />

# Arbeiten mit selbstsignierten Zertifikaten für Punkt-zu-Standort-Verbindungen

Dieser Artikel unterstützt Sie dabei, mithilfe von „makecert“ ein selbstsigniertes Zertifikat zu erstellen und aus diesem Clientzertifikate zu generieren. Die Schritte gelten für „makecert“ unter Windows 10. „makecert“ wurde für die Erstellung von Zertifikaten validiert, die mit Punkt-zu-Standort-Verbindungen kompatibel sind.

Bei Punkt-zu-Standort-Verbindungen ist die bevorzugte Methode für Zertifikate die Verwendung Ihrer unternehmenseigenen Zertifikatlösung. Stellen Sie dabei sicher, dass Sie die Clientzertifikate im allgemeinen Format für Namenswerte „Name@Ihre\_Domäne.com“ erstellen, nicht im Format „NetBIOS-Domänenname\\Benutzername“.

Wenn Sie nicht über eine Unternehmenslösung verfügen, ist ein selbstsigniertes Zertifikat erforderlich, um Punkt-zu-Standort-Clients die Verbindung mit einem virtuellen Netzwerk zu ermöglichen. Wir wissen, dass „makecert“ veraltet ist, es ist aber immer noch eine gültige Methode zum Erstellen selbstsignierter Zertifikate, die mit Punkt-zu-Standort-Verbindungen kompatibel sind.

## Erstellen eines selbstsignierten Zertifikats

„makecert“ ist eine Möglichkeit zum Erstellen eines selbstsignierten Zertifikats. Die folgenden Schritte führen Sie durch die Erstellung eines selbstsignierten Zertifikats mithilfe von „makecert“. Diese Schritte gelten nicht spezifisch für ein Bereitstellungsmodell. Sie sind für das Ressourcen-Manager-Modell und das klassische Modell gültig.

### So erstellen Sie ein selbstsigniertes Zertifikat

1. Laden Sie auf einem Windows 10-Computer das [Windows Software Development Kit (SDK) für Windows 10](https://dev.windows.com/de-DE/downloads/windows-10-sdk) herunter, und installieren Sie es.

2. Nach der Installation finden Sie das Hilfsprogramm „makecert.exe“ unter folgendem Pfad: C:\\Programme (x86)\\Windows Kits\\10\\bin<arch>.
		
	Beispiel:
	
		C:\Program Files (x86)\Windows Kits\10\bin\x64\makecert.exe

3. Als Nächstes erstellen und installieren Sie ein Zertifikat im Speicher für persönliche Zertifikate auf dem Computer. Das folgende Beispiel erstellt eine entsprechende *CER*-Datei, die Sie beim Konfigurieren der Punkt-zu-Standort-Verbindung in Azure hochladen. Führen Sie den folgenden Befehl als Administrator aus, wobei *CertificateName* der Name ist, den Sie für das Zertifikat verwenden möchten.<br><br>Wenn Sie das folgende Beispiel unverändert ausführen, erhalten Sie ein Zertifikat und die entsprechende Datei namens *CertificateName.cer*. Die CER-Datei finden Sie in dem Verzeichnis, in dem Sie den Befehl ausgeführt haben. Das Zertifikat befindet sich unter „Zertifikate - Aktueller Benutzer\\Eigene Zertifikate\\Zertifikate“.

    	makecert -sky exchange -r -n "CN=CertificateName" -pe -a sha1 -len 2048 -ss My "CertificateName.cer"

4. Das selbstsignierte Zertifikat wird zum Erstellen von Clientzertifikaten verwendet. Wenn Sie die CER-Datei für das selbstsignierte Zertifikat im Rahmen der Punkt-zu-Standort-Konfiguration hochladen, teilen Sie Azure mit, dass die Zertifikate, die von Clientcomputern verwendet werden, vertrauenswürdig sind.<br><br>Jeder Computer mit einem installierten Clientzertifikat, der mit den richtigen VPN-Clienteinstellungen konfiguriert ist, kann eine Punkt-zu-Standort-Verbindung mit Ihrem virtuellen Netzwerk herstellen. Aus diesem Grund müssen Sie sicherstellen, dass Clientzertifikate nur bei Bedarf generiert und installiert werden und dass dieses selbstsignierte Zertifikat gesichert und sicher gespeichert wird. Bei Bedarf können Sie später dieses selbstsignierte Zertifikat auf einem anderen Computer installieren und weitere Clientzertifikate generieren oder die CER-Datei exportieren.
 

## Erstellen und Installieren von Clientzertifikaten

Das selbstsignierte Zertifikat ist nicht das Zertifikat, das Sie auf Ihren Clients installieren. Sie müssen aus einem selbstsignierten Zertifikat ein Clientzertifikat generieren. Dieses Clientzertifikat exportieren und installieren Sie auf den Clientcomputern. Die folgenden Schritte gelten nicht für ein spezifisches Bereitstellungsmodell. Sie sind für das Ressourcen-Manager-Modell und das klassische Modell gültig.

### Teil 1: Generieren eines Clientzertifikats aus einem selbstsignierten Zertifikat

Die folgenden Schritte stellen eine der Möglichkeiten dar, ein Clientzertifikat aus einem selbstsignierten Zertifikat zu generieren. Sie können mehrere Clientzertifikate aus dem gleichen Zertifikat generieren. Jedes Clientzertifikat kann anschließend exportiert und auf dem Clientcomputer installiert werden.

1. Öffnen Sie auf dem Computer, den Sie zum Erstellen des selbstsignierten Zertifikats verwendet haben, eine Eingabeaufforderung als Administrator.

2. Wechseln Sie zu dem Speicherort, an dem die Clientzertifikatdatei gespeichert werden soll. *CertificateName* steht für das selbstsignierte Zertifikat, das Sie generiert haben. Wenn Sie das folgende Beispiel ausführen (wobei Sie CertificateName in den Namen Ihres Zertifikats ändern), erhalten Sie ein Clientzertifikat mit dem Namen ClientCertificateName in Ihrem persönlichen Zertifikatspeicher.

3. Geben Sie folgenden Befehl ein:

    	makecert.exe -n "CN=ClientCertificateName" -pe -sky exchange -m 96 -ss My -in "CertificateName" -is my -a sha1

4. Alle Zertifikate werden im Zertifikatspeicher „Zertifikate - Aktueller Benutzer\\Eigene Zertifikate\\Zertifikate“ auf dem Computer gespeichert. Sie können mit diesem Verfahren bei Bedarf beliebig viele Clientzertifikate generieren.

### Teil 2: Exportieren eines Clientzertifikats

1. Um ein Clientzertifikat zu exportieren, öffnen Sie **certmgr.msc**. Klicken Sie mit der rechten Maustaste auf das Clientzertifikat, das Sie exportieren möchten, klicken Sie auf **Alle Aufgaben** und anschließend auf **Exportieren**. Dadurch wird der **Zertifikatexport-Assistent** geöffnet.

2. Klicken Sie im Assistenten auf **Weiter**, wählen Sie **Ja, privaten Schlüssel exportieren** aus, und klicken Sie dann auf **Weiter**.

3. Auf der Seite **Format der zu exportierenden Datei** können Sie die Standardwerte übernehmen. Klicken Sie auf **Weiter**.
 
4. Auf der Seite **Sicherheit** müssen Sie den privaten Schlüssel schützen. Wenn Sie ein Kennwort verwenden möchten, müssen Sie sich das für dieses Zertifikat festgelegte Kennwort unbedingt merken oder notieren. Klicken Sie auf **Weiter**.

5. Wählen Sie unter **Zu exportierende Datei** die Option **Durchsuchen**, um zu dem Speicherort zu wechseln, in den das Zertifikat exportiert werden soll. Geben Sie unter **Dateiname** einen Namen für die Zertifikatdatei ein. Klicken Sie auf **Weiter**.

6. Klicken Sie auf **Fertig stellen**, um das Zertifikat zu exportieren.

### Teil 3: Installieren eines Clientzertifikats

Auf jedem Client, der über eine Punkt-zu-Standort-Verbindung mit Ihrem virtuellen Netzwerk verbunden werden soll, muss ein Clientzertifikat installiert sein. Dieses Zertifikat wird zusätzlich zum erforderlichen VPN-Konfigurationspaket benötigt. Die folgenden Schritte führen Sie durch die manuelle Installation des Clientzertifikats.

1. Suchen Sie die *PFX*-Datei, und kopieren Sie sie auf den Clientcomputer. Doppelklicken Sie auf dem Clientcomputer auf die *PFX*-Datei, um sie zu installieren. Lassen Sie den **Speicherort** auf **Aktueller Benutzer** eingestellt, und klicken Sie dann auf **Weiter**.

2. Nehmen Sie auf der Seite **Zu importierende Datei** keine Änderungen vor. Klicken Sie auf **Weiter**.

3. Geben Sie auf der Seite **Schutz für den privaten Schlüssel** das Kennwort für das Zertifikat ein (sofern Sie ein Kennwort festgelegt haben), oder überprüfen Sie die Richtigkeit des Sicherheitsprinzipals, der das Zertifikat installiert, und klicken Sie dann auf **Weiter**.

4. Behalten Sie auf der Seite **Zertifikatspeicher** den Standardspeicherort bei, und klicken Sie dann auf **Weiter**.

5. Klicken Sie auf **Fertig stellen**. Klicken Sie in der **Sicherheitswarnung** für die Zertifikatinstallation auf **Ja**. Das Zertifikat wird nun erfolgreich importiert.

## Nächste Schritte

Setzen Sie die Punkt-zu-Standort-Konfiguration fort.

- Schritte für das Bereitstellungsmodell **Resource Manager** finden Sie unter [Konfigurieren einer Punkt-zu-Standort-VPN-Verbindung mit einem virtuellen Netzwerk mithilfe von PowerShell](vpn-gateway-howto-point-to-site-rm-ps.md).
- Schritte für das **klassische** Bereitstellungsmodell finden Sie unter [Konfigurieren einer Punkt-zu-Standort-VPN-Verbindung mit einem VNet](vpn-gateway-point-to-site-create.md).

<!---HONumber=AcomDC_0824_2016-->
---
title: Generieren und Übertragen von HSM-geschützten Schlüsseln für den Azure-Schlüsseltresor | Microsoft Docs
description: Verwenden Sie diesen Artikel zum Planen, Generieren und anschließenden Übertragen Ihrer eigenen HSM-geschützten Schlüssel für die Nutzung mit dem Azure-Schlüsseltresor.
services: key-vault
documentationcenter: ''
author: cabailey
manager: mbaldwin
tags: azure-resource-manager

ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: cabailey

---
# <a name="how-to-generate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a>Gewusst wie: Generieren und Übertragen von HSM-geschützten Schlüsseln für den Azure-Schlüsseltresor
## <a name="introduction"></a>Einführung
Zur Steigerung der Sicherheit können Sie bei Verwendung des Azure-Schlüsseltresors Schlüssel in HSMs (Hardwaresicherheitsmodule) importieren oder darin generieren. Diese Schlüssel verbleiben immer innerhalb der HSM-Grenzen. Dieses Szenario wird häufig als *Bring Your Own Key* (BYOK) bezeichnet. Die HSMs sind FIPS 140-2 Ebene 2 überprüft. Für den Azure-Schlüsseltresor wird die Thales nShield-Familie der HSMs zum Schützen der Schlüssel verwendet.

Verwenden Sie die Informationen in diesem Thema zum Planen, Generieren und anschließenden Übertragen Ihrer eigenen HSM-geschützten Schlüssel für die Nutzung mit Azure Key Vault.

Diese Funktion für Azure China nicht zur Verfügung. 

> [!NOTE]
> Weitere Informationen zum Azure-Schlüsseltresor finden Sie unter [Was ist der Azure-Schlüsseltresor?](key-vault-whatis.md)  
> 
> Ein Tutorial zu den ersten Schritten, z.B. zum Erstellen eines Schlüsseltresors für HSM-geschützte Schlüssel, finden Sie unter [Erste Schritte mit Azure Key Vault](key-vault-get-started.md).
> 
> 

Weitere Informationen zum Generieren und Übertragen eines HSM-geschützten Schlüssels über das Internet:

* Sie generieren den Schlüssel auf einer Offlinearbeitsstation, um die Angriffsfläche zu reduzieren.
* Der Schlüssel wird mit einem Schlüsselaustauschschlüssel (Key Exchange Key, KEK) verschlüsselt. Die Verschlüsselung bleibt bis zur Übertragung an die HSMs des Azure-Schlüsseltresors bestehen. Nur die verschlüsselte Version Ihres Schlüssels verlässt die ursprüngliche Arbeitsstation.
* Mit dem Toolset werden Eigenschaften für Ihren Mandantenschlüssel festgelegt, mit dem Ihr Schlüssel an die Sicherheitsumgebung des Azure-Schlüsseltresors gebunden wird. Nachdem die HSMs des Azure-Schlüsseltresors Ihren Schlüssel empfangen und entschlüsselt haben, kann er nur von diesen HSMs verwendet werden. Der Schlüssel kann nicht exportiert werden. Diese Bindung wird mit den Thales-HSMs erzwungen.
* Der Schlüsselaustauschschlüssel (Key Exchange Key, KEK), der zum Verschlüsseln Ihres Schlüssels verwendet wird, wird innerhalb der HSMs des Azure-Schlüsseltresors generiert und kann nicht exportiert werden. Die HSMs erzwingen, dass außerhalb der HSMs keine unverschlüsselte Version des Schlüsselaustauschschlüssels vorhanden sein kann. Darüber hinaus verfügt das Toolset über den Nachweis von Thales, dass der Schlüsselaustauschschlüssel nicht exportiert werden kann und innerhalb eines Original-HSM von Thales generiert wurde.
* Das Toolset enthält einen Nachweis von Thales, dass die Sicherheitsumgebung des Azure-Schlüsseltresors ebenfalls in einem von Thales hergestellten Original-HSM generiert wurde. Dieser Nachweis ist ein Beleg dafür, dass Microsoft Originalhardware verwendet.
* Microsoft verwendet separate Schlüsselaustauschschlüssel (KEK) und getrennte Sicherheitsumgebungen (Security Worlds) in jeder geografischen Region. Durch diese Trennung wird sichergestellt, dass Ihr Schlüssel nur in Rechenzentren in der Region verwendet werden kann, in der Sie die Verschlüsselung durchgeführt haben. Ein Schlüssel eines Kunden aus Europa kann beispielsweise nicht in Rechenzentren in Nordamerika oder Asien verwendet werden.

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a>Weitere Informationen zu Thales-HSMs und Microsoft-Diensten
Thales e-Security ist ein weltweit führender Anbieter von Lösungen für Datenverschlüsselung und Cybersicherheit für die Bereiche Finanzdienstleistungen, Hightech, Fertigung, Behörden und Technologie. Thales hat über 40 Jahre Erfahrung, was den Schutz der Daten von Unternehmen und Behörden betrifft. Die Lösungen der Firma werden von vier der fünf größten Energie- und Luft- und Raumfahrtunternehmen genutzt. 22 NATO-Staaten nutzen diese Lösungen, die damit zum Schutz von mehr als 80 % der weltweiten Zahlungstransaktionen beitragen.

Microsoft hat mit Thales zusammengearbeitet, um den Entwicklungsstand der HSMs zu verbessern. Dank dieser Verbesserungen können Sie die typischen Vorteile von gehosteten Diensten nutzen, ohne die Kontrolle über Ihre Schlüssel abzugeben. Vor allem ist es vorteilhaft, dass die HSMs aufgrund dieser Verbesserungen von Microsoft verwaltet werden können, damit Sie dies nicht übernehmen müssen. Als Clouddienst kann der Azure-Schlüsseltresor schnell zentral hochskaliert werden, um die Auslastungsspitzen Ihrer Organisation zu bewältigen. Gleichzeitig ist der Schlüssel in den HSMs von Microsoft geschützt: Sie behalten die Kontrolle über den Lebenszyklus des Schlüssels, da Sie ihn generieren und an die HSMs von Microsoft übertragen.

## <a name="implementing-bring-your-own-key-(byok)-for-azure-key-vault"></a>Implementieren von „Bring Your Own Key“ (BYOK) für den Azure-Schlüsseltresor
Verwenden Sie die folgenden Informationen und Verfahren, wenn Sie Ihren eigenen HSM-geschützten Schlüssel generieren und dann an den Azure-Schlüsseltresor übertragen möchten. Dies ist das BYOK-Szenario (Bring Your Own Key).

## <a name="prerequisites-for-byok"></a>Voraussetzungen für BYOK
Die folgende Tabelle enthält eine Liste mit Voraussetzungen, die beim Azure-Schlüsseltresor für Bring Your Own Key (BYOK) erfüllt sein müssen.

| Anforderung | Weitere Informationen |
| --- | --- |
| Azure-Abonnement |Um einen Azure-Schlüsseltresor erstellen zu können, benötigen Sie ein Azure-Abonnement: [Registrieren Sie sich für die kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/) |
| Azure-Schlüsseltresor, der HSMs unterstützt |Weitere Informationen zu den Dienstebenen und Funktionen für den Azure-Schlüsseltresor finden Sie auf der Website [Azure-Schlüsseltresor – Preise](https://azure.microsoft.com/pricing/details/key-vault/) . |
| Thales-HSM, Smartcards und Supportsoftware |Sie benötigen Zugriff auf ein Thales-Hardwaresicherheitsmodul sowie grundlegende Kenntnisse zum Betrieb von Thales-HSMs. Eine Liste mit kompatiblen Modellen bzw. Informationen zum Kauf eines HSM, für den Fall, dass Sie noch keins besitzen, finden Sie unter [Thales-Hardwaresicherheitsmodul](https://www.thales-esecurity.com/msrms/buy). |
| Folgende Hardware und Software:<ol><li>Eine x64-Arbeitsstation im Offlinemodus mit einem Windows-Betriebssystem der Mindestversion Windows 7 und Thales nShield-Software der Mindestversion 11.50.<br/><br/>Wenn auf dieser Arbeitsstation Windows 7 ausgeführt wird, müssen Sie [Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe) installieren.</li><li>Eine Arbeitsstation, die mit dem Internet verbunden ist und über ein Windows-Betriebssystem der Mindestversion Windows 7 verfügt.</li><li>Ein USB-Laufwerk oder ein anderes tragbares Speichergerät mit mindestens 16 MB freiem Speicherplatz.</li></ol> |Aus Sicherheitsgründen wird empfohlen, die erste Arbeitsstation nicht mit einem Netzwerk zu verbinden. Diese Empfehlung wird jedoch nicht programmgesteuert erzwungen.<br/><br/>Beachten Sie, dass diese Arbeitsstation in den folgenden Anleitungen als nicht verbundene Arbeitsstation bezeichnet wird.</p></blockquote><br/>Falls Ihr Mandantenschlüssel für ein Produktionsnetzwerk gilt, empfehlen wir außerdem, eine zweite separate Arbeitsstation zu verwenden, um das Toolset herunterzuladen und den Mandantenschlüssel hochzuladen. Zu Testzwecken können Sie aber auch ein und dieselbe Arbeitsstation verwenden.<br/><br/>Beachten Sie, dass die zweite Arbeitsstation in den folgenden Anleitungen als Arbeitsstation mit Internetverbindung bezeichnet wird.</p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Generieren und Übertragen des Schlüssels an das HSM des Azure-Schlüsseltresors
Sie verwenden die folgenden fünf Schritte zum Generieren und Übertragen Ihres Schlüssels an das HSM von Azure Key Vault:

* [Schritt 1: Vorbereiten der Arbeitsstation mit Internetverbindung](#step-1-prepare-your-internet-connected-workstation)
* [Schritt 2: Vorbereiten der verbindungslosen Arbeitsstation](#step-2-prepare-your-disconnected-workstation)
* [Schritt 3: Generieren des Schlüssels](#step-3-generate-your-key)
* [Schritt 4: Vorbereiten des Schlüssels für die Übertragung](#step-4-prepare-your-key-for-transfer)
* [Schritt 5: Übertragen des Schlüssels an den Azure-Schlüsseltresor](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1:-prepare-your-internet-connected-workstation"></a>Schritt 1: Vorbereiten der Arbeitsstation mit Internetverbindung
Führen Sie für diesen ersten Schritt die folgenden Verfahren auf der Arbeitsstation mit Internetverbindung aus.

### <a name="step-1.1:-install-azure-powershell"></a>Schritt 1.1: Installieren von Azure PowerShell
Laden Sie auf der Arbeitsstation mit Internetverbindung das Azure PowerShell-Modul herunter, das die Cmdlets zum Verwalten des Azure-Schlüsseltresors enthält, und installieren Sie es. Hierfür ist mindestens die Version 0.8.13 erforderlich.

Installationsanweisungen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../powershell-install-configure.md).

### <a name="step-1.2:-get-your-azure-subscription-id"></a>Schritt 1.2: Abrufen der Azure-Abonnement-ID
Starten Sie eine Azure PowerShell-Sitzung, und melden Sie sich mit dem folgenden Befehl bei Ihrem Azure-Konto an:

        Add-AzureAccount
Geben Sie im Popup-Browserfenster den Benutzernamen und das Kennwort Ihres Azure-Kontos ein. Verwenden Sie anschließend den Befehl [Get-AzureSubscription](https://msdn.microsoft.com/library/azure/dn790366.aspx) :

        Get-AzureSubscription
Suchen Sie in der Ausgabe die ID für das Abonnement, das Sie für den Azure-Schlüsseltresor verwenden möchten. Sie benötigen diese Abonnement-ID später noch.

Lassen Sie das Azure PowerShell-Fenster geöffnet.

### <a name="step-1.3:-download-the-byok-toolset-for-azure-key-vault"></a>Schritt 1.3: Herunterladen des BYOK-Toolsets für den Azure-Schlüsseltresor
Wechseln Sie zum Microsoft Download Center, und [laden Sie das Azure-Schlüsseltresor-BYOK-Toolset für Ihre geografische Region oder Instanz von Azure herunter](http://www.microsoft.com/download/details.aspx?id=45345) . Ermitteln Sie anhand der folgenden Informationen den Namen des herunterzuladenden Pakets und seinen entsprechenden SHA-256-Pakethash:

- - -
**Nordamerika:**

KeyVault-BYOK-Tools-UnitedStates.zip

305F44A78FEB750D1D478F6A0C345B097CD5551003302FA465C73D9497AB4A03

- - -
**Europa:**

KeyVault-BYOK-Tools-Europe.zip

C73BB0628B91471CA7F9ADFCE247561C6016A5103EF1A315D49C3EA23AFC0B9C

- - -
**Asien:**

KeyVault-BYOK-Tools-AsiaPacific.zip

BE9A84B6C76661929F9FDAD627005D892B3B8F9F19F351220BB4F9C356694192

- - -
**Lateinamerika:**

KeyVault-BYOK-Tools-LatinAmerica.zip

9E8EE11972DECE8F05CD898AF64C070C375B387CED716FDCB788544AE27D3D23

- - -
**Japan:**

KeyVault-BYOK-Tools-Japan.zip

E6B88C111D972A02ABA3325F8969C4E36FD7565C467E9D7107635E3DDA11A8B2

- - -
**Australien:**

KeyVault-BYOK-Tools-Australia.zip

7660D7A675506737857B14F527232BE51DC269746590A4E5AB7D50EDD220675D

- - -
[**Azure Government:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-Tools-USGovCloud.zip

53801A3043B0F8B4A50E8DC01A935C2BFE61F94EE027445B65C52C1ACC2B5E80

- - -
**Kanada:**

KeyVault-BYOK-Tools-Canada.zip

A42D9407B490E97693F8A5FA6B60DC1B06B1D1516EDAE7C9A71AA13E12CF1345

- - -
**Deutschland:**

KeyVault-BYOK-Tools-Germany.zip

4795DA855E027B2CA8A2FF1E7AE6F03F772836C7255AFC68E576410BDD28B48E

- - -
**Indien:**

KeyVault-BYOK-Tools-India.zip

26853511EB767A33CF6CD880E78588E9BBE04E619B17FBC77A6B00A5111E800C

- - -
Verwenden Sie in der Azure PowerShell-Sitzung das [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) -Cmdlet, um die Integrität des heruntergeladenen BYOK-Toolsets zu überprüfen.

    Get-FileHash KeyVault-BYOK-Tools-*.zip

Das Toolset enthält Folgendes:

* Ein Schlüsselaustauschschlüssel-Paket (Key Exchange Key, KEK), dessen Name mit **BYOK-KEK-pkg-.**
* Ein Security World-Paket, dessen Name mit **BYOK-SecurityWorld-pkg-.**
* Ein Python-Skript mit dem Namen **verifykeypackage.py**.
* Eine ausführbare Befehlszeilendatei namens **KeyTransferRemote.exe** und die zugehörigen DLLs.
* Ein Visual C++ Redistributable Package namens **vcredist_x64.exe**.

Kopieren Sie das Paket auf ein USB-Laufwerk oder ein anderes tragbares Speichergerät.

## <a name="step-2:-prepare-your-disconnected-workstation"></a>Schritt 2: Vorbereiten der verbindungslosen Arbeitsstation
Führen Sie für diesen zweiten Schritt die folgenden Verfahren auf der Arbeitsstation durch, die nicht mit einem Netzwerk (dem Internet oder Ihrem internen Netzwerk) verbunden ist.

### <a name="step-2.1:-prepare-the-disconnected-workstation-with-thales-hsm"></a>Schritt 2.1: Vorbereiten der verbindungslosen Arbeitsstation per Thales-HSM
Installieren Sie die Supportsoftware nCipher (Thales) auf einem Windows-Computer, und schließen Sie dann ein Thales-HSM an diesen Computer an.

Stellen Sie sicher, dass sich die Thales-Tools in Ihrem Pfad befinden (**%nfast_home%\bin** und **%nfast_home%\python\bin**). Geben Sie beispielsweise Folgendes ein:

        set PATH=%PATH%;”%nfast_home%\bin”;”%nfast_home%\python\bin”

Weitere Informationen finden Sie im Benutzerhandbuch zum Thales-HSM.

### <a name="step-2.2:-install-the-byok-toolset-on-the-disconnected-workstation"></a>Schritt 2.2: Installieren des BYOK-Toolsets auf der verbindungslosen Arbeitsstation
Kopieren Sie das BYOK-Toolsetpaket vom USB-Laufwerk bzw. vom tragbaren Speichergerät, und führen Sie Folgendes aus:

1. Extrahieren Sie die Dateien aus dem heruntergeladenen Paket in einen beliebigen Ordner.
2. Führen Sie in diesem Ordner die Datei „vcredist_x64.exe“ aus.
3. Folgen Sie den Anweisungen zum Installieren der Visual C++-Laufzeitkomponenten für Visual Studio 2013.

## <a name="step-3:-generate-your-key"></a>Schritt 3: Generieren des Schlüssels
Führen Sie für diesen dritten Schritt die folgenden Verfahren auf der verbindungslosen Arbeitsstation durch.

### <a name="step-3.1:-create-a-security-world"></a>Schritt 3.1: Erstellen einer Sicherheitsumgebung (Security World)
Starten Sie eine Eingabeaufforderung, und führen Sie das Thales-new-world-Programm aus.

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

Dieses Programm erstellt eine Datei für die **Security World** unter „%NFAST_KMDATA%\local\world“ entsprechend dem Ordner „C:\ProgramData\nCipher\Key Management Data\local“. Sie können verschiedene Werte für das Quorum verwenden, aber in unserem Beispiel werden Sie aufgefordert, jeweils drei leere Karten und PINs einzugeben. Sie erhalten dann mit zwei beliebigen Karten vollständigen Zugriff auf die Security World. Diese Karten werden zur **Administratorkartengruppe** für die neue Security World.

Gehen Sie wie folgt vor:

* Sichern Sie die World-Datei. Bewahren Sie die World-Datei, die Administratorkarten und die dazugehörigen PINs an einem geschützten Ort auf, und stellen Sie sicher, dass Personen jeweils nur Zugang zu einer Karte haben.

### <a name="step-3.2:-validate-the-downloaded-package"></a>Schritt 3.2: Überprüfen des heruntergeladenen Pakets
Dieser Schritt ist optional, wird aber empfohlen, damit Sie Folgendes überprüfen können:

* Der Schlüsselaustauschschlüssel, der im Toolset enthalten ist, wurde mit einem Original-HSM von Thales generiert.
* Der Hash der Security World, der im Toolset enthalten ist, wurde mit einem Original-HSM von Thales generiert.
* Der Schlüsselaustauschschlüssel kann nicht exportiert werden.

> [!NOTE]
> Um das heruntergeladene Paket überprüfen zu können, muss das HSM verbunden sein, eingeschaltet sein und über eine Security World verfügen (z. B. die eben erstellte).
> 
> 

So überprüfen Sie das heruntergeladene Paket

1. Führen Sie das Skript „verifykeypackage.py“ aus, indem Sie je nach Region oder Instanz von Azure Folgendes eingeben:
   
   * Für Nordamerika:
     
           python verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * Für Europa:
     
           python verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * Für Asien:
     
           python verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * Für Lateinamerika:
     
           python verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * Für Japan:
     
           python verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * Für Australien:
     
           python verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * Für [Azure Government](https://azure.microsoft.com/features/gov/)(arbeitet mit der Instanz der US-Regierung von Azure):
     
           python verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * Für Kanada:
     
           python verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * Für Deutschland:
     
           python verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * Für Indien:
     
           python verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
     > [!TIP]
     > Die Thales-Software enthält Python unter „%NFAST_HOME%\python\bin“.
     > 
     > 
2. Vergewissern Sie sich, dass Folgendes angezeigt wird, um eine erfolgreiche Überprüfung zu melden: **Result: SUCCESS**

Mit diesem Skript wird die Signaturgeberkette bis zum Thales-Stammschlüssel überprüft. Der Hash dieses Stammschlüssels ist in das Skript eingebettet, und sein Wert sollte **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**lauten. Sie können diesen Wert auch separat bestätigen, indem Sie die [Thales-Website](http://www.thalesesec.com/)besuchen.

Sie können nun einen neuen Schlüssel erstellen.

### <a name="step-3.3:-create-a-new-key"></a>Schritt 3.3: Erstellen eines neuen Schlüssels
Generieren Sie einen Schlüssel, indem Sie das Thales-Programm **generatekey** verwenden.

Führen Sie den folgenden Befehl aus, um den Schlüssel zu generieren:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Gehen Sie wie folgt vor, wenn Sie diesen Befehl ausführen:

* Der Parameter *protect* muss wie gezeigt auf den Wert **module**festgelegt werden. So wird ein modulgeschützter Schlüssel erstellt. Das BYOK-Toolset unterstützt OCS-geschützte Schlüssel nicht.
* Ersetzen Sie den Wert von *contosokey* für **ident** und **plainname** durch einen beliebigen Zeichenfolgenwert. Um den Verwaltungsaufwand zu minimieren und das Fehlerrisiko zu senken, empfehlen wir, jeweils den gleichen Wert zu verwenden. Der Wert **Ident** darf nur Zahlen, Bindestriche und Kleinbuchstaben enthalten.
* „pubexp“ wird in diesem Beispiel leer gelassen (Standard), aber Sie können bestimmte Werte angeben. Weitere Informationen finden Sie in der Thales-Dokumentation.

Mit diesem Befehl wird im Ordner „%NFAST_KMDATA%\local“ eine Tokenschlüsseldatei erstellt, deren Name mit **key_simple_** beginnt. Danach folgt der Wert von **ident**, der im Befehl angegeben wurde. Beispiel: **key_simple_contosokey**. Diese Datei enthält einen verschlüsselten Schlüssel.

Sichern Sie diese Tokenschlüsseldatei an einem sicheren Ort.

> [!IMPORTANT]
> Wenn Sie Ihren Schlüssel später in den Azure-Schlüsseltresor übertragen, kann Microsoft diesen Schlüssel nicht für Sie zurück exportieren. Daher ist es sehr wichtig, dass Sie Ihren Schlüssel und die Security World sorgfältig sichern. Informationen und bewährte Methoden zum Sichern des Schlüssels erhalten Sie bei Thales.
> 
> 

Sie können Ihren Schlüssel jetzt an den Azure-Schlüsseltresor übertragen.

## <a name="step-4:-prepare-your-key-for-transfer"></a>Schritt 4: Vorbereiten des Schlüssels für die Übertragung
Führen Sie für diesen vierten Schritt die folgenden Verfahren auf der verbindungslosen Arbeitsstation durch.

### <a name="step-4.1:-create-a-copy-of-your-key-with-reduced-permissions"></a>Schritt 4.1: Erstellen einer Kopie des Schlüssels mit reduzierten Berechtigungen
Um die Berechtigungen für den Schlüssel zu reduzieren, führen Sie an einer Eingabeaufforderung je nach Region oder Instanz von Azure Folgendes aus:

* Für Nordamerika:
  
        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* Für Europa:
  
        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* Für Asien:
  
        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* Für Lateinamerika:
  
        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* Für Japan:
  
        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* Für Australien:
  
        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* Für [Azure Government](https://azure.microsoft.com/features/gov/)(arbeitet mit der Instanz der US-Regierung von Azure):
  
        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* Für Kanada:
  
        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* Für Deutschland:
  
        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* Für Indien:
  
        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1

Ersetzen Sie beim Ausführen dieses Befehls *contosokey* durch den gleichen Wert, den Sie in **Schritt 3.3: Erstellen eines neuen Schlüssels** unter [Generieren des Schlüssels](#step-3-generate-your-key) angegeben haben.

Sie werden aufgefordert, Ihre Security World-Administratorkarten einzuführen.

Wenn der Befehl abgeschlossen ist, wird **Result: SUCCESS** angezeigt, und die Kopie Ihres Schlüssels mit reduzierten Berechtigungen ist in der Datei „key_xferacId_<contosokey>“ enthalten.

### <a name="step-4.2:-inspect-the-new-copy-of-the-key"></a>Schritt 4.2: Überprüfen der neuen Kopie des Schlüssels
Optional können Sie die Thales-Hilfsprogramme ausführen, um zu bestätigen, dass der neue Schlüssel nur über minimale Berechtigungen verfügt:

* aclprint.py:
  
        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* kmfile-dump.exe:
  
        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  Ersetzen Sie beim Ausführen dieser Befehle „contosokey“ durch den Wert, den Sie in **Schritt 3.3: Erstellen eines neuen Schlüssels** unter [Generieren des Schlüssels](#step-3-generate-your-key) angegeben haben.

### <a name="step-4.3:-encrypt-your-key-by-using-microsoft’s-key-exchange-key"></a>Schritt 4.3: Verschlüsseln des Schlüssels mit dem Schlüsselaustauschschlüssel von Microsoft
Führen Sie je nach geografischer Region oder Instanz von Azure einen der folgenden Befehle aus:

* Für Nordamerika:
  
        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Für Europa:
  
        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Für Asien:
  
        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Für Lateinamerika:
  
        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Für Japan:
  
        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Für Australien:
  
        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Für [Azure Government](https://azure.microsoft.com/features/gov/)(arbeitet mit der Instanz der US-Regierung von Azure):
  
        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Für Kanada:
  
        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Für Deutschland:
  
        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Für Indien:
  
        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

Gehen Sie wie folgt vor, wenn Sie diesen Befehl ausführen:

* Ersetzen Sie *contosokey* durch den Bezeichner, den Sie unter **Schritt 3.3: Erstellen eines neuen Schlüssels** im Schritt [Generieren des Schlüssels](#step-3-generate-your-key) verwendet haben.
* Ersetzen Sie *SubscriptionID* durch die ID des Azure-Abonnements, das Ihren Schlüsseltresor enthält. Sie haben diesen Wert vorher unter **Schritt 1.2: Abrufen der Azure-Abonnement-ID** im Schritt [Vorbereiten der Arbeitsstation mit Internetverbindung](#step-1-prepare-your-internet-connected-workstation) abgerufen.
* Ersetzen Sie *ContosoFirstHSMKey* durch eine Bezeichnung, die als Name Ihrer Ausgabedatei verwendet wird.

Wenn dieser Vorgang erfolgreich ist, wird **Result: SUCCESS** angezeigt, und im aktuellen Ordner ist eine neue Datei mit dem folgenden Namen enthalten: „TransferPackage-*ContosoFirstHSMkey*byok“.

### <a name="step-4.4:-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Schritt 4.4: Kopieren des Schlüsselübertragungspakets auf die Arbeitsstation mit Internetverbindung
Verwenden Sie ein USB-Laufwerk oder anderes tragbares Speichergerät, um die Ausgabedatei aus dem vorherigen Schritt (KeyTransferPackage-ContosoFirstHSMkey.byok) auf die Arbeitsstation mit Internetverbindung zu kopieren.

## <a name="step-5:-transfer-your-key-to-azure-key-vault"></a>Schritt 5: Übertragen des Schlüssels an den Azure-Schlüsseltresor
Verwenden Sie für diesen letzten Schritt auf der Arbeitsstation mit Internetverbindung das Cmdlet [Add-AzureKeyVaultKey](https://msdn.microsoft.com/library/azure/dn868048\(v=azure.300\).aspx), um das Schlüsselübertragungspaket hochzuladen, das Sie von der nicht verbundenen Arbeitsstation in das HSM von Azure Key Vault kopiert haben:

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\TransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

Wenn der Upload erfolgreich ist, werden die Eigenschaften des gerade hinzugefügten Schlüssels angezeigt.

## <a name="next-steps"></a>Nächste Schritte
Sie können diesen HSM-geschützten Schlüssel jetzt in Ihrem Schlüsseltresor verwenden. Weitere Informationen finden Sie im Abschnitt **Verwenden eines Hardwaresicherheitsmoduls (HSM)** im Lernprogramm [Erste Schritte mit dem Azure-Schlüsseltresor](key-vault-get-started.md) .

<!--HONumber=Oct16_HO2-->



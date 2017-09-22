---
title: "Microsoft Azure Stack Development Kit – Versionshinweise | Microsoft-Dokumentation"
description: 
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a7e61ea4-be2f-4e55-9beb-7a079f348e05
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2017
ms.author: helaw
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 9e82cf367bdc03aaafe1050078194cc3bd746bac
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---

# <a name="azure-stack-development-kit-release-notes"></a>Versionshinweise für Azure Stack Development Kit
Diese Versionshinweise enthalten Informationen zu neuen Features und bekannten Problemen.

## <a name="release-build-201706271"></a>Versionsbuild 20170627.1
Ab Version [20170627.1](azure-stack-updates.md#determine-the-current-version) wurde Azure Stack Proof of Concept (POC) in Azure Stack Development Kit umbenannt.  Wie Azure Stack POC auch, soll das Azure Stack Development Kit als Entwicklungs- und Evaluierungsumgebung zum Erkunden der Azure Stack-Features und als Entwicklungsplattform für Azure Stack dienen.

### <a name="whats-new"></a>Neuigkeiten
- Sie können CLI 2.0 jetzt verwenden, um Azure Stack-Ressourcen auf gängigen Betriebssystemen über die Befehlszeile zu verwalten.
- DSV2-VM-Größen ermöglichen die Vorlagenportabilität zwischen Azure und Azure Stack.
- Cloudbetreiber können eine Vorschau der Benutzeroberfläche für die Kapazitätsverwaltung auf dem entsprechenden Blatt anzeigen.
- Sie können jetzt die Erweiterung „Azure-Diagnose“ verwenden, um Diagnosedaten von Ihren virtuellen Computern zu sammeln.  Die Erfassung dieser Daten ist nützlich, um die Leistung von Workloads zu analysieren und Probleme zu untersuchen.
- Eine neue [Benutzeroberfläche für die Bereitstellung](azure-stack-run-powershell-script.md) ersetzt die bisherigen Skriptschritte zum Durchführen der Bereitstellung.  Die neue Umgebung für die Bereitstellung enthält eine allgemeine grafische Benutzeroberfläche für den gesamten Bereitstellungslebenszyklus.
- Microsoft-Konten (MSA) werden während der Bereitstellung jetzt unterstützt.
- Multi-Factor Authentication (MFA) wird während der Bereitstellung jetzt unterstützt.  Bisher musste MFA während der Bereitstellung deaktiviert werden.

### <a name="known-issues"></a>Bekannte Probleme
#### <a name="deployment"></a>Bereitstellung
* Unter Umständen fällt Ihnen auf, dass die Bereitstellung länger als bei den vorherigen Versionen dauert. 
* Get-AzureStackLogs generiert Diagnoseprotokolle, aber der Status wird in der Konsole nicht protokolliert.
* Sie müssen die neue [Benutzeroberfläche für die Bereitstellung](azure-stack-run-powershell-script.md) verwenden, um Azure Stack bereitzustellen, da der Vorgang ansonsten ggf. nicht erfolgreich ist.
* Bereitstellungen, für die der Parameter *PublicVLANID* verwendet wird, schlagen fehl.

#### <a name="portal"></a>Portal
* Es kann sein, dass im Portal ein leeres Dashboard angezeigt wird.  Sie können das Dashboard wiederherstellen, indem Sie das Zahnrad oben rechts im Portal und dann die Option „Standardeinstellungen wiederherstellen“ wählen.
* Mandanten können den gesamten Marketplace ohne Abonnement durchsuchen, und es werden administrative Elemente wie Pläne und Angebote angezeigt.  Diese Elemente sind für Mandanten nicht funktional.
* Beim Auswählen einer Infrastrukturrolleninstanz wird ein Fehler (Verweisfehler) angezeigt. Nutzen Sie die Aktualisierungsfunktion des Browsers, um das Verwaltungsportal zu aktualisieren.
* Die Schaltfläche „Verschieben“ ist auf dem Blatt „Ressourcengruppe“ deaktiviert.  Dies ist das erwartete Verhalten, da das Verschieben von Ressourcengruppen zwischen Abonnements derzeit nicht unterstützt wird.
* Sie erhalten wiederholt Benachrichtigungen für veröffentlichte Marketplace-Elemente, für die der Download abgeschlossen wurde.
* Sie können mit den Azure Stack-Portalen keine Berechtigungen für Ihr Abonnement anzeigen.  Eine Problemumgehung besteht darin, die Berechtigungen mit PowerShell zu überprüfen.
* Hierzu müssen Sie `-TenantID` als Flag hinzufügen, wenn Sie eine abgeschlossene Bereitstellung als Automatisierungsskript aus dem Portal exportieren.

#### <a name="services"></a>Dienste
* Key Vault-Dienste müssen über das Mandantenportal oder die Mandanten-API erstellt werden.  Wenn Sie als Administrator angemeldet sind, sollten Sie unbedingt das Mandantenportal verwenden, um neue Key Vault-Tresore, -Geheimnisse und -Schlüssel zu erstellen.
* Es ist keine Marketplace-Benutzeroberfläche zum Erstellen von VM-Skalierungsgruppen vorhanden, aber sie können per Vorlage erstellt werden.
* Es ist für Sie nicht möglich, über das Portal einen Lastenausgleich einem Back-End-Netzwerk zuzuordnen.  Diese Aufgabe kann mit PowerShell oder per Vorlage durchgeführt werden.
* VM-Verfügbarkeitsgruppen können nur mit einer Fehlerdomäne und einer Updatedomäne konfiguriert werden.  
* Ein Mandant muss über ein vorhandenes Speicherkonto verfügen, bevor er eine neue Azure-Funktion erstellt.
* Für die VM tritt ggf. ein Fehler mit dem Hinweis „Das Argument kann nicht an den Parameter 'VM Network Adapter' gebunden werden, da es NULL ist.“ auf.  Die erneute Bereitstellung des virtuellen Computers ist erfolgreich.  
* Das Löschen von Mandantenabonnements führt zu verwaisten Ressourcen.  Eine Problemumgehung besteht darin, zuerst Mandantenressourcen oder die gesamte Ressourcengruppe zu löschen und anschließend Mandantenabonnements zu löschen. 
* Sie müssen beim Erstellen eines Lastenausgleichs für das Netzwerk eine NAT-Regel erstellen. Andernfalls erhalten Sie nach der Erstellung des Lastenausgleichs einen Fehler, wenn Sie versuchen, eine NAT-Regel hinzuzufügen.
* Mandanten können virtuelle Computer erstellen, die die Kontingentvorgabe übersteigen.  Dieses Verhalten ist möglich, weil Computekontingente nicht erzwungen werden.
* Mandanten haben die Option, einen virtuellen Computer mit georedundantem Speicher zu erstellen.  Diese Konfiguration führt dazu, dass bei der Erstellung des virtuellen Computers ein Fehler auftritt.
* Es kann bis zu einer Stunde dauern, bevor Mandanten Datenbanken in einer neuen SQL SKU oder MySQL SKU erstellen können. 
* Die direkte Erstellung von Elementen auf SQL- und MySQL-Hostservern, die nicht vom Ressourcenanbieter durchgeführt wird, wird nicht unterstützt und kann zu einem Konfliktzustand führen.
* Für AzureRM PowerShell 1.2.10 sind zusätzliche Konfigurationsschritte erforderlich:
    * Führen Sie Folgendes aus, nachdem Sie Add-AzureRMEnvironment für Azure AD-Bereitstellungen ausgeführt haben.  Geben Sie die Werte für Name und GraphAudience an, indem Sie die Ausgabe von `Add-AzureRMEnvironment` verwenden.
      
      ```PowerShell
      Set-AzureRmEnvironment -Name <Environment Name> -GraphAudience <Graph Endpoint URL>
      ```
    * Führen Sie Folgendes aus, nachdem Sie Add-AzureRMEnvironment für AD FS-Bereitstellungen ausgeführt haben.  Geben Sie die Werte für Name und GraphAudience an, indem Sie die Ausgabe von `Add-AzureRMEnvironment` verwenden.
      
      ```PowerShell
      Set-AzureRmEnvironment <Environment Name> -GraphAudience <Graph Endpoint URL> -EnableAdfsAuthentication:$true
      ```
    
    Beispielsweise kann für eine Azure AD-Umgebung Folgendes verwendet werden:

    ```PowerShell
      Set-AzureRmEnvironment AzureStack -GraphAudience https://graph.local.azurestack.external/
    ```

#### <a name="fabric"></a>Fabric
* Der Compute-Ressourcenanbieter zeigt einen unbekannten Status an.
* Die IP-Adresse und das Modell für BMC werden nicht in den Hauptinformationen eines Skalierungseinheitknotens angezeigt.  Dies ist das erwartete Verhalten für das Azure Stack Development Kit.
* Die Neustartaktion sollte für die Computecontrollerinfrastruktur-Rolle (AzS-XRP01-Instanz) nicht verwendet werden.
* Das Blatt „Infrastructure backup“ (Infrastruktursicherung) sollte nicht verwendet werden.


<properties
	pageTitle="Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure Key Vault | Microsoft Azure"
	description="Hier erhalten Sie Informationen zur Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure Key Vault."
	services="key-vault"
	documentationCenter=""
	authors="adamglick"
	manager="danpl"
	editor=""/>

<tags
	ms.service="key-vault"
	ms.workload="key-vault"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/17/2016"
	ms.author="sumedhb;aglick"/>
	

# Azure Key Vault: Verfügbarkeit und Redundanz

Azure Key Vault bietet mehrere Redundanzebenen, um sicherzustellen, dass Ihre Schlüssel bzw. geheimen Schlüssel Ihrer Anwendung zur Verfügung stehen, auch wenn einzelne Komponenten des Dienstes ausfallen sollten.

Der Inhalt Ihres Schlüsseltresors wird innerhalb der Region sowie in eine sekundäre Region repliziert, die innerhalb desselben Gebiets mindestens 240 km entfernt ist. Dadurch wird eine sehr hohe Dauerhaftigkeit Ihrer Schlüssel und geheimen Schlüssel sichergestellt.

Wenn einzelne Komponenten innerhalb des Key Vault-Diensts ausfallen, springen andere Komponenten in der Region ein, um Ihre Anforderung zu erfüllen. Es gibt in diesem Fall keine Beeinträchtigung der Funktionalität. Sie müssen keine Maßnahmen ergreifen, da diese Schritte automatisch und transparent für Sie erfolgen.

In dem seltenen Fall, dass eine gesamte Azure-Region nicht verfügbar sein sollte, werden die Anforderungen an den Schlüsseltresor in dieser Region automatisch an eine sekundäre Region weitergeleitet (was als Failover bezeichnet wird). Wenn die primäre Region wieder verfügbar ist, werden Anforderungen wieder zurück an die primäre Region geleitet (was als Failback bezeichnet wird). Sie müssen wiederum keine Maßnahmen ergreifen, da diese Schritte automatisch erfolgen. Es sind verschiedene Einschränkungen, die Sie kennen sollten:

  * Beim Failover einer Region kann es einige Minuten dauern, bis das Failover des Diensts erfolgt. Anforderungen während dieser Zeit können erfolglos bleiben, bis das Failover abgeschlossen ist.

  * Nachdem ein Failover abgeschlossen wurde, befindet sich der Schlüsseltresor im Modus ___Schreibgeschützt___. Folgende Anforderungen werden in diesem Modus unterstützt:
    * Auflisten von Schlüsseltresoren
    * Abrufen von Eigenschaften von Schlüsseltresoren
    * Auflisten geheimer Schlüssel 
    * Abrufen geheimer Schlüssel
    * Auflisten von Schlüsseln
    * Abrufen (von Eigenschaften) von Schlüsseltresoren
    * Verschlüsseln
    * Entschlüsseln
    * Umschließen
    * Aufheben der Umschließung
    * Überprüfen
    * Signieren
    * Sichern

  * Sobald auf ein Failover ein Failback erfolgt ist, stehen alle Anforderungstypen (d. h. Lese- ___und___ Schreibanforderungen) wieder zur Verfügung.

<!---HONumber=AcomDC_0525_2016-->
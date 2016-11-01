<properties
	pageTitle="Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure Key Vault | Microsoft Azure"
	description="Hier erhalten Sie Informationen zur Vorgehensweise bei einer Azure-Dienstunterbrechung mit Auswirkungen auf Azure Key Vault."
	services="key-vault"
	documentationCenter=""
	authors="adamglick"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="key-vault"
	ms.workload="key-vault"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/26/2016"
	ms.author="sumedhb;aglick"/>


# Azure Key Vault: Verfügbarkeit und Redundanz

Azure Key Vault-Features bieten mehrere Redundanzebenen, um sicherzustellen, dass Ihre Schlüssel bzw. geheimen Schlüssel Ihrer Anwendung auch dann zur Verfügung stehen, wenn einzelne Komponenten des Dienstes ausfallen sollten.

Der Inhalt Ihres Schlüsseltresors wird innerhalb der Region sowie in eine sekundäre Region repliziert, die mindestens 240 km entfernt ist (jedoch innerhalb des gleichen Gebiets liegt). Dies gewährleistet eine sehr hohe Dauerhaftigkeit Ihrer Schlüssel und geheimen Schlüssel.

Wenn einzelne Komponenten innerhalb des Key Vault-Diensts ausfallen, springen andere Komponenten in der Region ein, um Ihre Anforderung zu erfüllen, sodass die Funktionalität nicht beeinträchtigt wird. Sie müssen keine Aktion ausführen, um dies auszulösen. Es geschieht automatisch und von Ihnen unbemerkt.

In dem seltenen Fall, dass eine gesamte Azure-Region nicht verfügbar sein sollte, werden die Anforderungen an den Azure Key Vault in dieser Region automatisch an eine sekundäre Region weitergeleitet (was als Failover bezeichnet wird). Wenn die primäre Region wieder verfügbar ist, werden Anforderungen wieder zurück an die primäre Region geleitet (was als Failback bezeichnet wird). Sie müssen wiederum keine Maßnahmen ergreifen, da diese Schritte automatisch erfolgen.

Es gibt verschiedene Einschränkungen, die Sie kennen sollten:

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
* Sobald auf ein Failover ein Failback erfolgt ist, stehen alle Anforderungstypen (d.h. Lese- ___und___ Schreibanforderungen) wieder zur Verfügung.

<!---HONumber=AcomDC_0831_2016-->
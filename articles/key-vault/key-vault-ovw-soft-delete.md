---
ms.assetid: 
title: "Vorläufiges Löschen in Azure Key Vault | Microsoft Docs"
ms.service: key-vault
author: BrucePerlerMS
ms.author: bruceper
manager: mbaldwin
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 5e92b1b234e4ceea5e0dd5d09ab3203c4a86f633
ms.openlocfilehash: 2b73fb55384cdcc8db2736557e0efef97b34fc45
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017

---
# <a name="azure-key-vault-soft-delete-feature-overview"></a>Übersicht über die Azure Key Vault-Funktion für vorläufiges Löschen

>[!NOTE]
>Bei dieser Vorschauversion von Azure Key Vault ist nur die Funktion für **vorläufiges Löschen** in der Vorschau. Insgesamt ist Azure Key Vault ein vollständiger Produktionsdienst.

In Bezug auf diese Vorschauversion von Azure Key Vault beschreiben wir das wiederherstellbare Löschen von Schlüsseltresoren und Key Vault-Objekten, das als vorläufiges Löschen bezeichnet wird. Insbesondere werden die folgenden Szenarien behandelt:

- Unterstützung für das wiederherstellbare Löschen von Schlüsseltresoren
- Unterstützung für das wiederherstellbare Löschen von Key Vault-Objekten (z. B. von Schlüsseln, geheimen Schlüsseln und Zertifikaten)

## <a name="supporting-interfaces"></a>Unterstützende Schnittstellen

Die Funktion für vorläufiges Löschen ist zunächst über die REST-, .NET-/C#- und PowerShell-Schnittstellen verfügbar. Ausführliche Informationen zu diesen Schnittstellen finden Sie in der [Referenz für Key Vault](https://docs.microsoft.com/azure/key-vault/).

## <a name="scenarios"></a>Szenarien

Azure-Schlüsseltresore sind nachverfolgte Ressourcen, die von Azure Resource Manager verwaltet werden. Azure Resource Manager gibt auch ein klar definiertes Verhalten für Löschvorgänge an, das erfordert, dass ein erfolgreicher DELETE-Vorgang dazu führt, dass auf die jeweilige Ressource nicht mehr zugegriffen werden kann. Bei der Funktion für vorläufiges Löschen geht es um die Wiederherstellung des gelöschten Objekts, unabhängig davon, ob der Löschvorgang versehentlich oder absichtlich erfolgte.

1. In einem typischen Szenario hat ein Benutzer einen Schlüsseltresor oder ein Key Vault-Objekt versehentlich gelöscht. Wenn der Schlüsseltresor oder das Key Vault-Objekt für einen zuvor festgelegten Zeitraum wiederherstellbar ist, kann der Benutzer den Löschvorgang möglicherweise rückgängig machen und die Daten wiederherstellen.

2. In einem anderen Szenario versucht ein nicht autorisierter Benutzer, einen Schlüsseltresor oder ein Key Vault-Objekt (z. B. einen Schlüssel in einem Tresor) zu löschen, um eine Unterbrechung des Geschäftsbetriebs zu verursachen. Die Trennung der Löschung des Schlüsseltresors oder Key Vault-Objekts vom tatsächlichen Löschen der zugrunde liegenden Daten kann als Sicherheitsmaßnahme verwendet werden, indem z. B. die Berechtigungen für das Löschen von Daten auf eine andere vertrauenswürdige Rolle beschränkt werden. Dies erfordert effektiv ein Quorum für einen Vorgang, der andernfalls zu einem sofortigen Datenverlust führen könnte.

### <a name="soft-delete-behavior"></a>Verhalten des vorläufigen Löschens

Bei dieser Vorschau ist der DELETE-Vorgang für einen Schlüsseltresor oder ein Key Vault-Objekt eine vorläufige Löschung, bei der die Ressourcen während einer bestimmten Aufbewahrungsdauer effektiv gespeichert werden, das Objekt aber scheinbar gelöscht wird. Der Dienst bietet darüber hinaus einen Mechanismus zur Wiederherstellung des gelöschten Objekts, bei dem der Löschvorgang im Wesentlichen rückgängig gemacht wird. 

Vorläufiges Löschen ist ein optionales Key Vault-Verhalten und in dieser Version **nicht standardmäßig aktiviert**. Ausführliche Informationen zum Aktivieren des vorläufigen Löschens finden Sie in den speziellen Anweisungen in der Referenz für die Schnittstelle Ihrer Wahl in der [Referenz für Key Vault](https://docs.microsoft.com/azure/key-vault/).

### <a name="key-vault-recovery"></a>Wiederherstellung eines Schlüsseltresors

Beim Löschen eines Schlüsseltresors erstellt der Dienst eine Proxyressource unter dem Abonnement, wobei genügend Metadaten für die Wiederherstellung hinzugefügt werden. Die Proxyressource ist ein gespeichertes Objekt, das am gleichen Speicherort wie der gelöschte Schlüsseltresor verfügbar ist. 

### <a name="key-vault-object-recovery"></a>Wiederherstellung eines Key Vault-Objekts

Beim Löschen eines Key Vault-Objekts, beispielsweise eines Schlüssels, versetzt der Dienst das Objekt in einen gelöschten Zustand, sodass keine Abrufvorgänge mehr darauf zugreifen können. Während sich das Key Vault-Objekt in diesem Zustand befindet, kann es nur aufgelistet, wiederhergestellt oder zwangsweise/endgültig gelöscht werden. 

Gleichzeitig plant Key Vault die Ausführung des Löschens der zugrunde liegenden Daten, die dem gelöschten Schlüsseltresor oder Key Vault-Objekt entsprechen, nach einem zuvor festgelegten Aufbewahrungsintervall. Der dem Tresor entsprechende DNS-Eintrag wird ebenfalls für die Dauer des Aufbewahrungsintervalls beibehalten.

### <a name="soft-delete-retention-period"></a>Aufbewahrungszeitraum für vorläufig gelöschte Ressourcen

Vorläufig gelöschte Ressourcen werden für einen festgelegten Zeitraum von 90 Tagen beibehalten. 

>[!NOTE]
> Dieser Aufbewahrungszeitraum für vorläufig gelöschte Ressourcen ist in der Preview-Version vom 10. Mai 2017 nicht konfigurierbar. 

Während des Aufbewahrungsintervalls für vorläufig gelöschte Ressourcen gilt Folgendes:

- Sie können alle im vorläufig gelöschten Zustand befindlichen Schlüsseltresore und Key Vault-Objekte für Ihr Abonnement auflisten sowie auf die entsprechenden Löschungs- und Wiederherstellungsinformationen zugreifen.
    - Nur Benutzer mit speziellen Berechtigungen können gelöschte Tresore auflisten. Wir empfehlen unseren Benutzern das Erstellen einer benutzerdefinierten Rolle mit diesen speziellen Berechtigungen für den Umgang mit gelöschten Tresoren.
- Es kann kein Schlüsseltresor mit demselben Namen am gleichen Speicherort erstellt werden. Dementsprechend kann auch kein Key Vault-Objekt in einem bestimmten Tresor erstellt werden, wenn dieser Schlüsseltresor ein Objekt mit demselben Namen enthält, das sich in einem gelöschten Zustand befindet. 
- Nur ein Benutzer mit speziellen Berechtigungen kann einen Schlüsseltresor oder ein Key Vault-Objekt durch Ausführen eines Wiederherstellungsbefehls für die entsprechende Proxyressource wiederherstellen.
    - Der Benutzer, ein Mitglied der benutzerdefinierten Rolle, der über die Berechtigung zum Erstellen eines Schlüsseltresors unter der Ressourcengruppe verfügt, kann den Tresor wiederherstellen.
- Nur ein Benutzer mit speziellen Berechtigungen kann das Löschen eines Schlüsseltresors oder Key Vault-Objekts durch Ausführen eines Löschbefehls für die entsprechende Proxyressource erzwingen.

Sofern ein Schlüsseltresor oder Key Vault-Objekt nicht wiederhergestellt wird, löscht der Dienst am Ende des Aufbewahrungsintervalls den vorläufig gelöschten Schlüsseltresor oder das vorläufig gelöschte Key Vault-Objekt und dessen Inhalt endgültig. Das Löschen von Ressourcen kann nicht neu geplant werden.

### <a name="permissioned-purge"></a>Berechtigungen für endgültiges Löschen

Das endgültige Löschen eines Schlüsseltresors kann über einen POST-Vorgang für die Proxyressource erfolgen und erfordert spezielle Berechtigungen. Im Allgemeinen kann nur der Besitzer des Abonnements einen Schlüsseltresor endgültig löschen. Der POST-Vorgang löst die sofortige Löschung dieses Tresors aus, die nicht rückgängig gemacht werden kann. 

Eine Ausnahme davon besteht, wenn das Azure-Abonnement als *nicht löschbar* markiert wurde. In diesem Fall kann der eigentliche Löschvorgang nur vom Dienst ausgeführt werden, und dies erfolgt als geplanter Prozess. 





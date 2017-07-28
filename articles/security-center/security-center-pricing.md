---
title: Security Center-Preise | Microsoft Docs
description: "Dieser Artikel enthält Informationen zu den Preisen für Azure Security Center."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 4d1364cd-7847-425a-bb3a-722cb0779f78
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: terrylan
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 367b8f38cb9fcf3dc36db83641cb1696710608ef
ms.contentlocale: de-de
ms.lasthandoff: 06/17/2017


---
# <a name="azure-security-center-pricing"></a>Azure Security Center-Preise
Azure Security Center unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen durch größere Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

## <a name="pricing-tiers"></a>Tarife
Security Center wird in zwei Tarifen angeboten:

* Der **Free-Tarif** wird automatisch für alle Azure-Abonnements aktiviert. Der Free-Tarif bietet einen Einblick in den Sicherheitszustand Ihrer Azure-Ressourcen, grundlegende Sicherheitsrichtlinien, Empfehlungen zur Sicherheit und die Integration in Sicherheitsprodukte und -Dienste von Partnern.
* Mit dem **Standard-Tarif** werden erweiterte Funktionen zur Bedrohungserkennung hinzugefügt, darunter Informationen zu Bedrohungen, Verhaltensanalysen, Erkennung von Anomalien, Sicherheitsincidents und Berichte zur Bewertung von Bedrohungen. Der Standard-Tarif wird für die ersten 60 Tage kostenlos angeboten.

Weitere Informationen finden Sie auf der Seite mit den [Preisen für Security Center](https://azure.microsoft.com/pricing/details/security-center/).

## <a name="try-standard-free-for-60-days"></a>Kostenloses Testen des Standard-Tarifs für 60 Tage
Der Standard-Tarif wird für die ersten 60 Tage kostenlos angeboten. Wenn Sie den Dienst über diese 60 Tage hinaus verwenden, beginnen wir automatisch mit der Berechnung des Diensts.

So erhalten Sie den Standard-Tarif:

1. Wählen Sie auf dem Blatt **Security Center** die Kachel **Richtlinie** aus.
2. Wählen Sie das Abonnement aus, das Sie auf den Standard-Tarif umstellen möchten.
3. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie** die Option **Tarif** aus.
4. Wählen Sie auf dem Blatt **Tarif auswählen** die Option **Standard** aus.
5. Klicken Sie auf **Auswählen**.


## <a name="why-upgrade-to-standard"></a>Gründe für ein Upgrade auf den Standard-Tarif
Der Standard-Tarif von Security Center bietet alle Funktionen des Free-Tarifs und zusätzlich die erweiterte Bedrohungserkennung. Die erweiterte Bedrohungserkennung ermöglicht die Identifizierung von aktiven Bedrohungen für Ihre Azure-Ressourcen und bietet Ihnen die erforderlichen Informationen, um schnell zu reagieren.

Für Security Center werden professionelle Sicherheitsanalysen genutzt, die weit über signaturbasierte Ansätze hinausgehen. Bahnbrechende Weiterentwicklungen der Big Data- und Machine Learning-Technologien kommen zum Einsatz, um Ereignisse im gesamten Cloudfabric auszuwerten. So lassen sich Bedrohungen erkennen, die bei Verwendung von manuellen Konzepten nicht identifiziert werden könnten, und die Entwicklung von Angriffen kann vorhergesagt werden.

Sicherheitsanalysen im Standard-Tarif:

* **Informationen zu Bedrohungen**: Es wird nach bekannten Angreifern gesucht. Hierzu werden globale Informationen zu Bedrohungen (Threat Intelligence) von Microsoft-Produkten und -Diensten, von der Microsoft Digital Crimes Unit, vom Microsoft Security Response Centersowie von externen Feeds genutzt.
* **Verhaltensanalyse**: Es werden bekannte Muster angewendet, um schädliches Verhalten zu erkennen.
* **Anomalieerkennung**: Es werden statistische Profile erstellt, um typische Verlaufsdaten zu erhalten. Sie werden benachrichtigt, wenn es zu Abweichungen von der Baseline kommt, die einem potenziellen Angriffsvektor entsprechen.

Im unten dargestellten Blatt **Sicherheitswarnungen** hat Security Center einen **Sicherheitsincident** festgestellt. Ein Sicherheitsincident ist eine Aggregation aller Warnungen für eine Ressource, die Kill Chain-Mustern entsprechen. Wenn Sie den Sicherheitsincident auswählen, werden ausführliche Informationen zum Incident und die dazugehörigen Warnungen angezeigt. Durch die Auswahl einer Warnung erhalten Sie weitere Informationen über den Vorfall.

![Sicherheitsvorfall][2]

Die unten dargestellt Warnung für die **Netzwerkkommunikation** enthält Details zur Warnung. Die Details umfassen die vollständige Beschreibung, den Schweregrad, den aktuellen Status (in diesem Fall „Verworfen“, der Benutzer hat also Maßnahmen ergriffen, um ihn zu schließen), die angegriffene Ressource und Wiederherstellungsschritte. Es gibt auch eine Liste mit Links zu Berichten von Microsoft mit Informationen zu Bedrohungen. Diese Berichte können als Sicherheitsmaßnahmen und für defensive Zwecke verwendet werden.

![Details der Sicherheitswarnung][3]

## <a name="enable-data-collection"></a>Aktivieren der Datensammlung
Um Verhaltensanalysen für virtuelle Computer zu aktivieren, muss die Datensammlung aktiviert sein.

So überprüfen Sie, ob die Datensammlung aktiviert ist:

1. Wählen Sie die Kachel **Richtlinie** aus. Das Blatt **Sicherheitsrichtlinie** wird geöffnet und zeigt Ihre Azure-Abonnements an.
2. Wählen Sie ein Abonnement aus.
3. Wenn **Datensammlung** deaktiviert ist, aktivieren Sie diese Option, und speichern Sie die Änderung.

> [!NOTE]
> Wenn Sie Azure Security Center Free verwenden, können Sie die Datensammlung von virtuellen Computern in der Sicherheitsrichtlinie deaktivieren. Die Datensammlung ist für Abonnements des Standard-Tarifs erforderlich.
>
>

Weitere Informationen finden Sie unter [Aktivieren der Datensammlung in Azure Security Center](security-center-enable-data-collection.md).

## <a name="next-steps"></a>Nächste Schritte
* In diesem Dokument wurden die Preise für Security Center vorgestellt. Weitere Preisinformationen finden Sie auf der Seite mit den [Preisen für Security Center](https://azure.microsoft.com/pricing/details/security-center/).
* Weitere Informationen zu den Funktionen für die erweiterte Erkennung von Security Center finden Sie unter [Azure Security Center-Erkennungsfunktionen](security-center-detection-capabilities.md).
* Weitere Informationen zum Verwalten und Schützen von Daten in Security Center finden Sie unter [Azure Security Center-Datensicherheit](security-center-data-security.md).
* Antworten auf Fragen zur Verwendung von Security Center finden Sie unter [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md).
* Wenn Sie noch Fragen zur Verwendung von Security Center oder zu Azure haben, besuchen Sie die [Azure-Foren](https://social.msdn.microsoft.com/Forums/home?forum=AzureSecurityCenter&filter=alltypes&sort=lastpostdesc).

<!--Image references-->
[1]: ./media/security-center-pricing/standard.png
[2]: ./media/security-center-pricing/incident.png
[3]: ./media/security-center-pricing/network-alert.png


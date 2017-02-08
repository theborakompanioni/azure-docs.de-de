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
ms.date: 10/12/2016
ms.author: terrylan
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 533fd337ba16a61579b6f322771913cda36d8272


---
# <a name="azure-security-center-pricing"></a>Azure Security Center-Preise
Azure Security Center unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen durch größere Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Azure-Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Spektrum an Sicherheitslösungen verwendet werden.

## <a name="pricing-tiers"></a>Tarife
Security Center wird in zwei Tarifen angeboten:

* Der **Free-Tarif** wird automatisch für alle Azure-Abonnements aktiviert. Der Free-Tarif bietet einen Einblick in den Sicherheitszustand Ihrer Azure-Ressourcen, grundlegende Sicherheitsrichtlinien, Empfehlungen zur Sicherheit und die Integration in Sicherheitsprodukte und -Dienste von Partnern.
* Mit dem **Standard-Tarif** werden erweiterte Funktionen zur Bedrohungserkennung hinzugefügt, darunter Informationen zu Bedrohungen, Verhaltensanalysen, Erkennung von Anomalien, Sicherheitsincidents und Berichte zur Bewertung von Bedrohungen. Für den Standard-Tarif ist eine kostenlose **90-Tage-Testversion** verfügbar.

Weitere Informationen finden Sie auf der Seite mit den [Preisen für Security Center](https://azure.microsoft.com/pricing/details/security-center/).

> [!NOTE]
> Die sicherheitsbezogenen Daten, die von Ihren geschützten Knoten generiert werden, werden von Security Center in Azure Storage gespeichert. Die für diesen Speicher anfallenden Kosten sind nicht im Preis dieses Diensts enthalten und werden separat in Rechnung gestellt (zu den üblichen [Preisen für Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/)). Speichergebühren fallen auch bei Verwendung der Testversion an.
> 
> 

## <a name="try-standard-free-for-90-days"></a>Kostenloses Testen des Standard-Tarifs für 90 Tage
Für den Standard-Tarif ist eine kostenlose 90-Tage-Testversion verfügbar. Um die kostenlose Testversion für den Standard-Tarif zu erhalten, wählen Sie die Kachel **Richtlinie** auf dem Blatt **Security Center** aus. Wählen Sie das Abonnement aus, das Sie auf den Standard-Tarif umstellen möchten. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie** die Option **Tarif** aus. Wählen Sie auf dem Blatt **Tarif auswählen** die Option **Standard – kostenlose Testversion** aus.

![Kostenlose Testversion][1]

Nach Ablauf der 90 Tage wird Ihnen die Nutzung in Rechnung gestellt (sofern Sie sich entscheiden, den Dienst weiterhin zu nutzen).

## <a name="upgrade-to-standard"></a>Umstellen auf den Standard-Tarif
Führen Sie ein Upgrade auf den Standard-Tarif durch, um die erweiterte Bedrohungserkennung hinzuzufügen. Um den Standard-Tarif zu erhalten, wählen Sie die Kachel **Richtlinie** auf dem Blatt **Security Center** aus. Wählen Sie das Abonnement aus, das Sie auf den Standard-Tarif umstellen möchten. Wählen Sie auf dem Blatt **Sicherheitsrichtlinie** die Option **Tarif** aus. Wählen Sie auf dem Blatt **Tarif auswählen** die Option **Standard** aus.

![Standard-Tarif][2]

## <a name="why-upgrade-to-standard"></a>Gründe für ein Upgrade auf den Standard-Tarif
Der Standard-Tarif von Security Center bietet alle Funktionen des Free-Tarifs und zusätzlich die erweiterte Bedrohungserkennung. Die erweiterte Bedrohungserkennung ermöglicht die Identifizierung von aktiven Bedrohungen für Ihre Azure-Ressourcen und bietet Ihnen die erforderlichen Informationen, um schnell zu reagieren.

Für Security Center werden professionelle Sicherheitsanalysen genutzt, die weit über signaturbasierte Ansätze hinausgehen. Bahnbrechende Weiterentwicklungen der Big Data- und Machine Learning-Technologien werden genutzt, um Ereignisse im gesamten Cloudfabric auszuwerten. So können Bedrohungen erkannt werden, die bei Verwendung von manuellen Konzepten nicht identifiziert werden können, und die Entwicklung von Angriffen kann vorhergesagt werden.

Sicherheitsanalysen im Standard-Tarif:

* **Informationen zu Bedrohungen**: Es wird nach bekannten Angreifern gesucht. Hierzu werden globale Informationen zu Bedrohungen (Threat Intelligence) von Microsoft-Produkten und -Diensten, von der Microsoft Digital Crimes Unit, vom Microsoft Security Response Centersowie von externen Feeds genutzt.
* **Verhaltensanalyse**: Es werden bekannte Muster angewendet, um schädliches Verhalten zu erkennen.
* **Anomalieerkennung**: Es werden statistische Profile erstellt, um typische Verlaufsdaten zu erhalten. Sie werden benachrichtigt, wenn es zu Abweichungen von der Baseline kommt, die einem potenziellen Angriffsvektor entsprechen.

Im unten dargestellten Blatt **Sicherheitswarnungen** hat Security Center einen **Sicherheitsincident** festgestellt. Ein Sicherheitsincident ist eine Aggregation aller Warnungen für eine Ressource, die Kill Chain-Mustern entsprechen. Wenn Sie den Sicherheitsincident auswählen, werden ausführliche Informationen zum Incident und die dazugehörigen Warnungen angezeigt. Durch die Auswahl einer Warnung erhalten Sie weitere Informationen über den Vorfall.

![Sicherheitsvorfall][3]

Die unten dargestellt Warnung für die **Netzwerkkommunikation** enthält Details zur Warnung. Die Details umfassen die vollständige Beschreibung, den Schweregrad, den aktuellen Status (in diesem Fall „Verworfen“, der Benutzer hat also Maßnahmen ergriffen, um ihn zu schließen), die angegriffene Ressource und Wiederherstellungsschritte. Es gibt auch eine Liste mit Links zu Berichten von Microsoft mit Informationen zu Bedrohungen. Diese Berichte können als Sicherheitsmaßnahmen und für defensive Zwecke verwendet werden.

![Details der Sicherheitswarnung][4]

## <a name="enable-data-collection"></a>Aktivieren der Datensammlung
Um Verhaltensanalysen für virtuelle Computer zu aktivieren, muss die Datensammlung aktiviert sein. Sie müssen möglicherweise beim ersten Zugriff auf Security Center oder bei der Erstellung einer Sicherheitsrichtlinie die Datensammlung aktivieren.

Um zu überprüfen, ob die Datensammlung aktiviert ist, wählen Sie die Kachel **Richtlinie** aus. Das Blatt **Sicherheitsrichtlinie** wird geöffnet und zeigt Ihre Azure-Abonnements an. Wählen Sie ein Abonnement aus. Wenn **Datensammlung** deaktiviert ist, aktivieren Sie diese Option, und speichern Sie die Änderung. Weitere Informationen finden Sie unter [Aktivieren der Datensammlung in Azure Security Center](security-center-enable-data-collection.md).

## <a name="next-steps"></a>Nächste Schritte
* In diesem Dokument wurden die Preise für Security Center vorgestellt. Weitere Preisinformationen finden Sie auf der Seite mit den [Preisen für Security Center](https://azure.microsoft.com/pricing/details/security-center/).
* Weitere Informationen zu den Funktionen für die erweiterte Erkennung von Security Center finden Sie unter [Azure Security Center-Erkennungsfunktionen](security-center-detection-capabilities.md).
* Antworten auf Fragen zur Verwendung von Security Center finden Sie unter [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md).
* Wenn Sie noch Fragen zur Verwendung von Security Center oder zu Azure haben, besuchen Sie die [Azure-Foren](https://social.msdn.microsoft.com/Forums/home?forum=AzureSecurityCenter&filter=alltypes&sort=lastpostdesc).

<!--Image references-->
[1]: ./media/security-center-pricing/free-trial.png
[2]: ./media/security-center-pricing/standard.png
[3]: ./media/security-center-pricing/incident.png
[4]: ./media/security-center-pricing/network-alert.png



<!--HONumber=Dec16_HO2-->



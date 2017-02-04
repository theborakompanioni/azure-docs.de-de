---
title: "Vorbereiten und Testen Ihres Angebots für die Bereitstellung im Azure Marketplace | Microsoft Docs"
description: "Detaillierte Informationen zum Bereitstellen von Marketinginhalten, Konfigurieren von Preisplänen und Testen des Angebots vor der Bereitstellung im Azure Marketplace."
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 3ccd2448-895b-477e-adf6-ab655a21d2fa
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: Azure
ms.workload: na
ms.date: 08/17/2016
ms.author: hascipio
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 7db86716cdf8f9eb921c3c1813970acae7a3016b


---
# <a name="complete-the-offer-creation-with-marketing-content"></a>Abschließen der Angebotserstellung mit Marketinginhalten
In diesem Schritt des Veröffentlichungsprozesses müssen Sie bestimmte Marketinginhalte sowie Details zu Ihrem Angebot und/oder SKUs im Azure Marketplace bereitstellen. So stellen Sie beispielsweise eine Beschreibung Ihres Produkts, Firmenlogos, Preispläne, detaillierte Informationen zu Plänen sowie weitere Informationen bereit, die zum Überführen Ihres Angebots und/oder Ihrer SKU in der Stagingumgebung erforderlich sind. Diese Informationen werden im Azure-Portal als Marketinginhalte verwendet. Sie starten diesen Prozess im [Veröffentlichungsportal][link-pubportal].

## <a name="step-1-provide-marketplace-marketing-content"></a>Schritt 1: Bereitstellen von Marketinginhalten im Marketplace
**Englisch ist die Standardsprache und die einzige unterstützte Sprache.** Geben Sie daher bitte alle Informationen in den Feldern auf Englisch ein. Alle Informationen können vor der Überführung in die Stagingumgebung jederzeit bearbeitet werden.

1. Wechseln Sie zum Veröffentlichungsportal: [https://publish.windowsazure.com](https://publish.windowsazure.com).
2. Klicken Sie im linken Menü auf die Registerkarte **Marketing** .
3. Klicken Sie im Hauptbereich auf die Schaltfläche **English (US)** .
   
   > [!IMPORTANT]
   > Alle Felder müssen ausgefüllt sein, einschließlich der Abbildungen, um das Angebot in die Stagingumgebung zu überführen.
   > 
   > 

### <a name="details-and-plans"></a>Details und Pläne
1. Geben Sie auf der Registerkarte **Details** den Angebotstitel (max. 50 Zeichen), eine kurze Angebotsübersicht (max. 100 Zeichen), eine ausführliche Angebotsübersicht (max. 256 Zeichen), eine Angebotsbeschreibung (max. 1300 Zeichen) sowie Logos ein.
2. Geben Sie auf der Registerkarte **Pläne** den Plantitel (max. 50 Zeichen), eine Planübersicht (max. 100 Zeichen) und eine Planbeschreibung (max. 2000 Zeichen) ein.
   
   > [!NOTE]
   > Sie können die Übersicht, die ausführliche Übersicht und die Beschreibung von Angebot und Plänen mit den folgenden HTML-Tags formatieren. Die zulässigen HTML-Tags sind h1, h2, h3, h4, h5, p, ol, ul, li, a[Ziel|HREF], strong, em, b, i.
   > 
   > 
3. Geben Sie für die Beschreibung des Angebots und des Plans nicht den gleichen Text ein.
4. Geben Sie für den Plantitel und die ausführliche Angebotsübersicht nicht den gleichen Text ein.
5. Geben Sie für den Plantitel und die Angebotsübersicht nicht den gleichen Text ein.
6. Geben Sie für ein Angebot mit mehreren Plänen keine identischen Plantitel ein.
7. Laden Sie Abbildungen gemäß den erforderlichen Spezifikationen (wie im Veröffentlichungsportal erläutert) im PNG-Format hoch. Laden Sie für jede gewünschte Größe ein Bild hoch.
8. Stellen Sie sicher, dass die Logos die unten beschriebenen Azure Marketplace-Richtlinien für Logos erfüllen.
   
   ![Abbildung](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-details-02.png)

**Azure Marketplace-Richtlinien für Logos**

Alle in das Veröffentlichungsportal hochgeladen Logos müssen folgende Richtlinien erfüllen:

* Die Farbpalette des Azure-Designs ist einfach und geradlinig. Verwenden Sie auf Ihrem Logo möglichst wenige Primär- und Sekundärfarben.
* Die Designfarben des Azure-Portals sind Weiß und Schwarz. Daher verwenden Sie diese Farben nicht als Hintergrundfarbe für Ihre Logos. Verwenden Sie Farben, die Ihre Logos im Azure-Portal markant hervorstechen lassen. Sie sollten einfache Primärfarben verwenden. **Wenn Sie transparenten Hintergrund verwenden, stellen Sie sicher, dass Logos/Text nicht weiß, schwarz oder blau sind.**
* Verwenden Sie im Logo keinen Hintergrund mit Farbverlauf.
* Platzieren Sie nach Möglichkeit keinen Text auf dem Logo, auch nicht Ihren Firmen- oder Markennamen. Das Erscheinungsbild Ihres Logos sollte klar und direkt und frei von Farbverläufen sein.
* Das Logo sollte nicht gestreckt sein.
* Ein kleines Logo sollte 40 x 40 px groß sein.
* Ein mittleres Logo sollte 90 x 90 px groß sein.
* Ein großes Logo sollte 115 x 115 px groß sein.
* Ein breites Logo sollte 255 x 115 px groß sein.
* Ein Herologo sollte 815 x 290 px groß sein.

> [!NOTE]
> Ein Herologo ist optional. Der Herausgeber muss kein Herologo hochladen. Ist das Herosymbol jedoch einmal hochgeladen, dann kann es nicht mehr aus dem Veröffentlichungsportal gelöscht werden. Zu diesem Zeitpunkt muss der Partner die Azure Marketplace-Richtlinien für Herosymbole einhalten.
> 
> 

  ![Abbildung](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-details-03.png)

**Zusätzliche Richtlinien für das Herologosymbol (optional)**

* Ein Herologo ist optional. Der Herausgeber muss kein Herologo hochladen. **Ist das Herosymbol jedoch einmal hochgeladen, dann kann es nicht mehr aus dem Veröffentlichungsportal gelöscht werden. Zu diesem Zeitpunkt muss der Partner die Azure Marketplace-Richtlinien für Herosymbole einhalten, sonst wird das Angebot nicht zur Produktion genehmigt.**
* Anzeigename des Herausgebers, Plantitel und ausführliche Angebotsübersicht werden in weißer Schrift angezeigt. Daher sollten Sie helle Farben im Hintergrund des Herosymbols vermeiden. Schwarzer, weißer und transparenter Hintergrund ist für Herosymbole nicht zulässig.
* Der Anzeigename des Herausgebers, der Plantitel, die ausführliche Angebotsübersicht und die Erstellen-Schaltfläche werden programmgesteuert in das Herologo eingebettet, wenn das Angebot live geschaltet wird. Sie sollten beim Entwerfen des Herologos keinen Text eingeben. Lassen Sie einfach Platz auf der rechten Seite, da der Text (also Anzeigename des Herausgebers, Plantitel, ausführliche Angebotsübersicht) von uns dort programmgesteuert eingefügt wird. Der Leerraum für den Text sollte 415 x 100 auf der rechten Seite betragen (und ist 370 Pixel von links versetzt).
  
  ![Abbildung](media/marketplace-publishing-push-to-staging/pubportal-herobanner.png)

### <a name="links"></a>Links
Geben Sie auf der Registerkarte **Links** auf der linken Seite Links mit Informationen an, die Kunden weiterhelfen könnten. Geben Sie für jeden Link einen Namen und eine URL an.

![Abbildung](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-link-01.png)

### <a name="sample-images-optional"></a>Beispielbilder (optional)
> [!NOTE]
> Das Hinzufügen von Beispielbildern ist ein optionaler Schritt.
> Sie können zwar mehrere Beispielbilder im Veröffentlichungsportal hochladen, es wird jedoch nur ein (nach dem Zufallsprinzip vom System ausgewähltes) Bild im Azure-Portal angezeigt. Aus diesem Grund sollten Sie nur ein einziges Beispielbild hochladen.
> 
> 

Laden Sie auf der Registerkarte **Beispielbilder** im Menü auf der linken Seite ein neues Bild hoch, indem Sie auf **Neues Bild hochladen** klicken. Wenn Sie ein vorhandenes Bild austauschen möchten, klicken Sie auf **Bild ersetzen**.

![Abbildung](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-sampleimg-01.png)

### <a name="legal"></a>Rechtliche Hinweise
Geben Sie auf der Registerkarte **Rechtliche Hinweise** einen Link zu Ihren Richtlinien/Nutzungsbedingungen an. Geben Sie die Bedingungen im großen Feld **Nutzungsbedingungen** ein, oder kopieren Sie sie in das Feld. Die Nutzungsbedingungen dürfen maximal 1.000.000 Zeichen umfassen.

![Abbildung](media/marketplace-publishing-push-to-staging/pubportal-marketingcontent-legal-01.png)

**Hinweis:** Für VM-Angebote gilt: Sobald ein Angebot/eine SKU im Azure-Portal bereitgestellt wird, können Sie die unten angegebenen Felder nicht ändern:

* **Angebotsbezeichner:** [Veröffentlichungsportal > „Virtual Machines“ > Ihr Angebot > Registerkarte „VM-Images“ > „Angebotsbezeichner“]
* **SKU-Bezeichner:** [Veröffentlichungsportal > „Virtual Machines“ > Ihr Angebot > Registerkarte „SKUs“ > „SKU hinzufügen“]
* **Herausgebernamespace:** [Veröffentlichungsportal > „Virtual Machines“ > Registerkarte „Vorgehensweise“ > „Informationen zu Ihrem Unternehmen“ (unter „Schritt 2: Registrieren Sie Ihr Unternehmen“) > „Herausgebernamespace“ > „Namespace“]

Für VM-Angebote gilt: Sobald ein Angebot/eine SKU im Azure Marketplace aufgeführt wird, können Sie die unten angegebenen Felder nicht ändern:

* **Angebotsbezeichner:** [Veröffentlichungsportal > „Virtual Machines“ > Ihr Angebot > „VM-Images“ > „Angebotsbezeichner“]
* **SKU-Bezeichner:** [Veröffentlichungsportal > „Virtual Machines“ > Ihr Angebot > Registerkarte „SKUs“ > „SKU hinzufügen“]
* **Herausgebernamespace:** [Veröffentlichungsportal > „Virtual Machines“ > Registerkarte „Vorgehensweise“ > „Informationen zu Ihrem Unternehmen“ (unter „Schritt 2: Registrieren“) > „Herausgebernamespace“ > „Namespace“]
* **Ports:** [Veröffentlichungsportal > „Virtual Machines“ > Ihr Angebot > Registerkarte „VM-Images“ > „Offene Ports“]
* **Preisänderung bei aufgelisteten SKUs**
* **Abrechnungsmodelländerung bei aufgelisteten SKUs**
* **Entfernen von Abrechnungsregionen bei aufgelisteten SKUs**
* **Ändern der Anzahl der Datenträger bei aufgelisteten SKUs**

## <a name="step-2-set-your-prices"></a>Schritt 2: Festlegen der Preise
### <a name="pricing-models"></a>Preismodelle
| Preismodell | Beschreibung |
| --- | --- |
| Basis |Monatliche Pauschale, Bezahlung zum Zeitpunkt des Kaufs, z. B. 10 USD/Monat. |
| Verbrauch (auch als Nutzung oder Verwendung bezeichnet) |Nutzungsbasierte Bezahlung, die vom Herausgeber definiert wird. Eine Überschreitung kann nicht pro Arbeitsplatz, pro Benutzer usw. definiert werden, da es kein Konzept eines Bruchteils von Benutzern oder Kapazitäten gibt, um die Nutzung anteilig zu verrechnen. Die Nutzung wird vom Partner auf Stundenbasis berechnet. Der Kunde zahlt die Gebühren monatlich im Nachhinein, im Gegensatz zur Vorauszahlung bei Monatsplänen. |
| Kostenlose Testversion |Der Kunde kann das Produkt oder den Dienst eine Zeit lang kostenlos testen und zahlt danach die normalen Gebühren. |
| Free-Tarif |Dieser Plan ist immer kostenlos. |
| Migration (Umwandlung oder Upgrade/Downgrade eines Plans) |Hierbei wechselt ein Benutzer aus dem aktuellen Plan in einen anderen akzeptablen Plan; wird vom Partner definiert. |

**Verfügbare Preismodelle nach Angebotstyp**

> [!IMPORTANT]
> Die Verfügbarkeit bestimmten Preismodelle variiert je nach Art Angebotstyp. Siehe hierzu die nachstehende Tabelle.
> 
> 

|  | Nur Basis | Nur Verbrauch | Basis und Verbrauch |
| --- | --- | --- | --- |
| VM-Image |Nein |Ja |Nein |
| Entwicklerdienst |Ja |Ja |Ja |

### <a name="21-set-your-vm-prices"></a>2.1. Festlegen der VM-Preise
Derzeit sind für virtuelle Computer **drei Arten von Abrechnungsmodellen**

* **Stündlich:** Die Abrechnung für den Kunden erfolgt auf Stundenbasis gemäß den Gebühren, die von den Herausgebern für die VM-Größen festgelegt sind. Beim **stündlichen Abrechnungsmodell** für die SKUs ergibt sich der Gesamtpreis aus der Summe der vom Herausgeber in Rechnung gestellten Softwarekosten und der von Microsoft in Rechnung gestellten Infrastrukturkosten. Diese Gesamtkosten werden dem Kunden als stündlicher und monatlicher Preis angezeigt, wenn er den Kauf erwägt (siehe Screenshot unten). **Der Herausgeber erhält 80% der in Rechnung gestellten Softwarekosten.** Bitte berücksichtigen Sie dies bei der Berechnung der Preise, die Sie für die SKUs festlegen.
  
    ![Abbildung](media/marketplace-publishing-push-to-staging/img2.1-01.png)
* **Kostenlose Testversion:** Hierbei handelt es sich um eine Abwandlung des stündlichen Abrechnungsmodells. Hier werden dem Kunde für die ersten 30 Tage nach der Bereitstellung des virtuellen Computers keine Softwarekosten in Rechnung gestellt. Nach 30 Tagen erfolgt die Abrechnung für den Kunden auf Stundenbasis gemäß der Gebühren, die von den Herausgebern im stündlichen Abrechnungsmodell festgelegt sind.
* **Bring-Your-Own-License (BYOL):** Die Herausgeber verwalten die Lizenzierung der auf dem virtuellen Computer ausgeführten Software.

**Wichtig:** Sobald ein Angebot/eine SKU im Azure Marketplace angeboten wird, können die unten angegebenen Felder nicht mehr geändert werden:

* **Preisänderung bei aufgelisteten SKUs**
* **Abrechnungsmodelländerung bei aufgelisteten SKUs**
* **Entfernen von Abrechnungsregionen bei aufgelisteten SKUs**
* **Ändern der Anzahl der Datenträger bei aufgelisteten SKUs**
* **Angebotsbezeichner:** [Veröffentlichungsportal > „Virtual Machines“ > Ihr Angebot > „VM-Images“ > „Angebotsbezeichner“]
* **SKU-Bezeichner:** [Veröffentlichungsportal > „Virtual Machines“ > Ihr Angebot > Registerkarte „SKUs“ > „SKU hinzufügen“]
* **Herausgebernamespace:** [Veröffentlichungsportal > „Virtual Machines“ > Registerkarte „Vorgehensweise“ > „Informationen zu Ihrem Unternehmen“ (unter „Schritt 2: Registrieren“) > „Herausgebernamespace“ > „Namespace“]
* **Ports:** [Veröffentlichungsportal > „Virtual Machines“ > Ihr Angebot > Registerkarte „VM-Images“ > „Offene Ports“]

### <a name="sell-to-countries-of-the-sku"></a>„Sell-to“-Länder der SKU
Sie müssen sorgfältig überlegen, wo Sie die SKUs zur Verfügung stellen. Manche Länder sind klassifiziert als „Überweisung durch Microsoft“ und andere klassifiziert als „Überweisung durch ISV“.

* In „Überweisung durch Microsoft“-Ländern nimmt Microsoft die Steuern von den Kunden ein und zahlt (überweist) die Steuern an den Staat.
* In „Überweisung durch ISV“-Ländern sind die Partner für die Einnahme der Steuern bei den Kunden und die Steuerzahlung an den Staat verantwortlich. Wenn Sie sich für den Verkauf in „Überweisung durch ISV“-Ländern entscheiden, müssen Sie über die Möglichkeit verfügen, die Steuern für die Länder Ihrer Wahl zu berechnen und zu zahlen.

> [!NOTE]
> Ihre SKUs werden erst in den Ländern verfügbar, wenn Sie ihre Preise im [Veröffentlichungsportal](https://publish.windowsazure.com)festlegen. Anleitungen zum Festlegen der Preise für SKUs gemäß stündlicher Abrechnung und BYOL finden Sie im Folgenden.
> 
> 

### <a name="211-how-to-setup-hourly-pricing-model-for-a-sku"></a>2.1.1 Einrichten des stündlichen Abrechnungsmodells für eine SKU
Bitte führen Sie die folgenden Schritte aus, um ein stündliches Abrechnungsmodell für eine SKU einzurichten:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SKUs** .
4. Stellen Sie sicher, dass die SKU als „Stündliches Abrechnungsmodell“ gekennzeichnet ist. Klicken Sie andernfalls auf die Schaltfläche **BEARBEITEN** , um das Abrechnungsmodell zurückzusetzen. Daraufhin wird ein Fenster geöffnet. Deaktivieren Sie das Kontrollkästchen „Abrechnung und Lizenzierung erfolgt extern durch Azure (auch bekannt als Bring-Your-Own-License)“, und speichern Sie die Änderungen.
5. Wenn Sie den kostenlosen Test für die ersten 30 Tage der SKU-Bereitstellung aktivieren möchten, wählen Sie die Option „Ein Monat“ auf die Frage: „Ist ein kostenloser Test verfügbar?“ Wählen Sie andernfalls die Option „Kein Test“. Führen Sie jetzt die folgenden Schritte aus.
6. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **PREISE** .
7. Wählen Sie Ihre Basisregion.
   
   ![Abbildung](media/marketplace-publishing-push-to-staging/img2.1.1_07.png)
8. Legen Sie die Preise für alle Kerne fest. **Sie müssen die Preise aller Kerne einer SKU angeben, auch wenn die SKU dies nicht unterstützt.**
   
    ![Abbildung](media/marketplace-publishing-push-to-staging/img2.1.1_08.png)
9. Legen Sie die Preise für die anderen Regionen manuell fest, oder legen Sie die Preise für andere Regionen mit dem AUTOPRICE-Assistenten für automatische Preisfestlegung gemäß der Basisregion fest. Wenn Sie den AUTOPRICE-Assistenten für die automatische Preisfestlegung verwenden möchten, klicken Sie auf die Schaltfläche **AUTOPRICE OTHER MARKETS BASED ON PRICES IN UNITED STATES** (Preise für andere Märkte basierend auf Preisen in den USA automatisch festlegen). **Hinweis:** Die Beschriftung der Schaltfläche kann abhängig von der ausgewählten Region unterschiedlich sein. Da wir während der Erstellung dieses Dokuments die USA ausgewählt haben, trägt die Schaltfläche im Screenshot unten die Beschriftung „Auto price other markets based on prices in United States“.
   
   ![Abbildung](media/marketplace-publishing-push-to-staging/img2.1.1_09.png)
10. Der Assistent für automatische Preisfestlegung wird geöffnet. Die erste Seite zeigt die Auswahl für den Basismarkt an. Treffen Sie Ihre Wahl, und gehen Sie durch Klicken auf die Schaltfläche „->“ zur nächsten Seite.
    
    ![Abbildung](media/marketplace-publishing-push-to-staging/img2.1.1_10.png)
11. Die Option zum Auswählen der Kerne und Pläne wird auf Seite 2 angezeigt. Wählen Sie die gewünschten Pläne aus, und klicken Sie auf die Schaltfläche „->“. Klicken Sie auf die Schaltfläche **Alle umschalten**, um alle **Servicepläne** und **Messgeräte** auszuwählen, oder aktivieren Sie die Kontrollkästchen manuell. **Sie müssen die Preise aller Kerne einer SKU angeben, auch wenn die SKU dies nicht unterstützt.** So stellen Sie sicher, dass jede Kerngröße ausgewählt wird.
    
    ![Abbildung](media/marketplace-publishing-push-to-staging/img2.1.1_11.png)
12. Seite 3 zeigt die Märkte bzw. Regionen an. Klicken Sie auf die Schaltfläche **Alle umschalten**, um alle Regionen auszuwählen, oder aktivieren Sie die Kontrollkästchen für die Regionen manuell. Klicken Sie auf die Schaltfläche „->“, um zur nächsten Seite zu wechseln. **Hinweis:** Länder, in denen die Steuerüberweisung durch Microsoft erfolgt, sind durch ein hausähnliches Symbol gekennzeichnet. Weitere Informationen finden Sie im Abschnitt „‚Sell-to‘- Länder der SKU“ auf dieser Seite.
    
    ![Abbildung](media/marketplace-publishing-push-to-staging/img2.1.1_12.png)
13. Seite 4 zeigt die Wechselkurse an. Klicken Sie auf die Schaltfläche „Fertig stellen“, um die Schritte auszuführen.

### <a name="212-how-to-setup-byol-pricing-model-for-a-sku"></a>2.1.2 Einrichten des BYOL-Abrechnungsmodells für eine SKU
Bitte führen Sie die folgenden Schritte aus, um ein BYOL-Abrechnungsmodell für eine SKU einzurichten:

1. Melden Sie sich beim [Veröffentlichungsportal](https://publish.windowsazure.com)an.
2. Navigieren Sie zur Registerkarte **VIRTUELLE COMPUTER** , und wählen Sie Ihr Angebot aus.
3. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **SKUs** .
4. Stellen Sie sicher, dass die SKU als „Bring-Your-Own-License-SKU“ gekennzeichnet ist. Falls nicht, klicken Sie auf die Schaltfläche BEARBEITEN, um das Abrechnungsmodell zurückzusetzen. Daraufhin wird ein Fenster geöffnet. Aktivieren Sie das Kontrollkästchen „Abrechnung und Lizenzierung erfolgt extern durch Azure (auch bekannt als Bring-Your-Own-License)“, und speichern Sie die Änderungen.
   
   ![Abbildung](media/marketplace-publishing-push-to-staging/img2.1.2_04.png)
5. Klicken Sie im Menü auf der linken Seite auf die Registerkarte **PREISE** .
6. Wählen Sie Ihre Basisregion, und stellen Sie die SKU durch Aktivieren des Kontrollkästchens für die SKU unter dem Abschnitt EXTERNALLY-LICENSED (BYOL) SKU AVAILABILITY (Extern lizenzierte SKU-Verfügbarkeit [BYOL]) in der Region zur Verfügung siehe (siehe Screenshot unten).
   
   ![Abbildung](media/marketplace-publishing-push-to-staging/img2.1.2_06.png)
7. Machen Sie die SKU manuell oder mithilfe des AUTOPRICE-Assistenten für automatische Preisfestlegung in anderen Regionen verfügbar. Sehen Sie sich dazu auf dieser Seite im Abschnitt **2.1.1 Einrichten des stündlichen Abrechnungsmodells für eine SKU** die Punkte 9 bis 13 an, die die Verwendung des AUTOPRICE-Assistenten für automatische Preisfestlegung erläutern.

### <a name="22-set-your-developer-service-prices"></a>2.2. Festlegen der Preise für Entwicklerdienste
Pläne können aus einer beliebigen Kombination aus Basis- und Verbrauchskonzepten bestehen. „Basis“ ist eine Monatspauschale, „Überschreitung“ ein nutzungsbasierter Preis. (Weitere Informationen siehe unten.)

**Beispiel:** Angebot eines Contoso-Entwicklerdiensts

| Plan | Preis | Umfang | Migrationspfad |
| --- | --- | --- | --- |
| Free |0 USD/Monat |Grundlegende Funktionalität. |Migration zu jedem anderen Plan möglich |
| Bronze |10 USD/Monat |Grundlegende Funktionalität und ein Kontingent von 1.000 Nutzungseinheiten für Feature X. |Migration zu den Plänen Bronze Plus, Silber und Gold möglich |
| Bronze Plus |Kostenloser Testzeitraum: 0 USD/Monat + 0 USD/meter01 |Grundlegende Funktionen und ein Kontingent von 10.000 Nutzungseinheiten für Feature X. Sobald das Kontingent für Feature X verbraucht ist, kann der Kunde über „meter01“ zur nutzungsabhängigen Zahlung wechseln. |Migration zu den Plänen Silber Plus und Gold möglich |
| Bronze Plus |Zahlungszeitraum (kostenloser Testzeitraum ist abgelaufen): 10 USD/Monat + 0,05 USD/meter01. |Grundlegende Funktionen und ein Kontingent von 10.000 Nutzungseinheiten für Feature X. Sobald das Kontingent für Feature X verbraucht ist, kann der Kunde über „meter01“ zur nutzungsabhängigen Zahlung wechseln. |Migration zu den Plänen Silber Plus und Gold möglich |
| Silber |0,15 USD/meter01 |Der Kunde kann nutzungsbasiert per „meter01“ bezahlen, dies gilt für Feature X. |Migration zu den Plänen Bronze und Gold möglich |
| Silber Plus |20 USD/Monat + 0,15 USD/meter01 + 0,01 USD/meter02 |Grundlegende Funktionen und ein Kontingent von 10.000 Nutzungseinheiten für Feature X sowie 100 Nutzungseinheiten für Feature Y. Sobald das Kontingent für Feature X verbraucht ist, kann der Kunde über „meter01“ zur nutzungsabhängigen Zahlung wechseln.  Sobald das Kontingent für Feature Y verbraucht ist, dann der Kunde basierend auf „meter02“ nutzungsbasiert bezahlen. |Migration zu den Plänen Bronze Plus und Gold möglich |
| Gold |1.000 USD/Monat |Kontingent von 10.000 Nutzungseinheiten für Feature X, 1.000 Nutzungseinheiten für Feature Y und unbegrenzte Nutzung von Feature Z. |Migration zu allen Plänen außer zum kostenlosen Tarif möglich |

## <a name="step-3-provide-support-information"></a>Schritt 3: Angeben von Supportinformationen
Die Kontaktinformationen werden nur für die interne Kommunikation zwischen dem Partner und Microsoft verwendet. Für die Endkunden steht eine Support-URL zur Verfügung.

1. Klicken Sie auf die Überschrift **Support** auf der linken Seite des Veröffentlichungsportals.
2. Füllen Sie die Felder für **Engineering-Kontaktdaten**aus.
3. Füllen Sie die Felder für **Kundendienst** aus. Wenn Sie nur E-Mail-Support anbieten, geben Sie eine Pseudotelefonnummer ein. Stattdessen wird dann Ihre bereitgestellte E-Mail-Adresse verwendet.
4. Geben Sie die Support-URL ein.

## <a name="step-4-choose-azure-marketplace-categories"></a>Schritt 4: Auswählen von Azure Marketplace-Kategorien
Auf der Registerkarte **Kategorien** stehen verschiedene Möglichkeiten zur Auswahl. Wenn Ihr Angebot sich hier einordnen lässt, können Sie bis zu fünf Kategorien auswählen.

## <a name="how-your-marketing-will-appear"></a>Verwendung Ihrer Marketinginhalte
Im Anschluss finden Sie eine ausführliche Übersicht darüber, wie die Marketinginhalte des Angebots auf der [Azure Marketplace-Website](https://azure.microsoft.com/marketplace/) und im [Azure-Portal](https://portal.azure.com) verwendet werden.

### <a name="azure-marketplace-website"></a>Azure Marketplace-Website
![Abbildung](media/marketplace-publishing-push-to-staging/acom-catalog-01.png)

![Abbildung](media/marketplace-publishing-push-to-staging/acom-catalog-02.png)

*Auflisten der Angebote auf der Azure Marketplace-Website*

![Abbildung](media/marketplace-publishing-push-to-staging/acom-listing-details-01.png)

*Beschreibung der Angebote auf der Azure Marketplace-Website*

![Abbildung](media/marketplace-publishing-push-to-staging/acom-listing-details-02.png)

*Preisübersicht zu den Angeboten auf der Azure Marketplace-Website*

### <a name="azure-portal"></a>Azure-Portal
![Abbildung](media/marketplace-publishing-push-to-staging/azureportal-galleryblade-01.png)

*Auflisten der Angebote im Azure-Portal*

![Abbildung](media/marketplace-publishing-push-to-staging/azureportal-galleryblade-02.png)

*Beschreibung der Angebote im Azure-Portal*

## <a name="next-steps"></a>Nächste Schritte
Nachdem nun Ihre Marketplace-Inhalte geladen wurden, geht es weiter mit dem Testen des Angebots in der Stagingumgebung. Da die Schritte jedoch je nach Angebotstyp unterschiedlich sind, müssen Sie den entsprechenden Angebotstyp in der Liste unten auswählen.

* [Testen des VM-Angebots in der Stagingphase](marketplace-publishing-vm-image-test-in-staging.md)
* [Testen Ihres Angebots einer Lösungsvorlage in der Stagingumgebung](marketplace-publishing-solution-template-test-in-staging.md)

## <a name="see-also"></a>Weitere Informationen
* [Erste Schritte: Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)

[img-map-acom]:media/marketplace-publishing-push-to-staging/pubportal-mapping-acom.jpg
[img-map-portal]:media/marketplace-publishing-push-to-staging/pubportal-mapping-azure-portal.jpg
[img-map-link]:media/marketplace-publishing-push-to-staging/marketing-content-guide-links.jpg
[img-map-logo]:media/marketplace-publishing-push-to-staging/marketing-content-guide-logos.jpg
[img-map-title]:media/marketplace-publishing-push-to-staging/marketing-content-guide-publisher-offer.png

[link-pubportal]:https://publish.windowsazure.com
[link-push-to-production]:marketplace-publishing-push-to-production.md



<!--HONumber=Dec16_HO2-->



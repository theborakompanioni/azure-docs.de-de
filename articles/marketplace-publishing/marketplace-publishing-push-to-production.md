<properties
   pageTitle="Bereitstellen Ihres Angebots im Azure Marketplace | Microsoft Azure"
   description="Hier erhalten Sie weitere Informationen zum Bereitstellen Ihres Angebots (VM-Image, Entwicklungsdienst, Datendienst usw.) in Azure Marketplace."
   services="marketplace-publishing"
   documentationCenter=""
   authors="HannibalSII"
   manager=""
   editor=""/>

<tags
   ms.service="marketplace"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/02/2016"
   ms.author="hascipio" />

# Bereitstellen Ihres Angebots im Azure Marketplace
Wenn Sie nach dem Testen von Kundenszenarien, Marketinginhalten und Ähnlichem mit Ihrem Angebot zufrieden sind und es veröffentlichen möchten, verwenden Sie auf der Registerkarte **Veröffentlichen** die Option **Für Produktionsumgebungen freigeben**.

1. Die vier Schritte auf der Seite VORGEHENSWEISE im Veröffentlichungsportal sollten abgeschlossen und grün sein. Stellen Sie für VM-Angebote sicher, dass die folgenden Richtlinien eingehalten werden.

    ![Abbildung][img-pubportal-walkthru-checked]

2. Wählen Sie in der Liste auf der linken Seite die Registerkarte **Veröffentlichen** aus.

    ![Abbildung][img-pubportal-menu-publish]

3. Klicken Sie auf die Schaltfläche **Genehmigung für Freigabe in Produktionsumgebungen anfordern**. Nach der Anforderung führt das Genehmigungsteam eine letzte Überprüfung durch. Anschließend ist Ihr Angebot im Azure Marketplace verfügbar.

    ![Abbildung][img-pubportal-publish-pushproduction]

>[AZURE.IMPORTANT] Bei VMs werden beim Klicken auf die Schaltfläche „Genehmigung für Freigabe in Produktionsumgebungen anfordern“ die folgenden Schritte im Hintergrund ausgeführt. Sie können den Fortschritt der einzelnen Schritte auf der Registerkarte VERÖFFENTLICHEN im Veröffentlichungsportal verfolgen. Sie müssen diese Seite in regelmäßigen Intervallen (bis der Status „Aufgelistet“ angezeigt wird) auf Fehlerinformationen überprüfen, die Korrektur Ihrerseits erfordern.

> - Ihre Produktionsanforderung wird zunächst an das Zertifizierungsteam geleitet, das die VHD-Datei überprüft. Wenn Sie allerdings Ihr bereits aufgeführtes Angebot aktualisieren, und die Anforderung also nur einer Marketingänderung entspricht, wird der Zertifizierungsschritt übersprungen.
> - Im nächsten Schritt wird die Anforderung an das Inhaltsprüfungsteam geleitet, das den Marketinginhalt des Angebots überprüft.
> - Wenn die oben genannten Schritte erfolgreich sind, wird das Angebot zur Produktion genehmigt. Zu diesem Zeitpunkt erhält es den Status „Aufgelistet“ im Veröffentlichungsportal. Dieser Status „Aufgelistet“ bedeutet jedoch nicht, dass der Vorgang abgeschlossen ist. Die folgenden Schritte müssen abgeschlossen sein, bevor das Angebot im Azure Marketplace verfügbar ist.
> - Nachdem das Angebot im obigen Schritt zur Produktion genehmigt ist, startet die alle Azure-Rechenzentren übergreifende Replikation des Angebots. In der Regel dauert es 24-48 Stunden, bis die Replikation abgeschlossen hat, aber es kann je nach Größe der virtuellen Festplatte bis zu einer Woche dauern. Wenn Sie allerdings Ihr bereits aufgeführtes Angebot aktualisieren, und nur eine Marketingänderung vorgenommen haben, verläuft die Replikation schneller.
> - Nach Abschluss der Replikation ist das Angebot im Azure Marketplace verfügbar.

> Solange das Angebot den Status **Entwurf** (also nicht **Für Stagingumgebung freigeben** oder **Für Produktionsumgebungen freigeben**) hat, kann es jederzeit gelöscht werden. Klicken Sie unten auf der Seite auf der Registerkarte **Verlauf** auf **Entwurf verwerfen**, um einen Entwurf zu löschen.


## Produktionscheckliste für alle VM-Angebote

- Stellen Sie sicher, dass Sie ein Microsoft Azure Certified-Partner sind.
- Die Option „SKU für Marketplace ausblenden, da sie immer über eine Lösungsvorlage erworben werden sollte“ auf der SKU-Registerkarte sollte nur dann auf JA festgelegt werden, wenn die SKU Teil einer Lösungsvorlage ist. Andernfalls sollte diese Option immer auf NEIN festgelegt werden.
- Denken Sie daran: Ändern Sie nicht die Sichtbarkeitseinstellungen der SKU, sobald die SKU aufgeführt ist. Diese Funktionalität wird nicht unterstützt.
- Stellen Sie sicher, dass die Logos den unten beschriebenen Azure Marketplace-Richtlinien für Logos entsprechen.
- Angebot und SKU-Beschreibung dürfen nicht identisch sein.
- SKU-Titel und ausführliche Angebotsübersicht dürfen nicht identisch sein.
- SKU-Titel und Zusammenfassung des Angebots dürfen nicht identisch sein.
- SKU-Titel dürfen für ein Angebot mit mehreren SKUs nicht identisch sein.

**Azure Marketplace-Richtlinien für Logos**

- Die Farbpalette des Azure-Designs ist einfach und geradlinig. Verwenden Sie auf Ihrem Logo möglichst wenige Primär- und Sekundärfarben.
- Die Designfarben des Azure-Portals sind Weiß und Schwarz. Daher verwenden Sie diese Farben nicht als Hintergrundfarbe für Ihre Logos. Verwenden Sie Farben, die Ihre Logos im Azure-Portal markant hervorstechen lassen. Sie sollten einfache Primärfarben verwenden. Wenn Sie transparenten Hintergrund verwenden, stellen Sie sicher, dass Logos und Text nicht weiß oder schwarz sind.
- Verwenden Sie im Logo keinen Hintergrund mit Farbverlauf.
- Platzieren Sie nach Möglichkeit keinen Text auf dem Logo, auch nicht Ihren Firmen- oder Markennamen.
- Das Erscheinungsbild Ihres Logos sollte klar und direkt und frei von Farbverläufen sein.
- Das Logo sollte nicht gestreckt sein.

**Zusätzliche Richtlinien für Herologos:**

- Ein Herologo ist optional. Der Herausgeber muss kein Herologo hochladen. **Ist das Herosymbol jedoch einmal hochgeladen, dann kann es nicht mehr aus dem Veröffentlichungsportal gelöscht werden. Zu diesem Zeitpunkt muss der Partner die Azure Marketplace-Richtlinien für Herosymbole einhalten, sonst wird das Angebot nicht zur Produktion genehmigt.**
- Anzeigename des Herausgebers, SKU-Titel und ausführliche Angebotsübersicht werden in weißer Schrift angezeigt. Daher sollten Sie helle Farben im Hintergrund des Herosymbols vermeiden. Schwarzer, weißer und transparenter Hintergrund ist für Herosymbole nicht zulässig.
- Der Anzeigename des Herausgebers, der SKU-Titel, die ausführliche Angebotsübersicht und die Erstellen-Schaltfläche werden programmgesteuert in das Herologo eingebettet, wenn das Angebot live geschaltet wird. Sie sollten beim Entwerfen des Herologos keinen Text eingeben. Lassen Sie einfach Leerraum auf der rechten Seite, da der Text (d.h. Anzeigename des Herausgebers, SKU-Titel, ausführliche Angebotsübersicht) programmgesteuert von uns dort berücksichtigt werden. Der Leerraum für den Text sollte 415 x 100 auf der rechten Seite betragen (und ist 370 Pixel von links versetzt).


## Zusätzliche Produktionscheckliste für bereits aufgeführte VM-Angebote

- Überprüfen Sie, ob bereits ein Angebot mit gleichem Angebotsnamen von Ihrem Unternehmen vorhanden ist. Wenn ja, sollten Sie eine neue Version der SKU dem vorhandenen Angebot hinzufügen, anstatt ein neues doppeltes Angebot zu erstellen.
- Datenträger sollten zwischen zwei Versionen der gleichen SKU nicht wechseln.
- Azure Marketplace unterstützt keine Preisänderung der aufgeführten SKUs, da dies die Abrechnung der bestehenden Kunden beeinflusst. Stellen Sie sicher, dass Sie die Preise der aufgeführten SKUs in den Regionen, in denen die SKU verfügbar ist, nicht ändern. Sie können jedoch neue SKUs hinzufügen, oder einer vorhandenen SKU neue Regionen hinzufügen.


## Nächste Schritte
Wenn das Angebot live geschaltet wurde, testen Sie die Kundenszenarien in der Produktionsumgebung, um zu prüfen, ob alle Verträge und Funktionen wie bei den Tests und Überprüfungen in der Stagingumgebung funktionieren.

## Weitere Informationen
- [Erste Schritte: Veröffentlichen eines Angebots im Azure Marketplace](marketplace-publishing-getting-started.md)

[img-pubportal-walkthru-checked]: media/marketplace-publishing-push-to-production/pubportal-walkthru-checked.png
[img-pubportal-menu-publish]: media/marketplace-publishing-push-to-production/pubportal-menu-publish.png
[img-pubportal-publish-pushproduction]: media/marketplace-publishing-push-to-production/pubportal-publish-pushproduction.png

<!---HONumber=AcomDC_0803_2016-->
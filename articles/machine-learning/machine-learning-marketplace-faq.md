---

ROBOTS: NOINDEX, NOFOLLOW
redirect_url: https://gallery.cortanaintelligence.com/
translationtype: Human Translation
ms.sourcegitcommit: f6006d5e83ad74f386ca23fe52879bfbc9394c0f
ms.openlocfilehash: 4f6e2816a55eb48fe947cc5bf21ee466e8116feb
ms.lasthandoff: 05/03/2017


---
# <a name="deprecated-publishing-and-using-machine-learning-apps-in-the-azure-marketplace-faq"></a>(Veraltet:) Veröffentlichen und Verwenden von Machine Learning-Apps im Azure Marketplace: häufig gestellte Fragen

> [!NOTE]
> DataMarket und Data Services werden eingestellt, und vorhandene Abonnements werden ab dem 31.3.2017 zurückgezogen und beendet. Dieser Artikel gilt daher als veraltet. 
> 
> Als Alternative können Sie Ihre Machine Learning-Experimente im [Cortana Intelligence-Katalog](https://gallery.cortanaintelligence.com/) veröffentlichen, damit sie der Data Science-Community zugute kommen. Weitere Informationen finden Sie unter [Teilen und Entdecken von Ressourcen im Cortana Intelligence-Katalog](https://docs.microsoft.com/en-us/azure/machine-learning/machine-learning-gallery-how-to-use-contribute-publish).


## <a name="questions-about-consuming-from-marketplace"></a>Fragen zur Nutzung des Marketplace
**1. Warum erhalte ich folgende Fehlermeldung, nachdem ich Daten für den Webdienst eingegeben habe?**

**Die Anforderung führte zu einem Back-End-Timeout oder Back-End-Fehler. Das Team untersucht das Problem. Wir entschuldigen uns für eventuelle Unannehmlichkeiten. (500)**

Ihr/e Eingabeparameter entspricht/entsprechen möglicherweise nicht dem erforderlichen Format für den bestimmten Webdienst. Das richtige Format für Eingabeparameter und die Einschränkungen für diesen Webdienst finden Sie im entsprechenden Dokumentationslink.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

**2. Mit welchen Anmeldeinformationen sollte ich auf die Ergebnisse zugreifen, und wie kann ich diese anzeigen, wenn ich den API-Link für den Webdienst, der in "Dieses Dataset durchsuchen" angezeigt wird, kopiere und in einem anderen Browserfenster einfüge?**

Sie sollten Ihr Marketplace-Konto als den Benutzernamen und den primären Kontoschlüssel als Kennwort verwenden. Sie finden den primären Kontoschlüssel auf der Seite **Dieses Dataset durchsuchen** unter der Beschreibung des Webdiensts (klicken Sie auf die Schaltfläche **Anzeigen**). Das Ergebnis kann im Browser angezeigt oder heruntergeladen werden, je nachdem, welchen Browser Sie verwenden.

**3. Warum erhalte ich folgende Fehlermeldung, nachdem ich auf der Seite "Dieses Dataset durchsuchen" Daten für den Webdienst eingegeben habe?** 

**Unerwarteter Fehler beim Verarbeiten Ihrer Anforderung. Bitte versuchen Sie es später noch einmal.**

Mindestens ein Eingabeparameter Ihres Webdiensts hat möglicherweise die maximale Länge bei der Nutzung des Webdiensts auf der Marketplace-Seite **Dieses Dataset durchsuchen** überschritten. Die Dienste können durch Verwendung von HTTP POST-Methoden mit einer längeren Eingabezeichenfolge aufgerufen werden. Beispiele finden Sie unter [Beispiellösungen für Machine Learning, die R verwenden und im Marketplace veröffentlicht wurden](machine-learning-r-csharp-web-service-examples.md).

**4. Warum wird mir auf der Registerkarte "API EXPLORER" im Store des klassischen Azure-Portals nichts angezeigt?** 

Dies ist ein bekanntes Problem mit dem Marketplace im klassischen Azure-Portal. Das Team arbeitet an einer Lösung zur Behebung dieses Problems. 

## <a name="questions-about-publishing-from-azure-machine-learning-on-marketplace"></a>Fragen zur Veröffentlichung aus Azure Machine Learning im Marketplace
**1. Warum werden meine Logo-/Bildertransaktionen für meinen Webdienst nicht aktualisiert?** 

Logos und Bilder werden im Veröffentlichungsportal zwischengespeichert, und eine Aktualisierung auf das neue Logo bzw. Bild im Portal kann bis zu 10 Tage dauern.

**2. Warum wird auf der Registerkarte "Details" meines Webdiensts im Marketplace ein Fehler angezeigt?**

Es gibt ein bekanntes Problem mit dem Marketplace bei der Verbindung mit Azure Machine Learning für Dienstdetails. Das Team arbeitet an einer Lösung zur Behebung dieses Problems.

**3. Warum funktioniert der R-Beispielcode im Azure Machine Learning-Webdienst nicht für die Nutzung der Webdienste im Marketplace?**

Bei der direkten Verbindung mit Azure Machine Learning-Webdiensten werden andere Authentifizierungssysteme eingesetzt als bei der Verbindung mit diesen Webdiensten über den Marketplace. Die Dienste im Marketplace sind OData-Dienste und können mit GET- oder POST-Methoden aufgerufen werden. 

**4. Warum werden die Supportlinks meiner Webdienstangebote nicht korrekt für einige meiner Angebote aktualisiert?**

Supportlinks gelten global pro Herausgeber, nicht pro Angebot. 

**5. Wie veröffentliche ich einen Webdienst mit Batcheingabemodus im Marketplace?**

Der Batcheingabemodus wird zurzeit nicht von Marketplace-Webdiensten unterstützt.

**6. An wen kann ich mich wenden, wenn ich Fragen zur Datenherausgabe oder Probleme bei der Veröffentlichung habe?**

Wenden Sie sich unter <mailto:datamarketbd@microsoft.com> an das Azure Marketplace-Team, um weitere Informationen zu erhalten.



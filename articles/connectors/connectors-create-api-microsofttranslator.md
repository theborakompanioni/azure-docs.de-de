---
title: Hinzufügen von Microsoft Translator in Logik-Apps | Microsoft Docs
description: Übersicht über den Microsoft Translator-Connector mit REST-API-Parametern
services: ''
suite: ''
documentationcenter: ''
author: MandiOhlinger
manager: erikre
editor: ''
tags: connectors

ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia

---
# Erste Schritte mit dem Microsoft Translator-Connector
Stellen Sie eine Verbindung mit Microsoft Translator her, um Text zu übersetzen, eine Sprache zu erkennen und mehr. Mit Microsoft Translator können Sie folgende Aktionen ausführen:

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die von Microsoft Translator abgerufen werden.
* Verwenden von Aktionen, um Text zu übersetzen, eine Sprache zu erkennen und mehr. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn beispielsweise eine neue Datei in Dropbox erstellt wird, können Sie den Text in der Datei mit Microsoft Translator in eine andere Sprache übersetzen.

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Trigger und Aktionen
Microsoft Translator umfasst die folgenden Aktionen. Es gibt keine Trigger.

| Trigger | Aktionen |
| --- | --- |
| Keine |<ul><li>Sprache erkennen</li><li>Text-to-Speech</li><li>Text übersetzen</li><li>Sprachen abrufen</li><li>Sprachen für Sprachausgabe abrufen</li></ul> |

Alle Connectors unterstützen Daten im JSON- und XML-Format.

## Herstellen einer Verbindung mit Microsoft Translator
> [!INCLUDE [Schritte zum Herstellen einer Verbindung mit Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]
> 
> 

## Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### Sprache erkennen
Erkennt die Ausgangssprache des angegebenen Texts. ```GET: /Detect```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |(Keine) |Text, dessen Sprache identifiziert wird |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### Text-to-Speech
Konvertiert einen angegebenen Text in eine Sprachausgabe (Audiodatenstrom im Wave-Format). ```GET: /Speak```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |(Keine) |Zu konvertierender Text |
| Sprache |string |Ja |query |(Keine) |Sprachcode zum Generieren der Sprache (Beispiel: „de-de“) |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### Text übersetzen
Übersetzt Text mit Microsoft Translator in eine angegebene Sprache. ```GET: /Translate```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |(Keine) |Zu übersetzender Text |
| languageTo |string |Ja |query |(Keine) |Code der Zielsprache (Beispiel: „fr“) |
| languageFrom |string |no |query |(Keine) |Quellsprache. Wenn diese nicht angegeben wurde, versucht Microsoft Translator, sie automatisch zu erkennen. (Beispiel: „de“) |
| category |string |no |query |allgemein |Übersetzungskategorie (Standard: „Allgemein“) |

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### Sprachen abrufen
Ruft alle Sprachen ab, die von Microsoft Translator unterstützt werden. ```GET: /TranslatableLanguages```

Es gibt keine Parameter für diesen Aufruf.

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### Sprachen für Sprachausgabe abrufen
Ruft die für die Sprachsynthese verfügbaren Sprachen ab. ```GET: /SpeakLanguages```

Es gibt keine Parameter für diesen Aufruf.

#### Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## Objektdefinitionen
#### Sprache: Sprachmodell für mit Microsoft Translator übersetzbare Sprachen
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Code |string |no |
| Name |string |no |

## Nächste Schritte
[Erstellen Sie eine Logik-App](../app-service-logic/app-service-logic-create-a-logic-app.md).

Kehren Sie zur [Liste mit den APIs](apis-list.md) zurück.

<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png

<!---HONumber=AcomDC_0824_2016-->
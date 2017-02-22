---
title: "Hinzufügen von Microsoft Translator in Logik-Apps | Microsoft Docs"
description: "Übersicht über den Microsoft Translator-Connector mit REST-API-Parametern"
services: 
suite: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: da782baf-8bf8-4973-8238-e469865f5328
ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/07/2016
ms.author: mandia
translationtype: Human Translation
ms.sourcegitcommit: b92f954680603891ced503a1134791312b5214f0
ms.openlocfilehash: add307420b9e16a60b0e57b0ba08188b2509978e


---
# <a name="get-started-with-the-microsoft-translator-connector"></a>Erste Schritte mit dem Microsoft Translator-Connector
Stellen Sie eine Verbindung mit Microsoft Translator her, um Text zu übersetzen, eine Sprache zu erkennen und mehr. Mit Microsoft Translator können Sie folgende Aktionen ausführen: 

* Erstellen eines Geschäftsworkflows basierend auf den Daten, die von Microsoft Translator abgerufen werden. 
* Verwenden von Aktionen, um Text zu übersetzen, eine Sprache zu erkennen und mehr. Diese Aktionen erhalten eine Antwort und stellen anschließend die Ausgabe anderen Aktionen zur Verfügung. Wenn beispielsweise eine neue Datei in Dropbox erstellt wird, können Sie den Text in der Datei mit Microsoft Translator in eine andere Sprache übersetzen.

Informationen zum Hinzufügen eines Vorgangs in Logik-Apps finden Sie unter [Erstellen einer Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="triggers-and-actions"></a>Trigger und Aktionen
Microsoft Translator umfasst die folgenden Aktionen. Es gibt keine Trigger.

| Trigger | Aktionen |
| --- | --- |
| Keine |<ul><li>Sprache erkennen</li><li>Text-to-Speech</li><li>Text übersetzen</li><li>Sprachen abrufen</li><li>Sprachen für Sprachausgabe abrufen</li></ul> |

Alle Connectors unterstützen Daten im JSON- und XML-Format.

## <a name="create-a-connection-to-microsoft-translator"></a>Herstellen einer Verbindung mit Microsoft Translator
> [!INCLUDE [Steps to create a connection to Microsoft Translator](../../includes/connectors-create-api-microsofttranslator.md)]
> 
> 

## <a name="swagger-rest-api-reference"></a>Swagger-REST-API – Referenz
Gilt für Version: 1.0.

### <a name="detect-language"></a>Sprache erkennen
Erkennt die Ausgangssprache des angegebenen Texts.  
```GET: /Detect```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |Keine |Text, dessen Sprache identifiziert wird |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="text-to-speech"></a>Text-to-Speech
Konvertiert einen angegebenen Text als Audiodatenstrom im Wave-Format in Sprache.  
```GET: /Speak```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |Keine |Zu konvertierender Text |
| Sprache |string |Ja |query |(Keine) |Sprachcode zum Generieren der Sprache (Beispiel: „de-de“) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="translate-text"></a>Text übersetzen
Konvertiert Text mit Microsoft Translator in eine bestimmte Sprache.  
```GET: /Translate```

| Name | Datentyp | Erforderlich | Enthalten in | Standardwert | Beschreibung |
| --- | --- | --- | --- | --- | --- |
| query |string |Ja |query |Keine |Zu übersetzender Text |
| languageTo |string |Ja |query |Keine |Code der Zielsprache (Beispiel: „fr“) |
| languageFrom |string |no |query |(Keine) |Quellsprache. Wenn diese nicht angegeben wurde, versucht Microsoft Translator, sie automatisch zu erkennen. (Beispiel: „de“) |
| category |string |no |query |allgemein |Übersetzungskategorie (Standard: „Allgemein“) |

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-languages"></a>Sprachen abrufen
Ruft alle Sprachen ab, die von Microsoft Translator unterstützt werden.  
```GET: /TranslatableLanguages```

Es gibt keine Parameter für diesen Aufruf. 

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

### <a name="get-speech-languages"></a>Sprachen für Sprachausgabe abrufen
Ruft die verfügbaren Sprachen für die Sprachsynthese ab.  
```GET: /SpeakLanguages``` 

Es gibt keine Parameter für diesen Aufruf.

#### <a name="response"></a>Antwort
| Name | Beschreibung |
| --- | --- |
| 200 |OK |
| die Standardeinstellung |Fehler beim Vorgang. |

## <a name="object-definitions"></a>Objektdefinitionen
#### <a name="language-language-model-for-microsoft-translator-translatable-languages"></a>Sprache: Sprachmodell für mit Microsoft Translator übersetzbare Sprachen
| Eigenschaftenname | Datentyp | Erforderlich |
| --- | --- | --- |
| Code |string |no |
| Name |string |no |

## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md).

Gehen Sie zur [Liste der APIs](apis-list.md)zurück.

<!--References-->
[5]: https://datamarket.azure.com/developer/applications/
[6]: ./media/connectors-create-api-microsofttranslator/register-your-application.png



<!--HONumber=Feb17_HO3-->



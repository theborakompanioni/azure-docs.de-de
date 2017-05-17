---
title: 'Azure Active Directory B2C: Verwenden der Sprachanpassung | Microsoft-Dokumentation'
description: 
services: active-directory-b2c
documentationcenter: 
author: sama
ms.assetid: eec4d418-453f-4755-8b30-5ed997841b56
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: article
ms.devlang: na
ms.date: 04/25/2017
ms.author: sama
ms.translationtype: Human Translation
ms.sourcegitcommit: 97fa1d1d4dd81b055d5d3a10b6d812eaa9b86214
ms.openlocfilehash: 7134467421927e9a26e23f89684eeb6864a2f9a3
ms.contentlocale: de-de
ms.lasthandoff: 05/11/2017


---
# <a name="azure-active-directory-b2c-using-language-customization"></a>Azure Active Directory B2C: Verwenden der Sprachanpassung

>[!NOTE] 
>Dieses Feature befindet sich in der Phase der öffentlichen Vorschau.  Es wird empfohlen, beim Verwenden dieses Features einen Testmandanten zu verwenden.  Es ist nicht geplant, für die allgemein verfügbare Version gegenüber der Vorschauversion Änderungen durchzuführen, aber wir behalten uns das Recht vor, zur Verbesserung des Features Änderungen vorzunehmen.  Nachdem Sie das Feature testen konnten, würden wir uns über einen Bericht zu Ihren Erfahrungen und Hinweise zu Verbesserungsmöglichkeiten freuen.  Sie können uns Feedback über das Azure-Portal zukommen lassen, indem Sie das Smiley-Tool oben rechts verwenden.   Falls für Sie die geschäftliche Anforderung besteht, mit diesem Feature während der Vorschauphase in den Livemodus zu wechseln, können Sie uns über Ihre Szenarien informieren, damit wir Ihnen geeignete Anleitungen und Hilfestellungen zukommen lassen können.  Sie können sich unter [aadb2cpreview@microsoft.com](mailto:aadb2cpreview@microsoft.com) an uns wenden.

Mit der Sprachanpassung können Sie Ihre User Journey in eine andere Sprache ändern, um etwaige Kundenanforderungen zu erfüllen.  Wir bieten Übersetzungen für 37 Sprachen an (siehe [Zusätzliche Informationen](#additional-information)).  Auch wenn Ihre Benutzeroberfläche nur für eine einzelne Sprache bestimmt ist, können Sie beliebigen Text auf den Seiten an Ihre Anforderungen anpassen.  

## <a name="how-does-language-customization-work"></a>Wie funktioniert die Sprachanpassung?
Mit der Sprachanpassung können Sie auswählen, in welchen Sprachen Ihre User Journey verfügbar ist.  Nach der Aktivierung des Features können Sie den Abfragezeichenfolgen-Parameter „ui_locales“ aus Ihrer Anwendung bereitstellen.  Wenn Sie Azure AD B2C aufrufen, übersetzen wir Ihre Seite in das von Ihnen angegebene Gebietsschema.  Mit dieser Art von Konfiguration haben Sie die vollständige Kontrolle über die Sprachen Ihrer User Journey, und die Spracheinstellungen des Kundenbrowsers können ignoriert werden.  Es kann aber auch sein, dass Sie nicht so genau steuern müssen, welche Sprachen Kunden angezeigt werden.  Wenn Sie den Parameter „ui_locales“ nicht angeben, wird die Benutzeroberfläche des Kunden durch die Einstellungen des Browsers vorgegeben.  Sie können weiterhin steuern, in welche Sprachen Ihre User Journey übersetzt wird, indem Sie eine unterstützte Sprache hinzufügen.  Wenn für den Browser eines Kunden das Anzeigen einer Sprache festgelegt ist, die Sie nicht unterstützen möchten, wird stattdessen die Sprache angezeigt, die Sie als Standardeinstellung für die unterstützte Kultur ausgewählt haben.

1. **Für „ui-locales“ angegebene Sprache**: Nachdem Sie die Sprachanpassung aktiviert haben, wird Ihre User Journey in die hier angegebene Sprache übersetzt.
2. **Vom Browser angeforderte Sprache**: Wenn keine Angaben für „ui-locales“ vorhanden sind, wird eine Übersetzung in die vom Browser angeforderte Sprache durchgeführt, **sofern sie in den unterstützten Sprachen enthalten ist**.
3. **Standardsprache für Richtlinie**: Wenn keine Sprache oder eine nicht unterstützte Sprache angegeben wird, wird eine Übersetzung in die Standardsprache der Richtlinie durchgeführt.

>[!NOTE]
>Falls Sie benutzerdefinierte Benutzerattribute verwenden, müssen Sie Ihre eigenen Übersetzungen angeben.  Ausführliche Informationen hierzu finden Sie unter [Anpassen von Zeichenfolgen](#customize-your-strings).
>

## <a name="support-uilocales-requested-languages"></a>Unterstützen von angeforderten Sprachen für „ui_locales“ 
Indem Sie die „Sprachanpassung“ für eine Richtlinie aktivieren, können Sie die Sprache der User Journey jetzt steuern. Fügen Sie hierzu den Parameter „ui_locales“ hinzu.
1. [Führen Sie diese Schritte aus, um im Azure-Portal zum Blatt „B2C-Funktionen“ zu navigieren](https://docs.microsoft.com/azure/active-directory-b2c/active-directory-b2c-app-registration#navigate-to-the-b2c-features-blade).
2. Navigieren Sie zu einer Richtlinie, die Sie für Übersetzungen aktivieren möchten.
3. Klicken Sie auf **Sprachanpassung**.
4. Lesen Sie sich die Warnung sorgfältig durch.  Nach der Aktivierung können Sie die „Sprachanpassung“ nicht mehr deaktivieren.
5. Aktivieren Sie das Feature, und klicken Sie auf **OK**.
6. Speichern Sie Ihre Richtlinie mit der Option in der oberen linken Ecke des Blatts **Richtlinie bearbeiten**.

## <a name="select-which-languages-your-user-journey-supports"></a>Auswählen der unterstützten Sprachen für die User Journey 
Erstellen Sie eine Liste mit zulässigen Sprachen für Ihre User Journey, in die die Übersetzung erfolgen soll, wenn der Parameter „ui_locales“ nicht angegeben ist.
1. Stellen Sie anhand der obigen Anleitung sicher, ob für Ihre Richtlinie die „Sprachanpassung“ aktiviert ist.
2. Wählen Sie auf dem Blatt **Richtlinie bearbeiten** die Option **Sprachanpassung**.
3. Das Blatt **Unterstützte Sprachen** wird geöffnet.  Auf dem Blatt können Sie die Option **Ressource hinzufügen** wählen.
4. Wählen Sie alle Sprachen aus, die unterstützt werden sollen.  

>[!NOTE]
>Wenn der Parameter „ui_locales“ nicht angegeben wird, wird die Seite nur in die Browsersprache des Kunden übersetzt, wenn sie in dieser Liste enthalten ist.
>

5. Klicken Sie unten auf **OK**.
6. Schließen Sie das Blatt **Sprachanpassung**, und **speichern** Sie die Richtlinie.

## <a name="customize-your-strings"></a>Anpassen von Zeichenfolgen
Mit der Sprachanpassung können Sie alle Zeichenfolgen Ihrer User Journey anpassen.
1. Stellen Sie anhand der obigen Anleitung sicher, ob für Ihre Richtlinie die „Sprachanpassung“ aktiviert ist.
2. Wählen Sie auf dem Blatt **Richtlinie bearbeiten** die Option **Sprachanpassung**.
3. Wählen Sie im linken Navigationsmenü die Option **Inhalt herunterladen**.
4. Wählen Sie die Seite aus, die Sie bearbeiten möchten.
5. Wählen Sie in der Dropdownliste die Sprache aus, für die Sie die Bearbeitung durchführen möchten.
6. Klicken Sie auf **Download**. 

Bei diesen Schritten erhalten Sie eine JSON-Datei, die Sie nutzen können, um mit dem Bearbeiten Ihrer Zeichenfolgen zu beginnen.

### <a name="changing-any-string-on-the-page"></a>Ändern einer beliebigen Zeichenfolge auf der Seite
1. Öffnen Sie die JSON-Datei, die Sie im Rahmen der obigen Anleitung heruntergeladen haben, in einem JSON-Editor.
2. Suchen Sie nach dem Element, das Sie ändern möchten.  Sie können nach der gewünschten `StringId` der Zeichenfolge oder nach dem `Value` suchen, den Sie ändern möchten.
3. Aktualisieren Sie das `Value`-Attribut mit den Daten, die angezeigt werden sollen.
4. Speichern Sie die Datei, und laden Sie Ihre Änderungen hoch.

### <a name="changing-extension-attributes"></a>Ändern von Erweiterungsattributen
Wenn Sie die Zeichenfolge für ein benutzerdefiniertes Benutzerattribut ändern oder eine Zeichenfolge dem JSON-Code hinzufügen möchten, hat diese das folgende Format:
```JSON
{
  "LocalizedStrings": [
    {
      "ElementType": "ClaimType",
      "ElementId": "extension_<ExtensionAttribute>",
      "StringId": "DisplayName",
      "Override": false,
      "Value": "<ExtensionAttributeValue>"
    }
    [...]
}
```
>[!IMPORTANT]
>Stellen Sie beim Überschreiben einer Zeichenfolge sicher, dass Sie den Wert für `Override` auf `true` festlegen.  Wenn der Wert nicht geändert wird, wird der Eintrag ignoriert. 
>

Ersetzen Sie `<ExtensionAttribute>` durch den Namen Ihres benutzerdefinierten Benutzerattributs.  
Ersetzen Sie `<ExtensionAttributeValue>` durch die neue Zeichenfolge, die angezeigt werden soll.

### <a name="using-localizedcollections"></a>Verwenden von LocalizedCollections
Wenn Sie eine vorgegebene Liste mit Werten für Antworten bereitstellen möchten, müssen Sie ein `LocalizedCollections`-Element erstellen.  Ein `LocalizedCollections`-Element ist ein Array mit Paaren aus `Name` und `Value`.  `Name` wird angezeigt, und `Value` ist der Wert, der im Anspruch zurückgegeben wird.  Für das Hinzufügen eines `LocalizedCollections`-Elements gilt das folgende Format:

```JSON
{
  "LocalizedStrings": [...],
  "LocalizedCollections": [{
      "ElementType":"ClaimType", 
      "ElementId":"<UserAttribute>",
      "TargetCollection":"Restriction",
      "Override": false,
      "Items":[
           {
                "Name":"<Response1>",
                "Value":"<Value1>"
           },
           {
                "Name":"<Response2>",
                "Value":"<Value2>"
           }
     ]
  }]
}
```
>[!IMPORTANT]
>Stellen Sie beim Überschreiben einer Zeichenfolge sicher, dass Sie den Wert für `Override` auf `true` festlegen.  Wenn der Wert nicht geändert wird, wird der Eintrag ignoriert. 
>

* `ElementId` ist das Benutzerattribut, für das `LocalizedCollections` als Antwort gilt.
* `Name` ist der Wert, der dem Benutzer angezeigt wird.
* `Value` wird im Anspruch zurückgegeben, wenn diese Option ausgewählt wird.

### <a name="upload-your-changes"></a>Hochladen von Änderungen
1. Navigieren Sie nach dem Eingeben der Änderungen in die JSON-Datei zurück zu Ihrem B2C-Mandanten.
2. Wählen Sie auf dem Blatt **Richtlinie bearbeiten** die Option **Sprachanpassung**.
3. Wählen Sie im linken Navigationsmenü die Option **Inhalt hochladen**.
4. Wählen Sie die Seite aus, für die Sie Änderungen hochladen möchten.
5. Wenn Sie eine Sprache hochladen möchten, für die Sie zuvor JSON-Code bereitgestellt haben, müssen Sie den vorherigen Eintrag löschen.  Sie können ihn löschen, indem Sie rechts von der Sprache auf `...` klicken und dann **Löschen** wählen.
6. Klicken Sie oben links auf **Hinzufügen**.
7. Wählen Sie die Sprache Ihrer JSON-Datei aus.
8. Klicken Sie rechts auf die Ordnerschaltfläche, und suchen Sie nach Ihrer JSON-Datei.
9. Klicken Sie unten auf dem Blatt auf die Schaltfläche **Hochladen**.
10. Wechseln Sie zurück zum Blatt **Richtlinie bearbeiten**, und klicken Sie auf **Speichern**.

## <a name="additional-information"></a>Zusätzliche Informationen
### <a name="recommendations-for-language-customization"></a>Empfehlungen für die Sprachanpassung
Wir empfehlen Ihnen, in Ihre Sprachressourcen nur Einträge für Zeichenfolgen einzufügen, die Sie explizit ersetzen möchten.  Es gilt eine Größenbeschränkung für die Datei, die aus Ihren gesamten JSON-Übersetzungen kompiliert wird.  Falls die Dateien zu groß werden, beeinträchtigt dies die Leistung Ihrer User Journey.
### <a name="page-ui-customization-labels-are-removed-once-language-customization-is-enabled"></a>Die Bezeichnungen der Seite für die Benutzeroberflächenanpassung werden entfernt, wenn die Sprachanpassung aktiviert wird.
Bei Aktivierung der Sprachanpassung werden Ihre vorherigen Bearbeitungen für Bezeichnungen anhand von „Seite für die Benutzeroberflächenanpassung“ entfernt (mit Ausnahme von benutzerdefinierten Benutzerattributen).  Diese Änderung wird vorgenommen, um Konflikte in Bezug darauf zu vermeiden, wo Sie Ihre Zeichenfolgen bearbeiten können.  Sie können Ihre Bezeichnungen und anderen Zeichenfolgen weiterhin ändern, indem Sie Sprachressourcen in „Sprachanpassung“ hochladen.
### <a name="microsoft-is-committed-to-provide-the-most-up-to-date-translations-for-your-use"></a>Microsoft versucht, Ihnen jeweils die aktuellsten Übersetzungen zur Verfügung zu stellen.
Wir arbeiten ständig an der Verbesserung der Übersetzungen und sorgen für deren Konformität.  Wir identifizieren Fehler und Änderungen der globalen Terminologie und nehmen die Aktualisierungen vor, die für Ihre User Journey am besten geeignet sind.
### <a name="social-identity-provider-translations"></a>Übersetzungen für den Fall „Soziales Netzwerk als Identitätsanbieter“
Wir stellen den OIDC-Parameter „ui_locales“ für die Anmeldung an sozialen Netzwerken bereit, aber er wird von einigen Identitätsanbietern für soziale Netzwerke, z.B. Facebook und Google, nicht respektiert. 
### <a name="browser-behavior"></a>Browserverhalten
Für Chrome und Firefox wird jeweils die festgelegte Sprache angefordert, und wenn es sich um eine unterstützte Sprache handelt, wird sie anstelle der Standardsprache angezeigt.  Für Edge wird derzeit keine Sprache angefordert, sondern es wird gleich die Standardsprache verwendet.
### <a name="known-issues"></a>Bekannte Probleme
* Das Hochladen von Sprachressourcen für die MFA-Seite in einer Richtlinie für die Profilbearbeitung ist derzeit nicht verfügbar.
* `LocalizedCollections` werden für Werte nicht generiert, wenn dies für den Antworttyp erforderlich ist.
### <a name="what-if-i-want-a-language-that-isnt-supported"></a>Was passiert, wenn eine Sprache nicht unterstützt wird?
Wir planen, eine Erweiterung für dieses Feature bereitzustellen, mit dem Sie eine JSON-Ressource für „benutzerdefinierte Sprachen“ hochladen können.  Mit dem Feature können Sie den Namen und den Sprachcode für eine beliebige Sprache angeben und *alle* Übersetzungen für die Sprache bereitstellen.  Wenn Sie dieses Feature benötigen, können Sie uns unter [aadb2cpreview@microsoft.com](mailto:aadb2cpreview@microsoft.com) Informationen zu Ihrem Szenario schicken.  

### <a name="what-languages-are-supported"></a>Welche Sprachen werden unterstützt?

| Sprache              | Sprachcode |
|-----------------------|---------------|
| Bengalisch                | bn            |
| Tschechisch                 | cs            |
| Dänisch                | da            |
| Deutsch                | de            |
| Griechisch                 | el            |
| Englisch               | en            |
| Spanisch               | es            |
| Finnisch               | fi            |
| Französisch                | fr            |
| Gujarati              | gu            |
| Hebräisch                | he            |
| Hindi                 | hi            |
| Kroatisch              | hr            |
| Ungarisch             | hu            |
| Italienisch               | it            |
| Japanisch              | ja            |
| Kannada               | kn            |
| Koreanisch                | ko            |
| Malayalam             | ml            |
| Marathi               | mr            |
| Malaiisch                 | ms            |
| Norwegisch (Bokmål)      | nb            |
| Niederländisch                 | nl            |
| Pandschabi               | pa            |
| Polnisch                | pl            |
| Portugiesisch (Brasilien)   | pt-br         |
| Portugiesisch (Portugal) | pt-pt         |
| Rumänisch              | ro            |
| Russisch               | ru            |
| Slowakisch                | sk            |
| Schwedisch               | sv            |
| Tamilisch                 | ta            |
| Telugu                | te            |
| Thailändisch                  | th            |
| Türkisch               | tr            |
| Chinesisch (vereinfacht)  | zh-hans       |
| Chinesisch (traditionell) | zh-hant       |


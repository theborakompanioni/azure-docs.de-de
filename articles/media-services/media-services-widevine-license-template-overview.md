---
title: "Übersicht über die Widevine-Lizenzvorlage | Microsoft Do"
description: "Dieses Thema bietet einen Überblick über eine Widevine-Lizenzvorlage, mit der Widevine-Lizenzen konfiguriert werden können."
author: juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0e6f1f05-7ed6-4ed6-82a0-0cc2182b075a
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: a90e56bb2b7db0bb964684f9cac04096a6577adc


---
# <a name="widevine-license-template-overview"></a>Übersicht über die Widevine-Lizenzvorlage
## <a name="overview"></a>Übersicht
Azure Media Services ermöglicht jetzt das Konfigurieren und Anfordern von Widevine-Lizenzen. Wenn der Endbenutzer-Player versucht, Ihre durch Widevine geschützten Inhalte wiederzugeben, wird eine Anforderung zum Erwerben einer Lizenz an den Lizenzbereitstellungsdienst gesendet. Wenn der Lizenzdienst die Anforderung genehmigt, wird die Lizenz ausgegeben. Diese wird an den Client gesendet und kann zum Entschlüsseln und Wiedergeben des angegebenen Inhalts verwendet werden.

Die Widevine-Lizenzanforderung ist als JSON-Nachricht formatiert.  

Beachten Sie, dass Sie die Möglichkeit haben, eine leere Nachricht ohne Werte nur mit „{}“ zu erstellen, und eine Lizenzvorlage mit allen Standardeinstellungen wird erstellt.  

    {  
       “payload”:“<license challenge>”,
       “content_id”: “<content id>” 
       “provider”: ”<provider>”
       “allowed_track_types”:“<types>”,
       “content_key_specs”:[  
          {  
             “track_type”:“<track type 1>”
          },
          {  
             “track_type”:“<track type 2>”
          },
          …
       ],
       “policy_overrides”:{  
          “can_play”:<can play>,
          “can persist”:<can persist>,
          “can_renew”:<can renew>,
          “rental_duration_seconds”:<rental duration>,
          “playback_duration_seconds”:<playback duration>,
          “license_duration_seconds”:<license duration>,
          “renewal_recovery_duration_seconds”:<renewal recovery duration>,
          “renewal_server_url”:”<renewal server url>”,
          “renewal_delay_seconds”:<renewal delay>,
          “renewal_retry_interval_seconds”:<renewal retry interval>,
          “renew_with_usage”:<renew with usage>
       }
    }

## <a name="json-message"></a>JSON-Nachricht
| Name | Wert | Beschreibung |
| --- | --- | --- |
| payload |Base64-codierte Zeichenfolge |Die von einem Client gesendete Lizenzanforderung. |
| content_id |Base64-codierte Zeichenfolge |Bezeichner, der zum Ableiten von Schlüssel-IDs und Inhaltsschlüsseln für „content_key_specs.track_type“ verwendet wird. |
| Anbieter |string |Wird zum Nachschlagen von Inhaltsschlüsseln und Richtlinien verwendet. Erforderlich. |
| policy_name |string |Der Name einer zuvor registrierten Richtlinie. Optional |
| allowed_track_types |enum |SD_ONLY oder SD_HD. Steuert, welche Inhaltsschlüssel in eine Lizenz aufgenommen werden müssen |
| content_key_specs |Array von JSON-Strukturen, siehe **Spezifikationen für Inhaltsschlüssel** weiter unten |Eine feiner abgestimmte Steuerung der zurückzugebenden Inhaltsschlüssel. Einzelheiten finden Sie unter „Spezifikationen für Inhaltsschlüssel“ weiter unten.  Nur entweder „allowed_track_types“ oder „content_key_specs“ kann angegeben werden. |
| use_policy_overrides_exclusively |Boolescher Wert, "true" oder "false" |Verwenden Sie Richtlinienattribute, die von „policy_overrides“ angegeben werden, und lassen Sie alle zuvor gespeicherten Richtlinien weg. |
| policy_overrides |JSON-Struktur, siehe **Außerkraftsetzungen von Richtlinien** weiter unten |Richtlinieneinstellungen für diese Lizenz.  Falls dieses Medienobjekt über eine vorab definierte Richtlinie verfügt, werden diese angegebenen Werte verwendet. |
| session_init |JSON-Struktur, siehe **Sitzungsinitialisierung** weiter unten |Optionale Daten, die an die Lizenz übergeben werden. |
| parse_only |Boolescher Wert, "true" oder "false" |Die Lizenzanforderung wird analysiert, jedoch wird keine Lizenz ausgestellt. Allerdings werden Werte aus der Lizenzanforderung in der Antwort zurückgegeben. |

## <a name="content-key-specs"></a>Spezifikationen für Inhaltsschlüssel
Wenn eine Richtlinie bereits vorhanden ist, müssen keine Werte in der Spezifikation für Inhaltsschlüssel angegeben werden.  Mit der bereits vorhandenen Richtlinie, die diesem Inhalt zugeordnet ist, wird der Schutz der Ausgabe, z. B. HDCP und CGMS, bestimmt.  Wenn eine vorhandene Richtlinie nicht beim Widevine-Lizenzserver registriert ist, kann der Inhaltsanbieter die Werte in die Lizenzanforderung einfügen.   

Jeder content_key_specs-Wert muss für alle Titel angegeben werden, unabhängig von der Option „use_policy_overrides_exclusively“. 

| Name | Wert | Beschreibung |
| --- | --- | --- |
| content_key_specs. track_type |string |Der Name eines Titeltyps. Wenn „content_key_specs“ in der Lizenzanforderung angegeben ist, sollten Sie unbedingt alle Titeltypen explizit angeben. Andernfalls wird die Wiedergabe nach 10 Sekunden beendet. |
| content_key_specs  <br/> security_level |UInt32 |Definiert die Clientstabilitätsanforderungen für die Wiedergabe. <br/>  1 – Softwarebasierte White-Box-Verschlüsselung ist erforderlich. <br/>  2 – Softwareverschlüsselung und ein verborgener Decoder sind erforderlich. <br/>  3 – Die zentralen Vorgänge für Daten und Verschlüsselung müssen innerhalb einer hardwaregestützten vertrauenswürdigen Ausführungsumgebung ausgeführt werden. <br/>  4 – Verschlüsselung und Decodierung müssen innerhalb einer hardwaregestützten vertrauenswürdigen Ausführungsumgebung ausgeführt werden.  <br/>  5 – Verschlüsselung, Decodierung und Verarbeitung von Medien (komprimiert und nicht komprimiert) müssen innerhalb einer hardwaregestützten vertrauenswürdigen Ausführungsumgebung verarbeitet werden. |
| content_key_specs <br/> required_output_protection.hdc |Zeichenfolge: HDCP_NONE, HDCP_V1 oder HDCP_V2 |Gibt an, ob HDCP erforderlich ist. |
| content_key_specs <br/>key |Base64- <br/>codierte Zeichenfolge |Der Inhaltsschlüssel, der für diesen Titel verwendet werden soll. Wenn ein Wert angegeben wird, ist „track_type“ oder „key_id“ erforderlich.  Mit dieser Option können Inhaltsanbieter den Inhaltsschlüssel für diesen Titel einfügen, statt einen Schlüssel durch den Widevine-Lizenzserver zu generieren oder zu suchen. |
| content_key_specs.key_id |Base64-codierte binäre Zeichenfolge, 16 Bytes |Eindeutiger Bezeichner für den Schlüssel. |

## <a name="policy-overrides"></a>Außerkraftsetzungen von Richtlinien
| Name | Wert | Beschreibung |
| --- | --- | --- |
| policy_overrides. can_play |Boolescher Wert, "true" oder "false" |Gibt an, dass die Wiedergabe des Inhalts zulässig ist. Die Standardeinstellung ist "false". |
| policy_overrides. can_persist |Boolescher Wert, "true" oder "false" |Gibt an, dass die Lizenz in einem permanenten Speicher für die Offlineverwendung beibehalten werden kann. Die Standardeinstellung ist "false". |
| policy_overrides. can_renew |Boolescher Wert: true oder false |Gibt an, dass die Verlängerung dieser Lizenz zulässig ist. Bei „true“ kann die Dauer der Lizenz über den Takt verlängert werden. Die Standardeinstellung ist "false". |
| policy_overrides. license_duration_seconds |int64 |Gibt das Zeitfenster für diese bestimmte Lizenz an. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Der Standardwert ist 0. |
| policy_overrides. rental_duration_seconds |int64 |Gibt das Zeitfenster an, in dem die Wiedergabe zulässig ist. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Der Standardwert ist 0. |
| policy_overrides. playback_duration_seconds |int64 |Das Anzeigezeitfenster, sobald die Wiedergabe innerhalb der Lizenzdauer beginnt. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Der Standardwert ist 0. |
| policy_overrides. renewal_server_url |string |Alle Taktanforderungen (Verlängerung) für diese Lizenz werden an die angegebene URL weitergeleitet. Dieses Feld wird nur verwendet, wenn „can_renew“ auf „true“ festgelegt ist. |
| policy_overrides. renewal_delay_seconds |int64 |Die Anzahl der Sekunden nach „license_start_time“, bevor der erste Versuch einer Verlängerung unternommen wird. Dieses Feld wird nur verwendet, wenn „can_renew“ auf „true“ festgelegt ist. Der Standardwert ist 0. |
| policy_overrides. renewal_retry_interval_seconds |int64 |Gibt die Verzögerung in Sekunden zwischen den nachfolgenden Anforderungen zu Lizenzverlängerung an, falls ein Fehler auftritt. Dieses Feld wird nur verwendet, wenn „can_renew“ auf „true“ festgelegt ist. |
| policy_overrides. renewal_recovery_duration_seconds |int64 |Das Zeitfenster, für das eine Fortsetzung der Wiedergabe zugelassen wird, während der Versuch einer Verlängerung unternommen wird, aber aufgrund von Back-End-Problemen mit dem Lizenzserver noch nicht erfolgreich war. Der Wert 0 gibt an, dass es keine Beschränkung für die Dauer gibt. Dieses Feld wird nur verwendet, wenn „can_renew“ auf „true“ festgelegt ist. |
| policy_overrides. renew_with_usage |Boolescher Wert: true oder false |Gibt an, dass die Lizenz zur Verlängerung gesendet werden muss, wenn die Verwendung gestartet wird. Dieses Feld wird nur verwendet, wenn „can_renew“ auf „true“ festgelegt ist. |

## <a name="session-initialization"></a>Sitzungsinitialisierung
| Name | Wert | Beschreibung |
| --- | --- | --- |
| provider_session_token |Base64-codierte Zeichenfolge |Dieses Sitzungstoken wird wieder in der Lizenz übergeben und ist in nachfolgenden Verlängerungen vorhanden.  Das Sitzungstoken bleibt nicht über Sitzungen hinaus erhalten. |
| provider_client_token |Base64-codierte Zeichenfolge |Clienttoken, das in der Lizenzantwort zurück gesendet wird.  Wenn die Lizenzanforderung ein Clienttoken enthält, wird dieser Wert ignoriert. Das Clienttoken bleibt über Lizenzsitzungen hinaus erhalten. |
| override_provider_client_token |Boolescher Wert, "true" oder "false" |Wenn „false“ festgelegt wird und die Lizenzanforderung ein Clienttoken enthält, verwenden Sie das Token aus der Anforderung, selbst wenn ein Clienttoken in dieser Struktur angegeben wurde.  Verwenden Sie bei „true“ immer das Token, das in dieser Struktur angegeben ist. |

## <a name="configure-your-widevine-licenses-using-net-types"></a>Konfigurieren Ihrer Widevine-Lizenzen mit .NET-Typen
Media Services bietet .NET-APIs, mit denen Sie Ihre Widevine-Lizenzen konfigurieren können. 

### <a name="classes-as-defined-in-the-media-services-net-sdk"></a>Klassen, wie im Media Services .NET SDK definiert
Im Folgenden finden Sie die Definitionen dieser Typen.

    public class WidevineMessage
    {
        public WidevineMessage();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public AllowedTrackTypes? allowed_track_types { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public ContentKeySpecs[] content_key_specs { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public object policy_overrides { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum AllowedTrackTypes
    {
        SD_ONLY = 0,
        SD_HD = 1
    }
    public class ContentKeySpecs
    {
        public ContentKeySpecs();

        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string key_id { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public RequiredOutputProtection required_output_protection { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public int? security_level { get; set; }
        [JsonProperty(NullValueHandling = NullValueHandling.Ignore)]
        public string track_type { get; set; }
    }

    public class RequiredOutputProtection
    {
        public RequiredOutputProtection();

        public Hdcp hdcp { get; set; }
    }

    [JsonConverter(typeof(StringEnumConverter))]
    public enum Hdcp
    {
        HDCP_NONE = 0,
        HDCP_V1 = 1,
        HDCP_V2 = 2
    }

### <a name="example"></a>Beispiel
Das folgende Beispiel zeigt, wie Sie .NET-APIs verwenden, um eine einfache Widevine-Lizenz zu konfigurieren.

    private static string ConfigureWidevineLicenseTemplate()
    {
        var template = new WidevineMessage
        {
            allowed_track_types = AllowedTrackTypes.SD_HD,
            content_key_specs = new[]
            {
                new ContentKeySpecs
                {
                    required_output_protection = new RequiredOutputProtection { hdcp = Hdcp.HDCP_NONE},
                    security_level = 1,
                    track_type = "SD"
                }
            },
            policy_overrides = new
            {
                can_play = true,
                can_persist = true,
                can_renew = false
            }
        };

        string configuration = JsonConvert.SerializeObject(template);
        return configuration;
    }


## <a name="media-services-learning-paths"></a>Media Services-Lernpfade
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

## <a name="see-also"></a>Weitere Informationen
[Verwenden von dynamischer allgemeiner Verschlüsselung mit PlayReady und/oder Widevine](media-services-protect-with-drm.md)




<!--HONumber=Nov16_HO3-->



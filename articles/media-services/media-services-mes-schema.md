---
title: Media Encoder Standard-Schema | Microsoft Docs
description: "Dieses Thema enthält eine Übersicht über das Media Encoder Standard-Schema."
author: Juliako
manager: erikre
editor: 
services: media-services
documentationcenter: 
ms.assetid: 4c060062-8ef2-41d9-834e-e81e8eafcf2e
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/10/2017
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: e126076717eac275914cb438ffe14667aad6f7c8
ms.openlocfilehash: a526610f5b09ce73a9c192ec45ae8aafab001401


---
# <a name="media-encoder-standard-schema"></a>Media Encoder Standard-Schema
In diesem Thema werden einige Elemente und Typen des XML-Schemas beschrieben, auf dem [Media Encoder Standard-Voreinstellungen](media-services-mes-presets-overview.md) basieren. Das Thema enthält eine Beschreibung der Elemente und der dazugehörigen gültigen Werte. Das vollständige Schema wird zu einem späteren Zeitpunkt veröffentlicht.  

## <a name="a-namepreseta-preset-root-element"></a><a name="Preset"></a> Voreinstellung (Stammelement)
Dient zum Definieren einer Voreinstellung für die Codierung.  

### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Codieren** |[Codieren](media-services-mes-schema.md#Encoding) |Stammelement, mit dem angegeben wird, dass die Eingabequellen codiert werden sollen. |
| **Ausgaben** |[Ausgaben](media-services-mes-schema.md#Output) |Auflistung der gewünschten Ausgabedateien. |

### <a name="attributes"></a>Attribute
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Version**<br/><br/> Erforderlich |**xs:decimal** |Die voreingestellte Version. Es gelten die folgenden Einschränkungen: xs:fractionDigits value="1" und xs:minInclusive value="1", z.B. **version="1.0"**. |

## <a name="a-nameencodinga-encoding"></a><a name="Encoding"></a> Codieren
Enthält eine Sequenz der unten angegebenen Elemente.  

### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **H264Video** |[H264Video](media-services-mes-schema.md#H264Video) |Einstellungen für die H.264-Videocodierung |
| **AACAudio** |[AACAudio](media-services-mes-schema.md#AACAudio) |Einstellungen für die AAC-Codierung von Audiodaten |
| **BmpImage** |[BmpImage](media-services-mes-schema.md#BmpImage) |Einstellungen für BMP-Bild |
| **PngImage** |[PngImage](media-services-mes-schema.md#PngImage) |Einstellungen für PNG-Bild |
| **JpgImage** |[JpgImage](media-services-mes-schema.md#JpgImage) |Einstellungen für JPG-Bild |

## <a name="a-nameh264videoa-h264video"></a><a name="H264Video"></a> H264Video
### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **TwoPass**<br/><br/> minOccurs="0" |**xs:boolean** |Derzeit wird nur die Einwegverschlüsselung unterstützt. |
| **KeyFrameInterval**<br/><br/> minOccurs="0"<br/><br/> **default="00:00:02"** |**xs:time** |Bestimmt den Standardabstand zwischen IDR-Bildern. |
| **SceneChangeDetection**<br/><br/> minOccurs="0"<br/><br/> default="false" |**xs:boolean** |Bei der Einstellung auf „true“, versucht der Codierer, eine Szenenänderung im Video zu erkennen, und fügt ein IDR-Bild ein. |
| **Komplexität**<br/><br/> minOccurs="0"<br/><br/> default="Balanced" |**xs:string** |Steuert den Kompromiss zwischen Codiergeschwindigkeit und Videoqualität. Dies kann einer der folgenden Werte sein: **Speed**, **Balanced** oder **Quality**.<br/><br/> Standardeinstellung: **Balanced** |
| **SyncMode**<br/><br/> minOccurs="0" | |Diese Funktion wird in einer zukünftigen Version verfügbar gemacht. |
| **H264Layers**<br/><br/> minOccurs="0" |[H264Layers](media-services-mes-schema.md#H264Layers) |Auflistung von Ausgabevideoebenen |

## <a name="a-nameh264layersa-h264layers"></a><a name="H264Layers"></a> H264Layers
### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **H264Layer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[H264Layer](media-services-mes-schema.md#H264Layer) |Auflistung von H264-Ebenen |

## <a name="a-nameh264layera-h264layer"></a><a name="H264Layer"></a> H264Layer
> [!NOTE]
> Videogrenzwerte basieren auf den Werten, die in der Tabelle mit den [„H264-Levels“](https://en.wikipedia.org/wiki/H.264/MPEG-4_AVC#Levels) beschrieben sind.  
> 
> 

### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs="0"<br/><br/> default="Auto" |**xs:string** |Dies kann einer der folgenden **xs:string**-Werte sein: **Auto**, **Baseline**, **Main**, **High**. |
| **Level**<br/><br/> minOccurs="0"<br/><br/> default="Auto" |**xs:string** | |
| **Bitrate**<br/><br/> minOccurs="0" |**xs:int** |Bitrate für diese Videoebene in KBit/s |
| **MaxBitrate**<br/><br/> minOccurs="0" |**xs:int** |Maximale Bitrate für diese Videoebene in KBit/s |
| **BufferWindow**<br/><br/> minOccurs="0"<br/><br/> default="00:00:05" |**xs:time** |Länge des Videopuffers |
| **Width**<br/><br/> minOccurs="0" |**xs:int** |Breite des Ausgabevideobilds in Pixel.<br/><br/> Beachten Sie, dass Sie derzeit sowohl „Width“ als auch „Height“ angeben müssen. „Width“ und „Height“ müssen gerade Zahlen sein. |
| **Height**<br/><br/> minOccurs="0" |**xs:int** |Höhe des Ausgabevideobilds in Pixel.<br/><br/> Beachten Sie, dass Sie derzeit sowohl „Width“ als auch „Height“ angeben müssen. „Width“ und „Height“ müssen gerade Zahlen sein.|
| **BFrames**<br/><br/> minOccurs="0" |**xs:int** |Anzahl von B-Bildern zwischen Referenzbildern. |
| **ReferenceFrames**<br/><br/> minOccurs="0"<br/><br/> default="3" |**xs:int** |Anzahl von Referenzbildern in einer Bildgruppe (GOP). |
| **EntropyMode**<br/><br/> minOccurs="0"<br/><br/> default="Cabac" |**xs:string** |Dies kann einer der folgenden Werte sein: **Cabac** und **Cavlc**. |
| **FrameRate**<br/><br/> minOccurs="0" |Rationale Zahl |Bestimmt die Bildfrequenz des Ausgabevideos. Verwenden Sie den Standardwert "0/1", damit der Codierer die gleiche Bildfrequenz wie beim Eingabevideo nutzen kann. Zulässige Werte sind allgemeine Videobildfrequenzen. Dies ist unten angegeben. Es ist aber die Eingabe einer beliebigen gültigen rationalen Zahl zulässig. Für „1/1“ ergibt sich beispielsweise 1 Bild/Sekunde, also ein gültiger Wert.<br/><br/> - 12/1 (12 Bilder/Sekunde)<br/><br/> - 15/1 (15 Bilder/Sekunde)<br/><br/> - 24/1 (24 Bilder/Sekunde)<br/><br/> - 24.000/1.001 (23,976 Bilder/Sekunde)<br/><br/> - 25/1 (25 Bilder/Sekunde)<br/><br/>  - 30/1 (30 Bilder/Sekunde)<br/><br/> - 30.000/1.001 (29,97 Bilder/Sekunde) |
| **AdaptiveBFrame**<br/><br/> minOccurs="0" |**xs:boolean** |Kopieren vom Azure Media Encoder |
| **Slices**<br/><br/> minOccurs="0"<br/><br/> default="0" |**xs:int** |Bestimmt, in wie viele Slices ein Bild aufgeteilt wird. Es wird empfohlen, die Standardeinstellung zu verwenden. |

## <a name="a-nameaacaudioa-aacaudio"></a><a name="AACAudio"></a> AACAudio
 Enthält eine Sequenz der unten angegebenen Elemente und Gruppen.  

 Weitere Informationen zu AAC finden Sie unter [AAC](https://en.wikipedia.org/wiki/Advanced_Audio_Coding).  

### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Profil**<br/><br/> minOccurs="0"<br/><br/> default="AACLC" |**xs:string** |Dies kann einer der folgenden Werte sein: **AACLC**, **HEAACV1** oder **HEAACV2**. |

### <a name="attributes"></a>Attribute
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Condition** |**xs:string** |Um zu erzwingen, dass der Encoder ein Asset erstellt, das bei einer Eingabe ohne Audio eine stille Audiospur enthält, geben Sie den Wert „InsertSilenceIfNoAudio“ an.<br/><br/> Wenn Sie eine Eingabe an den Encoder senden, die keine Audiodaten, sondern nur Videodaten enthält, besteht das Ausgabeasset standardmäßig nur aus Dateien mit Videodaten. Einige Player können derartige Ausgabedatenströme möglicherweise nicht verarbeiten. Mit dieser Einstellung können Sie den Encoder zwingen, der Ausgabe in diesem Szenario eine stille Audiospur hinzuzufügen. |

### <a name="groups"></a>Gruppen
| Referenz | Beschreibung |
| --- | --- |
| [AudioGroup](media-services-mes-schema.md#AudioGroup)<br/><br/> minOccurs="0" |In der Beschreibung von [AudioGroup](media-services-mes-schema.md#AudioGroup) finden Sie Informationen zur richtigen Anzahl von Kanälen, zur Samplingrate und zur Bitrate, die für die einzelnen Profile festgelegt werden kann. |

## <a name="a-nameaudiogroupa-audiogroup"></a><a name="AudioGroup"></a> AudioGroup
Ausführliche Informationen dazu, welche Werte für die einzelnen Profile gelten, finden Sie unten in der Tabelle mit den „Audiocodec-Details“.  

### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Kanäle**<br/><br/> minOccurs="0" |**xs:int** |Gibt die Anzahl von codierten Audiokanälen an. Gültige Optionen sind: 1, 2, 5, 6, 8.<br/><br/> Standardeinstellung: 2. |
| **SamplingRate**<br/><br/> minOccurs="0" |**xs:int** |Audiosamplingrate in Hz |
| **Bitrate**<br/><br/> minOccurs="0" |**xs:int** |Bitrate für die Codierung der Audiodaten in KBit/s |

### <a name="audio-codec-details"></a>Audiocodec-Details
Audiocodec|Details  
-----------------|---  
**AACLC**|1:<br/><br/> - 11025 : 8 &lt;= Bitrate &lt; 16<br/><br/> - 12000 : 8 &lt;= Bitrate &lt; 16<br/><br/> - 16000 : 8 &lt;= Bitrate &lt;32<br/><br/>- 22050 : 24 &lt;= Bitrate &lt; 32<br/><br/> - 24000 : 24 &lt;= Bitrate &lt; 32<br/><br/> - 32000 : 32 &lt;= Bitrate &lt;= 192<br/><br/> - 44100 : 56 &lt;= Bitrate &lt;= 288<br/><br/> - 48000 : 56 &lt;= Bitrate &lt;= 288<br/><br/> - 88200 : 128 &lt;= Bitrate &lt;= 288<br/><br/> - 96000 : 128 &lt;= Bitrate &lt;= 288<br/><br/> 2:<br/><br/> - 11025 : 16 &lt;= Bitrate &lt; 24<br/><br/> - 12000 : 16 &lt;= Bitrate &lt; 24<br/><br/> - 16000 : 16 &lt;= Bitrate &lt; 40<br/><br/> - 22050 : 32 &lt;= Bitrate &lt; 40<br/><br/> - 24000 : 32 &lt;= Bitrate &lt; 40<br/><br/> - 32000 :  40 &lt;= Bitrate &lt;= 384<br/><br/> - 44100 : 96 &lt;= Bitrate &lt;= 576<br/><br/> - 48000 : 96 &lt;= Bitrate &lt;= 576<br/><br/> - 88200 : 256 &lt;= Bitrate &lt;= 576<br/><br/> - 96000 : 256 &lt;= Bitrate &lt;= 576<br/><br/> 5/6:<br/><br/> - 32000 : 160 &lt;= Bitrate &lt;= 896<br/><br/> - 44100 : 240 &lt;= Bitrate &lt;= 1024<br/><br/> - 48000 : 240 &lt;= Bitrate &lt;= 1024<br/><br/> - 88200 : 640 &lt;= Bitrate &lt;= 1024<br/><br/> - 96000 : 640 &lt;= Bitrate &lt;= 1024<br/><br/> 8:<br/><br/> - 32000 : 224 &lt;= Bitrate &lt;= 1024<br/><br/> - 44100 : 384 &lt;= Bitrate &lt;= 1024<br/><br/> - 48000 : 384 &lt;= Bitrate &lt;= 1024<br/><br/> - 88200 : 896 &lt;= Bitrate &lt;= 1024<br/><br/> - 96000 : 896 &lt;= Bitrate &lt;= 1024  
**HEAACV1**|1:<br/><br/> - 22050 : Bitrate = 8<br/><br/> - 24000 : 8 &lt;= Bitrate &lt; 10<br/><br/> - 32000 : 12 &lt;= Bitrate &lt;= 64<br/><br/> - 44100 : 20 &lt;= Bitrate &lt;= 64<br/><br/> - 48000 : 20 &lt;= Bitrate &lt;= 64<br/><br/> - 88200 : Bitrate = 64<br/><br/> 2:<br/><br/> - 32000 : 16 &lt;= Bitrate &lt;= 128<br/><br/> - 44100 : 16 &lt;= Bitrate &lt;= 128<br/><br/> - 48000 : 16 &lt;= Bitrate &lt;= 128<br/><br/> - 88200 : 96 &lt;= Bitrate &lt;= 128<br/><br/> - 96000 : 96 &lt;= Bitrate &lt;= 128<br/><br/> 5/6:<br/><br/> - 32000 : 64 &lt;= Bitrate &lt;= 320<br/><br/> - 44100 : 64 &lt;= Bitrate &lt;= 320<br/><br/> - 48000 : 64 &lt;= Bitrate &lt;= 320<br/><br/> - 88200 : 256 &lt;= Bitrate &lt;= 320<br/><br/> - 96000 : 256 &lt;= Bitrate &lt;= 320<br/><br/> 8:<br/><br/> - 32000 : 96 &lt;= Bitrate &lt;= 448<br/><br/> - 44100 : 96 &lt;= Bitrate &lt;= 448<br/><br/> - 48000 : 96 &lt;= Bitrate &lt;= 448<br/><br/> - 88200 : 384 &lt;= Bitrate &lt;= 448<br/><br/> - 96000 : 384 &lt;= Bitrate &lt;= 448  
**HEAACV2**|2:<br/><br/> - 22050 : 8 &lt;= Bitrate &lt;= 10<br/><br/> - 24000 : 8 &lt;= Bitrate &lt; 10<br/><br/> - 32000 : 12 &lt;= Bitrate &lt;= 64<br/><br/> - 44100 : 20 &lt;= Bitrate &lt;= 64<br/><br/> - 48000 : 20 &lt;= Bitrate &lt;= 64<br/><br/> - 88200 : 64 &lt;= Bitrate &lt;= 64  
  

## <a name="a-nameclipa-clip"></a><a name="Clip"></a> Clip
### <a name="attributes"></a>Attribute
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **StartTime** |**xs:duration** |Gibt die Startzeit einer Darstellung an. Der Wert von „StartTime“ muss mit den absoluten Zeitstempeln des Eingabevideos übereinstimmen. Wenn beispielsweise das erste Bild des Eingabevideos den Zeitstempel 12:00:10.000 hat, sollte „StartTime“ mindestens 12:00:10.000 betragen. |
| **Duration** |**xs:duration** |Gibt die Dauer einer Darstellung an (z.B. die Einblendung einer Überlagerung im Video). |

## <a name="a-nameoutputa-output"></a><a name="Output"></a> Ausgabe
### <a name="attributes"></a>Attribute
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **FileName** |**xs:string** |Der Name der Ausgabedatei.<br/><br/> Sie können die Makros in der folgenden Tabelle verwenden, um die Ausgabedateinamen zu erstellen. Beispiel:<br/><br/> **"Outputs": [      {       "FileName": "{Basename}*{Resolution}*{Bitrate}.mp4",       "Format": {         "Type": "MP4Format"       }     }   ]** |

### <a name="macros"></a>Makros
| Makro | Beschreibung |
| --- | --- |
| **{Basename}** |Wenn Sie die VoD-Codierung durchführen, umfasst {Basename} die ersten 32 Zeichen der AssetFile.Name-Eigenschaft für die primäre Datei im Eingabeasset.<br/><br/> Wenn das Eingabeasset ein Live-Archiv ist, wird {Basename} aus den trackName-Attributen im Servermanifest abgeleitet. Wenn Sie einen Subclipauftrag mit TopBitrate verwenden, z.B. „<VideoStream\>TopBitrate</VideoStream\>“, und die Ausgabedatei Videodaten enthält, umfasst {Basename} die ersten 32 Zeichen des trackName-Elements der Videoebene mit der höchsten Bitrate.<br/><br/> Falls Sie stattdessen zum Übermitteln eines Subclipauftrags alle Eingabebitraten verwenden, z.B. „<VideoStream\>*</VideoStream\>“, und die Ausgabedatei Videodaten enthält, umfasst {Basename} die ersten 32 Zeichen des trackName-Elements der entsprechenden Videoebene. |
| **{Codec}** |Wird „H264“ für Videodaten und „AAC“ für Audiodaten zugeordnet. |
| **{Bitrate}** |Gibt die Ziel-Videobitrate an, wenn die Ausgabedatei Video- und Audiodaten enthält, bzw. die Ziel-Audiobitrate, wenn die Ausgabedatei nur Audiodaten enthält. Als Wert wird die Bitrate in KBit/s verwendet. |
| **{Channel}** |Audiokanalanzahl, wenn die Datei Audiodaten enthält. |
| **{Width}** |Breite des Videos in der Ausgabedatei in Pixel, wenn die Datei Videodaten enthält. |
| **{Height}** |Höhe des Videos in der Ausgabedatei in Pixel, wenn die Datei Videodaten enthält. |
| **{Extension}** |Erbt von der „Type“-Eigenschaft für die Ausgabedatei. Der Name der Ausgabedatei hat eine der folgenden Erweiterungen: „mp4“, „ts“, „jpg“, „png“ oder „bmp“. |
| **{Index}** |Für Miniaturansichten obligatorisch. Sollte nur einmal vorhanden sein. |

## <a name="a-namevideoa-video-complex-type-inherits-from-codec"></a><a name="Video"></a> Video (komplexer Typ erbt vom Codec)
### <a name="attributes"></a>Attribute
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Starten** |**xs:string** | |
| **Schritt** |**xs:string** | |
| **Bereich** |**xs:string** | |
| **PreserveResolutionAfterRotation** |**xs:boolean** |Eine ausführliche Beschreibung finden Sie im Abschnitt [PreserveResolutionAfterRotation](media-services-mes-schema.md#PreserveResolutionAfterRotation). |

### <a name="a-namepreserveresolutionafterrotationa-preserveresolutionafterrotation"></a><a name="PreserveResolutionAfterRotation"></a> PreserveResolutionAfterRotation
Es wird empfohlen, das PreserveResolutionAfterRotation-Flag zusammen mit Auflösungswerten zu verwenden, die als Prozentsätze ausgedrückt werden (Width = „100%“, Height = „100%“).  

Standardmäßig sind die Einstellungen für die Codierungsauflösung (Width, Height) in den Voreinstellungen des Media Encoder Standard (MES) für Videos mit einer Drehung von 0 Grad bestimmt. Wenn Ihr Eingabevideo beispielsweise über eine Auflösung von 1.280 x 720 und eine Drehung von 0 Grad verfügt, wird mit den Standardvoreinstellungen sichergestellt, dass die Ausgabe die gleiche Auflösung aufweist. Siehe Abbildung unten.  

![MESRoation1](./media/media-services-shemas/media-services-mes-roation1.png) 

Wenn Eingangsvideodaten nicht mit einer Drehung von&0; Grad erfasst wurden (z.B. mit einem vertikal gehaltenen Smartphone oder Tablet), bedeutet dies aber Folgendes: MES wendet standardmäßig die Einstellungen für die Codierauflösung (Width, Height) auf die Eingangsvideodaten an und gleicht anschließend die Drehung aus. Ein Beispiel ist in der Abbildung unten zu sehen. Für die Voreinstellung wird Width = „100%“, Height = „100%“ verwendet. Dies wird von MES so interpretiert, dass die Ausgabe 1.280 Pixel breit und 720 Pixel hoch ist. Nach der Drehung des Videos wird das Bild dann so verkleinert, dass es in das Fenster passt, sodass sich links und rechts leere Bereiche ergeben.  

![MESRoation2](./media/media-services-shemas/media-services-mes-roation2.png) 

Falls das obige Verhalten nicht gewünscht ist, können Sie das PreserveResolutionAfterRotation-Flag verwenden und auf „true“ festlegen (Standardeinstellung ist „false“). Wenn also Width = „100%“, Height = „100%“ voreingestellt und PreserveResolutionAfterRotation auf „true“ festgelegt ist, wird für Eingangsvideodaten mit einer Breite von 1.280 Pixel und einer Höhe von 720 Pixel mit einer Drehung von 90 Grad eine Ausgabe mit 0 Grad Drehung und einer Breite von 720 Pixel und einer Höhe von 1.280 Pixel erzeugt. Siehe Abbildung unten.  

![MESRoation3](./media/media-services-shemas/media-services-mes-roation3.png) 

## <a name="a-nameformatgroupa-formatgroup-group"></a><a name="FormatGroup"></a> FormatGroup (Gruppe)
### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **BmpFormat** |**BmpFormat** | |
| **PngFormat** |**PngFormat** | |
| **JpgFormat** |**JpgFormat** | |

## <a name="a-namebmplayera-bmplayer"></a><a name="BmpLayer"></a> BmpLayer
### <a name="element"></a>Element
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Width**<br/><br/> minOccurs="0" |**xs:int** | |
| **Height**<br/><br/> minOccurs="0" |**xs:int** | |

### <a name="attributes"></a>Attribute
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Condition** |**xs:string** | |

## <a name="a-namepnglayera-pnglayer"></a><a name="PngLayer"></a> PngLayer
### <a name="element"></a>Element
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Width**<br/><br/> minOccurs="0" |**xs:int** | |
| **Height**<br/><br/> minOccurs="0" |**xs:int** | |

### <a name="attributes"></a>Attribute
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Condition** |**xs:string** | |

## <a name="a-namejpglayera-jpglayer"></a><a name="JpgLayer"></a> JpgLayer
### <a name="element"></a>Element
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Width**<br/><br/> minOccurs="0" |**xs:int** | |
| **Height**<br/><br/> minOccurs="0" |**xs:int** | |
| **Quality**<br/><br/> minOccurs="0" |**xs:int** |Gültige Werte: 1 (am schlechtesten) bis 100 (am besten) |

### <a name="attributes"></a>Attribute
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **Condition** |**xs:string** | |

## <a name="a-namepnglayersa-pnglayers"></a><a name="PngLayers"></a> PngLayers
### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **PngLayer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[PngLayer](media-services-mes-schema.md#PngLayer) | |

## <a name="a-namebmplayersa-bmplayers"></a><a name="BmpLayers"></a> BmpLayers
### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **BmpLayer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[BmpLayer](media-services-mes-schema.md#BmpLayer) | |

## <a name="a-namejpglayersa-jpglayers"></a><a name="JpgLayers"></a> JpgLayers
### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **JpgLayer**<br/><br/> minOccurs="0" maxOccurs="unbounded" |[JpgLayer](media-services-mes-schema.md#JpgLayer) | |

## <a name="a-namebmpimagea-bmpimage-complex-type-inherits-from-video"></a><a name="BmpImage"></a> BmpImage (komplexer Typ erbt vom Video)
### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-Ebenen |

## <a name="a-namejpgimagea-jpgimage-complex-type-inherits-from-video"></a><a name="JpgImage"></a> JpgImage (komplexer Typ erbt vom Video)
### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-Ebenen |

## <a name="a-namepngimagea-pngimage-complex-type-inherits-from-video"></a><a name="PngImage"></a> PngImage (komplexer Typ erbt vom Video)
### <a name="elements"></a>Elemente
| Name | Typ | Beschreibung |
| --- | --- | --- |
| **PngLayers**<br/><br/> minOccurs="0" |[PngLayers](media-services-mes-schema.md#PngLayers) |PNG-Ebenen |

## <a name="examples"></a>Beispiele
Beispiele für XML-Voreinstellungen, die basierend auf diesem Schema erstellt werden, finden Sie unter [Task Presets for MES (Media Encoder Standard)](media-services-mes-presets-overview.md) (Aufgabenvoreinstellungen für MES (Media Encoder Standard)).

## <a name="next-steps"></a>Nächste Schritte
[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>Feedback geben
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]




<!--HONumber=Feb17_HO3-->



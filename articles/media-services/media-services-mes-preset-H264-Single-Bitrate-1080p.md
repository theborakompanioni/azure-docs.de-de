---
title: "Media Encoder Standard-Voreinstellung „H264 Single Bitrate 1080p“ – Azure | Microsoft-Dokumentation"
description: "Das Thema enthält eine Übersicht über die Aufgabenvoreinstellung **H264 Single Bitrate 1080p**."
author: Juliako
manager: cfowler
editor: 
services: media-services
documentationcenter: 
ms.assetid: 0951fea8-15af-420b-9648-8c5c1abf8173
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: juliako
ms.translationtype: Human Translation
ms.sourcegitcommit: bdf41edfa6260749a91bc52ec0a2b62fcae99fb0
ms.openlocfilehash: 0ed50475d2ba34643f9e6d62491a8ec7ac40769b
ms.contentlocale: de-de
ms.lasthandoff: 01/27/2017

---

# <a name="h264-single-bitrate-1080p"></a>H264 Single Bitrate 1080p
`Media Encoder Standard` definiert eine Reihe von Codierungsvoreinstellungen, die Sie beim Erstellen von Codierungsaufträgen verwenden können. Sie können mithilfe von `preset name` angeben, in welchem Format Ihre Mediendatei codiert werden soll. Oder Sie erstellen eigene JSON- oder XML-basierte Voreinstellungen (mithilfe von UTF-8- oder UTF-16-Codierung). In diesem Fall übergeben Sie die benutzerdefinierte Voreinstellung dann an den Encoder. Eine Liste aller von diesem `Media Encoder Standard`-Encoder unterstützten Voreinstellungsnamen finden Sie unter [Task Presets for Media Encoder Standard](media-services-mes-presets-overview.md) (Aufgabenvoreinstellungen für Media Encoder Standard).  
  
 In diesem Thema wird die Voreinstellung `H264 Single Bitrate 1080p` im XML- und JSON-Format gezeigt.  
  
 Diese Voreinstellung erzeugt eine einzelne MP4-Datei mit einer Bitrate von 6.750 KBit/s und AAC-Stereoaudio. Ausführliche Informationen zu Profil, Bitrate, Samplingrate usw. dieser Voreinstellung finden Sie im unten aufgeführten XML- bzw. JSON-Code. Erläuterungen zur Bedeutung der einzelnen Elemente in diesen Voreinstellungen sowie gültige Werte für jedes Element finden Sie im Thema [Media Encoder Standard schema](media-services-mes-schema.md) (Media Encoder Standard-Schema).  
  
 XML  
  
```  
<?xml version="1.0" encoding="utf-16"?>  
<Preset xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" Version="1.0" xmlns="http://www.windowsazure.com/media/encoding/Preset/2014/03">  
  <Encoding>  
    <H264Video>  
      <KeyFrameInterval>00:00:02</KeyFrameInterval>  
      <SceneChangeDetection>true</SceneChangeDetection>  
      <H264Layers>  
        <H264Layer>  
          <Bitrate>6750</Bitrate>  
          <Width>1920</Width>  
          <Height>1080</Height>  
          <FrameRate>0/1</FrameRate>  
          <Profile>Auto</Profile>  
          <Level>auto</Level>  
          <BFrames>3</BFrames>  
          <ReferenceFrames>3</ReferenceFrames>  
          <Slices>0</Slices>  
          <AdaptiveBFrame>true</AdaptiveBFrame>  
          <EntropyMode>Cabac</EntropyMode>  
          <BufferWindow>00:00:05</BufferWindow>  
          <MaxBitrate>6750</MaxBitrate>  
        </H264Layer>  
      </H264Layers>  
      <Chapters />  
    </H264Video>  
    <AACAudio>  
      <Profile>AACLC</Profile>  
      <Channels>2</Channels>  
      <SamplingRate>48000</SamplingRate>  
      <Bitrate>128</Bitrate>  
    </AACAudio>  
  </Encoding>  
  <Outputs>  
    <Output FileName="{Basename}_{Width}x{Height}_{VideoBitrate}.mp4">  
      <MP4Format />  
    </Output>  
  </Outputs>  
</Preset>  
```  
  
 JSON  
  
```  
{  
  "Version": 1.0,  
  "Codecs": [  
    {  
      "KeyFrameInterval": "00:00:02",  
      "SceneChangeDetection": true,  
      "H264Layers": [  
        {  
          "Profile": "Auto",  
          "Level": "auto",  
          "Bitrate": 6750,  
          "MaxBitrate": 6750,  
          "BufferWindow": "00:00:05",  
          "Width": 1920,  
          "Height": 1080,  
          "BFrames": 3,  
          "ReferenceFrames": 3,  
          "AdaptiveBFrame": true,  
          "Type": "H264Layer",  
          "FrameRate": "0/1"  
        }  
      ],  
      "Type": "H264Video"  
    },  
    {  
      "Profile": "AACLC",  
      "Channels": 2,  
      "SamplingRate": 48000,  
      "Bitrate": 128,  
      "Type": "AACAudio"  
    }  
  ],  
  "Outputs": [  
    {  
      "FileName": "{Basename}_{Width}x{Height}_{VideoBitrate}.mp4",  
      "Format": {  
        "Type": "MP4Format"  
      }  
    }  
  ]  
}  
```


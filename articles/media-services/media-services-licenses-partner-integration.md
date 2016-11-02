<properties 
    pageTitle="Übermitteln von Widevine-Lizenzen an Azure Media Services mithilfe von Partnern | Microsoft Azure" 
    description="In diesem Artikel wird beschrieben, wie Sie mithilfe von Azure Media Services (AMS) einen Stream übermitteln, der von AMS mit PlayReady- und Widevine-DRMs dynamisch verschlüsselt wird. Die PlayReady-Lizenz stammt vom Media Services PlayReady-Lizenzserver, und die Widevine-Lizenz wird vom castLabs-Lizenzserver übermittelt." 
    services="media-services" 
    documentationCenter="" 
    authors="Juliako" 
    manager="erikre" 
    editor=""/>

<tags 
    ms.service="media-services" 
    ms.workload="media" 
    ms.tgt_pltfrm="na" 
    ms.devlang="na" 
    ms.topic="article" 
    ms.date="09/26/2016"  
    ms.author="juliako"/>


#<a name="using-partners-to-deliver-widevine-licenses-to-azure-media-services"></a>Übermitteln von Widevine-Lizenzen an Azure Media Services mithilfe von Partnern

##<a name="overview"></a>Übersicht

Mit Microsoft Azure Media Services können Sie mit Widevine DRM geschützte MPEG-DASH-Inhalte übermitteln. Widevine DRM wird gemäß der Common Encryption (CENC)-Spezifikation verschlüsselt.

Ab Media Services .NET SDK, Version 3.5.2, ermöglicht Media Services Ihnen die Konfiguration der Widevine-Lizenzvorlage und das Abrufen von Widevine-Lizenzen. Sie können sich auch von folgenden AMS-Partnern bei der Übermittlung von Widevine-Lizenzen unterstützen lassen: [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/), [EZDRM](http://ezdrm.com/), [castLabs](http://castlabs.com/company/partners/azure/).

##<a name="castlabs"></a>castLabs

Sie können Widevine-Lizenzen mithilfe von [castLabs](http://castlabs.com/company/partners/azure/) übermitteln. Weitere Informationen finden Sie unter [Übermitteln von DRM-Lizenzen an Azure Media Services mithilfe von castLabs](media-services-castlabs-integration.md)

##<a name="axinom"></a>Axinom

Sie können Widevine-Lizenzen mithilfe von [Axinom](http://www.axinom.com/press/ibc-axinom-drm-6/) übermitteln. Weitere Informationen finden Sie unter [Übermitteln von DRM-Lizenzen an Azure Media Services mithilfe von Axinom](media-services-axinom-integration.md)


##<a name="media-services-learning-paths"></a>Media Services-Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

##<a name="provide-feedback"></a>Feedback geben

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

##<a name="see-also"></a>Weitere Informationen

[Verwenden von dynamischer allgemeiner Verschlüsselung mit PlayReady und/oder Widevine.](media-services-protect-with-drm.md)

[Blog von Mingfei](https://azure.microsoft.com/blog/azure-media-services-adds-google-widevine-packaging-for-delivering-multi-drm-stream/)




<!--HONumber=Oct16_HO2-->



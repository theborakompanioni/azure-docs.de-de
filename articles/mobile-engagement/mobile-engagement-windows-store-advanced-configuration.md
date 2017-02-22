---
title: "Erweiterte Konfiguration für Engagement-SDK für Windows Universal-Apps"
description: "Erweiterte Konfigurationsoptionen für Azure Mobile Engagement mit Windows Universal-Apps"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6d85dd5d-ac07-43ba-bbe4-e91c3a17690b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-store
ms.devlang: dotnet
ms.topic: article
ms.date: 10/04/2016
ms.author: piyushjo;ricksal
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: cb9454212c94cf65093219c3d24c71277ede7877


---
# <a name="advanced-configuration-for-windows-universal-apps-engagement-sdk"></a>Erweiterte Konfiguration für Engagement-SDK für Windows Universal-Apps
> [!div class="op_single_selector"]
> * [Universal Windows](mobile-engagement-windows-store-advanced-configuration.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-configuration.md)
> 
> 

Hier wird erläutert, wie verschiedene Konfigurationsoptionen für Android-Apps in Azure Mobile Engagement konfiguriert werden.

## <a name="prerequisites"></a>Voraussetzungen
[!INCLUDE [Prereqs](../../includes/mobile-engagement-windows-store-prereqs.md)]

## <a name="advanced-configuration"></a>Erweiterte Konfiguration
### <a name="disable-automatic-crash-reporting"></a>Deaktivieren der automatischen Absturzberichtsfunktion
Sie können die automatische Absturzberichtsfunktion von Engagement deaktivieren. Dann erfolgt beim Auftreten einer nicht behandelten Ausnahme keine Aktion von Engagement.

> [!WARNING]
> Wenn Sie diese Funktion deaktivieren, sendet Engagement bei einem unbehandelten Absturz in Ihrer App keinen Absturzbericht **UND** schließt weder die Sitzung noch Aufträge.
> 
> 

Zum Deaktivieren der automatischen Absturzberichtsfunktion passen Sie Ihre Konfiguration an, je nachdem, wie Sie sie deklariert haben:

#### <a name="from-engagementconfigurationxml-file"></a>In der Datei `EngagementConfiguration.xml`
Setzen Sie die Berichterstattung bei Abstürzen zwischen den Tags `<reportCrash>` und `</reportCrash>` auf `false`.

#### <a name="from-engagementconfiguration-object-at-run-time"></a>Im Objekt `EngagementConfiguration` während der Laufzeit
Setzen Sie Absturzbericht auf „false“ mithilfe des Objekts „EngagementConfiguration“.

        /* Engagement configuration. */
        EngagementConfiguration engagementConfiguration = new EngagementConfiguration();
        engagementConfiguration.Agent.ConnectionString = "Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}";

        /* Disable Engagement crash reporting. */
        engagementConfiguration.Agent.ReportCrash = false;

### <a name="disable-real-time-reporting"></a>Deaktivieren der Echtzeit-Berichterstattung
Standardmäßig meldet der Engagement-Dienst Protokolle in Echtzeit. Wenn Ihre Anwendung Protokolle häufig meldet, ist es besser, die Protokolle zu puffern und sie in regelmäßigen Abständen alle auf einmal zu melden. Dies wird als „Burstmodus“ bezeichnet.

Rufen Sie dazu die folgende Methode auf:

        EngagementAgent.Instance.SetBurstThreshold(int everyMs);

Das Argument ist ein Wert in **Millisekunden**. Immer wenn Sie die Protokollierung in Echtzeit erneut aktivieren möchten, rufen Sie die Methode ohne Parameter oder mit dem Wert 0 auf.

Der Burstmodus verlängert leicht die Akkulaufzeit, wirkt sich jedoch auf den Engagement-Monitor aus: Die Dauer von allen Sitzungen und Aufträgen wird auf den Burstschwellenwert gerundet (folglich sind eventuell Sitzungen und Aufträge, die kürzer als der Burstschwellenwert sind, möglicherweise nicht sichtbar). Sie sollten einen Burstschwellenwert von höchstens 30.000 (30s) verwenden. Gespeicherte Protokolle sind auf 300 Elemente beschränkt. Wenn der Sendevorgang zu lange dauert, können einige Protokolle verloren gehen.

> [!WARNING]
> Der Burstschwellenwert kann nicht auf einen Zeitraum von weniger als einer Sekunde konfiguriert werden. Wenn Sie dies versuchen, zeigt das SDK eine Ablaufverfolgung mit dem Fehler an und setzt sich automatisch auf den Standardwert (null Sekunden) zurück. Dies löst aus, dass das SDK die Protokolle in Echtzeit meldet.
> 
> 

[here]:http://www.nuget.org/packages/Capptain.WindowsCS
[NuGet website]:http://docs.nuget.org/docs/start-here/overview



<!--HONumber=Feb17_HO3-->



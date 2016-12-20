---
title: "Erweiterte Berichtserstellungsoptionen für das Android-SDK für Azure Mobile Engagement"
description: "Hier wird beschrieben, wie Sie mit der erweiterten Berichterstellung Analysedaten für das Android-SDK für Azure Mobile Engagement erfassen."
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 7da7abd5-19d6-4892-94d8-818e5424b2cd
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: mobile-android
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2016
ms.author: piyushjo;ricksal
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 2a1445afa2c2fca1a31ad9c012b9c8a917ebf65c


---
# <a name="advanced-reporting-with-engagement-on-android"></a>Erweiterte Berichterstellung mit Engagement unter Android
> [!div class="op_single_selector"]
> * [Universal Windows](mobile-engagement-windows-store-integrate-engagement.md)
> * [Windows Phone Silverlight](mobile-engagement-windows-phone-integrate-engagement.md)
> * [iOS](mobile-engagement-ios-integrate-engagement.md)
> * [Android](mobile-engagement-android-advanced-reporting.md)
> 
> 

Dieses Thema beschreibt zusätzliche Berichterstellungsszenarien in Ihrer Android-Anwendung. Sie können diese Optionen auf die App anwenden, die im [Erste Schritte](mobile-engagement-android-get-started.md) -Tutorial erstellt wird.

## <a name="prerequisites"></a>Voraussetzungen
[!INCLUDE [Prereqs](../../includes/mobile-engagement-android-prereqs.md)]

Das Tutorial, das Sie abgeschlossen haben, war absichtlich direkt und einfach gehalten, aber Sie können aus einer Reihe von erweiterten Optionen auswählen.

## <a name="modifying-your-activity-classes"></a>Ändern Ihrer `Activity` -Klassen
Im Tutorial [Erste Schritte](mobile-engagement-android-get-started.md) mussten Sie lediglich dafür sorgen, dass die `*Activity`-Unterklassen von den entsprechenden `Engagement*Activity`-Klassen erben. Wenn z.B. Ihre Legacyaktivität `ListActivity` erweiterte, war die Erweiterung von `EngagementListActivity` erforderlich.

> [!IMPORTANT]
> Stellen Sie bei der Verwendung von `EngagementListActivity` oder `EngagementExpandableListActivity` sicher, dass jeder Aufruf von `requestWindowFeature(...);` vor dem Aufruf von `super.onCreate(...);` erfolgt, da es andernfalls zu einem Absturz kommt.
> 
> 

Sie finden diese Klassen im Ordner `src` , und Sie können sie in Ihr Projekt kopieren. Die Klassen befinden sich auch in **JavaDoc**.

## <a name="alternate-method-call-startactivity-and-endactivity-manually"></a>Alternative Methode: `startActivity()` und `endActivity()` manuell aufrufen.
Wenn Sie die `Activity`-Klassen nicht überladen können oder möchten, können Sie die Aktivitäten stattdessen durch direktes Aufrufen der Methoden von `EngagementAgent` starten und beenden.

> [!IMPORTANT]
> Das Android-SDK ruft die `endActivity()`-Methode niemals auf, auch nicht beim Schließen der Anwendung (unter Android werden Anwendungen niemals geschlossen). Daher wird *DRINGEND* empfohlen, die `startActivity()`-Methode in der `onResume`-Rückruffunktion *ALLER* Aktivitäten und die `endActivity()`-Methode in der `onPause()`-Rückruffunktion *ALLER* Aktivitäten aufzurufen. Dies ist die einzige Möglichkeit, um sicherzustellen, dass die Sitzungen nicht verloren gehen. Wenn eine Sitzung verloren geht, wird die Verbindung vom Engagement-Dienst zum Engagement-Back-End niemals getrennt (da der Dienst verbunden bleibt, so lange eine Sitzung aussteht).
> 
> 

Beispiel:

    public class MyActivity extends Some3rdPartyActivity
    {
      @Override
      protected void onResume()
      {
        super.onResume();
        String activityNameOnEngagement = EngagementAgentUtils.buildEngagementActivityName(getClass()); // Uses short class name and removes "Activity" at the end.
        EngagementAgent.getInstance(this).startActivity(this, activityNameOnEngagement, null);
      }

      @Override
      protected void onPause()
      {
        super.onPause();
        EngagementAgent.getInstance(this).endActivity();
      }
    }

Dieses Beispiel ähnelt der `EngagementActivity`-Klasse und ihren Varianten, deren Quellcode im Ordner `src` bereitgestellt wird.

## <a name="using-applicationoncreate"></a>Verwenden von "Application.onCreate()"
Sämtlicher in `Application.onCreate()` und in anderen Anwendungsrückruffunktionen hinzugefügter Code wird für alle Prozesse der Anwendung ausgeführt, einschließlich des Engagement-Diensts. Dies kann unerwünschte Nebeneffekte haben, z. B. nicht erforderliche Speicherbelegungen und Threads im Engagement-Prozess oder doppelte Übertragungsempfänger oder -dienste.

Wenn Sie `Application.onCreate()` außer Kraft setzen, sollten Sie den folgenden Codeausschnitt am Anfang der `Application.onCreate()`-Funktion hinzufügen:

     public void onCreate()
     {
       if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
         return;

       ... Your code...
     }

Sie können die gleichen Schritte für `Application.onTerminate()`, `Application.onLowMemory()` und `Application.onConfigurationChanged(...)` ausführen.

Sie können auch `EngagementApplication` anstelle von `Application` erweitern: Die Rückruffunktion `Application.onCreate()` übernimmt die Prozessprüfung und ruft `Application.onApplicationProcessCreate()` nur auf, wenn es sich bei dem aktuellen Prozess nicht um den Prozess handelt, der den Engagement-Dienst hostet. Dieselben Regeln gelten für die anderen Rückruffunktionen.

## <a name="tags-in-the-androidmanifestxml-file"></a>Tags in der Datei "AndroidManifest.xml"
Das `android:label`-Attribut ermöglicht Ihnen, im Diensttag der Datei AndroidManifest.xml den Namen des Engagement-Diensts so auszuwählen, wie er den Endbenutzern auf dem Bildschirm für aktive Dienste ihres Mobiltelefons angezeigt wird. Sie sollten dieses Attribut auf `"<Your application name>Service"` festlegen (z.B. `"AcmeFunGameService"`).

Durch die Angabe des Attributs `android:process` wird sichergestellt, dass der Engagement-Dienst im eigenen Prozess ausgeführt wird (durch Ausführen von Engagement im gleichen Prozess wie die Anwendung wird der Thread „main/UI“ potenziell weniger reaktionsfähig).

## <a name="building-with-proguard"></a>Erstellen mit ProGuard
Wenn Sie das Anwendungspaket mithilfe von ProGuard erstellen, müssen Sie einige Klassen behalten. Sie können den folgenden Codeausschnitt der Konfiguration verwenden:

    -keep public class * extends android.os.IInterface
    -keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
    <methods>;
     }



<!--HONumber=Nov16_HO3-->



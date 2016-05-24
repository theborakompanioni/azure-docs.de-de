<properties
	pageTitle="Erweiterte Berichtserstellungsoptionen für Android Azure Mobile Engagement SDK"
	description="Erweiterte Berichtserstellungsoptionen für Android für Azure Mobile Engagement SDK"
	services="mobile-engagement"
	documentationCenter="mobile"
	authors="piyushjo"
	manager="erikre"
	editor="" />

<tags
	ms.service="mobile-engagement"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-android"
	ms.devlang="Java"
	ms.topic="article"
	ms.date="05/04/2016"
	ms.author="piyushjo;ricksal" />

# Berichtserstellungsoptionen mit Engagement unter Android

> [AZURE.SELECTOR]
- [Android](mobile-engagement-android-advanced-reporting.md)

Dieses Thema beschreibt zusätzliche Berichtserstellungsszenarien in Ihrer Android-Anwendung. Das sind Optionen, die Sie auswählen und auf die App anwenden können, die im [Erste Schritte](mobile-engagement-android-get-started.md)-Lernprogramm erstellt wird.

## Voraussetzungen

[AZURE.INCLUDE [Voraussetzungen](../../includes/mobile-engagement-android-prereqs.md)]

Das Lernprogramm, das Sie abgeschlossen haben, war absichtlich direkt und einfach, aber Sie können aus einer Reihe von Optionen auswählen.

## Erstellen mit ProGuard

Wenn Sie das Anwendungspaket mithilfe von ProGuard erstellen, müssen Sie einige Klassen behalten. Sie können den folgenden Codeausschnitt der Konfiguration verwenden:


			-keep public class * extends android.os.IInterface
			-keep class com.microsoft.azure.engagement.reach.activity.EngagementWebAnnouncementActivity$EngagementReachContentJS {
			<methods>;
		 	}

## Tags in der Datei "AndroidManifest.xml"

Im Dienst-Tag der Datei "AndroidManifest.xml" ermöglicht es das `android:label`-Attribut Ihnen, den Namen des Engagement-Diensts so auszuwählen, wie er den Endbenutzern auf dem Bildschirm für aktive Dienste ihres Mobiltelefons angezeigt wird. Es wird empfohlen, dieses Attribut auf `"<Your application name>Service"` festzulegen (z. B. `"AcmeFunGameService"`).

Durch die Angabe des Attributs `android:process` wird sichergestellt, dass der Engagement-Dienst im eigenen Prozess ausgeführt wird (durch Ausführen von Engagement in demselben Prozess wie die Anwendung wird der Thread „main/UI“ potenziell weniger reaktionsfähig).

## Verwenden von "Application.onCreate()"

Sämtlicher in `Application.onCreate()` und in anderen Anwendungsrückruffunktionen hinzugefügter Code wird für alle Prozesse der Anwendung ausgeführt, einschließlich des Engagement-Diensts. Dies kann unerwünschte Nebeneffekte haben, z. B. nicht erforderliche Speicherbelegungen und Threads im Engagement-Prozess oder doppelte Übertragungsempfänger oder -dienste.

Wenn Sie `Application.onCreate()` außer Kraft setzen, wird empfohlen, den folgenden Codeausschnitt am Anfang der `Application.onCreate()`-Funktion hinzuzufügen:

			 public void onCreate()
			 {
			   if (EngagementAgentUtils.isInDedicatedEngagementProcess(this))
			     return;

			   ... Your code...
			 }

Sie können dieselben Schritte für `Application.onTerminate()`, `Application.onLowMemory()` und `Application.onConfigurationChanged(...)` ausführen.

Sie können auch `EngagementApplication` anstelle von `Application` erweitern: Die Rückruffunktion `Application.onCreate()` übernimmt die Prozessprüfung und ruft `Application.onApplicationProcessCreate()` nur auf, wenn es sich bei dem aktuellen Prozess nicht um den Prozess handelt, der den Engagement-Dienst hostet. Dieselben Regeln gelten für die anderen Rückruffunktionen.

## Ändern der `Activity`-Klassen

Im [Erste-Schritte-Lernprogramm](mobile-engagement-android-get-started.md) mussten Sie lediglich dafür sorgen, dass die `*Activity`-Unterklassen von den entsprechenden `Engagement*Activity`-Klassen erben (wenn z. B. Ihre Legacyaktivität `ListActivity` erweiterte, war die Erweiterung von `EngagementListActivity` erforderlich).

> [AZURE.IMPORTANT] Stellen Sie bei der Verwendung von `EngagementListActivity` oder `EngagementExpandableListActivity` sicher, dass jeder Aufruf von `requestWindowFeature(...);` vor dem Aufruf von `super.onCreate(...);` erfolgt, da es andernfalls zu einem Absturz kommt.

Sie finden diese Klassen im Ordner `src`, und Sie können sie in Ihr Projekt kopieren. Die Klassen befinden sich auch in **JavaDoc**.

## Alternative Methode: `startActivity()` und `endActivity()` manuell aufrufen.

Wenn Sie die `Activity`-Klassen nicht überladen können oder möchten, können Sie die Aktivitäten stattdessen durch direktes Aufrufen der Methoden von `EngagementAgent` starten und beenden.

> [AZURE.IMPORTANT] Das Android-SDK ruft die `endActivity()`-Methode niemals auf, auch nicht beim Schließen der Anwendung (unter Android werden Anwendungen eigentlich niemals geschlossen). Daher wird *DRINGEND* empfohlen, die `startActivity()`-Methode in der `onResume`-Rückruffunktion *ALLER* Aktivitäten und die `endActivity()`-Methode in der `onPause()`-Rückruffunktion *ALLER* Aktivitäten aufzurufen. Dies ist die einzige Möglichkeit, um sicherzustellen, dass die Sitzungen nicht verloren gehen. Wenn eine Sitzung verloren geht, wird die Verbindung vom Engagement-Dienst zum Engagement-Back-End niemals getrennt (da der Dienst verbunden bleibt, so lange eine Sitzung aussteht).

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

Dieses Beispiel ähnelt der `EngagementActivity`-Klasse und ihrer Varianten, deren Quellcode im Ordner `src` bereitgestellt wird.

<!---HONumber=AcomDC_0511_2016-->
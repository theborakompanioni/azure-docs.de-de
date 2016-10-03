<properties
	pageTitle="Aktivieren von App-übergreifendem SSO auf Android mit ADAL | Microsoft Azure"
	description="Verwenden der ADAL SDK-Features zum Aktivieren von SSO über Ihre Anwendungen hinweg. "
	services="active-directory"
	documentationCenter=""
	authors="brandwe"
	manager="mbaldwin"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="android"
	ms.devlang="java"
	ms.topic="article"
	ms.date="09/16/2016"
	ms.author="brandwe"/>


# Aktivieren von App-übergreifendem SSO auf Android mit ADAL


Die Möglichkeit des einmaligen Anmeldens (Single Sign-On, SSO), bei dem Benutzer ihre Anmeldeinformationen nur einmal eingeben und diese Anmeldeinformationen automatisch auch für andere Anwendungen funktionieren, wird heute von den Kunden erwartet. Die Schwierigkeit, den Benutzernamen und das Kennwort auf einem kleinen Bildschirm einzugeben – häufig in Kombination mit einer weiteren Datenabfrage wie etwa einem Telefonanruf oder einem per SMS gesendeten Code (zweistufige Authentifizierung) – führt beim Benutzer schnell zu Unmut, wenn dieser Vorgang für Ihr Produkt mehrfach durchgeführt werden muss.

Wenn Sie außerdem eine Identitätsplattform nutzen, die andere Anwendungen verwenden können (beispielsweise Microsoft-Konten oder ein Office365-Geschäftskonto), erwarten Kunden, dass diese Anmeldeinformationen unabhängig vom Anbieter über all ihre Anwendungen hinweg verfügbar sind.

Die Microsoft Identity-Plattform nimmt Ihnen in Kombination mit unseren Microsoft Identity-SDKs diese harte Arbeit ab. So können Sie Ihren Kunden SSO entweder innerhalb Ihrer eigenen Anwendungssuite oder – mithilfe unserer Brokerfeatures und Authentifizierungsanwendungen – für das gesamte Gerät bereitstellen.

In dieser exemplarischen Vorgehensweise erfahren Sie, wie Sie unser SDK innerhalb Ihrer Anwendung konfigurieren, damit Ihre Kunden von diesen Vorteilen profitieren.

Die vorliegende exemplarische Vorgehensweise gilt für:

* Azure Active Directory
* Azure Active Directory B2C
* Azure Active Directory B2B
* Bedingter Zugriff mit Azure Active Directory

Hinweis: In diesem Dokument wird davon ausgegangen, dass Sie wissen, wie Sie [Anwendungen im Vorgängerportal für Azure Active Directory bereitstellen](active-directory-how-to-integrate.md), und dass Sie das [Microsoft Identity Android SDK](https://github.com/AzureAD/azure-activedirectory-library-for-android) in Ihre Anwendung integriert haben.

## SSO-Konzepte in der Microsoft Identity-Plattform

### Microsoft-Identitätsbroker

Microsoft stellt Anwendungen für jede mobile Plattform bereit, die als Brücke für Anmeldeinformationen von Anwendungen verschiedener Anbieter fungieren sowie spezielle erweiterte Features unterstützen, bei denen die Überprüfung der Anmeldeinformationen über eine einzige sichere Stelle erfolgt. Wir nennen diese Komponenten **Broker**. Unter iOS und Android werden diese über herunterladbare Anwendungen bereitgestellt. Diese Anwendungen können Kunden entweder unabhängig installieren, oder sie werden von Unternehmen, die einige oder alle Geräte für ihre Mitarbeiter verwalten, per Push auf das Gerät übertragen. Diese Broker unterstützen die Sicherheitsverwaltung basierend auf der Konfiguration des IT-Administrators entweder nur für einige Anwendungen oder für das gesamte Gerät. Unter Windows wird diese Funktion durch eine Kontoauswahl bereitgestellt, die in das Betriebssystem integriert ist. Die technische Bezeichnung hierfür lautet Webauthentifizierungsbroker.

Um zu verstehen, wie wir diese Broker einsetzen und wie Ihre Kunden diese während des Anmeldeflusses für die Microsoft Identity-Plattform wahrnehmen, werden diese Abläufe im Folgenden näher erläutert.

### Muster für die Anmeldung auf mobilen Geräten

Der Zugriff auf Anmeldeinformationen auf Geräten folgt für die Microsoft Identity-Plattform zwei grundlegenden Mustern:

* Nicht brokergestützte Anmeldungen
* Brokergestützte Anmeldungen

#### Nicht brokergestützte Anmeldungen

Nicht brokergestützte Anmeldungen sind Anmeldungen, die anwendungsintern erfolgen und den lokalen Gerätespeicher für diese Anwendung nutzen. Dieser Speicher kann für mehrere Anwendungen freigegeben werden, aber die Anmeldeinformationen sind eng an die App oder App-Suite gebunden, die diese Anmeldeinformationen nutzt. Dieses Muster kennen Sie wahrscheinlich von zahlreichen mobilen Anwendungen, bei denen Sie innerhalb der Anwendung einen Benutzernamen und ein Kennwort eingeben.

Diese Anmeldungen bieten folgende Vorteile:

-  Die Benutzerinteraktion läuft vollständig innerhalb der Anwendung ab.
-  Anmeldeinformationen können gemeinsam mit anderen Anwendungen genutzt werden, die das gleiche Signaturzertifikat verwenden. Auf diese Weise erzielen Sie SSO-Funktionalität für Ihre Anwendungssuite.
-  Die Kontrolle der Anmeldeabläufe liegt sowohl vor als auch nach der Anmeldung bei der Anwendung.

Diese Anmeldungen haben folgende Nachteile:

- Benutzer können nicht bei allen Apps mit einer Microsoft-Identität von SSO profitieren, sondern nur bei den Microsoft-Identitäten, die Ihre Anwendung besitzt und konfiguriert hat.
- Ihre Anwendung kann nicht mit erweiterten Geschäftsfunktionen wie etwa dem bedingten Zugriff oder der Intune-Produktsuite verwendet werden.
- Ihre Anwendung kann keine auf Zertifikaten basierende Authentifizierung für Geschäftsbenutzer bereitstellen.

Die folgende Abbildung zeigt, wie die Microsoft Identity SDKs zusammen mit dem gemeinsam genutzten Speicher Ihrer Anwendung für die Bereitstellung von SSO sorgen:

```
+------------+ +------------+  +-------------+
|            | |            |  |             |
|   App 1    | |   App 2    |  |   App 3     |
|            | |            |  |             |
|            | |            |  |             |
+------------+ +------------+  +-------------+
| Azure SDK  | | Azure SDK  |  | Azure SDK   |
+------------+-+------------+--+-------------+
|                                            |
|            App Shared Storage              |
+--------------------------------------------+
```

#### Brokergestützte Anmeldungen

Brokergestützte Anmeldungen sind Anmeldeabläufe, die innerhalb der Brokeranwendung stattfinden und auf den Speicher und die Sicherheit des Brokers zurückgreifen, um Anmeldeinformationen für alle Anwendungen auf dem Gerät einzusetzen, die die Microsoft Identity-Plattform nutzen. Das bedeutet, dass Ihre Anwendungen bei der Benutzeranmeldung auf den Broker zurückgreifen. Unter iOS und Android wird diese Funktion über herunterladbare Anwendungen bereitgestellt. Diese Anwendungen können Kunden entweder unabhängig installieren, oder sie werden von Unternehmen, die das Gerät für ihre Benutzer verwalten, per Push auf das Gerät übertragen. Ein Beispiel für diesen Anwendungstyp ist die Azure Authenticator-App unter iOS. Unter Windows wird diese Funktionalität durch eine Kontoauswahl bereitgestellt, die in das Betriebssystem integriert ist. Die technische Bezeichnung hierfür lautet Webauthentifizierungsbroker. Der Ablauf variiert je nach Plattform, und eine nicht ordnungsgemäße Verwaltung kann die Benutzererfahrung beeinträchtigen. Sie kennen dieses Muster sicherlich, wenn Sie Facebook installiert haben und die Facebook-Anmeldefunktion in einer anderen Anwendung verwenden. Die Microsoft Identity-Plattform bedient sich des gleichen Musters.

Bei iOS führt dies zu einer „Übergangsanimation“, bei der Ihre Anwendung in den Hintergrund und die Azure Authenticator-Anwendungen in den Vordergrund rückt, sodass der Benutzer das gewünschte Konto für die Anmeldung auswählen kann.

Unter Android und Windows wird die Kontoauswahl im Vordergrund Ihrer Anwendung angezeigt, was für den Benutzer weniger störend ist.

#### Aufrufen des Brokers

Wenn ein kompatibler Broker auf dem Gerät installiert ist, beispielsweise die Azure Authenticator-Anwendung, übernehmen die Microsoft Identity SDKs automatisch den Aufruf des Brokers für Sie, wenn ein Benutzer angibt, dass er sich mit einem beliebigen Konto von der Microsoft Identity-Plattform aus anmelden möchte. Hierbei kann es sich um ein persönliches Microsoft-Konto, ein Geschäfts- oder Schulkonto oder um ein Konto handeln, das Sie über eines unserer B2C- und B2B-Produkte in Azure bereitstellen und hosten. Durch die Verwendung extrem sicherer Algorithmen und Verschlüsselungen stellen wir sicher, dass die Anmeldeinformationen auf sichere Weise abgerufen und an Ihre Anwendung zurückgegeben werden. Die technischen Details dieser Mechanismen werden nicht veröffentlicht, wurden aber in Zusammenarbeit mit Apple und Google entwickelt.

**Der Entwickler kann entscheiden, ob das Microsoft Identity SDK den Broker aufrufen oder den nicht brokergestützten Ablauf verwenden soll.** Wenn sich der Entwickler allerdings gegen den brokergestützten Ablauf entscheidet, verzichtet er auf die Vorteile der Nutzung von SSO-Anmeldeinformationen, die der Benutzer möglicherweise bereits auf dem Gerät hinzugefügt hat. Außerdem kann die Anwendung ohne brokergestützte Anmeldung keine der von Microsoft bereitgestellten Geschäftsfunktionen wie etwa den bedingten Zugriff, Intune-Verwaltungsfunktionen und eine zertifikatbasierte Authentifizierung nutzen.

Diese Anmeldungen bieten die folgenden Vorteile:

-  Benutzer profitieren bei allen ihren Anwendungen von SSO – unabhängig vom Anbieter.
-  Ihre Anwendung kann mit erweiterten Geschäftsfunktionen wie etwa dem bedingten Zugriff oder der Intune-Produktsuite verwendet werden.
-  Ihre Anwendung kann eine auf Zertifikaten basierende Authentifizierung für Geschäftsbenutzer bereitstellen.
- Die Anmeldung ist deutlich sicherer, da die Identität der Anwendung und des Benutzers von der Brokeranwendung mit zusätzlichen Sicherheitsalgorithmen und Verschlüsselung verifiziert wird.

Diese Anmeldungen haben folgende Nachteile:

- Unter iOS muss der Benutzer die Anmeldeinformationen außerhalb der Anwendung auswählen.
- Die Anmeldeumgebung für Ihre Kunden kann nicht innerhalb Ihrer Anwendung verwaltet werden.



Die folgende Abbildung zeigt, wie die Microsoft Identity SDKs zusammen mit den Brokeranwendungen für die Bereitstellung von SSO sorgen:

```
+------------+ +------------+   +-------------+
|            | |            |   |             |
|   App 1    | |   App 2    |   |   Someone   |
|            | |            |   |    Else's   |
|            | |            |   |     App     |
+------------+ +------------+   +-------------+
| Azure SDK  | | Azure SDK  |   | Azure SDK   |
+-----+------+-+-----+------+-  +-------+-----+
      |              |                  |
      |       +------v------+           |
      |       |             |           |
      |       | Microsoft   |           |
      +-------> Broker      |^----------+
              | Application
              |             |
              +-------------+
              |             |
              |   Broker    |
              |   Storage   |
              |             |
              +-------------+

```
              
Mit diesen Hintergrundinformationen verfügen Sie nun über ein besseres Verständnis der Funktionsweise und Implementierung von SSO innerhalb Ihrer Anwendung mithilfe der Microsoft Identity-Plattform und den zugehörigen SDKs verfügen.


## Aktivieren von App-übergreifendem SSO mit ADAL

In diesem Abschnitt verwenden wir das ADAL Android SDK für folgende Aufgaben:

- Aktivieren von nicht brokergestütztem SSO für Ihre App-Suite
- Aktivieren der Unterstützung für brokergestütztes SSO


### Aktivieren von SSO für nicht brokergestütztes SSO

Für nicht brokergestütztes, anwendungsübergreifendes SSO übernehmen die Microsoft Identity SDKs einen Großteil der komplexen SSO-Verwaltung für Sie. Hierzu gehört das Suchen des richtigen Benutzers im Cache sowie das Verwalten einer Liste der angemeldeten Benutzer, die Sie abfragen können.

Gehen Sie folgendermaßen vor, um übergreifendes SSO für Anwendungen in Ihrem Besitz zu aktivieren:

1. Stellen Sie sicher, dass all Ihre Anwendungen dieselbe Client-ID oder Anwendungs-ID verwenden.
* Stellen Sie sicher, dass all Ihre Anwendungen über denselben SharedUserID-Satz verfügen.
* Stellen Sie sicher, dass all Ihre Anwendungen dasselbe Signaturzertifikat aus dem Google Play Store verwenden, sodass Sie den Speicher gemeinsam verwenden können.

#### Schritt 1: Verwenden derselben Client-ID/Anwendungs-ID für alle Anwendungen in Ihrer App-Suite

Damit die Microsoft Identity-Plattform Kenntnis davon hat, dass Token über Ihre Anwendungen hinweg verwendet werden können, muss jede Ihrer Anwendungen dieselbe Client-ID oder Anwendungs-ID verwenden. Hierbei handelt es sich um den eindeutigen Bezeichner, den Sie bei der Registrierung Ihrer ersten Anwendung im Portal erhalten haben.

Nun fragen Sie sich vielleicht, wie unterschiedliche Apps gegenüber dem Microsoft Identity-Dienst identifiziert werden, wenn sie die gleiche Anwendungs-ID verwenden. Die Antwort lautet: mithilfe von **Umleitungs-URIs**. Jede Anwendung kann mehrere Umleitungs-URIs im Integrationsportal registrieren. Jede App innerhalb Ihrer Suite kann einen anderen Umleitungs-URI verwenden. Nachfolgend sehen Sie ein Beispiel hierfür:

Umleitungs-URI für App1: `msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D`

Umleitungs-URI für App2: `msauth://com.example.userapp1/KmB7PxIytyLkbGHuI%2UitkW%2Fejk%4E`

Umleitungs-URI für App3: `msauth://com.example.userapp2/Pt85PxIyvbLkbKUtBI%2SitkW%2Fejk%9F`

...

Diese URIs werden unterhalb der gleichen Client-ID/Anwendungs-ID geschachtelt und auf der Grundlage des Umleitungs-URIs ermittelt, den Sie in Ihrer SDK-Konfiguration an uns zurückgeben.

```
+-------------------+
|                   |
|  Client ID        |
+---------+---------+
          |
          |           +-----------------------------------+
          |           |  App 1 Redirect URI               |
          +----------^+                                   |
          |           +-----------------------------------+
          |
          |           +-----------------------------------+
          +----------^+  App 2 Redirect URI               |
          |           |                                   |
          |           +-----------------------------------+
          |
          +----------^+-----------------------------------+
                      |  App 3 Redirect URI               |
                      |                                   |
                      +-----------------------------------+

```


*Das Format dieser Umleitungs-URIs wird nachstehend erläutert. Grundsätzlich können Sie einen beliebigen Umleitungs-URI verwenden. Wenn Sie aber den Broker unterstützen möchten, muss ein ähnliches wie das obige Format gewählt werden.*


#### Schritt 2: Konfigurieren des gemeinsam genutzten Speichers auf Android

Das Festlegen von `SharedUserID` geht über den Rahmen dieses Artikels hinaus, entsprechende Informationen finden Sie aber in der Google Android-Dokumentation zum [Manifest](http://developer.android.com/guide/topics/manifest/manifest-element.html). Wichtig ist, dass Sie entscheiden, wie die sharedUserID lauten soll. Diese Informationen müssen Sie über all Ihre Anwendungen hinweg verwenden.

Sobald `SharedUserID` in all Ihren Anwendungen vorhanden ist, sind Sie bereit für die Verwendung von SSO.

> [AZURE.WARNING] 
Wenn Sie einen Speicher über Ihre Anwendungen hinweg gemeinsam nutzen, kann jede Anwendung Benutzer löschen oder – noch schlimmer – anwendungsübergreifend alle Token löschen. Dies ist insbesondere dann katastrophal, wenn Sie über Anwendungen verfügen, die zur Ausführung von Hintergrundaufgaben auf die Token zurückgreifen. Seien Sie deshalb bei der Verwendung eines gemeinsam genutzten Speichers bei allen Entfernungsvorgängen über die Microsoft Identity SDKs besonders vorsichtig.

Das ist alles! Das Microsoft Identity SDK verwendet jetzt Anmeldeinformationen über all Ihre Anwendungen hinweg. Die Benutzerliste wird ebenfalls über alle Anwendungsinstanzen hinweg verwendet.

### Aktivieren von SSO für brokergestütztes SSO

Die Fähigkeit einer Anwendung, jeden der auf dem Gerät installierten Broker zu verwenden, ist **standardmäßig deaktiviert**. Damit Ihre Anwendung mit dem Broker verwendet werden kann, müssen Sie einige zusätzliche Konfigurationsschritte ausführen und Ihrer Anwendung Code hinzufügen.

Folgende Schritte sind erforderlich:

1. Aktivieren des Brokermodus im Anwendungscode für den MS SDK-Aufruf
2. Festlegen eines neuen Umleitungs-URI und Bereitstellen dieses URI in Ihrer App und App-Registrierung
3. Einrichten der korrekten Berechtigungen im Android-Manifest


#### Schritt 1: Aktivieren des Brokermodus in Ihrer Anwendung
Die Fähigkeit Ihrer Anwendung zur Verwendung des Brokers wird aktiviert, wenn Sie die Einstellungen oder die anfängliche Einrichtung Ihrer Authentifizierungsinstanz erstellen. Erreicht wird dies durch Festlegen des ApplicationSettings-Typs in Ihrem Code:

```
AuthenticationSettings.Instance.setUseBroker(true);
```


#### Schritt 2: Festlegen eines neuen Umleitungs-URI mit Ihrem URL-Schema

Um sicherzustellen, dass die Anmeldeinformationstoken immer an die richtige Anwendung zurückgegeben werden, muss sichergestellt werden, dass der Rückruf an Ihre Anwendung in einer Weise erfolgt, die das Android-Betriebssystem überprüfen kann. Das Android-Betriebssystem verwendet den Hash des Zertifikats im Google Play Store. Dieser kann nicht durch eine nicht autorisierte Anwendung ausgespäht werden. Deshalb nutzen wir den Hash gemeinsam mit dem URI unserer Brokeranwendung, um sicherzustellen, dass die Token an die richtige Anwendung zurückgegeben werden. Es ist erforderlich, dass Sie diesen eindeutigen Umleitungs-URI sowohl in Ihrer Anwendung als auch in unserem Entwicklerportal als Umleitungs-URI festlegen.

Der Umleitungs-URI muss das folgende Format besitzen:

`msauth://packagename/Base64UrlencodedSignature`

Beispiel: *msauth://com.example.userapp/IcB5PxIyvbLkbFVtBI%2FitkW%2Fejk%3D*

Dieser Umleitungs-URI muss in Ihrer App-Registrierung über das [klassische Azure-Portal](https://manage.windowsazure.com/) angegeben werden. Weitere Informationen zur Azure AD-App-Registrierung finden Sie unter [Integration in Azure Active Directory](active-directory-how-to-integrate.md).


#### Schritt 3: Einrichten der korrekten Berechtigungen in Ihrer Anwendung

Unsere Brokeranwendung in Android verwendet das Feature zur Kontoverwaltung des Android-Betriebssystems, um Anmeldeinformationen anwendungsübergreifend zu verwalten. Damit der Broker in Android verwendet werden kann, muss Ihr App-Manifest über Berechtigungen zum Verwenden von AccountManager-Konten verfügen. Dies wird in der [Google-Dokumentation zu Account Manager](http://developer.android.com/reference/android/accounts/AccountManager.html) ausführlich erläutert.

Insbesondere sind dies folgende Berechtigungen:

```
GET_ACCOUNTS
USE_CREDENTIALS
MANAGE_ACCOUNTS
```

### Sie haben SSO konfiguriert!

Jetzt verwendet das Microsoft Identity SDK Anmeldeinformationen automatisch über Ihre Anwendungen hinweg und ruft den Broker auf, wenn dieser auf dem Gerät vorhanden ist.

<!---HONumber=AcomDC_0921_2016-->
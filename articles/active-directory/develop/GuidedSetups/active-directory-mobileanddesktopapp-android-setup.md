---
title: "Azure AD v2 – Android – Erste Schritte – Einrichtung | Microsoft-Dokumentation"
description: Informationen, wie eine Android-App ein Zugriffstoken abrufen und die Microsoft Graph-API oder APIs aufrufen kann, die Zugriffstoken vom Azure Active Directory-v2-Endpunkt erfordern
services: active-directory
documentationcenter: dev-center-name
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 820acdb7-d316-4c3b-8de9-79df48ba3b06
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 05/09/2017
ms.author: andret
ms.custom: aaddev
ms.translationtype: Human Translation
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.openlocfilehash: a43d7e30a6f4176afba27f0de2c2c116df741080
ms.contentlocale: de-de


---

## <a name="set-up-your-project"></a>Einrichten des Projekts

> Möchten Sie stattdessen das Android Studio-Projekt dieses Beispiels herunterladen? [Laden Sie ein Projekt herunter](https://github.com/Azure-Samples/active-directory-android-native-v2/archive/master.zip), und fahren Sie mit dem Schritt [Konfiguration](#create-an-application-express) fort, um das Codebeispiel vor der Ausführung zu konfigurieren.


### <a name="create-a-new-project"></a>Erstellen eines neuen Projekts 
1.  Öffnen Sie Android Studio, und wechseln Sie zu: `File` > `New` > `New Project`
2.  Geben Sie der Anwendung einen Namen, und klicken Sie auf `Next`.
3.  Stellen Sie sicher, dass Sie die *API 21 oder höher (Android 5.0)* auswählen, und klicken Sie auf `Next`.
4.  Belassen Sie `Empty Activity`, klicken Sie auf `Next` und dann auf `Finish`.


### <a name="add-the-microsoft-authentication-library-msal-to-your-project"></a>Hinzufügen der MSAL (Microsoft Authentication Library) zu Ihrem Projekt
1.  Wechseln Sie in Android Studio zu: `Gradle Scripts` > `build.gradle (Module: app)`
2.  Kopieren Sie den folgenden Code, und fügen Sie ihn unter `Dependencies` ein:

```ruby  
compile ('com.microsoft.identity.client:msal:0.1.+') {
    exclude group: 'com.android.support', module: 'appcompat-v7'
}
compile 'com.android.volley:volley:1.0.0'
```

<!--start-collapse-->
### <a name="about-this-package"></a>Informationen zu diesem Paket

Das obige Paket installiert die Microsoft Authentication Library (MSAL). Die MSAL kümmert sich um das Erfassen, Zwischenspeichern und Aktualisieren von Benutzertoken für den Zugriff auf APIs, die vom Azure Active Directory v2-Endpunkt geschützt werden.
<!--end-collapse-->

## <a name="create-your-applications-ui"></a>Erstellen der Benutzeroberfläche Ihrer Anwendung

1.  Öffnen Sie `activity_main.xml` unter `res` > `layout`.
2.  Ändern Sie das Layout der Aktivität von `android.support.constraint.ConstraintLayout` o.ä. in `LinearLayout`.
3.  Fügen Sie die Eigenschaft `android:orientation="vertical"` dem Knoten `LinearLayout` hinzu.
4.  Kopieren Sie den folgenden Code, und fügen Sie ihn in den Knoten `LinearLayout` ein, wobei Sie den aktuellen Inhalt ersetzen:

```xml
<TextView
    android:text="Welcome, "
    android:textColor="#3f3f3f"
    android:textSize="50px"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginLeft="10dp"
    android:layout_marginTop="15dp"
    android:id="@+id/welcome"
    android:visibility="invisible"/>

<Button
    android:id="@+id/callGraph"
    android:text="Call Microsoft Graph"
    android:textColor="#FFFFFF"
    android:background="#00a1f1"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginTop="200dp"
    android:textAllCaps="false" />

<TextView
    android:text="Getting Graph Data..."
    android:textColor="#3f3f3f"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_marginLeft="5dp"
    android:id="@+id/graphData"
    android:visibility="invisible"/>

<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="0dip"
    android:layout_weight="1"
    android:gravity="center|bottom"
    android:orientation="vertical" >

    <Button
        android:text="Sign Out"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="15dp"
        android:textColor="#FFFFFF"
        android:background="#00a1f1"
        android:textAllCaps="false"
        android:id="@+id/clearCache"
        android:visibility="invisible" />
</LinearLayout>
```



Aufgrund ständiger Weiterentwicklung stimmt die Version des in Android Studio installierten Android-SDK unter Umständen nicht mit der Version im Code überein. In diesem Tutorial wird von Version 23 des Android-SDK ausgegangen. Das war die neueste Version zum Zeitpunkt des Verfassens. Mit neuen Ausgaben des SDK kann die Versionsnummer größer werden. Es empfiehlt sich, die neueste verfügbare Version zu verwenden.

Zwei Symptome nicht übereinstimmender Versionen sind:

- Wenn Sie das Projekt erstellen oder erneut erstellen, erhalten Sie möglicherweise eine Gradle-Fehlermeldung mit einem Hinweis wie **Ziel nicht gefunden: Google Inc.:Google APIs:n**.
- Standard-Android-Objekte im Code, die auf Grundlage von `import` -Ausdrücken aufgelöst werden sollten, können Fehlermeldungen verursachen.

Wenn eines dieser Symptome auftritt, stimmt die Version des in Android Studio installierten Android SDK möglicherweise nicht mit dem SDK-Ziel des heruntergeladenen Projekts überein. Zur Überprüfung der Version nehmen Sie die folgenden Änderungen vor:

1. Klicken Sie in Android Studio auf **Tools** > **Android** > **SDK Manager**. Wenn nicht die neueste Version der SDK-Plattform installiert ist, so klicken Sie zur Installation. Notieren Sie sich die Versionsnummer.
2. Öffnen Sie auf der Registerkarte **Projektexplorer** unter **Gradle Scripts** (Gradle-Skripts) die Datei **build.gradle (module: app)**. Vergewissern Sie sich, dass für **compileSdkVersion** und **buildToolsVersion** die neueste installierte SDK-Version festgelegt ist. Die Tags könnten folgendermaßen aussehen:

             compileSdkVersion 'Google Inc.:Google APIs:23'
            buildToolsVersion "23.0.2"
3. Klicken Sie im Android Studio-Projektexplorer mit der rechten Maustaste auf den Projektknoten, und wählen Sie **Properties** (Eigenschaften) und dann in der linken Spalte **Android** aus. Stellen Sie sicher, dass für **Project Build Target** (Projektbuildziel) dieselbe SDK-Version wie für **targetSdkVersion** angegeben ist.

In Android Studio wird anders als bei Eclipse die Manifestdatei nicht mehr dazu verwendet, die Ziel-SDK und die SDK-Mindestversion anzugeben.

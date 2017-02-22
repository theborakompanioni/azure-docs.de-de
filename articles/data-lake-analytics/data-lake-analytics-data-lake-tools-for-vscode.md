---
title: "Verwenden der Azure Data Lake-Tools für Visual Studio Code | Microsoft Docs"
description: "Hier erfahren Sie, wie Sie die Azure Data Lake-Tools für Visual Studio Code verwenden können, um U-SQL-Skripts zu erstellen, zu testen und auszuführen. "
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/30/2016
ms.author: jejiang
translationtype: Human Translation
ms.sourcegitcommit: e79513590bb37570764f398e716182a11c74612a
ms.openlocfilehash: 59cc35bc740625ed0582c1557fac9a04bf0cb8bc

---

# <a name="use-the-azure-data-lake-tools-for-visual-studio-code"></a>Verwenden der Azure Data Lake-Tools für Visual Studio Code

Hier erfahren Sie, wie Sie die Azure Data Lake-Tools für Visual Studio Code (VSCode) verwenden können, um U-SQL-Skripts zu erstellen, zu testen und auszuführen.  Diese Informationen sind auch im folgenden Video enthalten:

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Voraussetzungen

Die Data Lake-Tools können auf den von VSCode unterstützten Plattformen installiert werden, z.B. Windows, Linux und macOS. Im Folgenden finden Sie die Voraussetzungen für die unterschiedlichen Plattformen.

- Windows

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).
    - [Java SE Runtime Environment, Version 8, Update 77 oder höher](https://java.com/download/manual.jsp). Sie müssen den Pfad der „java.exe“ der Systemumgebungsvariablen PATH hinzufügen.  Anleitungen hierzu finden Sie unter [How do I set or change the Path system variable?]( https://www.java.com/download/help/path.xml) (Festlegen oder Ändern der Systemvariablen PATH). Der Pfad sieht in etwa wie folgt aus: „C:\Programme\Microsoft Files\Java\jdk1.8.0_77\jre\bin“
    - [.NET Core SDK 1.0.1-Preview 2 oder .NET Core 1.0.1 Runtime]( https://www.microsoft.com/net/download).
    
- Linux (Ubuntu 14.04 LTS wird empfohlen)

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx). Geben Sie zur Installation den folgenden Befehl ein:

        sudo dpkg -i code_<Versionsnummer>_amd64.deb

    - [Mono 4.2.x](http://www.mono-project.com/docs/getting-started/install/linux/). 

        - Aktualisieren Sie die Deb-Paketquelle, indem Sie folgende Befehle ausführen:

                sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
                echo "deb http://download.mono-project.com/repo/debian wheezy/snapshots 4.2.4.4/main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
                sudo apt-get update

        - Installieren Sie Mono, indem Sie den folgenden Befehl ausführen:

                sudo apt-get install mono-complete

            > [!NOTE] 
            > Mono 4.6 wird nicht unterstützt.  Sie müssen Version 4.6 vor der Installation von 4.2.x vollständig deinstallieren.  

        - [Java SE Runtime Environment, Version 8, Update 77 oder höher](https://java.com/download/manual.jsp). Eine Anleitung finden Sie [hier]( https://java.com/en/download/help/linux_x64_install.xml).
        - [.NET Core SDK 1.0.1-Preview 2 oder .NET Core 1.0.1 Runtime]( https://www.microsoft.com/net/download).
- macOS

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).
    - [Mono 4.2.4](http://download.mono-project.com/archive/4.2.4/macos-10-x86/). 
    - [Java SE Runtime Environment, Version 8, Update 77 oder höher](https://java.com/download/manual.jsp). Eine Anleitung finden Sie [hier](https://java.com/en/download/help/mac_install.xml).
    - [.NET Core SDK 1.0.1-Preview 2 oder .NET Core 1.0.1 Runtime]( https://www.microsoft.com/net/download).

## <a name="install-the-data-lake-tools"></a>Installieren der Data Lake-Tools

Nachdem Sie die erforderlichen Komponenten installiert haben, können Sie die Data Lake-Tools für VSCode installieren.

**So installieren Sie die Data Lake-Tools**

1. Öffnen Sie **Visual Studio Code**.
2. Drücken Sie **STRG+P**, und geben Sie dann Folgendes ein:

        ext install usql-vscode-ext
    Eine Liste der Erweiterungen für Visual Studio Code wird angezeigt. Eine davon ist **Azure Data Lake Tool (Preview)**.
3. Klicken Sie neben **Azure Data Lake Tool (Preview)** auf **Installieren**. Nach wenigen Sekunden wird die Schaltfläche „Installieren“ in „Neu laden“ geändert.
4. Klicken Sie auf **Neu laden**, um die Erweiterung zu aktivieren.
5. Klicken Sie auf **OK** , um zu bestätigen. Die Azure Data Lake-Tools werden im Bereich „Erweiterungen“ angezeigt.

    ![Installation der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)


## <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure

Bevor Sie U-SQL-Skripts kompilieren und ausführen können, müssen Sie eine Verbindung mit Ihrem Azure-Konto herstellen.

**So stellen Sie eine Verbindung mit Azure her**

1.  Öffnen Sie die Befehlspalette, indem Sie **STRG+UMSCHALT+P** drücken. 
2.  Geben Sie **ADL:Anmelden** ein.

    ![Befehlspalette der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)

2.  Befolgen Sie die Anweisungen, um sich über die Webseite anzumelden. Nachdem die Verbindung hergestellt wurde, wird Ihr Kontoname auf der Statusleiste unten im Fenster angezeigt.

> [!NOTE] 
> Wenn für Ihr Konto die zweistufige Authentifizierung aktiviert ist, sollten Sie die Authentifizierung per Telefon statt per PIN verwenden.

Um sich abzumelden, verwenden Sie den Befehl **ADL:Abmelden**.

## <a name="list-data-lake-analytics-accounts"></a>Auflisten der Data Lake Analytics-Konten

Um die Verbindung zu testen, können Sie Ihre Data Lake Analytics-Konten auflisten:

**So listen Sie die Data Lake Analytics-Konten in Ihrem Azure-Abonnement auf**

1. Öffnen Sie die Befehlspalette, indem Sie **STRG+UMSCHALT+P** drücken.
2. Geben Sie **ADL:Konten auflisten** ein.  Die Konten werden im Bereich **Ausgabe** angezeigt.

## <a name="work-with-u-sql"></a>Arbeiten mit U-SQL

Sie müssen eine U-SQL-Datei oder einen entsprechenden Ordner öffnen, um mit U-SQL zu arbeiten.

**So öffnen Sie einen Ordner für das U-SQL-Projekt**

1. Klicken Sie in Visual Studio Code auf das Menü **Datei** und dann auf **Ordner öffnen**.
2. Geben Sie einen Ordner an, und klicken Sie dann auf **Ordner auswählen**.
3. Klicken Sie auf das Menü **Datei** und dann auf **Neu**. Eine Datei mit dem Namen **Unbenannt-1** wird dem Projekt hinzugefügt.
4. Kopieren Sie den folgenden Code in die Datei „Unbenannt-1“:

        @departments  = 
            SELECT * FROM 
                (VALUES
                    (31,    "Sales"),
                    (33,    "Engineering"), 
                    (34,    "Clerical"),
                    (35,    "Marketing")
                ) AS 
                      D( DepID, DepName );
         
        OUTPUT @departments
            TO “/Output/departments.csv”

    Das Skript erstellt eine Datei mit dem Namen „departments.csv“ mit einigen Daten im Ordner „/Output“.

5. Speichern Sie die Datei als **myUSQL.usql** im geöffneten Ordner. Beachten Sie, dass eine Konfigurationsdatei **adltools_settings.json** ebenfalls dem Projekt hinzugefügt wird.
4. Öffnen und konfigurieren Sie **adltools_settings.json** mit den folgenden Eigenschaften:

    - Konto: Ein Data Lake Analytics-Konto in Ihrem Azure-Abonnement.
    - Optionale Einstellungen:

        - Priorität: Der Prioritätsbereich liegt zwischen 1 und 1000, wobei 1 die höchste Priorität ist. Der Standardwert ist 1000.
        - Parallelität: Der Parallelitätsbereich liegt zwischen 1 und 150. Der Standardwert ist 150. 

        > [!NOTE] 
        > Wenn die Einstellungen ungültig sind, werden die Standardwerte verwendet.

     ![Konfigurationsdatei der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

    Ein Data Lake Analytics-Computerkonto ist erforderlich, um U-SQL-Aufträge zu kompilieren und auszuführen.  Sie müssen das Computerkonto konfigurieren, bevor Sie U-SQL-Aufträge kompilieren und ausführen können.

Im Vergleich mit dem Öffnen einer Datei haben Sie durch das Öffnen eines Ordners folgende Möglichkeiten:

- Verwenden einer CodeBehind-Datei.  Bei Verwendung einer einzelnen Datei wird CodeBehind nicht unterstützt.
- Verwenden einer Konfigurationsdatei. Wenn Sie einen Ordner öffnen, verwenden die Skripts im Arbeitsordner die gleiche Konfigurationsdatei.


Die U-SQL-Skriptkompilierung wird remote vom Data Lake Analytics-Dienst ausgeführt.  Wenn Sie den Kompilierungsbefehl ausgeben, wird das U-SQL-Skript an Ihr Data Lake Analytics-Konto gesendet. Das Kompilierungsergebnis wird später von Visual Studio Code empfangen. Aufgrund der Remotekompilierung benötigt Visual Studio Code die Informationen für die Verbindung mit Ihrem Data Lake Analytics-Konto in der Konfigurationsdatei.

**So kompilieren Sie ein U-SQL-Skript**

1. Öffnen Sie die Befehlspalette, indem Sie **STRG+UMSCHALT+P** drücken. 
2. Geben Sie **ADL: Skript kompilieren** ein. Die Kompilierungsergebnisse werden im Ausgabefenster angezeigt. Sie können auch mit der rechten Maustaste auf eine Skriptdatei klicken und dann auf **ADL: Skript kompilieren** klicken, um einen U-SQL-Auftrag zu kompilieren. Das Kompilierungsergebnis wird im Ausgabebereich angezeigt.
 

**So übermitteln Sie ein U-SQL-Skript**

1. Öffnen Sie die Befehlspalette, indem Sie **STRG+UMSCHALT+P** drücken. 
2. Geben Sie **ADL: Auftrag übermitteln** ein.  Sie können auch mit der rechten Maustaste auf eine Skriptdatei klicken und dann auf **ADL: Auftrag übermitteln** klicken, um einen U-SQL-Auftrag zu übermitteln. 

Nach der Übermittlung eines U-SQL-Auftrags werden im Ausgabefenster in VSCode Übermittlungsprotokolle angezeigt. Wenn die Übermittlung erfolgreich war, wird auch die Auftrags-URL angezeigt. Sie können die Auftrags-URL in einem Webbrowser öffnen, um den Auftragsstatus in Echtzeit nachzuverfolgen.

Legen Sie zum Aktivieren der Ausgabe von Auftragsdetails „jobInformationOutputPath“ in der Datei **vscode for u-sql_settings.json** fest.
 
## <a name="use-code-behind-file"></a>Verwenden einer CodeBehind-Datei

Eine CodeBehind-Datei ist eine CSharp-Datei, die einem U-SQL-Skript zugeordnet ist. Sie können ein Skript speziell für UDO/UDA/UDT/UDF in der CodeBehind-Datei definieren. UDO/UDA/UDT/UDF kann direkt im Skript verwendet werden, ohne die Assembly zuerst zu registrieren. Die CodeBehind-Datei wird im gleichen Ordner wie die Peering-U-SQL-Skriptdatei gespeichert. Wenn das Skript den Namen „xxx.usql“ erhält, heißt die CodeBehind-Datei „xxx.usql.cs“. Durch manuelles Löschen der CodeBehind-Datei wird die CodeBehind-Funktion für das zugehörige U-SQL-Skript deaktiviert. Weitere Informationen über das Schreiben von Kundencode für ein U-SQL-Skript finden Sie unter [Writing and Using Custom Code in U-SQL – User-Defined Functions]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/) (Schreiben und Verwenden von benutzerdefiniertem Code in U-SQL – benutzerdefinierte Funktionen).

Zur Unterstützung von CodeBehind muss ein Arbeitsordner geöffnet werden. 

**So generieren Sie eine CodeBehind-Datei**

1. Öffnen Sie eine Quelldatei. 
2. Öffnen Sie die Befehlspalette, indem Sie **STRG+UMSCHALT+P** drücken.
3. Geben Sie **ADL: CodeBehind generieren** ein.  Eine CodeBehind-Datei wird im gleichen Ordner erstellt. 

Sie können auch mit der rechten Maustaste auf eine Skriptdatei klicken und dann auf **ADL: CodeBehind generieren** klicken, um eine CodeBehind-Datei zu generieren. 

Das Kompilieren und Übermitteln eines U-SQL-Skripts mit CodeBehind erfolgt wie bei einem eigenständigen U-SQL-Skript.

Die beiden folgenden Screenshots zeigen eine CodeBehind-Datei und die zugehörige U-SQL-Skriptdatei:
 
![CodeBehind der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![CodeBehind der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

## <a name="use-assemblies"></a>Verwenden von Assemblys

Informationen zum Entwickeln von Assemblys finden Sie unter [Entwickeln von U-SQL-Assemblys für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-assemblies.md).

Mit den Data Lake-Tools können Sie benutzerdefinierte Codeassemblys im Data Lake Analytics-Katalog registrieren.

**So registrieren Sie eine Assembly**

1.  Drücken Sie **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen.
2.  Geben Sie **ADL:Assembly registrieren** ein.
3.  Wählen Sie ein Data Lake Analytics-Konto aus.
4.  Wählen Sie eine Datenbank aus.
5.  Geben Sie den lokalen Assemblypfad an.

Der folgende U-SQL-Code veranschaulicht, wie eine Assembly aufgerufen wird. In diesem Beispiel lautet der Name der Assembly *test*.

    REFERENCE ASSEMBLY [test];
    @a=EXTRACT Iid int,Starts DateTime,Region string,Query string,DwellTime int,Results string,ClickedUrls string 
    FROM @"ruoxin/SearchLog.txt" USING Extractors.Tsv();
    
    @d=SELECT DISTINCT Region FROM @a;
    
    @d1=PROCESS @d
        PRODUCE Region string,
                Mkt string
                USING new USQLApplication_codebehind.MyProcessor();
    
    OUTPUT @d1 TO @"ruoxin/SearchLogtest.txt" USING Outputters.Tsv();



## <a name="access-data-lake-analytics-catalog"></a>Zugreifen auf den Data Lake Analytics-Katalog

Nachdem Sie die Verbindung mit Azure hergestellt haben, können Sie mit den folgenden Schritten auf den U-SQL-Katalog zugreifen:

**So greifen Sie auf die U-SQL-Metadaten zu**

1.  Drücken Sie **STRG+UMSCHALT+P**, und geben Sie dann **ADL:Tabellen auflisten** ein.
2.  Klicken Sie auf eines der Data Lake Analytics-Konten.
3.  Klicken Sie auf eine der Data Lake Analytics-Datenbanken.
4.  Klicken Sie auf eines der Schemas. Die Tabellen werden angezeigt.

## <a name="additional-features"></a>Zusätzliche Funktionen

Die Data Lake-Tools für VSCode unterstützen die folgenden Funktionen:

-   IntelliSense-AutoVervollständigen. Vorschläge werden für Schlüsselwörter, Methoden, Variablen usw. eingeblendet. Unterschiedliche Symbole stellen die verschiedene Objekttypen dar:

    - Scala-Datentyp
    - Komplexer Datentyp
    - Integrierte UDTs
    - .NET-Sammlung und -Klassen
    - C#-Ausdrücke
    - Integrierte C#-UDFs, -UDOs und -UDAAGs 
    - U-SQL-Funktionen
    - U-SQL-Windowingfunktionen
 
    ![IntelliSense-Objekttypen der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   IntelliSense-AutoVervollständigen für Data Lake Analytics-Metadaten. Die Data Lake-Tools laden Data Lake Analytics-Metadateninformationen lokal herunter.  Die IntelliSense-Funktion füllt automatisch die Objekte anhand der Data Lake Analytics-Metadaten aus, einschließlich Datenbank, Schema, Tabelle, Sicht, TVF, Prozeduren und C#-Assemblys.
 
    ![IntelliSense-Metadaten der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   IntelliSense-Fehlermarker. Die Data Lake-Tools unterstreichen die Bearbeitungsfehler für U-SQL und C#. 
-   Syntaxmarkierungen. Die Data Lake-Tools verwenden verschiedene Farben um Variablen, Schlüsselwörter, Datentypen, Funktionen usw. zu unterscheiden. 

    ![Syntaxmarkierungen der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

##<a name="next-steps"></a>Nächste Schritte:

- Informationen zu den ersten Schritten mit Data Lake Analytics finden Sie unter [Tutorial: Erste Schritte mit Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Weitere Informationen zur Verwendung von Data Lake-Tools für Visual Studio finden Sie unter [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Informationen zum Entwickeln von Assemblys finden Sie unter [Entwickeln von U-SQL-Assemblys für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-assemblies.md).






<!--HONumber=Dec16_HO1-->



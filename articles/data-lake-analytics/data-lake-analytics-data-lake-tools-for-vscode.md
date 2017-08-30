---
title: "Azure Data Lake Tools: Verwenden von Azure Data Lake Tools für Visual Studio Code | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Azure Data Lake Tools für Visual Studio Code verwenden, um U-SQL-Skripts zu erstellen, zu testen und auszuführen. "
Keywords: "VSCode, Azure Data Lake Tools, lokales Ausführen, lokales Debuggen, lokales Debugging, Vorschau Speicherdatei, Hochladen in Speicherpfad"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 833d14af47454a01fa3c97ffa854d688dd35871f
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---

# <a name="use-azure-data-lake-tools-for-visual-studio-code"></a>Verwenden von Azure Data Lake Tools für Visual Studio Code

Erfahren Sie, wie Sie Azure Data Lake Tools für Visual Studio Code (VS Code) verwenden, um U-SQL-Skripts zu erstellen, zu testen und auszuführen. Diese Informationen sind auch im folgenden Video enthalten:

<a href="https://www.youtube.com/watch?v=J_gWuyFnaGA&feature=youtu.be"><img src="./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-video.png"></a>

## <a name="prerequisites"></a>Voraussetzungen

Data Lake Tools können auf den von VS Code unterstützten Plattformen installiert werden. Zu den unterstützten Plattformen gehören Windows, Linux und macOS. Für die verschiedenen Plattformen gelten die folgenden Voraussetzungen:

- Windows

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).
    - [Java SE Runtime Environment, Version 8, Update 77 oder höher](https://java.com/download/manual.jsp). Fügen Sie den Pfad der Datei „java.exe“ der Systemumgebungsvariablen PATH hinzu. Anleitungen zur Konfiguration finden Sie unter [How do I set or change the Path system variable?]( https://www.java.com/download/help/path.xml) (Festlegen oder Ändern der Systemvariablen PATH). Der Pfad sieht in etwa wie folgt aus: „C:\Programme\Java\jdk1.8.0_77\jre\bin“.
    - [.NET Core SDK 1.0.3 oder .NET Core 1.1 Runtime](https://www.microsoft.com/net/download).
    
- Linux (Ubuntu 14.04 LTS wird empfohlen)

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx). Geben Sie den folgenden Befehl für die Installation von Code ein:

              sudo dpkg -i code_<version_number>_amd64.deb

    - [Mono 4.2.x](http://www.mono-project.com/docs/getting-started/install/linux/). 

        - Geben Sie zum Aktualisieren der deb-Paketquelle folgende Befehle ein:

                sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv-keys 3FA7E0328081BFF6A14DA29AA6A19B38D3D831EF
                echo "deb http://download.mono-project.com/repo/debian wheezy/snapshots 4.2.4.4/main" | sudo tee /etc/apt/sources.list.d/mono-xamarin.list
                sudo apt-get update

        - Geben Sie den folgenden Befehl für die Installation von Mono ein:

                sudo apt-get install mono-complete

            > [!NOTE] 
            > Mono 4.6 wird nicht unterstützt. Deinstallieren Sie Version 4.6 vor der Installation von 4.2.x vollständig.  

        - [Java SE Runtime Environment, Version 8, Update 77 oder höher](https://java.com/download/manual.jsp). Anweisungen zur Installation finden Sie auf der Seite [Linux 64-bit installation instructions for Java]( https://java.com/en/download/help/linux_x64_install.xml) (Anweisungen zur Installation von Java unter 64-Bit-Versionen von Linux).
        - [.NET Core SDK 1.0.3 oder .NET Core 1.1 Runtime](https://www.microsoft.com/net/download).
- macOS

    - [Visual Studio Code]( https://www.visualstudio.com/products/code-vs.aspx).
    - [Mono 4.2.4](http://download.mono-project.com/archive/4.2.4/macos-10-x86/). 
    - [Java SE Runtime Environment, Version 8, Update 77 oder höher](https://java.com/download/manual.jsp). Anweisungen zur Installation finden Sie auf der Seite [Linux 64-bit installation instructions for Java](https://java.com/en/download/help/mac_install.xml) (Anweisungen zur Installation von Java unter 64-Bit-Versionen von Linux).
    - [.NET Core SDK 1.0.3 oder .NET Core 1.1 Runtime](https://www.microsoft.com/net/download).

## <a name="install-data-lake-tools"></a>Installieren von Data Lake Tools

Nachdem Sie die erforderlichen Komponenten installiert haben, können Sie Data Lake Tools für VS Code installieren.

**So installieren Sie Data Lake Tools**

1. Öffnen Sie Visual Studio Code.
2. Drücken Sie STRG+P, und geben Sie dann den folgenden Befehl ein:
```
ext install usql-vscode-ext
```
Eine Liste der Erweiterungen für Visual Studio Code wird angezeigt. Eine davon ist **Azure Data Lake Tools**.

3. Wählen Sie neben **Azure Data Lake Tools** die Option **Installieren** aus. Nach wenigen Sekunden wird die Schaltfläche **Installieren** in **Neu laden** geändert.
4. Wählen Sie **Neu laden** aus, um die Erweiterung zu aktivieren.
5. Wählen Sie zum Fortzufahren **OK** aus. Azure Data Lake Tools wird im Bereich **Erweiterungen** angezeigt.
    ![Bereich „Erweiterungen“ für Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extensions.png)

## <a name="activate-azure-data-lake-tools"></a>Aktivieren der Azure Data Lake Tools
Erstellen Sie eine neue USQL-Datei, oder öffnen Sie eine vorhandene USQL-Datei, um die Erweiterung zu aktivieren. 

## <a name="connect-to-azure"></a>Herstellen einer Verbindung mit Azure

Damit Sie U-SQL-Skripts in Data Lake Analytics kompilieren und ausführen können, müssen Sie eine Verbindung mit Ihrem Azure-Konto herstellen.

**So stellen Sie eine Verbindung mit Azure her**

1.  Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen. 
2.  Geben Sie **ADL: Login** ein. Die Anmeldeinformationen werden im Bereich **Ausgabe** angezeigt.

    ![Befehlspalette von Data Lake Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login.png)
    ![Informationen zur Geräteanmeldung bei Data Lake Tools für Visual Studio](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-login-info.png)
3. Klicken Sie bei gedrückter STRG-Taste auf die Anmelde-URL https://aka.ms/devicelogin, um die Anmeldewebseite zu öffnen. Geben Sie den Code **G567LX42V** in das Textfeld ein, und wählen Sie dann **Fortfahren** aus.

   ![Einfügen des Codes zum Anmelden bei den Data Lake Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-extension-login-paste-code.png )   
4.  Befolgen Sie die Anweisungen, um sich über die Webseite anzumelden. Wenn eine Verbindung hergestellt wurde, wird der Name Ihres Azure-Kontos auf der Statusleiste in der linken unteren Ecke des Fensters **Visual Studio Code** angezeigt. 

    > [!NOTE] 
    > Wenn für Ihr Konto zwei Faktoren aktiviert wurden, empfehlen wir anstelle einer PIN die Verwendung der Telefonauthentifizierung.

Zum Abmelden geben Sie den Befehl **ADL: Logout** ein.

## <a name="list-your-data-lake-analytics-accounts"></a>Auflisten Ihrer Data Lake Analytics-Konten

Um die Verbindung zu testen, können Sie eine Liste Ihrer Data Lake Analytics-Konten abrufen.

**So listen Sie die Data Lake Analytics-Konten in Ihrem Azure-Abonnement auf**

1. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen.
2. Geben Sie **ADL: List Accounts** ein. Die Konten werden im Bereich **Ausgabe** angezeigt.

## <a name="open-the-sample-script"></a>Öffnen des Beispielskripts
Öffnen Sie die Befehlspalette (STRG+UMSCHALT+P), und geben Sie **ADL: Open Sample Script** ein. Dadurch wird eine weitere Instanz dieses Beispiels geöffnet. In dieser Instanz können Sie das Skript auch bearbeiten, konfigurieren und übermitteln.

## <a name="work-with-u-sql"></a>Arbeiten mit U-SQL

Sie müssen eine U-SQL-Datei oder einen entsprechenden Ordner öffnen, um mit U-SQL zu arbeiten.

**So öffnen Sie einen Ordner für das U-SQL-Projekt**

1. Wählen Sie in Visual Studio Code im Menü **Datei** die Option **Ordner öffnen** aus.
2. Geben Sie einen Ordner an, und wählen Sie dann **Ordner auswählen** aus.
3. Wählen Sie Menü **Datei** die Option **Neu** aus. Eine Datei mit dem Namen „Unbenannt-1“ wird dem Projekt hinzugefügt.
4. Geben Sie den folgenden Code in die Datei „Unbenannt-1“ ein:

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

5. Speichern Sie die Datei als **myUSQL.usql** im geöffneten Ordner. Dem Projekt wird außerdem die Konfigurationsdatei „adltools_settings.json“ hinzugefügt.
4. Öffnen Sie „adltools_settings.json“, und konfigurieren Sie die Datei mit den folgenden Eigenschaften:

    - Konto: Ein Data Lake Analytics-Konto in Ihrem Azure-Abonnement.
    - Datenbank: eine Datenbank in Ihrem Konto. Der Standardwert ist **master**.
    - Schema: ein Schema in der Datenbank. Der Standardwert ist **dbo**.
    - Optionale Einstellungen:
        - Priorität: Der Prioritätsbereich liegt zwischen 1 und 1000, wobei 1 die höchste Priorität ist. Der Standardwert ist **1000**.
        - Parallelität: Der Parallelitätsbereich liegt zwischen 1 und 150. Der Standardwert ist die maximal zulässige Parallelität in Ihrem Azure Data Lake Analytics-Konto. 
        
        > [!NOTE] 
        > Wenn die Einstellungen ungültig sind, werden die Standardwerte verwendet.

    ![Konfigurationsdatei der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-configuration-file.png)

    Ein Compute-Konto für Data Lake Analytics ist erforderlich, um U-SQL-Aufträge zu kompilieren und auszuführen. Sie müssen das Computerkonto konfigurieren, bevor Sie U-SQL-Aufträge kompilieren und ausführen können.
    
Nach dem Speichern der Konfiguration werden die Informationen zu Konto, Datenbank und Schema auf der Statusleiste links unten in der entsprechenden USQL-Datei angezeigt. 
 
 
Im Vergleich zum Öffnen einer Datei können Sie durch Öffnen eines Ordners folgende Aktionen durchführen:

- Verwenden einer CodeBehind-Datei: Bei Verwendung einer einzelnen Datei wird CodeBehind nicht unterstützt.
- Hinzufügen einer Konfigurationsdatei: Wenn Sie einen Ordner öffnen, verwenden die Skripts im Arbeitsordner die gleiche Konfigurationsdatei.


Die U-SQL-Skriptkompilierung wird remote vom Data Lake Analytics-Dienst ausgeführt. Wenn Sie den Befehl **compile** übergeben, wird das U-SQL-Skript an Ihr Data Lake Analytics-Konto gesendet. Das Kompilierungsergebnis wird später von Visual Studio Code empfangen. Aufgrund der Remotekompilierung benötigt Visual Studio Code eine Auflistung der Informationen für die Verbindung mit Ihrem Data Lake Analytics-Konto in der Konfigurationsdatei.

**So kompilieren Sie ein U-SQL-Skript**

1. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen. 
2. Geben Sie **ADL: Skript kompilieren** ein. Die Kompilierungsergebnisse werden im Fenster **Ausgabe** angezeigt. Sie können auch mit der rechten Maustaste auf eine Skriptdatei klicken und dann **ADL: Compile Script** auswählen, um einen U-SQL-Auftrag zu kompilieren. Das Kompilierungsergebnis wird im Bereich **Ausgabe** angezeigt.
 

**So übermitteln Sie ein U-SQL-Skript**

1. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen. 
2. Geben Sie **ADL: Auftrag übermitteln** ein.  Sie können auch mit der rechten Maustaste auf eine Skriptdatei klicken und dann **ADL: Submit Job** auswählen. 

Nach dem Übermitteln eines U-SQL-Auftrags werden die Übermittlungsprotokolle in Visual Studio Code im Fenster **Ausgabe** angezeigt. Wenn die Übermittlung erfolgreich war, wird auch die Auftrags-URL angezeigt. Sie können die Auftrags-URL in einem Webbrowser öffnen, um den Auftragsstatus in Echtzeit nachzuverfolgen.

Legen Sie zum Aktivieren der Ausgabe von Auftragsdetails **jobInformationOutputPath** in VS Code in der Datei **u-sql_settings.json** fest.
 
## <a name="use-a-code-behind-file"></a>Verwenden einer CodeBehind-Datei

Eine CodeBehind-Datei ist eine C#-Datei, die einem U-SQL-Skript zugeordnet ist. Sie können ein Skript speziell für UDO, UDA, UDT und UDF in der CodeBehind-Datei definieren. UDO, UDA, UDT und UDF können direkt im Skript verwendet werden, ohne die Assembly zuvor zu registrieren. Die CodeBehind-Datei wird im gleichen Ordner wie die U-SQL-Skriptdatei für das Peering gespeichert. Wenn das Skript den Namen „xxx.usql“ erhält, heißt die CodeBehind-Datei „xxx.usql.cs“. Durch manuelles Löschen der CodeBehind-Datei wird das CodeBehind-Feature für das zugehörige U-SQL-Skript deaktiviert. Weitere Informationen zum Schreiben von Kundencode für ein U-SQL-Skript finden Sie unter [Writing and Using Custom Code in U-SQL: User-Defined Functions]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/) (Schreiben und Verwenden von benutzerdefiniertem Code in U-SQL: benutzerdefinierte Funktionen).

Zur Unterstützung von CodeBehind muss ein Arbeitsordner geöffnet werden. 

**So generieren Sie eine CodeBehind-Datei**

1. Öffnen Sie eine Quelldatei. 
2. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen.
3. Geben Sie **ADL: CodeBehind generieren** ein. Eine CodeBehind-Datei wird im gleichen Ordner erstellt. 

Sie können auch mit der rechten Maustaste auf eine Skriptdatei klicken und dann **ADL: Generate Code Behind** auswählen. 

Das Kompilieren und Übermitteln eines U-SQL-Skripts mit einer CodeBehind-Datei erfolgt wie bei einem eigenständigen U-SQL-Skript.

Die beiden folgenden Screenshots zeigen eine CodeBehind-Datei und die zugehörige U-SQL-Skriptdatei:
 
![CodeBehind für Data Lake Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind.png)

![CodeBehind-Skriptdatei für Data Lake Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-code-behind-call.png) 

## <a name="use-assemblies"></a>Verwenden von Assemblys

Weitere Informationen zum Entwickeln von Assemblys finden Sie unter [Entwickeln von U-SQL-Assemblys für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-assemblies.md).

Sie können mit Data Lake Tools benutzerdefinierte Codeassemblys im Data Lake Analytics-Katalog registrieren.

**So registrieren Sie eine Assembly**

Eine Assembly können Sie über die Befehle **ADL: Register Assembly** oder **ADL: Register Assembly through Configuration** registrieren.

**So führen Sie die Registrierung mit dem Befehl „ADL: Register Assembly“ durch**
1.  Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen.
2.  Geben Sie **ADL: Register Assembly** ein. 
3.  Geben Sie den lokalen Assemblypfad an. 
4.  Wählen Sie ein Data Lake Analytics-Konto aus.
5.  Wählen Sie eine Datenbank aus.

Ergebnis: Das Portal wird in einem Browser geöffnet, und der Assemblyregistrierungsprozess wird angezeigt.  

Alternativ können Sie den Befehl **ADL: Register Assembly** auch auslösen, indem Sie im Datei-Explorer mit der rechten Maustaste auf die DLL-Datei klicken. 

**So führen Sie die Registrierung mit dem Befehl „ADL: Register Assembly through Configuration“ durch**
1.  Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen.
2.  Geben Sie **ADL: Register Assembly through Configuration** ein. 
3.  Geben Sie den lokalen Assemblypfad an. 
4.  Die JSON-Datei wird angezeigt. Überprüfen und bearbeiten Sie bei Bedarf die Assemblyabhängigkeiten und Ressourcenparameter. Die entsprechenden Anweisungen werden im Fenster **Ausgabe** angezeigt. Speichern Sie die JSON-Datei (STRG+S), um mit der Assemblyregistrierung fortzufahren.

![CodeBehind für Data Lake Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-register-assembly-advance.png)
>[!NOTE]
>- Assemblyabhängigkeiten: Azure Data Lake Tools erkennt automatisch, ob die DLL Abhängigkeiten aufweist. Die erkannten Abhängigkeiten werden nach ihrer Erkennung in der JSON-Datei angezeigt. 
>- Ressourcen: Sie können Ihre DLL-Ressourcen (z.B. TXT-, PNG- oder CSV-Dateien) als Teil der Assemblyregistrierung hochladen. 

Alternativ können Sie den Befehl **ADL: Register Assembly through Configuration** auch auslösen, indem Sie im Datei-Explorer mit der rechten Maustaste auf die DLL-Datei klicken. 

Der folgende U-SQL-Code veranschaulicht, wie eine Assembly aufgerufen wird. In diesem Beispiel lautet der Name der Assembly *test*.

```
REFERENCE ASSEMBLY [test];

@a = 
    EXTRACT 
        Iid int,
    Starts DateTime,
    Region string,
    Query string,
    DwellTime int,
    Results string,
    ClickedUrls string 
    FROM @"Sample/SearchLog.txt" 
    USING Extractors.Tsv();

@d =
    SELECT DISTINCT Region 
    FROM @a;

@d1 = 
    PROCESS @d
    PRODUCE 
        Region string,
    Mkt string
    USING new USQLApplication_codebehind.MyProcessor();

OUTPUT @d1 
    TO @"Sample/SearchLogtest.txt" 
    USING Outputters.Tsv();
```


## <a name="access-the-data-lake-analytics-catalog"></a>Zugreifen auf den Data Lake Analytics-Katalog

Nachdem Sie die Verbindung mit Azure hergestellt haben, können Sie mit den folgenden Schritten auf den U-SQL-Katalog zugreifen.

**So greifen Sie auf Azure Data Lake Analytics-Metadaten zu**

1.  Drücken Sie STRG+UMSCHALT+P, und geben Sie dann **ADL: List Tables** ein.
2.  Wählen Sie eines der Data Lake Analytics-Konten aus.
3.  Wählen Sie eine der Data Lake Analytics-Datenbanken aus.
4.  Wählen Sie eines der Schemas aus. Sie können die Liste der Tabellen anzeigen.

## <a name="view-data-lake-analytics-jobs"></a>Anzeigen von Data Lake Analytics-Aufträgen

**So zeigen Sie Data Lake Analytics-Aufträge an**
1.  Öffnen Sie die Befehlspalette (STRG+UMSCHALT+P), und wählen Sie **ADL: Show Job** aus. 
2.  Wählen Sie ein Data Lake Analytics- oder ein lokales Konto aus. 
3.  Warten Sie, bis die Liste der Aufträge für das Konto angezeigt wird.
4.  Wählen Sie in der Auftragsliste einen Auftrag aus. Data Lake Tools öffnet die Auftragsdetails im Azure-Portal und zeigt die JobInfo-Datei in VS Code an.

![IntelliSense-Objekttypen der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-show-job.png)

## <a name="azure-data-lake-storage-integration"></a>Integration von Azure Data Lake Storage

Sie können Befehle zu Azure Data Lake-Speicher für Folgendes verwenden:
 - Durchsuchen der Azure Data Lake-Speicherressourcen 
 - Anzeigen einer Vorschau der Azure Data Lake-Speicherdatei  
 - Hochladen der Datei direkt in den Azure Data Lake-Speicher in VS Code 

### <a name="list-the-storage-path"></a>Auflisten des Speicherpfads 
Sie können den Speicherpfad über die Befehlspalette oder durch Klicken mit der rechten Maustaste auflisten.

**So listen Sie den Speicherpfad über die Befehlspalette auf**

1.  Öffnen Sie die Befehlspalette (STRG+UMSCHALT+P), und geben Sie **ADL: List Storage Path** ein.

    ![Data Lake Tools für Visual Studio Code – List Storage Path](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-storage.png)

2.  Wählen Sie die bevorzugte Methode zum Auflisten des Speicherpfads aus. Als Beispiel wird **Enter a path** verwendet.

    ![Data Lake Tools für Visual Studio Code – eine Möglichkeit zum Auflisten des Speicherpfads](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)

    > [!NOTE]
    >- VS Code behält den zuletzt besuchten Pfad in jedem Data Lake Analytics-Konto bei. Im Beispiel: „/tt/ss“.
    >- Browse from root path (Vom Stammpfad durchsuchen): Die Auflistung erfolgt vom Stammpfad Ihres ausgewählten Data Lake Analytics-Kontos oder eines lokalen Pfads aus.
    >- Enter a path (Pfad eingeben): Die Auflistung erfolgt von einem Pfad Ihres ausgewählten Data Lake Analytics-Kontos oder eines lokalen Pfads aus.
    
3. Wählen Sie ein Konto aus dem lokalen Pfad oder aus einem Data Lake Analytics-Konto aus.

    ![Data Lake Tools für Visual Studio Code – Auswählen von „more“](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4. Wählen Sie **more** (mehr) aus, um weitere Data Lake Analytics-Konten aufzulisten, und wählen Sie dann ein Data Lake Analytics-Konto aus.

    ![Data Lake Tools für Visual Studio Code – Auswählen eines Kontos](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

5.  Geben Sie einen Azure-Speicherpfad ein, z.B. „/output“.

    ![Data Lake Tools für Visual Studio Code – Eingeben des Speicherpfads](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-path.png)

6.  Ergebnis: In der Befehlspalette werden die auf Ihren Eingaben basierenden Pfadinformationen aufgelistet.

    ![Data Lake Tools für Visual Studio Code – Ergebnis für „List Storage Path“](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-path.png)

Alternativ können Sie den relativen Pfad auch auflisten, indem Sie durch Klicken mit der rechten Maustaste das Kontextmenü öffnen.

**So listen Sie den Speicherpfad durch Klicken mit der rechten Maustaste auf**

1.  Klicken Sie mit der rechten Maustaste auf die Pfadzeichenfolge, um **ADL: List Storage Path** auszuwählen.

       ![Data Lake Tools für Visual Studio Code – Anzeigen des Kontextmenüs](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-path.png)

2. Der ausgewählte relative Pfad wird in der Befehlspalette angezeigt.

   ![Data Lake Tools für Visual Studio Code – ausgewählter relativer Pfad](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-relative-path.png)

3.  Wählen Sie ein Konto aus dem lokalen Pfad oder aus einem Data Lake Analytics-Konto aus.

       ![Data Lake Tools für Visual Studio Code – Auswählen eines Kontos](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

4.  Ergebnis: In der Befehlspalette werden Ordner und Dateien des aktuellen Pfads aufgelistet.

       ![Data Lake Tools für Visual Studio Code – Auflisten des aktuellen Pfads](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-current.png)

### <a name="preview-the-storage-file"></a>Anzeigen einer Vorschau der Speicherdatei
Sie können über die Befehlspalette oder durch Klicken mit der rechten Maustaste eine Vorschau der Speicherdatei anzeigen.

**So zeigen Sie eine Vorschau der Speicherdatei über die Befehlspalette an**

1.  Öffnen Sie die Befehlspalette (STRG+UMSCHALT+P), und geben Sie **ADL: Preview Storage File** ein.

       ![Data Lake Tools für Visual Studio Code – Preview Storage File](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-preview.png)

2.  Wählen Sie ein Konto aus dem lokalen Pfad oder aus einem Data Lake Analytics-Konto aus.

       ![Data Lake Tools für Visual Studio Code – Auflisten der Konten](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  Wählen Sie **more** (mehr) aus, um weitere Data Lake Analytics-Konten aufzulisten, und wählen Sie dann ein Data Lake Analytics-Konto aus.

       ![Data Lake Tools für Visual Studio Code – Auswählen eines Kontos](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-select-adla-account.png)

4.  Geben Sie einen Azure-Speicherpfad oder eine Datei ein, z.B. „/output/SearchLog.txt“.

       ![Data Lake Tools für Visual Studio Code – Eingeben des Speicherpfads und einer Datei](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

5.  Ergebnis: In der Befehlspalette werden die auf Ihren Eingaben basierenden Pfadinformationen aufgelistet.

       ![Data Lake Tools für Visual Studio Code – Ergebnis für „Preview Storage File“](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

**So listen Sie den Speicherpfad durch Klicken mit der rechten Maustaste auf**

1.  Klicken Sie mit der rechten Maustaste auf einen Dateipfad, um eine Datei in der Vorschau anzuzeigen.

   ![Data Lake Tools für Visual Studio Code – Anzeigen des Kontextmenüs](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-right-click-preview.png) 

2.  Wählen Sie ein Konto aus dem lokalen Pfad oder aus einem Data Lake Analytics-Konto aus.

       ![Data Lake Tools für Visual Studio Code – Auswählen eines Kontos](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

3.  Ergebnis: Das Vorschauergebnis der Datei wird in VS Code angezeigt.

       ![Data Lake Tools für Visual Studio Code – Ergebnis für „Preview Storage File“](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-preview-results.png)

### <a name="upload-a-file"></a>Hochladen einer Datei 

Sie können Dateien über die Befehle **ADL: Upload File** oder **ADL: Upload File through Configuration** hochladen.

**So laden Sie Dateien über den Befehl „ADL: Upload File“ hoch**
1. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen, oder klicken Sie mit der rechten Maustaste im Skript-Editor, und geben Sie dann **Upload File** ein.
2.  Um die Datei hochzuladen, geben Sie einen lokalen Pfad ein.

    ![Data Lake Tools für Visual Studio Code – Eingeben des lokalen Pfads](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-input-local-path.png)

3. Wählen Sie eine der Methoden zum Auflisten des Speicherpfads aus. Als Beispiel wird **Enter a path** verwendet.

    ![Data Lake Tools für Visual Studio Code – List Storage Path](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account-selectoneway.png)
    >[!NOTE]
    >- VS Code behält den zuletzt besuchten Pfad in jedem Data Lake Analytics-Konto bei. Im Beispiel: „/tt/ss“.
    >- Browse from root path (Vom Stammpfad durchsuchen): Die Auflistung erfolgt vom Stammpfad Ihres ausgewählten Data Lake Analytics-Kontos oder eines lokalen Pfads aus.
    >- Enter a path (Pfad eingeben): Die Auflistung erfolgt von einem Pfad Ihres ausgewählten Data Lake Analytics-Kontos oder eines lokalen Pfads aus.

4. Wählen Sie ein Konto aus dem lokalen Pfad oder aus einem Data Lake Analytics-Konto aus.

    ![Data Lake Tools für Visual Studio Code – Speichern per Klick mit der rechten Maustaste](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-list-account.png)

5. Geben Sie einen Azure-Speicherpfad ein, z.B. „/output“.

       ![Data Lake Tools for Visual Studio Code enter storage path](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-input-preview-file.png)

6. Suchen des Azure-Speicherpfads Wählen Sie **Choose current folder** aus.

    ![Data Lake Tools für Visual Studio Code – Auswählen eines Ordners](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-choose-current-folder.png)

7.  Ergebnis: Im Fenster **Ausgabe** wird der Dateiuploadstatus angezeigt.

       ![Data Lake Tools für Visual Studio Code – Uploadstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)    

**So laden Sie Dateien über den Befehl „ADL: Upload File through Configuration“ hoch**
1.  Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen, oder klicken Sie mit der rechten Maustaste im Skript-Editor, und geben Sie dann **Upload File through Configuration** ein.
2.  In VS Code wird eine JSON-Datei angezeigt. Sie können Dateipfade eingeben und mehrere Dateien gleichzeitig hochladen. Die entsprechenden Anweisungen werden im Fenster **Ausgabe** angezeigt. Speichern Sie die JSON-Datei (STRG+S), um mit dem Dateiupload fortzufahren.

       ![Data Lake Tools für Visual Studio Code – Dateipfad](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-file.png)

3.  Ergebnis: Im Fenster **Ausgabe** wird der Dateiuploadstatus angezeigt.

       ![Data Lake Tools für Visual Studio Code – Uploadstatus](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-upload-status.png)     

Alternativ können Sie eine Datei in den Speicher hochladen, indem Sie im Skript-Editor mit der rechten Maustaste auf den vollständigen oder relativen Pfad der Datei klicken. Geben Sie den lokalen Pfad ein, und wählen Sie dann das Konto aus. Im Fenster **Ausgabe** wird der Uploadstatus angezeigt. 

### <a name="open-azure-storage-explorer"></a>Öffnen des Azure-Speicher-Explorers
Sie können den **Azure-Speicher-Explorer** über den Befehl **ADL: Open Web Azure Storage Explorer** oder über das Kontextmenü öffnen.

**So öffnen Sie den Azure-Speicher-Explorer**

1. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen.
2. Geben Sie **Open Web Azure Storage Explorer** ein, oder klicken Sie im Skript-Editor mit der rechten Maustaste auf einen relativen oder vollständigen Pfad, und wählen Sie dann **Open Web Azure Storage Explorer** aus.
3. Wählen Sie ein Data Lake Analytics-Konto aus.

Data Lake Tools öffnet den Azure-Speicherpfad im Azure-Portal. Sie können den Pfad aus dem Web suchen und eine Vorschau der Datei anzeigen.

### <a name="local-run-and-local-debug-for-windows-users"></a>Lokales Ausführen und lokales Debuggen für Windows-Benutzer
Bei einer lokalen Ausführung des U-SQL-Skripts werden Ihre lokalen Daten und das Skript lokal überprüft, bevor Ihr Code in Data Lake Analytics veröffentlicht wird. Mit der lokalen Debugfunktion können Sie die folgenden Aufgaben ausführen, bevor Ihr Code an Data Lake Analytics übermittelt wird: 
- Debuggen Ihres C#-CodeBehind 
- Ausführen des Codes in einzelnen Schritten 
- Lokales Überprüfen des Skripts

Informationen zum lokalen Ausführen und lokalen Debuggen finden Sie unter [Lokales Ausführen und lokales Debuggen von U-SQL mit Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).

## <a name="additional-features"></a>Zusätzliche Funktionen

Data Lake Tools für VS Code unterstützt die folgenden Features:

-   Automatische IntelliSense-Vervollständigung: Vorschläge werden in Popupfenstern für Elemente wie z.B. Schlüsselwörter, Methoden und Variablen angezeigt. Unterschiedliche Symbole stellen die verschiedene Objekttypen dar:

    - Scala-Datentyp
    - Komplexer Datentyp
    - Integrierte UDTs
    - .NET-Sammlung und -Klassen
    - C#-Ausdrücke
    - Integrierte C#-UDFs, -UDOs und -UDAAGs 
    - U-SQL-Funktionen
    - U-SQL-Windowingfunktionen
 
    ![IntelliSense-Objekttypen der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-objects.png)
 
-   Automatische IntelliSense-Vervollständigung für Data Lake Analytics-Metadaten: Data Lake Tools lädt die Data Lake Analytics-Metadateninformationen lokal herunter. Das IntelliSense-Feature füllt automatisch die Objekte anhand der Data Lake Analytics-Metadaten aus. Dazu gehören u.a. Datenbank, Schema, Tabelle, Sicht, Tabellenwertfunktion, Prozeduren und C#-Assemblys.
 
    ![IntelliSense-Metadaten der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-auto-complete-metastore.png)

-   IntelliSense-Fehlerkennzeichnung: Data Lake Tools unterstreicht die Bearbeitungsfehler für U-SQL und C#. 
-   Syntaxhervorhebung: Data Lake Tools verwendet verschiedene Farben, um Elemente wie Variablen, Schlüsselwörter, Datentypen und Funktionen zu unterscheiden. 

    ![Syntaxmarkierungen der Data Lake-Tools für Visual Studio Code](./media/data-lake-analytics-data-lake-tools-for-vscode/data-lake-tools-for-vscode-syntax-highlights.png)

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zum lokalen Ausführen und lokalen Debuggen von U-SQL mit Visual Studio Code finden Sie unter [Lokales Ausführen und lokales Debuggen von U-SQL mit Visual Studio Code](data-lake-tools-for-vscode-local-run-and-debug.md).
- Weitere Informationen zu den ersten Schritten mit Data Lake Analytics finden Sie unter [Tutorial: Erste Schritte mit Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Weitere Informationen zu Data Lake Tools für Visual Studio finden Sie unter [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Weitere Informationen zum Entwickeln von Assemblys finden Sie unter [Entwickeln von U-SQL-Assemblys für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-assemblies.md).





---
title: "Azure Data Lake Tools – Lokales Ausführen und lokales Debuggen von U-SQL mit Visual Studio Code | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie die Azure Data Lake Tools für Visual Studio Code zum lokalen Ausführen und lokalen Debuggen verwenden können."
Keywords: "VSCode, Azure Data Lake Tools, lokales Ausführen, lokales Debuggen, lokales Debugging, Vorschau Speicherdatei, Hochladen in Speicherpfad"
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: DJ
editor: jejiang
tags: azure-portal
ms.assetid: dc9b21d8-c5f4-4f77-bcbc-eff458f48de2
ms.service: data-lake-analytics
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: big-data
ms.date: 07/14/2017
ms.author: jejiang
ms.translationtype: HT
ms.sourcegitcommit: c999eb5d6b8e191d4268f44d10fb23ab951804e7
ms.openlocfilehash: e5e767a6f31be052c9bae059b49b31d59fdfd894
ms.contentlocale: de-de
ms.lasthandoff: 07/17/2017

---

# <a name="u-sql-local-run-and-local-debug-with-visual-studio-code"></a>Lokales Ausführen und lokales Debuggen von U-SQL mit Visual Studio Code

## <a name="prerequisites"></a>Voraussetzungen
- Azure Data Lake Tools für Visual Studio Code. Anweisungen finden Sie unter [Verwenden der Azure Data Lake Tools für Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- C# für Visual Studio Code (wenn Sie das lokale Debuggen von U-SQL ausführen möchten).

   ![Data Lake Tools für Visual Studio Code – Installation von vscodeCsharp](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-install-ms-vscodecsharp.png)
   
   > [!NOTE]
   > Die Funktionen zum lokalen Ausführen und lokalen Debuggen von U-SQL werden derzeit nur für Windows-Benutzer unterstützt. 


## <a name="set-up-u-sql-local-run-environment"></a>Einrichten der Umgebung zum lokalen Ausführen von U-SQL

1. Öffnen Sie die Befehlspalette durch Drücken von **STRG+UMSCHALT+P**, und geben Sie **ADL: Download LocalRun Dependency** ein, um die Pakete herunterzuladen.  

   ![DownloadLocalRun](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/DownloadLocalRun.png)

2. Suchen Sie die Abhängigkeitspakete unter dem im folgenden Ausgabebereich angezeigten Pfad, und installieren Sie dann BuildTools und Win10SDK 10240. Beispiel:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency
`  
  ![LocateDependency](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/LocateDependencyPath.png)

- Installieren von BuildTools – führen Sie für die Installation die Anweisungen des Assistenten aus.   

  ![InstallBuildTools](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallBuildTools.png)
- Installieren von Win10SDK 10240 – führen Sie die Installationsanweisungen aus.  

  ![InstallWin10SDK](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/InstallWin10SDK.png)
3. Einrichten der Umgebungsvariablen – legen Sie die Umgebungsvariable **SCOPE_CPP_SDK** auf folgenden Wert fest:  
`C:\Users\xxx\.vscode\extensions\usqlextpublisher.usql-vscode-ext-x.x.x\LocalRunDependency\CppSDK_3rdparty
`  
Starten Sie das Betriebssystem neu, damit die Umgebungsvariableneinstellungen wirksam werden.  

   ![ConfigSCOPE_CPP_SDK](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/ConfigScopeCppSDk.png)

## <a name="start-local-run-service-and-submit-u-sql-job-to-local-account"></a>Starten des Diensts für die lokale Ausführung und Übermitteln eines U-SQL-Auftrags an ein lokales Konto 
Als Erstbenutzer werden Sie aufgefordert, die Abhängigkeitspakete mit dem Befehl **ADL: Download Localrun Dependency** herunterzuladen, sofern dies noch nicht erfolgt ist.
1. Drücken Sie **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen, und geben Sie **ADL: Start Local Run Service** ein.
2. Akzeptieren Sie bei erstmaliger Verwendung die Lizenzbedingungen. 

   ![Akzeptieren der Lizenzbedingungen ](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/AcceptEULA.png)   
3. Die Befehlskonsole wird angezeigt. Als Erstbenutzer müssen Sie „3“ eingeben. Geben Sie dann einen lokalen Ordnerpfad für die Eingabe und Ausgabe Ihrer Daten ein. Für die anderen Optionen können Sie einfach den Standardwert verwenden. 

   ![Data Lake Tools für Visual Studio Code – Befehlskonsole für lokale Ausführung](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-cmd.png)
4. Drücken Sie STRG+UMSCHALT+P, um die Befehlspalette zu öffnen, geben Sie **ADL: Submit Job** ein, und wählen Sie dann **Local** aus, um den Auftrag an Ihr lokales Konto zu übermitteln.

   ![Data Lake Tools für Visual Studio Code – Auswählen von „local“](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-select-local.png)
5. Nach dem Übermitteln des Auftrags können Sie die Übermittlungsdetails anzeigen, indem Sie im Ausgabefenster auf „jobUrl“ klicken. Zudem können Sie über die Befehlskonsole den Übermittlungsstatus des Auftrags anzeigen. Geben Sie in der Befehlskonsole „7“ ein, wenn Sie weitere Auftragsdetails einsehen möchten.

   ![Data Lake Tools für Visual Studio Code – Ausgabe für lokale Ausführung](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-run-result.png)
   ![Data Lake Tools für Visual Studio Code – Status für lokale Ausführung](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-localrun-cmd-status.png) 



## <a name="start-local-debug-for-u-sql-job"></a>Starten des lokalen Debuggens für den U-SQL-Auftrag  
Als Erstbenutzer werden Sie aufgefordert, die Abhängigkeitspakete mit dem Befehl **ADL: Download Localrun Dependency** herunterzuladen, sofern dies noch nicht erfolgt ist.
  
1. Drücken Sie **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen, und geben Sie **ADL: Start Local Run Service** ein. Das Befehlsfenster wird angezeigt. Stellen Sie sicher, dass **DataRoot** festgelegt ist.
3. Legen Sie einen Breakpoint im zugrunde liegenden C#-Code fest.
4. Drücken Sie im Skript-Editor **STRG+UMSCHALT+P**, um die Befehlspalette zu öffnen, und geben Sie **Local Debug** ein, um den Dienst für das lokale Debuggen zu starten.

![Data Lake Tools für Visual Studio Code – Ergebnis des lokales Debuggens](./media/data-lake-analytics-data-lake-tools-for-vscode-local-run-and-debug/data-lake-tools-for-vscode-local-debug-result.png)


## <a name="next-steps"></a>Nächste Schritte
- Informationen zur Verwendung der Azure Data Lake Tools für Visual Studio Code finden Sie unter [Verwenden der Azure Data Lake Tools für Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).
- Informationen zu den ersten Schritten mit Data Lake Analytics finden Sie unter [Tutorial: Erste Schritte mit Azure Data Lake Analytics](data-lake-analytics-get-started-portal.md).
- Weitere Informationen zur Verwendung von Data Lake-Tools für Visual Studio finden Sie unter [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
- Informationen zum Entwickeln von Assemblys finden Sie unter [Entwickeln von U-SQL-Assemblys für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-assemblies.md).

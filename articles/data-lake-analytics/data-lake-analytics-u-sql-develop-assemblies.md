---
title: "Entwickeln von U-SQL-Assemblys für Azure Data Lake Analytics-Aufträge | Microsoft-Dokumentation"
description: "Hier erfahren Sie, wie Sie Assemblys zur Verwendung und Wiederverwendung in Data Lake Analytics-Aufträgen entwickeln. "
services: data-lake-analytics
documentationcenter: 
author: jejiang
manager: jhubbard
editor: cgronlun
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/30/2016
ms.author: jejiang
ms.translationtype: Human Translation
ms.sourcegitcommit: 9ae7e129b381d3034433e29ac1f74cb843cb5aa6
ms.openlocfilehash: c49f80f8dcd330d7f46726241e7178351b9cc28f
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="develop-u-sql-assemblies-for-azure-data-lake-analytics-jobs"></a>Entwickeln von U-SQL-Assemblys für Azure Data Lake Analytics-Aufträge
Hier erfahren Sie, wie Sie CodeBehind in Assemblys verwandeln, die in Data Lake Analytics-Aufträgen verwendet und wiederverwendet werden können. 

Mit U-SQL können Sie problemlos eigenen Code in .NET-Sprachen wie C#, VB.NET oder F# hinzufügen. Sie können sogar Ihre eigene Laufzeit bereitstellen, um andere Sprachen zu unterstützen.

Die einfachste Möglichkeit zur Verwendung von benutzerdefiniertem Code ist die Verwendung der Data Lake-Tools für die CodeBehind-Funktionen von Visual Studio. Weitere Informationen finden Sie im [Tutorial: Entwickeln von U-SQL-Skripts mit Data Lake-Tools für Visual Studio](data-lake-analytics-data-lake-tools-get-started.md). Die Verwendung von CodeBehind hat einige Nachteile:

- Der Quellcode wird für jede Skriptübermittlung hochgeladen.
- CodeBehind kann nicht mit anderen Aufträgen geteilt werden.

Zur Kompensierung dieser Nachteile können Sie CodeBehind in Assemblys verwandeln und diese im Data Lake Analytics-Katalog registrieren.

## <a name="prerequisites"></a>Voraussetzungen
* Visual Studio 2017, Visual Studio 2015, Visual Studio 2013 Update 4 oder Visual Studio 2012 mit Installation von Visual C++
* Microsoft Azure SDK für .NET, Version 2.5 oder höher.  Führen Sie die Installation mit dem Webplattform-Installer oder dem Visual Studio-Installer durch.
* Data Lake Analytics-Konto.  Weitere Informationen finden Sie unter [Erste Schritte mit Azure Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md).
* Arbeiten Sie das Lernprogramm [Erste Schritte mit U-SQL-Studio für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md) durch.
* Stelle Sie eine Verbindung zu Azure her.
* Laden Sie die Datenquelle hoch (siehe [Erste Schritte mit U-SQL-Studio für Azure Data Lake Analytics](data-lake-analytics-u-sql-get-started.md)). 

## <a name="develop-assemblies-for-u-sql"></a>Entwickeln von Assemblys für U-SQL

**So erstellen und übermitteln Sie einen U-SQL-Auftrag**

1. Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**.
2. Erweitern Sie **Installiert** > **Vorlagen** > **Azure Data Lake** > **U-SQL(ADLA)**, wählen Sie die Vorlage **Klassenbibliothek (für U-SQL-Anwendung)** aus, und klicken Sie anschließend auf **OK**.
3. Schreiben Sie Ihren Code in „Class1.cs“.  Im Anschluss sehen Sie ein Codebeispiel.

        using Microsoft.Analytics.Interfaces;

        namespace USQLApplication_codebehind
        {
            [SqlUserDefinedProcessor]
            public class MyProcessor : IProcessor
            {
                public override IRow Process(IRow input, IUpdatableRow output)
                {
                    output.Set(0, input.Get<string>(0));
                    output.Set(0, input.Get<string>(0));
                    return output.AsReadOnly();
                }
            }
        }
4. Klicken Sie auf das Menü **Build** und anschließend auf **Projektmappe erstellen**, um die DLL zu erstellen.

## <a name="register-assemblies"></a>Registrieren von Assemblys

Siehe [Verwenden des Azure Data Lake Analytics-Katalogs (U-SQL)](data-lake-analytics-use-u-sql-catalog.md).


## <a name="use-the-assemblies"></a>Verwenden der Assemblys

Siehe [Verwenden der Azure Data Lake-Tools für Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md).

## <a name="see-also"></a>Weitere Informationen
* [Erste Schritte mit Data Lake Analytics mithilfe von PowerShell](data-lake-analytics-get-started-powershell.md)
* [Erste Schritte mit Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md)
* [Verwenden von Data Lake-Tools für Visual Studio zum Entwickeln von U-SQL-Anwendungen](data-lake-analytics-data-lake-tools-get-started.md)
* [Verwenden des Azure Data Lake Analytics-Katalogs (U-SQL)](data-lake-analytics-use-u-sql-catalog.md)
* [Verwenden der Azure Data Lake-Tools für Visual Studio Code](data-lake-analytics-data-lake-tools-for-vscode.md)

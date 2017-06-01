---
title: Entwickeln von benutzerdefinierten Operatoren von U-SQL (UDOs) | Microsoft Docs
description: "Erfahren Sie, wie Sie benutzerdefinierte Operatoren zur Verwendung und Wiederverwendung in Data Lake Analytics-Aufträgen entwickeln. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: e5189e4e-9438-46d1-8686-ed4836bf3356
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2016
ms.author: edmaca
ms.translationtype: Human Translation
ms.sourcegitcommit: 2db2ba16c06f49fd851581a1088df21f5a87a911
ms.openlocfilehash: ef310a094667f390addd5d0df3dc68d67100d2f4
ms.contentlocale: de-de
ms.lasthandoff: 05/08/2017


---
# <a name="develop-u-sql-user-defined-operators-udos"></a>Entwickeln von benutzerdefinierten Operatoren von U-SQL (UDOs)
Erfahren Sie, wie man benutzerdefinierte Operatoren entwickelt, um Daten in einem U-SQL-Auftrag zu verarbeiten.

Anweisungen für die Entwicklung allgemeiner Assemblys für U-SQL, finden Sie unter [Entwickeln U-SQL-Assemblys für Azure Data Lake Analytics-Aufträge](data-lake-analytics-u-sql-develop-assemblies.md).

## <a name="define-and-use-user-defined-operator-in-u-sql"></a>Definieren und Verwenden von benutzerdefinierten Operatoren in U-SQL
**So erstellen und übermitteln Sie einen U-SQL-Auftrag**

1. Klicken Sie im Menü **Datei** auf **Neu** und dann auf **Projekt**.
2. Wählen Sie den Typ **U-SQL-Projekt** aus.

    ![Neues Visual Studio-U-SQL-Projekt](./media/data-lake-analytics-data-lake-tools-get-started/data-lake-analytics-data-lake-tools-new-project.png)
3. Klicken Sie auf **OK**. Visual Studio erstellt eine Projektmappe mit der Datei „Script.usql“.
4. Erweitern Sie im **Projektmappen-Explorer** die Datei „Script.usql“, und doppelklicken Sie dann auf **Script.usql.cs**.
5. Fügen Sie den folgenden Code in die Datei ein:

        using Microsoft.Analytics.Interfaces;
        using System.Collections.Generic;

        namespace USQL_UDO
        {
            public class CountryName : IProcessor
            {
                private static IDictionary<string, string> CountryTranslation = new Dictionary<string, string>
                {
                    {
                        "Deutschland", "Germany"
                    },
                    {
                        "Schwiiz", "Switzerland"
                    },
                    {
                        "UK", "United Kingdom"
                    },
                    {
                        "USA", "United States of America"
                    },
                    {
                        "中国", "PR China"
                    }
                };

                public override IRow Process(IRow input, IUpdatableRow output)
                {

                    string UserID = input.Get<string>("UserID");
                    string Name = input.Get<string>("Name");
                    string Address = input.Get<string>("Address");
                    string City = input.Get<string>("City");
                    string State = input.Get<string>("State");
                    string PostalCode = input.Get<string>("PostalCode");
                    string Country = input.Get<string>("Country");
                    string Phone = input.Get<string>("Phone");

                    if (CountryTranslation.Keys.Contains(Country))
                    {
                        Country = CountryTranslation[Country];
                    }
                    output.Set<string>(0, UserID);
                    output.Set<string>(1, Name);
                    output.Set<string>(2, Address);
                    output.Set<string>(3, City);
                    output.Set<string>(4, State);
                    output.Set<string>(5, PostalCode);
                    output.Set<string>(6, Country);
                    output.Set<string>(7, Phone);

                    return output.AsReadOnly();
                }
            }
        }
6. Öffnen Sie die Datei **Script.usql**, und fügen Sie das folgende U-SQL-Skript ein:

        @drivers =
            EXTRACT UserID      string,
                    Name        string,
                    Address     string,
                    City        string,
                    State       string,
                    PostalCode  string,
                    Country     string,
                    Phone       string
            FROM "/Samples/Data/AmbulanceData/Drivers.txt"
            USING Extractors.Tsv(Encoding.Unicode);

        @drivers_CountryName =
            PROCESS @drivers
            PRODUCE UserID string,
                    Name string,
                    Address string,
                    City string,
                    State string,
                    PostalCode string,
                    Country string,
                    Phone string
            USING new USQL_UDO.CountryName();    

        OUTPUT @drivers_CountryName
            TO "/Samples/Outputs/Drivers.csv"
            USING Outputters.Csv(Encoding.Unicode);
7. Geben Sie das Data Lake Analytics-Konto, die -Datenbank und das -Schema an.
8. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Script.usql**, und klicken Sie dann auf **Skript erstellen**.
9. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Script.usql**, und klicken Sie dann auf **Skript senden**.
10. Wenn Sie noch nicht mit Ihrem Azure-Abonnement verbunden sind, werden Sie zur Eingabe der Anmeldeinformationen für Ihr Azure-Konto aufgefordert.
11. Klicken Sie auf **Submit**. Nach Abschluss der Übermittlung werden Ergebnisse und ein Auftragslink im Fenster „Ergebnisse“ angezeigt.
12. Sie müssen auf die Schaltfläche **Aktualisieren** klicken, um den letzten Auftragsstatus anzuzeigen und den Bildschirm zu aktualisieren.

**So zeigen Sie die Ausgabe an**

1. Erweitern Sie im **Server-Explorer** nacheinander **Azure**, **Data Lake Analytics**, Ihr Data Lake Analytics-Konto und **Speicherkonten**. Klicken Sie mit der rechten Maustaste auf den Standardspeicher, und klicken Sie dann auf **Explorer**.
2. Erweitern Sie „Beispielcode“ und „Ausgaben“, und doppelklicken Sie dann auf **Drivers.csv**.

## <a name="see-also"></a>Siehe auch
* [Erste Schritte mit Data Lake Analytics mithilfe von PowerShell](data-lake-analytics-get-started-powershell.md)
* [Erste Schritte mit Data Lake Analytics mithilfe des Azure-Portals](data-lake-analytics-get-started-portal.md)
* [Verwenden von Data Lake-Tools für Visual Studio zum Entwickeln von U-SQL-Anwendungen](data-lake-analytics-data-lake-tools-get-started.md)


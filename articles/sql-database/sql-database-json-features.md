---
title: "JSON-Features für die Azure SQL-Datenbank | Microsoft Docs"
description: "Mit Azure SQL-Datenbank können Sie Daten in JavaScript Object Notation (JSON) analysieren, abfragen und formatieren."
services: sql-database
documentationcenter: 
author: jovanpop-msft
manager: jhubbard
editor: 
ms.assetid: 55860105-2f5f-4b10-87a0-99faa32b5653
ms.service: sql-database
ms.custom: development
ms.devlang: NA
ms.date: 11/15/2016
ms.author: jovanpop
ms.workload: NA
ms.topic: article
ms.tgt_pltfrm: NA
translationtype: Human Translation
ms.sourcegitcommit: 76d06bd9e0b033a2fb993f6183900e42e5232665
ms.openlocfilehash: 741095b0ec2ae2ce125b59dfc5d1112d0a92de43


---
# <a name="getting-started-with-json-features-in-azure-sql-database"></a>Getting started with JSON features in Azure SQL Database (Erste Schritte mit JSON-Features in der Azure SQL-Datenbank)
In Azure SQL-Datenbank können Sie Daten analysieren und abfragen, die im JavaScript Object Notation-Format [(JSON)](http://www.json.org/) dargestellt werden, und Ihre relationalen Daten als JSON-Text exportieren.

JSON ist ein gängiges Datenformat zum Austauschen von Daten in modernen Web- und mobilen Anwendungen. JSON wird auch zum Speichern von halbstrukturierten Daten in Protokolldateien oder in NoSQL-Datenbanken wie [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) verwendet. Viele REST-Webdienste geben Ergebnisse als JSON-Text formatiert zurück oder akzeptieren Daten im JSON-Format. Die meisten Azure-Dienste, z.B. [Azure Search](https://azure.microsoft.com/services/search/), [Azure Storage](https://azure.microsoft.com/services/storage/)und [Azure DocumentDB](https://azure.microsoft.com/services/documentdb/) verfügen über REST-Endpunkte, die Daten im JSON-Format zurückgeben oder verarbeiten.

Mit Azure SQL-Datenbank können Sie problemlos mit JSON-Daten arbeiten und Ihre Datenbank in moderne Dienste integrieren.

## <a name="overview"></a>Übersicht
Azure SQL-Datenbank stellt die folgenden Funktionen für das Arbeiten mit JSON-Daten bereit:

![JSON-Funktionen](./media/sql-database-json-features/image_1.png)

Mithilfe der integrierten Funktionen [JSON_VALUE](https://msdn.microsoft.com/library/dn921898.aspx), [JSON_QUERY](https://msdn.microsoft.com/library/dn921884.aspx) und [ISJSON](https://msdn.microsoft.com/library/dn921896.aspx) können Sie Daten aus einem JSON-Text extrahieren oder überprüfen, ob er ordnungsgemäß formatiert ist. Mit der Funktion [JSON_MODIFY](https://msdn.microsoft.com/library/dn921892.aspx) können Sie einen Wert im JSON-Text aktualisieren. Bei komplexeren Abfragen und Analysen kann die Funktion [OPENJSON](https://msdn.microsoft.com/library/dn921885.aspx) ein Array von JSON-Objekten in eine Reihe von Zeilen umwandeln. Für das zurückgegebene Resultset kann eine beliebige SQL-Abfrage ausgeführt werden. Schließlich können Sie mit einer [FOR JSON](https://msdn.microsoft.com/library/dn921882.aspx) -Klausel in den relationalen Tabellen gespeicherte Daten als JSON-Text formatieren.

## <a name="formatting-relational-data-in-json-format"></a>Formatieren von relationalen Daten im JSON-Format
Für einen Webdienst, der Daten aus der Datenbankschicht entnimmt und eine Antwort im JSON-Format bereitstellt, oder clientseitige JavaScript-Frameworks oder Bibliotheken, die Daten im JSON-Format akzeptieren, können Sie den Datenbankinhalt direkt in einer SQL-Abfrage im JSON-Format formatieren. Sie müssen nicht mehr Anwendungscode schreiben, der Ergebnisse aus Azure SQL-Datenbank als JSON formatiert, oder eine JSON-Serialisierungsbibliothek einbeziehen, um tabellarische Abfrageergebnisse zu konvertieren und dann Objekte in das JSON-Format zu serialisieren. Stattdessen können Sie die FOR JSON-Klausel verwenden, um SQL-Abfrageergebnisse in Azure SQL-Datenbank im JSON-Format zu formatieren und direkt in Ihrer Anwendung zu verwenden.

Im folgenden Beispiel werden Zeilen aus der Tabelle „Sales.Customer“ mithilfe der FOR JSON-Klausel im JSON-Format formatiert:

```
select CustomerName, PhoneNumber, FaxNumber
from Sales.Customers
FOR JSON PATH
```

Die FOR JSON PATH-Klausel formatiert die Ergebnisse der Abfrage als JSON-Text. Spaltennamen werden als Schlüssel verwendet, während die Zellenwerte als JSON-Werte generiert werden:

```
[
{"CustomerName":"Eric Torres","PhoneNumber":"(307) 555-0100","FaxNumber":"(307) 555-0101"},
{"CustomerName":"Cosmina Vlad","PhoneNumber":"(505) 555-0100","FaxNumber":"(505) 555-0101"},
{"CustomerName":"Bala Dixit","PhoneNumber":"(209) 555-0100","FaxNumber":"(209) 555-0101"}
]
```

Das Resultset wird als JSON-Array formatiert, in dem jede Zeile als separates JSON-Objekt formatiert ist.

PATH gibt an, dass Sie das Ausgabeformat für Ihr JSON-Ergebnis mithilfe der Punktnotation in Spaltenaliasen anpassen können. Die folgende Abfrage ändert den Namen des Schlüssels „CustomerName“ in das Ausgabe-JSON-Format und fügt Telefon- und Faxnummern in das Unterobjekt „Contact“ ein:

```
select CustomerName as Name, PhoneNumber as [Contact.Phone], FaxNumber as [Contact.Fax]
from Sales.Customers
where CustomerID = 931
FOR JSON PATH, WITHOUT_ARRAY_WRAPPER
```

Die Ausgabe dieser Abfrage sieht folgendermaßen aus:

```
{
    "Name":"Nada Jovanovic",
    "Contact":{
           "Phone":"(215) 555-0100",
           "Fax":"(215) 555-0101"
    }
}
```

In diesem Beispiel wird durch Angabe der Option [WITHOUT_ARRAY_WRAPPER](https://msdn.microsoft.com/library/mt631354.aspx) ein einzelnes JSON-Objekt statt eines Arrays zurückgegeben. Sie können diese Option verwenden, wenn Sie wissen, dass Sie ein einzelnes Objekt als Ergebnis der Abfrage zurückgeben.

Der größte Vorteil der FOR JSON-Klausel ist, dass sie damit komplexe hierarchische Daten aus Ihrer Datenbank als geschachtelte JSON-Objekte oder -Arrays formatiert zurückgeben können. Das folgende Beispiel zeigt, wie Aufträge des Kunden als geschachteltes Array von Aufträgen einbezogen werden:

```
select CustomerName as Name, PhoneNumber as Phone, FaxNumber as Fax,
        Orders.OrderID, Orders.OrderDate, Orders.ExpectedDeliveryDate
from Sales.Customers Customer
    join Sales.Orders Orders
        on Customer.CustomerID = Orders.CustomerID
where Customer.CustomerID = 931
FOR JSON AUTO, WITHOUT_ARRAY_WRAPPER

```

Statt einzelne Abfragen zu senden, um Daten des Kunden zu erhalten, und dann eine Liste der zugehörigen Aufträge abzurufen, können Sie alle erforderlichen Daten, wie in der folgenden Beispielausgabe gezeigt, mit einer einzigen Abfrage erhalten:

```
{
  "Name":"Nada Jovanovic",
  "Phone":"(215) 555-0100",
  "Fax":"(215) 555-0101",
  "Orders":[
    {"OrderID":382,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":395,"OrderDate":"2013-01-07","ExpectedDeliveryDate":"2013-01-08"},
    {"OrderID":1657,"OrderDate":"2013-01-31","ExpectedDeliveryDate":"2013-02-01"}
]
}
```

## <a name="working-with-json-data"></a>Arbeiten mit JSON-Daten
Wenn Sie nicht über streng strukturierte Daten verfügen, komplexe Unterobjekte, Arrays oder hierarchische Daten haben, oder Ihre Datenstrukturen sich mit der Zeit weiterentwickeln, kann das JSON-Format Sie bei der Darstellung beliebiger komplexer Datenstrukturen unterstützen.

JSON ist ein Textformat, das wie jeder andere Stringtyp in Azure SQL-Datenbank verwendet werden kann. Sie können JSON-Daten im standardmäßigen NVARCHAR-Datentyp senden oder speichern:

```
CREATE TABLE Products (
  Id int identity primary key,
  Title nvarchar(200),
  Data nvarchar(max)
)
go
CREATE PROCEDURE InsertProduct(@title nvarchar(200), @json nvarchar(max))
AS BEGIN
    insert into Products(Title, Data)
    values(@title, @json)
END
```

Die in diesem Beispiel verwendeten JSON-Daten werden mithilfe des NVARCHAR(MAX)-Datentyps dargestellt. JSON kann in diese Tabelle eingefügt oder mithilfe von Transact-SQL-Standardsyntax als Argument der gespeicherten Prozedur bereitgestellt werden, wie im folgenden Beispiel gezeigt:

```
EXEC InsertProduct 'Toy car', '{"Price":50,"Color":"White","tags":["toy","children","games"]}'
```

Jede clientseitige Sprache oder Bibliothek, die mit Zeichenfolgendaten in Azure SQL-Datenbank funktioniert, ist auch mit JSON-Daten verwendbar. JSON kann in einer Tabelle gespeichert werden, die den NVARCHAR-Datentyp unterstützt, z.B. einer speicheroptimierten Tabelle oder einer Tabelle mit Systemversionsverwaltung. JSON ist weder für den clientseitigen Code noch in der Datenbankschicht mit Einschränkungen verbunden.

## <a name="querying-json-data"></a>Abfragen von JSON-Daten
Wenn Sie im JSON-Format formatierte Daten in Azure SQL-Tabellen gespeichert haben, können Sie diese Daten mithilfe von JSON-Funktionen in beliebigen SQL-Abfragen verwenden.

Mit JSON-Funktionen, die in Azure SQL-Datenbank zur Verfügung stehen, können Sie im JSON-Format formatierte Daten wie jeden anderen SQL-Datentyp behandeln. Sie können problemlos Werte aus dem JSON-Text extrahieren und JSON-Daten in beliebigen Abfragen verwenden:

```
select Id, Title, JSON_VALUE(Data, '$.Color'), JSON_QUERY(Data, '$.tags')
from Products
where JSON_VALUE(Data, '$.Color') = 'White'

update Products
set Data = JSON_MODIFY(Data, '$.Price', 60)
where Id = 1
```

Die Funktion JSON_VALUE extrahiert einen Wert aus JSON-Text, der in der Spalte „Data“ gespeichert ist. Diese Funktion verwendet einen JavaScript-ähnlichen Pfad, um auf einen zu extrahierenden Wert in JSON-Text zu verweisen. Der extrahierte Wert kann in einem beliebigen Teil der SQL-Abfrage verwendet werden.

Die Funktion JSON_QUERY ähnelt JSON_VALUE. Im Gegensatz zu JSON_VALUE extrahiert diese Funktion komplexe Unterobjekte, z.B. Arrays oder Objekte, die in JSON-Text enthalten sind.

Mit der JSON_MODIFY-Funktion können Sie die Pfadangabe des Werts in dem JSON-Text angeben, der aktualisiert werden soll, sowie einen neuen Wert, der den alten überschreibt. Auf diese Weise können Sie JSON-Text problemlos ohne erneute Analyse der gesamten Struktur aktualisieren.

Da JSON in einem Standardtext gespeichert wird, gibt es keine Garantie, dass die in Textspalten gespeicherten Werte ordnungsgemäß formatiert sind. Sie können mithilfe standardmäßiger Azure SQL-Datenbank-CHECK-Einschränkungen und der ISJSON-Funktion überprüfen, ob in der JSON-Spalte gespeicherter Text ordnungsgemäß formatiert ist:

```
ALTER TABLE Products
    ADD CONSTRAINT [Data should be formatted as JSON]
        CHECK (ISJSON(Data) > 0)
```

Wenn der Eingabetext ordnungsgemäßes JSON-Format aufweist, gibt die ISJSON-Funktion den Wert 1 zurück. Bei jeder Einfügung oder Aktualisierung in der JSON-Spalte stellt diese Einschränkung sicher, dass der neue Textwert richtiges JSON-Format aufweist.

## <a name="transforming-json-into-tabular-format"></a>Transformieren von JSON in Tabellenformat
Mit Azure SQL-Datenbank können Sie auch JSON-Auflistungen in Tabellenformat transformieren und JSON-Daten laden oder abfragen.

OPENJSON ist eine Tabellenwertfunktion, die JSON-Text analysiert, ein Array von JSON-Objekten sucht, die Elemente des Arrays durchläuft und für jedes Element des Arrays im Ausgabeergebnis eine Zeile zurückgibt.

![JSON im Tabellenformat](./media/sql-database-json-features/image_2.png)

Im obigen Beispiel können wir angeben, wo das JSON-Array zu suchen ist, das geöffnet werden soll (im Pfad „$.Orders“), welche Spalten als Ergebnis zurückgegeben werden sollten, und wo die JSON-Werte zu finden sind, die als Zellen zurückgegeben werden.

Wir können ein JSON-Array in der Variablen @orders in einen Satz von Zeilen transformieren, dieses Resultset analysieren oder Zeilen in eine Standardtabelle einfügen:

```
CREATE PROCEDURE InsertOrders(@orders nvarchar(max))
AS BEGIN

    insert into Orders(Number, Date, Customer, Quantity)
    select Number, Date, Customer, Quantity
    OPENJSON (@orders)
     WITH (
            Number varchar(200),
            Date datetime,
            Customer varchar(200),
            Quantity int
     )

END
```
Die als JSON-Array formatierte und der gespeicherten Prozedur als Parameter bereitgestellte Auflistung der Aufträge kann analysiert und in die Tabelle „Orders“ eingefügt werden.

## <a name="next-steps"></a>Nächste Schritte
In diesen Ressourcen finden Sie weitere Informationen zur Integration von JSON in Ihre Anwendung:

* [TechNet-Blog](https://blogs.technet.microsoft.com/dataplatforminsider/2016/01/05/json-in-sql-server-2016-part-1-of-4/)
* [MSDN-Dokumentation](https://msdn.microsoft.com/library/dn921897.aspx)
* [Channel 9-Video](https://channel9.msdn.com/Shows/Data-Exposed/SQL-Server-2016-and-JSON-Support)

Weitere Informationen zu verschiedenen Szenarios der Integration von JSON in Ihre Anwendung enthalten die Demos in diesem [Channel 9-Video](https://channel9.msdn.com/Events/DataDriven/SQLServer2016/JSON-as-a-bridge-betwen-NoSQL-and-relational-worlds), oder suchen Sie in den [JSON-Blogbeiträgen](http://blogs.msdn.com/b/sqlserverstorageengine/archive/tags/json/) nach einem Szenario, das Ihrem Anwendungsfall entspricht.




<!--HONumber=Nov16_HO3-->



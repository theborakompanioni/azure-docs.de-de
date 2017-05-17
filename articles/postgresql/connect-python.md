---
title: "Lesevorgänge mit Python über Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: "Unter der Annahme, dass keine Vorkenntnisse vorhanden sind, wird erläutert, wie ein Python-Codebeispiel zum Schreiben und Lesen von Daten aus einer Tabelle in Azure-Datenbank für PostgreSQL ausgeführt wird."
services: postgresql
author: MightyPen
ms.author: genemi
manager: jhubbard
editor: jasonh
ms.assetid: 
ms.service: postgresql-database
ms.custom: quick start connect
ms.workload: drivers
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: article
ms.date: 05/10/2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: 42f5e8191e52d1e2a4502a961158b6fec1c061d1
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017

---
# <a name="python-reading-from-azure-database-for-postgresql"></a>Lesevorgänge mit Python über Azure-Datenbank für PostgreSQL


Dieser Artikel bietet einen kurzen Überblick über das Python-Programm, das eine SQL SELECT-Anweisung zum Lesen einer Zeile aus einer Tabelle verwendet. In dem Artikel wird zudem erläutert, wie die Anforderungen zum Ausführen des Python-Programms ermittelt und die entsprechenden Komponenten installiert werden können.

Unsere Python-Beispielprogramm und die beschriebenen Python-bezogenen Tools sind alle gleichermaßen für verschiedene Plattformen wie Linux, Mac und Windows geeignet. 


## <a name="install-the-python-interpreter"></a>Installieren des Python-Interpreter


Das Python-Codebeispiel in diesem Artikel wurde für den Python-Interpreter Version 2.7 geschrieben und kann nicht von Version 3.x ausgeführt werden.

Über den folgenden Link kann der Python-Interpreter Version 2.7 heruntergeladen und installiert werden:

- [Download des Python-Interpreter über python.org](https://www.python.org/downloads/)

Stellen Sie nach der Installation sicher, dass Sie den Interpreter finden und über eine Befehlszeile ausführen können. Verwenden Sie z.B. folgenden Befehl:

`python.exe -?`


## <a name="install-pipexe-the-python-module-installer"></a>Installieren des Installationsprogramms für das Python-Modul (pip.exe)


Bei der Installation des Python-Interpreter wird möglicherweise auch pip.exe installiert, womöglich in einem Unterverzeichnis mit dem Namen *Skripts/*. Mit Pip.exe werden spezielle Python-Module installiert. Stellen Sie sicher, dass Sie pip.exe finden und ausführen können.

`pip.exe`

Wenn pip.exe nicht ausgeführt werden kann, prüfen Sie, ob die Programmdatei des Python-Dienstprogramms mit dem Namen **get-pip.py** vorhanden ist. Wenn get-pip.py vorhanden ist, können Sie die Datei zum Abrufen von pip.exe ausführen:

`python.exe get-pip.py`


## <a name="install-psycopg-the-connection-module"></a>Installieren Sie das Verbindungsmodul psycopg.


Unser Python-Programm benötigt ein Modul, das eine Verbindung zwischen unserem Programm und dem Azure-Datenbank für PostgreSQL-Server herstellen kann. Der Name des Verbindungsmoduls lautet **psycopg2**. Weitere Details finden Sie hier:

- [psycopg2-Website](http://initd.org/psycopg/)

Installieren Sie psycopg2, indem Sie den folgenden pip.exe-Installationsbefehl verwenden:

`pip.exe install psycopg2`


## <a name="create-an-azure-database-for-postgresql-server"></a>Erstellen eines Servers für Azure-Datenbank für PostgreSQL


Wenn Sie noch keinen Zugriff auf einen Azure-Datenbank für PostgreSQL-Server haben, lesen Sie die Dokumentation, in der die Methoden zum Erstellen eines Servers erläutert werden:

- [Erstellen eines Servers für Azure-Datenbank für PostgreSQL mithilfe des Azure-Portals](quickstart-create-server-database-portal.md)
- [Erstellen eines Servers für Azure-Datenbank für PostgreSQL mithilfe der Azure CLI](quickstart-create-server-database-azure-cli.md)


## <a name="obtain-the-connection-string-values"></a>Abrufen der Werte der Verbindungszeichenfolge


Über das Azure-Portal können Sie die Werte der Verbindungszeichenfolge für Ihren Azure-Datenbank für PostgreSQL-Server abrufen. Sie benötigen die Parameterwerte, die in der folgenden Tabelle beschrieben werden.

- Spalte *Name* &ndash; Zeigt die Parameterbezeichner an, die psycopg2 benötigt.
- Spalte *-Symbol* &ndash; Zeigt die Parameterbezeichner an, die von unserem Python-Beispielprogramm *PythonDriver.py* benötigt werden.


| Name | -Symbol | Beispielwert |
| :--  | :--     | :--           |
| host | -h | myazurepostgresql.database.windows.net |
| user | -U | myalias@myazurepostgresql |
| dbname | -d angeben, | postgres<br />*(Alle PostgreSQL-Server verfügen über eine Datenbank mit dem Namen **postgres**.)* |
| port | -p | 5432 *(wahrscheinlich dieser bestimmte Wert 5432)* |
| password | -P | MySecretPassword |
||||


## <a name="the-python-sample-program"></a>Das Python-Beispielprogramm


Dieser Abschnitt enthält den Quellcode für unser Python-Beispielprogramm. Sie können das Programm weiter unten in diesem Artikel ausführen.

```python
# PythonDriver.py
# Python version 2.7

import psycopg2
import sys
import getopt

def main():

    host = ""
    user = ""
    dbname = ""
    port = ""
    password = ""

    try:
        opts, args = getopt.getopt(sys.argv[1:], "h:U:d:p:P:", [])
    except getopt.GetoptError as exc:
        print str(exc)
        usage()
        exit(2)

    for o, a in opts:
        if o == "-h": host = str(a)
        if o == "-U": user = str(a)
        if o == "-d": dbname = str(a)
        if o == "-p": port = str(a)
        if o == "-P": password = str(a)

    conn_string = "host={0} user={1} dbname={2} password={3}".format(host, user, dbname, password)

    try:
        # Create a connection.  Raise an exception if cannot connect.
        conn = psycopg2.connect(conn_string) 
        cursor = conn.cursor()

        #cursor.execute("DROP TABLE testpy1;")
        #conn.commit()

        cursor.execute("CREATE TABLE testpy1 (id serial PRIMARY KEY, num integer, data varchar);")

        cursor.execute("INSERT INTO testpy1 (num, data) VALUES (%s, %s)", (100, "First'row"))
        cursor.execute("INSERT INTO testpy1 (num, data) VALUES (%s, %s)", (110, "Second_row"))
        conn.commit()

        cursor.execute("SELECT id, num, data FROM testpy1;")
        result = cursor.fetchone()

        # Optionally, you can comment these clean-up lines to leave
        # the testpy1 table available for your inspection by pgAdmin. 
        cursor.execute("DROP TABLE testpy1;")
        conn.commit()

    except Exception as exc:
        print "FAILED ", exc
        exit(1)

    print "SUCCESS: ", result

if __name__ == "__main__":
    main()
```


## <a name="command-lines-to-run-the-sample-program"></a>Befehlszeilen zum Ausführen des Beispielprogramms


In diesem Abschnitt werden die Befehlszeilen aufgeführt, die zum Testen des Python-Beispielprogramms zusammen mit der Ausgabe der eigentlichen Bestätigung verwendet werden.

Bei den zuvor beschriebenen Parametern müssen Sie die hier angezeigten Beispielwerte durch die tatsächlichen Werte der Verbindungszeichenfolge ersetzen.

Die genaue Syntax zur Ausführung des vorangehenden Python-Beispielprogramms kann geringfügig abweichen. Die genaue Syntax hängt von Ihrem Betriebssystem und dem von Ihnen verwendeten Konsolentyp ab.


#### <a name="windows-cmdexe-console"></a>Windows-Konsole cmd.exe


Der folgende Codeblock stellt einen tatsächlichen Testlauf des Python-Beispielprogramms dar. Eine einfache cmd.exe-Befehlszeile wurde verwendet. Nach jedem Zeilenfortsetzungszeichen „^“ wurde Folgendes eingegeben:

1. Die Eingabetaste wurde gedrückt.
2. Der Ausdruck **Mehr?** wurde angezeigt.
3. Ein anderer Teil der gesamten Zeile wurde eingegeben usw.

Diese Zeilenfortsetzungsmethode wurde verwendet, um zu verhindern, dass das Beispiel so umfassend wird, dass die Darstellung hier oder der Druck beeinträchtigt wird.

```cmd
set Prompt=[$P\]$_$+$G$G$S

[C:\Users\myalias\AppData\Local\Programs\Python\Python27\]
>> .\python.exe ".\_myalias\PythonDriver.py" ^
More?  -h myazurepostgresql.database.windows.net ^
More?  -p 5432 ^
More?  -d postgres ^
More?  -U myalias@myazurepostgresql ^
More?  -P mySecretPassword
SUCCESS:  (1, 100, "First'row")

[C:\Users\myalias\AppData\Local\Programs\Python\Python27\]
>>
```

Die Zeile **ERFOLGREICH** kann als Bestätigung betrachtet werden, dass die Anwendung ausgeführt wurde.

Als alternatives Verfahren könnte die gesamte Befehlszeile in eine BAT-Datei eingefügt werden. Anschließend könnte die BAT-Datei über die Befehlszeile cmd.exe ausgeführt werden.


#### <a name="powershell-file"></a>PowerShell-Datei


In der PowerShell-Konsole werden keine Zeilenfortsetzungszeichen in der Befehlszeile unterstützt. Aus diesem Grund haben wir in diesem PowerShell-Abschnitt die Befehle in eine PowerShell-Datei eingefügt. Dann haben wir die Datei über die PowerShell-Befehlszeile ausgeführt.

Kopieren Sie den folgenden Code in eine Datei namens *PythonDriverRun.ps1*.

```powershell
# PythonDriverRun.ps1
cd C:\Users\myalias\AppData\Local\Programs\Python\Python27\

.\python.exe `
 .\_myalias\PythonDriver.py `
 -h myazurepostgresql.database.windows.net `
 -p 5432 `
 -d postgres `
 -U myalias@myazurepostgresql `
 -P mySecretPassword
```

Führen Sie PythonDriverRun.ps1 aus, wie nachfolgend dargestellt wird. Die Zeile **ERFOLGREICH** kann als Bestätigung betrachtet werden, dass das Programm den Druckvorgang ausführt.

```cmd
[C:\Users\myalias\AppData\Local\Programs\Python\Python27\]
0 >> .\_myalias\PythonDriverRun.ps1
SUCCESS:  (1, 100, "First'row")

[C:\Users\myalias\AppData\Local\Programs\Python\Python27\]
0 >>
```

Wenn Sie PythonDriverRun.ps1 direkt über die PowerShell-Befehlszeile ausführen möchten, müssen Sie ein „&“-Zeichen gefolgt von einem Leerzeichen voranstellen. Ohne das führende „&“ wird die Bestätigungsmeldung zu schnell ausgeblendet.

```
[C:\Users\myalias\AppData\Local\Programs\Python\Python27\]
0 >> & .\python.exe ".\_myalias\PythonDriver.py" -h myazurepostgresql.database.windows.net -p 5432 -d postgres -U myalias@myazurepostgresql -P mySecretPassword
SUCCESS:  (1, 100, "First'row")
```


## <a name="install-pgadmin-to-inspect-your-server"></a>Installieren von pgAdmin zur Überprüfung des Servers


Wenn das Programm PythonDriverRun.ps1 beendet wird, wird es automatisch bereinigt, indem die von der Anwendung erstellte Tabelle „testpy1“ gelöscht wird. Sie können mithilfe eines „#“-Zeichens außerhalb der Quellcodezeile kommentieren, die die Anweisung **DROP TABLE** ausgibt. Bei dieser Option würde die Tabelle bestehen bleiben, damit Sie die Tabelle anschließend überprüfen können.

Mit dem Tool pgAdmin können Sie einen beliebigen PostgreSQL-Server und die Objekte auf dem Server überprüfen. Benutzer von Microsoft SQL Server oder Azure SQL-Datenbank können Ähnlichkeiten zwischen SQL Server Management Studio (SSMS) und pgAdmin feststellen.

Bei Bedarf können Sie **pgAdmin** installieren, um den Server und Ihre **testpy1**-Tabelle zu überprüfen.


#### <a name="1-install-pgadmin"></a>1. Installieren Sie pgAdmin.


Installationsanweisungen für pgAdmin finden Sie unter folgendem Link:

- [http://www.pgadmin.org/](http://www.pgadmin.org/)

Der Name der ausführbaren Datei ist möglicherweise nicht nur einfach „pgAdmin.exe“, sondern auch **pgAdmin4.exe**.

Um pgAdmin4.exe auf einem Windows-Computer auszuführen, geben Sie den folgenden Befehl in solch einem Muster in einer Befehlszeile ein:

`"C:\Program Files (x86)\pgAdmin 4\v1\runtime\pgAdmin4.exe"`


#### <a name="2-connect-pgadmin-to-your-server"></a>2. Stellen Sie eine Verbindung zwischen pgAdmin und Ihrem Server her.

 
Wenn die Benutzeroberfläche von pgAdmin angezeigt wird, navigieren Sie zum **Browser**-Bereich. Klicken Sie dann mit der rechten Maustaste auf **Server** > **Erstellen** > **Server**. Hier bedeutet *Erstellen*, dass eine *Verbindung* zu einem beliebigen vorhandenen PostgreSQL-Server, einschließlich einem Azure-Datenbank für PostgreSQL-Server, hergestellt wird.

Wenn die Verbindung hergestellt wurde, wird die Objektstruktur im **Browser**-Bereich angezeigt.


#### <a name="3-navigate-in-the-pgadmin-tree-to-your-table"></a>3. Navigieren Sie in der pgAdmin-Struktur zu Ihrer Tabelle.


Um die Tabelle anzuzeigen, erweitern Sie die Strukturelemente wie folgt:

- **Server** &gt; *[IhrServerhier]* &gt; **Datenbanken** &gt; postgres &gt; **Schemas** &gt; Öffentlich &gt; **Tabellen** &gt; testpy1

![pgAdmin mit „testpy1“-Tabelle in der Struktur](./media/connect-python/pgAdmin-postgresql-table-testpy1.jpg)


#### <a name="4-drop-the-testpy1-table"></a>4. Löschen Sie die Tabelle „testpy1“.


Klicken Sie zur endgültigen Bereinigung mit der rechten Maustaste auf den Knoten „testpy1“ und dann auf **Löschen**.


## <a name="next-steps"></a>Nächste Schritte

- [Verbindung zwischen Python und der Azure SQL-Datenbank](../sql-database/sql-database-connect-query-python.md)
- [Datenverbindungsbibliotheken für Azure-Datenbank für PostgreSQL](concepts-connection-libraries.md)


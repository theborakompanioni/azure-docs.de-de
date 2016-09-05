<properties 
	pageTitle="Erstellen von benutzerdefinierten R-Modulen in Azure Machine Learning | Microsoft Azure" 
	description="Schnellstart für das Erstellen von benutzerdefinierten R-Modulen in Azure Machine Learning." 
	services="machine-learning" 
	documentationCenter="" 
	authors="bradsev"  
	manager="paulettm" 
	editor="cgronlun" />

<tags 
	ms.service="machine-learning" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.tgt_pltfrm="na" 
	ms.workload="tbd" 
	ms.date="08/19/2016" 
	ms.author="bradsev;ankarloff" />


# Benutzerdefinierte R-Module in Azure Machine Learning  


In diesem Thema erfahren Sie, wie ein benutzerdefiniertes R-Modul erstellt und in Azure Machine Learning bereitgestellt wird. Es wird beschrieben, was ein benutzerdefiniertes R-Modul ist und welche Dateien verwendet werden, um es zu definieren. Es veranschaulicht, wie die Dateien erstellt werden, die ein Modul definieren, und wie dieses Modul zur Bereitstellung in einem Machine Learning-Arbeitsbereich registriert wird. Die in der Definition des benutzerdefinierten Moduls verwendeten Elemente und Attribute werden dann ausführlicher beschrieben. Ebenfalls wird erläutert, wie zusätzliche Funktionen und Dateien sowie mehrere Ausgaben zu verwenden sind.

[AZURE.INCLUDE [machine-learning-kostenlose-Testversion](../../includes/machine-learning-free-trial.md)]


## Was ist ein benutzerdefiniertes R-Modul?

Ein **benutzerdefiniertes Modul** ist ein vom Benutzer bestimmtes Modul, dass in Ihrem Arbeitsbereich hochgeladen und als Teil eines Azure Machine Learning Experiments ausgeführt werden kann. Ein **benutzerdefiniertes R-Modul** ist ein benutzerdefiniertes Modul, das eine vom Benutzer erstellte R-Funktion ausführt. **R** ist eine Programmiersprache für statistische Berechnungen und Grafiken, die häufig von Statistikern und Datenanalysten für die Implementierung von Algorithmen verwendet wird. Derzeit wird in benutzerdefinierten Modulen ausschließlich R unterstützt, für zukünftige Versionen sind jedoch bereits weitere Sprachen geplant.

Benutzerdefinierte Module haben in Azure Machine Learning einen **erstrangigen Status** und können somit genau wie andere Module verwendet werden. Sie können mit anderen Modulen ausgeführt sowie in veröffentlichte Experimente oder in Visualisierungen eingeschlossen werden. Sie haben die Kontrolle über den vom Modul implementierten Algorithmus, die verwendeten Eingangs- und Ausgangsports, die Parameter für die Modellierung und andere Laufzeitmerkmale. Ein Experiment mit benutzerdefinierten Modulen kann auch im Cortana Intelligence-Katalog veröffentlicht und somit komfortabel weitergegeben werden.


## Dateien in einem benutzerdefinierten R-Modul

Ein benutzerdefiniertes R-Modul wird durch eine ZIP-Datei definiert, die mindestens zwei Dateien enthält:

* Eine **Quelldatei**, die die vom Modul offengelegte R-Funktion implementiert
* Eine **XML-Definitionsdatei**, die die benutzerdefinierte Modul-Schnittstelle beschreibt

Weitere Zusatzdateien können auch in die ZIP-Datei eingeschlossen werden, die Funktionen bereitstellt, auf die vom benutzerdefinierten Modul zugegriffen werden kann. Diese Option wird im Teil **Argumente** des Referenzabschnitts **Elemente in der XML-Definitionsdatei** im Anschluss an das Schnellstartbeispiel behandelt.


## Schnellstartbeispiel: Definieren, Packen und Registrieren eines benutzerdefinierten R-Moduls

In diesem Beispiel wird veranschaulicht, wie die erforderlichen Dateien für ein benutzerdefiniertes R-Modul erstellt und in einer ZIP-Datei komprimiert werden und dann das Modul im Machine Learning-Arbeitsbereich registriert wird. Das Beispiel-ZIP-Paket und die Beispiel-Dateien können [hier](http://go.microsoft.com/fwlink/?LinkID=524916&clcid=0x409) heruntergeladen werden.

## Die Quelldatei
Denken Sie zum Beispiel an ein Modul namens **Custom Add Rows**, das die Standardimplementierung des Moduls **Add Rows** ändert, welches zum Verketten von Zeilen (Vorkommen) aus zwei Datasets (Datenrahmen) verwendet wird. Das Standardmodul **Add Rows** hängt die Zeilen des zweiten Eingabedatasets mithilfe des `rbind`-Algorithmus an das erste Eingabedataset an. Die benutzerdefinierte `CustomAddRows`-Funktion akzeptiert ebenfalls zwei Datasets, aber auch einen booleschen Swap-Parameter als zusätzliche Eingabe. Wenn der Swap-Parameter **FALSE** ist, wird das gleiche Dataset zurückgegeben wie bei der Standard-Implementierung. Ist der Swap-Parameter hingegen **TRUE**, werden stattdessen Zeilen des ersten Eingabedatasets an das zweite Dataset angehängt. Die Datei „CustomAddRows.R“, welche die Implementierung der `CustomAddRows`-R-Funktion enthält, die vom Modul **Custom Add Rows** verfügbar gemacht wird, enthält den folgenden R-Code:

	CustomAddRows <- function(dataset1, dataset2, swap=FALSE) 
	{
		if (swap)
		{
			return (rbind(dataset2, dataset1));
		}
		else
		{
			return (rbind(dataset1, dataset2));
		} 
	} 

### Die XML-Definitionsdatei
Um diese `CustomAddRows`Funktion als Azure Machine Learning-Modul offenzulegen, muss eine XML-Datei erstellt werden, um anzugeben, wie das Modul **Custom Add Rows** aussehen und sich verhalten soll.

	<!-- Defined a module using an R Script -->
	<Module name="Custom Add Rows">
	    <Owner>Microsoft Corporation</Owner>
	    <Description>Appends one dataset to another. Dataset 2 is concatenated to Dataset 1 when Swap is FALSE, and vice versa when Swap is TRUE.</Description>
	
	<!-- Specify the base language, script file and R function to use for this module. -->		
	    <Language name="R" 
		 sourceFile="CustomAddRows.R" 
		 entryPoint="CustomAddRows" />  
		
	<!-- Define module input and output ports -->
	<!-- Note: The values of the id attributes in the Input and Arg elements must match the parameter names in the R Function CustomAddRows defined in CustomAddRows.R. -->
	    <Ports>
			<Input id="dataset1" name="Dataset 1" type="DataTable">
				<Description>First input dataset</Description>
			</Input>
			<Input id="dataset2" name="Dataset 2" type="DataTable">
				<Description>Second input dataset</Description>
			</Input>
			<Output id="dataset" name="Dataset" type="DataTable">
				<Description>The combined dataset</Description>
			</Output>
	    </Ports>
		
	<!-- Define module parameters -->
	    <Arguments>
			<Arg id="swap" name="Swap" type="bool" >
				<Description>Swap input datasets.</Description>
			</Arg>
	    </Arguments>
	</Module>

 
Wichtig: Der Wert der **id**-Attribute der **Input**- und **Arg**-Elemente in der XML-Datei muss EXAKT mit den Funktionsparameternamen des R-Codes in der Datei „CustomAddRows.R“ übereinstimmen (im Beispiel: *dataset1*, *dataset2* und *swap*). Entsprechend muss der Wert des **entryPoint**-Attributs des **Language**-Elements EXAKT mit dem Namen der Funktion im R-Skript übereinstimmen (im Beispiel: *CustomAddRows*).

Das **id**-Attribut für das **Output**-Element muss hingegen keiner Variablen im R-Skript entsprechen. Sind mehrere Ausgaben erforderlich, geben Sie mithilfe der R-Funktion einfach eine Liste mit Ergebnissen zurück, und achten Sie darauf, dass die Ergebnisse *in der gleichen Reihenfolge* angegeben sind wie die in der XML-Datei deklarierten **Output**-Elemente.

### Packen und Registrieren des Moduls
Speichern Sie diese beiden Dateien als *CustomAddRows.R* und *CustomAddRows.xml*, und fassen Sie die beiden Dateien in einer ZIP-Datei namens *CustomAddRows.zip* zusammen.

Navigieren Sie in Machine Learning Studio zum Machine Learning-Arbeitsbereich, klicken Sie im unteren Bereich auf die Schaltfläche **+NEU**, und wählen Sie **MODUL > FROM ZIP PACKAGE** (AUS ZIP-PAKET) aus, um das neue Modul **Custom Add Rows** hochzuladen. Dadurch werden die Dateien in Ihrem Arbeitsbereich registriert.

![ZIP-Datei hochladen](./media/machine-learning-custom-r-modules/upload-from-zip-package.png)

Auf das Modul **Custom Add Rows** kann jetzt von Machine Learning-Experimenten aus zugegriffen werden.


## Elemente in der XML-Definitionsdatei

### Modul-Elemente
Das**Modul**-Element wird verwendet, um ein benutzerdefiniertes Modul in der XML-Datei zu definieren. Mehrere Module können in einer XML-Datei definiert werden, in der mehrere **Modul**-Elemente verwendet werden. Jedes Modul in Ihrem Arbeitsbereich muss einen eindeutigen Namen haben. Wenn Sie ein benutzerdefiniertes Modul mit dem Namen eines bereits vorhandenen benutzerdefinierten Moduls registrieren, wird das vorhandene Modul durch das neue ersetzt. Benutzerdefinierte Module können bei der Registrierung jedoch den gleichen Namen besitzen wie ein bereits vorhandenes Azure Machine Learning-Modul. In diesem Fall werden sie in der Kategorie**Benutzerdefiniert** der Modulpalette angezeigt.

	<Module name="Custom Add Rows" isDeterministic="false"> 
		<Owner>Microsoft Corporation</Owner>
		<Description>Appends one dataset to another...</Description>/> 


Innerhalb des **Module**-Elements können optional zwei zusätzliche Elemente angegeben werden:

- Ein in das Modul eingebettetes **Owner**-Element
- Ein **Description**-Element mit Text, der angezeigt wird, wenn der Benutzer die Direkthilfe für das Modul aufruft oder auf der Machine Learning-Benutzeroberfläche auf das Modul zeigt.


Regeln für Zeichenbeschränkungen in den Module-Elementen:

* Der Wert des **Namen**-Attributs im **Modul**-Element darf maximal 64 Zeichen lang sein.
* Der Inhalt des **Beschreibungs**-Elements darf 128 Zeichen nicht überschreiten.
* Der Inhalt des **Besitzer**-Elements darf 32 Zeichen nicht überschreiten.


Die Ergebnisse eines Moduls können deterministisch oder nicht deterministisch sein.** Standardmäßig werden alle Module als deterministisch betrachtet. Das bedeutet: Bei einer unveränderten Gruppe von Eingabeparametern und Daten sollte das Modul bei jeder Ausführung die gleichen Ergebnisse zurückgeben. Aufgrund dieses Verhaltens führt Azure Machine Learning Studio als deterministisch gekennzeichnete Module nur dann erneut aus, wenn sich ein Parameter oder die Eingabedaten geändert haben. Die Rückgabe der zwischengespeicherten Ergebnisse ermöglicht zudem eine schnellere Ausführung von Experimenten.

Manche Funktionen sind nicht deterministisch. Hierzu zählen etwa „RAND“ oder eine Funktion, die das aktuelle Datum oder die aktuelle Uhrzeit zurückgibt. Wenn Ihr Modul eine nicht deterministische Funktion verwendet, können Sie das optionale **isDeterministic**-Attribut auf **FALSE** festlegen, um das Modul als nicht deterministisch zu kennzeichnen. Dadurch wird das Modul bei jeder Ausführung des Experiments erneut ausgeführt, auch wenn sich die Moduleingabe und die Parameter nicht geändert haben.

### Definition des "Language"-Elements
Das**Sprachen**-Element in Ihrer XML-Definitionsdatei wird verwendet, um die Sprache des benutzerdefinierten Moduls anzugeben. R ist derzeit die einzige unterstützte Sprache. Der Wert des **SourceFile**-Attributs muss der Name der R-Datei sein, die die aufzurufende Funktion enthält, wenn das Modul ausgeführt wird. Diese Datei muss im Zip-Paket sein. Der Wert des **EntryPoint**-Attributs ist der Name der aufgerufenen Funktion und muss mit einer in der Quelldatei definierten gültigen Funktion übereinstimmen.

	<Language name="R" sourceFile="CustomAddRows.R" entryPoint="CustomAddRows" />


### Ports
Die Eingabe- und Ausgabeports für ein benutzerdefiniertes Modul sind in den untergeordneten Elementen des Abschnitts **Ports** der XML-Definitionsdatei angegeben. Die Reihenfolge dieser Elemente bestimmt das Layout (UX) für die Benutzer. Das erste untergeordnete **Input**- oder **Output**-Element, das im **Ports**-Element der XML-Datei aufgelistet wird, wird zum äußerst linken Eingabeport der Machine Learning-Benutzeroberfläche. Jeder Eingabe- und Ausgabeport kann optional über ein untergeordnetes **Description**-Element mit Text verfügen, der angezeigt wird, wenn Sie auf der Machine Learning-Benutzeroberfläche den Mauszeiger auf den Port bewegen.

**Port-Regeln**:

* Maximale Anzahl von **Eingabe- und Ausgabe-Ports** beträgt jeweils 8.

### Input-Elemente
Über Eingabeports können Sie Daten an Ihre R-Funktion und Ihren Arbeitsbereich übergeben. Die unterstützten **Datentypen** für Eingabe-Ports lauten folgendermaßen:

**DataTable:** Dieser Typ wird als data.frame an Ihre R-Funktion übergeben. Tatsächlich werden alle Typen (z. B. CSV-Dateien oder ARFF-Dateien), die von Machine Learning unterstützt werden und mit **DataTable** kompatibel sind, automatisch in eine data.frame konvertiert.

		<Input id="dataset1" name="Input 1" type="DataTable" isOptional="false">
        	<Description>Input Dataset 1</Description>
       	</Input>

Das**ID**-Attribut, das jedem **DataTable** Eingangs-Port zugeordnet ist, muss einen eindeutigen Wert haben, der mit seinem entsprechenden benannten Parameter in der R-Funktion übereinstimmen muss. Für optionale **DataTable**-Ports, die nicht als Eingabe in einem Experiment übergeben werden, wird der Wert **NULL** an die R-Funktion übergeben, und optionale ZIP-Ports werden ignoriert, wenn die Eingabe nicht verbunden ist. Das **IsOptional**-Attribut ist für die Typen **DataTable** und **Zip** optional und standardmäßig *False*.
	   
**Zip**: Benutzerdefinierte Module können ZIP-Dateien als Eingabe akzeptieren. Diese Eingabe wird im R-Arbeitsverzeichnis Ihrer Funktion entpackt.

		<Input id="zippedData" name="Zip Input" type="Zip" IsOptional="false">
        	<Description>Zip files to be extracted to the R working directory.</Description>
       	</Input>

Bei benutzerdefinierten R-Modulen muss die ID für einen ZIP-Port keinem Parameter der R-Funktion entsprechen. Der Grund: Die ZIP-Datei wird automatisch in das R-Arbeitsverzeichnis extrahiert.

**Eingabe-Regeln:**

* Der Wert des **ID**-Attributs des **Eingabe**-Elements muss ein gültiger R-Variablenname sein.
* Der Wert vom **ID**-Attribut des **Eingabe**-Elements darf 64 Zeichen nicht überschreiten.
* Der Wert des **Namen**-Attributs vom **Eingabe**-Element darf nicht länger als 64 Zeichen sein.
* Der Inhalt des **Beschreibungs**-Elements darf 128 Zeichen nicht überschreiten.
* Der Wert des **Typen**-Attributs vom **Eingabe**-Element muss *Zip* oder *DataTable* sein.
* Der Wert des **IsOptional** -Attributs vom **Eingabe**-Element ist nicht erforderlich (und standardmäßig *false*, wenn nicht angegeben); aber wenn er angegeben wird, muss er *true* oder *false* sein.

### Ausgabe-Elemente

**Standard-Ausgabe-Ports:** Ausgabe-Ports sind den Rückgabewerten Ihrer R-Funktion zugeordnet, die anschließend von nachfolgenden Modulen verwendet werden können. *DataTable* ist zurzeit der einzige unterstützte Standardtyp für Ausgabeports. (Unterstützung für *Learners* und *Transforms* in Kürze zu erwarten.) Eine *DataTable*-Ausgabe wird wie folgt definiert:

	<Output id="dataset" name="Dataset" type="DataTable">
		<Description>Combined dataset</Description>
	</Output>

Für Ausgaben in benutzerdefinierten R-Modulen muss der Wert des **ID**-Attributs mit nichts im R-Skript übereinstimmen, jedoch eindeutig sein. Für ein einzelne Modulausgabe muss der Rückgabewert der R-Funktion ein *data.frame* sein. Um mehr als ein Objekt eines unterstützten Datentyps auszugeben, müssen die entsprechende Ausgangs-Ports in der XML-Definitionsdatei angegeben und die Objekte als Liste zurückgegeben werden. Die Ausgangsobjekte werden den Ausgangsports von links nach rechts zugewiesen und entsprechen der Reihenfolge, in der die Objekte in der zurückgegebenen Liste angegeben sind.

Wenn Sie also beispielsweise das Modul **Custom Add Rows** ändern möchten, um die beiden ursprünglichen Datasets *dataset1* und *dataset2* zusammen mit dem neu verknüpften Dataset namens *dataset* auszugeben (von links nach rechts als *dataset*, *dataset1*, *dataset2*), definieren Sie die Ausgabeports in der Datei „CustomAddRows.xml“ wie folgt:

	<Ports> 
		<Output id="dataset" name="Dataset Out" type="DataTable"> 
			<Description>New Dataset</Description> 
		</Output> 
		<Output id="dataset1_out" name="Dataset 1 Out" type="DataTable"> 
			<Description>First Dataset</Description> 
		</Output> 
		<Output id="dataset2_out" name="Dataset 2 Out" type="DataTable"> 
			<Description>Second Dataset</Description> 
		</Output> 
		<Input id="dataset1" name="Dataset 1" type="DataTable"> 
			<Description>First Input Table</Description>
		</Input> 
		<Input id="dataset2" name="Dataset 2" type="DataTable"> 
			<Description>Second Input Table</Description> 
		</Input> 
	</Ports> 


Geben Sie dann die Liste der Objekte in einer Liste in der richtigen Reihenfolge in "CustomAddRows.R" zurück:

	CustomAddRows <- function(dataset1, dataset2, swap=FALSE) { 
		if (swap) { dataset <- rbind(dataset2, dataset1)) } 
		else { dataset <- rbind(dataset1, dataset2)) 
		} 
	return (list(dataset, dataset1, dataset2)) 
	} 
	
**Visualisierungsausgabe:** Sie können auch einen Ausgabeport vom Typ *Visualization* angeben, der die Ausgabe des R-Grafikgeräts und die Konsolenausgabe anzeigt. Dieser Port ist nicht Teil der R-Funktionsausgabe und hat keinerlei Auswirkungen auf die Reihenfolge der anderen Ausgabeporttypen. Um den benutzerdefinierten Modulen einen Visualisierungsport hinzuzufügen, fügen Sie ein **Ausgabe**-Element mit einem *Visualisierungswert* für sein **Typen**-Attribut hinzu.

	<Output id="deviceOutput" name="View Port" type="Visualization">
      <Description>View the R console graphics device output.</Description>
    </Output>
	
**Ausgabe-Regeln:**

* Der Wert des **ID**-Attributs vom **Ausgabe**-Element muss ein gültiger R-Variablenname sein.
* Der Wert vom **ID**-Attribut des **Ausgabe**-Elements darf 32 Zeichen nicht überschreiten.
* Der Wert des **Namen**-Attributs vom **Ausgabe**-Element darf nicht länger als 64 Zeichen sein.
* Der Wert des **Typen**-Attribut vom **Ausgabe**-Element muss *Visualisierung* sein.

### Argumente
Zusätzliche Daten können an die R-Funktion über die Modulparameter übergeben werden, die im **Argumente**-Element definiert sind. Diese Parameter werden im äußerst rechten Eigenschaftenbereich der Machine Learning-Benutzeroberfläche angezeigt, wenn das Modul ausgewählt ist. Argumente können jeder der unterstützten Typen sein oder Sie können bei Bedarf eine benutzerdefinierte Enumeration erstellen. Ähnlich wie die **Ports**-Elemente können **Argumente** -Elemente ein optionales **Beschreibungs**-Element besitzen, das den Text angibt, der angezeigt wird, wenn Sie mit der Maus auf den Namen des Parameters zeigen. Optionale Eigenschaften für ein Modul (beispielsweise „defaultValue“, „minValue“ und „maxValue“) können jedem Argument als Attribute eines **Properties**-Elements hinzugefügt werden. Welche Eigenschaften für die **Properties**-Elemente gültig sind, hängt vom jeweiligen Argumenttyp ab und wird mit den unterstützten Argumenttypen im nächsten Abschnitt beschrieben. Wie bei Eingaben und Ausgaben ist es wichtig, dass jedem Parameter eindeutige ID-Werte zugeordnet sind. In unserem Schnellstart-Beispiel war der zugeordnete ID-Parameter *swap*.

### Arg-Element
Ein Modul-Parameter wird mithilfe des untergeordneten**Arg**-Elements vom Abschnitt **Argumente** der XML-Definitionsdatei definiert. Wie bei den untergeordneten Elementen im Abschnitt **Ports** definiert die Reihenfolge der Parameter im Abschnitt **Argumente** das Layout in der UX. Die Parameter werden von oben nach unten in der Benutzeroberfläche in der gleichen Reihenfolge, in der sie in der XML-Datei definiert sind, angezeigt. Im Anschluss werden die Typen aufgeführt, die von Machine Learning für Parameter unterstützt werden.

**int** – ein Parameter des Typs Ganzzahl (32 Bit).

		<Arg id="intValue1" name="Int Param" type="int">
			<Properties min="uts, it is critical that each of the parameters have unique id values associated with them. In addition, the id values must correspond to the named parameters in your R funct0" max="100" default="0" />
			<Description>Integer Parameter</Description>
       </Arg>



* *Optionale Eigenschaften*: **min**, **max** und **Standard**

**double** – ein Parameter des Typs "double".

       <Arg id="doubleValue1" name="Double Param" type="double">
           <Properties min="0.000" max="0.999" default="0.3" />
		   <Description>Double Parameter</Description>
       </Arg>


* *Optionale Eigenschaften*: **min**, **max** und **Standard**

**bool ** – ein boolescher Parameter, der durch ein Kontrollkästchen in der UX dargestellt wird.

		<Arg id="boolValue1" name="Boolean Param" type="bool">
			<Properties default="true" />
			<Description>Boolean Parameter</Description>
		</Arg>



* *Optionale Eigenschaften*: **Standard** -false, wenn nicht festgelegt

**string**: eine Standardzeichenfolge.

        <Arg id="stringValue1" name="My string Param" type="string">
		   <Properties default="Default string value." isOptional="true" />
           <Description>String Parameter 1</Description>
        </Arg>


* *Optionale Eigenschaften*: **default** und **isOptional** – Eine optionale Zeichenfolge ohne Standardwert wird als **NULL** an die R-Funktion übergeben, wenn vom Benutzer nicht anderweitig ein Wert angegeben wird.

**ColumnPicker**: ein Spaltenauswahl-Parameter. Dieser Typ wird in der UX als Spaltenauswahl gerendert. Mit dem **Property**-Element wird hier die ID des Ports angegeben, von dem aus Spalten ausgewählt werden, wobei der Zielporttyp *DataTable* lauten muss. Das Ergebnis der Spaltenauswahl wird als Zeichenfolgenliste mit den Namen der ausgewählten Spalten an die R-Funktion übergeben.

		<Arg id="colset" name="Column set" type="ColumnPicker">	  
		  <Properties portId="datasetIn1" allowedTypes="Numeric" default="NumericAll"/>
		  <Description>Column set</Description>
		</Arg>


* *Erforderliche Eigenschaften*: **PortId** - entspricht die ID eines Eingabe-Elements mit dem Typ *DataTable*.
* *Optionale Eigenschaften*:
	* **allowedTypes**: Dient zum Filtern der zur Auswahl stehenden Spaltentypen. Folgende Werte sind gültig:
		* 	Numeric
		* 	Boolean
		* 	Kategorisch
		* 	String
		* 	Bezeichnung
		* 	Feature
		* 	Punkte
		* 	Alle

	* **Standard** - gültige Standardauswahl für die Spaltenauswahl umfassen:
		* Keine
		* NumericFeature
		* NumericLabel
		* NumericScore
		* NumericAll
		* BooleanFeature
		* BooleanLabel
		* BooleanScore
		* BooleanAll
		* CategoricalFeature
		* CategoricalLabel
		* CategoricalScore
		* CategoricalAll
		* StringFeature
		* StringLabel
		* StringScore
		* StringAll
		* AllLabel
		* AllFeature
		* AllScore
		* Alle

                            							
**DropDown**: Eine vom Benutzer angegebene aufgezählte (Dropdown-)Liste. Angabe der Dropdown-Elemente werden innerhalb des**Eigenschaften**-Elements mithilfe eines **Elementen**-Element angegeben. Die ID (**id**) für die einzelnen **Item**-Elemente muss eindeutig und eine gültige R-Variable sein. Der Name (**name**) eines **Item**-Elements fungiert sowohl als angezeigter Text als auch als Wert, der an die R-Funktion übergeben wird.

	<Arg id="color" name="Color" type="DropDown">
      <Properties default="red">
        <Item id="red" name="Red Value"/>
        <Item id="green" name="Green Value"/>
        <Item id="blue" name="Blue Value"/>
      </Properties>
      <Description>Select a color.</Description>
    </Arg>	

* *Optionale Eigenschaften*:
	* **Standard** – der Wert für die Standardeigenschaft muss mit dem ID-Wert eines der **Element**- Elemente übereinstimmen.


### Zusätzliche Dateien

Jede Datei in der ZIP-Datei des benutzerdefinierten Moduls ist während der Ausführungszeit verfügbar. Gegebenenfalls vorhandene Verzeichnisstrukturen bleiben erhalten. Das bedeutet, dass die lokale Dateierfassung auf die gleiche Weise funktioniert wie bei der Ausführung von Azure Machine Learning.

> [AZURE.NOTE] Beachten Sie, dass alle Dateien in das Verzeichnis „src“ extrahiert werden und daher alle Pfade über das Präfix „src/“ verfügen müssen.

Angenommen Sie möchten alle Zeilen mit NAs und alle doppelten Zeilen in einem DataSet entfernen, bevor Sie es an CustomAddRows ausgeben, und bereits eine R-Funktion geschrieben haben, die dies in einer Datei „RemoveDupNARows.R“ ausführt:

	RemoveDupNARows <- function(dataFrame) {
		#Remove Duplicate Rows:
		dataFrame <- unique(dataFrame)
		#Remove Rows with NAs:
		finalDataFrame <- dataFrame[complete.cases(dataFrame),]
		return(finalDataFrame)
	}
Sie können die zusätzliche Datei "RemoveDupNARows.R" in der "CustomAddRows"-Funktion bereitstellen:

	CustomAddRows <- function(dataset1, dataset2, swap=FALSE) {
		source("src/RemoveDupNARows.R")
			if (swap) { 
				dataset <- rbind(dataset2, dataset1))
	 		} else { 
	  			dataset <- rbind(dataset1, dataset2)) 
	 		} 
		dataset <- removeDupNARows(dataset)
		return (dataset)
	}

Laden Sie anschließend eine ZIP-Datei mit "CustomAddRows.R", "CustomAddRows.xml" und "RemoveDupNARows.R" als benutzerdefiniertes R-Modul hoch.


## Ausführungsumgebung

Die Ausführungsumgebung für das R-Skript verwendet die gleiche Version von R wie das Modul **Execute R Script** und kann die gleichen Standardpakete verwenden. Sie können Ihrem benutzerdefinierten Modul auch zusätzliche R-Pakete hinzufügen, indem Sie sie in das ZIP-Paket des benutzerdefinierten Moduls einschließen. Laden Sie sie einfach wie in Ihrer eigenen R-Umgebung in Ihr R-Skript.

**Einschränkungen der Ausführungsumgebung** umfassen:

* Nicht beständiges Dateisystem: Dateien, die beim Ausführen des benutzerdefinierten Moduls geschrieben werden, können nicht über mehrere Ausführungen des gleichen Moduls hinweg beibehalten werden.
* Kein Netzwerkzugriff



 

<!---HONumber=AcomDC_0824_2016-->
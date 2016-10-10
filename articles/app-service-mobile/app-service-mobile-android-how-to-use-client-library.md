<properties
	pageTitle="Verwenden der Mobile Apps-Clientbibliothek von Android"
	description="Verwenden des Android-Client-SDK für Azure Mobile Apps."
	services="app-service\mobile"
	documentationCenter="android"
	authors="RickSaling"
	manager="erikre"
	editor=""/>

<tags
	ms.service="app-service-mobile"
	ms.workload="mobile"
	ms.tgt_pltfrm="mobile-android"
	ms.devlang="java"
	ms.topic="article"
	ms.date="09/23/2016"
	ms.author="adrianha"/>


# Verwenden der Android-Clientbibliothek für Mobile Apps

[AZURE.INCLUDE [app-service-mobile-selector-client-library](../../includes/app-service-mobile-selector-client-library.md)]

Diese Anleitung beschreibt die Verwendung des Android-Client-SDK für Mobile Apps, um häufige Szenarien, wie z.B. die Folgenden zu implementieren:

- Abfragen von Daten (Einfügen, Aktualisieren und Löschen)
- Authentifizierung
- Behandeln von Fehlern
- Anpassen des Clients

Er bietet auch eine fundierte Einführung in gängigen Clientcode, der in den meisten mobilen Apps verwendet wird.

Dieser Leitfaden konzentriert sich auf die Clientseite des Android-SDK. Weitere Informationen zu den serverseitigen SDKs für mobile Apps finden Sie unter [Arbeiten mit dem .NET-Back-End-SDK][10] oder [Verwenden des Node.js-Back-End-SDKs][11].

## Referenzdokumentation

Sie finden die [Javadocs-API-Referenz][12] für die Android-Clientbibliothek auf GitHub.

## Unterstützte Plattformen

Das Azure-Android-SDK für Mobile Apps unterstützt die API-Level 19 bis 24 (KitKat bis Nougat).

Die Authentifizierung für den „Serverfluss“ verwendet eine Webansicht für die dargestellte Benutzeroberfläche. Wenn das Gerät keine Benutzeroberfläche in Form einer Webansicht darstellen kann, sind andere Authentifizierungsmethoden erforderlich, die außerhalb des Bereichs des Produkts liegen. Dieses SDK eignet sich nicht für Geräte vom Typ „Überwachung“ oder für ähnlich eingeschränkte Geräte.

## Einrichtung und Voraussetzungen

Schließen Sie das Tutorial zum [Schnellstart für Mobile Apps](app-service-mobile-android-get-started.md) ab. Diese Aufgabe stellt sicher, dass alle Voraussetzungen für die Entwicklung von Azure Mobile Apps erfüllt sind. Das Schnellstarttutorial hilft Ihnen auch dabei, Ihr Konto zu konfigurieren und Ihre erste mobile App zu erstellen.

Wenn Sie das Schnellstarttutorial nicht absolvieren möchten, führen Sie folgende Aufgaben durch:

- [Erstellen Sie ein Mobile App-Back-End][13] zur Verwendung mit Ihrer Android-App.
- [Aktualisieren Sie die Gradle-Builddateien](#gradle-build) in Android Studio.
- [Aktivieren Sie die Internetberechtigung](#enable-internet).

###<a name="gradle-build"></a>Aktualisieren der Gradle-Builddatei

Ändern Sie beide **build.gradle**-Dateien:

1. Fügen Sie diesen Code in die *Project*-Ebene der **build.gradle**-Datei im *buildscript*-Tag ein:

		buildscript {
		    repositories {
		        jcenter()
		    }
		}

2. Fügen Sie diesen Code in die *Module app*-Ebene der **build.gradle**-Datei im *dependencies*-Tag ein:

		compile 'com.microsoft.azure:azure-mobile-android:3.1.0'

    Derzeit ist 3.1.0 die neueste Version. Die unterstützten Versionen sind [hier][14] aufgeführt.

###<a name="enable-internet"></a>Aktivieren der Internetberechtigung

Für den Zugriff auf Azure muss für Ihre App die INTERNET-Berechtigung aktiviert sein. Wenn sie nicht bereits aktiviert ist, fügen Sie Ihrer Datei **AndroidManifest.xml** die folgende Codezeile hinzu:

	<uses-permission android:name="android.permission.INTERNET" />

## Die fundierte Einführung

Dieser Abschnitt behandelt einen Teil des Codes in der Schnellstart-App, die sich auf die Verwendung von Azure Mobile Apps bezieht.

###<a name="data-object"></a>Definieren von Clientdatenklassen

Für den Zugriff auf Daten aus SQL Azure-Tabellen definieren Sie Clientdatenklassen, die den Tabellen im Mobile App-Back-End entsprechen. Für die Beispiele in diesem Thema wird eine Tabelle mit dem Namen **ToDoItem** verwendet, die folgende Spalten hat:

- id
- Text
- complete

Das entsprechend typisierte clientseitige Objekt sieht wie folgt aus:

	public class ToDoItem {
		private String id;
		private String text;
		private Boolean complete;
	}

Der Code ist in einer Datei namens **ToDoItem.java** gespeichert.

Wenn Ihre SQL Azure-Tabelle weitere Spalten enthält, würden Sie die entsprechenden Felder dieser Klasse hinzufügen. Wenn das Datenübertragungsobjekt (Data Transfer Object, DTO) z.B. eine Integerspalte „Priority“ enthielte, könnten Sie dieses Feld zusammen mit seinen Getter- und Setter-Methoden hinzufügen:

	private Integer priority;

	/**
	* Returns the item priority
	*/
	public Integer getPriority() {
	    return mPriority;
	}
	
	/**
	* Sets the item priority
	*
	* @param priority
	*            priority to set
	*/
	public final void setPriority(Integer priority) {
	    mPriority = priority;
	}

Informationen zum Erstellen zusätzlicher Tabellen in Ihrem Mobile Apps-Back-End finden Sie unter [Gewusst wie: Definieren eines Tabellencontrollers][15] (.NET-Back-End) oder [Definieren von Tabellen mit einem dynamischen Schema][16] (Node.js-Back-End). Für ein Node.js-Back-End können Sie auch die Einstellung **Einfache Tabellen** im [Azure-Portal] verwenden.

###<a name="create-client"></a>Gewusst wie: Erstellen des Clientkontexts

Der folgende Code erstellt das **MobileServiceClient**-Objekt, das für den Zugriff auf Ihr Mobile App-Back-End verwendet wird. Der Code gehört in die `onCreate`-Methode der **Activity**-Klasse, die in *AndroidManifest.xml* als **MAIN**-Aktion und **LAUNCHER**-Kategorie angegeben ist. Im Schnellstartcode gehört er in die Datei **ToDoActivity.java**.

		MobileServiceClient mClient = new MobileServiceClient(
			"MobileAppUrl", // Replace with the Site URL
			this)

Ersetzen Sie in diesem Code `MobileAppUrl` durch die URL des Mobile App-Back-Ends. Sie finden die URL im [Azure-Portal] auf dem Blatt für das Mobile App-Back-End. Zum Kompilieren dieser Codezeile müssen Sie auch folgende **import**-Anweisung hinzufügen:

	import com.microsoft.windowsazure.mobileservices.*;

###<a name="instantiating"></a>Erstellen von Tabellenverweisen

Der einfachste Weg, Daten im Back-End abzufragen oder zu ändern, ist das *typisierte Programmiermodell*, da Java eine stark typisierte Sprache ist. Dieses Modell ermöglicht eine nahtlose JSON-Serialisierung und -Deserialisierung mithilfe der [gson][3]-Bibliothek, wenn Daten zwischen Clientobjekten und Tabellen in der Azure SQL-Datenbankinstanz im Back-End gesendet werden.

Um auf eine Tabelle zuzugreifen, erstellen Sie zuerst ein [MobileServiceTable][8]-Objekt durch Aufrufen der **getTable**-Methode im [MobileServiceClient][9]. Diese Methode verfügt über zwei Überladungen:

	public class MobileServiceClient {
	    public <E> MobileServiceTable<E> getTable(Class<E> clazz);
	    public <E> MobileServiceTable<E> getTable(String name, Class<E> clazz);
	}

Im folgenden Code ist **mClient** ein Verweis auf Ihr MobileServiceClient-Objekt. Die erste Überladung wird verwendet, wenn Klassenname und Tabellenname gleich sind und dieser Name im Schnellstartcode verwendet wird:

	MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable(ToDoItem.class);

Die zweite Überladung wird verwendet, wenn sich Tabellenname und Klassenname unterscheiden: Der erste Parameter ist der Tabellenname.

	MobileServiceTable<ToDoItem> mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

###<a name="binding"></a>Gewusst wie: Datenbindung in der Benutzeroberfläche

Datenbindung besteht aus drei Komponenten:

- Die Datenquelle
- Das Bildschirmlayout
- Der Adapter, der diese beiden Komponenten verbindet.

In unserem Beispielcode geben wir die Daten aus der Mobile Apps-SQL Azure-Tabelle **ToDoItem** in ein Array zurück. Diese Aktivität ist ein typisches Muster für Datenanwendungen. Datenbankabfragen geben häufig eine Sammlung von Zeilen zurück, die der Client in Form einer Liste oder eines Arrays erhält. In diesem Beispiel ist das Array die Datenquelle.

Der Code gibt ein Bildschirmlayout an, das die Ansicht der auf dem Gerät angezeigten Daten definiert. Diese beiden Komponenten sind über einen Adapter verbunden, der in diesem Code eine Erweiterung der **ArrayAdapter&lt;ToDoItem&gt;**-Klasse ist.

#### <a name="layout"></a>Definieren des Layouts

Das Layout wird durch mehrere XML-Codeabschnitte definiert. In einem existierenden Layout stellt der folgende Code die **ListView** dar, die wir mit unseren Serverdaten auffüllen möchten.

    <ListView
        android:id="@+id/listViewToDo"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        tools:listitem="@layout/row_list_to_do" >
    </ListView>

Das *listitem*-Attribut im obigen Code definiert die ID des Layouts für eine bestimmte Zeile in der Liste. Dieser Code gibt ein Kontrollkästchen und den zugehörigen Text an und wird einmal für jedes Element in der Liste instanziiert. In diesem Layout wird das Feld **id** nicht angezeigt, und bei einem komplexeren Layout würden zusätzliche Felder angezeigt. Dieser Code befindet sich in der Datei **row\_list\_to\_do.xml**.

	<?xml version="1.0" encoding="utf-8"?>
	<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
	    android:layout_width="match_parent"
	    android:layout_height="match_parent"
	    android:orientation="horizontal">
	    <CheckBox
	        android:id="@+id/checkToDoItem"
	        android:layout_width="wrap_content"
	        android:layout_height="wrap_content"
	        android:text="@string/checkbox_text" />
	</LinearLayout>


#### <a name="adapter"></a>Definieren des Adapters

Da die Datenquelle in unserer Ansicht ein **ToDoItem**-Array ist, leiten wir die Unterklasse für unseren Adapter von der **ArrayAdapter&lt;ToDoItem&gt;**-Klasse ab. Diese Unterklasse produziert eine Ansicht für jedes **ToDoItem** und verwendet dabei das **row\_list\_to\_do**-Layout.

In unserem Code definieren wir die folgende Klasse als Erweiterung der **ArrayAdapter&lt;E&gt;**-Klasse:

	public class ToDoItemAdapter extends ArrayAdapter<ToDoItem> {

Überschreiben Sie die **getView**-Methode der Klasse. Beispiel:

    @Override
	public View getView(int position, View convertView, ViewGroup parent) {
		View row = convertView;

		final ToDoItem currentItem = getItem(position);

		if (row == null) {
			LayoutInflater inflater = ((Activity) mContext).getLayoutInflater();
			row = inflater.inflate(R.layout.row_list_to_do, parent, false);
		}

		row.setTag(currentItem);

		final CheckBox checkBox = (CheckBox) row.findViewById(R.id.checkToDoItem);
		checkBox.setText(currentItem.getText());
		checkBox.setChecked(false);
		checkBox.setEnabled(true);

        checkBox.setOnClickListener(new View.OnClickListener() {

            @Override
            public void onClick(View arg0) {
                if (checkBox.isChecked()) {
                    checkBox.setEnabled(false);
                    if (mContext instanceof ToDoActivity) {
                        ToDoActivity activity = (ToDoActivity) mContext;
                        activity.checkItem(currentItem);
                    }
                }
            }
        });


		return row;
	}

Wir erstellen eine Instanz dieser Klasse in unserer Aktivität wie folgt:

	ToDoItemAdapter mAdapter;
	mAdapter = new ToDoItemAdapter(this, R.layout.row_list_to_do);

Der zweite Parameter für den ToDoItemAdapter-Konstruktor ist ein Verweis auf das Layout. Jetzt können wir die **ListView** instanziieren und den Adapter der **ListView** zuweisen.

	ListView listViewToDo = (ListView) findViewById(R.id.listViewToDo);
	listViewToDo.setAdapter(mAdapter);

### <a name="api"></a>Die API-Struktur

Tabellenvorgänge für Mobile Apps und benutzerdefinierte API-Aufrufe sind asynchron. Verwenden Sie die Objekte [Future] und [AsyncTask] für die asynchronen Methoden zum Abfragen, Einfügen, Aktualisieren und Löschen. Die Verwendung von Future-Objekten erleichtert die Ausführung mehrerer Vorgänge in einem Hintergrundthread, ohne sich um mehrere geschachtelte Rückrufe kümmern zu müssen.

Sehen Sie sich im [Azure-Portal] als Beispiel die Datei **ToDoActivity.java** im Android-Schnellstartprojekt an.

#### <a name="use-adapter"></a>Verwenden des Adapters

Sie sind nun in der Lage, die Datenbindung zu verwenden. Der folgende Code zeigt, wie Elemente in der Tabelle abgerufen werden und der lokale Adapter mit den zurückgegebenen Elementen gefüllt wird.

    public void showAll(View view) {
        AsyncTask<Void, Void, Void> task = new AsyncTask<Void, Void, Void>(){
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final List<ToDoItem> results = mToDoTable.execute().get();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (ToDoItem item : results) {
                                mAdapter.add(item);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        };
		runAsyncTask(task);
    }

Rufen Sie den Adapter bei jeder Änderung der **ToDoItem**-Tabelle auf. Da Änderungen in einzelnen Datensätzen erfolgen, verarbeiten Sie eine einzelne Zeile anstatt einer Sammlung. Beim Einfügen von Elementen rufen Sie die **add**-Methode des Adapters auf, beim Löschen rufen Sie die **remove**-Methode auf.

##<a name="querying"></a>Abfragen von Daten aus Ihrem Mobile App-Back-End

Dieser Abschnitt beschreibt, wie Sie Abfragen an Ihr Mobile App-Back-End stellen können. Dies umfasst folgende Aufgaben:

- [Zurückgeben aller Elemente]
- [Zurückgegebene Daten filtern]
- [Zurückgegebene Daten sortieren]
- [Daten seitenweise zurückgeben]
- [Bestimmte Spalten auswählen]
- [Verketten von Abfragemethoden](#chaining)

### <a name="showAll"></a>Zurückgeben aller Elemente einer Tabelle

Die folgende Abfrage gibt alle Elemente aus der **ToDoItem**-Tabelle zurück.

	List<ToDoItem> results = mToDoTable.execute().get();

Die Variable *results* gibt das Resultset der Abfrage als Liste zurück.

### <a name="filtering"></a>Gewusst wie: Filtern zurückgegebener Daten

Bei der Ausführung der folgenden Abfrage werden alle Elemente aus der **ToDoItem**-Tabelle zurückgegeben, wobei **complete** gleich **false** ist.

	List<ToDoItem> result = mToDoTable.where()
								.field("complete").eq(false)
								.execute().get();

**mToDoTable** ist der Verweis auf die Tabelle im mobilen Dienst, die wir zuvor erstellt haben.

Definieren Sie einen Filter mit einem **where**-Methodenaufruf des Tabellenverweises. Der **where**-Methode folgt eine **field**-Methode, der wiederum eine Methode folgt, die das logische Prädikat angibt. Mögliche Prädikatmethoden sind **eq** (equals, ist gleich), **ne** (not equal, ist ungleich), **gt** (greater than, größer als), **ge** (greater than or equal to, größer als oder gleich), **lt** (less than, kleiner als) oder **le** (less than or equal to, kleiner als oder gleich). Mit diesen Methoden können Sie die numerischen und Zeichenfolgenfelder mit bestimmten Werten vergleichen.

Sie können nach Daten filtern. Mit den folgenden Methoden können Sie das gesamte Datumsfeld oder Teile des Datums vergleichen: **year**, **month**, **day**, **hour**, **minute** und **second**. Im folgenden Beispiel wird ein Filter für Elemente hinzugefügt, deren *Fälligkeitsdatum* 2013 entspricht.

	mToDoTable.where().year("due").eq(2013).execute().get();

Die folgenden Methoden unterstützen komplexe Filter für Zeichenfolgenfelder: **startsWith**, **endsWith**, **concat**, **subString**, **indexOf**, **replace**, **toLower**, **toUpper**, **trim** und **length**. Das folgende Beispiel filtert nach Tabellenzeilen, deren *text*-Spalte mit „PRI0“ beginnt.

	mToDoTable.where().startsWith("text", "PRI0").execute().get();

Die folgenden Operatormethoden werden von Zahlenfeldern unterstützt: **add**, **sub**, **mul**, **div**, **mod**, **floor**, **ceiling** und **round**. Das folgende Beispiel filtert nach Tabellenzeilen, in denen **duration** eine gerade Zahl ist.

	mToDoTable.where().field("duration").mod(2).eq(0).execute().get();

Sie können Prädikate mit diesen logischen Methoden verknüpfen: **and**, **or** und **not**. Im folgenden Beispiel werden zwei der obigen Beispiele kombiniert.

	mToDoTable.where().year("due").eq(2013).and().startsWith("text", "PRI0")
				.execute().get();

Gruppieren und Schachteln von logischen Operatoren:

	mToDoTable.where()
				.year("due").eq(2013)
					.and
				(startsWith("text", "PRI0").or().field("duration").gt(10))
				.execute().get();

Eine detailliertere Besprechung und weitere Beispiele für Filter finden Sie unter [Exploring the richness of the Android client query model](http://hashtagfail.com/post/46493261719/mobile-services-android-querying) (Untersuchen der umfassenden Möglichkeiten des Android-Clientabfragemodells).

### <a name="sorting"></a>Gewusst wie: Sortieren zurückgegebener Daten

Der folgende Code gibt alle Elemente aus einer **ToDoItems**-Tabelle in aufsteigender Reihenfolge nach dem *text*-Feld sortiert zurück. *mToDoTable* ist der Verweis auf die zuvor erstellte Back-End-Tabelle:

	mToDoTable.orderBy("text", QueryOrder.Ascending).execute().get();

Der erste Parameter der **orderBy**-Methode ist eine Zeichenfolge mit dem Namen des Felds, nach dem sortiert werden soll. Für den zweiten Parameter wird die Aufzählung **QueryOrder** verwendet, um anzugeben, ob auf- oder absteigend sortiert werden soll. Beim Filtern mit der ***where***-Methode muss die ***where***-Methode vor dem Aufruf der ***orderBy***-Methode aufgerufen werden.

### <a name="paging"></a>Seitenweises Zurückgeben von Daten

Das erste Beispiel zeigt, wie Sie die ersten fünf Elemente einer Tabelle abrufen können. Die Abfrage gibt die Elemente aus einer **ToDoItems**-Tabelle zurück. **mToDoTable** ist der Verweis auf die zuvor erstellte Back-End-Tabelle:

    List<ToDoItem> result = mToDoTable.top(5).execute().get();


Hier ist eine Abfrage, welche die ersten fünf Elemente überspringt und die nächsten fünf zurückgibt:

	mToDoTable.skip(5).top(5).execute().get();

### <a name="selecting"></a>Gewusst wie: Auswählen bestimmter Spalten

Der folgende Code veranschaulicht, wie alle Elemente aus einer **ToDoItems**-Tabelle zurückgegeben werden, zeigt aber nur das **complete**-Feld und das **text**-Feld an. **mToDoTable** ist der Verweis auf die zuvor erstellte Back-End-Tabelle.

	List<ToDoItemNarrow> result = mToDoTable.select("complete", "text").execute().get();

Die Parameter der select-Funktion sind in diesem Fall die Zeichenfolgennamen der Tabellenspalten, die Sie zurückgeben möchten.

Die **select**-Methode muss Methoden wie **where** und **orderBy** folgen. Auf sie können Pagingmethoden wie **top** folgen.

### <a name="chaining"></a>Verketten von Abfragemethoden

Die beim Abfragen von Back-End-Tabellen verwendeten Methoden können verkettet werden. Durch die Verkettung von Abfragemethoden können Sie spezielle Spalten gefilterter Zeilen mit Sortierung und Paging abfragen. Sie können komplexe logische Filter erstellen. Jede Abfragemethode gibt ein Query-Objekt zurück. Um die Methodenserie zu beenden und die Abfrage auszuführen, rufen Sie die **execute**-Methode auf. Beispiel:

	mToDoTable.where()
        .year("due").eq(2013)
		.and().startsWith("text", "PRI0")
		.or().field("duration").gt(10)
		.orderBy(duration, QueryOrder.Ascending)
        .select("id", "complete", "text", "duration")
        .top(20)
		.execute().get();

Die verketteten Abfragemethoden müssen wie folgt sortiert werden:

1. Filtermethoden (**where**).
2. Sortiermethoden (**orderBy**).
3. Auswahlmethoden (**select**).
4. Pagingmethoden (**skip** und **top**).

##<a name="inserting"></a>Einfügen von Daten in das Back-End

Instanziieren Sie eine Instanz der *ToDoItem*-Klasse und legen deren Eigenschaften fest.

	ToDoItem item = new ToDoItem();
	item.text = "Test Program";
	item.complete = false;

Verwenden Sie dann **insert()**, um ein Objekt einzufügen:

	ToDoItem entity = mToDoTable.insert(item).get();

Die zurückgegebene Entität entspricht den in die Back-End-Tabelle eingefügten Daten, die ID und alle anderen für das Back-End festgelegten Werte inbegriffen.

Mobile Apps-Tabellen erfordern eine Primärschlüsselspalte namens **id**. Diese Spalte ist standardmäßig eine Zeichenfolge. Der Standardwert der ID-Spalte ist eine GUID. Sie können auch andere eindeutige Werte bereitstellen, beispielsweise E-Mail-Adressen oder Benutzernamen. Wenn für einen eingefügten Datensatz kein Zeichenfolgen-ID-Wert angegeben ist, generiert das Back-End eine neue GUID.

Zeichenfolgen-ID-Werte bieten folgende Vorteile:

+ IDs können ohne Roundtrip zur Datenbank generiert werden.
+ Datensätze aus unterschiedlichen Tabellen oder Datenbanken lassen sich leichter zusammenführen.
+ ID-Werte lassen sich leichter in die Anwendungslogik integrieren.

Zeichenfolgen-ID-Werte sind für die Unterstützung der Offlinesynchronisierung **ERFORDERLICH**.

##<a name="updating"></a>Aktualisieren von Daten in einer mobilen App

Um Daten in einer Tabelle zu aktualisieren, übergeben Sie das neue Objekt an die **update()**-Methode.

    mToDoTable.update(item).get();

In diesem Beispiel ist *item* ein Verweis auf eine Zeile in der *ToDoItem*-Tabelle, an der einige Änderungen vorgenommen wurden. Die Zeile mit dem gleichen **id**-Wert wird aktualisiert.

##<a name="deleting"></a>Löschen von Daten in einer mobilen App

Der folgende Code zeigt, wie Sie Daten durch Angabe des Datenobjekts aus einer Tabelle löschen.

	mToDoTable.delete(item);

Sie können ein Element auch durch Angabe des **id**-Felds der zu löschenden Zeile löschen.

	String myRowId = "2FA404AB-E458-44CD-BC1B-3BC847EF0902";
   	mToDoTable.delete(myRowId);

##<a name="lookup"></a>Gewusst wie: Abfragen bestimmter Elemente

Mit der **lookUp()**-Methode suchen Sie nach einem Element mit einem bestimmten **id**-Feld:

	ToDoItem result = mToDoTable
						.lookUp("0380BAFB-BCFF-443C-B7D5-30199F730335")
						.get();

##<a name="untyped"></a>Gewusst wie: Arbeiten mit nicht typisierten Daten

Das Programmiermodell ohne Typisierung ermöglicht Ihnen eine genaue Steuerung der JSON-Serialisierung. Es gibt einige häufige Szenarien, in denen der Einsatz eines nicht typisierten Programmiermodells wünschenswert ist. Beispiel: Ihre Back-End-Tabelle enthält viele Spalten, und Sie müssen nur auf eine Teilmenge der Spalten verweisen. Im typisierten Modell müssen Sie alle Spalten der Tabelle in Ihrer mobilen App in Ihrer Datenklasse definieren.

Die meisten API-Aufrufe für den Datenzugriff sind gleich wie beim typisierten Programmiermodell. Im untypisierten Modell rufen Sie Methoden des **MobileServiceJsonTable**-Objekts anstelle des **MobileServiceTable**-Objekts auf.

### <a name="json_instance"></a>Erstellen einer Instanz einer nicht typisierten Tabelle

Ähnlich wie beim typisierten Modell benötigen Sie zunächst einen Tabellenverweis. In diesem Fall handelt es sich jedoch um ein **MobileServicesJsonTable**-Objekt. Rufen Sie den Verweis durch Aufruf der **getTable**-Methode in einer Instanz des Clients ab:

    private MobileServiceJsonTable mJsonToDoTable;
	//...
    mJsonToDoTable = mClient.getTable("ToDoItem");

Nachdem Sie eine Instanz der **MobileServiceJsonTable** erstellt haben, steht dieser Instanz praktisch die gleiche API zur Verfügung wie beim typisierten Programmiermodell. In einigen Fällen akzeptieren die Methoden einen nicht typisierten Parameter anstelle eines typisierten Parameters.

### <a name="json_insert"></a>Einfügen in nicht typisierte Tabellen

Der folgende Code zeigt, wie Sie Elemente einfügen können. Sie müssen zunächst ein [JsonObject][1] erstellen, das Teil der [gson][3]-Bibliothek ist.

	JsonObject jsonItem = new JsonObject();
	jsonItem.addProperty("text", "Wake up");
	jsonItem.addProperty("complete", false);

Verwenden Sie anschließend **insert()**, um das nicht typisierte Objekt in die Tabelle einzufügen.

    mJsonToDoTable.insert(jsonItem).get();

Wenn Sie die ID des eingefügten Objekts abrufen müssen, verwenden Sie die **getAsJsonPrimitive()**-Methode.

	jsonItem.getAsJsonPrimitive("id").getAsInt());

### <a name="json_delete"></a>Löschen aus einer nicht typisierten Tabelle

Der folgende Code zeigt, wie Sie eine Instanz löschen, in diesem Fall die Instanz des **JsonObject**, die Sie im vorigen *insert*-Beispiel eingefügt haben. Der Code ist derselbe wie im typisierten Fall, aber die Methode weist eine andere Signatur auf, da sie auf ein **JsonObject** verweist.

         mToDoTable.delete(jsonItem);

Sie können eine Instanz auch direkt anhand ihrer ID löschen:

		 mToDoTable.delete(ID);

### <a name="json_get"></a>Zurückgeben aller Zeilen aus einer nicht typisierten Tabelle

Der folgende Code ruft eine gesamte Tabelle ab. Da Sie eine JSON-Tabelle verwenden, können Sie nur einige der Spalten der Tabelle selektiv abrufen.

    public void showAllUntyped(View view) {
        new AsyncTask<Void, Void, Void>() {
            @Override
            protected Void doInBackground(Void... params) {
                try {
                    final JsonElement result = mJsonToDoTable.execute().get();
                    final JsonArray results = result.getAsJsonArray();
                    runOnUiThread(new Runnable() {

                        @Override
                        public void run() {
                            mAdapter.clear();
                            for (JsonElement item : results) {
                                String ID = item.getAsJsonObject().getAsJsonPrimitive("id").getAsString();
                                String mText = item.getAsJsonObject().getAsJsonPrimitive("text").getAsString();
                                Boolean mComplete = item.getAsJsonObject().getAsJsonPrimitive("complete").getAsBoolean();
                                ToDoItem mToDoItem = new ToDoItem();
                                mToDoItem.setId(ID);
                                mToDoItem.setText(mText);
                                mToDoItem.setComplete(mComplete);
                                mAdapter.add(mToDoItem);
                            }
                        }
                    });
                } catch (Exception exception) {
                    createAndShowDialog(exception, "Error");
                }
                return null;
            }
        }.execute();
    }

Für das nicht typisierte Modell steht der gleiche Satz an Filter- sowie Filter- und Pagingmethoden zur Verfügung wie für das typisierte Modell.

##<a name="custom-api"></a>Gewusst wie: Aufrufen einer benutzerdefinierten API

Mit einer benutzerdefinierten API können Sie benutzerdefinierte Endpunkte definieren, die Serverfunktionen zur Verfügung stellen, welche keinem Einfüge-, Aktualisierungs-, Lösch- oder Lesevorgang zugeordnet sind. Durch die Verwendung einer benutzerdefinierten API erhalten Sie mehr Kontrolle über das Messaging, einschließlich Lesen und Einstellen der HTTP-Nachrichten-Header sowie Definieren eines von JSON abweichenden Nachrichtentextformats.

Rufen Sie von einem Android-Client aus die **invokeApi**-Methode auf, um den benutzerdefinierten API-Endpunkt aufzurufen. Das folgende Beispiel zeigt, wie Sie einen API-Endpunkt mit dem Namen **completeAll** aufrufen, der eine Sammlungsklasse mit dem Namen **MarkAllResult** zurückgibt.

	public void completeItem(View view) {

	    ListenableFuture<MarkAllResult> result = mClient.invokeApi( "completeAll", MarkAllResult.class );

	    	Futures.addCallback(result, new FutureCallback<MarkAllResult>() {
	    		@Override
	    		public void onFailure(Throwable exc) {
	    			createAndShowDialog((Exception) exc, "Error");
	    		}

	    		@Override
	    		public void onSuccess(MarkAllResult result) {
	    			createAndShowDialog(result.getCount() + " item(s) marked as complete.", "Completed Items");
	                refreshItemsFromTable();
	    		}
	    	});
	    }

Die **invokeApi**-Methode wird beim Client aufgerufen, der eine POST-Anfrage an die neue benutzerdefinierte API sendet. Das von der benutzerdefinierten API zurückgegebene Ergebnis wird in einem Meldungsdialogfeld angezeigt, ebenso wie jegliche Fehler. Mit anderen Versionen von **invokeApi** können Sie optional ein Objekt im Anforderungshauptteil senden, die HTTP-Methode angeben und Abfrageparameter mit der Anforderung senden. Nicht typisierte Versionen von **invokeApi** werden ebenfalls bereitgestellt.

##<a name="authentication"></a>Hinzufügen von Authentifizierung zur App

Es stehen bereits Tutorials zur Verfügung, in denen das Hinzufügen dieser Features im Detail beschrieben wird.

App Service unterstützt die [Authentifizierung von App-Benutzern](app-service-mobile-android-get-started-users.md) mit einer Vielzahl externer Identitätsanbieter: Facebook, Google, Microsoft-Konto, Twitter und Azure Active Directory. Sie können Berechtigungen für Tabellen vergeben, um den Zugriff auf bestimmte Operationen auf authentifizierte Benutzer zu beschränken. Außerdem können Sie die Identität authentifizierter Benutzer verwenden, um Autorisierungsregeln auf Ihrem Back-End zu implementieren.

Insgesamt werden zwei Authentifizierungsflüsse unterstützt: ein **Serverfluss** und ein **Clientfluss**. Der Serverfluss bietet die einfachste Authentifizierungsform, da in diesem Fall die Webschnittstelle des Identitätsanbieters verwendet wird. Zum Implementieren der Serverflussauthentifizierung sind keine weiteren SDKs erforderlich. Die Serverflussauthentifizierung stellt keine enge Integration in das mobile Gerät bereit und wird nur für PoC-Szenarien (Proof of Concept) empfohlen.

Der Clientfluss ermöglicht eine engere Integration in gerätespezifische Funktionen wie z.B. die einmalige Anmeldung, da in diesem Fall vom Identitätsanbieter bereitgestellte SDKs verwendet werden. Sie können z.B. das Facebook-SDK in Ihre mobile Anwendung integrieren. Der mobile Client wechselt zur Facebook-App und bestätigt Ihre Anmeldung, bevor er wieder zu Ihrer mobilen App zurückwechselt.

Zum Aktivieren der Authentifizierung in Ihrer App sind vier Schritte erforderlich:

- Registrieren Sie Ihre App zur Authentifizierung bei einem Identitätsanbieter.
- Konfigurieren Sie das App Service-Back-End.
- Schränken Sie die Tabellenberechtigungen ausschließlich auf authentifizierte Benutzer im App Service-Back-End ein.
- Fügen Sie Ihrer App Authentifizierungscode hinzu.

Sie können Berechtigungen für Tabellen vergeben, um den Zugriff auf bestimmte Operationen auf authentifizierte Benutzer zu beschränken. Außerdem können Sie die SID eines authentifizierten Benutzers verwenden, um Anfragen zu verändern. Weitere Informationen finden Sie unter [Erste Schritte mit Authentifizierungen] und in der Dokumentation des Server-SDK.

### <a name="caching"></a>Hinzufügen des Authentifizierungscodes zur App

Der folgende Code startet einen Serverfluss-Anmeldeprozess mit dem Google-Anbieter:

	MobileServiceUser user = mClient.login(MobileServiceAuthenticationProvider.Google);

Rufen Sie die ID des angemeldeten Benutzers mit der **getUserId**-Methode aus einem **MobileServiceUser** ab. Ein Beispiel zur Verwendung von „Futures“ zum Abrufen der asynchronen APIs für die Anmeldung finden Sie unter [Erste Schritte mit Authentifizierungen].

### <a name="caching"></a>Zwischenspeichern von Authentifizierungstokens

Beim Zwischenspeichern von Authentifizierungstoken müssen Benutzer-ID und Authentifizierungstoken lokal auf dem Gerät gespeichert werden. Beim nächsten Start der App prüfen Sie den Cache. Wenn diese Werte vorhanden sind, können Sie die Anmeldeprozedur überspringen und den Client mit den vorhandenen Daten füllen. Diese Daten sind jedoch vertraulich und sollten aus Sicherheitsgründen verschlüsselt gespeichert werden, falls das Telefon gestohlen wird.

Ein vollständiges Beispiel zum Zwischenspeichern von Authentifizierungstoken finden Sie im Abschnitt [Zwischenspeichern von Authentifizierungstokens][7].

Wenn Sie versuchen, ein abgelaufenes Token zu verwenden, erhalten Sie die Antwort *401 – nicht autorisiert*. Mithilfe von Filtern können Sie Authentifizierungsfehler behandeln. Filter fangen Anforderungen an das App Service-Back-End ab. Der Filtercode prüft Antworten auf 401-Fehler, löst den Anmeldeprozess aus und setzt anschließend die Anforderung fort, die den 401-Fehler ausgelöst hatte.

## <a name="adal"></a>Gewusst wie: Authentifizieren von Benutzern mit der Active Directory-Authentifizierungsbibliothek

Nutzen Sie die Active Directory-Authentifizierungsbibliothek (Active Directory Authentication Library, ADAL), um Benutzer mithilfe von Azure Active Directory bei Ihrer Anwendung anzumelden. Aufgrund der besseren Bedienbarkeit und der Möglichkeit, zusätzliche Anpassungen vorzunehmen, wird eine Clientflussanmeldung den `loginAsync()`-Methoden häufig vorgezogen.

1. Konfigurieren Sie Ihr mobiles App-Back-End für die AAD-Anmeldung, indem Sie die im Tutorial [So konfigurieren Sie Ihre App Service-Anwendung zur Verwendung der Azure Active Directory-Anmeldung](app-service-mobile-how-to-configure-active-directory-authentication.md) beschriebenen Schritte ausführen. Schließen Sie auch den optionalen Schritt zur Registrierung einer nativen Clientanwendung ab.

2. Installieren Sie ADAL, indem Sie die Datei „build.gradle“ so ändern, dass sie folgende Definitionen enthält:

```
repositories {
    mavenCentral()
    flatDir {
        dirs 'libs'
    }
    maven {
        url "YourLocalMavenRepoPath\\.m2\\repository"
    }
}
packagingOptions {
    exclude 'META-INF/MSFTSIG.RSA'
    exclude 'META-INF/MSFTSIG.SF'
}
dependencies {
    compile fileTree(dir: 'libs', include: ['*.jar'])
    compile('com.microsoft.aad:adal:1.1.1') {
        exclude group: 'com.android.support'
    } // Recent version is 1.1.1
    compile 'com.android.support:support-v4:23.0.0'
}
```

3. Fügen Sie Ihrer Anwendung den unten stehenden Code hinzu, und nehmen Sie die folgenden Ersetzungen vor:

* Ersetzen Sie **INSERT-AUTHORITY-HERE** durch den Namen des Mandanten, in dem Sie Ihre Anwendung bereitgestellt haben. Das Format sollte https://login.windows.net/contoso.onmicrosoft.com entsprechen. Sie können diesen Wert von der Registerkarte „Domäne“ in Ihrer Azure Active Directory-Instanz im [klassischen Azure-Portal] kopieren.

* Ersetzen Sie **INSERT-RESOURCE-ID-HERE** durch die Client-ID für Ihr mobiles App-Back-End. Sie können die Client-ID im Portal auf der Registerkarte **Erweitert** unter **Azure Active Directory-Einstellungen** abrufen.

* Ersetzen Sie **INSERT-CLIENT-ID-HERE** durch die Client-ID, die Sie aus der nativen Clientanwendung kopiert haben.

* Ersetzen Sie mithilfe des HTTPS-Schemas **INSERT-REDIRECT-URI-HERE** durch den Endpunkt _/.auth/login/done_ Ihrer Website. Dieser Wert sollte etwa so aussehen: \_https://contoso.azurewebsites.net/.auth/login/done_.

		private AuthenticationContext mContext;

		private void authenticate() {
            String authority = "INSERT-AUTHORITY-HERE";
            String resourceId = "INSERT-RESOURCE-ID-HERE";
            String clientId = "INSERT-CLIENT-ID-HERE";
            String redirectUri = "INSERT-REDIRECT-URI-HERE";
            try {
                mContext = new AuthenticationContext(this, authority, true);
                mContext.acquireToken(this, resourceId, clientId, redirectUri, PromptBehavior.Auto, "", callback);
            } catch (Exception exc) {
                exc.printStackTrace();
            }
		}

		private AuthenticationCallback<AuthenticationResult> callback = new AuthenticationCallback<AuthenticationResult>() {
            @Override
            public void onError(Exception exc) {
                if (exc instanceof AuthenticationException) {
                    Log.d(TAG, "Cancelled");
                } else {
                    Log.d(TAG, "Authentication error:" + exc.getMessage());
                }
            }

            @Override
			public void onSuccess(AuthenticationResult result) {
                if (result == null || result.getAccessToken() == null
                        || result.getAccessToken().isEmpty()) {
                    Log.d(TAG, "Token is empty");
                } else {
                    try {
                        JSONObject payload = new JSONObject();
                        payload.put("access_token", result.getAccessToken());
                        ListenableFuture<MobileServiceUser> mLogin = mClient.login("aad", payload.toString());
                        Futures.addCallback(mLogin, new FutureCallback<MobileServiceUser>() {
                            @Override
                            public void onFailure(Throwable exc) {
                                exc.printStackTrace();
                            }
                            @Override
                            public void onSuccess(MobileServiceUser user) {
                                Log.d(TAG, "Login Complete");
                            }
                        });
                    }
                    catch (Exception exc){
                        Log.d(TAG, "Authentication error:" + exc.getMessage());
                    }
                }
            }
		};

		@Override
		protected void onActivityResult(int requestCode, int resultCode, Intent data) {
            super.onActivityResult(requestCode, resultCode, data);
            if (mContext != null) {
                mContext.onActivityResult(requestCode, resultCode, data);
            }
		}

## Hinzufügen von Pushbenachrichtigungen zur App

In der [Übersicht][6] erfahren Sie, wie Microsoft Azure Notification Hubs eine Vielzahl von Pushbenachrichtigungen unterstützt. In [diesem Tutorial][5] wird bei jedem Einfügen eines Datensatzes eine Pushbenachrichtigung an alle Geräte gesendet.

## Hinzufügen von Offlinesynchronisierung zu Ihrer App

Das Schnellstarttutorial enthält Code, der Offlinesynchronisierung implementiert. Suchen Sie nach Code, dem Kommentare vorangestellt sind:

	// Offline Sync

Indem Sie die Auskommentierung der folgenden Codezeilen aufheben, können Sie Offlinesynchronisierung implementieren, und Sie können mit ähnlichem Code den Code anderer Mobile Apps ergänzen.

##<a name="customizing"></a>Gewusst wie: Anpassen des Clients

Sie haben mehrere Möglichkeiten, das Standardverhalten des Clients anzupassen.

### <a name="headers"></a>Gewusst wie: Anpassen der Anforderungsheader

Konfigurieren Sie einen **ServiceFilter**, um jeder Anforderung einen benutzerdefinierten HTTP-Header hinzuzufügen:

	private class CustomHeaderFilter implements ServiceFilter {

        @Override
        public ListenableFuture<ServiceFilterResponse> handleRequest(
                	ServiceFilterRequest request,
					NextServiceFilterCallback next) {

            runOnUiThread(new Runnable() {

                @Override
                public void run() {
	        		request.addHeader("My-Header", "Value");	                }
            });

            SettableFuture<ServiceFilterResponse> result = SettableFuture.create();
            try {
                ServiceFilterResponse response = next.onNext(request).get();
                result.set(response);
            } catch (Exception exc) {
                result.setException(exc);
            }
        }

### <a name="serialization"></a>Gewusst wie: Anpassen der Serialisierung

Der Client geht davon aus, dass alle Tabellennamen, Spaltennamen und Datentypen im Back-End genau den im Client definierten Datenobjekten entsprechen. Diese Namen können sich jedoch aus verschiedensten Gründen auf Server und Client unterschieden. In Ihrem Szenario möchten Sie vielleicht eine der folgenden Arten von Anpassungen vornehmen:

- Die in der App Service-Tabelle verwendeten Spaltennamen entsprechen nicht den Namen, die Sie im Client verwenden.
- Verwenden Sie eine App Service-Tabelle, die einen anderen Namen trägt als die Klasse, die der Tabelle im Client zugeordnet ist.
- Aktivieren Sie die automatische Großschreibung von Eigenschaften.
- Fügen Sie Objekten komplexe Eigenschaften hinzu.

### <a name="columns"></a>Zuordnen unterschiedlicher Namen zwischen Client und Server

Nehmen Sie an, Ihr Java-Clientcode verwendet Namen nach Java-Standardschema für die **ToDoItem**-Objekteigenschaften, wie z.B. die folgenden Eigenschaften:

- mId
- mText
- mComplete
- mDuration

Serialisieren Sie die clientseitigen Namen zu JSON-Namen, die den Spaltennamen in der **ToDoItem**-Tabelle auf dem Server entsprechen. Der folgende Code verwendet die [gson][3]-Bibliothek, um die Eigenschaften mit Anmerkungen zu versehen:

	@com.google.gson.annotations.SerializedName("text")
	private String mText;

	@com.google.gson.annotations.SerializedName("id")
	private int mId;

	@com.google.gson.annotations.SerializedName("complete")
	private boolean mComplete;

	@com.google.gson.annotations.SerializedName("duration")
	private String mDuration;

### <a name="table"></a>Zuordnen unterschiedlicher Tabellennamen zwischen Client und Back-End

Ordnen Sie den Clienttabellennamen einem anderen Tabellennamen eines mobilen Diensts zu, indem Sie eine Überschreibung der [getTable()][4]-Methode verwenden:

	mToDoTable = mClient.getTable("ToDoItemBackup", ToDoItem.class);

### <a name="conversions"></a>Automatisieren von Tabellennamenzuordnungen

Sie können mit der [gson][3]-API eine Konvertierungsstrategie angeben, die für jede Spalte gilt. Die Android-Clientbibliothek verwendet im Hintergrund [gson][3], um Java-Objekte zu JSON-Daten zu serialisieren, bevor die Daten an Azure App Service übertragen werden. Der folgende Code verwendet die **setFieldNamingStrategy()**-Methode, um die Strategie festzulegen. Dieses Beispiel löscht für jeden Feldnamen das erste Zeichen (ein „m“) und wandelt das folgende Zeichen zu einem Kleinbuchstaben um. Die Zeichenfolge „mId“ beispielsweise wird in „id“ umgewandelt.

	client.setGsonBuilder(
	    MobileServiceClient
	    .createMobileServiceGsonBuilder()
	    .setFieldNamingStrategy(new FieldNamingStrategy() {
	        public String translateName(Field field) {
	            String name = field.getName();
	            return Character.toLowerCase(name.charAt(1))
	                + name.substring(2);
	            }
	        });

Dieser Code muss vor der Verwendung von **MobileServiceClient** ausgeführt werden.

### <a name="complex"></a>Speichern von Objekt- oder Array-Eigenschaften in einer Tabelle

Bisher wurden bei den vorliegenden Serialisierungsbeispielen primitive Datentypen wie Ganzzahlen und Zeichenfolgen verwendet. Primitive Datentypen lassen sich problemlos zu JSON serialisieren. Wenn Sie ein komplexes Objekt hinzufügen möchten, das sich nicht automatisch zu JSON serialisieren lässt, müssen Sie die JSON-Serialisierungsmethode bereitstellen. Ein Beispiel für die Bereitstellung einer benutzerdefinierten JSON-Serialisierungsmethode finden Sie im Blogeintrag [Customizing serialization using the gson library in the Mobile Services Android client][2] (Benutzerdefinierte Serialisierung mit der gson-Bibliothek im Android-Client für mobile Dienste).

<!-- Anchors. -->

[What is Mobile Services]: #what-is
[Concepts]: #concepts
[How to: Create the Mobile Services client]: #create-client
[How to: Create a table reference]: #instantiating
[The API structure]: #api
[How to: Query data from a mobile service]: #querying
[Zurückgeben aller Elemente]: #showAll
[Zurückgegebene Daten filtern]: #filtering
[Zurückgegebene Daten sortieren]: #sorting
[Daten seitenweise zurückgeben]: #paging
[Bestimmte Spalten auswählen]: #selecting
[How to: Concatenate query methods]: #chaining
[How to: Bind data to the user interface]: #binding
[How to: Define the layout]: #layout
[How to: Define the adapter]: #adapter
[How to: Use the adapter]: #use-adapter
[How to: Insert data into a mobile service]: #inserting
[How to: update data in a mobile service]: #updating
[How to: Delete data in a mobile service]: #deleting
[How to: Look up a specific item]: #lookup
[How to: Work with untyped data]: #untyped
[How to: Authenticate users]: #authentication
[Cache authentication tokens]: #caching
[How to: Handle errors]: #errors
[How to: Design unit tests]: #tests
[How to: Customize the client]: #customizing
[Customize request headers]: #headers
[Customize serialization]: #serialization
[Next Steps]: #next-steps
[Setup and Prerequisites]: #setup

<!-- Images. -->

<!-- URLs. -->
[Get started with Azure Mobile Apps]: app-service-mobile-android-get-started.md
[ASCII control codes C0 and C1]: http://en.wikipedia.org/wiki/Data_link_escape_character#C1_set
[Mobile Services SDK for Android]: http://go.microsoft.com/fwlink/p/?LinkID=717033
[Azure-Portal]: https://portal.azure.com
[Erste Schritte mit Authentifizierungen]: app-service-mobile-android-get-started-users.md
[1]: http://google-gson.googlecode.com/svn/trunk/gson/docs/javadocs/com/google/gson/JsonObject.html
[2]: http://hashtagfail.com/post/44606137082/mobile-services-android-serialization-gson
[3]: http://go.microsoft.com/fwlink/p/?LinkId=290801
[4]: http://go.microsoft.com/fwlink/p/?LinkId=296840
[5]: app-service-mobile-android-get-started-push.md
[6]: ../notification-hubs/notification-hubs-push-notification-overview.md#integration-with-app-service-mobile-apps
[7]: app-service-mobile-android-get-started-users.md#cache-tokens
[8]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/table/MobileServiceTable.html
[9]: http://azure.github.io/azure-mobile-apps-android-client/com/microsoft/windowsazure/mobileservices/MobileServiceClient.html
[10]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[11]: app-service-mobile-node-backend-how-to-use-server-sdk.md
[12]: http://azure.github.io/azure-mobile-apps-android-client/
[13]: app-service-mobile-android-get-started.md#create-a-new-azure-mobile-app-backend
[14]: http://go.microsoft.com/fwlink/p/?LinkID=717034
[15]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#how-to-define-a-table-controller
[16]: app-service-mobile-node-backend-how-to-use-server-sdk.md#TableOperations
[Future]: http://developer.android.com/reference/java/util/concurrent/Future.html
[AsyncTask]: http://developer.android.com/reference/android/os/AsyncTask.html

<!---HONumber=AcomDC_0928_2016-->
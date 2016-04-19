## Dynamischer Serviceplan

Im dynamischen Serviceplan werden Ihre mit Azure Functions erstellte Apps einer Functions App-Instanz zugewiesen. Wenn weitere Instanzen benötigt werden, werden diese dynamisch hinzugefügt. Diese Instanzen können mehrere Compute-Ressourcen umfassen und so die verfügbare Azure-Infrastruktur optimal nutzen. Darüber hinaus werden Ihre Funktionen parallel ausgeführt. Dadurch wird die Gesamtzeit, die für die Verarbeitung von Anfragen benötigt wird, minimiert. Die Ausführungszeit für jede Funktion wird in Sekunden aufaddiert und von der enthaltenden Funktions-App aggregiert. Die Kosten sind abhängig von der Anzahl von Instanzen, deren Speichergröße sowie der gesamten Ausführungszeit, gemessen in Gigabyte mal Sekunde. Dies ist eine hervorragende Möglichkeit, wenn Sie nur von Zeit zu Zeit Rechenleistung benötigen oder die Laufzeiten Ihrer Aufträge in der Regel sehr kurz sind, da Sie nur dann für Serverressourcen bezahlen, wenn diese auch tatsächlich genutzt werden.

### Speichertarif

Je nach den Bedürfnissen Ihrer Funktion müssen Sie die Speichermenge auswählen, die Sie für deren Ausführung in der Funktions-App (Funktionscontainer) benötigen. Die Optionen für die Speichergröße reichen von **128 MB bis hin zu 1536 MB**. Die ausgewählte Speichergröße entspricht dem Arbeitssatz, der von allen Funktionen benötigt wird, die Teil Ihrer Funktions-App sind. Wenn Sie für Ihren Code mehr Speicher benötigen, als Sie ausgewählt hatten, wird die Funktions-App-Instanz aufgrund von fehlendem Speicherplatz heruntergefahren.

### Skalieren

Die Azure Functions-Plattform evaluiert den Datenverkehr basierend auf den konfigurierten Triggern, um festzulegen, wann zentral hoch- oder herunterskaliert wird. Die Granularität der Skalierung ist die Funktions-App. Zentrales Hochskalieren bedeutet in diesem Fall, weitere Instanzen einer Funktions-App hinzuzufügen. Wenn sich der Datenverkehr verringert, bedeutet dies wiederum, dass die Instanzen für die Funktions-App reduziert werden. Wenn die App gar nicht mehr angefragt wird, werden die Instanzen auf Null herunterskaliert.

### Ressourcenverbrauch und Abrechnung

Im dynamischen Modus wird die Ressourcenzuweisung anders als im Standard-App Service-Plan durchgeführt.Daher unterscheidet sich auch das Verbrauchsmodell, das ein „Pay-per-Use“-Modell ermöglicht. Der Verbrauch wird pro Funktions-App gemeldet und nur für den Zeitraum, für den der Code ausgeführt wird. Er wird durch Multiplikation der Speichergröße (in GB) mit der Gesamtausführungszeit (in Sekunden) für alle Funktionen berechnet, die in dieser Funktions-App laufen. Die Einheit des Verbrauchs wird in **GB-s (Gigabyte mal Sekunde)** angegeben.

<!---HONumber=AcomDC_0406_2016-->
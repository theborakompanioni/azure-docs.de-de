## <a name="incremental-and-complete-deployments"></a>Inkrementelle und vollständige Bereitstellungen
Bei der Bereitstellung Ihrer Ressourcen geben Sie an, dass es sich bei der Bereitstellung entweder um ein inkrementelles Update oder um ein vollständiges Update handelt. Der Hauptunterschied zwischen diesen beiden Modi besteht darin, wie Resource Manager vorhandene Ressourcen in der Ressourcengruppe behandelt, die nicht in der Vorlage sind:

* Im vollständigen-Modus **löscht** Resource Manager Ressourcen, die in der Ressourcengruppe vorhanden, aber nicht in der Vorlage angegeben sind. 
* Im inkrementellen Modus lässt Resource Manager Ressourcen **unverändert**, die in der Ressourcengruppe vorhanden, aber nicht in der Vorlage angegeben sind.

In beiden Modi versucht Resource Manager, alle in der Vorlage angegebenen Ressourcen bereitstellen. Wenn die Ressource bereits in der Ressourcengruppe vorhanden ist und ihre Einstellungen unverändert sind, führt der Vorgang zu keiner Änderung. Wenn Sie die Einstellungen für eine Ressource ändern, wird die Ressource mit diesen neuen Einstellungen bereitgestellt. Wenn Sie versuchen, den Speicherort oder Typ einer vorhandenen Ressource zu aktualisieren, tritt bei der Bereitstellung ein Fehler auf. Stellen Sie stattdessen eine neue Ressource mit dem gewünschten Speicherort oder Typ bereit.

Resource Manager verwendet standardmäßig den inkrementellen Modus.

Im folgenden Szenario wird der Unterschied zwischen dem inkrementellen und dem vollständigen Modus veranschaulicht:

Die **vorhandene Ressourcengruppe** enthält Folgendes:

* Ressource A
* Ressource B
* Ressource C

Die **Vorlage** definiert Folgendes:

* Ressource A
* Ressource B
* Ressource D

Bei der Bereitstellung im **inkrementellen** Modus enthält die Ressourcengruppe Folgendes:

* Ressource A
* Ressource B
* Ressource C
* Ressource D

Bei der Bereitstellung im **vollständigen** Modus wird Ressource C gelöscht. Die Ressourcengruppe enthält Folgendes:

* Ressource A
* Ressource B
* Ressource D

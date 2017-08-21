## <a name="microsoft-open-source-code-of-conduct"></a>Microsoft Open-Source-Verhaltensregeln

Für dieses Projekt wird der [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Verhaltenskodex für Microsoft Open Source) beachtet.
Weitere Informationen finden Sie in den [häufig gestellten Fragen zum Verhaltenskodex](https://opensource.microsoft.com/codeofconduct/faq/). Sie können sich auch an [opencode@microsoft.com](mailto:opencode@microsoft.com) wenden, wenn Sie weitere Fragen oder Kommentare haben.

## <a name="contribute-to-azure-technical-documentation"></a>Mitwirken an der technischen Dokumentation von Azure
Wir freuen uns über Beiträge aus der Community (Benutzer, Kunden, Partner, MSFT-Mitarbeiter außerhalb der zentralen Azure-Produkteinheiten usw.) sowie von Mitarbeitern der zentralen Azure-Produkteinheiten. Die Art Ihres Beitrags hängt davon ab, wer Sie sind:

* **Community – kleinere Aktualisierungen**: Wenn Sie einfach nur kleinere Aktualisierungen beitragen möchten, suchen Sie den Artikel in diesem Repository, oder rufen Sie den Artikel auf [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) auf, und klicken Sie im Artikel auf den Link **Bearbeiten**, der zum GitHub-Quelltext des Artikels führt. Verwenden Sie anschließend einfach die GitHub-Benutzeroberfläche, um Ihre Aktualisierungen vorzunehmen. Alternativ können Sie eine Kopie des Repositorys anlegen und Aktualisierungen über die Kopie übermitteln.

* **Community – neue Artikel**: Wenn Sie Teil der Azure-Community sein möchten und einen neuen Artikel erstellen möchten, müssen Sie mit einem Mitarbeiter zusammenarbeiten, um diese neuen Inhalte in einer Kombination aus Arbeit im öffentlichen und privaten Repository aufzunehmen.

* **Mitarbeiter**: Wenn Sie ein technischer Redakteur, Programmmanager oder Entwickler des Produktteams für einen Azure-Dienst sind und es Ihre Aufgabe ist, zu technischen Artikeln beizutragen oder sie zu erstellen, sollten Sie das private Repository (https://github.com/MicrosoftDocs/azure-docs-pr) verwenden. Wenn Sie wesentliche Änderungen an einem vorhandenen Artikel vornehmen, Bilder hinzufügen oder austauschen oder einen neuen Artikel verfassen möchten, müssen Sie eine Kopie dieses Repositorys erstellen, Git Bash und einen Markdown-Editor installieren und einige Git-Befehle erlernen. Weitere Informationen finden Sie im [internen Handbuch für Mitwirkende](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master).


## <a name="about-your-contributions-to-azure-content"></a>Über Ihre Beiträge zu Azure-Inhalten
### <a name="minor-corrections"></a>Kleinere Korrekturen
Kleinere Korrekturen oder Klarstellungen, die Sie für die Dokumentation oder für Codebeispiele in diesem Repository einreichen, werden durch die [Nutzungsbedingungen für „docs.microsoft.com“](https://docs.microsoft.com/legal/termsofuse) abgedeckt.

### <a name="larger-submissions"></a>Größere Beiträge
Wenn Sie einen Pull Request mit neuen Inhalten oder signifikanten Änderungen an der Dokumentation und an Codebeispielen übermitteln, senden wir Ihnen einen Kommentar in GitHub, in dem wir Sie bitten, online eine Lizenzvereinbarung für Beiträge (Contribution License Agreement, CLA) zu unterzeichnen. Dies gilt, wenn Sie kein Mitarbeiter von Microsoft sind. Sie müssen das Onlineformular ausfüllen, damit wir Ihren Pull Request annehmen können.

## <a name="tools-and-setup"></a>Tools und Setup
Mitwirkende aus der Community können die GitHub-Benutzeroberfläche oder eine Kopie des Repositorys für Beiträge verwenden. Mitarbeiter sollten im [internen Handbuch für Mitwirkende](https://review.docs.microsoft.com/en-us/help/contribute/?branch=master) nach weiteren Informationen dazu suchen, wie sie zur technischen Dokumentation beitragen können.

## <a name="repository-organization"></a>Aufbau des Repositorys
Der Inhalt im Repository „azure-docs“ folgt in seiner Struktur der Dokumentation unter „https://docs.microsoft.com/azure“. Das Repository enthält zwei Stammordner:

### <a name="articles"></a>\articles
Der Ordner *\articles* enthält die Dokumentationsartikel, formatiert als Markdowndateien mit der Erweiterung *.md*. Artikel werden normalerweise nach Azure-Dienst gruppiert.

Der Ordner *\articles* enthält den Ordner *\media* für Mediendateien zu den Stammverzeichnisartikeln. Dieser enthält Unterordner mit den Bildern für den jeweiligen Artikel.  Die Dienstordner enthalten einen separaten Medienordner für die Artikel im jeweiligen Dienstordner. Die Medienordner für Bilder zu einem Artikel heißen genauso wie die Artikeldatei, aber ohne die Dateierweiterung *.md*.

### <a name="includes"></a>\includes
Sie können Abschnitte mit wiederverwendbaren Inhalten erstellen, die in einen oder mehrere Artikel eingefügt werden sollen. 

## <a name="how-to-use-markdown-to-format-your-topic"></a>Verwenden von Markdown für die Formatierung
Bei allen Artikeln in diesem Repository wird ein an GitHub angepasstes Markdown verwendet.  Hier sind zugehörige Ressourcen aufgelistet:

* [Grundlegendes zu Markdown](https://help.github.com/articles/markdown-basics/)
* [Markdown-Cheatsheet zum Ausdrucken](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)


## <a name="labels"></a>Bezeichnungen
Im öffentlichen Repository „azure-docs“ werden automatisierte Bezeichnungen Pull Requests zugewiesen, damit wir den Pull Request-Workflow besser verwalten können und damit wir Sie über den Pull Request-Zustand informieren können:

* Informationen zum CLA (Contribution License Agreement, Lizenzvereinbarung für Mitwirkende)
  * cla-not-required: Die Änderung ist relativ unbedeutend, und es ist keine Unterzeichnung eines CLA erforderlich.
  * cla-required: Der Umfang der Änderung ist relativ groß, und Sie müssen ein CLA unterzeichnen.
  * cla-signed: Der Mitwirkende hat das CLA unterzeichnet, sodass der Pull Request jetzt zur Überprüfung weitergeleitet werden kann.
* Änderung an Autor gesendet: Der Autor wurde über den ausstehenden Pull Request informiert.
* ready-to-merge: Kann von unserem Team für die Pull Request-Prüfung geprüft werden.



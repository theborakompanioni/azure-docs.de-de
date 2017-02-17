# <a name="azure-technical-documentation-contributor-guide"></a>Leitfaden für Mitwirkende bei der technischen Dokumentation zu Azure
Sie haben das GitHub-Repository mit der Quelle für die technische Azure-Dokumentation gefunden, die unter [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) veröffentlicht wird.

Dieses Repository enthält auch Hinweise dazu, wie Sie zu unserer technischen Dokumentation beitragen können. Die einzelnen Artikel dieses Leitfadens für Mitwirkende finden Sie [im Index](contributor-guide/contributor-guide-index.md).

## <a name="contribute-to-azure-documentation"></a>Mitwirken bei der Azure-Dokumentation
Vielen Dank für Ihr Interesse an der Azure-Dokumentation.

* [Möglichkeiten zur Mitwirkung](#ways-to-contribute)
* [Verhaltenskodex](#code-of-conduct)
* [Über Ihre Beiträge zu Azure-Inhalten](#about-your-contributions-to-azure-content)
* [Aufbau des Repositorys](#repository-organization)
* [Verwenden von GitHub und Git sowie dieses Repositorys](#use-github-git-and-this-repository)
* [Verwenden von Markdown für die Formatierung](#how-to-use-markdown-to-format-your-topic)
* [Weitere Ressourcen](#more-resources)
* [Index aller Artikel im Leitfaden für Mitwirkende](contributor-guide/contributor-guide-index.md) (öffnet eine neue Seite)

## <a name="ways-to-contribute"></a>Möglichkeiten zur Mitwirkung
Sie können wie folgt Updates für die [Azure-Dokumentation](https://docs.microsoft.com/azure) senden:

* Tragen Sie zu technischen Artikeln über die GitHub-Benutzeroberfläche bei. Das ist einfach. Entweder finden Sie den Artikel in diesem Repository, oder Sie besuchen die Seite mit dem Artikel unter [https://docs.microsoft.com/azure](https://docs.microsoft.com/azure) und klicken dort auf den Link, der zum GitHub-Quelltext des Artikels führt.
* Wenn Sie wesentliche Änderungen an einem vorhandenen Artikel vornehmen, Bilder hinzufügen oder austauschen oder einen neuen Artikel verfassen möchten, müssen Sie einen Fork (d.h. eine Abspaltung) dieses Repositorys erstellen, Git Bash und Markdown Pad installieren und einige Git-Befehle erlernen.

## <a name="code-of-conduct"></a>Verhaltenskodex
Für dieses Projekt wird der [Microsoft Open Source Code of Conduct](https://opensource.microsoft.com/codeofconduct/) (Verhaltenskodex für Microsoft Open Source) beachtet. Weitere Informationen finden Sie in den [häufig gestellten Fragen zum Verhaltenskodex](https://opensource.microsoft.com/codeofconduct/faq/). Sie können sich auch an [opencode@microsoft.com](mailto:opencode@microsoft.com) wenden, wenn Sie weitere Fragen oder Kommentare haben.

## <a name="about-your-contributions-to-azure-content"></a>Über Ihre Beiträge zu Azure-Inhalten
### <a name="minor-corrections"></a>Kleinere Korrekturen
Kleinere Korrekturen oder Klarstellungen, die Sie für die Dokumentation oder für Codebeispiele in diesem Repository einreichen, werden durch die [Nutzungsbedingungen für „docs.microsoft.com“](https://docs.microsoft.com/legal/termsofuse) abgedeckt.

### <a name="larger-submissions"></a>Größere Beiträge
Wenn Sie einen Pull Request mit neuen Inhalten oder signifikanten Änderungen an der Dokumentation und an Codebeispielen übermitteln, senden wir Ihnen einen Kommentar in GitHub, in dem wir Sie bitten, online eine Lizenzvereinbarung für Beiträge (Contribution License Agreement, CLA) zu unterzeichnen. Dies gilt, wenn Sie kein Mitarbeiter von Microsoft sind. Sie müssen das Onlineformular ausfüllen, damit wir Ihren Pull Request annehmen können.

## <a name="repository-organization"></a>Aufbau des Repositorys
Der Inhalt im Repository „azure-docs“ folgt in seiner Struktur der Dokumentation unter „https://docs.microsoft.com/azure“. Das Repository enthält zwei Stammordner:

### <a name="articles"></a>\articles
Der Ordner *\articles* enthält die Dokumentationsartikel, formatiert als Markdowndateien mit der Erweiterung *.md*. Artikel werden normalerweise nach Azure-Dienst gruppiert.

Für die Artikel müssen strenge Richtlinien für die Dateibenennung befolgt werden. Ausführliche Informationen hierzu finden Sie in [unserer Anleitung zur Benennung von Dateien](contributor-guide/file-names-and-locations.md).

Der Ordner *\articles* enthält den Ordner *\media* für Mediendateien zu den Stammverzeichnisartikeln. Dieser enthält Unterordner mit den Bildern für den jeweiligen Artikel.  Die Dienstordner enthalten einen separaten Medienordner für die Artikel im jeweiligen Dienstordner. Die Medienordner für Bilder zu einem Artikel heißen genauso wie die Artikeldatei, aber ohne die Dateierweiterung *.md*.

### <a name="includes"></a>\includes
Sie können Abschnitte mit wiederverwendbaren Inhalten erstellen, die in einen oder mehrere Artikel eingefügt werden sollen. Weitere Informationen finden Sie unter [Benutzerdefinierte Erweiterungen für unsere technischen Inhalte](contributor-guide/custom-markdown-extensions.md).

### <a name="markdown-templates"></a>\markdown templates
Dieser Ordner enthält unsere Markdown-Standardvorlage mit der grundlegenden Markdown-Formatierung, die Sie für einen Artikel benötigen.

### <a name="contributor-guide"></a>\contributor-guide
Dieser Ordner enthält die Artikel unseres Leitfadens für Mitwirkende.

## <a name="use-github-git-and-this-repository"></a>Verwenden von GitHub und Git sowie dieses Repositorys
Lesen Sie unter [Installieren und Einrichten der Tools zum Erstellen von Beiträgen in GitHub](contributor-guide/tools-and-setup.md), wie Sie über die GitHub-Benutzeroberfläche kleine Änderungen vornehmen oder einen Fork oder Klon des Repositorys erstellen, um größere Beiträge zu verfassen.

Sie möchten Git Bash installieren und lokal arbeiten? Dann finden Sie die Schritte zum Erstellen einer Verzweigung für die lokale Bearbeitung, zum Vornehmen von Änderungen und zum Rückschreiben der Änderungen in die Hauptverzweigung unter [Git-Befehle zum Erstellen eines neuen Artikels oder zum Aktualisieren eines vorhandenen Artikels](contributor-guide/git-commands-for-master.md).

### <a name="branches"></a>Branches
Wir empfehlen, lokale Bearbeitungsbranches zu erstellen, deren Umfang sich an den beabsichtigten Änderungen orientiert. Jeder Branch sollte auf genau ein Konzept oder einen einzelnen Artikel begrenzt sein. Dies vereinfacht den Arbeitsablauf und verringert die Wahrscheinlichkeit von Konflikten bei der Zusammenführung (Merge).  Die folgenden Vorhaben rechtfertigen vom Umfang her einen neuen Branch:

* Ein neuer Artikel (und zugehörige Bilder)
* Überarbeitung von Rechtschreibung und Grammatik in einem Artikel
* Änderung der Formatierung bei einer großen Zahl von Artikeln (z.B. neue Copyrightfußzeile)

## <a name="how-to-use-markdown-to-format-your-topic"></a>Verwenden von Markdown für die Formatierung
Bei allen Artikeln in diesem Repository wird ein an GitHub angepasstes Markdown verwendet.  Hier sind zugehörige Ressourcen aufgelistet:

* [Grundlegendes zu Markdown](https://help.github.com/articles/markdown-basics/)
* [Markdown-Cheatsheet zum Ausdrucken](./contributor-guide/media/documents/markdown-cheatsheet.pdf?raw=true)

## <a name="article-metadata"></a>Metadaten zu Artikeln
Metadaten zu Artikeln sind Voraussetzung für bestimmte Funktionen, z.B. für die Zuordnung von Autoren und Mitwirkenden, Brotkrümelnavigation und Suchmaschinenoptimierung sowie für die Berichterstellung, die von Microsoft zum Bewerten der Leistung von Inhalten eingesetzt wird. Metadaten sind also wichtig. [Hier finden Sie eine Anleitung zur korrekten Verwendung von Metadaten](contributor-guide/article-metadata.md).

### <a name="labels"></a>Bezeichnungen
Automatisierte Bezeichnungen werden Pull Requests zugewiesen, damit wir den Pull Request-Workflow besser verwalten können und damit wir Sie über den Pull Request-Zustand informieren können:

* Informationen zum CLA (Contribution License Agreement, Lizenzvereinbarung für Mitwirkende)
  * cla-not-required: Die Änderung ist relativ unbedeutend, und es ist keine Unterzeichnung eines CLA erforderlich.
  * cla-required: Der Umfang der Änderung ist relativ groß, und Sie müssen ein CLA unterzeichnen.
  * cla-signed: Der Mitwirkende hat das CLA unterzeichnet, sodass der Pull Request jetzt zur Überprüfung weitergeleitet werden kann.
* Pillar-Bezeichnungen: Bezeichnungen wie PnP, Modern Apps und TDC dienen der Kategorisierung von Pull Requests nach der internen Organisation, von der der Pull Request überprüft werden muss.
* Änderung an Autor gesendet: Der Autor wurde über den ausstehenden Pull Request informiert.

## <a name="more-resources"></a>Weitere Ressourcen
Alle Anleitungsthemen finden Sie im [Index zum Leitfaden für Mitwirkende](contributor-guide/contributor-guide-index.md).



<!--HONumber=Feb17_HO2-->



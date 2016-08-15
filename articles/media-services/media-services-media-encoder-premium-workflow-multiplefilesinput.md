<properties
	pageTitle="Verwenden mehrerer Eingabedateien und Komponenteneigenschaften mit Premium-Encoder | Microsoft Azure"
	description="In diesem Thema wird das Verwenden von „setRuntimeProperties“ zum Arbeiten mit mehreren Eingabedateien und Übergeben benutzerdefinierter Daten an den Medienprozessor „Media Encoder Premium Workflow“ erläutert."
	services="media-services"
	documentationCenter=""
	authors="xpouyat"
	manager="erikre"
	editor=""/>

<tags
	ms.service="media-services"
	ms.workload="media"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="07/12/2016"  
	ms.author="xpouyat;anilmur;juliako"/>

# Verwenden mehrerer Eingabedateien und Komponenteneigenschaften mit Premium-Encoder

## Übersicht

Es gibt Szenarien, in denen Sie Komponenteneigenschaften möglicherweise anpassen, den XML-Inhalt von Cliplisten angeben oder mehrere Eingabedateien senden müssen, wenn Sie eine Aufgabe mit dem Medienprozessor **Media Encoder Premium Workflow** übermitteln. Hier einige Beispiele:

- Überlagern von Video mit Text und Festlegen des Textwerts (z.B. das aktuelle Datum) zur Laufzeit für jedes Eingabevideo.
- Anpassen des XML-Codes von Cliplisten (zum Angeben einer oder mehrerer Quelldateien, einer Kürzung usw.).
- Überlagern des Eingabevideos mit einem Logobild bei der Codierung des Videos.

Um den **Media Encoder Premium Workflow** zu informieren, dass Sie einige Eigenschaften im Workflow beim Erstellen des Tasks oder Senden mehrerer Eingabedateien ändern, müssen Sie eine Konfigurationszeichenfolge verwenden, die **setRuntimeProperties** und/oder **transcodeSource** enthält. In diesem Thema werden diese beiden Konfigurationselemente erläutert.


## Syntax der Konfigurationszeichenfolge

Für die im Codierungstask festzulegende Konfigurationszeichenfolge wird ein XML-Dokument wie das folgende verwendet:

    <?xml version="1.0" encoding="utf-8"?>
    <transcodeRequest>
      <transcodeSource>
      </transcodeSource>
      <setRuntimeProperties>
        <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
      </setRuntimeProperties>
    </transcodeRequest>


Der folgende C#-Code liest die XML-Konfiguration aus einer Datei und übergibt sie der Aufgabe in einem Auftrag:

    XDocument configurationXml = XDocument.Load(xmlFileName);
    IJob job = _context.Jobs.CreateWithSingleTask(
                                                  "Media Encoder Premium Workflow",
                                                  configurationXml.ToString(),
                                                  myAsset,
                                                  "Output asset",
                                                  AssetCreationOptions.None);


## Anpassen von Komponenteneigenschaften  

### Eigenschaft mit einem einfachen Wert
In einigen Fällen ist es sinnvoll, eine Komponenteneigenschaft zusammen mit der Workflowdatei anzupassen, die im Media Encoder Premium Workflow ausgeführt wird.

Angenommen, Sie haben einen Workflow entworfen, der Ihre Videos mit Text überlagert, und der Text (z.B. das aktuelle Datum) soll zur Laufzeit festgelegt werden. Senden Sie hierzu im Codierungstask den für die Texteigenschaft der Überlagerungskomponente als neuen Wert festzulegenden Text. Sie können diesen Mechanismus verwenden, um andere Eigenschaften einer Komponente im Workflow zu ändern (z.B. Position oder Farbe der Überlagerung, Bitrate des AVC-Encoders usw.).

**setRuntimeProperties** dient zum Überschreiben einer Eigenschaft in den Komponenten des Workflows.

Beispiel:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Optional Overlay/Overlay/filename" value="MyLogo.png"/>
          <property propertyPath="Optional Text Overlay/Text To Image Converter/text" value="Today is Friday the 13th of May, 2016"/>
      </setRuntimeProperties>
    </transcodeRequest>


### Eigenschaft mit einem XML-Wert

Zum Festlegen einer Eigenschaft, die einen XML-Wert erwartet, nehmen Sie mithilfe von `<![CDATA[ and ]]>` eine Kapselung vor.

Beispiel:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

>[AZURE.NOTE]Setzen Sie hinter `<![CDATA[` kein Wagenrücklaufzeichen.


### Wert von „propertyPath“

In den vorherigen Beispielen hatte „propertyPath“ den Wert „/Media File Input/filename“ oder „/inactiveTimeout“ oder „clipListXml“. Allgemein folgt also auf den Namen der Komponente der Name der Eigenschaft. Der Pfad kann mehr oder weniger Ebenen haben, wie z.B. „/primarySourceFile“ (da sich die Eigenschaft am Stamm des Workflows befindet) oder „/Video Processing/Graphic Overlay/Opacity“ (da sich die Überlagerung in einer Gruppe befindet).

Um den Pfad und die Eigenschaft zu überprüfen, klicken Sie auf die Aktionsschaltfläche unmittelbar neben jeder Eigenschaft. Sie können auf diese Aktionsschaltfläche klicken und **Bearbeiten** auswählen. So wird der tatsächliche Namen der Eigenschaft und unmittelbar darüber der Namespace angezeigt.

![Aktion/Bearbeiten](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture6_actionedit.png)

![Eigenschaft](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture7_viewproperty.png)

## Mehrere Eingabedateien

Jeder Task, den Sie an den **Media Encoder Premium Workflow** übermitteln, erfordert zwei Objekte:

- Das erste ist ein *Workflowobjekt*, das eine Workflowdatei enthält. Sie können Workflowdateien mit dem [Workflow-Designer](media-services-workflow-designer.md) entwerfen.
- Das zweite ist ein *Medienobjekt*, das die Mediendatei(en) enthält, die Sie codieren möchten.

Beim Senden mehrerer Mediendateien an den Encoder **Media Encoder Premium Workflow** gelten die folgenden Einschränkungen:

- Alle Mediendateien müssen im gleichen *Medienobjekt* enthalten sein. Das Verwenden mehrerer Medienobjekte wird nicht unterstützt.
- Sie müssen die primäre Datei in diesem Medienobjekt festlegen (im Idealfall ist dies die Hauptvideodatei, die der Encoder verarbeiten soll).
- Konfigurationsdaten, die das Element **setRuntimeProperties** und/oder **transcodeSource** enthalten, müssen an den Prozessor übergeben werden.
  - **setRuntimeProperties** dient zum Überschreiben des Dateinamens oder einer anderen Eigenschaft in den Komponenten des Workflows.
  - **transcodeSource** dient zum Angeben des XML-Inhalts der Clipliste.

Verbindungen im Workflow:

 - Wenn Sie eine oder mehrere „Media File Input“-Komponenten verwenden und **setRuntimeProperties** zum Angeben des Dateinamens nutzen möchten, dürfen Sie diese nicht mit dem Pin der primären Dateikomponente verbinden. Stellen Sie sicher, dass es keine Verbindung zwischen dem primären Dateiobjekt und „Media File Input“-Komponenten gibt.
 - Wenn Sie lieber mit Cliplisten-XML-Code und einer Medienquellenkomponente arbeiten, können Sie beide verbinden.

![Keine Verbindung der primären Quelldatei mit der Mediendateieingabe](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture0_nopin.png)

*Es gibt keine Verbindung zwischen der primären Datei und „Media File Input“-Komponenten, wenn Sie setRuntimeProperties verwenden, um die Dateinameneigenschaft festzulegen.*

![Verbindung zwischen Cliplisten-XML-Code und Cliplistenquelle](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture1_pincliplist.png)

*Sie können Cliplisten-XML-Code mit der Medienquelle verbinden und transcodeSource verwenden.*


### Anpassung des Cliplisten-XML-Codes
Sie können den Cliplisten-XML-Code im Workflow mithilfe von **sourceTranscode** im XML-Code der Konfigurationszeichenfolge zur Laufzeit angeben. Hierfür muss der Pin „Cliplisten-XML-Code“ mit der Komponente „Medienquelle“ im Workflow verbunden werden.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <transcodeSource>
          <clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>video-part1.mp4</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
          </clipList>
        </transcodeSource>
        <setRuntimeProperties>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Wenn Sie angeben möchten, dass „/primarySourceFile“ diese Eigenschaft zum Benennen der Ausgabedateien mithilfe von „Ausdrücken“ verwendet, sollten Sie den Cliplisten-XML-Code als Eigenschaft *hinter* der „/primarySourceFile“-Eigenschaft angeben. Dies soll verhindern, dass die Clipliste von der „/primarySourceFile“-Einstellung überschrieben wird.

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="c:\temp\start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
                <mediaFile>
                  <file>c:\temp\start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

Mit zusätzlicher framegenauer Kürzung:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="/primarySourceFile" value="start.mxf" />
          <property propertyPath="/inactiveTimeout" value="65" />
          <property propertyPath="clipListXml" value="xxx">
          <extendedValue><![CDATA[<clipList>
            <clip>
              <videoSource>
                <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </videoSource>
              <audioSource>
               <trim>
                  <inPoint fps="25">00:00:05:24</inPoint>
                  <outPoint fps="25">00:00:10:24</outPoint>
                </trim>
                <mediaFile>
                  <file>start.mxf</file>
                </mediaFile>
              </audioSource>
            </clip>
            <primaryClipIndex>0</primaryClipIndex>
            </clipList>]]>
          </extendedValue>
          </property>
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


## Beispiel

Nehmen wir ein Beispiel, bei dem Sie das Eingabevideo bei der Codierung mit einem Logobild überlagern möchten. In diesem Beispiel heißen das Eingabevideo „MyInputVideo.mp4“ und das Logo „MyLogo.png“. Führen Sie die folgenden Schritte aus:

- Erstellen eines Workflowobjekts mit der Workflowdatei (siehe folgendes Beispiel).
- Erstellen eines Medienobjekts, das zwei Dateien enthält: „MyInputVideo.mp4“ als primäre Datei und „MyLogo.png“.
- Senden eines Tasks zum Medienprozessor „Media Encoder Premium Workflow“ mit den zuvor genannten Eingabemedienobjekten und Angeben der folgenden Konfigurationszeichenfolge.

Konfiguration:

    <?xml version="1.0" encoding="utf-8"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="MyInputVideo.mp4" />
          <property propertyPath="/primarySourceFile" value="MyInputVideo.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="MyLogo.png" />
        </setRuntimeProperties>
      </transcodeRequest>


Im obigen Beispiel wird der Name der Videodatei an die „Media File Input“-Komponente und die Eigenschaft primarySourceFile gesendet. Der Name der Logodatei wird an eine andere „Media File Input“-Komponente gesendet, die mit der Grafiküberlagerungskomponente verbunden ist.

>[AZURE.NOTE]Der Name der Videodatei wird an die primarySourceFile-Eigenschaft übergeben. Der Grund dafür ist die Verwendung dieser Eigenschaft im Workflow, um z.B. mithilfe von Ausdrücken den ordnungsgemäßen Ausgabedateinamen zu erstellen.


### Schritte zum Erstellen eines Workflows zum Überlagern des Videos mit einem Logo     

Es folgen die Schritte zum Erstellen eines Workflows, der zwei Dateien als Eingabe verwendet: ein Video und ein Bild. Das Bild soll das Video überlagern.

Öffnen Sie **Workflow-Designer**, und wählen Sie **Datei** > **Neuer Arbeitsbereich** > **Blaupause transcodieren** aus.

Der neue Workflow enthält drei Elemente:

- Primäre Quelldatei
- Cliplisten-XML
- Ausgabedatei/-medienobjekt

![Neuer Workflow für Codierung](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture9_empty.png)

*Neuer Workflow für Codierung*


Wir beginnen mit dem Hinzufügen einer „Media File Input“-Komponente, damit unsere Eingabemediendatei akzeptiert wird. Um dem Workflow eine Komponente hinzuzufügen, suchen Sie im Repository-Suchfeld danach und ziehen den gewünschten Eintrag in den Designerbereich.

Fügen Sie als Nächstes die Datei hinzu, die zum Entwerfen Ihres Workflows verwendet werden soll. Klicken Sie hierzu auf den Hintergrundbereich des Workflow-Designers, und suchen Sie im rechten Eigenschaftsbereich die Option „Primäre Quelldatei“. Klicken Sie auf das Ordnersymbol, und wählen Sie die entsprechende Videodatei aus.

![Primäre Dateiquelle](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture10_primaryfile.png)

*Primäre Dateiquelle*


Geben Sie die Datei anschließend im Feld „Mediendateieingabe“ ein.

![Eingabequelle für Mediendatei](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture11_mediafileinput.png)

*Eingabequelle für Mediendatei*


Nachdem dieser Vorgang abgeschlossen ist, untersucht die „Media File Input“-Komponente die Datei und füllt ihre Ausgabepins auf, um die untersuchte Datei widerzuspiegeln.

Im nächsten Schritt wird ein „Videodatentyp-Updater“ zum Angeben des Farbraums als „Rec.709“ hinzugefügt. Fügen Sie einen „Videoformatkonverter“ hinzu, der auf „Datenlayout/Layouttyp = Konfigurierbar planar“ festgelegt ist. Dadurch werden der Videostream in ein Format konvertiert, das als Quelle für die Überlagerungkomponente verwendet werden kann.

![Videodatentyp-Updater und Formatkonverter](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter.png)

*Videodatentyp-Updater und Formatkonverter*

![Layouttyp = Konfigurierbar planar](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture12_formatconverter2.png)

*Layouttyp ist „Konfigurierbar planar“*

Als Nächstes fügen Sie eine Videoüberlagerungskomponente hinzu und verbinden den Pin „Nicht komprimiertes Video“ mit dem Pin „Nicht komprimiertes Video“ der Medieneingabedatei.

Fügen Sie eine andere „Media File Input“-Komponente (zum Laden der Logodatei) hinzu, klicken Sie auf diese Komponente, und benennen Sie sie in „Media File Input Logo“ um. Wählen Sie über die Dateieigenschaft ein Bild (z.B. eine PNG-Datei) aus. Verbinden Sie den Pin „Nicht komprimiertes Bild“ mit dem Pin „Nicht komprimiertes Bild“ der Überlagerung.

![Überlagerungskomponente und Bilddateiquelle](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture13_overlay.png)

*Überlagerungskomponente und Bilddateiquelle*


Wenn Sie die Position des Logos für das Video ändern möchten (es z.B. lieber 10% entfernt von der linken oberen Ecke des Videos positionieren möchten), deaktivieren Sie das Kontrollkästchen „Manuelle Eingabe“. Dies ist möglich, da Sie eine „Media File Input“-Komponente verwenden, um der Überlagerungskomponente die Logodatei anzugeben.

![Position der Überlagerung](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture14_overlay_position.png)

*Position der Überlagerung*


Fügen Sie zum Codieren des Videodatenstroms in H.264 der Designeroberfläche die Komponenten „AVC-Videoencoder“ und „AAC-Encoder“ hinzu. Verbinden Sie die Pins. Richten Sie den AAC-Encoder ein, und wählen Sie: Audioformatkonvertierung/Voreinstellung: 2.0 (L, R).

![Audio- und Videoencoder](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture15_encoders.png)

*Audio- und Videoencoder*


Fügen Sie nun die Komponenten **ISO Mpeg-4 Multiplexer** und **Dateiausgabe** hinzu, und verbinden Sie die Pins wie gezeigt.

![MP4-Multiplexer und Dateiausgabe](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture16_mp4output.png)

*MP4-Multiplexer und Dateiausgabe*


Sie müssen den Namen der Ausgabedatei festlegen. Klicken Sie auf die Komponente **Dateiausgabe**, und bearbeiten Sie den Ausdruck für die Datei:

    ${ROOT_outputWriteDirectory}\${ROOT_sourceFileBaseName}_withoverlay.mp4

![Dateiausgabename](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture17_filenameoutput.png)

*Dateiausgabename*

Sie können den Workflow lokal ausführen, um sicherzustellen, dass er ordnungsgemäß ausgeführt wird.

Anschließend können Sie ihn in Azure Media Services ausführen.

Bereiten Sie zuerst in Azure Media Services ein Medienobjekt vor, das die Videodatei und das Logo enthält. Dazu können Sie .NET oder REST-API verwenden. Sie können hierzu auch das Azure-Portal oder den [Azure Media Services-Explorer](https://github.com/Azure/Azure-Media-Services-Explorer) (AMSE) nutzen.

Dieses Tutorial zeigt Ihnen, wie Medienobjekte mit dem AMSE verwaltet werden. Es gibt zwei Möglichkeiten, einem Medienobjekt Dateien hinzuzufügen:

- Erstellen Sie einen lokalen Ordner, in den Sie die beiden Dateien kopieren, und legen Sie ihn dann per Drag&Drop auf der Registerkarte **Medienobjekt** ab.
- Laden Sie die Videodatei als Medienobjekt hoch. Zeigen Sie anschließend die Informationen zum Medienobjekt an, wechseln Sie zur Registerkarte „Dateien“, und laden Sie eine zusätzliche Datei (mit dem Logo) hoch.

>[AZURE.NOTE]Stellen Sie sicher, dass sich eine primäre Datei im Medienobjekt (der Hauptvideodatei) befindet.

![Medienobjektdateien im AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture18_assetinamse.png)

*Medienobjektdateien im AMSE*


Wählen Sie das Medienobjekt aus, und codieren Sie es mit dem Premium-Encoder. Laden Sie den Workflow hoch, und wählen Sie ihn aus.

Klicken Sie auf die Schaltfläche, um Daten an den Prozessor zu übergeben, und fügen Sie zum Festlegen der Laufzeiteigenschaften den folgenden XML-Code hinzu:

![Premium-Encoder im AMSE](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture19_amsepremium.png)

*Premium-Encoder im AMSE*


Fügen Sie dann die folgenden XML-Daten ein. Sie müssen für „Media File Input“ und „primarySourceFile“ den Namen der Videodatei angeben. Geben Sie auch den Namen der Datei mit dem Logo an.

    <?xml version="1.0" encoding="utf-16"?>
      <transcodeRequest>
        <setRuntimeProperties>
          <property propertyPath="Media File Input/filename" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="/primarySourceFile" value="Microsoft_HoloLens_Possibilities_816p24.mp4" />
          <property propertyPath="Media File Input Logo/filename" value="logo.png" />
        </setRuntimeProperties>
      </transcodeRequest>

![setRuntimeProperties](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture20_amsexmldata.png)

*setRuntimeProperties*


Bei Verwendung des .NET SDK zum Erstellen und Ausführen des Tasks müssen diese XML-Daten als Konfigurationszeichenfolge übergeben werden.

    public ITask AddNew(string taskName, IMediaProcessor mediaProcessor, string configuration, TaskOptions options);

Nach Abschluss des Auftrags wird die MP4-Datei im Ausgabemedienobjekt mit der Überlagerung angezeigt!

![Überlagerung für das Video](./media/media-services-media-encoder-premium-workflow-multiplefilesinput/capture21_resultoverlay.png)

*Überlagerung für das Video*


Sie können den Beispielworkflow von [GitHub](https://github.com/Azure/azure-media-services-samples/tree/master/Encoding%20Presets/VoD/MediaEncoderPremiumWorkfows/) herunterladen.


## Weitere Informationen

- [Introducing Premium Encoding in Azure Media Services (in englischer Sprache)](http://azure.microsoft.com/blog/2015/03/05/introducing-premium-encoding-in-azure-media-services)

- [How to Use Premium Encoding in Azure Media Services](http://azure.microsoft.com/blog/2015/03/06/how-to-use-premium-encoding-in-azure-media-services) (Verwenden von Premium Encoding in Azure Media Services)

- [Codieren von On-Demand-Inhalten mit Azure Media Services](media-services-encode-asset.md#media_encoder_premium_workflow)

- [Media Encoder Premium Workflow-Formate und -Codecs](media-services-premium-workflow-encoder-formats.md)

- [Workflow-Beispieldateien](https://github.com/AzureMediaServicesSamples/Encoding-Presets/tree/master/VoD/MediaEncoderPremiumWorkfows)

- [Azure Media Services Explorer](http://aka.ms/amse)

## Media Services-Lernpfade

[AZURE.INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## Feedback geben

[AZURE.INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]

<!---HONumber=AcomDC_0803_2016-->
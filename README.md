# La guida di Quartz ai dati problematici

**Una guida completa ai problemi riscontrati nei dati dal mondo reale insieme a suggerimenti su come risolverli.**

Come reporter il tuo mondo è pieno di dati. E quei dati sono pieni di problemi. Questa guida porta soluzioni a molti dei tipici problemi che incontrerai quando lavorerai con i dati.

La maggior parte di questi problemi possono essere risolti. Alcuni di questi non possono essere risolti e ciò significa che non dovresti usare i dati. Altri ancora non possono essere risolti ma, con attenzione, è possibile continuare a utilizzare i dati. Per lavorare con queste situazioni, questa guida è organizzata da chi è meglio attrezzato per risolvere il problema: tu, la tua fonte, un esperto, ecc. Nella descrizione di ogni problema puoi trovare suggerimenti su cosa fare se quella persona non può aiutarti.

Non è possibile riesaminare tutti i set di dati riscontrati per tutti questi problemi. Se provi a farlo, non pubblicherai mai nulla. Tuttavia, acquisendo familiarità con i tipi di problemi più comuni, si avranno maggiori possibilità di identificare prima un problema.

L'autore originale della guida può essere contattato qui: [Chris](mailto:chrisgroskopf@gmail.com).

Questo lavoro è concesso in licenza [Creative Commons Attribution-NonCommercial 4.0 International License](http://creativecommons.org/licenses/by-nc/4.0/). Send your pull requests!

# Traduzioni

* [Cinese](http://djchina.org/2016/07/12/bad_data_guide/) (completa)
* [Cinese](http://cn.gijn.org/2016/01/10/quartz%E5%9D%8F%E6%95%B0%E6%8D%AE%E6%8C%87%E5%8D%97%E7%B2%BE%E9%80%89%EF%BC%9A%E5%A4%84%E7%90%86%E6%95%B0%E6%8D%AE%E7%9A%84%E6%AD%A3%E7%A1%AE%E6%96%B9%E5%BC%8F%E4%B8%80%E8%A7%88/) (partial)
* [Giapponese](https://github.com/piyo-ko/bad-data-guide/blob/master/README_ja.md)
* [Portoghese](http://escoladedados.org/2016/09/08/guia-quartz-para-limpeza-de-dados/)
* [Spagnolo](http://es.schoolofdata.org/guia-quartz/)
* [Italiano]() (parziale)

# Indice

## Problemi nella sorgente

* [Valori mancanti](#values-are-missing)
* [Zero sostituiscono valori mancanti](#zeros-replace-missing-values)
* [Mancano dati che sai che dovrebbero esserci](#data-are-missing-you-know-should-be-there)
* [Righe o valori duplicati](#rows-or-values-are-duplicated)
* [Spelling non consistente](#spelling-is-inconsistent)
* [Name order is inconsistent](#name-order-is-inconsistent)
* [Formati data incoerenti](#date-formats-are-inconsistent)
* [Unità non specificate](#units-are-not-specified)
* [Categories are badly chosen](#categories-are-badly-chosen)
* [Nomi dei campi ambigui](#field-names-are-ambiguous)
* [Provenienza non documentata](#provenance-is-not-documented)
* [Valori sospetti presenti](#suspicious-values-are-present)
* [Data are too coarse](#data-are-too-coarse)
* [Totals differ from published aggregates](#totals-differ-from-published-aggregates)
* [Il foglio ha 65536 righe](#spreadsheet-has-65536-rows)
* [Il foglio ha 255 colonne](#spreadsheet-has-255-columns)
* [Il foglio ha date nel 1900, 1904, 1969 o 1970](#spreadsheet-has-dates-in-1900-1904-1969-or-1970)
* [Testo convertito in numeri](#text-has-been-converted-to-numbers)
* [Numeri salvati come testo](#numbers-have-been-stored-as-text)

## Problemi che dovresti risolvere

* [Text is garbled](#text-is-garbled)
* [Line endings are garbled](#line-endings-are-garbled)
* [I dati sono in un PDF](#data-are-in-a-pdf)
* [Dati troppo granulari](#data-are-too-granular)
* [Dati inseriti a mano](#data-were-entered-by-humans)
* [Data are intermingled with formatting and annotations](#data-are-intermingled-with-formatting-and-annotations)
* [Aggregations were computed on missing values](#aggregations-were-computed-on-missing-values)
* [Sample is not random](#sample-is-not-random)
* [Margin-of-error is too large](#margin-of-error-is-too-large)
* [Margin-of-error is unknown](#margin-of-error-is-unknown)
* [Sample is biased](#sample-is-biased)
* [Data have been manually edited](#data-have-been-manually-edited)
* [Inflation skews the data](#inflation-skews-the-data)
* [Natural/seasonal variation skews the data](#naturalseasonal-variation-skews-the-data)
* [Timeframe has been manipulated](#timeframe-has-been-manipulated)
* [Frame of reference has been manipulated](#frame-of-reference-has-been-manipulated)

## Problemi dove un esperto può aiutarti

* [Author is untrustworthy](#author-is-untrustworthy)
* [Collection process is opaque](#collection-process-is-opaque)
* [Data assert unrealistic precision](#data-assert-unrealistic-precision)
* [There are inexplicable outliers](#there-are-inexplicable-outliers)
* [An index masks underlying variation](#an-index-masks-underlying-variation)
* [Results have been p-hacked](#results-have-been-p-hacked)
* [Benford's Law fails](#benfords-law-fails)
* [Troppo bello per essere vero](#too-good-to-be-true)

## Problemi dove un programmatore potrebbe aiutarti

* [Data are aggregated to the wrong categories or geographies](#data-are-aggregated-to-the-wrong-categories-or-geographies)
* [Dati presenti in documenti scansionati](#data-are-in-scanned-documents)

# Lista dettagliata di tutti i problemi

## Problemi che dovresti risolvere

### Valori mancanti

Rimuovere valori vuoti o _null_ in un dataset a meno che non si ha assoluta certezza del loro significato. Se il dataset contiene dati annui, il valore per quell'anno non è mai stato raccolto? Se è un sondaggio, l'utente si è rifiutato di rispondere alla domanda?

Ogni volta che lavori con dei valori mancanti dovresti chiederti: "So cosa significa la mancanza di questo valore?" Se la risposta è no, dovresti chiedere maggiori informazioni alla tua fonte.

### Zero sostituisce valori mancanti

Peggio di un valore che manca è quando viene utilizzato un valore arbitrario. Questo può essere il risultato di intervento manuale che non ha valutato le implicazioni di quel zero oppure può accadere come il risultato di processi automatizzati che semplicemente non sanno come gestire valori nulli. In ogni caso, se vedi degli zeri in una serie di numeri dovresti chiederti se quei valori sono davvero il numero "0" o se invece significano "niente". (A volte `-1` viene usato in questo modo.) Se non sei sicuro, chiedi alla tua fonte.

La stessa cautela dovrebbe essere esercitata per altri valori non numerici dove uno `0` potrebbe essere rappresentato in un altro modo. Per esempio un falso `0` in una data lo vediamo in `1970-01-01T00:00:00Z` o in `1969-12-31T24:59:59Z` per il [timestamps in Unix](https://en.wikipedia.org/wiki/Unix_time#Encoding_time_as_a_number). Un falso `0` per un luogo potrebbe essere rappresentato da `0°00'00.0"N+0°00'00.0"E` o semplicemente `0°N 0°E`, un punto nell'Oceano Atlantico a sud del Ghana noto come [Null Island](https://en.wikipedia.org/wiki/Null_Island).

Vedi anche:

* [Valori sospetti presenti](#suspicious-values-are-present)
* [Il foglio ha date nel 1900, 1904, 1969 o 1970](#spreadsheet-has-dates-in-1900-1904-1969-or-1970)

### Mancano dati che sai che dovrebbero esserci

A volte mancano dati che, per la tua conoscenza dell'argomento, sai che mancano dal dataset. Se hai un set di dati che copre gli Stati Uniti, è possibile verificare che tutti i 50 stati siano rappresentati. (senza dimenticarsi dei [territori](https://en.wikipedia.org/wiki/Territories_of_the_United_States)—50 non sarebbe il numero corretto se il dataset dovrebbe includere anche il Porto Rico.) Se hai a che fare con un set di dati dei giocatori di calcio, assicurati che abbia il numero di squadre che ti aspetti. Verifica che alcuni giocatori che conosci siano inseriti. Fidati della tua intuizione se qualcosa sembra mancare e prova a ricontrollare con la tua fonte. Non è da escludere che l'insieme (detto anche universo) dei tuoi dati potrebbe essere più piccolo di quanto pensi.

### Righe o valori duplicati

Se vedi la stessa riga apparire più volte nel dataset, dovresti capirne il motivo. A volte non si tratta di una riga intera. Alcuni dati possono includere "emendamenti" che utilizzano gli stessi identificatori univoci della transazione originale. Se questo viene ignorato, qualsiasi calcolo effettuato con i suddetti sarebbe errato. Se noti qualcosa  dovrebbe essere unico, verifica che effettivamente lo sia, altrimenti verifica con la tua fonte.

### Spelling inconsistente

L'ortografia è uno dei modi più ovvi per capire se i dati sono stati compilati a mano. Non limitarti a guardare i nomi delle persone: quelli sono spesso il posto più difficile per rilevare errori di ortografia. Cerca invece, ad esempio, i luoghi in cui i nomi delle città non sono coerenti. ("Los Angelos" è un errore molto comune.) Se li trovi puoi essere abbastanza sicuro che i dati sono stati compilati o modificati a mano e questo è un segnale di cautela. I dati che sono stati modificati a mano hanno maggiori probabilità di avere errori. Questo non significa che non dovresti usarli, ma potresti dover correggere manualmente quegli errori o comunque tenerne conto nei tuoi rapporti.

[OpenRefine](http://openrefine.org/) ha una funzione di [text clustering](https://github.com/OpenRefine/OpenRefine/wiki/Clustering) può aiutare a semplificare il processo di correzione ortografico suggerendo partite ravvicinate tra valori inconsistenti all'interno di una colonna (per esempio, armonizzando `Treviso` e `Trevso`). E' importante [documentare i cambiamenti che vengono fatti](https://github.com/OpenRefine/OpenRefine/wiki/Exporters) per fornire una [buona provenienza dei dati](#provenance-is-not-documented).

Vedi anche:

* [Dati inseriti a mano](#data-were-entered-by-humans)

### Ordine dei nomi incoerente

I tuoi dati contengono nomi mediorientali o asiatici? Sei sicuro che i cognomi sono sempre nello stesso posto? Is it possible anyone in your dataset [uses a mononym](https://en.wikipedia.org/wiki/Indonesian_names#Indonesian_naming_system)? These are the sorts of things that data makers habitually get wrong. If you're working with a list of ethnically diverse names—which is any list of names—then you should do at least a cursory review before assuming that joining the `first_name` and `last_name` columns will give you something that is appropriate to publish.

* [Dati inseriti a mano](#data-were-entered-by-humans)

### Formati data incoerenti

Quale di queste date è in Settembre?

* `10/9/15`
* `9/10/15`

Se il primo è stato scritto da un europeo e dal secondo da un americano [allora entrambe le date sono in Settembre](https://en.wikipedia.org/wiki/Date_format_by_country). Senza conoscere la storia dei dati che non puoi sapere di sicuro.Sapere da dove provengono i tuoi dati e sii sicuro che fosse tutto creato da gente dello stesso continente.

* [Dati inseriti a mano](#data-were-entered-by-humans)
* [Provenienza non documentata](#provenance-is-not-documented)

### Unità non specificate

Neither `weight` nor `cost` conveys any information about the unit of measurement. Don't be too quick to assume that data produced within the United States are in units of pounds and dollars. Scientific data are often metric. Foreign prices may be specified in their local currency. If the data do not spell out their units, go back to your source and find out. Even if it does spell out its units always be wary of meanings that may have shifted over time. A dollar in 2010 is not a dollar today. And a [ton](https://en.wikipedia.org/wiki/Short_ton) is not a [ton](https://en.wikipedia.org/wiki/Long_ton) nor a [tonne](https://en.wikipedia.org/wiki/Tonne).

Vedi anche:

* [Nomi dei campi ambigui](#field-names-are-ambiguous)
* [Inflation skews the data](#inflation-skews-the-data)

### Categories are badly chosen

Watch out for values which purport to be only `true` or `false`, but really aren't. This is often the case with surveys where `refused` or `no answer` are also valid—and meaningful—values. Another common problem is the usage of any kind of `other` category. If the categories in a dataset are a bunch of countries and an `other`, what does that mean? Does it mean that the person collecting the data didn't know the right answer? Were they in international waters? Expatriates? Refugees?

Le cattive categorie possono anche escludere artificialmente i dati.Questo spesso accade con le statistiche del crimine.L'FBI ha definito il crimine di "colza" in una varietà di modi diversi nel tempo.In effetti, hanno fatto un tale lavoro povero di capire quale stupro è che molti criminologi sostengono che le loro statistiche non dovrebbero essere affatto utilizzate.Una definizione cattiva potrebbe significare un crimine è contato in una categoria diversa di quanto ti aspetti o che non è stato conteggiato affatto.Essere eccezionalmente articoli di questo problema quando si lavora con gli argomenti in cui le definizioni tendono ad essere arbitrarie, come `razza` e `etnia`.

### Nomi dei campi ambigui

What is a `residence`? Is it where someone lives or where they pay taxes? Is it a city or a county? Field names in data are never as specific as we would like, but particular concern should be applied to those that could obviously mean two or more things. Even if you correctly infer what the values are supposed to mean, that ambiguity could have easily caused the person collecting the data to enter the wrong value.

### Provenienza non documentata

I dati sono creati da una varietà di tipi di individui e organizzazioni, comprese le aziende, i governi, i non profit e i teorici della cospirazione del lavoro di noci.I dati sono raccolti in molti modi diversi, inclusi sondaggi, sensori e satelliti.Può essere digitato, toccato o scarabocchiato.Sapendo dove provengono i tuoi dati possono darti una grande quantità di informazioni sui suoi limiti.

I dati del sondaggio, ad esempio, sono raramente esaustivi.I sensori variano nella loro precisione.I governi sono spesso inclinati per darti informazioni imparziali.I dati da una zona di guerra possono avere un forte pregiudizio geografico dovuto al pericolo di attraversare le linee di battaglia.Per peggiorare questa situazione, queste diverse fonti sono spesso concatenate daisy.Gli analisti politici ridistribuiscono frequentemente i dati ricevuti dal governo.I dati che sono stati scritti da un medico possono essere cacciati da un'infermiera.Ogni fase in quella catena è un'opportunità per errore.Sapere dove provengono i tuoi dati.

Vedi anche:

* [Unità non specificate](#units-are-not-specified)

### Valori sospetti presenti

If you see any of these values in your data, treat them with an abundance of caution:

Numbers:

* [`65,535`](https://en.wikipedia.org/wiki/65535_%28number%29)
* [`255`](https://en.wikipedia.org/wiki/255_%28number%29)
* [`2,147,483,647`](https://en.wikipedia.org/wiki/2147483647_%28number%29)
* [`4,294,967,295`](https://en.wikipedia.org/wiki/4294967295)
* [`555-3485`](https://en.wikipedia.org/wiki/555_%28telephone_number%29)
* `99999` (or any other long sequence of 9's)
* `00000` (or any other sequence of 0's)

Dates:

* [`1970-01-01T00:00:00Z`](https://en.wikipedia.org/wiki/Unix_time#Encoding_time_as_a_number)
* [`1969-12-31T23:59:59Z`](https://en.wikipedia.org/wiki/Unix_time#Encoding_time_as_a_number)
* [`January 1st, 1900`](#spreadsheet-has-dates-in-1900-1904-1969-or-1970)
* [`January 1st, 1904`](#spreadsheet-has-dates-in-1900-1904-1969-or-1970)

Locations:

* [`0°00'00.0"N+0°00'00.0"E`](https://en.wikipedia.org/wiki/Null_Island) or simply [`0°N 0°E`](https://en.wikipedia.org/wiki/Null_Island)
* US zip code `12345` (Schenectady, New York)
* US zip code `90210` (Beverly Hills, CA)

Each of these numbers has an indication of a particular error made by either a human or a computer. If you see them, ensure they actually mean what you think they mean!

Vedi anche:

* [Il foglio ha 65536 righe](#spreadsheet-has-65536-rows)
* [Il foglio ha 255 colonne](#spreadsheet-has-255-columns)
* [Spreadsheet has dates in 1900 or 1904](#spreadsheet-has-dates-in-1900-or-1904)

### Data are too coarse

Hai stati stati e hai bisogno di contee. Hai datori di lavoro e hai bisogno di dipendenti. Ti hanno dato anni, ma vuoi mesi. In molti casi otteniamo dati che sono stati aggregati per i nostri scopi.

I dati di solito non possono essere disaggregati una volta che sono stati fusi insieme. Se ti vengono dati dati troppo grossolani, dovrai chiedere la tua fonte di qualcosa di più specifico. Potrebbero non averlo. Se ce l'hanno, potrebbero non essere in grado o disposti a darti a te. Ci sono molti set di dati federali che non possono essere accessibili a livello locale per proteggere la privacy di individui che potrebbero essere identificati in modo univoco da loro. (Ad esempio, una singola vita nazionale somalo nel Texas occidentale.) Tutto quello che puoi fare è chiedere.

Una cosa che non dovresti mai fare è dividere un valore annuale entro 12 e chiamarlo la "media al mese". Senza conoscere la distribuzione dei valori, quel numero sarà privo di significato. (Forse tutte le istanze si sono verificate in un mese o una stagione. Forse i dati seguono una tendenza esponenziale anziché un lineare.) È sbagliato. Non farlo

Vedi anche:

* [Dati troppo granulari](#data-are-too-granular)
* [Data are aggregated to the wrong categories or geographies](#data-are-aggregated-to-the-wrong-categories-or-geographies)

### Totals differ from published aggregates

Imagine that after a long FOIA fight you receive a "complete" list of incidents of police use-of-force. You open it up and discover it has 2,467 rows. Great, time to report it out. Not so fast. Before you publish anything from that dataset go find the last time that police chief went on the record about his department's use of force. You may find that in an interview six weeks earlier he said "less than 2,000 times" or that he named a specific number and it doesn't match your dataset.

These sorts of discrepancies between published statistics and raw data can be a very great source of leads. Often the answer will be simple. For instance, the data you were given may not cover the same time period he was speaking about. But sometimes you'll catch them in a lie. Either way, you should make sure the published numbers match the totals for the data you're given.

### Il foglio ha 65536 righe

Il numero massimo di righe è stato permesso che un foglio di calcolo eccellente vecchio stile sia stato 65.536.Se si riceve un set di dati con quel numero di righe che hai quasi certamente dato dati troncati.Torna indietro e chiedi il resto.Le versioni più recenti di Excel hanno permesso 1.048.576 righe, quindi è meno probabile che lavorerai con i dati che colpiscono il limite.

### Il foglio ha 255 colonne

L'app Numbers di Apple può gestire solo i fogli di calcolo con 255 colonne e l'app troncerà i file con più colonne senza avvisare l'utente. Se si riceve un set di dati con esattamente 255 colonne, chiedi se il file è mai stato aperto o convertito con i numeri.

### Il foglio ha date nel 1900, 1904, 1969 o 1970

For reasons beyond obscure, Excel's default date from which it counts all other dates is `January 1st, 1900`, *unless* you're using Excel on a Mac, in which case it's `January 1st, 1904`. There are a variety of ways in which data in Excel can be entered or calculated incorrectly and end up as one of these two dates. If you spot them in your data, it's probably an issue.

Many databases and applications will often generate a date of `1970-01-01T00:00:00Z` or `1969-12-31T23:59:59Z` which is the [Unix epoch for timestamps](https://en.wikipedia.org/wiki/Unix_time#Encoding_time_as_a_number). In other words this is what happens when a system tries to display an empty value or a `0` value as a date.

### Testo convertito in numeri

Not all numerals are numbers. For instance, the US Census Bureau uses "FIPS codes" to identify every place in the United States. These codes are of various lengths and are numeric. However, they are *not* numbers. `037` is the FIPS code for Los Angeles County. It is not the number `37`. The numerals `37` are, however, a valid FIPS code: for North Carolina. Excel and other spreadsheets will often make the mistake of assuming numerals are numbers and stripping the leading zeros. This can cause all kinds of problems if you try to convert it to another file format or merge it with another dataset. Watch out for data where this has happened before it was given to you.

### Numeri salvati come testo

Quando si lavora con fogli di calcolo, i numeri possono essere memorizzati come testo con formattazione indesiderata.Ciò accade spesso quando un foglio di calcolo è ottimizzato per presentare i dati piuttosto che renderlo disponibile per il riutilizzo.Ad esempio, invece di rappresentare un milione di dollari con il numero "1000000" una cella potrebbe contenere la stringa "1.000.000" o "1 000 000 000" o "USD 1.000.000" con la formattazione di virgolette, unità e spazi inseriti come caratteri.Excel può prendersi cura di alcuni semplici casi con funzioni integrate, ma spesso dovrai usare le formule per spogliare i caratteri fino a quando le celle sono abbastanza pulite da essere riconosciute come numeri.Buona pratica è di memorizzare i numeri senza formattazione e includere le informazioni di supporto nei nomi delle colonne o sui metadati.

## Issues that you should solve

### Text is garbled

All letters are represented by computers as numbers. Encoding problems are issues that arise when text is represented by a specific set of numbers (called an "encoding") and you don't know what it is. This leads to a phenomenon called [mojibake](https://en.wikipedia.org/wiki/Mojibake) where the text in your data looks like garbage, or like this: ���.

In the vast majority of cases your text editor or spreadsheet application will figure out the correct encoding, however, if it screws it up you could be publishing somebody's name with a weird character in the middle. Your source should be able to tell you what encoding your data are in. In the event they can't there are ways of guessing that are about fairly reliable. Ask a programmer.

### Line endings are garbled

Tutti i file di testo e "Testo", come CSV, utilizzare caratteri invisibili per rappresentare le estremità delle linee.I computer Windows, Mac e Linux hanno storicamente disaccordo su ciò che questi personaggi che terminano la linea dovrebbero essere.Tentativo di aprire un file salvato su un sistema operativo da un altro sistema operativo può a volte causare Excel o altre applicazioni per non identificare correttamente le interruzioni di linea.

Typically, this is easy to resolve by simply opening the file in any general-purpose text editor and re-saving it. If the file is exceptionally large you may need to consider using a command-line tool or enlisting the help of a programmer. You can read more about this issue [here](https://nicercode.github.io/blog/2013-04-30-excel-and-line-endings/).

### I dati sono in un PDF

A tremendous amount of data—especially government data—are only available in PDF format. If you have real, textual data inside the PDF then there are several good options for extracting them. (If you've got [scanned documents](#data-are-in-scanned-documents) that's a different problem.) One excellent, free tool is [Tabula](http://tabula.technology/). However, if you have Adobe Creative Cloud then you also have access to Acrobat Pro, which has an excellent feature for exporting tables in PDFs to Excel. Either solution should be able to extract most tabular data from a PDF.

Vedi anche:

* [Data are in scanned documents](#data-are-in-scanned-documents)

### Dati troppo granulari

This is the opposite of [Data are too coarse](#data-are-too-coarse). In this case you've got counties, but you want states or you've got months but you want years. Fortunately this is usually pretty straightforward.

I dati possono essere aggregati utilizzando la funzione TABLE PIVOT di Excel o Google Documenti, utilizzando un database SQL o scrivendo codice personalizzato.I tavoli per pivot sono uno strumento favoloso che ogni reporter dovrebbe imparare, ma hanno i loro limiti.Per set di dati o aggregazioni eccezionalmente grandi a gruppi insoliti dovresti chiedere a un programmatore e possono creare una soluzione più facile da verificare e riutilizzare.

Vedi anche:

* [Data are too coarse](#data-are-too-coarse)
* [Data are aggregated to the wrong categories or geographies](#data-are-aggregated-to-the-wrong-categories-or-geographies).

### Dati inseriti a mano

L'immissione dei dati umani è un problema così comune che i sintomi di esso sono menzionati in almeno 10 delle altre questioni descritte qui.Non esiste un modo peggiore per avvitare i dati che lasciare un singolo tipo umano in, senza convalida.Ad esempio, ho acquisito una volta il database completo delle licenze per cani per Cook County, Illinois.Invece di richiedere alla persona che registra il loro cane a scegliere una razza da una lista, i creatori del sistema avevano semplicemente dato loro un campo di testo da digitare. Di conseguenza, questo database conteneva almeno 250 ortografie di `Chihuahua`. Anche con i migliori strumenti disponibili, i dati questo disordinato non può essere salvato.Sono efficacemente privi di significato.Questo non è così importante con i dati del cane, ma non vuoi che accada con soldati feriti o ticker azionari.Attenzione ai dati inseriti dall'uomo.

### Data are intermingled with formatting and annotations

Complex representations of data such as HTML and XML allow for a clean separation between data and formatting, but this is not the case for common tabular representations of data such as a spreadsheet. Yet, people still try. A common problem with data provided as a spreadsheet is that the first few rows of data will actually be descriptions or notes about the data rather than column headings or data itself. A key or data dictionary may also be placed in the middle of the spreadsheet. Header rows may be repeated. Or the spreadsheet will include multiple tables (which may have different column headings) one after the other in the same sheet rather than separated into different sheets.

In all of these cases the main solution is simply to identify the problem. Obviously trying to perform any analysis on a spreadsheet that has these kinds of problems will fail, sometimes for non-obvious reasons. When looking at new data for the first time it's always a good idea to ensure there aren't extra header rows or other formatting characters inserted amongst the data.

### Aggregations were computed on missing values

Imagine a dataset with 100 rows and a column called `cost`. In 50 of the rows the `cost` column is blank. What is the average of that column? Is it `sum_of_cost / 50` or `sum_of_cost / 100`? There is no one definitive answer. In general, if you're going to compute aggregates on columns that are missing data, you can safely do so by filtering out the missing rows first, but be careful not to compare aggregates from two different columns where different rows were missing values! In some cases the missing values might also be legitimately interpreted as `0`. If you're not sure, ask an expert or just don't do it.

This is an error you can make in your analysis, but it's also an error that others can make and pass on to you, so watch out for it if data comes to you with aggregates already computed.

Vedi anche:

* [Values are missing](#values-are-missing)
* [Zero sostituiscono valori mancanti](#zeros-replace-missing-values)

### Sample is not random

A non-random sampling error occurs when a survey or other sampled dataset either intentionally or accidentally fails to cover the entire population. This can happen for a variety of reasons ranging from time-of-day to the respondent's native language and is a common source of error in sociological research. It can also happen for less obvious reasons, such as when a researcher thinks they have a complete dataset and chooses to work with only part of it. If the original dataset was incomplete for any reason then any conclusions drawn from their sample will be incorrect. The only thing you can do to fix a non-random sample is avoid using that data.

Vedi anche:

* [Sample is biased](#sample-is-biased)

### Margin-of-error is too large

I know of no other single issue that causes more reporting errors than the unreflective usage of numbers with very large margins-of-error. MOE is usually associated with survey data. The most likely place a reporter encounters it is when using polling data or the US Census Bureau's [American Community Survey](https://www.census.gov/programs-surveys/acs/) data. The MOE is a measure of the range of possible true values. It may be expressed as a number (`400 +/- 80`) or as a percentage of the whole (`400 +/- 20%`). The smaller the relevant population, the larger the MOE will be. For example, according to the 2014 5-year ACS estimates, the number of Asians living in New York is `1,106,989 +/- 3,526` (0.3%). The number of Filipinos is `71,969 +/- 3,088` (4.3%). The number of Samoans is `203 +/- 144` (71%).

The first two numbers are safe to report. The third number should never be used in published reporting. There is no one rule about when a number is not accurate enough to use, but as a rule of thumb, you should be cautious about using any number with a MOE over 10%.

Vedi anche:

* [Margin-of-error is unknown](#margin-of-error-is-unknown)

### Margin-of-error is unknown

Sometimes the problem isn't that the margin of error is [too large](#margin-of-error-is-too-large), it's that nobody ever bothered to figure out what it was in the first place. This is one problem with unscientific polls. Without computing a MOE, it is impossible to know how accurate the results are. As a general rule, anytime you have data that are from a survey you should ask for what the MOE is. If the source can't tell you, those data probably aren't worth using for any serious analysis.

Vedi anche:

* [Margin-of-error is too large](#margin-of-error-is-too-large)

### Sample is biased

Like [a sample that is not random](#sample-is-not-random), a biased sample results from a lack of care with how the sampling is executed. Or, from willfully misrepresenting it. A sample might be biased because it was conducted on the internet and poorer people don't use the internet as frequently as the rich. Surveys must be carefully weighted to ensure they cover proportional segments of any population that could skew the results. It's almost impossible to do this perfectly so it is often done wrong.

Vedi anche:

* [Sample is not random](#sample-is-not-random)

### Data have been manually edited

Manual editing is almost the same problem as [data being entered by humans](#data-were-entered-by-humans) except that it happens after the fact. In fact, data are often manually edited in an attempt to fix data that were originally entered by humans. Problems start to creep in when the person doing the editing doesn't have complete knowledge of the original data. I once saw someone spontaneously "correct" a name in a dataset from `Smit` to `Smith`. Was that person's name really `Smith`? I don't know, but I do know that value is now a problem. Without a record of that change, it's impossible to verify what it should be.

Issues with manual editing are one reason why you always want to ensure your data have [well-documented provenance](#provenance-is-not-documented). A lack of provenance can be a good indication that someone may have monkeyed with it. Academics and policy analysts often get data from the government, monkey with them and then redistribute them to journalists. Without any record of their changes it's impossible to know if the changes they made were justified. Whenever feasible always try to get the *primary source* or at least the earliest version you can and then do your own analysis from that.

Vedi anche:

* [Provenienza non documentata](#provenance-is-not-documented)
* [Dati inseriti a mano](#data-were-entered-by-humans)

### Inflation skews the data

Currency inflation means that over time money changes in value. There is no way to tell if numbers have been "inflation adjusted" just by looking at them. If you get data and you aren't sure if they have been adjusted then check with your source. If they haven't you'll likely want to perform the adjustment. This [inflation adjuster](http://inflation-adjust.herokuapp.com/) is a good place to start.

Vedi anche:

* [Natural/seasonal variation skews the data](#nationalseasonal-variation-skews-the-data)

### Natural/seasonal variation skews the data

Many types of data fluctuate naturally due to some underlying forces. The best known example of this is employment fluctuating with the seasons. Economists have developed a variety of methods of compensating for this variation. The details of those methods aren't particularly important, but it is important that you know if the data you're using have been "seasonally adjusted". If they haven't and you want to compare employment from month to month you will probably want to get adjusted data from your source. (Adjusting them yourself is much harder than with inflation.)

Vedi anche:

* [Inflation skews the data](#inflation-skews-the-data)

### Timeframe has been manipulated

A source can accidentally or intentionally misrepresent the world by giving you data that stops or starts at a specific time. For a potent example see 2015's widely reported "national crime wave". There was no crime wave. What there was was a series of spikes in particular cities when compared to just the last few years. Had journalists examined a wider timeframe they would have seen that violent crime was higher virtually everywhere in the US ten years before. And twenty years before it was nearly double.

If you have data that covers a limited timeframe try to avoid starting your calculations with the very first time period you have data for. If you start a few years (or months or days) into the data you can have confidence that you aren't making a comparison which would be invalidated by having a single additional data point.

Vedi anche:

* [Frame of reference has been manipulated](#frame-of-reference-has-been-manipulated)

### Frame of reference has been manipulated

Crime statistics are often manipulated for political purposes by comparing to a year when crime was very high. This can be expressed either as a change (down `60%` since 2004) or via an index (`40`, where 2004 = 100). In either of these cases, 2004 may or may not be an appropriate year for comparison. It could have been an unusually high crime year.

This also happens when comparing places. If I want to make one country look bad, I simply express the data about it relative to whichever country is doing the best.

This problem tends to crop up in subjects where people have a strong confirmation bias. ("Just as I thought, crime is up!") Whenever possible try comparing rates from several different starting points to see how the numbers shift. And whatever you do, *don't use this technique yourself* to make a point you think is important. That's inexcusable.

Vedi anche:

* [Timeframe has been manipulated](#timeframe-has-been-manipulated)

## Issues a third-party expert should help you solve

### Author is untrustworthy

Sometimes the only data you have are from a source you would rather not rely on. In some situations that's just fine. The only people who know how many guns are made are gun manufacturers. However, if you have data from a questionable maker always check it with another expert. Better yet, check it with two or three. Don't publish data from a biased source unless you have substantial corroborating evidence.

### Collection process is opaque

It's very easy for false assumptions, errors or outright falsehoods to be introduced into these data collection processes. For this reason it's important that methods used be transparent. It's rare that you'll know exactly how a dataset was gathered, but indications of a problem can include numbers that [assert unrealistic precision](#data-asserts-unrealistic-precision) and data that [are Troppo bello per essere vero](#too-good-to-be-true).

Sometimes the origin story may just be fishy: did such-and-such academic really interview 50 active gang members from the south side of Chicago? If the way the data were gathered seems questionable and your source can't offer you [ironclad provenance](#provenance-is-not-documented) then you should always verify with another expert that the data could reasonably have been collected in the way that was described.

Vedi anche:

* [Provenienza non documentata](#provenance-is-not-documented)
* [Data assert unrealistic precision](#data-assert-unrealistic-precision)
* [Troppo bello per essere vero](#too-good-to-be-true)

### Data assert unrealistic precision

Al di fuori della dura scienza, poche cose vengono abitualmente misurate con più di due punti decimali di precisione.Se un dataset atterra sulla tua scrivania, pretende di mostrare le emissioni di una fabbrica al 7 ° punto decimale che è un giveaway morto che è stato stimato da altri valori. Quello in e per sé potrebbe non essere un problema, ma è importante essere trasparenti sulle stime. Sono spesso sbagliati.

### There are inexplicable outliers

I recently created a dataset of how long it takes for messages to reach different destinations over the internet. All of the times were in the range from `0.05` to `0.8` seconds, except for three. The other three were all over `5,000` seconds. This is a major red flag that something has gone wrong in the production of the data. In this particular case an error in the code I wrote caused some failures to continue counting while all other messages were being sent and received.

Outliers such as these can dramatically screw up your statistics—especially if you're using averages. (You should probably be using medians.) Whenever you have a new dataset it is a good idea to take a look at the largest and smallest values and ensure they are in a reasonable range. If the data justifies it you may also want to do a more statistically rigorous analysis using [standard deviations](https://en.wikipedia.org/wiki/Standard_deviation) or [median deviations](https://en.wikipedia.org/wiki/Median_absolute_deviation).

As a side-benefit of doing this work, outliers are often a great way to find story leads. If there really was one country where it took 5,000 times as long to send a message over the internet, that would be a great story.

### An index masks underlying variation

Gli analisti che vogliono seguire la tendenza di un problema spesso creano indici di vari valori per tenere traccia dei progressi. Non c'è nulla di intrinsecamente sbagliato nell'utilizzare un indice. Possono avere un grande potere esplicativo.Tuttavia è importante esercitare cautela su indici che combinano misure disparate.

For example, the United Nations [Gender Inequality Index](http://hdr.undp.org/en/content/gender-inequality-index-gii) combines several measures related to women's progress toward equality. One of the measures used in the GII is "representation of women in parliament". Two countries in the world have laws mandating gender representation in their parliaments: China and Pakistan. As a result these two countries perform far better in the index than countries that are similar in all other ways. Is this fair? It doesn't really matter, because it is confusing to anyone who doesn't know about this factor. The GII and similar indices should always be used with careful analysis to ensure their underlying variables don't swing the index in unexpected ways.

### Results have been p-hacked

P-hacking is intentionally altering the data, changing the statistical analysis, or selectively reporting results in order to have statistically significant findings. Examples of this include: stop collecting data once you have a significant result, remove observations to get a significant result, or perform many analyses and only report the few that are significant. There has been some [good reporting](http://fivethirtyeight.com/features/science-isnt-broken) on this problem.

Se hai intenzione di pubblicare i risultati di uno studio che devi capire cosa sia il valore P, cosa significa e poi fare una decisione istruita sul fatto che i risultati valga la pena utilizzare. Molti e molti risultati dello studio della spazzatura arrivano nelle principali pubblicazioni perché i giornalisti non capiscono i valori P.

Vedi anche:

* [Margin-of-error is too large](#margin-of-error-is-too-large)

### Benford's Law fails

La [legge di Benford](https://en.wikipedia.org/wiki/Benford's_law) è una teoria che afferma che le piccole cifre (1, 2, 3) appaiono all'inizio dei numeri molto più frequentemente rispetto alle grandi cifre (7, 8, 9). Nella legge di  Benford può essere utilizzata per rilevare anomalie nelle pratiche contabili o nei risultati delle elezioni, anche se in pratica può essere facilmente applicabile.Se si sospetta che un set di dati sia stato creato o modificato per ingannare, la legge di Benford è un eccellente primo test, ma dovresti sempre verificare i tuoi risultati con un esperto prima di concludere i tuoi dati sono stati manipolati.

### Troppo bello per essere vero

Non esiste un set di dati globale dell'opinione pubblica. Nessuno conosce il numero esatto di persone che vivono in Siberia. Le statistiche del crimine non sono paragonabili ai confini. Il governo degli Stati Uniti non ti dirà quanto materiale fissile mantiene a portata di mano.

Attenzione a tutti i dati che pretendono di rappresentare qualcosa che non si può pubblicamente sapere. Non sono dati ma la stime di qualcuno, probabilmente sbagliata. Nel dubbio chiedi a un esperto del settore di verificarlo.

## Problemi risolvibili da un programmatore

### Data are aggregated to the wrong categories or geographies

Sometimes your data are at about the right level of detail (neither [too coarse](#data-are-too-coarse) nor [too granular](#data-are-too-granular)), but they have been aggregated to a different grouping than you want. The classic example of this is data that are aggregated by zip codes that you would prefer to have by city neighborhoods. In many cases this is an impossible problem to solve without getting more granular data from your source, but sometimes the data can be proportionally mapped from one group to another. This must be undertaken only with careful understanding of the [margin-of-error](#margin-of-error-is-too-large) that may be introduced in the process. If you've got data aggregated to the wrong groups, ask a programmer if it is possible to re-aggregate it.

Vedi anche:

* [Data are too coarse](#data-are-too-coarse)
* [Dati troppo granulari](#data-are-too-granular)
* [Margin-of-error is too large](#margin-of-error-is-too-large)

### Dati presenti in documenti scansionati

Grazie agli open-data spesso governi ed enti pubblici sono tenuti a fornirti dati, anche se non vogliono. Una tattica molto comune da quest'ultimi per ostacolare è di fornire scansioni o fotografie delle pagine. Questi possono essere file di immagine o, più probabile, dei PDF.

È possibile estrarre il testo dalle immagini e convertirlo in dati. Questo è fatto attraverso un processo chiamato riconoscimento ottico-carattere (OCR). I software attuali possono essere accurati quasi al 100%, ma molto dipende dalla natura del documento. Ogni volta che usi un OCR per estrarre dei dati è consigliabile verificare che i risultati corrispondano all'originale.

Ci sono molti siti Web dove puoi caricare un documento per far fare il riconoscimento dei dati, ma ci sono anche strumenti gratuiti che un programmatore potrebbe essere in grado di utilizzare nei tuoi documenti specifici. Chiedi a loro quale sia la migliore strategia per i documenti che hai.

Vedi anche:

* [I dati sono in un PDF](#data-are-in-a-pdf)
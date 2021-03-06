---
title: Verarbeiten von Azure-Blobdaten mit erweiterter Analyse | Microsoft Docs
description: Verarbeiten von Daten im Azure-Blobspeicher
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8a59078-91d3-4440-b85c-430363c3f4d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2016
ms.author: fashah;garye;bradsev
translationtype: Human Translation
ms.sourcegitcommit: ba61d00f277af579c87a130336ead9879b82a6de
ms.openlocfilehash: 0678c47b28cff54986b79b901c7d2a92136610fc


---
# <a name="a-nameheadingaprocess-azure-blob-data-with-advanced-analytics"></a><a name="heading"></a>Verarbeiten von Azure-Blobdaten mit erweiterter Analyse
In diesem Dokument werden das Durchsuchen von Daten und Generieren von Funktionen aus Daten in einem Azure-Blobspeicher beschrieben. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>Laden der Daten in ein Pandas-DataFrame
Um ein Dataset zu untersuchen und zu bearbeiten, muss es aus der Blobquelle in eine lokale Datei heruntergeladen werden, die anschließend in ein Pandas-DataFrame geladen werden kann. Nachfolgend sehen Sie für dieses Verfahren erforderlichen Schritte:

1. Laden Sie die Daten mithilfe des Blobdiensts und folgenden Python-Beispielcodes aus dem Azure-Blob herunter. Ersetzen Sie die Variablen im Code durch die für Ihre Umgebung geltenden Werte: 
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))
2. Lesen Sie die Daten aus der heruntergeladenen Datei in ein Pandas-DataFrame ein.
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Sie können nun die Daten durchsuchen und Funktionen mit diesem DataSet generieren.

## <a name="a-nameblob-dataexplorationadata-exploration"></a><a name="blob-dataexploration"></a>Durchsuchen von Daten
Hier sind einige Beispiele für Möglichkeiten zum Durchsuchen von Daten mithilfe von Pandas:

1. Überprüfen der Anzahl von Zeilen und Spalten: 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. Überprüfen der ersten oder letzten Zeilen im DataSet:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Überprüfen des Datentyps der einzelnen importierten Spalten:
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Überprüfen der Basisstatistiken für die Spalten im DataSet:
   
        dataframe_blobdata.describe()
5. Überprüfen der Anzahl der Einträge für jeden Spaltenwert:
   
        dataframe_blobdata['<column_name>'].value_counts()
6. Zählen der fehlenden Werte im Vergleich zur tatsächlichen Anzahl der Einträge in den einzelnen Spalten:
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Löschen fehlender Werte in einer bestimmten Spalte aus den Daten:
   
     dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape
   
   Alternatives Ersetzen der fehlenden Werte mit der "mode"-Funktion:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        
8. Erstellen eines Histogramms mithilfe der Variablenanzahl von Gruppen zur Darstellung der Verteilung einer Variablen:    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Überprüfen der Korrelationen zwischen Variablen mithilfe eines Punktdiagramms oder einer integrierten "correlation"-Funktion:
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <a name="a-nameblob-featuregenafeature-generation"></a><a name="blob-featuregen"></a>Generieren von Funktionen
Sie können Funktionen wie folgt mithilfe von Python generieren:

### <a name="a-nameblob-countfeatureaindicator-value-based-feature-generation"></a><a name="blob-countfeature"></a>Indikatorwertbasiertes Generieren von Funktionen
Kategorische Funktionen können wie folgt erstellt werden:

1. Untersuchen Sie die Verteilung der Kategoriespalte:
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. Generieren Sie Indikatorwerte für alle Spaltenwerte:
   
        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. Führen Sie die Indikatorspalte mit dem ursprünglichen DataFrame zusammen: 
   
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. Entfernen Sie die ursprüngliche Variable:
   
        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <a name="a-nameblob-binningfeatureabinning-feature-generation"></a><a name="blob-binningfeature"></a>Gruppenbasierte Funktionsgenerierung
Zum Generieren von klassifizierten Funktionen gehen Sie wie folgt vor:

1. Fügen Sie eine Folge von Spalten zum Klassifizieren einer numerischen Spalte hinzu:
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. Konvertieren Sie die Klassifizierung in eine Folge von booleschen Variablen:
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. Führen Sie zum Schluss die Platzhaltervariablen mit dem ursprünglichen DataFrame zusammen:
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <a name="a-namesql-featuregenawriting-data-back-to-azure-blob-and-consuming-in-azure-machine-learning"></a><a name="sql-featuregen"></a>Zurückschreiben von Daten in das Azure-Blob und Verwenden in Azure Machine Learning
Nachdem Sie die Daten untersucht und die erforderlichen Funktionen erstellt haben, können Sie die Daten (als Stichproben oder über Funktionen) mithilfe der folgenden Schritte in ein Azure-Blob hochladen und in Azure Machine Learning verwenden. Beachten Sie, dass in Azure Machine Learning Studio ebenfalls weitere Funktionen erstellt werden können. 

1. Schreiben Sie den DataFrame in eine lokale Datei:
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Laden Sie die Daten in ein Azure-Blob hoch:
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. Die Daten können nun wie im folgenden Screenshot gezeigt mit dem Modul [Import Data][import-data] von Azure Machine Learning aus dem Blob gelesen werden:

![Reader-Blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/




<!--HONumber=Dec16_HO2-->



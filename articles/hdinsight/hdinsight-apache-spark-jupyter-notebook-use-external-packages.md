---
title: Verwenden benutzerdefinierter Maven-Pakete mit Jupyter Notebooks in Spark unter Azure | Microsoft-Dokumentation
description: "Eine Schritt-für-Schritt-Anleitung zum Konfigurieren von Jupyter Notebooks, die mit Spark-Clustern in HDInsight verfügbar sind, sodass sie externe Spark-Pakete verwenden."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2a8bc545-064e-436f-8b5f-e67c26cfbf98
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: a939a0845d7577185ff32edd542bcb2082543a26
ms.openlocfilehash: 2dd0d456d0c6b1c83a409fead63dacff26c03198


---
# <a name="use-external-packages-with-jupyter-notebooks-in-apache-spark-clusters-on-hdinsight"></a>Verwenden externer Pakete mit Jupyter Notebooks in Apache Spark-Clustern unter HDInsight
> [!div class="op_single_selector"]
> * [Verwenden von Cell Magic](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
> * [Verwenden von Skriptaktionen](hdinsight-apache-spark-python-package-installation.md)
>
>

Erfahren Sie, wie Sie ein Jupyter Notebook in einem Apache Spark-Cluster in HDInsight konfigurieren, um externe, von der Community bereitgestellte **maven**-Pakete zu verwenden, die nicht im Lieferumfang des Clusters enthalten sind. 

Sie können das [Maven Repository](http://search.maven.org/) nach einer vollständigen Liste der verfügbaren Pakete durchsuchen. Sie können die Liste der verfügbaren Pakete auch aus anderen Quellen abrufen. Beispielsweise steht eine vollständige Liste der von der Community bereitgestellten Pakete auf [Spark-Pakete](http://spark-packages.org/)zur Verfügung.

In diesem Artikel erfahren Sie, wie Sie das Paket [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar) mit Jupyter Notebook verwenden.



## <a name="prerequisites"></a>Voraussetzungen
Sie benötigen Folgendes:

* Ein Azure-Abonnement. Siehe [Kostenlose Azure-Testversion](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).
* Einen Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).

## <a name="use-external-packages-with-jupyter-notebooks"></a>Verwenden von externen Paketen mit Jupyter Notebooks
1. Klicken Sie im [Azure-Portal](https://portal.azure.com/)im Startmenü auf die Kachel für Ihren Spark-Cluster (sofern Sie die Kachel ans Startmenü angeheftet haben). Sie können auch unter **Alle durchsuchen** > **HDInsight-Cluster** zu Ihrem Cluster navigieren.   
2. Klicken Sie auf dem Blatt für den Spark-Cluster auf **Quicklinks** und anschließend auf dem Blatt **Cluster-Dashboard** auf **Jupyter Notebook**. Geben Sie die Administratoranmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.

    > [!NOTE]
    > Sie können auch das Jupyter Notebook für Ihren Cluster aufrufen, indem Sie in Ihrem Browser die folgende URL öffnen. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters:
    > 
    > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
    > 

   

3. Erstellen Sie ein neues Notebook. Klicken Sie auf **Neu** und anschließend auf **Spark**.
   
    ![Erstellen eines neuen Jupyter Notebooks](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.createnotebook.png "Erstellen eines neuen Jupyter Notebooks")

4. Ein neues Notebook mit dem Namen „Untitled.pynb“ wird erstellt und geöffnet. Klicken Sie oben auf den Namen des Notebooks, und geben Sie einen Anzeigenamen ein.
   
    ![Angeben eines neuen Namens für das Notebook](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/hdispark.note.jupyter.notebook.name.png "Angeben eines neuen Namens für das Notebook")

5. Verwenden Sie die `%%configure` -Magic, um das Notebook so zu konfigurieren, dass es ein externes Paket verwendet. Stellen Sie sicher, dass Notebooks, die externe Pakete verwenden, die `%%configure` -Magic in der ersten Codezelle aufrufen. Dadurch wird sichergestellt, dass der Kernel für die Verwendung des Pakets konfiguriert ist, bevor die Sitzung gestartet wird.

    >[!IMPORTANT] 
    >Wenn Sie vergessen, den Kernel in der ersten Zelle zu konfigurieren, können Sie `%%configure` mit dem Parameter `-f` verwenden. Allerdings wird die Sitzung dadurch neu gestartet, und der bisherige Fortschritt geht verloren.

    | HDInsight-Version | Befehl |
    |-------------------|---------|
    |Für HDInsight 3.3 und HDInsight 3.4 | `%%configure` <br>`{ "packages":["com.databricks:spark-csv_2.10:1.4.0"] }`|
    | Für HDInsight 3.5 | `%%configure`<br>`{ "conf": {"spark.jars.packages": "com.databricks:spark-csv_2.10:1.4.0" }}`|

6. Im obigen Codeausschnitt werden die Maven-Koordinaten für das externe Paket im zentralen Maven-Repository erwartet. In diesem Codeausschnitt ist `com.databricks:spark-csv_2.10:1.4.0` die Maven-Koordinate für das Paket **spark-csv** . Nachstehend finden Sie eine Anleitung zur Erstellung von Koordinaten für ein Paket.
   
    a. Suchen Sie das Paket im Maven-Repository. In diesem Tutorial verwenden wir [spark-csv](http://search.maven.org/#artifactdetails%7Ccom.databricks%7Cspark-csv_2.10%7C1.4.0%7Cjar).
   
    b. Sammeln Sie im Repository die Werte für **GroupId**, **ArtifactId** und **Version**.
   
    ![Verwenden externer Pakete mit Jupyter Notebooks](./media/hdinsight-apache-spark-jupyter-notebook-use-external-packages/use-external-packages-with-jupyter.png "Verwenden externer Pakete mit Jupyter Notebooks")
   
    c. Verketten Sie die drei Werte, getrennt durch einen Doppelpunkt (**:**).
   
        com.databricks:spark-csv_2.10:1.4.0

7. Führen Sie die Codezelle mit der `%%configure` -Magic aus. Dadurch wird die zugrunde liegende Livy-Sitzung für die Verwendung des von Ihnen bereitgestellten Pakets konfiguriert. Sie können das Paket nun in den folgenden Codezellen in Ihrem Notebook verwenden, wie unten dargestellt.
   
        val df = sqlContext.read.format("com.databricks.spark.csv").
        option("header", "true").
        option("inferSchema", "true").
        load("wasbs:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

8. Anschließend können Sie die Codeausschnitte wie unten dargestellt ausführen, um die Daten aus dem im vorherigen Schritt erstellten Datenrahmen anzuzeigen.
   
        df.show()
   
        df.select("Time").count()

## <a name="a-nameseealsoasee-also"></a><a name="seealso"></a>Weitere Informationen
* [Übersicht: Apache Spark in Azure HDInsight](hdinsight-apache-spark-overview.md)

### <a name="scenarios"></a>Szenarios
* [Spark mit BI: Durchführen interaktiver Datenanalysen mithilfe von Spark in HDInsight mit BI-Tools](hdinsight-apache-spark-use-bi-tools.md)
* [Spark mit Machine Learning: Analysieren von Gebäudetemperaturen mithilfe von Spark in HDInsight und HVAC-Daten](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark mit Machine Learning: Vorhersage von Lebensmittelkontrollergebnissen mithilfe von Spark in HDInsight](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark-Streaming: Erstellen von Echtzeitstreaminganwendungen mithilfe von Spark in HDInsight](hdinsight-apache-spark-eventhub-streaming.md)
* [Websiteprotokollanalyse mithilfe von Spark in HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Erstellen und Ausführen von Anwendungen
* [Erstellen einer eigenständigen Anwendung mit Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Remoteausführung von Aufträgen in einem Spark-Cluster mithilfe von Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Tools und Erweiterungen

* [Verwenden von externen Python-Paketen mit Jupyter-Notebooks in Apache Spark-Clustern unter HDInsight (Linux)](hdinsight-apache-spark-python-package-installation.md)
* [Verwenden des HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Erstellen und Übermitteln von Spark Scala-Anwendungen](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Use HDInsight Tools Plugin for IntelliJ IDEA to debug Spark applications remotely (Verwenden von HDInsight-Tools-Plug-Ins für IntelliJ IDEA zum Remotedebuggen von Spark-Anwendungen)](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Verwenden von Zeppelin-Notebooks mit einem Spark-Cluster in HDInsight](hdinsight-apache-spark-use-zeppelin-notebook.md)
* [Verfügbare Kernels für Jupyter-Notebook im Spark-Cluster für HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Installieren von Jupyter Notebook auf Ihrem Computer und Herstellen einer Verbindung zum Apache Spark-Cluster in Azure HDInsight (Vorschau)](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Verwalten von Ressourcen
* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)




<!--HONumber=Jan17_HO4-->



---
title: Analysieren von Daten mit Azure Machine Learning | Microsoft Docs
description: Azure Machine Learning wird verwendet, um ein Predictive Machine Learning-Modell basierend auf in Azure SQL Data Warehouse gespeicherten Daten zu erstellen.
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: barbkess
editor: 
ms.assetid: 95635460-150f-4a50-be9c-5ddc5797f8a9
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 10/31/2016
ms.author: kevin;barbkess
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: edc3a915a59d83718d05ce39a1ce2bcd14333da4


---
# <a name="analyze-data-with-azure-machine-learning"></a>Analysieren von Daten mit Azure Machine Learning
> [!div class="op_single_selector"]
> * [Power BI](sql-data-warehouse-get-started-visualize-with-power-bi.md)
> * [Azure Machine Learning](sql-data-warehouse-get-started-analyze-with-azure-machine-learning.md)
> * [Visual Studio](sql-data-warehouse-query-visual-studio.md)
> * [sqlcmd](sql-data-warehouse-get-started-connect-sqlcmd.md) 
> * [SSMS](sql-data-warehouse-query-ssms.md)
> 
> 

In diesem Tutorial wird Azure Machine Learning verwendet, um ein Predictive Machine Learning-Modell basierend auf in Azure SQL Data Warehouse gespeicherten Daten zu erstellen. Es wird eine gezielte Marketingkampagne für Adventure Works, einen Fahrradladen, erstellt und dabei vorhergesagt, ob ein Kunde ein Fahrrad kauft.

> [!VIDEO https://channel9.msdn.com/Blogs/Windows-Azure/Integrating-Azure-Machine-Learning-with-Azure-SQL-Data-Warehouse/player]
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Für dieses Lernprogramm ist Folgendes erforderlich:

* Für eine SQL Data Warehouse-Instanz werden AdventureWorksDW-Beispieldaten vorab geladen. Informationen zur Bereitstellung finden Sie unter [Erstellen eines SQL Data Warehouse][Erstellen eines SQL Data Warehouse]. Wählen Sie die Option zum Laden der Beispieldaten. Wenn Sie bereits über ein Data Warehouse verfügen, aber noch keine Beispieldaten besitzen, können Sie [Beispieldaten manuell laden][Beispieldaten manuell laden].

## <a name="1-get-data"></a>1. Datensammlung
Die Daten befinden sich in der Sicht „dbo.vTargetMail“ in der AdventureWorksDW-Datenbank. Gehen Sie wie folgt vor, um diese Daten zu lesen:

1. Melden Sie sich bei [Azure Machine Learning Studio][Azure Machine Learning Studio] an, und klicken Sie auf „Meine Experimente“.
2. Klicken Sie auf **+ NEU**, und wählen Sie **Leeres Experiment**.
3. Geben Sie einen Namen für das Experiment ein: Zielgerichtetes Marketing.
4. Ziehen Sie das **Reader** -Modul aus dem Modulbereich in den Zeichenbereich.
5. Geben Sie im Eigenschaftenbereich ausführliche Informationen zu Ihrer SQL Data Warehouse-Datenbank ein.
6. Geben Sie die **Datenbankabfrage** zum Lesen der für Sie interessanten Daten an.

```sql
SELECT [CustomerKey]
  ,[GeographyKey]
  ,[CustomerAlternateKey]
  ,[MaritalStatus]
  ,[Gender]
  ,cast ([YearlyIncome] as int) as SalaryYear
  ,[TotalChildren]
  ,[NumberChildrenAtHome]
  ,[EnglishEducation]
  ,[EnglishOccupation]
  ,[HouseOwnerFlag]
  ,[NumberCarsOwned]
  ,[CommuteDistance]
  ,[Region]
  ,[Age]
  ,[BikeBuyer]
FROM [dbo].[vTargetMail]
```

Führen Sie das Experiment aus, indem Sie unterhalb des Experimentbereichs auf **Ausführen** klicken.
![Ausführen des Experiments][1]

Klicken Sie nach Abschluss des Experiments auf den Ausgabeport im unteren Bereich des Reader-Moduls, und wählen Sie **Visualisieren** , um die importierten Daten anzuzeigen.
![Anzeigen der importierten Daten][3]

## <a name="2-clean-the-data"></a>2. Bereinigen der Daten
Löschen Sie einige Spalten, die für das Modell nicht relevant sind, um die Daten zu bereinigen. Gehen Sie dazu folgendermaßen vor:

1. Ziehen Sie das Modul **Project Columns** in den Zeichenbereich.
2. Klicken Sie im Eigenschaftenbereich auf **Spaltenauswahl starten** , um die zu löschenden Spalten anzugeben.
   ![Project Columns][4]
3. Schließen Sie zwei Spalten aus: CustomerAlternateKey und GeographyKey.
   ![Entfernen überflüssiger Spalten][5]

## <a name="3-build-the-model"></a>3. Erstellen des Modells
Wir teilen die Daten im Verhältnis 80:20: 80 Prozent zum Trainieren eines Machine Learning-Modells und 20 Prozent zum Testen des Modells. Für dieses binäre Klassifizierungsproblem nutzen wir die Zwei-Klassen-Algorithmen.

1. Ziehen Sie das Modul **Split** in den Zeichenbereich.
2. Geben Sie im Eigenschaftenbereich für den Anteil der Zeilen im ersten Ausgabedatensatz „0,8“ ein.
   ![Aufteilen von Daten in Trainings- und Testsätze][6]
3. Ziehen Sie das Modul **Two-Class Boosted Decision Tree** in den Zeichenbereich.
4. Ziehen Sie das Modul **Modell trainieren** in den Zeichenbereich, und legen Sie die Eingaben fest. Klicken Sie im Eigenschaftenbereich auf **Spaltenauswahl starten** .
   * Erste Eingabe: ML-Algorithmus
   * Zweite Eingabe: Daten zum Trainieren des Algorithmus.
     ![Verbinden des „Modell trainieren“-Moduls][7]
5. Wählen Sie die Spalte **BikeBuyer** als die vorherzusagende Spalte aus.
   ![Auswählen der vorherzusagenden Spalte][8]

## <a name="4-score-the-model"></a>4. Bewertung des Modells
Wir testen nun die Leistung des Modells mithilfe von Testdaten. Wir vergleichen zwei Algorithmen, um zu ermitteln, welcher Algorithmus die bessere Leistung erzielt.

1. Ziehen Sie das Modul **Score Model** in den Zeichenbereich.
    Erste Eingabe: Trainiertes Modell. Zweite Eingabe: Testdaten ![Bewertung des Modells][9]
2. Ziehen Sie das Modul **Two-Class Bayes Point Machine** in den Experimentbereich. Wir sehen uns an, wie dieser Algorithmus im Vergleich zum Modul „Two-Class Boosted Decision Tree“ abschneidet.
3. Kopieren Sie die Module „Train Model“ und „Score Model“, und fügen Sie sie im Zeichenbereich ein.
4. Ziehen Sie das Modul **Evaluate Model** in den Zeichenbereich, um die beiden Algorithmen zu vergleichen.
5. **Ausführen** aus.
   ![Ausführen des Experiments][10]
6. Klicken Sie unten im Modul „Modell evaluieren“ auf den Ausgabeport und anschließend auf „Visualisieren“.
   ![Anzeigen der Auswertungsergebnisse][11]

Folgende Metriken stehen zur Verfügung: ROC-Kurve, Precision-Recall-Diagramm und Lorenz-Kurve. Anhand dieser Metriken erkennen wir, dass mit dem ersten Modell eine bessere Leistung als mit dem zweiten Modell erzielt wird. Wenn Sie die Vorhersage des ersten Modells anzeigen möchten, klicken Sie im „Bewertungsmodell“ auf den Ausgabeport und anschließend auf „Visualisieren“.
![Anzeigen der Bewertungsergebnisse][12]

Ihrem Testdatensatz werden zwei weitere Spalten hinzugefügt.

* Bewertete Wahrscheinlichkeiten: Die Wahrscheinlichkeit, dass es sich bei einem Kunden um einen Fahrradkäufer handelt.
* Bewertete Beschriftungen: die vom Modell vorgenommene Klassifizierung – Fahrradkäufer (1) oder kein Fahrradkäufer (0). Der Wahrscheinlichkeitsschwellenwert für die Beschriftung ist auf 50 Prozent festgelegt, kann aber angepasst werden.

Durch einen Vergleich der Spalte „BikeBuyer“ (tatsächliche Werte) mit „Bewertete Beschriftungen“ (Vorhersage) können Sie die Leistung des Modells ermitteln. Als Nächstes können Sie mit diesem Modell Vorhersagen für neue Kunden treffen und das Modell als Webdienst veröffentlichen oder Ergebnisse zurück in SQL Data Warehouse schreiben.

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Erstellen von vorhersehbaren Machine Learning-Vorhersagemodellen finden Sie unter [Einführung in Machine Learning in der Cloud][Einführung in Machine Learning in der Cloud].

<!--Image references-->
[1]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img1_reader.png
[2]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img2_visualize.png
[3]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img3_readerdata.png
[4]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img4_projectcolumns.png
[5]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img5_columnselector.png
[6]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img6_split.png
[7]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img7_train.png
[8]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img8_traincolumnselector.png
[9]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img9_score.png
[10]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img10_evaluate.png
[11]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img11_evalresults.png
[12]: media/sql-data-warehouse-get-started-analyze-with-azure-machine-learning/img12_scoreresults.png


<!--Article references-->
[Azure Machine Learning Studio]:https://studio.azureml.net/
[Einführung in Machine Learning in der Cloud]:https://azure.microsoft.com/documentation/articles/machine-learning-what-is-machine-learning/
[Beispieldaten auch manuell laden]: sql-data-warehouse-load-sample-databases.md
[Erstellen eines SQL Data Warehouse]: sql-data-warehouse-get-started-provision.md



<!--HONumber=Nov16_HO2-->



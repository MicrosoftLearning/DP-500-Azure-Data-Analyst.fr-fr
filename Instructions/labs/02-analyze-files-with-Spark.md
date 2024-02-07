---
lab:
  title: Analyser les données dans un lac de données avec Spark
  module: 'Model, query, and explore data in Azure Synapse'
---

# Analyser les données dans un lac de données avec Spark

Apache Spark est un moteur open source pour le traitement des données distribuées. Il est largement utilisé pour explorer, traiter et analyser d’énormes volumes de données dans Data Lake Storage. Spark est disponible comme option de traitement dans de nombreux produits de plateforme de données, notamment Azure HDInsight, Azure Databricks et Azure Synapse Analytics sur la plateforme cloud Microsoft Azure. L’un des avantages de Spark est la prise en charge d’un large éventail de langages de programmation, notamment Java, Scala, Python et SQL. Cela fait de Spark une solution très flexible pour les charges de travail de traitement des données, y compris le nettoyage et la manipulation des données, l’analyse statistique et le Machine Learning, ainsi que l’analytique données et la visualisation des données.

Ce labo prend environ **45** minutes.

## Avant de commencer

Vous avez besoin d’un [abonnement Azure](https://azure.microsoft.com/free) dans lequel vous avez un accès administratif.

## Provisionner un espace de travail Azure Synapse Analytics

Vous aurez besoin d’un espace de travail Azure Synapse Analytics avec accès au stockage du lac de données et d’un pool Apache Spark que vous pouvez utiliser pour interroger et traiter des fichiers dans le lac de données.

Dans cet exercice, vous allez utiliser la combinaison d’un script PowerShell et d’un modèle ARM pour approvisionner un espace de travail Azure Synapse Analytics.

1. Connectez-vous au [portail Azure](https://portal.azure.com) à l’adresse `https://portal.azure.com`.
2. Utilisez le bouton **[\>_]** à droite de la barre de recherche, en haut de la page, pour créer un environnement Cloud Shell dans le portail Azure, puis sélectionnez un environnement ***PowerShell*** et créez le stockage si vous y êtes invité. Cloud Shell fournit une interface de ligne de commande dans un volet situé en bas du portail Azure, comme illustré ici :

    ![Portail Azure avec un volet Cloud Shell](../images/cloud-shell.png)

    > **Remarque** : si vous avez créé un shell cloud qui utilise un environnement *Bash*, utilisez le menu déroulant en haut à gauche du volet Cloud Shell pour le remplacer par ***PowerShell***.

3. Notez que vous pouvez redimensionner le volet Cloud Shell en faisant glisser la barre de séparation en haut du volet. Vous pouvez aussi utiliser les icônes **&#8212;** , **&#9723;** et **X** situées en haut à droite du volet pour réduire, agrandir et fermer le volet. Pour plus d’informations sur l’utilisation d’Azure Cloud Shell, consultez la [documentation Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

4. Dans le volet PowerShell, entrez les commandes suivantes pour cloner ce référentiel :

    ```
    rm -r dp500 -f
    git clone https://github.com/MicrosoftLearning/DP-500-Azure-Data-Analyst dp500
    ```

5. Une fois le référentiel cloné, entrez les commandes suivantes pour accéder au dossier de ce labo et exécutez le script **setup.ps1** qu’il contient :

    ```
    cd dp500/Allfiles/02
    ./setup.ps1
    ```

6. Si vous y êtes invité, choisissez l’abonnement que vous souhaitez utiliser (uniquement si vous avez accès à plusieurs abonnements Azure).
7. Quand vous y êtes invité, entrez un mot de passe approprié à définir pour votre pool Azure Synapse SQL.

    > **Remarque** : veillez à mémoriser ce mot de passe.

8. Attendez que le script se termine. Cela prend généralement environ 10 minutes, mais dans certains cas, cela peut prendre plus de temps. Pendant que vous patientez, consultez l’article [Apache Spark dans Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/spark/apache-spark-overview) dans la documentation d’Azure Synapse Analytics.

## Interroger des données dans des fichiers

Le script configure un espace de travail Azure Synapse Analytics et un compte Stockage Azure pour héberger le lac de données, puis charge certains fichiers de données dans le lac de données.

### Afficher les fichiers dans le lac de données

1. Une fois le script terminé, dans le portail Azure, accédez au groupe de ressources **dp500-*xxxxxxx*** qu’il a créé, puis sélectionnez votre espace de travail Synapse.
2. Dans la page **Vue d’ensemble** de votre espace de travail Synapse, dans la carte **Ouvrir Synapse Studio**, sélectionnez **Ouvrir** pour ouvrir Synapse Studio dans un nouvel onglet de navigateur. Connectez-vous si vous y êtes invité.
3. Sur le côté gauche de Synapse Studio, utilisez l’icône **&rsaquo;&rsaquo;** pour développer le menu. Cela permet d’afficher les différentes pages de Synapse Studio qui vous permettront de gérer les ressources et d’effectuer des tâches d’analytique de données.
4. Sur la page **Gérer**, sélectionnez l’onglet **Pools Apache Spark** et notez qu’un pool Spark portant un nom similaire à **spark*xxxxxxx*** a été provisionné dans l’espace de travail. Vous utiliserez ce pool Spark plus tard pour charger et analyser des données contenues dans les fichiers du lac de données stockés dans l’espace de travail.
5. Dans la page **Données**, affichez l’onglet **Lié** et vérifiez que votre espace de travail inclut un lien vers votre compte de stockage Azure Data Lake Storage Gen2, qui doit avoir un nom similaire à **synapse*xxxxxxx* (Primary - datalake*xxxxxxx*)**.
6. Développez votre compte de stockage et vérifiez qu’il contient un conteneur de système de fichiers nommé **files**.
7. Sélectionnez le conteneur **files** et notez qu’il contient des dossiers nommés **sales** et **synapse**. Le dossier **synapse** est utilisé par Azure Synapse et le dossier **sales** contient les fichiers de données que vous allez interroger.
8. Ouvrez le dossier **sales** et le dossier **orders** qu’il contient, puis observez que le dossier **orders** contient des fichiers .csv contenant trois ans de données de ventes.
9. Cliquez avec le bouton droit sur l’un des fichiers et sélectionnez **Aperçu** pour afficher les données qu’il contient. Notez que les fichiers ne contiennent pas de ligne d’en-tête. Vous pouvez donc désélectionner l’option permettant d’afficher les en-têtes de colonne.

### Utiliser Spark pour explorer les données

1. Sélectionnez l’un des fichiers dans le dossier **orders**, puis, dans la liste **Nouveau notebook** de la barre d’outils, sélectionnez **Charger vers DataFrame**. Un dataframe est une structure dans Spark qui représente un jeu de données tabulaire.
2. Dans le nouvel onglet **Notebook 1** qui s’ouvre, dans la liste **Joindre à**, sélectionnez votre pool Spark (**spark*xxxxxxx***). Utilisez ensuite le bouton **▷ Exécuter tout** pour exécuter toutes les cellules du notebook (il n’y en a actuellement qu’une seule !).

    Remarque : comme c’est la première fois que vous exécutez du code Spark dans cette session, le pool Spark doit être démarré. Cela signifie que la première exécution dans la session peut prendre quelques minutes. Les exécutions suivantes seront plus rapides.

3. Pendant que vous attendez que la session Spark s’initialise, passez en revue le code généré. Il ressemble au code suivant :

    ```Python
    %%pyspark
    df = spark.read.load('abfss://files@datalakexxxxxxx.dfs.core.windows.net/sales/orders/2019.csv', format='csv'
    ## If header exists uncomment line below
    ##, header=True
    )
    display(df.limit(10))
    ```

4. Une fois que le code a fini de s’exécuter, passez en revue la sortie sous la cellule du notebook. Il affiche les dix premières lignes du fichier que vous avez sélectionné, avec des noms de colonnes automatiques sous la forme **_c0**, **_c1**, **_c2**, etc.
5. Modifiez le code afin que la fonction **spark.read.load** lise les données de <u>tous</u> les fichiers CSV du dossier et que la fonction **display** affiche les 100 premières lignes. Votre code doit ressembler à ceci (*datalakexxxxxxx* correspond au nom de votre magasin de lac de données) :

    ```Python
    %%pyspark
    df = spark.read.load('abfss://files@datalakexxxxxxx.dfs.core.windows.net/sales/orders/*.csv', format='csv'
    )
    display(df.limit(100))
    ```

6. Utilisez le bouton **▷** à gauche de la cellule de code pour exécuter uniquement cette cellule, puis examinez les résultats.

    Le dataframe inclut désormais les données de tous les fichiers, mais les noms des colonnes ne sont pas utiles. Spark utilise une approche « schéma à la lecture » pour essayer de déterminer les types de données appropriés aux colonnes en fonction des données qu’ils contiennent, et si une ligne d’en-tête est présente dans un fichier texte, elle peut être utilisée pour identifier les noms de colonnes (en spécifiant un paramètre **header=True** dans la fonction **load**). Vous pouvez également définir un schéma explicite pour le dataframe.

7. Modifiez le code comme suit (en remplaçant *datalakexxxxxxx),* pour définir un schéma explicite pour le dataframe qui inclut les noms de colonnes et les types de données. Réexécutez le code dans la cellule.

    ```Python
    %%pyspark
    from pyspark.sql.types import *
    from pyspark.sql.functions import *

    orderSchema = StructType([
        StructField("SalesOrderNumber", StringType()),
        StructField("SalesOrderLineNumber", IntegerType()),
        StructField("OrderDate", DateType()),
        StructField("CustomerName", StringType()),
        StructField("Email", StringType()),
        StructField("Item", StringType()),
        StructField("Quantity", IntegerType()),
        StructField("UnitPrice", FloatType()),
        StructField("Tax", FloatType())
        ])

    df = spark.read.load('abfss://files@datalakexxxxxxx.dfs.core.windows.net/sales/orders/*.csv', format='csv', schema=orderSchema)
    display(df.limit(100))
    ```

8. Dans les résultats, utilisez le bouton **+ Code** pour ajouter une nouvelle cellule de code au notebook. Ensuite, dans la nouvelle cellule, ajoutez le code suivant pour afficher le schéma du dataframe :

    ```Python
    df.printSchema()
    ```

9. Exécutez la nouvelle cellule et vérifiez que le schéma du dataframe correspond à la fonction **orderSchema** que vous avez définie. La fonction **printSchema** peut être utile lors de l’utilisation d’un dataframe dont le schéma est automatiquement déduit.

## Analyser les données dans un dataframe

L’objet **dataframe** dans Spark est similaire à un dataframe Pandas dans Python. Il comprend un large éventail de fonctions que vous pouvez utiliser pour manipuler, filtrer, regrouper et analyser les données qu’il contient.

### Filtrer un dataframe

1. Ajoutez une nouvelle cellule de code au notebook, puis entrez-y le code suivant :

    ```Python
    customers = df['CustomerName', 'Email']
    print(customers.count())
    print(customers.distinct().count())
    display(customers.distinct())
    ```

2. Exécutez la nouvelle cellule de code et passez en revue les résultats. Observez les informations suivantes :
    - Lorsque vous effectuez une opération sur un dataframe, le résultat est un nouveau dataframe (dans ce cas, un nouveau dataframe **customers** est créé en sélectionnant un sous-ensemble spécifique de colonnes dans le dataframe **df**).
    - Les dataframes fournissent des fonctions telles que **count** et **distinct** qui peuvent être utilisées pour résumer et filtrer les données qu’ils contiennent.
    - La syntaxe `dataframe['Field1', 'Field2', ...]` offre un moyen rapide de définir un sous-ensemble de colonnes. Vous pouvez également utiliser la méthode **select**, pour que la première ligne du code ci-dessus puisse être écrite sous la forme `customers = df.select("CustomerName", "Email")`

3. Modifiez le code comme suit :

    ```Python
    customers = df.select("CustomerName", "Email").where(df['Item']=='Road-250 Red, 52')
    print(customers.count())
    print(customers.distinct().count())
    display(customers.distinct())
    ```

4. Exécutez ce code modifié pour afficher les clients qui ont acheté le produit *Road-250 Red, 52*. Notez que vous pouvez « chaîner » plusieurs fonctions afin que la sortie d’une fonction devienne l’entrée de la suivante. Dans ce cas, le dataframe créé par la méthode **select** est le dataframe source de la méthode **where** utilisée pour appliquer des critères de filtrage.

### Agréger et regrouper des données dans un dataframe

1. Ajoutez une nouvelle cellule de code au notebook, puis entrez-y le code suivant :

    ```Python
    productSales = df.select("Item", "Quantity").groupBy("Item").sum()
    display(productSales)
    ```

2. Exécutez la cellule de code que vous avez ajoutée et remarquez que les résultats affichent la somme des quantités de commandes regroupées par produit. La méthode **groupBy** regroupe les lignes par *Item*, et la fonction d’agrégation **sum** suivante est appliquée à toutes les colonnes numériques restantes (dans ce cas, *Quantity*).

3. Ajoutez encore une nouvelle cellule de code au notebook, puis entrez-y le code suivant :

    ```Python
    yearlySales = df.select(year("OrderDate").alias("Year")).groupBy("Year").count().orderBy("Year")
    display(yearlySales)
    ```

4. Exécutez la cellule de code que vous avez ajoutée et notez que les résultats indiquent le nombre de commandes client par an. Notez que la méthode **select** inclut une fonction SQL **year** pour extraire le composant année du champ *OrderDate*, puis une méthode **alias** est utilisée pour affecter un nom de colonne à la valeur année extraite. Les données sont ensuite regroupées par la colonne *Year* dérivée et le nombre de lignes dans chaque groupe est calculé avant que la méthode **orderBy** soit finalement utilisée pour trier le dataframe résultant.

## Interroger des données à l’aide de Spark SQL

Comme vous l’avez vu, les méthodes natives de l’objet dataframe vous permettent d’interroger et d’analyser des données de manière assez efficace. Toutefois, de nombreux analystes de données sont plus à l’aise avec la syntaxe SQL. Spark SQL est une API de langage SQL dans Spark que vous pouvez utiliser pour exécuter des instructions SQL, ou même pour conserver des données dans des tables relationnelles.

### Utiliser Spark SQL dans le code PySpark

Le langage par défaut dans les notebooks Azure Synapse Studio est PySpark, qui est un runtime Python basé sur Spark. Dans ce runtime, vous pouvez utiliser la bibliothèque **spark.sql** pour incorporer la syntaxe Spark SQL dans votre code Python et utiliser des constructions SQL telles que des tables et des vues.

1. Ajoutez une nouvelle cellule de code au notebook, puis entrez-y le code suivant :

    ```Python
    df.createOrReplaceTempView("salesorders")

    spark_df = spark.sql("SELECT * FROM salesorders")
    display(spark_df)
    ```

2. Exécutez la cellule et passez en revue les résultats. Observez que :
    - Le code conserve les données dans le dataframe **df** comme une vue temporaire nommée **salesorders**. Spark SQL prend en charge l’utilisation de vues temporaires ou de tables persistantes en tant que sources pour les requêtes SQL.
    - La méthode **spark.sql** est ensuite utilisée pour exécuter une requête SQL sur la vue **salesorders**.
    - Les résultats de la requête sont enregistrés dans un dataframe.

### Exécuter du code SQL dans une cellule

Bien qu’il soit utile d’incorporer des instructions SQL dans une cellule contenant du code PySpark, les analystes de données veulent souvent simplement travailler directement dans SQL.

1. Ajoutez une nouvelle cellule de code au notebook, puis entrez-y le code suivant :

    ```sql
    %%sql
    SELECT YEAR(OrderDate) AS OrderYear,
           SUM((UnitPrice * Quantity) + Tax) AS GrossRevenue
    FROM salesorders
    GROUP BY YEAR(OrderDate)
    ORDER BY OrderYear;
    ```

2. Exécutez la cellule et passez en revue les résultats. Observez que :
    - La ligne `%%sql` au début de la cellule (appelée *commande magique*) indique que le runtime de langage Spark SQL doit être utilisé à la place de PySpark pour exécuter le code dans cette cellule.
    - Le code SQL référence la vue **salesorders** que vous avez créée précédemment à l’aide de PySpark.
    - La sortie de la requête SQL s’affiche automatiquement en tant que résultat sous la cellule.

> **Remarque** : Pour plus d’informations sur Spark SQL et les dataframes, consultez la [documentation Spark SQL](https://spark.apache.org/docs/2.2.0/sql-programming-guide.html).

## Visualiser les données avec Spark

Selon le proverbe, une image vaut mille mots, et un graphique exprime souvent plus qu’un millier de lignes de données. Bien que les notebooks d’Azure Synapse Analytics comprennent une vue graphique intégrée pour les données provenant d’un dataframe ou d’une requête Spark SQL, cette vue n’ pas été conçue pour la création de graphiques complets. Toutefois, vous pouvez utiliser les bibliothèques graphiques Python telles que **matplotlib** et **seaborn** pour créer des graphiques à partir de données dans des dataframes.

### Afficher les résultats sous forme de graphique

1. Ajoutez une nouvelle cellule de code au notebook, puis entrez-y le code suivant :

    ```sql
    %%sql
    SELECT * FROM salesorders
    ```

2. Exécutez ce code et observez qu’il retourne les données de la vue **salesorders** que vous avez créée précédemment.
3. Dans la section des résultats sous la cellule, modifiez l’option **Affichage** de **Tableau** à **Graphique**.
4. Utilisez le bouton **Options d’affichage** en haut à droite du graphique pour afficher le volet d’options du graphique. Définissez ensuite les options comme suit et sélectionnez **Appliquer** :
    - **Type de graphique** : Graphique à barres
    - **Clé** : Élément
    - **Valeurs** : Quantité
    - **Groupe de séries** : *laissez vide*
    - **Agrégation** : Somme
    - **Empilé** : *Non sélectionné*

5. Vérifiez que le graphique ressemble à ceci :

    ![Graphique à barres de produits par quantités totales des commandes](../images/notebook-chart.png)

### Bien démarrer avec **matplotlib**

1. Ajoutez une nouvelle cellule de code au notebook, puis entrez-y le code suivant :

    ```Python
    sqlQuery = "SELECT CAST(YEAR(OrderDate) AS CHAR(4)) AS OrderYear, \
                    SUM((UnitPrice * Quantity) + Tax) AS GrossRevenue \
                FROM salesorders \
                GROUP BY CAST(YEAR(OrderDate) AS CHAR(4)) \
                ORDER BY OrderYear"
    df_spark = spark.sql(sqlQuery)
    df_spark.show()
    ```

2. Exécutez le code et observez qu’il retourne un dataframe Spark contenant le chiffre d’affaires annuel.

    Pour visualiser les données sous forme graphique, nous allons commencer en utilisant la bibliothèque Python **matplotlib**. Cette bibliothèque est la bibliothèque de traçage principale sur laquelle de nombreuses autres bibliothèques sont basées, et elle offre une grande flexibilité dans la création de graphiques.

3. Ajoutez une nouvelle cellule de code au notebook, puis ajoutez-y le code suivant :

    ```Python
    from matplotlib import pyplot as plt

    # matplotlib requires a Pandas dataframe, not a Spark one
    df_sales = df_spark.toPandas()

    # Create a bar plot of revenue by year
    plt.bar(x=df_sales['OrderYear'], height=df_sales['GrossRevenue'])

    # Display the plot
    plt.show()
    ```

4. Exécutez la cellule et passez en revue les résultats, qui se composent d’un histogramme indiquant le chiffre d’affaires brut total pour chaque année. Notez les fonctionnalités suivantes du code utilisé pour produire ce graphique :
    - La bibliothèque **matplotlib** nécessite un dataframe *Pandas*. Vous devez donc convertir le dataframe *Spark* retourné par la requête Spark SQL dans ce format.
    - Au cœur de la bibliothèque **matplotlib** figure l’objet **pyplot**. Il s’agit de la base de la plupart des fonctionnalités de traçage.
    - Les paramètres par défaut aboutissent à un graphique utilisable, mais il existe de nombreuses façons de le personnaliser.

5. Modifiez le code pour tracer le graphique comme suit :

    ```Python
    # Clear the plot area
    plt.clf()

    # Create a bar plot of revenue by year
    plt.bar(x=df_sales['OrderYear'], height=df_sales['GrossRevenue'], color='orange')

    # Customize the chart
    plt.title('Revenue by Year')
    plt.xlabel('Year')
    plt.ylabel('Revenue')
    plt.grid(color='#95a5a6', linestyle='--', linewidth=2, axis='y', alpha=0.7)
    plt.xticks(rotation=45)

    # Show the figure
    plt.show()
    ```

6. Réexécutez la cellule de code et examinez les résultats. Le graphique inclut désormais un peu plus d’informations.

    Un tracé est techniquement contenu dans une **figure**. Dans les exemples précédents, la figure a été créée implicitement pour vous, mais vous pouvez la créer explicitement.

7. Modifiez le code pour tracer le graphique comme suit :

    ```Python
    # Clear the plot area
    plt.clf()

    # Create a Figure
    fig = plt.figure(figsize=(8,3))

    # Create a bar plot of revenue by year
    plt.bar(x=df_sales['OrderYear'], height=df_sales['GrossRevenue'], color='orange')

    # Customize the chart
    plt.title('Revenue by Year')
    plt.xlabel('Year')
    plt.ylabel('Revenue')
    plt.grid(color='#95a5a6', linestyle='--', linewidth=2, axis='y', alpha=0.7)
    plt.xticks(rotation=45)

    # Show the figure
    plt.show()
    ```

8. Réexécutez la cellule de code et examinez les résultats. La figure détermine la forme et la taille du tracé.

    Une figure peut contenir plusieurs sous-tracés, chacun sur son propre *axe*.

9. Modifiez le code pour tracer le graphique comme suit :

    ```Python
    # Clear the plot area
    plt.clf()

    # Create a figure for 2 subplots (1 row, 2 columns)
    fig, ax = plt.subplots(1, 2, figsize = (10,4))

    # Create a bar plot of revenue by year on the first axis
    ax[0].bar(x=df_sales['OrderYear'], height=df_sales['GrossRevenue'], color='orange')
    ax[0].set_title('Revenue by Year')

    # Create a pie chart of yearly order counts on the second axis
    yearly_counts = df_sales['OrderYear'].value_counts()
    ax[1].pie(yearly_counts)
    ax[1].set_title('Orders per Year')
    ax[1].legend(yearly_counts.keys().tolist())

    # Add a title to the Figure
    fig.suptitle('Sales Data')

    # Show the figure
    plt.show()
    ```

10. Réexécutez la cellule de code et examinez les résultats. La figure contient les sous-tracés spécifiés dans le code.

> **Remarque** : Pour en savoir plus sur le traçage avec matplotlib, consultez la [documentation matplotlib](https://matplotlib.org/).

### Utiliser la bibliothèque **seaborn**

Bien que **matplotlib** vous permette de créer des graphiques complexes de plusieurs types, il peut nécessiter du code complexe pour obtenir les meilleurs résultats. Pour cette raison, au fil des ans, de nombreuses nouvelles bibliothèques ont été construites sur la base de matplotlib pour en extraire la complexité et améliorer ses capacités. L’une de ces bibliothèques est **seaborn**.

1. Ajoutez une nouvelle cellule de code au notebook, puis entrez-y le code suivant :

    ```Python
    import seaborn as sns

    # Clear the plot area
    plt.clf()

    # Create a bar chart
    ax = sns.barplot(x="OrderYear", y="GrossRevenue", data=df_sales)
    plt.show()
    ```

2. Exécutez le code et observez qu’il affiche un graphique à barres utilisant la bibliothèque seaborn.
3. Ajoutez une nouvelle cellule de code au notebook, puis entrez-y le code suivant :

    ```Python
    # Clear the plot area
    plt.clf()

    # Set the visual theme for seaborn
    sns.set_theme(style="whitegrid")

    # Create a bar chart
    ax = sns.barplot(x="OrderYear", y="GrossRevenue", data=df_sales)
    plt.show()
    ```

4. Exécutez le code modifié et notez que seaborn vous permet de définir un thème de couleur cohérent pour vos tracés.

5. Ajoutez une nouvelle cellule de code au notebook, puis entrez-y le code suivant :

    ```Python
    # Clear the plot area
    plt.clf()

    # Create a bar chart
    ax = sns.lineplot(x="OrderYear", y="GrossRevenue", data=df_sales)
    plt.show()
    ```

6. Exécutez le code pour afficher le chiffre d’affaires annuel sous forme de graphique en courbes.

> **Remarque** : Pour en savoir plus sur le traçage avec seaborn, consultez la [documentation seaborn](https://seaborn.pydata.org/index.html).

## Supprimer les ressources Azure

Si vous avez fini d’explorer Azure Synapse Analytics, vous devriez supprimer les ressources que vous avez créées afin d’éviter des coûts Azure inutiles.

1. Fermez l’onglet du navigateur Synapse Studio et revenez dans le portail Azure.
2. Dans le portail Azure, dans la page **Accueil**, sélectionnez **Groupes de ressources**.
3. Sélectionnez le groupe de ressources **dp500-*xxxxxxx*** pour votre espace de travail Synapse Analytics (et non le groupe de ressources managé) et vérifiez qu’il contient l’espace de travail Synapse, le compte de stockage et le pool Spark correspondant.
4. Au sommet de la page **Vue d’ensemble** de votre groupe de ressources, sélectionnez **Supprimer le groupe de ressources**.
5. Entrez le nom du groupe de ressources **dp500-*xxxxxxx*** pour confirmer que vous souhaitez le supprimer, puis sélectionnez **Supprimer**.

    Après quelques minutes, le groupe de ressources de l’espace de travail Azure Synapse et le groupe de ressources managé de l’espace de travail qui lui est associé seront supprimés.

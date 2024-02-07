---
lab:
  title: Interroger des fichiers avec un pool SQL serverless
  module: 'Model, query, and explore data in Azure Synapse'
---

# Interroger des fichiers avec un pool SQL serverless

SQL est probablement le langage le plus utilisé au monde pour travailler avec des données. La plupart des analystes de données savent utiliser des requêtes SQL pour récupérer, filtrer et agréger des données, le plus souvent dans des bases de données relationnelles. Alors que les organisations tirent de plus en plus profit du stockage de fichiers évolutif pour créer des lacs de données, SQL reste souvent le choix préféré pour interroger les données. Azure Synapse Analytics fournit des pools SQL serverless qui vous permettent de dissocier le moteur de requête SQL du stockage de données et d’exécuter des requêtes sur des fichiers de données dans des formats de fichiers courants tels que du texte délimité et Parquet.

Ce labo prend environ **40** minutes.

## Avant de commencer

Vous avez besoin d’un [abonnement Azure](https://azure.microsoft.com/free) dans lequel vous avez un accès administratif.

## Provisionner un espace de travail Azure Synapse Analytics

Vous aurez besoin d’un espace de travail Azure Synapse Analytics avec accès à Data Lake Storage. Vous pouvez utiliser le pool SQL serverless intégré pour interroger des fichiers dans le lac de données.

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
    cd dp500/Allfiles/01
    ./setup.ps1
    ```

6. Si vous y êtes invité, choisissez l’abonnement que vous souhaitez utiliser (uniquement si vous avez accès à plusieurs abonnements Azure).
7. Quand vous y êtes invité, entrez un mot de passe approprié à définir pour votre pool Azure Synapse SQL.

    > **Remarque** : veillez à mémoriser ce mot de passe.

8. Attendez que le script se termine. Cela prend généralement environ 10 minutes, mais dans certains cas, cela peut prendre plus de temps. Pendant que vous attendez, consultez l’article [Pool SQL serverless dans Azure Synapse Analytics](https://docs.microsoft.com/azure/synapse-analytics/sql/on-demand-workspace-overview) dans la documentation d’Azure Synapse Analytics.

## Interroger des données dans des fichiers

Le script configure un espace de travail Azure Synapse Analytics et un compte Stockage Azure pour héberger le lac de données, puis charge certains fichiers de données dans le lac de données.

### Afficher les fichiers dans le lac de données

1. Une fois le script terminé, dans le portail Azure, accédez au groupe de ressources **dp500-*xxxxxxx*** qu’il a créé, puis sélectionnez votre espace de travail Synapse.
2. Dans la page **Vue d’ensemble** de votre espace de travail Synapse, dans la carte **Ouvrir Synapse Studio**, sélectionnez **Ouvrir** pour ouvrir Synapse Studio dans un nouvel onglet de navigateur. Connectez-vous si vous y êtes invité.
3. Sur le côté gauche de Synapse Studio, utilisez l’icône **&rsaquo;&rsaquo;** pour développer le menu. Cela permet d’afficher les différentes pages de Synapse Studio qui vous permettront de gérer les ressources et d’effectuer des tâches d’analytique de données.
4. Dans la page **Données**, affichez l’onglet **Lié** et vérifiez que votre espace de travail inclut un lien vers votre compte de stockage Azure Data Lake Storage Gen2, qui doit avoir un nom similaire à **synapse*xxxxxxx* (Primary - datalake*xxxxxxx*)**.
5. Développez votre compte de stockage et vérifiez qu’il contient un conteneur de système de fichiers nommé **files**.
6. Sélectionnez le conteneur **files**, et constatez qu’il contient un dossier nommé **sales**. Ce dossier contient les fichiers de données que vous allez interroger.
7. Ouvrez le dossier **sales** et le dossier **csv** qu’il contient, puis observez que ce dossier contient des fichiers .csv correspondant à trois années de données de vente.
8. Cliquez avec le bouton droit sur l’un des fichiers et sélectionnez **Aperçu** pour afficher les données qu’il contient. Notez que les fichiers ne contiennent pas de ligne d’en-tête. Vous pouvez donc désélectionner l’option permettant d’afficher les en-têtes de colonne.
9. Fermez l’aperçu, puis utilisez le bouton **↑** pour revenir au dossier **sales**.
10. Dans le dossier **sales**, ouvrez le dossier **json** et observez qu’il contient des exemples de commandes client dans des fichiers .json. Affichez un aperçu de l’un de ces fichiers pour voir le format JSON utilisé pour une commande client.
11. Fermez l’aperçu, puis utilisez le bouton **↑** pour revenir au dossier **sales**.
12. Dans le dossier **sales**, ouvrez le dossier **parquet** et notez qu’il contient un sous-dossier pour chaque année (2019 à 2021). Dans chacun d’eux, un fichier nommé **orders.snappy.parquet** contient les données de commande de cette année. 
13. Revenez au dossier **sales** pour afficher les dossiers **csv**, **json** et **parquet**.

### Utiliser SQL pour interroger des fichiers CSV

1. Sélectionnez le dossier **csv**, puis, dans la liste **Nouveau script SQL** de la barre d’outils, cliquez sur **Sélectionner les 100 premières lignes**.
2. Dans la liste **Type de fichier**, sélectionnez **Format texte**, puis appliquez les paramètres pour ouvrir un nouveau script SQL qui interroge les données dans le dossier.
3. Dans le volet **Propriétés** du script **SQL Script 1** qui est créé, remplacez le nom par **Sales CSV query** et modifiez les paramètres de résultat pour afficher **Toutes les lignes**. Ensuite, dans la barre d’outils, sélectionnez **Publier** pour enregistrer le script et utilisez le bouton **Propriétés** (qui ressemble à **.**) à droite de la barre d’outils pour masquer le volet **Propriétés**.
4. Passez en revue le code SQL qui a été généré, qui doit ressembler à ceci :

    ```SQL
    -- This is auto-generated code
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/csv/',
            FORMAT = 'CSV',
            PARSER_VERSION='2.0'
        ) AS [result]
    ```

    Ce code utilise OPENROWSET pour lire les données des fichiers CSV dans le dossier sales et récupère les 100 premières lignes de données.

5. Dans la liste **Se connecter à**, vérifiez que l’option **Intégré** est cochée : cela représente le pool SQL intégré qui a été créé avec votre espace de travail.
6. Dans la barre d’outils, utilisez le bouton **&#9655; Exécuter** pour exécuter le code SQL, puis passez en revue les résultats, qui doivent ressembler à ceci :

    | C1 | C2 | C3 | C4 | C5 | C6 | C7 | C8 | C9 |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | SO45347 | 1 | 2020-01-01 | Clarence Raji | clarence35@adventure-works.com |Road-650 Noir, 52 | 1 | 699,0982 | 55,9279 |
    | ... | ... | ... | ... | ... | ... | ... | ... | ... |

7. Notez que les résultats se composent de colonnes C1, C2, etc. Dans cet exemple, les fichiers CSV n’incluent pas les en-têtes de colonne. Bien qu’il soit possible d’utiliser les données à l’aide des noms de colonnes génériques qui ont été attribués ou par position ordinale, il sera plus facile de comprendre les données si vous définissez un schéma tabulaire. Pour ce faire, ajoutez une clause WITH à la fonction OPENROWSET comme indiqué ici (en remplaçant *datalakexxxxx* par le nom de votre compte Data Lake Storage), puis réexécutez la requête :

    ```SQL
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/csv/',
            FORMAT = 'CSV',
            PARSER_VERSION='2.0'
        )
        WITH (
            SalesOrderNumber VARCHAR(10) COLLATE Latin1_General_100_BIN2_UTF8,
            SalesOrderLineNumber INT,
            OrderDate DATE,
            CustomerName VARCHAR(25) COLLATE Latin1_General_100_BIN2_UTF8,
            EmailAddress VARCHAR(50) COLLATE Latin1_General_100_BIN2_UTF8,
            Item VARCHAR(30) COLLATE Latin1_General_100_BIN2_UTF8,
            Quantity INT,
            UnitPrice DECIMAL(18,2),
            TaxAmount DECIMAL (18,2)
        ) AS [result]
    ```

    À présent, les résultats ressemblent à ceci :

    | SalesOrderNumber | SalesOrderLineNumber | OrderDate | CustomerName | EmailAddress | Élément | Quantité | UnitPrice | TaxAmount |
    | -- | -- | -- | -- | -- | -- | -- | -- | -- |
    | SO45347 | 1 | 2020-01-01 | Clarence Raji | clarence35@adventure-works.com |Road-650 Noir, 52 | 1 | 699.10 | 55.93 |
    | ... | ... | ... | ... | ... | ... | ... | ... | ... |

8. Publiez les modifications apportées à votre script, puis fermez le volet de script.

### Utiliser SQL pour interroger des fichiers parquet

Bien que le format CSV soit un format facile à utiliser, il est courant dans les scénarios de traitement big data d’utiliser des formats de fichiers optimisés pour la compression, l’indexation et le partitionnement. L’un de ces formats les plus courants est *parquet*.

1. Dans l’onglet **fichiers** qui contient le système de fichiers de votre lac de données, revenez au dossier **sales** pour afficher les dossiers **csv**, **json** et **parquet**.
2. Sélectionnez le dossier **parquet**, puis, dans la liste **Nouveau script SQL** de la barre d’outils, cliquez sur **Sélectionner les 100 premières lignes**.
3. Dans la liste **Type de fichier**, sélectionnez **Format Parquet**, puis appliquez les paramètres pour ouvrir un nouveau script SQL qui interroge les données dans le dossier. Le script doit ressembler à ceci :

    ```SQL
    -- This is auto-generated code
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/parquet/**',
            FORMAT = 'PARQUET'
        ) AS [result]
    ```

4. Exécutez le code et vérifiez qu’il retourne des données de commande dans le même schéma que les fichiers CSV que vous avez explorés précédemment. Les informations de schéma sont incorporées dans le fichier parquet, de sorte que les noms de colonnes appropriés sont affichés dans les résultats.
5. Modifiez le code comme suit (en remplaçant *datalakexxxxxxx* par le nom de votre compte Data Lake Storage), puis exécutez-le.

    ```sql
    SELECT YEAR(OrderDate) AS OrderYear,
           COUNT(*) AS OrderedItems
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/parquet/**',
            FORMAT = 'PARQUET'
        ) AS [result]
    GROUP BY YEAR(OrderDate)
    ORDER BY OrderYear
    ```

6. Notez que les résultats incluent le nombre de commandes des trois années. Le caractère générique utilisé dans le chemin d’accès BULK permet à la requête de renvoyer les données de tous les sous-dossiers.

    Les sous-dossiers reflètent les *partitions* dans les données parquet. Cette technique est souvent utilisée pour optimiser les performances des systèmes capables de traiter plusieurs partitions de données en parallèle. Vous pouvez également utiliser des partitions pour filtrer les données.

7. Modifiez le code comme suit (en remplaçant *datalakexxxxxxx* par le nom de votre compte Data Lake Storage), puis exécutez-le.

    ```sql
    SELECT YEAR(OrderDate) AS OrderYear,
           COUNT(*) AS OrderedItems
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/parquet/year=*/',
            FORMAT = 'PARQUET'
        ) AS [result]
    WHERE [result].filepath(1) IN ('2019', '2020')
    GROUP BY YEAR(OrderDate)
    ORDER BY OrderYear
    ```

8. Passez en revue les résultats et notez qu’ils incluent uniquement les ventes des années 2019 et 2020. Ce filtrage est obtenu en utilisant un caractère générique pour la valeur du dossier de partition dans le chemin d’accès BULK (*year=\**) et une clause WHERE basée sur la propriété *filepath* des résultats retournés par OPENROWSET (qui, dans ce cas, a l’alias *[result]*).

9. Nommez votre script **Sales Parquet query** et publiez-le. Fermez ensuite le volet de script.

### Utiliser SQL pour interroger des fichiers JSON

JSON est un autre format de données populaire. Il est donc utile pour pouvoir interroger des fichiers .json dans un pool SQL serverless.

1. Sous l’onglet **fichiers** qui contient le système de fichiers de votre lac de données, revenez au dossier **sales** pour afficher les dossiers **csv**, **json** et **parquet**.
2. Sélectionnez le dossier **json**, puis, dans la liste **Nouveau script SQL** de la barre d’outils, cliquez sur **Sélectionner les 100 premières lignes**.
3. Dans la liste **Type de fichier**, sélectionnez **Format texte**, puis appliquez les paramètres pour ouvrir un nouveau script SQL qui interroge les données dans le dossier. Le script doit ressembler à ceci :

    ```sql
    -- This is auto-generated code
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/json/',
            FORMAT = 'CSV',
            PARSER_VERSION = '2.0'
        ) AS [result]
    ```

    Le script a été conçu pour interroger des données délimitées par des virgules (CSV) plutôt que JSON. Vous devez donc apporter quelques modifications pour que tout fonctionne correctement.

4. Modifiez le script comme suit (en remplaçant *datalakexxxxxxx* par le nom de votre compte Data Lake Storage) :
    - Supprimez le paramètre de version de l’analyseur.
    - Ajoutez des paramètres pour la marque de fin de champ, les champs entre guillemets et les marques de fin de ligne avec le code de caractère *0x0b*.
    - Présentez les résultats comme un champ unique contenant la ligne de données JSON sous forme de chaîne NVARCHAR(MAX).

    ```sql
    SELECT
        TOP 100 *
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/json/',
            FORMAT = 'CSV',
            FIELDTERMINATOR ='0x0b',
            FIELDQUOTE = '0x0b',
            ROWTERMINATOR = '0x0b'
        ) WITH (Doc NVARCHAR(MAX)) as rows
    ```

5. Exécutez le code modifié et observez que les résultats incluent un document JSON pour chaque commande.

6. Modifiez la requête comme suit (en remplaçant *datalakexxxxxxx* par le nom de votre compte Data Lake Storage) afin qu’elle utilise la fonction JSON_VALUE pour extraire des valeurs de champ individuelles des données JSON.

    ```sql
    SELECT JSON_VALUE(Doc, '$.SalesOrderNumber') AS OrderNumber,
           JSON_VALUE(Doc, '$.CustomerName') AS Customer,
           Doc
    FROM
        OPENROWSET(
            BULK 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/json/',
            FORMAT = 'CSV',
            FIELDTERMINATOR ='0x0b',
            FIELDQUOTE = '0x0b',
            ROWTERMINATOR = '0x0b'
        ) WITH (Doc NVARCHAR(MAX)) as rows
    ```

7. Nommez votre script **Sales JSON query ** et publiez-le. Fermez ensuite le volet de script.

## Accéder aux données externes dans une base de données

Jusqu’à présent, vous avez utilisé la fonction OPENROWSET dans une requête SELECT pour récupérer des données provenant de fichiers se trouvant dans un lac de données. Les requêtes ont été exécutées dans le contexte de la base de données **master** dans votre pool SQL serverless. Cette approche convient pour une exploration initiale des données, mais si vous envisagez de créer des requêtes plus complexes, il peut être plus efficace d’utiliser la fonctionnalité *PolyBase* de Synapse SQL pour créer des objets dans une base de données qui fait référence à l’emplacement externe des données.

### Créer une source de données externe

En définissant une source de données externe dans une base de données, vous pouvez l’utiliser pour référencer l’emplacement du lac de données où les fichiers sont stockés.

1. Dans Synapse Studio, sur la page **Développer**, dans le menu **+**, sélectionnez **Script SQL**.
2. Dans le volet du nouveau script, ajoutez le code suivant (en remplaçant *datalakexxxxxxx* par le nom de votre compte Data Lake Storage) pour créer une base de données et y ajouter une source de données externe.

    ```sql
    CREATE DATABASE Sales
      COLLATE Latin1_General_100_BIN2_UTF8;
    GO;

    Use Sales;
    GO;

    CREATE EXTERNAL DATA SOURCE sales_data WITH (
        LOCATION = 'https://datalakexxxxxxx.dfs.core.windows.net/files/sales/'
    );
    GO;
    ```

3. Modifiez les propriétés du script pour remplacer son nom par **Create Sales DB** et publiez-le.
4. Vérifiez que le script est connecté au pool SQL **Built-in** et à la base de données **master**, puis exécutez-le.
5. Revenez à la page **Données** et utilisez le bouton **↻** en haut à droite de Synapse Studio pour actualiser la page. Affichez ensuite l’onglet **Espace de travail** dans le volet **Données**, qui affiche à présent une liste de **bases de données SQL**. Développez cette liste pour vérifier que la base de données **Sales** a été créée.
6. Développez la base de données **Sales**, son dossier **External Resources** et le dossier **External data sources** sous celui-ci pour afficher la source de données externe **sales_data** que vous avez créée.
7. Dans le menu **...** de la base de données **Sales**, sélectionnez **Nouveau script SQL** > **Script vide**. Ensuite, dans le volet du nouveau script, entrez et exécutez la requête suivante :

    ```sql
    SELECT *
    FROM
        OPENROWSET(
            BULK 'csv/*.csv',
            DATA_SOURCE = 'sales_data',
            FORMAT = 'CSV',
            PARSER_VERSION = '2.0'
        ) AS orders
    ```

    La requête utilise la source de données externe pour se connecter au lac de données et la fonction OPENROWSET doit maintenant uniquement référencer le chemin d’accès relatif aux fichiers .csv.

8. Modifiez le code comme suit pour interroger les fichiers parquet à l’aide de la source de données.

    ```sql
    SELECT *
    FROM  
        OPENROWSET(
            BULK 'parquet/year=*/*.snappy.parquet',
            DATA_SOURCE = 'sales_data',
            FORMAT='PARQUET'
        ) AS orders
    WHERE orders.filepath(1) = '2019'
    ```

### Créer une table externe

La source de données externe facilite l’accès aux fichiers dans le lac de données, mais la plupart des analystes de données utilisant SQL ont l’habitude de travailler avec des tables dans une base de données. Heureusement, vous pouvez également définir des formats de fichiers externes et des tables externes qui encapsulent des ensembles de lignes de fichiers dans des tables de base de données.

1. Remplacez le code SQL par l’instruction suivante pour définir un format de données externe pour les fichiers CSV et une table externe qui fait référence aux fichiers CSV, puis exécutez-le :

    ```sql
    CREATE EXTERNAL FILE FORMAT CsvFormat
        WITH (
            FORMAT_TYPE = DELIMITEDTEXT,
            FORMAT_OPTIONS(
            FIELD_TERMINATOR = ',',
            STRING_DELIMITER = '"'
            )
        );
    GO;

    CREATE EXTERNAL TABLE dbo.orders
    (
        SalesOrderNumber VARCHAR(10),
        SalesOrderLineNumber INT,
        OrderDate DATE,
        CustomerName VARCHAR(25),
        EmailAddress VARCHAR(50),
        Item VARCHAR(30),
        Quantity INT,
        UnitPrice DECIMAL(18,2),
        TaxAmount DECIMAL (18,2)
    )
    WITH
    (
        DATA_SOURCE =sales_data,
        LOCATION = 'csv/*.csv',
        FILE_FORMAT = CsvFormat
    );
    GO
    ```

2. Actualisez et développez le dossier **Tables externes** dans le volet **Données** et vérifiez qu’une table nommée **dbo.orders** a été créée dans la base de données **Sales**.
3. Dans le menu **...** de la table **dbo.orders**, sélectionnez **Nouveau script SQL** > **Sélectionner les 100 premières lignes**.
4. Exécutez le script SELECT qui a été généré et vérifiez qu’il récupère les 100 premières lignes de données de la table, qui à leur tour font référence aux fichiers du lac de données.

    >**Remarque :** vous devez toujours choisir la méthode qui convient le mieux à vos besoins et cas d’usage spécifiques. Pour plus d’informations, consultez les articles [Comment utiliser OPENROWSET avec le pool SQL serverless dans Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql/develop-openrowset) et [Accéder à un stockage externe en utilisant le pool SQL serverless dans Azure Synapse Analytics](https://learn.microsoft.com/en-us/azure/synapse-analytics/sql/develop-storage-files-overview?tabs=impersonation).

## Visualiser les résultats de requête

Maintenant que vous avez exploré différentes façons d’interroger des fichiers dans le lac de données à l’aide de requêtes SQL, vous pouvez analyser les résultats de ces requêtes pour obtenir des informations sur les données. Souvent, les informations sont plus faciles à comprendre en visualisant les résultats de la requête dans un graphique, ce que vous pouvez facilement faire à l’aide de la fonctionnalité de graphique intégrée à l’éditeur de requête de Synapse Studio.

1. Dans la page **Développer**, créez une requête SQL vide.
2. Vérifiez que le script est connecté au pool SQL **Built-in** et à la base de données **Sales**.
3. Modifiez et exécutez le code SQL suivant :

    ```sql
    SELECT YEAR(OrderDate) AS OrderYear,
           SUM((UnitPrice * Quantity) + TaxAmount) AS GrossRevenue
    FROM dbo.orders
    GROUP BY YEAR(OrderDate)
    ORDER BY OrderYear;
    ```

4. Dans le volet **Résultats**, sélectionnez **Graphique** et affichez le graphique qui est créé pour vous. Il doit s’agir d’un graphique en courbes.
5. Remplacez la colonne **Category** par **OrderYear** afin que le graphique en courbes affiche la tendance des revenus sur la période de trois ans allant de 2019 à 2021 :

    ![Graphique en courbes affichant les revenus par an](../images/yearly-sales-line.png)

6. Définissez le **Type de graphique** sur **Colonne** pour afficher les revenus annuels sous forme d’histogramme :

    ![Histogramme affichant les revenus par an](../images/yearly-sales-column.png)

7. Testez la fonctionnalité de graphique dans l’éditeur de requête. Elle offre des outils graphiques de base que vous pouvez utiliser pour explorer les données de manière interactive et vous permet d’enregistrer des graphiques sous forme d’images à inclure dans les rapports. Toutefois, la fonctionnalité est limitée par rapport aux outils de visualisation des données d’entreprise tels que Microsoft Power BI.

## Supprimer les ressources Azure

Si vous avez fini d’explorer Azure Synapse Analytics, vous devriez supprimer les ressources que vous avez créées afin d’éviter des coûts Azure inutiles.

1. Fermez l’onglet du navigateur Synapse Studio et revenez dans le portail Azure.
2. Dans le portail Azure, dans la page **Accueil**, sélectionnez **Groupes de ressources**.
3. Sélectionnez le groupe de ressources **dp500-*xxxxxxx*** pour votre espace de travail Synapse Analytics (et non le groupe de ressources managées) et vérifiez qu’il contient l’espace de travail Synapse et le compte de stockage de votre espace de travail.
4. Au sommet de la page **Vue d’ensemble** de votre groupe de ressources, sélectionnez **Supprimer le groupe de ressources**.
5. Entrez le nom du groupe de ressources **dp500-*xxxxxxx*** pour confirmer que vous souhaitez le supprimer, puis sélectionnez **Supprimer**.

    Après quelques minutes, le groupe de ressources de l’espace de travail Azure Synapse et le groupe de ressources managé de l’espace de travail qui lui est associé seront supprimés.

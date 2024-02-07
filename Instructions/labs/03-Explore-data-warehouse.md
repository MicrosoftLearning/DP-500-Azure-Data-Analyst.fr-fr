---
lab:
  title: Explorer un entrepôt de données relationnelles
  module: 'Model, query, and explore data in Azure Synapse'
---

# Explorer un entrepôt de données relationnelles

Azure Synapse Analytics s’appuie sur un ensemble de capacités évolutives pour prendre en charge l’entreposage de données d’entreprise, y compris l’analyse de données basées sur des fichiers dans un lac de données, ainsi que les entrepôts de données relationnelles à grande échelle et les pipelines de transfert et de transformation de données utilisés pour les charger. Dans ce labo, vous allez découvrir comment utiliser un pool SQL dédié dans Azure Synapse Analytics pour stocker et interroger des données dans un entrepôt de données relationnelles.

Ce labo prend environ **45** minutes.

## Avant de commencer

Vous avez besoin d’un [abonnement Azure](https://azure.microsoft.com/free) dans lequel vous avez un accès administratif.

## Provisionner un espace de travail Azure Synapse Analytics

Un *espace de travail* Azure Synapse Analytics constitue un point central pour la gestion des données et des temps d’exécution du traitement des données. Vous pouvez mettre à disposition un espace de travail à l’aide de l’interface interactive du portail Azure, ou vous pouvez déployer un espace de travail et les ressources qu’il contient à l’aide d’un script ou d’un modèle. Dans la plupart des scénarios de production, il est préférable d’automatiser la mise à disposition à l’aide de scripts et de modèles afin d’intégrer le déploiement des ressources dans un processus de développement et d’opérations reproductibles (*DevOps*).

Dans cet exercice, vous allez utiliser une combinaison d’un script PowerShell et d’un modèle ARM pour la mise à disposition d’Azure Synapse Analytics.

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
    cd dp500/Allfiles/03
    ./setup.ps1
    ```

6. Si vous y êtes invité, choisissez l’abonnement que vous souhaitez utiliser (uniquement si vous avez accès à plusieurs abonnements Azure).
7. Quand vous y êtes invité, entrez un mot de passe approprié à définir pour votre pool Azure Synapse SQL.

    > **Remarque** : veillez à mémoriser ce mot de passe.

8. Attendez que le script se termine. Cela prend généralement environ 15 minutes, mais dans certains cas, cela peut prendre plus de temps. Pendant que vous patientez, consultez l’article [Qu’est-ce qu’un pool SQL dédié dans Azure Synapse Analytics ?](https://docs.microsoft.com/azure/synapse-analytics/sql-data-warehouse/sql-data-warehouse-overview-what-is) dans la documentation d’Azure Synapse Analytics.

## Explorer le schéma de l’entrepôt de données

Dans ce labo, l’entrepôt de données est hébergé dans un pool SQL dédié dans Azure Synapse Analytics.

### Démarrer le pool SQL dédié

1. Une fois le script terminé, dans le portail Azure, accédez au groupe de ressources **dp500-*xxxxxxx*** qu’il a créé, puis sélectionnez votre espace de travail Synapse.
2. Dans la page **Vue d’ensemble** de votre espace de travail Synapse, dans la carte **Ouvrir Synapse Studio**, sélectionnez **Ouvrir** pour ouvrir Synapse Studio dans un nouvel onglet de navigateur. Connectez-vous si vous y êtes invité.
3. Sur le côté gauche de Synapse Studio, utilisez l’icône **&rsaquo;&rsaquo;** pour développer le menu. Cela permet d’afficher les différentes pages de Synapse Studio qui vous permettront de gérer les ressources et d’effectuer des tâches d’analyse de données.
4. Sur la page **Gérer**, vérifiez que l’onglet **Pools SQL** est sélectionné, puis sélectionnez le pool SQL dédié **sql*xxxxxxx*** et utilisez son icône **▷** pour le démarrer. À l’invite, confirmez que vous souhaitez le réactiver.
5. Attendez que le pool SQL se réactive. Cette opération peut prendre quelques minutes. Utilisez le bouton **↻ Actualiser** pour vérifier son état régulièrement. L’état affiche  **En ligne** lorsqu’il est prêt.

### Afficher les tables de la base de données

1. Dans Synapse Studio, sélectionnez la page **Données** et vérifiez que l’onglet **Espace de travail** est sélectionné et contient une catégorie **Base de données SQL**.
2. Développez **Base de données SQL**, le pool **sql*xxxxxxx*** et son dossier **Tables** pour afficher les tables de la base de données.

    Un entrepôt de données relationnelles est généralement basé sur un schéma qui comprend des tables de *faits* et de *dimension*. Les tables sont optimisées pour les requêtes d’analyse dans lesquelles les mesures numériques des tables de faits sont agrégées par les attributs des entités représentées par les tables de dimension. Cela vous permet, par exemple, d’agréger les revenus des ventes en ligne par produit, client, date, etc.
    
3. Développez la table **dbo.FactInternetSales** et son dossier **Columns** pour afficher les colonnes de cette table. Notez que la plupart des colonnes sont des *clés* qui font référence à des lignes dans les tables de dimension. D’autres sont des valeurs numériques (*mesures*) pour l’analyse.
    
    Les clés sont utilisées pour lier une table de faits à une ou plusieurs tables de dimension, souvent dans un *schéma en étoile* dans laquelle la table de faits est directement liée à chaque table de dimension (formant une « étoile » à plusieurs branches avec la table de faits au centre).

4. Examinez les colonnes de la table **dbo.DimPromotion** et notez la clé **PromotionKey** qui identifie de manière unique chaque ligne de la table. Notez également la clé **AlternateKey**.

    En règle générale, les données d’un entrepôt de données ont été importées à partir d’une ou plusieurs sources transactionnelles. La clé *alternative* reflète l’identificateur métier de l’instance de cette entité dans la source, mais une clé de *substitution* numérique unique est généralement générée pour identifier de manière unique chaque ligne de la table de dimension de l’entrepôt de données. L’un des avantages de cette approche est qu’elle permet à l’entrepôt de données de contenir plusieurs instances de la même entité à différents moments (par exemple, des enregistrements pour le même client reflétant son adresse au moment où une commande a été passée).

5. Examinez les colonnes de la table **dbo.DimProduct** et notez qu’elle contient une colonne **ProductSubcategoryKey**, qui fait référence à la table **dbo.DimProductSubcategory**, qui contient à son tour une colonne **ProductCategoryKey** qui fait référence à la table **dbo.DimProductCategory**.

    Dans certains cas, les dimensions sont partiellement normalisées dans plusieurs tables associées pour permettre différents niveaux de granularité. Par exemple, les produits peuvent être regroupés en sous-catégories et en catégories. Il en résulte qu’une simple étoile est étendue à un schéma de type *flocon*, dans lequel la table de faits centrale est liée à une table de dimension, qui est liée à d’autres tables de dimension.

6. Examinez les colonnes de la table **dbo.DimDate** et notez qu’elle contient plusieurs colonnes qui reflètent différents attributs temporels d’une date, y compris le jour de la semaine, le jour du mois, le mois, l’année, le nom du jour, le nom du mois, etc.

    Les dimensions temporelles d’un entrepôt de données sont généralement implémentées en tant que table de dimension contenant une ligne pour chacune des plus petites unités temporelles de granularité (souvent appelées *grain* de la dimension) par lesquelles vous souhaitez agréger les mesures dans les tables de faits. Dans ce cas, le grain le plus bas auquel les mesures peuvent être agrégées est une date individuelle, et la table contient une ligne pour chaque date de la première à la dernière date référencée dans les données. Les attributs de la table **DimDate** permettent aux analystes d’agréger des mesures basées sur n’importe quelle clé de date dans la table de faits, à l’aide d’un ensemble cohérent d’attributs temporels (par exemple, l’affichage des commandes par mois en fonction de la date de commande). La table **FactInternetSales** contient trois clés liées à la table **DimDate** : **OrderDateKey**, **DueDateKey** et **ShipDateKey**.

## Interroger les tables de l’entrepôt de données

Maintenant que vous avez exploré certains des aspects les plus importants du schéma de l’entrepôt de données, vous êtes prêt à interroger les tables et à extraire des données.

### Interroger les tables de faits et de dimension

Les valeurs numériques dans un entrepôt de données relationnelles sont stockées dans des tables de faits avec des tables de dimension associées que vous pouvez utiliser pour agréger les données entre plusieurs attributs. La plupart des requêtes dans un entrepôt de données relationnelles impliquent l’agrégation et le regroupement de données (à l’aide des fonctions d’agrégation et des clauses GROUP BY) entre des tables liées (à l’aide des clauses JOIN).

1. Sur la page **Données**, sélectionnez le pool SQL **sql*xxxxxxx*** et, dans son menu **...**, sélectionnez **Nouveau script SQL** > **Script vide**.
2. Lorsqu’un nouvel onglet **Script SQL 1** s’ouvre, dans son volet **Propriétés**, remplacez le nom du script par **Analyse des ventes en ligne** et modifiez les **Paramètres de résultat par requête** pour renvoyer toutes les lignes. Utilisez ensuite le bouton **Publier** dans la barre d’outils pour enregistrer le script, puis le bouton **Propriétés** (qui ressemble à **.**) à droite de la barre d’outils pour fermer le volet **Propriétés** et voir le volet Script.
3. Dans le script vide, ajoutez le code suivant :

    ```sql
    SELECT  d.CalendarYear AS Year,
            SUM(i.SalesAmount) AS InternetSalesAmount
    FROM FactInternetSales AS i
    JOIN DimDate AS d ON i.OrderDateKey = d.DateKey
    GROUP BY d.CalendarYear
    ORDER BY Year;
    ```

4. Utilisez le bouton **▷ Exécuter** pour exécuter le script et examinez les résultats qui doivent montrer le total des ventes en ligne de chaque année. Cette requête relie la table des faits des ventes en ligne à une table de dimension temporelle sur la base de la date de la commande, et agrège la mesure du montant des ventes dans la table des faits sur la base de l’attribut du mois civil de la table de dimension.

5. Modifiez la requête comme suit pour ajouter l’attribut mois à partir de la dimension temps, puis exécutez la requête modifiée.

    ```sql
    SELECT  d.CalendarYear AS Year,
            d.MonthNumberOfYear AS Month,
            SUM(i.SalesAmount) AS InternetSalesAmount
    FROM FactInternetSales AS i
    JOIN DimDate AS d ON i.OrderDateKey = d.DateKey
    GROUP BY d.CalendarYear, d.MonthNumberOfYear
    ORDER BY Year, Month;
    ```

    Notez que les attributs de la dimension temps vous permettent d’agréger les mesures de la table de faits à plusieurs niveaux hiérarchiques, dans le cas présent année et mois. C’est un modèle courant dans les entrepôts de données.

6. Modifiez la requête comme suit pour supprimer le mois et ajouter une deuxième dimension à l’agrégation, puis exécutez-la pour afficher les résultats (qui affichent les totaux annuels des ventes en ligne pour chaque région) :

    ```sql
    SELECT  d.CalendarYear AS Year,
            g.EnglishCountryRegionName AS Region,
            SUM(i.SalesAmount) AS InternetSalesAmount
    FROM FactInternetSales AS i
    JOIN DimDate AS d ON i.OrderDateKey = d.DateKey
    JOIN DimCustomer AS c ON i.CustomerKey = c.CustomerKey
    JOIN DimGeography AS g ON c.GeographyKey = g.GeographyKey
    GROUP BY d.CalendarYear, g.EnglishCountryRegionName
    ORDER BY Year, Region;
    ```

    Notez que la zone géographique est une dimension en *flocon* qui est liée à la table de faits des ventes en ligne par le biais de la dimension client. Vous avez donc besoin de deux jointures dans la requête pour agréger les ventes en ligne par zone géographique.

7. Modifiez et réexécutez la requête pour ajouter une autre dimension en flocon et agréger les ventes régionales annuelles par catégorie de produit :

    ```sql
    SELECT  d.CalendarYear AS Year,
            pc.EnglishProductCategoryName AS ProductCategory,
            g.EnglishCountryRegionName AS Region,
            SUM(i.SalesAmount) AS InternetSalesAmount
    FROM FactInternetSales AS i
    JOIN DimDate AS d ON i.OrderDateKey = d.DateKey
    JOIN DimCustomer AS c ON i.CustomerKey = c.CustomerKey
    JOIN DimGeography AS g ON c.GeographyKey = g.GeographyKey
    JOIN DimProduct AS p ON i.ProductKey = p.ProductKey
    JOIN DimProductSubcategory AS ps ON p.ProductSubcategoryKey = ps.ProductSubcategoryKey
    JOIN DimProductCategory AS pc ON ps.ProductCategoryKey = pc.ProductCategoryKey
    GROUP BY d.CalendarYear, pc.EnglishProductCategoryName, g.EnglishCountryRegionName
    ORDER BY Year, ProductCategory, Region;
    ```

    Cette fois, la dimension en flocon pour la catégorie de produit nécessite trois jointures pour refléter la relation hiérarchique entre les produits, les sous-catégories et les catégories.

8. Publiez le script pour l’enregistrer.

### Utiliser les fonctions de classement

Une autre exigence courante lors de l’analyse de grands volumes de données consiste à regrouper les données par partitions et à déterminer le *classement* de chaque entité dans la partition en fonction d’une métrique spécifique.

1. Sous la requête existante, ajoutez le code SQL suivant pour extraire les valeurs des ventes de l’année 2022 sur différentes partitions en fonction du nom du pays/de la région :

    ```sql
    SELECT  g.EnglishCountryRegionName AS Region,
            ROW_NUMBER() OVER(PARTITION BY g.EnglishCountryRegionName
                              ORDER BY i.SalesAmount ASC) AS RowNumber,
            i.SalesOrderNumber AS OrderNo,
            i.SalesOrderLineNumber AS LineItem,
            i.SalesAmount AS SalesAmount,
            SUM(i.SalesAmount) OVER(PARTITION BY g.EnglishCountryRegionName) AS RegionTotal,
            AVG(i.SalesAmount) OVER(PARTITION BY g.EnglishCountryRegionName) AS RegionAverage
    FROM FactInternetSales AS i
    JOIN DimDate AS d ON i.OrderDateKey = d.DateKey
    JOIN DimCustomer AS c ON i.CustomerKey = c.CustomerKey
    JOIN DimGeography AS g ON c.GeographyKey = g.GeographyKey
    WHERE d.CalendarYear = 2022
    ORDER BY Region;
    ```

2. Sélectionnez uniquement le nouveau code de requête et utilisez le bouton **▷ Exécuter** pour l’exécuter. Les résultats doivent ressembler à la table suivante :

    | Région | RowNumber | OrderNo | LineItem | SalesAmount | RegionTotal | RegionAverage |
    |--|--|--|--|--|--|--|
    |Australie|1|SO73943|2|2.2900|2172278.7900|375.8918|
    |Australie|2|SO74100|4|2.2900|2172278.7900|375.8918|
    |...|...|...|...|...|...|...|
    |Australie|5779|SO64284|1|2443.3500|2172278.7900|375.8918|
    |Canada|1|SO66332|2|2.2900|563177.1000|157.8411|
    |Canada|2|SO68234|2|2.2900|563177.1000|157.8411|
    |...|...|...|...|...|...|...|
    |Canada|3568|SO70911|1|2443.3500|563177.1000|157.8411|
    |France|1|SO68226|3|2.2900|816259.4300|315.4016|
    |France|2|SO63460|2|2.2900|816259.4300|315.4016|
    |...|...|...|...|...|...|...|
    |France|2588|SO69100|1|2443.3500|816259.4300|315.4016|
    |Allemagne|1|SO70829|3|2.2900|922368.2100|352.4525|
    |Allemagne|2|SO71651|2|2.2900|922368.2100|352.4525|
    |...|...|...|...|...|...|...|
    |Allemagne|2617|SO67908|1|2443.3500|922368.2100|352.4525|
    |Royaume-Uni|1|SO66124|3|2.2900|1051560.1000|341.7484|
    |Royaume-Uni|2|SO67823|3|2.2900|1051560.1000|341.7484|
    |...|...|...|...|...|...|...|
    |Royaume-Uni|3077|SO71568|1|2443.3500|1051560.1000|341.7484|
    |États-Unis|1|SO74796|2|2.2900|2905011.1600|289.0270|
    |États-Unis|2|SO65114|2|2.2900|2905011.1600|289.0270|
    |...|...|...|...|...|...|...|
    |États-Unis|10051|SO66863|1|2443.3500|2905011.1600|289.0270|

    Observez les faits suivants sur ces résultats :

    - Il existe une ligne pour chaque poste de commande client.
    - Les lignes sont organisées en partitions en fonction de la zone géographique où la vente a été effectuée.
    - Les lignes de chaque partition géographique sont numérotées en fonction du montant des ventes (du montant le plus faible au montant le plus élevé).
    - Chaque ligne comprend le montant des ventes en ligne ainsi que les montants totaux et moyens des ventes régionales.

3. Sous les requêtes existantes, ajoutez le code suivant pour appliquer des fonctions de fenêtrage dans une requête GROUP BY et classer les villes de chaque région en fonction de leur montant total de ventes :

    ```sql
    SELECT  g.EnglishCountryRegionName AS Region,
            g.City,
            SUM(i.SalesAmount) AS CityTotal,
            SUM(SUM(i.SalesAmount)) OVER(PARTITION BY g.EnglishCountryRegionName) AS RegionTotal,
            RANK() OVER(PARTITION BY g.EnglishCountryRegionName
                        ORDER BY SUM(i.SalesAmount) DESC) AS RegionalRank
    FROM FactInternetSales AS i
    JOIN DimDate AS d ON i.OrderDateKey = d.DateKey
    JOIN DimCustomer AS c ON i.CustomerKey = c.CustomerKey
    JOIN DimGeography AS g ON c.GeographyKey = g.GeographyKey
    GROUP BY g.EnglishCountryRegionName, g.City
    ORDER BY Region;
    ```

4. Sélectionnez uniquement le nouveau code de requête et utilisez le bouton **▷ Exécuter** pour l’exécuter. Examinez les résultats et observez ce qui suit :
    - Les résultats incluent une ligne pour chaque ville, regroupée par région.
    - Le total des ventes (somme des montants de ventes individuels) est calculé pour chaque ville.
    - Le total des ventes régionales (somme de la somme des montants des ventes de chaque ville de la région) est calculé en fonction de la partition régionale.
    - Le classement de chaque ville dans sa partition régionale est calculé en triant le montant total des ventes par ville par ordre décroissant.

5. Publiez le script mis à jour pour enregistrer les modifications.

> **Conseil** : ROW_NUMBER et RANK sont des exemples de fonctions de classement disponibles dans Transact-SQL. Pour plus d’informations, consultez la section [Fonctions de classement](https://docs.microsoft.com/sql/t-sql/functions/ranking-functions-transact-sql) de la documentation relative au language Transact-SQL.

### Extraire un nombre approximatif

Lorsque l’on explore de très grands volumes de données, l’exécution des requêtes peut prendre beaucoup de temps et de ressources. Souvent, l’analyse des données ne nécessite pas des valeurs totalement précises : une comparaison des valeurs approximatives peut suffire.

1. Sous les requêtes existantes, ajoutez le code suivant pour extraire le nombre de commandes de vente de chaque année civile :

    ```sql
    SELECT d.CalendarYear AS CalendarYear,
        COUNT(DISTINCT i.SalesOrderNumber) AS Orders
    FROM FactInternetSales AS i
    JOIN DimDate AS d ON i.OrderDateKey = d.DateKey
    GROUP BY d.CalendarYear
    ORDER BY CalendarYear;
    ```

2. Sélectionnez uniquement le nouveau code de requête et utilisez le bouton **▷ Exécuter** pour l’exécuter. Examinez les résultats qui sont renvoyés :
    - Sous l’onglet **Résultats** de la requête, consultez le nombre de commandes pour chaque année.
    - Sous l’onglet **Messages**, examinez le temps d’exécution total de la requête.
3. Modifiez la requête comme suit pour renvoyer un nombre approximatif pour chaque année. Exécutez la requête à nouveau.

    ```sql
    SELECT d.CalendarYear AS CalendarYear,
        APPROX_COUNT_DISTINCT(i.SalesOrderNumber) AS Orders
    FROM FactInternetSales AS i
    JOIN DimDate AS d ON i.OrderDateKey = d.DateKey
    GROUP BY d.CalendarYear
    ORDER BY CalendarYear;
    ```

4. Examinez les résultats qui sont renvoyés :
    - Sous l’onglet **Résultats** de la requête, consultez le nombre de commandes pour chaque année. Ces résultats devraient se situer à moins de 2 % des chiffres réels obtenus par la requête précédente.
    - Sous l’onglet **Messages**, examinez le temps d’exécution total de la requête. Cette requête doit être plus courte que la requête précédente.

5. Publiez le script pour enregistrer les modifications.

> **Conseil :** pour plus d’informations, consultez la documentation au sujet de la fonction [APPROX_COUNT_DISTINCT](https://docs.microsoft.com/sql/t-sql/functions/approx-count-distinct-transact-sql).

## Défi - Analyser les ventes des revendeurs

1. Créez un script vide pour le pool SQL **sql*xxxxxxx*** et enregistrez-le sous le nom **Analyze Reseller Sales**.
2. Créez des requêtes SQL dans le script pour rechercher les informations suivantes sur la base de la table de faits **FactResellerSales** et des tables de dimension associées :
    - Quantité totale d’articles vendus par année fiscale et trimestre.
    - Quantité totale d’articles vendus par année fiscale, trimestre et région de vente associés à l’employé qui a effectué la vente.
    - Quantité totale d’articles vendus par année fiscale, trimestre et région de vente par catégorie de produit.
    - Classement de chaque territoire de vente par année fiscale en fonction du montant total des ventes de l’année.
    - Nombre approximatif de commandes par année dans chaque territoire de vente.

    > **Conseil** : comparez vos requêtes aux requêtes du script **Solution** sur la page **Développer** de Synapse Studio.

3. Expérimentez des requêtes pour explorer les autres tables du schéma de l’entrepôt de données à votre guise.
4. Lorsque vous avez terminé, sur la page **Gérer**, mettez le pool SQL dédié **sql*xxxxxxx*** en pause.

## Supprimer les ressources Azure

Si vous avez fini d’explorer Azure Synapse Analytics, vous devriez supprimer les ressources que vous avez créées afin d’éviter des coûts Azure inutiles.

1. Fermez l’onglet du navigateur Synapse Studio et revenez dans le portail Azure.
2. Dans le portail Azure, dans la page **Accueil**, sélectionnez **Groupes de ressources**.
3. Sélectionnez le groupe de ressources **dp500-*xxxxxxx*** pour votre espace de travail Synapse Analytics (et non le groupe de ressources managé) et vérifiez qu’il contient l’espace de travail Synapse, le compte de stockage et le pool SQL dédié pour votre espace de travail.
4. Au sommet de la page **Vue d’ensemble** de votre groupe de ressources, sélectionnez **Supprimer le groupe de ressources**.
5. Entrez le nom du groupe de ressources **dp500-*xxxxxxx*** pour confirmer que vous souhaitez le supprimer, puis sélectionnez **Supprimer**.

    Après quelques minutes, le groupe de ressources de l’espace de travail Azure Synapse et le groupe de ressources managé de l’espace de travail qui lui est associé seront supprimés.

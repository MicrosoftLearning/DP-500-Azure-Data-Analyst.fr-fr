---
lab:
  title: Créer un modèle de schéma en étoile
  module: Prepare data for tabular models in Power BI
---

# Créer un modèle de schéma en étoile

## Vue d’ensemble

**La durée estimée pour effectuer ce labo est de 30 minutes.**

Dans ce labo, vous allez utiliser Power BI Desktop pour développer un modèle de données sur l’entrepôt de données Azure Synapse Adventure Works. Le modèle de données vous permet de publier une couche sémantique sur l’entrepôt de données.

Dans ce labo, vous découvrez comment :

- Créer une connexion Power BI à un pool SQL Azure Synapse Analytics

- Développer des requêtes de modèle

- Organiser le diagramme du modèle

## Prise en main

Dans cet exercice, préparez votre environnement.

### Charger des données dans Azure Synapse Analytics

   > **Remarque** : si vous avez déjà chargé des données dans Azure Synapse Analytics à l’aide d’un clone git, vous pouvez ignorer cette tâche et passer à **Configurer Power BI.**

1. Connectez-vous au [portail Azure](https://portal.azure.com) à l’aide des informations de connexion situées sous l’onglet Ressources, à droite de la machine virtuelle.
2. Utilisez le bouton **[\>_]** à droite de la barre de recherche, en haut de la page, pour créer un environnement Cloud Shell dans le portail Azure, puis sélectionnez un environnement ***PowerShell*** et créez le stockage si vous y êtes invité. Cloud Shell fournit une interface de ligne de commande dans un volet situé en bas du portail Azure, comme illustré ici :

    ![Portail Azure avec un volet Cloud Shell](../images/cloud-shell.png)

    > **Remarque** : si vous avez créé un shell cloud qui utilise un environnement *Bash*, utilisez le menu déroulant en haut à gauche du volet Cloud Shell pour le remplacer par ***PowerShell***.

3. Notez que vous pouvez redimensionner le volet Cloud Shell en faisant glisser la barre de séparation en haut du volet. Vous pouvez aussi utiliser les icônes **&#8212;** , **&#9723;** et **X** situées en haut à droite du volet pour réduire, agrandir et fermer le volet. Pour plus d’informations sur l’utilisation d’Azure Cloud Shell, consultez la [documentation Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

4. Dans le volet PowerShell, entrez la commande suivante pour cloner ce référentiel :

    ```
    rm -r dp500 -f
    git clone https://github.com/MicrosoftLearning/DP-500-Azure-Data-Analyst dp500
    ```

5. Une fois le référentiel cloné, entrez les commandes suivantes pour accéder au dossier **setup** et exécutez le script **setup.ps1** qu’il contient :

    ```
    cd dp500/Allfiles/04
    ./setup.ps1
    ```

6. Quand vous y êtes invité, entrez un mot de passe approprié à définir pour votre pool Azure Synapse SQL.

    > **Remarque** : veillez à mémoriser ce mot de passe.

7. Attendez que le script se termine. Cela prend généralement environ 20 minutes, ou plus dans certains cas.
8. Après avoir créé l’espace de travail Synapse et le pool SQL et chargé les données, le script met en pause le pool pour éviter les frais inutiles dans Azure. Lorsque vous êtes prêt à utiliser vos données dans Azure Synapse Analytics, vous devez réactiver le pool SQL.

### Cloner le référentiel pour cette formation

1. Dans le menu Démarrer, ouvrez l’invite de commandes.

    ![](../images/command-prompt.png)
2. Dans la fenêtre d’invite de commandes, accédez au lecteur D en tapant :

    `d:`

   Appuyez sur Entrée.

    ![](../images/command-prompt-2.png)

3. Dans la fenêtre d’invite de commandes, entrez la commande ci-après pour télécharger les fichiers de cours et les enregistrer dans un dossier appelé DP500.

   `git clone https://github.com/MicrosoftLearning/DP-500-Azure-Data-Analyst DP500`

4. Une fois le référentiel cloné, ouvrez le lecteur D dans l’explorateur de fichiers pour vous assurer que les fichiers ont été téléchargés.

### Configurer Power BI

Dans cette tâche, vous allez configurer Power BI.

1. Pour ouvrir Power BI Desktop, accédez à la barre des tâches et sélectionnez le raccourci vers **Power BI Desktop**.

2. Fermez la fenêtre de démarrage en sélectionnant le **X** en haut à droite.

3. Dans le coin supérieur droit de Power BI Desktop, si vous n’êtes pas déjà connecté, sélectionnez **Se connecter**. Utilisez les informations d’identification du labo pour terminer le processus de connexion.

4. Vous êtes redirigé vers la page d’inscription de Power BI dans Microsoft Edge. Sélectionnez **Continuer** pour terminer l’inscription.

   ![](../images/dp500-create-a-star-schema-model-image3b.png)

5. Entrez un numéro de téléphone à 10 chiffres et sélectionnez **Prise en main**. Sélectionnez **Prise en main** à nouveau. Vous êtes redirigé vers Power BI.

6. En haut à droite, sélectionnez l’icône de profil, puis sélectionnez **Démarrer la version d’évaluation**.

   ![](../images/dp500-create-a-dataflow-image3.png)

7. Lorsque vous y êtes invité, sélectionnez **Démarrer la version d’évaluation**.

8. Effectuez les tâches restantes pour terminer la configuration de la version d’évaluation.

   *Conseil : l’expérience du navigateur web de Power BI est appelée **service Power BI**.*

9. Sélectionnez Espaces de travail, puis **Créer un espace de travail**.

    ![](../images/dp500-create-a-star-schema-model-image2a.png)

10. Créez un espace de travail nommé DP500 labs, puis sélectionnez **Enregistrer**.

    *Remarque : le nom de l’espace de travail doit être unique au sein d’un locataire. Si une erreur s’affiche, modifiez le nom de l’espace de travail.*

11. Revenez à Power BI Desktop. Si vous voyez **Se connecter** dans le coin supérieur droit de l’écran, reconnectez-vous à l’aide des informations d’identification fournies sous l’onglet Ressources de l’environnement du labo. Si vous êtes déjà connecté, passez à l’étape suivante.

12. Accédez à Power BI Desktop, sélectionnez **Fichier**, puis **Options et paramètres**. Sélectionnez **Options**, puis **Sécurité** et, sous Navigateur d’authentification, cochez **Utiliser mon navigateur web par défaut**, puis sélectionnez **OK**. Fermez Power BI Desktop. N’enregistrez pas votre fichier.

    *Vous allez rouvrir Power BI Desktop dans l’exercice suivant.*

### Démarrer le pool SQL

Dans cette tâche, vous allez démarrer le pool SQL.

1. Dans Microsoft Edge, accédez à [https://portal.azure.com](https://portal.azure.com/).

1. Utilisez les informations d’identification du labo pour terminer le processus de connexion.

1. Sélectionnez **Azure Synapse Analytics** parmi les services Azure. Sélectionnez votre espace de travail Synapse.

   ![](../images/dp500-create-a-star-schema-model-image3c.png)

1. Recherchez le pool SQL dédié, puis sélectionnez-le.

   ![](../images/dp500-create-a-star-schema-model-image3d.png)

1. Réactivez le pool SQL.

   ![](../images/dp500-create-a-star-schema-model-image3e.png)

   *Important : le pool SQL est une ressource coûteuse. Limitez l’utilisation de cette ressource lorsque vous travaillez sur ce labo. La tâche finale de ce labo vous demandera de mettre la ressource en pause.*

### Lier votre espace de travail Power BI à Azure Synapse Analytics

Dans cette tâche, vous allez lier votre espace de travail Power BI existant à votre espace de travail Azure Synapse Analytics.

1. Dans le pool SQL dédié dans le portail Azure, sélectionnez **Ouvrir dans Synapse Studio** dans le ruban.

1. Sur la page d’accueil d’Azure Synapse Studio, sélectionnez **Visualiser** pour lier votre espace de travail Power BI.

   ![](../images/dp500-create-a-star-schema-model-image3f.png)

1. Dans la liste déroulante **Nom de l’espace de travail**, sélectionnez l’espace de travail que vous avez créé dans la tâche précédente, puis sélectionnez **Créer**.

 ![](../images/dp500-create-a-star-schema-model-image3g.png)

![](../images/dp500-create-a-star-schema-model-image3h.png)

1. Accédez à **Gérer** et sélectionnez **Publier tout** pour vous assurer que les modifications sont publiées.

## Développer un modèle de données

Dans cet exercice, vous allez développer un modèle DirectQuery pour prendre en charge l’analyse et la création de rapports avec Power BI sur le thème des ventes aux revendeurs de l’entrepôt de données.

### Télécharger un fichier de jeu de données

Dans cette tâche, vous allez télécharger un fichier de source de données Power BI à partir de Synapse Studio.

1. Dans **Synapse Studio**, à gauche, sélectionnez **Développer**.

 ![](../images/dp500-create-a-star-schema-model-image4.png)

2. Dans le volet **Développer**, développez **Power BI**, puis l’espace de travail et sélectionnez **Jeux de données Power BI**. S’il n’est pas présent, cliquez sur **Publier tout** pour publier l’espace de travail et actualiser le navigateur.

 ![](../images/dp500-create-a-star-schema-model-image5.png)

 *Remarque : si vous ne voyez aucune donnée ici, vérifiez que votre pool SQL dédié est en cours d’exécution et que votre espace de travail Power BI est lié à votre espace de travail Synapse.*

3. Dans le volet **Jeux de données Power BI**, sélectionnez **Nouveau jeu de données Power BI**.

 ![](../images/dp500-create-a-star-schema-model-image6.png)

4. Dans le volet gauche, en bas, sélectionnez **Démarrer**.

 ![](../images/dp500-create-a-star-schema-model-image7.png)

5. Sélectionnez votre pool SQL, **sqldw**, puis **Continuer**.

 ![](../images/dp500-create-a-star-schema-model-image8.png)

6. Pour télécharger le fichier .pbids, sélectionnez **Télécharger**.

 ![](../images/dp500-create-a-star-schema-model-image9.png)

 *Un fichier .pbids contient une connexion à votre pool SQL. Il s’agit d’un moyen pratique de démarrer votre projet. Une fois ouvert, il crée une solution Power BI Desktop qui contient déjà les détails de connexion à votre pool SQL.*

7. Une fois le fichier .pbids téléchargé, ouvrez-le.

 *Lorsque le fichier s’ouvre, il vous invite à créer des requêtes à l’aide de la connexion. Vous allez définir ces requêtes dans la tâche suivante.*

### Créer des requêtes de modèle

Dans cette tâche, vous allez créer cinq requêtes Power Query qui seront chargées chacune sous forme de table dans votre modèle.

1. Dans Power BI Desktop, dans la fenêtre **Base de données SQL Server**, à gauche, sélectionnez **Compte Microsoft**.

 ![](../images/dp500-create-a-star-schema-model-image10.png)

2. Sélectionnez **Connexion**.

3. Connectez-vous à l’aide des informations d’identification Azure fournies par le labo.

4. Cliquez sur **Se connecter**.

 ![](../images/dp500-create-a-star-schema-model-image11.png)

5. Dans la fenêtre **Navigateur**, sélectionnez la table **DimDate** (sans la cocher).

6. Dans le volet droit, notez le résultat de l’aperçu, qui affiche un sous-ensemble des lignes de la table.

 ![](../images/dp500-create-a-star-schema-model-image12.png)

7. Pour créer des requêtes (qui deviendront des tables de modèle), cochez les sept tables suivantes :

- DimDate

- DimProduct
  
- DimProductCategory
  
- DimProductSubcategory

- DimReseller

- DimSalesTerritory

- FactResellerSales

8. Pour appliquer des transformations aux requêtes, en bas à droite, sélectionnez **Transformer les données**.

 ![](../images/dp500-create-a-star-schema-model-image13.png)

 *La transformation des données vous permet de définir les données qui seront disponibles dans votre modèle.*

9. Dans la fenêtre **Paramètres de connexion**, sélectionnez l’option **DirectQuery**.

 ![](../images/dp500-create-a-star-schema-model-image14.png)

 *Cette décision est importante. DirectQuery est un mode de stockage. Une table de modèle qui utilise le mode de stockage DirectQuery ne stocke pas de données. Par conséquent, lorsqu’un visuel de rapport Power BI interroge une table DirectQuery, Power BI envoie une requête native à la source de données. Ce mode de stockage peut être utilisé pour les grands magasins de données tels qu’Azure Synapse Analytics (parce qu’il peut être peu pratique ou peu rentable d’importer de grands volumes de données) ou lorsque des résultats en temps quasi réel sont requis.*

10. Sélectionnez **OK**.

 ![](../images/dp500-create-a-star-schema-model-image15.png)

11. Dans la fenêtre **Éditeur Power Query**, dans le volet **Requêtes** (à gauche), notez qu’il existe une requête pour chaque table cochée.

 *Vous allez maintenant réviser la définition de chaque requête. Chaque requête devient une table de modèle lorsqu’elle est appliquée au modèle. Vous allez maintenant renommer les requêtes afin qu’elles soient décrites de manière plus conviviale et concise, et appliquer des transformations pour fournir les colonnes requises pour les besoins connus des rapports.*

12. Sélectionnez la requête **DimDate**.

 ![](../images/dp500-create-a-star-schema-model-image17.png)

13. Dans le volet **Paramètres de requête** (à droite), pour renommer la requête, dans la zone **Nom**, remplacez le texte par **Date**, puis appuyez sur **Entrée**.

 ![](../images/dp500-create-a-star-schema-model-image18.png)

14. Pour supprimer des colonnes inutiles, sous l’onglet de ruban **Accueil**, dans le groupe **Gérer les colonnes**, sélectionnez l’icône **Choisir les colonnes**.

 ![](../images/dp500-create-a-star-schema-model-image19.png)

15. Dans la fenêtre **Choisir les colonnes**, décochez la première case pour décocher toutes les cases.

 ![](../images/dp500-create-a-star-schema-model-image20.png)

16. Cochez les cinq colonnes ci-après.

- DateKey

- FullDateAlternateKey

- EnglishMonthName

- FiscalQuarter

- FiscalYear

 ![](../images/dp500-create-a-star-schema-model-image21.png)

 *Cette sélection de colonnes détermine ce qui sera disponible dans votre modèle.*

17. Sélectionnez **OK**.

 ![](../images/dp500-create-a-star-schema-model-image22.png)

18. Dans le volet **Paramètres de requête**, dans la liste **Étapes appliquées**, notez qu’une étape a été ajoutée pour supprimer d’autres colonnes.

 ![](../images/dp500-create-a-star-schema-model-image23.png)

 *Power Query définit les étapes permettant d’atteindre la structure et les données souhaitées. Chaque transformation est une étape de la logique de requête.*

19. Pour renommer la colonne **FullDateAlternateKey**, double-cliquez sur l’en-tête de colonne **FullDateAlternateKey**.

20. Remplacez le texte par **Date**, puis appuyez sur **Entrée**.

 ![](../images/dp500-create-a-star-schema-model-image24.png)

21. Notez qu’une nouvelle étape appliquée est ajoutée à la requête.

 ![](../images/dp500-create-a-star-schema-model-image25.png)

22. Renommer les colonnes suivantes :

- **EnglishMonthName** en **Month**

- **FiscalQuarter** en **Quarter**

- **FiscalYear** en **Year**

23. Pour valider la conception de la requête, dans la barre d’état (en bas de la fenêtre), vérifiez que la requête comporte cinq colonnes.

 ![](../images/dp500-create-a-star-schema-model-image26.png)

 *Important : si la conception de la requête ne correspond pas, revoyez les étapes de l’exercice pour apporter des corrections.*

 *La conception de la requête **Date** est maintenant terminée.*

24. Dans le volet **Étapes appliquées**, cliquez avec le bouton droit sur la dernière étape, puis sélectionnez **Afficher la requête native**.

 ![](../images/dp500-create-a-star-schema-model-image27.png)

25. Dans la fenêtre **Requête native**, passez en revue l’instruction SELECT qui reflète la conception de la requête.

 *Ce concept est important. Une requête native est ce que Power BI utilise pour interroger la source de données. Pour garantir des performances optimales, le développeur de base de données doit s’assurer que cette requête est optimisée en créant des index appropriés, etc.*

26. Pour fermer la fenêtre **Requête native**, sélectionnez **OK**.

 ![](../images/dp500-create-a-star-schema-model-image28.png)

27. Sélectionnez la table **DimProductCategory**.

28. Renommez la requête en **Product Details**.

29. Sous l’onglet Accueil du ruban, dans le groupe Combiner, sélectionnez **Fusionner des requêtes.**

 *Remarque : nous fusionnons des requêtes pour obtenir les détails des produits, la catégorie et la sous-catégorie. Ces informations seront utilisées dans la dimension Product.*

30. Sélectionnez la table **DimProductSubcategory**, puis la colonne **ProductCategoryKey** dans chaque table. Sélectionnez **OK**.

 ![](../images/dp500-create-a-star-schema-model-image28a.png)

 *Remarque : utilisez la jointure par défaut pour cette fusion, qui est une jointure externe gauche.*

31. Développez la colonne **DimProductSubcategory**. Sélectionnez les colonnes **ProductSubcategoryKey** et **EnglishProductSubcategoryName**. Désélectionnez **Utiliser le nom de colonne original comme préfixe**.

 ![](../images/dp500-create-a-star-schema-model-image28b.png)

 *La fonctionnalité Développer permet de joindre des tables basées sur des contraintes de clé étrangère dans les données sources. L’approche de conception adoptée par ce labo consiste à joindre des tables de dimension en flocon pour produire une représentation dénormalisée des données.*

32. Sélectionnez **OK**.

33. Renommez la colonne **DimProductSubcategory.ProductSubcategoryKey** en **ProductSubcategoryKey** et **DimProductSubcategory.EnglishProductSubcategoryName** en **EnglishProductSubcategoryName**.

34. Supprimez toutes les colonnes sauf :

   - ProductSubcategoryKey

   - EnglishProductCategoryName

   - EnglishProductSubcategoryName

   Vous devez maintenant avoir trois colonnes avec 37 lignes.

35. Sélectionnez la requête **DimProduct**.

 ![](../images/dp500-create-a-star-schema-model-image29.png)

36. Renommez la requête en **Product**.

 ![](../images/dp500-create-a-star-schema-model-image30.png)

37. Sous l’onglet Accueil du ruban, dans le groupe Combiner, sélectionnez **Fusionner des requêtes.**

38. Sélectionnez la table **Product Details**, puis la colonne **ProductSubcategoryKey** dans la table Product et dans la table Product Details.

    ![](../images/dp500-create-a-star-schema-model-image30a.png)

39. Sélectionnez **OK**.

40. Développez la colonne Product Details et sélectionnez les colonnes **EnglishProductSubcategoryName** et **EnglishProductCategoryName**.

    ![](../images/dp500-create-a-star-schema-model-image30b.png)

41. Sélectionnez **OK**.

42. Pour filtrer la requête, dans l’en-tête de colonne **FinishedGoodsFlag**, ouvrez le menu déroulant et décochez **FALSE**.

 ![](../images/dp500-create-a-star-schema-model-image31.png)

43. Sélectionnez **OK**.

44. Renommer les colonnes suivantes :

- **EnglishProductName** en **Product**

- **Product Details.EnglishProductCategoryName** en **Subcategory**

- **Product Details.** en **Category**

45. Supprimez toutes les colonnes sauf :

- ProductKey

- Produit

- Couleur

- Sous-catégorie

- Catégorie

46. Dans le volet **Étapes appliquées**, cliquez avec le bouton droit sur la dernière étape, puis sélectionnez **Afficher la requête native**.

 ![](../images/dp500-create-a-star-schema-model-image33.png)

47. Dans la fenêtre **Requête native**, passez en revue l’instruction SELECT qui reflète la conception de la requête.

48. Pour fermer la fenêtre **Requête native**, sélectionnez **OK**.

49. Vérifiez que la requête comporte cinq colonnes.

 *La conception de la requête **Product** est maintenant terminée.*

50. Sélectionnez la requête **DimReseller**.

 ![](../images/dp500-create-a-star-schema-model-image34.png)

51. Renommez la requête en **Reseller**.

52. Supprimez toutes les colonnes sauf :

- ResellerKey

- BusinessType

- ResellerName

53. Renommer les colonnes suivantes :

- **BusinessType** en **Business Type** (séparé par un espace)

- **ResellerName** en **Reseller**

54. Vérifiez que la requête comporte trois colonnes.

 *La conception de la requête **Reseller** est maintenant terminée.*

55. Sélectionnez la requête **DimSalesTerritory**.

 ![](../images/dp500-create-a-star-schema-model-image35.png)

56. Renommez la requête en **Territory**.

57. Supprimez toutes les colonnes sauf :

- SalesTerritoryKey

- SalesTerritoryRegion

- SalesTerritoryCountry

- SalesTerritoryGroup

58. Renommer les colonnes suivantes :

- **SalesTerritoryRegion** en **Region**

- **SalesTerritoryCountry** en **Country**

- **SalesTerritoryGroup** en **Group**

59. Vérifiez que la requête comporte quatre colonnes.

 *La conception de la requête **Territory** est maintenant terminée.*

60. Sélectionnez la requête **FactResellerSales**.

 ![](../images/dp500-create-a-star-schema-model-image36.png)

61. Renommez la requête en **Sales**.

62. Supprimez toutes les colonnes sauf :

- ResellerKey

- ProductKey

- OrderDateKey

- SalesTerritoryKey

- OrderQuantity

- UnitPrice

 ![](../images/dp500-create-a-star-schema-model-image37.png)

63. Renommer les colonnes suivantes :

- **OrderQuantity** en **Quantity**

- **UnitPrice** en **Price**

64. Pour ajouter une colonne calculée, sous l’onglet de ruban **Ajouter une colonne**, dans le groupe **Général**, sélectionnez **Colonne personnalisée**.

 ![](../images/dp500-create-a-star-schema-model-image38.png)

65. Dans la fenêtre **Colonne personnalisée**, dans la zone **Nouveau nom de colonne**, remplacez le texte par **Revenue**.

 ![](../images/dp500-create-a-star-schema-model-image39.png)

66. Dans la zone **Formule de colonne personnalisée**, entrez la formule suivante :

 ```
 [Quantity] * [Price]
 ```

67. Sélectionnez **OK**.

68. Pour modifier le type de données de colonne, dans l’en-tête de colonne **Revenue**, sélectionnez **ABC123**, puis sélectionnez **Nombre décimal**.

 ![](../images/dp500-create-a-star-schema-model-image40.png)

69. Examinez la requête native ainsi que la logique de calcul de la colonne **Revenue**.

70. Vérifiez que la requête comporte sept colonnes.

 *La conception de la requête **Sales** est maintenant terminée.*

71. Cliquez avec le bouton droit sur la table **Product Details** et désélectionnez **Activer le chargement**. Cela désactivera le chargement de la table Product Details dans le modèle de données et elle n’apparaîtra pas dans le rapport.

 ![](../images/dp500-create-a-star-schema-model-image40a.png)

72. Répétez cette étape, en désélectionnant Activer le chargement pour la table **DimProductSubcategory**.

73. Pour appliquer les requêtes, sous l’onglet de ruban **Accueil**, dans le groupe **Fermer**, sélectionnez l’icône **Fermer &amp; appliquer**.

 ![](../images/dp500-create-a-star-schema-model-image41.png)

 *Chaque requête est appliquée pour créer une table de modèle. Étant donné que la connexion de données utilise le mode de stockage DirectQuery, seule la structure du modèle est créée. Aucune donnée n’est importée. Le modèle se compose désormais d’une table pour chaque requête.*

74. Dans Power BI Desktop, lorsque les requêtes ont été appliquées, dans le coin inférieur gauche de la barre d’état, notez que le mode de stockage du modèle est DirectQuery.

 ![](../images/dp500-create-a-star-schema-model-image42.png)

### Organiser le diagramme de modèle

Dans cette tâche, vous allez organiser le diagramme de modèle pour comprendre facilement la conception du schéma en étoile.

1. Dans Power BI Desktop, à gauche, sélectionnez la vue **Modèle**.

 ![](../images/dp500-create-a-star-schema-model-image43.png)

2. Pour redimensionner le diagramme du modèle afin de l’adapter à l’écran, en bas à droite, sélectionnez l’icône **Ajuster à la page**.

 ![](../images/dp500-create-a-star-schema-model-image44.png)

3. Faites glisser les tables pour que la table de faits **Sales** se trouve au milieu du diagramme et que les tables restantes, qui sont des tables de dimension, se trouvent autour de la table de faits.

4. Si l’une des tables de dimension n’est pas liée à la table de faits, utilisez les instructions suivantes pour créer une relation :

- Faites glisser la colonne de la clé de dimension (par exemple, **ProductKey**) et déposez-la sur la colonne correspondante de la table **Sales**.

- Dans la fenêtre **Créer une relation**, sélectionnez **OK**.

5. Passez en revue la disposition finale du diagramme de modèle.

 ![](../images/dp500-create-a-star-schema-model-image45.png)

 *La création du modèle de schéma en étoile est maintenant terminée. De nombreuses configurations de modélisation peuvent désormais être appliquées, comme l’ajout de hiérarchies, de calculs et la définition de propriétés telles que la visibilité des colonnes.*

6. Pour enregistrer la solution, en haut à gauche, sélectionnez le menu **Fichier** puis sélectionnez **Enregistrer sous**.

7. Dans la fenêtre **Enregistrer sous**, accédez au dossier **D:\DP500\Allfiles\04\MySolution**.

8. Dans la zone **Nom du fichier**, entrez **Sales Analysis**.

 ![](../images/dp500-create-a-star-schema-model-image46.png)

9. Sélectionnez **Enregistrer**.

10. Fermez Power BI Desktop.

### Mettre en pause le pool SQL

Dans cette tâche, vous allez arrêter le pool SQL.

1. Dans un navigateur Web, accédez à [https://portal.azure.com](https://portal.azure.com/).

2. Localisez le pool SQL.

3. Mettez le pool SQL en pause.

---
lab:
  title: Créer un flux de données
  module: Prepare data for tabular models in Power BI
---

# Créer un flux de données

## Vue d’ensemble

**La durée estimée pour effectuer ce tutoriel est de 45 minutes.**

Dans ce labo, vous allez créer un flux de données pour livrer les données de dimension de date provenant de l’entrepôt de données Azure Synapse Adventure Works. Le flux de données fournira une définition cohérente des données de date à l’usage des analystes de l’organisation.

Dans ce labo, vous découvrez comment :

- Utiliser Power Query Online pour développer un flux de données

- Utiliser Power BI Desktop pour consommer un flux de données

## Prise en main

Dans cet exercice, vous allez préparer votre environnement.

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

1. Après avoir créé l’espace de travail Synapse et le pool SQL et chargé les données, le script met en pause le pool pour éviter les frais inutiles dans Azure. Lorsque vous êtes prêt à utiliser vos données dans Azure Synapse Analytics, vous devez réactiver le pool SQL.

### Cloner le référentiel pour cette formation

1. Dans le menu Démarrer, ouvrez l’invite de commandes.

    ![](../images/command-prompt.png)

1. Dans la fenêtre d’invite de commandes, accédez au lecteur D en tapant :

    `d:` 

   Appuyez sur Entrée.

    ![](../images/command-prompt-2.png)


1. Dans la fenêtre d’invite de commandes, entrez la commande ci-après pour télécharger les fichiers de cours et les enregistrer dans un dossier appelé DP500.
    
    `
    git clone https://github.com/MicrosoftLearning/DP-500-Azure-Data-Analyst DP500
    `
   
2. Une fois le référentiel cloné, fermez la fenêtre d’invite de commandes. 
   
3. Ouvrez le lecteur D dans l’explorateur de fichiers pour vous assurer que les fichiers ont été téléchargés.

### Configurer Power BI Desktop

Dans cette tâche, vous allez configurer Power BI Desktop.

1. Pour ouvrir l’Explorateur de fichiers, dans la barre des tâches, sélectionnez le raccourci vers **Explorateur de fichiers**.

1. Accédez au dossier **D:\DP500\Allfiles\05\Starter**.

1. Pour ouvrir un fichier Power BI Desktop prédéveloppé, double-cliquez sur le fichier **Sales Analysis - Create a dataflow.pbix**.

1. Si vous n’êtes pas déjà connecté, dans le coin supérieur droit de Power BI Desktop, sélectionnez **Se connecter**. Utilisez les informations d’identification du labo pour terminer le processus de connexion.

    ![](../images/dp500-create-a-dataflow-image2.png)

1. Pour enregistrer le fichier, dans le ruban **Fichier**, sélectionnez **Enregistrer sous**.

1. Dans la fenêtre **Enregistrer sous**, accédez au dossier **D:\DP500\Allfiles\05\MySolution**.

1. Accédez à Power BI Desktop, sélectionnez **Fichier**, **Options et paramètres**, **Options**, puis **Sécurité** et sous Navigateur d’authentification, cochez **Utiliser mon navigateur par défaut** et sélectionnez **Enregistrer**.

    *Vous allez mettre à jour la solution Power BI Desktop afin d’utiliser un flux de données pour obtenir les données de dimension de date.*

### Se connecter au service Power BI

Dans cette tâche, vous allez vous connecter au service Power BI, démarrer une licence d’évaluation et créer un espace de travail.

*Important : si vous avez déjà configuré Power BI dans votre environnement de machine virtuelle, passez à la tâche suivante.*

1. Dans un navigateur Web, accédez à [https://powerbi.com](https://powerbi.com/).

1. Utilisez les informations d’identification du labo pour terminer le processus de connexion.

    *Important : vous devez utiliser les mêmes informations d’identification que celles utilisées pour vous connecter à partir de Power BI Desktop.*

1. En haut à droite, sélectionnez l’icône de profil, puis sélectionnez **Démarrer la version d’évaluation**.

    ![](../images/dp500-create-a-dataflow-image3.png)

1. Lorsque vous y êtes invité, sélectionnez **Démarrer la version d’évaluation**.


2. Effectuez les tâches restantes pour terminer la configuration de la version d’évaluation.

    *Conseil : l’expérience du navigateur web de Power BI est appelée **service Power BI**.*

9. Sélectionnez Espaces de travail, puis **Créer un espace de travail**.

    ![](../images/dp500-create-a-star-schema-model-image2a.png)

10. Créez un espace de travail nommé DP500 labs, puis sélectionnez **Enregistrer**.

    *Remarque : le nom de l’espace de travail doit être unique au sein d’un locataire. Si une erreur s’affiche, modifiez le nom de l’espace de travail.*

Une fois l’espace de travail créé, il est ouvert. Dans un exercice ultérieur, vous allez créer un flux de données pour cet espace de travail.

### Démarrer le pool SQL

Dans cette tâche, vous allez démarrer le pool SQL.

1. Dans un navigateur Web, accédez à [https://portal.azure.com](https://portal.azure.com/).

1. Utilisez les informations d’identification du labo pour terminer le processus de connexion.

1. Utilisez la barre de recherche pour localiser Azure Synapse Analytics. 

1. Sélectionnez l’instance Azure Synapse Analytics.
    ![](../images/synapse-instance.png)

1. Recherchez le pool SQL dédié, puis sélectionnez-le.
    ![](../images/dedicated-sql-pool.png)

1. Réactivez le pool SQL dédié.

    ![](../images/resume-sql-pool.png)

    *Important : le pool SQL est une ressource coûteuse. Limitez l’utilisation de cette ressource lorsque vous travaillez sur ce labo. La tâche finale de ce labo vous demandera de mettre la ressource en pause.*

## Développer un flux de données

Dans cet exercice, vous allez développer un flux de données pour prendre en charge le développement de modèles Power BI. Ce flux de données fournit une représentation cohérente de la table de dimension de date de l’entrepôt de données.

### Examiner le modèle de données

Dans cette tâche, vous allez passer en revue le modèle de données développé dans Power BI Desktop.

1. Basculez vers la solution Power BI Desktop.

1. À gauche, basculez vers la vue **Modèle**.

    ![](../images/dp500-create-a-dataflow-image8.png)

1. Dans le diagramme de modèle, notez la table **Date**.

    ![](../images/dp500-create-a-dataflow-image9.png)

    *La table **Date** a été créée par l’analyste d’entreprise. Elle ne représente pas une définition cohérente des données de date et n’inclut pas de colonnes de décalage utiles pour prendre en charge les filtres de date relatifs. Dans un exercice ultérieur, vous allez remplacer cette table par une nouvelle table obtenue d’un flux de données.*

### Créer un flux de données

Dans cette tâche, vous allez créer un flux de données qui représente une définition cohérente des données de date.

1. Dans le service Power BI, sélectionnez **Nouveau**, **Flux de données**.

    ![](../images/dp500-create-a-dataflow-image10.png)

1. Dans la vignette **Définir de nouvelles tables**, sélectionnez **Ajouter de nouvelles tables**.

    ![](../images/dp500-create-a-dataflow-image12.png)

    *L’ajout de nouvelles tables implique l’utilisation de Power Query Online pour définir des requêtes.*

1. Pour choisir une source de données, sélectionnez **Azure Synapse Analytics (SQL DW).**

    ![](../images/dp500-create-a-dataflow-image13.png)

    *Conseil : vous pouvez utiliser la zone de recherche (située en haut à droite) pour trouver la source de données.*

1. Entrez les paramètres de connexion à Synapse.

     - Entrez le nom du serveur à partir du portail Azure.
     
     ![](../images/synapse-sql-pool-connection-string.png)
     
      Le nom du serveur doit ressembler à celui-ci :
      
      synapsewsxxxxx.sql.azuresynapse.net
      
     - Vérifiez que le type d’authentification est **Compte professionnel**. Si vous êtes invité à vous connecter, utilisez les informations d’identification fournies pour le labo.
     ![](../images/synapse-sql-pool-sign-in.png)

1. En bas à droite, sélectionnez **Suivant**.

    ![](../images/dp500-create-a-dataflow-image14.png)

1. Dans le volet de navigation Power Query, développez sqldw et sélectionnez la table **DimDate** (sans la cocher).

    ![](../images/dp500-create-a-dataflow-image15.png)

1. Notez l’aperçu des données de table.

1. Pour créer une requête, cochez la table **DimDate**.

    ![](../images/dp500-create-a-dataflow-image16.png)

1. En bas à droite, sélectionnez **Transformer les données**.

    ![](../images/dp500-create-a-dataflow-image17.png)

    *Power Query Online sera désormais utilisé pour appliquer des transformations à la table. Il offre une expérience presque identique à celle de l’Éditeur Power Query dans Power BI Desktop.*

1. Dans le volet **Paramètres de requête** (à droite), pour renommer la requête, dans la zone **Nom**, remplacez le texte par **Date**, puis appuyez sur **Entrée**.

    ![](../images/dp500-create-a-dataflow-image18.png)

1. Pour supprimer des colonnes inutiles, sous l’onglet de ruban **Accueil**, dans le groupe **Gérer les colonnes**, sélectionnez l’icône **Choisir les colonnes**.

    ![](../images/dp500-create-a-dataflow-image19.png)

1. Dans la fenêtre **Choisir les colonnes**, décochez la première case pour décocher toutes les cases.

    ![](../images/dp500-create-a-dataflow-image20.png)


1. Cochez les cinq colonnes ci-après.

    - DateKey

    - FullDateAlternateKey

    - MonthNumberOfYear

    - FiscalQuarter

    - FiscalYear

    ![](../images/dp500-create-a-dataflow-image21.png)

1. Sélectionnez **OK**.

    ![](../images/dp500-create-a-dataflow-image22.png)

  
1. Dans le volet **Paramètres de requête**, dans la liste **Étapes appliquées**, notez qu’une étape a été ajoutée pour supprimer d’autres colonnes.

    ![](../images/dp500-create-a-dataflow-image23.png)

    *Power Query définit les étapes permettant d’atteindre la structure et les données souhaitées. Chaque transformation est une étape de la logique de requête.*

1. Pour renommer la colonne **FullDateAlternateKey**, double-cliquez sur l’en-tête de colonne **FullDateAlternateKey**.

1. Remplacez le texte par **Date**, puis appuyez sur **Entrée**.

    ![](../images/dp500-create-a-dataflow-image24.png)

1. Pour ajouter une colonne calculée, sous l’onglet de ruban **Ajouter une colonne**, dans le groupe **Général**, sélectionnez **Colonne personnalisée**.

    ![](../images/dp500-create-a-dataflow-image25.png)

   

1. Dans la fenêtre **Colonne personnalisée**, dans la zone **Nom de la nouvelle colonne**, remplacez le texte par **Year**.

1. Dans la liste déroulante **Type de données**, sélectionnez **Texte**.

    ![](../images/dp500-create-a-dataflow-image26.png)

1. Dans la zone **Formule de colonne personnalisée**, entrez la formule suivante :

    *Conseil : toutes les formules peuvent être copiées et collées à partir du fichier **D:\DP500\Allfiles\05\Assets\Snippets.txt**.*.


    ```
    "FY" & Number.ToText([FiscalYear])
    ```


1. Sélectionnez **OK**.

    *Vous allez maintenant ajouter quatre colonnes personnalisées.*

1. Ajoutez une autre colonne personnalisée nommée **Quarter** avec le type de données **Texte** à l’aide de la formule suivante :


    ```
    [Year] & " Q" & Number.ToText([FiscalQuarter])
    ```


1. Ajoutez une deuxième colonne personnalisée nommée **Month** avec le type de données **Texte** à l’aide de la formule suivante :


    ```
    Date.ToText([Date], "yyyy-MM")
    ```

1. Ajoutez une troisième colonne personnalisée nommée **Month Offset** (incluez un espace entre les mots) avec le type de données **Nombre entier**, à l’aide de la formule suivante :


    ```
    ((Date.Year([Date]) * 12) + Date.Month([Date])) - ((Date.Year(DateTime.LocalNow()) * 12) + Date.Month(DateTime.LocalNow()))
    ```


    *Cette formule détermine le nombre de mois à partir du mois en cours. Le mois actuel est égal à zéro, les mois précédents sont négatifs et les prochains mois sont positifs. Par exemple, le mois dernier a la valeur -1.*

   

1. Ajoutez une quatrième colonne personnalisée nommée **Month Offset Filter** (incluez des espaces entre les mots) avec le type de données **Texte**, à l’aide de la formule suivante :


    ```
    if [Month Offset] > 0 then Number.ToText([Month Offset]) & " month(s) future"

    else if [Month Offset] = 0 then "Current month"

    else Number.ToText(-[Month Offset]) & " month(s) ago"
    ```


    *Cette formule transpose le décalage numérique dans un format de texte convivial.*

    *Conseil : toutes les formules peuvent être copiées et collées à partir du fichier **D:\DP500\Allfiles\05\Assets\Snippets.txt**.*.

1. Pour supprimer des colonnes inutiles, sous l’onglet de ruban **Accueil**, dans le groupe **Gérer les colonnes**, sélectionnez l’icône **Choisir les colonnes**.

    ![](../images/dp500-create-a-dataflow-image27.png)

1. Dans la fenêtre **Choisir des colonnes**, décochez les colonnes suivantes :

    - MonthNumberOfYear

    - FiscalQuarter

    - FiscalYear

    ![](../images/dp500-create-a-dataflow-image28.png)

1. Sélectionnez **OK**.

1. En bas à droite, sélectionnez **Enregistrer &amp; fermer**.

    ![](../images/dp500-create-a-dataflow-image29.png)

1. Dans la fenêtre **Enregistrer votre flux de données**, dans la zone **Nom**, entrez **Corporate Date**.

1. Dans la zone **Description**, saisissez : **Consistent date definition for use in all Adventure Works datasets**.

1. Conseil : la description peut être copiée et collée à partir du fichier **D:\DP500\Allfiles\05\Assets\Snippets.txt**.

    ![](../images/dp500-create-a-dataflow-image30.png)

1. Sélectionnez **Enregistrer**.

    ![](../images/dp500-create-a-dataflow-image31.png)

1. Dans le service Power BI, dans le volet **Navigation**, sélectionnez le nom de votre espace de travail.

    *Cette action ouvre la page d’accueil de l’espace de travail.*

1. Pour actualiser le flux de données, placez le curseur sur **Corporate Date**, puis sélectionnez l’icône **Actualiser maintenant**.

    ![](../images/dp500-create-a-dataflow-image32.png)

  

1. Pour accéder aux paramètres du flux de données, placez le curseur sur **Corporate Date**, sélectionnez les points de suspension, puis **Paramètres**.

    ![](../images/dp500-create-a-dataflow-image33.png)

1. Passez en revue les options de configuration.

    ![](../images/dp500-create-a-dataflow-image34.png)

    *Deux paramètres doivent être configurés. Tout d’abord, l’actualisation planifiée doit être configurée afin de mettre à jour quotidiennement les données du flux de données. Ainsi, les décalages de mois seront calculés à l’aide de la date actuelle. Ensuite, le flux de données doit être certifié (par un réviseur autorisé). Un flux de données certifié déclare qu’il répond aux normes de qualité et peut être considéré comme fiable et faisant autorité.*

    *Outre la configuration des paramètres, l’autorisation de consommer le flux de données doit être accordée à tous les créateurs de contenu.*

## Consommer un flux de données

Dans cet exercice, dans la solution Power BI Desktop, vous allez remplacer la table **Date** existante par une nouvelle table dont les données proviennent du flux de données.

### Supprimer la table Date d’origine

Dans cette tâche, vous allez supprimer la table **Date** d’origine.

1. Basculez vers la solution Power BI Desktop.

1. Dans le diagramme du modèle, cliquez avec le bouton droit sur la table **Date**, puis sélectionnez **Supprimer du modèle**.

    ![](../images/dp500-create-a-dataflow-image35.png)

1. Quand vous êtes invité à supprimer la page, sélectionnez **OK**.

    ![](../images/dp500-create-a-dataflow-image36.png)

  


### Ajouter une nouvelle table Date

Dans cette tâche, vous allez ajouter une nouvelle table **Date** dont les données proviennent du flux de données.

1. Sous le ruban **Accueil**, dans le groupe **Données**, sélectionnez l’icône **Obtenir des données**.

    ![](../images/dp500-create-a-dataflow-image37.png)

1. Dans la fenêtre **Obtenir des données**, à gauche, sélectionnez **Power Platform**, puis sélectionnez **Flux de données Power BI**.

    ![](../images/dp500-create-a-dataflow-image38.png)

1. Cliquez sur **Se connecter**.

    ![](../images/dp500-create-a-dataflow-image39.png)

  

1. Dans la fenêtre **Flux de données Power BI**, sélectionnez **Se connecter**.

    ![](../images/dp500-create-a-dataflow-image40.png)

1. Utilisez les informations d’identification du labo pour terminer le processus de connexion.

    *Important : vous devez utiliser les mêmes informations d’identification que celles utilisées pour vous connecter au service Power BI.*

1. Cliquez sur **Se connecter**.

    ![](../images/dp500-create-a-dataflow-image41.png)

1. Dans la fenêtre **Navigateur**, dans le volet gauche, développez le dossier de votre espace de travail, puis celui du flux de données **Corporate Date**.

    ![](../images/dp500-create-a-dataflow-image42.png)


1. Cochez la table **Date**.

    ![](../images/dp500-create-a-dataflow-image43.png)

1. Sélectionnez **Charger**.

    ![](../images/dp500-create-a-dataflow-image44.png)

    *Vous pouvez transformer les données à l’aide de l’éditeur Power Query.*

1. Lorsque la nouvelle table est ajoutée au modèle, créez une relation en faisant glisser la colonne **DateKey** de la table **Date** vers la colonne **OrderDateKey** de la table **Sales**.

    ![](../images/dp500-create-a-dataflow-image45.png)

    *Il existe de nombreuses autres possibilités de configurations de modèle, telles que le masquage de colonnes ou la création d’une hiérarchie.*

### Valider le modèle

Dans cette tâche, vous allez tester le modèle en créant une disposition de rapport simple.

1. À gauche, basculez vers la vue **Rapport**.

    ![](../images/dp500-create-a-dataflow-image46.png)

1. Dans le volet **Visualisations**, sélectionnez le visuel de graphique à barres empilées pour ajouter un visuel à la page.

    ![](../images/dp500-create-a-dataflow-image47.png)

1. Redimensionnez le visuel de façon à ce qu’il remplisse toute la page.

  

1. Dans le volet **Données**, développez la table **Date**, puis faites glisser le champ **Month Offset Filter** dans le visuel de graphique à barres.

    ![](../images/dp500-create-a-dataflow-image48.png)

1. Dans le volet **Données**, développez la table **Sales**, puis faites glisser le champ **Sales Amount** dans le visuel de graphique à barres.

    ![](../images/dp500-create-a-dataflow-image49.png)


1. Pour trier l’axe vertical, en haut à droite du visuel, sélectionnez les points de suspension, puis **Trier l’axe** > **Month Offset Filter**.

    ![](../images/dp500-create-a-dataflow-image50.png)

1. Pour vous assurer que les valeurs de filtre de décalage de mois trient chronologiquement, dans le volet **Données**, sélectionnez le champ **Month Offset Filter**.

1. Dans le groupe **Trier** de l’onglet de ruban **Outils de colonne**, sélectionnez **Trier**, puis **Month Offset**.

    ![](../images/dp500-create-a-dataflow-image51.png)

1. Passez en revue le visuel de graphique à barres mis à jour qui trie désormais chronologiquement.

    *L’avantage principal de l’utilisation de colonnes de décalage de date est que les rapports peuvent filtrer par dates relatives de manière personnalisée. (Segments et filtres et filtrage par date et période relatives, mais ce comportement ne peut pas être personnalisé. Ils n’autorisent pas non plus le filtrage par trimestres.)*

1. Enregistrez le fichier Power BI Desktop.

1. Fermez Power BI Desktop.

### Mettre en pause le pool SQL

Dans cette tâche, vous allez arrêter le pool SQL.

1. Dans un navigateur Web, accédez à [https://portal.azure.com](https://portal.azure.com/).

1. Localisez le pool SQL.

1. Mettez le pool SQL en pause.

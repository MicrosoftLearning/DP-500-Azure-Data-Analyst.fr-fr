---
lab:
  title: Créer un modèle composite
  module: Design and build tabular models
---

# Créer un modèle composite

## Vue d’ensemble

**La durée estimée pour effectuer ce tutoriel est de 30 minutes.**

Dans ce labo, vous allez créer un modèle composite en ajoutant une table à un modèle DirectQuery prédéveloppé.

Dans ce labo, vous découvrez comment :

- Créer un modèle composite.

- Créer des relations de modèle.

- Créer des mesures.

## Prise en main

Dans cet exercice, vous allez préparer votre environnement.

### Cloner le référentiel pour cette formation

1. Dans le menu Démarrer, ouvrez l’invite de commandes.

    ![](../images/command-prompt.png)

1. Dans la fenêtre d’invite de commandes, accédez au lecteur D en tapant :

    `d:` 

   Appuyez sur Entrée.

    ![](../images/command-prompt-2.png)


1. Dans la fenêtre d’invite de commandes, entrez la commande ci-après pour télécharger les fichiers de cours et les enregistrer dans un dossier appelé DP500.
    
    `git clone https://github.com/MicrosoftLearning/DP-500-Azure-Data-Analyst DP500`
   
1. Une fois le référentiel cloné, fermez la fenêtre d’invite de commandes. 
   
1. Ouvrez le lecteur D dans l’explorateur de fichiers pour vous assurer que les fichiers ont été téléchargés.

### Configurer Power BI Desktop

Dans cette tâche, vous allez ouvrir un fichier de modèle Power BI prédéveloppé.

1. Pour ouvrir l’Explorateur de fichiers, dans la barre des tâches, sélectionnez le raccourci vers **Explorateur de fichiers**.

2. Accédez au dossier **D:\DP500\Allfiles\08\Starter**.

3. Pour ouvrir un fichier Power BI Desktop prédéveloppé, double-cliquez sur le fichier **Sales Analysis - Create a composite model.pbit**. 

4. Si vous êtes invité à prendre connaissance d’un risque de sécurité potentiel, sélectionnez **OK**.

5. Renseignez SQLServerInstance, SqlServerDatabase et Culture comme indiqué ci-dessous. Sélectionnez **Charger**.

    SqlServerInstance = ```localhost```

    SqlServerDatabase = ```AdventureWorksDW2022-DP500```

    Culture = ```en```

    ![](../images/dp500-create-a-composite-model-image3.png)

6. Dans l’invite de connexion à la base de données SQL Server, sélectionnez **Connecter**.

7. Dans la fenêtre Prise en charge du chiffrement, sélectionnez **OK**.

8. Dans la fenêtre Requête de base de données native, sélectionnez **Exécuter**.

9. Enregistrez le fichier. Dans le menu **Fichier**, sélectionnez **Enregistrer sous**.

10. Dans la fenêtre **Enregistrer sous**, accédez au dossier **D:\DP500\Allfiles\08\MySolution**. Le nom de fichier est **Sales Analysis - Create a composite model.pbix**.

11. Sélectionnez **Enregistrer**.

### Vérifier l’état

Dans cette tâche, vous allez examiner le rapport prédéveloppé.

1. Dans Power BI Desktop, en bas à droite de la barre d’état, notez que le mode de stockage est DirectQuery.

    ![](../images/dp500-create-a-composite-model-image4.png)

    *Un modèle DirectQuery comprend des tables qui utilisent le mode de stockage DirectQuery. Une table qui utilise le mode de stockage DirectQuery transmet des requêtes à la source de données sous-jacente. Les modélisateurs de données utilisent souvent ce mode de stockage pour modéliser de grands volumes de données. Dans cet exemple, la source de données sous-jacente est une base de données SQL Server.*

1. Examinez la conception du rapport.

    ![](../images/dp500-create-a-composite-model-image5.png)

    *Cette page de rapport comporte un titre et deux visuels. Le visuel de segment permet de filtrer sur un seul exercice fiscal, tandis que le visuel d’histogramme affiche des montants de ventes trimestriels. Vous allez améliorer cette conception en ajoutant des cibles de vente au visuel d’histogramme.*

1. Dans le segment **Fiscal Year**, sélectionnez **FY2021**.

    ![](../images/dp500-create-a-composite-model-image6.png)

    *Il est important de comprendre que les sélections de segments modifient les filtres appliqués au visuel d’histogramme. Power BI actualise le visuel d’histogramme. Cela implique que les données sont récupérées de la base de données source. Ainsi, le visuel d’histogramme affiche les données sources les plus récentes. (Une mise en cache au niveau du rapport peut se produire, ce qui signifie que le rapport peut réutiliser des données interrogées précédemment.)*

### Examiner le modèle de données

Dans cette tâche, vous allez passer en revue le modèle de données prédéveloppé.

1. Basculez vers la vue **Modèle**.

    ![](../images/dp500-create-a-composite-model-image7.png)

1. Utilisez le diagramme de modèle pour passer en revue la conception du modèle.

    ![](../images/dp500-create-a-composite-model-image8.png)

    *Le modèle comprend trois tables de dimension et une table de faits. La table de faits **Sales** représente les détails de la commande client. Il s’agit d’une conception classique de schéma en étoile. La barre située en haut de chaque table indique qu’elle utilise le mode de stockage DirectQuery. Le fait que chaque tableau ait une barre bleue indique que tous les tableaux appartiennent au même groupe source.*

    *Vous allez étendre le modèle avec une autre table de faits pour pouvoir analyser les faits des cibles de ventes.*

## Créer un modèle composite

Dans cet exercice, vous allez ajouter une table d’importation qui convertit le modèle DirectQuery en modèle composite.

*Un modèle composite comprend plusieurs groupes sources.*

### Ajouter une table

Dans cette tâche, vous allez ajouter une table qui stocke les cibles de vente obtenues d’un classeur Excel.

1. Sous l’onglet de ruban **Accueil**, dans le groupe **Données**, sélectionnez **Classeur Excel**.

    ![](../images/dp500-create-a-composite-model-image9.png)

1. Dans la fenêtre **Ouvrir**, accédez au dossier **D:\DP500\Allfiles\08\Assets**.

1. Sélectionnez le fichier **SalesTargets.xlsx**.

    ![](../images/dp500-create-a-composite-model-image10.png)

1. Sélectionnez **Ouvrir**.

    ![](../images/dp500-create-a-composite-model-image11.png)

1. Dans la fenêtre **Navigateur**, cochez la table **Targets**.

    ![](../images/dp500-create-a-composite-model-image12.png)

1. Dans le volet de visualisation (situé à droite), notez que la table comprend trois colonnes et que chaque ligne de la table représente un trimestre fiscal, une région de vente et un montant de ventes cible.

    ![](../images/dp500-create-a-composite-model-image13.png)

    *Vous allez importer ces données pour ajouter une table au modèle DirectQuery. Étant donné qu’il n’est pas possible de se connecter à un classeur Excel à l’aide de DirectQuery, Power BI l’importera.*

1. Sélectionnez **Transformer les données**.

    ![](../images/dp500-create-a-composite-model-image14.png)

1. Dans la fenêtre Éditeur Power Query, pour renommer la première colonne, double-cliquez sur l’en-tête de colonne **Period**.

1. Renommez la colonne **Fiscal Quarter**, puis appuyez sur **Entrée**.

    ![](../images/dp500-create-a-composite-model-image15.png)

1. Pour modifier le type de données de la troisième colonne, dans l’en-tête de colonne **Target Amount**, sélectionnez l’icône de type de données (123), puis **Nombre décimal fixe**.

    ![](../images/dp500-create-a-composite-model-image16.png)

1. Pour appliquer la requête, dans l’onglet de ruban **Accueil**, dans le groupe **Fermer**, sélectionnez l’icône **Fermer &amp; appliquer**.

    ![](../images/dp500-create-a-composite-model-image17.png)

1. Si un message vous invite à prendre connaissance d’un risque de sécurité potentiel, lisez-le, puis sélectionnez **OK**.

    ![](../images/dp500-create-a-composite-model-image18.png)

1. Dans Power BI Desktop, une fois le processus de chargement terminé, dans le diagramme de modèle, positionnez la nouvelle table directement sous la table **Order Date**.

    *Il se peut que la table ne soit pas visible. Dans ce cas, faites défiler la page horizontalement pour afficher la table.*

    ![](../images/dp500-create-a-composite-model-image19.png)

1. Notez que la table **Targets** n’a pas de barre bleue en haut.

    *L’absence de barre indique que la table appartient au groupe source d’importation.*

### Créer des relations de modèle

Dans cette tâche, vous allez créer deux relations de modèle.

1. Pour créer une relation, dans la table **Sales Territory**, faites glisser la colonne **Region** et déposez-la dans la colonne **Region** de la table **Targets**.

    ![](../images/dp500-create-a-composite-model-image20.png)

1. Dans la fenêtre **Créer une relation**, notez que la liste déroulante **Cardinalité** est définie sur **Un à plusieurs**.

    *La colonne **Region** de la table **Sales Territory** contient des valeurs uniques, tandis que la colonne **Region** de la table **Targets** contient des valeurs en double. Cette cardinalité un-à-plusieurs est courante dans les relations entre les tables de dimension et les tables de faits.*

1. Sélectionnez **OK**.

    ![](../images/dp500-create-a-composite-model-image21.png)

1. Dans le diagramme de modèle, notez qu’une relation existe maintenant entre les tables **Sales Territory** et **Target**.

1. Notez également que la ligne de relation est différente de celle des autres lignes de relation.

    ![](../images/dp500-create-a-composite-model-image22.png)

    *La ligne « déconnectée » indique que la relation est une relation limitée. Une relation de modèle est limitée lorsque le côté « unique » n’est pas garanti. Dans ce cas, cela est dû au fait que la relation s’étend sur plusieurs groupes sources. Au moment de la requête, l’évaluation des relations peut être différente pour les relations limitées. Pour plus d’informations, consultez [Relations limitées](https://docs.microsoft.com/power-bi/transform-model/desktop-relationships-understand).*

1. Créez une autre relation, cette fois entre la colonne **Fiscal Quarter** de la table **Order Date** et la colonne **Fiscal Quarter** de la table **Targets**.

    ![](../images/dp500-create-a-composite-model-image23.png)

1. Dans la fenêtre **Créer une relation**, notez que la liste déroulante **Cardinalité** est définie sur **Plusieurs à plusieurs**.

    *Étant donné que les deux colonnes contiennent des valeurs en double, Power BI Desktop définit automatiquement la cardinalité sur plusieurs à plusieurs. Toutefois, la direction du filtrage croisé par défaut est incorrecte.*

1. Dans la liste déroulante **Direction du filtrage croisé**, sélectionnez **À sens unique (Order Date filtre Targets)**.

    ![](../images/dp500-create-a-composite-model-image24.png)

    *Il est courant que les tables de dimension filtrent les tables de faits. Dans cette conception de modèle, il n’est pas nécessaire (ou efficace) de propager des filtres de la table de faits à la table de dimension.*

1. Sélectionnez **OK**.

    ![](../images/dp500-create-a-composite-model-image25.png)

### Définir les propriétés du modèle

Dans cette tâche, vous allez définir les propriétés du modèle de la nouvelle table.

1. Dans la table **Targets**, sélectionnez la colonne **Fiscal Quarter**.

1. Tout en appuyant sur la touche **Ctrl**, sélectionnez la colonne **Region**.

1. Dans le volet **Propriétés**, définissez la propriété **Est masquée** sur **Oui**.

    ![](../images/dp500-create-a-composite-model-image26.png)

1. Dans la table **Targets**, sélectionnez la colonne **Target Amount**.

1. Dans le volet **Propriétés**, dans la section **Mise en forme**, définissez la propriété **Nombre de décimales** sur **2**.

    ![](../images/dp500-create-a-composite-model-image27.png)

### Ajouter des mesures

Dans cette tâche, vous allez ajouter deux mesures pour pouvoir analyser la variance de la cible de vente.

1. Passez à l’affichage **Report**.

    ![](../images/dp500-create-a-composite-model-image28.png)

1. Pour créer une mesure, dans le volet **Données** (situé à droite), cliquez avec le bouton droit sur la table **Targets**, puis sélectionnez **Nouvelle mesure**.

    ![](../images/dp500-create-a-composite-model-image29.png)

1. Dans la barre de formule, ajoutez la définition de mesure suivante.

    *Conseil : toutes les définitions de mesure peuvent être copiées et collées à partir du fichier ***D:\DP500\Allfiles\08\Assets\Snippets.txt***.*


    ```
    Variance = SUM ( 'Sales'[Sales Amount] ) - SUM ( 'Targets'[Target Amount] )
    ```


    *La mesure nommée **Variance** soustrait la somme de **Target Amount** de la somme de **Sales Amount**.*

1. Dans l’onglet de ruban contextuel **Outils de mesure**, à l’intérieur du groupe **Mise en forme**, sous Nombre de décimales, entrez **2**.

    ![](../images/dp500-create-a-composite-model-image30.png)

1. Créez une autre mesure à l’aide de la définition de mesure suivante.


    ```
    Variance Margin =

    DIVIDE (

    [Variance],

    SUM ( 'Targets'[Target Amount] )

    )
    ```


    *La mesure nommée **Variance Margin** utilise la fonction DAX [DIVIDE](https://docs.microsoft.com/dax/divide-function-dax) pour diviser la mesure **Variance** par la somme de la colonne **Target Amount**.*

1. Sous l’onglet de ruban contextuel **Outils de mesure**, dans le groupe **Mise en forme**, dans la liste déroulante **Format**, sélectionnez **Pourcentage**.

    ![](../images/dp500-create-a-composite-model-image31.png)

1. Dans le volet **Données**, dans la table **Targets**, vérifiez qu’il existe deux mesures.

    ![](../images/dp500-create-a-composite-model-image32.png)

### Mettre à jour la mise en page du rapport

Dans cette tâche, vous allez mettre à jour le rapport pour utiliser les nouvelles mesures.

1. Dans le rapport, sélectionnez le visuel d’histogramme.

1. Dans le volet **Données**, faites glisser le champ **Target Amount** dans le volet **Visualisations**, à l’intérieur de la zone **Valeurs**, directement sous le champ **Sales Amount**.

    ![](../images/dp500-create-a-composite-model-image33.png)

1. Notez que le visuel d’histogramme affiche désormais Sales Amount et Target Amount.

1. Faites glisser les deux mesures dans la zone **Info-bulles**.

    ![](../images/dp500-create-a-composite-model-image34.png)

1. Placez le curseur sur n’importe quelle colonne pour afficher une info-bulle. Vous remarquerez qu’elle affiche les valeurs de mesure.

    ![](../images/dp500-create-a-composite-model-image35.png)

    *Vous avez maintenant terminé la création d’un modèle composite qui combine des tables DirectQuery et importe des tables. Vous pouvez optimiser le modèle afin d’améliorer les performances des requêtes en définissant des tables de dimension pour utiliser le mode de stockage double et en ajoutant des agrégations. Toutefois, ces améliorations seront l’objectif d’apprentissage d’autres labos.*

### Terminer

Dans cette tâche, vous allez terminer.

1. Enregistrez le fichier Power BI Desktop.

    ![](../images/dp500-create-a-composite-model-image36.png)

1. Fermez Power BI Desktop.

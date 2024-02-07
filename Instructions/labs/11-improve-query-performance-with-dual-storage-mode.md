---
lab:
  title: Améliorer le niveau de performance des requêtes avec le mode de stockage double
  module: Optimize enterprise-scale tabular models
---

# Améliorer le niveau de performance des requêtes avec le mode de stockage double

## Vue d’ensemble

**La durée estimée pour effectuer ce labo est de 30 minutes.**

Dans ce labo, vous allez améliorer les performances d’un modèle composite en définissant certaines tables de sorte qu’elles utilisent le mode de stockage double.

Dans ce labo, vous découvrez comment :

- Définir le mode de stockage double

- Utiliser l’analyseur de performances pour passer en revue les activités d’actualisation

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

Dans cette tâche, vous allez ouvrir une solution Power BI Desktop prédéveloppée.

1. Pour ouvrir l’Explorateur de fichiers, dans la barre des tâches, sélectionnez le raccourci vers **Explorateur de fichiers**.

2. Accédez au dossier **D:\DP500\Allfiles\11\Starter**.

3. Pour ouvrir un fichier Power BI Desktop prédéveloppé, double-cliquez sur le fichier **Sales Analysis - Improve query performance with dual storage mode.pbix**.

4. Si un message vous invite à prendre connaissance d’un risque de sécurité potentiel, lisez-le, puis sélectionnez **OK**.

5. Si vous êtes invité à approuver l’exécution d’une requête de base de données native, sélectionnez **Exécuter**.

6. Pour enregistrer le fichier, sous l’onglet de ruban **Fichier**, sélectionnez **Enregistrer sous**.

7. Dans la fenêtre **Enregistrer sous**, accédez au dossier **D:\DP500\Allfiles\11\MySolution**.

8. Sélectionnez **Enregistrer**.

### Vérifier l’état

Dans cette tâche, vous allez examiner le rapport prédéveloppé.

1. Dans Power BI Desktop, en bas à droite de la barre d’état, notez que le mode de stockage est Mixte.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image3.png)

    *Un modèle mixte comprend des tables de différents groupes sources. Ce modèle comporte une table d’importation qui extrait ses données d’un classeur Excel. Les tables restantes utilisent une connexion DirectQuery à une base de données SQL Server, qui est l’entrepôt de données.*

2. Examinez la conception du rapport.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image4.png)

    *Cette page de rapport comporte un titre et deux visuels. Le visuel de segment permet de filtrer sur un seul exercice, tandis que le visuel d’histogramme affiche les ventes trimestrielles et les montants cibles. Vous pouvez améliorer les performances du rapport en définissant des tables pour utiliser le mode de stockage double.*

### Examiner le modèle de données

Dans cette tâche, vous allez passer en revue le modèle de données prédéveloppé.

1. Basculez vers la vue **Modèle**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image5.png)

2. Utilisez le diagramme de modèle pour passer en revue la conception du modèle.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image6.png)

    *Le modèle comprend trois tables de dimension et deux tables de faits. La table de faits **Sales** représente les détails des commandes, tandis que la table **Targets** représente les cibles de ventes trimestrielles. Il s’agit d’une conception de schéma en étoile classique. La barre située en haut de certaines tables indique qu’elles utilisent le mode de stockage DirectQuery. Chaque table qui présente une barre bleue appartient au même groupe source.*

    *Dans ce labo, vous allez configurer des tables pour utiliser le mode de stockage double.*

## Configurer le mode de stockage double

Dans cet exercice, vous allez configurer le mode de stockage double.

*Une table de modèles qui utilise le mode de stockage double utilise à la fois l’importation et le mode de stockage DirectQuery. Power BI détermine le mode de stockage le plus efficace à utiliser en fonction de chaque requête, en s’efforçant d’utiliser le mode d’importation dans la mesure du possible, car il est plus rapide.*

### Utiliser l’analyseur de performances

Dans cette tâche, vous allez ouvrir l’analyseur de performances et l’utiliser pour inspecter les événements d’actualisation.

1. Passez à l’affichage **Report**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image7.png)

2. Pour inspecter les événements d’actualisation de visuels, sous l’onglet de ruban **Affichage**, à l’intérieur du groupe de volets **Afficher**, sélectionnez **Analyseur de performances**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image8.png)

3. Dans le volet **Analyseur de performances** (situé à gauche du volet **Visualisations**), sélectionnez **Démarrer l’enregistrement**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image9.png)

    *L’analyseur de performances inspecte et affiche la durée nécessaire pour mettre à jour ou actualiser les visuels. Chaque visuel émet au moins une requête vers la base de données source. Pour plus d’informations, consultez [Utiliser l’analyseur de performances pour examiner les performances des éléments de rapport](https://docs.microsoft.com/power-bi/create-reports/desktop-performance-analyzer).*

4. Sélectionnez **Actualiser les visuels**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image10.png)

5. Dans le volet **Analyseur de performances**, développez le visuel **Segment** et notez l’événement de requête directe.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image11.png)

    *Chaque fois que vous voyez un événement de requête directe, il vous indique que Power BI a utilisé le mode de stockage DirectQuery pour récupérer les données de la base de données source.*

6. Développez le visuel **Sales Result by Fiscal Quarter**. Vous remarquerez qu’il a également enregistré un événement de requête directe.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image12.png)

    *Vous configurez toujours un visuel de segment à l’aide d’un ou plusieurs champs de la même table. Il n’est pas possible d’utiliser des champs de différentes tables pour configurer un segment. De plus, un segment utilise presque toujours des champs d’une table de dimension. Ainsi, pour améliorer les performances des requêtes des visuels de segment, assurez-vous qu’ils stockent des données importées. Dans ce cas, étant donné que les tables de dimension utilisent le mode de stockage DirectQuery, vous pouvez les définir sur le mode de stockage double. Étant donné que les tables de dimension stockent peu de lignes (par rapport aux tables de faits), elles ne doivent pas entraîner un cache de modèle excessivement volumineux.*

### Configurer le mode de stockage double

Dans cette tâche, vous allez définir toutes les tables de dimension pour utiliser le mode de stockage double.

1. Basculez vers la vue **Modèle**.

2. Sélectionnez l’en-tête de la table **Product**.

3. Tout en appuyant sur la touche **Ctrl**, sélectionnez également les en-têtes des tables **Order Date** et **Sales Territory**.

4. Dans le volet **Propriétés**, développez la section **Avancé**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image13.png)

5. Dans la liste déroulante **Mode de stockage**, sélectionnez **Double**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image14.png)

6. Quand vous êtes invité à confirmer la mise à jour, sélectionnez **OK**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image15.png)

    *L’avertissement vous informe que Power BI Desktop peut prendre beaucoup de temps pour importer les données dans les tables de modèle.*

7. Dans le diagramme de modèle, notez la barre rayée en haut de chaque table de dimension.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image16.png)

    *Une barre rayée indique le mode de stockage double.*

### Vérifier l’état

Dans cette tâche, vous allez examiner le rapport prédéveloppé.

1. Passez à l’affichage **Report**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image17.png)

2. Dans le volet **Analyseur de performances**, sélectionnez **Effacer**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image18.png)

3. Actualisez les visuels.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image19.png)

4. Notez que le visuel de segment n’utilise plus de connexion de requête directe.

    *Power BI interroge le cache de modèles des données importées, de sorte que le segment s’actualise désormais plus rapidement.*

5. Notez toutefois que le visuel d’histogramme continue d’utiliser une connexion de requête directe.

    *Cela est dû au fait que le champ **Sales Amount** est une colonne de la table **Sales** qui utilise le mode de stockage DirectQuery.*

6. Sélectionnez les visuels d’histogramme, puis, dans le volet **Visualisations**, dans la zone **Valeurs**, supprimez le champ **Sales Amount**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image20.png)

7. Supprimez également les deux champs de la zone **Info-bulles**.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image21.png)

    *Ces deux mesures dépendent de la colonne **Sales Amount**.*

8. Dans le volet **Analyseur de performances**, développez le dernier événement d’actualisation. Vous remarquerez que le visuel d’histogramme n’utilise plus de connexion de requête directe.

    *Cela est dû au fait que le visuel d’histogramme utilise désormais uniquement deux tables qui sont toutes deux mises en cache dans le modèle. La table **Order Date** utilise le mode de stockage double, tandis que la table **Targets** utilise le mode de stockage par importation.*

    *Vous avez maintenant amélioré les performances des requêtes spécifiques dans lesquelles Power BI peut récupérer des données à partir du cache du modèle. La principale leçon à retenir est que les tables de dimension qui concernent les tables de faits DirectQuery doivent généralement être définies sur le mode de stockage double. De cette façon, lorsqu’elles sont interrogées par un segment, les requêtes seront rapides.*

    *Vous pouvez optimiser davantage le modèle pour améliorer les performances des requêtes en ajoutant des agrégations. Toutefois, cette amélioration sera l’objectif d’apprentissage d’un autre labo.*

### Terminer

Dans cette tâche, vous allez terminer.

1. Enregistrez le fichier Power BI Desktop.

    ![](../images/dp500-improve-query-performance-with-dual-storage-mode-image22.png)

2. Fermez Power BI Desktop.

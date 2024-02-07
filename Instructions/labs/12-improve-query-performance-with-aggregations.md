---
lab:
  title: Améliorer le niveau de performance des requêtes à l’aide d’agrégations
  module: Optimize enterprise-scale tabular models
---

# Améliorer le niveau de performance des requêtes à l’aide d’agrégations

## Vue d’ensemble

**La durée estimée pour effectuer ce labo est de 30 minutes.**

Dans ce labo, vous allez ajouter une agrégation pour améliorer les performances des requêtes de la table de faits **Sales**.

Dans ce labo, vous découvrez comment :

- Configurer une agrégation

- Utiliser l’Analyseur de performances pour déterminer si Power BI utilise une agrégation

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

2. Accédez au dossier **D:\DP500\Allfiles\12\Starter**.

3. Pour ouvrir un fichier Power BI Desktop prédéveloppé, double-cliquez sur le fichier **Sales Analysis - Improve query performance with aggregations.pbix**.

    *Si un message vous invite à prendre connaissance d’un risque de sécurité potentiel, **sélectionnez OK***.
    
    *Si vous êtes invité à approuver l’exécution d’une requête de base de données native, sélectionnez **Exécuter**.

4. Pour enregistrer le fichier, sous l’onglet de ruban **Fichier**, sélectionnez **Enregistrer sous**.

5. Dans la fenêtre **Enregistrer sous**, accédez au dossier **D:\DP500\Allfiles\12\MySolution**.

6. Sélectionnez **Enregistrer**.

### Vérifier l’état

Dans cette tâche, vous allez examiner le rapport prédéveloppé.

1. Dans Power BI Desktop, en bas à droite de la barre d’état, notez que le mode de stockage est **Mixte**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image2.png)

    *Un modèle en mode mixte comprend des tables provenant de différents groupes sources. Ce modèle a une table d’importation qui extrait ses données d’un classeur Excel. Les tables restantes utilisent une connexion DirectQuery à une base de données SQL Server, qui est l’entrepôt de données.*

2. Examinez la conception du rapport.

    ![](../images/dp500-improve-query-performance-with-aggregations-image3.png)

    *Cette page de rapport comprend un titre et deux visuels. Le visuel de segment permet de filtrer sur un seul exercice, tandis que le visuel d’histogramme affiche les ventes trimestrielles et les montants cibles. Dans ce labo, vous allez améliorer les performances du rapport en ajoutant une agrégation.*

### Examiner le modèle de données

Dans cette tâche, vous allez passer en revue le modèle de données prédéveloppé.

1. Basculez vers la vue **Modèle**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image4.png)

2. Utilisez le diagramme de modèle pour passer en revue la conception du modèle.

    ![](../images/dp500-improve-query-performance-with-aggregations-image5.png)

    *Le modèle comprend trois tables de dimension et deux tables de faits. La table de faits **Sales** représente les détails des commandes, tandis que la table **Targets** représente les cibles de ventes trimestrielles. Il s’agit d’une conception de schéma en étoile classique. La barre située en haut de certaines tables indique qu’elles utilisent le mode de stockage DirectQuery. Chaque table qui présente une barre bleue appartient au même groupe source.*

    *Les trois tables de dimension ont une barre rayée, ce qui indique qu’elles utilisent le mode de stockage double. Cela signifie que les tables utilisent à la fois le mode de stockage par importation et DirectQuery. Power BI détermine le mode de stockage le plus efficace à utiliser en fonction de chaque requête, en s’efforçant d’utiliser le mode d’importation dans la mesure du possible, car il est plus rapide.*

    *Dans ce labo, vous allez ajouter une agrégation pour améliorer les performances des requêtes spécifiques de la table **Sales**.*

### Utiliser l’analyseur de performances

Dans cette tâche, vous allez ouvrir l’analyseur de performances et l’utiliser pour inspecter les événements d’actualisation.

1. Passez à l’affichage **Report**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image6.png)

2. Pour inspecter les événements d’actualisation de visuels, sous l’onglet de ruban **Affichage**, à l’intérieur du groupe de volets **Afficher**, sélectionnez **Analyseur de performances**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image7.png)

3. Dans le volet **Analyseur de performances** (situé à gauche du volet **Visualisations**), sélectionnez **Démarrer l’enregistrement**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image8.png)

    *L’analyseur de performances inspecte et affiche la durée nécessaire pour mettre à jour ou actualiser les visuels. Chaque visuel émet au moins une requête vers la base de données source. Pour plus d’informations, consultez [Utiliser l’analyseur de performances pour examiner les performances des éléments de rapport](https://docs.microsoft.com/power-bi/create-reports/desktop-performance-analyzer).*

4. Sélectionnez **Actualiser les visuels**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image9.png)

5. Dans le volet **Analyseur de performances**, développez le visuel **Sales Result by Fiscal Quarter** et notez l’événement de requête directe.

6. Prenez note de la durée totale en millisecondes afin de pouvoir l’utiliser comme base de référence pour la comparaison plus tard dans ce labo.

    ![](../images/dp500-improve-query-performance-with-aggregations-image10.png)

    *Chaque fois que vous voyez un événement de requête directe, il vous indique que Power BI a utilisé le mode de stockage DirectQuery pour récupérer les données de la base de données source.*

    *Une raison courante pour laquelle une table de faits de l’entrepôt de données utilise le mode DirectQuery est le grand volume de données qu’elle contient. Il n’est pas possible ni économiquement pratique d’importer un tel volume de données. Toutefois, le modèle de données peut mettre en cache une vue agrégée de la table de faits qui peut aider à améliorer les performances de requêtes spécifiques, généralement de haut niveau.*

    *Dans ce labo, vous allez ajouter une agrégation de données de la table **Sales** pour améliorer spécifiquement les performances des actualisations de visuels qui interrogent la somme de la colonne **Sales Amount** par date et territoire de vente.*

## Configurer une agrégation

Dans cet exercice, vous allez configurer une agrégation.

*Les agrégations dans Power BI peuvent améliorer les performances des requêtes sur des tables DirectQuery exceptionnellement volumineuses. En utilisant des agrégations, le modèle de données met en cache les données à un niveau agrégé en mémoire. Power BI utilise automatiquement l’agrégation chaque fois que cela est possible.*

### Ajouter une table d’agrégation

Dans cette tâche, vous allez ajouter une table d’agrégation au modèle.

1. Pour ouvrir la fenêtre Éditeur Power Query, sous l’onglet de ruban **Accueil**, dans le groupe **Requêtes**, sélectionnez l’icône **Transformer les données**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image11.png)

2. Dans la fenêtre Éditeur Power Query, dans le volet **Requêtes**, cliquez avec le bouton droit sur la requête **Sales**, puis sélectionnez **Dupliquer**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image12.png)

3. Dans le volet **Requêtes**, notez l’ajout d’une nouvelle requête.

    ![](../images/dp500-improve-query-performance-with-aggregations-image13.png)

    *Vous allez appliquer une transformation pour regrouper les colonnes **OrderDateKey** et **SalesTerritoryKey** et agréger la somme de la colonne **Sales Amount**.*

4. Dans le volet **Paramètres de requête** (situé à droite), dans la zone **Nom**, remplacez le texte par **Sales Agg**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image14.png)

5. Sous l’onglet de ruban **Transformer**, dans le groupe **Table**, sélectionnez **Grouper par**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image15.png)

6. Dans la fenêtre **Grouper par**, sélectionnez l’option **Avancé**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image16.png)

    *L’option avancée permet de regrouper plusieurs colonnes.*

7. Dans la liste déroulante de regroupement, sélectionnez **OrderDateKey**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image17.png)

8. Sélectionnez **Ajouter un regroupement**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image18.png)

9. Dans la deuxième liste déroulante de regroupement, sélectionnez **SalesTerritoryKey**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image19.png)

10. Dans la zone **Nouveau nom de colonne**, remplacez le texte par **Sales Amount**.

11. Dans la liste déroulante **Opération**, sélectionnez **Somme**.

12. Dans la liste déroulante **Colonne**, sélectionnez **SalesAmount**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image20.png)

13. Sélectionnez **OK**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image21.png)

14. Sous l’onglet de ruban **Accueil**, dans le groupe **Fermer**, cliquez sur l’icône **Fermer &amp; appliquer**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image22.png)

    *Power BI Desktop ajoute une nouvelle table au modèle.*

15. Enregistrez le fichier Power BI Desktop.

    ![](../images/dp500-improve-query-performance-with-aggregations-image23.png)

### Définir les propriétés du modèle

Dans cette tâche, vous allez définir les propriétés du modèle pour la nouvelle table.

1. Basculez vers la vue **Modèle**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image24.png)

2. Dans le diagramme de modèle, positionnez la nouvelle table à droite de la table **Targets**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image25.png)

3. Notez que la table **Sales Agg** a une barre bleue en haut qui indique qu’elle utilise le mode de stockage DirectQuery.

    *Bien que les agrégations puissent utiliser le mode de stockage DirectQuery, dans ces cas, elles doivent se connecter à une vue matérialisée dans la source de données. Dans ce labo, l’agrégation utilisera le mode de stockage par importation.*

4. Sélectionnez la table **Sales Agg**.

5. Dans le volet **Propriétés**, développez la section **Avancé**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image26.png)

6. Dans la liste déroulante **Mode de stockage**, sélectionnez **Importer**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image27.png)

7. Quand vous êtes invité à confirmer la mise à jour, sélectionnez **OK**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image28.png)

    *L’avertissement vous informe que Power BI Desktop peut prendre beaucoup de temps pour importer les données dans les tables de modèle. Il vous informe également qu’il s’agit d’une action irréversible. Il n’est pas possible de reconvertir une table en mode de stockage par importation en une table en mode de stockage DirectQuery (à moins de restaurer une version antérieure du fichier Power BI Desktop).*

8. Notez que Power BI Desktop a chargé 6 806 lignes de données dans la nouvelle table.

    ![](../images/dp500-improve-query-performance-with-aggregations-image29.png)

    *Ces lignes représentent chaque combinaison de dates de commande et de région de vente. Cette petite quantité de données a servi à générer un volume potentiellement très important de lignes de la table de faits.*

9. Dans la table **Sales Agg**, sélectionnez la colonne **Sales Amount**.

10. Dans le volet **Propriétés**, dans la section **Mise en forme**, dans la liste déroulante **Type de données**, sélectionnez **Nombre décimal fixe**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image30.png)

    *Pour gérer l’agrégation (plus loin dans cet exercice), le type de données doit correspondre à celui de la colonne **Sales Amount** de la table **Sales**.*

11. Quand vous êtes invité à confirmer la mise à jour, sélectionnez **OK**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image31.png)

### Créer des relations de modèle

Dans cette tâche, vous allez créer deux relations de modèle.

1. Pour créer une relation, dans la table **Order Date**, faites glisser la colonne **DateKey** et déposez-la sur la colonne **OrderDateKey** de la table **Sales Agg**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image32.png)

2. Dans la fenêtre **Créer une relation**, notez que la liste déroulante **Cardinalité** est définie sur **Un à plusieurs**.

    *La colonne **DateKey** de la table **Order Date** contient des valeurs uniques, tandis que la colonne **OrderDateKey** de la table **Sales Agg** contient des valeurs en double. Cette cardinalité un-à-plusieurs est courante pour les relations entre dimension et agrégations basées sur des tables de faits.*

3. Sélectionnez **OK**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image33.png)

4. Dans le diagramme de modèle, notez qu’une relation existe maintenant entre les tables **Order Date** et **Sales Agg**.

5. Créez une autre relation, cette fois entre la colonne **SalesTerritoryKey** de la table **Sales Territory** et la colonne **SalesTerritoryKey** de la table **Sales Agg**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image34.png)

6. Dans la fenêtre **Créer une relation**, sélectionnez **OK**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image35.png)

    *Les tâches que vous avez effectuées dans ce labo ont ajouté une table d’importation au modèle et l’ont associée à d’autres tables du modèle. Toutefois, il ne s’agit pas encore d’une agrégation que Power BI peut utiliser de manière transparente pour améliorer les performances des requêtes. Vous allez configurer l’agrégation dans la tâche suivante.*

7. Passez en revue le diagramme de modèle et notez que la table **Sales Agg** est désormais liée à deux tables de dimension.

    ![](../images/dp500-improve-query-performance-with-aggregations-image36.png)

### Configurer une agrégation

Dans cette tâche, vous allez configurer une agrégation.

1. Dans le diagramme de modèle, cliquez avec le bouton droit sur l’en-tête de la table **Sales Agg**, puis sélectionnez **Gérer les agrégations**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image37.png)

2. Dans la fenêtre **Gérer les agrégations**, pour la colonne **OrderDateKey**, définissez les propriétés suivantes :

    - Résumé : **GroupBy**

    - Table de détails : **Sales**

    - Colonne de détails : **OrderDateKey**

    ![](../images/dp500-improve-query-performance-with-aggregations-image38.png)

3. Pour la colonne **Sales Amount**, définissez les propriétés suivantes :

    - Résumé : **Sum**

    - Table de détails : **Sales**

    - Colonne de détails : **Sales Amount**

4. Pour la colonne **SalesTerritoryKey**, définissez les propriétés suivantes :

    - Résumé : **GroupBy**

    - Table de détails : **Sales**

    - Colonne de détails : **SalesTerritoryKey**

5. Vérifiez que la configuration de l’agrégation ressemble à ce qui suit :

    ![](../images/dp500-improve-query-performance-with-aggregations-image39.png)

6. Notez l’avertissement qui décrit que Power BI masquera la table.

    ![](../images/dp500-improve-query-performance-with-aggregations-image40.png)

    *Power BI Desktop ne masquera pas la table de la même manière que les autres objets de modèle masqués. Power BI masque toujours les agrégations, et même les calculs de modèle ne pourront jamais y faire référence.*

7. Sélectionnez **Appliquer tout**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image41.png)

8. Dans le diagramme de modèle, notez que la table **Sales Agg** est une table masquée.

    ![](../images/dp500-improve-query-performance-with-aggregations-image42.png)

    *À présent, chaque fois qu’un visuel interroge la table **Sales** pour obtenir la somme de la colonne **Sales Amount**, en regroupant n’importe quelle colonne des tables **Order Date** ou **Sales Territory**, Power BI utilise l’agrégation à la place.*

### Tester l’agrégation

Dans cette tâche, vous allez tester l’agrégation et déterminer si Power BI l’utilise.

1. Passez à l’affichage **Report**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image43.png)

2. Dans le volet **Analyseur de performances**, sélectionnez **Actualiser les visuels**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image44.png)

3. Développez le visuel **Sales Result by Fiscal Quarter** et notez qu’il n’a plus d’événement de requête directe.

4. Comparez la durée avec la valeur de référence que vous avez notée plus tôt dans ce labo.

    ![](../images/dp500-improve-query-performance-with-aggregations-image45.png)

    *Que se passe-t-il lorsque les utilisateurs filtrent le visuel d’histogramme en fonction d’autres tables ?*

5. Pour cloner le segment **Fiscal Year**, sélectionnez d’abord le segment.

6. Sous l’onglet de ruban **Accueil**, dans le groupe **Presse-papiers**, sélectionnez **Copier**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image46.png)

7. Sous l’onglet de ruban **Accueil**, dans le groupe **Presse-papiers**, sélectionnez **Coller**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image47.png)

8. Positionnez le nouveau segment directement sous le segment d’origine.

    ![](../images/dp500-improve-query-performance-with-aggregations-image48.png)

9. Sélectionnez le nouveau segment, puis, dans le volet **Visualisations**, dans la zone **Champ**, supprimez le champ **Fiscal Year**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image49.png)

10. Dans le volet **Champs**, développez la table **Sales Territory**, puis faites glisser le champ **Group** dans la zone **Champ**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image50.png)

11. Dans le segment **Group**, sélectionnez un groupe au choix (sauf Blank).

    ![](../images/dp500-improve-query-performance-with-aggregations-image51.png)

    *Power BI utilise-t-il l’agrégation ?*

    *La réponse est oui, car l’agrégation regroupe en fonction de la colonne **SalesTerritoryKey**. Cette colonne est liée à la table **Sales Territory**. Par conséquent, vous pouvez utiliser n’importe quelle colonne de la table **Sales Territory** pour filtrer le visuel d’histogramme et il utilisera l’agrégation.*

12. Clonez le segment **Group** pour créer un segment en fonction du champ **Category** de la table **Product**.

    ![](../images/dp500-improve-query-performance-with-aggregations-image52.png)

    *Power BI utilise-t-il l’agrégation ?*

    *La réponse est non car l’agrégation ne regroupe pas en fonction de la colonne **ProductKey** (ou toute autre colonne de la table **Product**). Dans ce cas, Power BI doit utiliser une connexion DirectQuery pour actualiser le visuel.*

    *Vous avez maintenant amélioré les performances des requêtes spécifiques en permettant à Power BI de récupérer des données à partir du cache du modèle. Le point clé à retenir est que les agrégations peuvent accélérer les performances des requêtes des table de faits, en particulier pour des regroupements de mesures spécifiques et de haut niveau. En outre, le mode de stockage double et les agrégations fonctionnent bien ensemble, offrant des opportunités pour Power BI, ce qui permet à Power BI d’éviter d’utiliser des connexions DirectQuery coûteuses aux données sources.*

### Terminer

Dans cette tâche, vous allez terminer.

1. Enregistrez le fichier Power BI Desktop.

    ![](../images/dp500-improve-query-performance-with-aggregations-image53.png)

2. Fermez Power BI Desktop.

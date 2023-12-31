---
lab:
  title: "Créer des ressources Power\_BI réutilisables"
  module: Manage the analytics development lifecycle
---

# Créer des ressources Power BI réutilisables

## Vue d’ensemble

**La durée estimée pour effectuer ce tutoriel est de 45 minutes.**

Dans ce labo, vous allez créer un jeu de données Power BI spécialisé qui étend un jeu de données principal. Le jeu de données spécialisé permettra l’analyse des ventes américaines par habitant.

Dans ce labo, vous découvrez comment :

- Créer une connexion en direct

- Créer un modèle DirectQuery local

- Utiliser la vue Traçabilité pour découvrir les ressources Power BI dépendantes

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

### Configurer Power BI

Dans cette tâche, vous allez configurer Power BI.

1. Pour ouvrir Power BI Desktop, accédez à la barre des tâches et sélectionnez le raccourci vers **Power BI Desktop**.

2. Fermez la fenêtre de prise en main.

3. En haut à droite de Power BI Desktop, si vous n’êtes pas déjà connecté, sélectionnez **Se connecter**. Utilisez les informations d’identification du labo pour terminer le processus de connexion.

    ![](../images/dp500-create-a-star-schema-model-image3.png)
4. Vous êtes redirigé vers la page d’inscription de Power BI dans Microsoft Edge. Sélectionnez **Continuer** pour terminer l’inscription.

    ![](../images/dp500-create-a-star-schema-model-image3b.png)

5. Entrez un numéro de téléphone à 10 chiffres et sélectionnez **Prise en main**. Sélectionnez **Prise en main** à nouveau. Vous êtes redirigé vers Power BI.

6. En haut à droite, sélectionnez l’icône de profil, puis sélectionnez **Démarrer la version d’évaluation**.

    ![](../images/dp500-create-a-dataflow-image3.png)

7. Lorsque vous y êtes invité, sélectionnez **Démarrer la version d’évaluation**.


8. Effectuez les tâches restantes pour terminer la configuration de la version d’évaluation.

    *Conseil : l’expérience du navigateur web de Power BI est appelée **service Power BI**.*

### Créer un espace de travail dans le service Power BI

Dans cette tâche, vous allez créer un espace de travail.

1. Dans le service Power BI, pour créer un espace de travail, dans le volet **Navigation** (situé à gauche), sélectionnez **Espaces de travail**, puis **Créer un espace de travail**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image3.png)


2. Dans le volet **Créer un espace de travail** (situé à droite), dans la zone **Nom de l’espace de travail**, saisissez un nom pour l’espace de travail.

    *Ce nom doit être unique au sein du locataire.*

    ![](../images/dp500-create-reusable-power-bi-artifacts-image4.png)

3. Sélectionnez **Enregistrer**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image5.png)

    *Une fois créé, l’espace de travail s’ouvre. Dans la tâche suivante, vous allez publier un jeu de données dans cet espace de travail.*

### Ouvrir le fichier de démarrage dans Power BI Desktop

1. Pour ouvrir l’Explorateur de fichiers, dans la barre des tâches, sélectionnez le raccourci vers **Explorateur de fichiers**.

2. Accédez au dossier **D:\DP500\Allfiles\16\Starter**.

3. Pour ouvrir un fichier Power BI Desktop prédéveloppé, double-cliquez sur le fichier **Sales Analysis - Create reusable Power BI artifacts.pbix**.

4. Si vous n’êtes pas déjà connecté, dans le coin supérieur droit de Power BI Desktop, sélectionnez **Se connecter**. Utilisez les informations d’identification du labo pour terminer le processus de connexion.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image7.png)

### Examiner le modèle de données

Dans cette tâche, vous allez passer en revue le modèle de données.

1. Dans Power BI Desktop, à gauche, passez à la vue **Modèle**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image8.png)

2. Utilisez le diagramme de modèle pour passer en revue la conception du modèle.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image9.png)

    *Le modèle comprend six tables de dimension et une table de faits. La table de faits **Sales** stocke les détails de la commande client. Il s’agit d’une conception classique de schéma en étoile.*

### Publier le modèle de données

Dans cette tâche, vous allez publier le modèle de données.

1. Pour publier le rapport, sous l’onglet de ruban **Accueil**, sélectionnez le bouton **Publier**. 

    *Si vous êtes invité à enregistrer vos modifications, sélectionnez **Enregistrer.***

    ![](../images/dp500-create-reusable-power-bi-artifacts-image10.png)

2. Dans la fenêtre **Publier sur Power BI**, sélectionnez votre espace de travail (et non l’espace de travail personnel), puis cliquez sur **Sélectionner**.

3. Une fois la publication réussie, sélectionnez **OK**.

    *Une fois publié, le modèle devient un jeu de données Power BI. Dans ce labo, ce jeu de données est un jeu de données de base qu’un analyste d’entreprise peut étendre pour créer un jeu de données spécialisé. Dans l’exercice suivant, vous allez créer un jeu de données spécialisé pour répondre à un besoin métier spécifique.*

4. Fermez Power BI Desktop.

5. Si vous êtes invité à enregistrer les modifications, sélectionnez **Ne pas enregistrer**.

## Créer un jeu de données spécialisé

Dans cet exercice, vous allez créer un jeu de données spécialisé pour permettre l’analyse des ventes américaines par habitant. Étant donné que le jeu de données de base ne contient pas de valeurs de remplissage, vous allez ajouter une nouvelle table pour étendre le modèle.

### Créer une connexion active

Dans cette tâche, vous allez créer un rapport qui utilise une connexion en direct au jeu de données **Sales Analysis - Create reusable Power BI artifacts**, que vous avez publié dans l’exercice précédent.

1. Pour ouvrir Power BI Desktop, accédez à la barre des tâches et sélectionnez le raccourci vers **Power BI Desktop**.

2. Fermez la fenêtre de prise en main.

3. Pour enregistrer le fichier, dans le ruban **Fichier**, sélectionnez **Enregistrer sous**.

4. Dans la fenêtre **Enregistrer sous**, accédez au dossier **D:\DP500\Allfiles\16\MySolution**.

5. Dans la zone **Nom du fichier**, entrez **US Sales Analysis**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image16.png)

6. Sélectionnez **Enregistrer**.

7. Pour créer une connexion en direct, sous l’onglet de ruban **Accueil**, dans le groupe **Données**, sélectionnez **Jeux de données Power BI**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image17.png)

8. Dans la fenêtre **Hub de données**, sélectionnez le jeu de données **Sales Analysis - Create reusable Power BI artifacts**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image18.png)

9. Cliquez sur **Se connecter**.

10. Dans la boîte de dialogue **Connecter vos données**, cochez la case en regard de **Sales Analysis - Create reusable Power BI artifacts**, puis sélectionnez **Envoyer** pour vous connecter à la source de données.

11. Si vous êtes invité à prendre connaissance d’un risque de sécurité potentiel, lisez la notification, puis sélectionnez **OK**.

12. En bas à gauche, dans la barre d’état, notez que le rapport se connecte en direct au jeu de données.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image20.png)

13. Basculez vers la vue **Modèle**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image21.png)

14. Si nécessaire, pour ajuster le diagramme de modèle à votre écran, en bas à droite, sélectionnez **Ajuster à l’écran**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image22.png)

15. Placez le curseur sur n’importe quel en-tête de table pour afficher une info-bulle et notez que le type de source de données est SQL Server Analysis Services, que le serveur fait référence à votre espace de travail et que la base de données est le jeu de données.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image23.png)

    *Ces propriétés indiquent qu’un modèle distant héberge la table. Dans la tâche suivante, vous allez apporter des modifications au modèle pour l’étendre. Ce processus crée un modèle DirectQuery local que vous pouvez modifier de plusieurs façons différentes.*

16. Enregistrez le fichier Power BI Desktop.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image24.png)

### Créer un modèle DirectQuery local

Dans cette tâche, vous allez créer un modèle DirectQuery local.

1. Sous l’onglet de ruban **Accueil**, dans le groupe **Modélisation**, sélectionnez **Apporter des modifications à ce modèle**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image25.png)

    *Remarque : si vous ne voyez pas l’option permettant d’apporter des modifications à ce modèle, vous devez activer la fonctionnalité d’évaluation DirectQuery pour les jeux de données PBI et AS.*
    - Accédez à **Fichier** > **Options et paramètres** > **Options**, puis dans la section Fonctionnalités d’évaluation, cochez la case DirectQuery pour jeux de données Power BI et Analysis Services pour activer cette fonctionnalité d’évaluation. Vous devrez peut-être redémarrer Power BI Desktop pour que les modifications entrent en vigueur. 

2. Lorsque vous y êtes invité, lisez le message de la fenêtre de boîte de dialogue, puis sélectionnez **Ajouter un modèle local**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image26.png)

    *Le modèle est maintenant un modèle DirectQuery. Il est désormais possible d’améliorer le modèle en modifiant certaines propriétés de table ou de colonne, ou en ajoutant des colonnes calculées. Il est même possible d’étendre le modèle avec de nouvelles tables de données provenant d’autres sources de données. Vous allez ajouter une table pour ajouter des données sur la population américaine au modèle.*

3. Dans la boîte de dialogue **Connecter vos données**, vérifiez que la case en regard de **Sales Analysis - Create reusable Power BI artifacts** est cochée, puis sélectionnez **Envoyer** pour modifier le mode de stockage de la source de données.

4. Placez le curseur sur n’importe quel en-tête de table pour afficher une info-bulle et notez que le mode de stockage de table est défini sur **DirectQuery**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image27.png)

### Concevoir la mise en page du rapport

Dans cette tâche, vous allez concevoir la disposition du rapport pour analyser les ventes américaines.

1. Passez à l’affichage **Report**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image28.png)

2. Dans le volet **Données** (à droite), développez la table **Reseller**.

3. Cliquez avec le bouton droit sur le champ **Country-Region**, puis sélectionnez **Ajouter aux filtres** > **Filtres de niveau rapport**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image29.png)

4. Développez le volet **Filtres** (à gauche du volet **Visualisations**).

5. Dans le volet **Filtres**, dans la section **Filtres dans toutes les pages**, dans la carte **Country-Region**, sélectionnez **United States**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image30.png)

6. Dans le volet **Visualisations**, sélectionnez l’icône de visuel de table pour ajouter un visuel de table.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image31.png)

7. Repositionnez et redimensionnez la table pour qu’elle remplisse toute la page.

8. Dans le volet **Données**, à partir de la table **Reseller**, faites glisser le champ **State-Province** et déposez-le dans le visuel de table.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image32.png)

9. Dans le volet **Données**, développez la table **Sales**, puis ajoutez le champ **Sales Amount** au visuel de table.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image33.png)

10. Pour trier les états par ordre décroissant du montant des ventes, sélectionnez l’en-tête **Sum of Sales Amount**.


    *Cette disposition de rapport fournit des détails de base sur les ventes des États des États-Unis. Toutefois, une exigence supplémentaire consiste à montrer les ventes par habitant et à trier les États par ordre décroissant de cette mesure.*

### Ajouter une table

Dans cette tâche, vous allez ajouter une table des données de population des États-Unis provenant d’une page web.

1. Basculez vers la vue **Modèle**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image35.png)

2. Sous l’onglet de ruban **Accueil**, dans le groupe **Données**, sélectionnez **Obtenir les données**, puis **Web**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image36.png)

3. Dans la zone **URL**, entrez le chemin d’accès au fichier suivant : **D:\DP500\Allfiles\16\Assets\us-resident-population-estimates-2020.html**

    *Pour les besoins de ce labo, Power BI Desktop accède à la page web à partir du système de fichiers.*

    *Conseil : le chemin d’accès au fichier peut être copié et collé à partir du fichier **D:\DP500\Allfiles\16\Assets\Snippets.txt**.*

4. Sélectionnez **OK**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image37.png)

5. Dans la fenêtre **Navigateur**, à droite, basculez vers **Affichage web**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image38.png)

    *La page web présente les estimations de la population résidente des États-Unis provenant du recensement d’avril 2020.*

6. Revenez à la vue Table.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image39.png)

7. À gauche, sélectionnez **Table 2**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image40.png)

8. Examinez l’aperçu de la vue de la table.

    *Cette table de données contient les données requises par votre modèle pour calculer les ventes par habitant. Vous devez préparer les données en effectuant quelques changements : supprimez la ligne **United States** ainsi que la colonne **RANK** et renommez les colonnes **STATE** et **NUMBER**.*

9. Pour préparer les données, sélectionnez **Transformer les données**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image41.png)

10. Dans la fenêtre Éditeur Power Query, dans le volet **Paramètres de la requête** (situé à droite), dans la zone **Nom**, remplacez le texte par **US Population**, puis appuyez sur **Entrée**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image42.png)

11. Pour supprimer la ligne **United States**, dans l’en-tête de colonne **STATE**, sélectionnez la flèche vers le bas, puis désélectionnez l’élément **United States** (faites défiler vers le bas de la liste).

    ![](../images/dp500-create-reusable-power-bi-artifacts-image43.png)

12. Sélectionnez **OK**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image44.png)

13. Cliquez avec le bouton droit sur l’en-tête de colonne, puis sélectionnez **Supprimer** pour supprimer la colonne **RANK**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image45.png)

14. Pour renommer la colonne **STATE**, double-cliquez sur l’en-tête de colonne, remplacez le texte par **State**, puis appuyez sur **Entrée**.

15. Renommez la colonne **NUMBER** en **Population**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image46.png)

16. Sur l’onglet de ruban **Accueil**, dans le groupe **Fermer**, sélectionnez l’icône **Fermer et appliquer** pour appliquer la requête.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image47.png)

17. Si vous êtes invité à prendre connaissance d’un risque de sécurité potentiel, lisez la notification, puis sélectionnez **OK**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image48.png)

    *Power BI Desktop applique la requête pour créer une table de modèle. Il ajoute une nouvelle table qui importe les données de population dans le modèle.*

18. Repositionnez la table **US Population** près de la table **Reseller**.

19. Pour créer une relation de modèle, à partir de la table **US Population**, faites glisser la colonne **State** et déposez-la dans la colonne **State-Province** de la table **Reseller**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image49.png)

20. Dans la fenêtre **Créer une relation**, dans la liste déroulante **Direction du filtage croisé**, sélectionnez **À double sens**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image50.png)

    *Chaque ligne de la table **Reseller** stocke un revendeur, de sorte que la colonne **State-Province** contient des valeurs en double (par exemple, il existe de nombreux revendeurs dans l’État de Californie). Lorsque vous créez la relation, Power BI Desktop détermine automatiquement les cardinalités de colonne et découvre qu’il s’agit d’une relation plusieurs-à-un. Pour vous assurer que les filtres se propagent de la table **Reseller** à la table **US Population**, la relation doit traverser le filtre dans les deux sens.*

21. Sélectionnez **OK**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image51.png)

22. Pour masquer la nouvelle table, dans l’en-tête de la table **US Population**, sélectionnez l’icône de visibilité.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image52.png)

    *La table n’a pas besoin d’être visible pour les créateurs de rapports.*

### Ajouter une mesure

Dans cette tâche, vous allez ajouter une mesure pour calculer les ventes par habitant.

1. Passez à l’affichage **Report**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image53.png)

2. Dans le volet **Données**, cliquez avec le bouton droit sur la table **Sales**, puis sélectionnez **Nouvelle mesure**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image54.png)

3. Dans la barre de formule, ajoutez la définition de mesure suivante.

    *Conseil : la définition de mesure peut être copiée et collée à partir du fichier **D:\DP500\Allfiles\16\Assets\Snippets.txt**.*

    ```
    Sales per Capita =
    DIVIDE(
    SUM(Sales[Sales Amount]),
    SUM('US Population'[Population])
    )
    ```

    *La mesure nommée **Sales per Capita** utilise la fonction DAX [DIVIDE](https://docs.microsoft.com/dax/divide-function-dax) pour diviser la somme de la colonne **Sales Amount** par la somme de la colonne **Population**.*

4. Dans l’onglet de ruban contextuel **Outils de mesure**, à partir du groupe **Mise en forme**, dans la zone des décimales, saisissez **4**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image55.png)

5. Pour ajouter la mesure au visuel de matrice, dans le volet **Données**, à partir de la table **Sales**, faites glisser le champ **Sales per Capita** dans le visuel de table.

    *La mesure évalue le résultat en combinant les données provenant d’un modèle distant dans le service Power BI avec les données de la table importée localement dans votre nouveau modèle.*

6. Pour trier les États par ordre décroissant de la valeur des ventes par habitant, sélectionnez l’en-tête de colonne **Sales per Capita**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image56.png)

### Publier la solution

Dans cette tâche, vous allez publier la solution, qui comprend un modèle de données spécialisé et un rapport.

1. Enregistrez le fichier Power BI Desktop.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image57.png)

2. Pour publier la solution, sous l’onglet de ruban **Accueil**, sélectionnez **Publier**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image58.png)

3. Dans la fenêtre **Publier sur Power BI**, sélectionnez votre espace de travail, puis **Sélectionner**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image59.png)

4. Une fois la publication réussie, sélectionnez **OK**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image60.png)

5. Fermez Power BI Desktop.

6. Si vous êtes invité à enregistrer les modifications, sélectionnez **Ne pas enregistrer**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image61.png)

### Passer en revue le jeu de données spécialisé

Dans cette tâche, vous allez passer en revue le jeu de données spécialisé dans le service Power BI.

1. Basculez vers le service Power BI (navigateur web).

2. Dans la page d’accueil de l’espace de travail, notez le rapport **US Sales Analysis** et le jeu de données **US Sales Analysis**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image62.png)


3. Placez le curseur sur le jeu de données **US Sales Analysis** et, lorsque les points de suspension s’affichent, cliquez dessus, puis sélectionnez **Afficher la traçabilité**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image63.png)

    *L’option **Afficher la traçabilité** prend en charge la recherche de dépendances entre les ressources Power BI. C’est important, par exemple, si vous allez publier des modifications dans un jeu de données de base. La vue Traçabilité vous indique les jeux de données dépendants susceptibles de nécessiter des tests.*

4. Dans la vue Traçabilité, notez le lien entre le rapport, le jeu de données **US Sales Analysis** et le jeu de données **Sales Analysis - Create reusable Power BI artifacts**.

    ![](../images/dp500-create-reusable-power-bi-artifacts-image64.png)

    *Lorsque les jeux de données Power BI sont liés à d’autres jeux de données, on parle de chaînage. Dans ce labo, le jeu de données **US Sales Analysis** est chaîné au jeu de données **Sales Analysis - Create reusable Power BI artifacts**, ce qui permet sa réutilisation à des fins spécialisées.*

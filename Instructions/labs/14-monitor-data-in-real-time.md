---
lab:
  title: Surveiller les données en temps réel
  module: Implement advanced data visualization techniques by using Power BI
---

# Surveiller les données en temps réel

## Vue d’ensemble

**La durée estimée pour effectuer ce labo est de 30 minutes.**

Dans ce labo, vous allez configurer un rapport pour utiliser l’actualisation automatique des pages. Les consommateurs de rapports pourront ainsi surveiller les résultats des ventes en ligne en temps réel.

Dans ce labo, vous découvrez comment :

- Utiliser l’analyseur de performances pour passer en revue les activités d’actualisation

- Configurer l’actualisation automatique des pages

- Créer et utiliser la fonction de détection des modifications

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

### Configurer la base de données

Dans cette tâche, vous allez utiliser SQL Server Management Studio (SSMS) pour configurer la base de données en exécutant deux scripts.

1. Pour ouvrir SSMS, dans la barre des tâches, sélectionnez le raccourci vers **SSMS**.

    ![](../images/dp500-monitor-data-in-real-time-image1.png)

2. Dans la fenêtre **Se connecter au serveur**, vérifiez que la liste déroulante **Nom du serveur** est définie sur **localhost** et que la liste déroulante Authentification est définie sur **Authentification Windows**.
    ![](../images/dp500-monitor-data-in-real-time-image2.png)

3. Cliquez sur **Se connecter**.

4. Pour ouvrir un fichier de script, dans le menu **Fichier**, sélectionnez **Ouvrir** > **Fichier**.

5. Dans la fenêtre **Ouvrir un fichier**, accédez au dossier **D:\DP500\Allfiles\14\Assets**.

6. Sélectionnez le fichier **1-Setup.sql**.

    ![](../images/dp500-monitor-data-in-real-time-image4.png)

7. Sélectionnez **Ouvrir**.

    ![](../images/dp500-monitor-data-in-real-time-image5.png)

8. Examinez le script.

    *Ce script crée une table nommée **FactInternetSalesRealTime**. Un autre script charge les données dans cette table pour simuler une charge de travail en temps réel des commandes client en ligne.*

9. Pour exécuter un script, dans la barre d’outils, sélectionnez **Exécuter** (ou appuyez sur **F5**).

    ![](../images/dp500-monitor-data-in-real-time-image6.png)

10. Pour fermer le fichier, dans le menu **Fichier**, cliquez sur **Fermer**.

11. Ouvrez le fichier **2-InsertOrders.sql**.

    ![](../images/dp500-monitor-data-in-real-time-image7.png)

12. Examinez également ce script.

    *Ce script exécute une boucle infinie. Pour chaque boucle, il insère une commande client, puis temporise pendant une période aléatoire comprise entre 1 et 15 secondes.*

13. Exécutez le script et laissez-le s’exécuter jusqu’à la fin du labo.

### Configurer Power BI Desktop

Dans cette tâche, vous allez ouvrir une solution Power BI Desktop prédéveloppée.

1. Pour ouvrir l’Explorateur de fichiers, dans la barre des tâches, sélectionnez le raccourci vers **Explorateur de fichiers**.

2. Accédez au dossier **D:\DP500\Allfiles\14\Starter**.

3. Pour ouvrir un fichier Power BI Desktop prédéveloppé, double-cliquez sur le fichier **Internet Sales - Monitor data in real time.pbix**.

4. Pour enregistrer le fichier, sous l’onglet de ruban **Fichier**, sélectionnez **Enregistrer sous**.

5. Dans la fenêtre **Enregistrer sous**, accédez au dossier **D:\DP500\Allfiles\14\MySolution**.

6. Sélectionnez **Enregistrer**.

### Vérifier l’état

Dans cette tâche, vous allez examiner le rapport prédéveloppé.

1. Dans Power BI Desktop, examinez la page de rapport.

    ![](../images/dp500-monitor-data-in-real-time-image9.png)

    *Cette page de rapport comporte un titre et deux visuels. Le visuel de carte affiche le nombre de commandes, tandis que le visuel d’histogramme affiche le montant des ventes pour chaque sous-catégorie de vélo.*

2. Pour actualiser le rapport, sous l’onglet de ruban **Affichage**, dans le groupe de volets **Afficher**, sélectionnez **Analyseur de performances**.

    ![](../images/dp500-monitor-data-in-real-time-image10.png)

3. Dans le volet **Analyseur de performances** (situé à droite du volet **Visualisations**), sélectionnez **Démarrer l’enregistrement**.

    ![](../images/dp500-monitor-data-in-real-time-image11.png)

    *L’analyseur de performances inspecte et affiche la durée nécessaire pour mettre à jour ou actualiser les visuels. Chaque visuel émet au moins une requête vers la base de données source. Pour plus d’informations, consultez [Utiliser l’analyseur de performances pour examiner les performances des éléments de rapport](https://docs.microsoft.com/power-bi/create-reports/desktop-performance-analyzer).*

4. Sélectionnez **Actualiser les visuels**.

    ![](../images/dp500-monitor-data-in-real-time-image12.png)

5. Notez que les visuels de rapport sont mis à jour pour afficher les derniers résultats des ventes en ligne.

    *Un rapport qui se connecte à un modèle DirectQuery local ne peut pas être actualisé à l’aide de la commande **Actualiser** (située sous l’onglet de ruban **Accueil**). Cela est dû au fait que Power BI Desktop actualise les connexions de table DirectQuery à la place. Pour actualiser les visuels de rapport, suivez les étapes que vous venez de suivre. Lorsqu’ils sont publiés sur le service Power BI, les consommateurs de rapports peuvent sélectionner **Actualiser** dans la barre d’actions pour actualiser les visuels de rapport.*

    *Lorsque vous concevez un rapport pour une analyse en temps réel, il est préférable de ne pas demander aux utilisateurs d’actualiser constamment la page du rapport. Comme vous le verrez dans l’exercice suivant, vous pouvez utiliser l’actualisation automatique des pages pour cela.*

## Configurer l’actualisation automatique de la page

Dans cet exercice, vous allez configurer l’actualisation automatique des pages et tester la fonction de détection des modifications.

*L’actualisation automatique des pages nécessite au moins la définition d’une table de modèles pour utiliser le mode de stockage DirectQuery.*

### Configurer l’actualisation automatique de la page

Dans cette tâche, vous allez configurer l’actualisation automatique des pages.

1. Pour sélectionner la page de rapport, sélectionnez d’abord une zone vide de la page de rapport.

2. Dans le volet **Visualisations**, sélectionnez l’icône de format (en forme de pinceau).

    ![](../images/dp500-monitor-data-in-real-time-image13.png)

3. Basculez le paramètre **Actualisation de la page** (le dernier paramètre dans la liste) sur **Activé**.

    ![](../images/dp500-monitor-data-in-real-time-image14.png)

    *L’actualisation automatique des pages est un paramètre qui s’applique au niveau de la page. Vous pouvez l’activer pour des pages spécifiques du rapport.*

4. Dans le volet **Analyseur de performances**, notez que les visuels de rapport viennent d’être actualisés.

5. Dans le volet **Visualisations**, développez les paramètres **Actualisation de la page**.

    ![](../images/dp500-monitor-data-in-real-time-image15.png)

6. Notez que par défaut, la page s’actualise toutes les 30 minutes.

7. Modifiez les paramètres pour actualiser la page toutes les 5 secondes.

    ![](../images/dp500-monitor-data-in-real-time-image16.png)

    *Important : cet intervalle d’actualisation fréquent vous aidera à travailler efficacement dans ce labo. Mais attention, définir un intervalle d’actualisation si fréquent pourrait avoir un impact sérieux sur les performances de la base de données source et des autres utilisateurs qui consultent le rapport.*

    *Étant donné qu’une commande client en ligne se charge toutes les 1 à 15 secondes, il arrive que l’actualisation de la page donne les mêmes résultats (parce que la base de données n’a enregistré aucune commande au cours des cinq dernières secondes). Il est préférable que les visuels du rapport ne soient actualisés qu’en cas de besoin. La tâche suivante consistera à configurer la fonction de détection des modifications à cet effet.*

    *Une fois publiés dans le service Power BI, les intervalles d’actualisation inférieurs à 30 minutes nécessitent d’enregistrer le rapport dans un espace de travail appartenant à la capacité Premium. En outre, un administrateur de capacité doit activer et configurer la capacité pour permettre ces intervalles fréquents. Pour plus d’informations, consultez [Actualisation automatique des pages dans Power BI](https://docs.microsoft.com/power-bi/create-reports/desktop-automatic-page-refresh).*

### Configurer la détection des modifications

Dans cette tâche, vous allez configurer la détection des modifications.

1. Dans les paramètres **Actualisation de la page**, définissez la liste déroulante **Type d’actualisation** sur **Détection des modifications**.

    ![](../images/dp500-monitor-data-in-real-time-image17.png)

2. Pour créer une mesure de détection des modifications, sélectionnez le lien **Ajouter la détection des modifications**.

    ![](../images/dp500-monitor-data-in-real-time-image18.png)

3. Dans la fenêtre **Détection des modifications**, notez que la configuration par défaut consiste à créer une mesure.

    ![](../images/dp500-monitor-data-in-real-time-image19.png)

4. Dans la liste déroulante **Choisir un calcul**, sélectionnez **Nombre (éléments distincs)**.

    ![](../images/dp500-monitor-data-in-real-time-image20.png)

5. Dans le volet **Champs** (situé à droite, à l’intérieur de la fenêtre), faites défiler vers le bas pour localiser la table **Internet Sales**.

6. Sélectionnez le champ **Sales Order** et notez que la fenêtre l’a ajoutée à la zone **Choisir un champ auquel l’appliquer**.

    ![](../images/dp500-monitor-data-in-real-time-image21.png)

7. Définissez le paramètre **Fréquence de vérification des changements** sur 5 secondes.

    ![](../images/dp500-monitor-data-in-real-time-image22.png)

8. Sélectionnez **Appliquer**.

    ![](../images/dp500-monitor-data-in-real-time-image23.png)

9. Dans le volet **Champs**, dans la table **Internet Sales**, notez l’ajout d’une mesure de détection des modifications.

    ![](../images/dp500-monitor-data-in-real-time-image24.png)

    *Power BI utilise désormais la mesure de détection des modifications pour interroger la base de données source toutes les cinq secondes. Chaque fois, Power BI stocke le résultat pour pouvoir le comparer la prochaine fois qu’il est utilisé. Lorsque les résultats diffèrent, cela signifie que les données ont changé (dans ce cas, la base de données a inséré de nouvelles commandes en ligne). Dans ce cas, Power BI actualise tous les visuels de la page de rapport.*

    *Une fois publiées dans le service Power BI, Power BI prend uniquement en charge les mesures de détection des modifications pour les capacités Premium.*

10. Dans le volet **Analyseur de performances**, sélectionnez **Effacer**.

    ![](../images/dp500-monitor-data-in-real-time-image25.png)

11. Notez que l’analyseur de performances affiche les requêtes de détection des modifications.

12. Notez que plusieurs requêtes de détection de modifications peuvent se produire avant que Power BI Desktop n’actualise les visuels du rapport.

    *C’est parce que la base de données n’a inséré aucune nouvelle commande client en ligne à ce moment-là. Cette configuration est désormais plus efficace, car les visuels de rapport ne s’actualisent qu’en cas de besoin.*

### Terminer

Dans cette tâche, vous allez terminer.

1. Enregistrez le fichier Power BI Desktop.

    ![](../images/dp500-monitor-data-in-real-time-image26.png)

2. Fermez Power BI Desktop.

3. Dans SSMS, pour arrêter l’exécution du script, dans la barre d’outils, sélectionnez **Arrêter** (ou appuyez sur **Alt+Attn**).

    ![](../images/dp500-monitor-data-in-real-time-image27.png)

4. Fermez le fichier de script.

5. Ouvrez le fichier **3-Cleanup.sql**.

    ![](../images/dp500-monitor-data-in-real-time-image28.png)

    *Ce script supprime la table **FactInternetSalesRealTime**.*

6. Exécutez le script.

7. Fermez SSMS.

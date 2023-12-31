# Créer un rapport paginé

## Vue d’ensemble

**La durée estimée pour effectuer ce tutoriel est de 45 minutes.**

Dans ce labo, vous allez utiliser Power BI Report Builder pour développer une disposition de rapport paginé parfaite au pixel près qui tire ses données de la base de données SQL Server **AdventureWorksDW2022-DP500**. Vous allez ensuite créer une source de données et un jeu de données et également configurer un paramètre du rapport. La disposition du rapport permet d’effectuer le rendu des données sur plusieurs pages et de les exporter au format PDF et dans d’autres formats.

Le rapport final va ressembler à ce qui suit :

![](../images/dp500-create-a-paginated-report-image1.png)

Dans ce labo, vous découvrez comment :

- utiliser le Générateur de rapports Power BI

- concevoir une disposition de rapport à plusieurs pages

- définir une source de données

- Définir un jeu de données

- créer un paramètre de rapport

- exporter un rapport au format PDF

## Prise en main

Dans cet exercice, vous allez ouvrir Power BI Report Builder pour créer et enregistrer un rapport.

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

### Créer le rapport

Dans cette tâche, vous allez ouvrir Power BI Report Builder pour créer et enregistrer un rapport.

1. Pour ouvrir Power BI Report Builder, dans la barre des tâches, cliquez sur le raccourci vers **Power BI Report Builder**.

    ![](../images/dp500-create-a-paginated-report-image2.png)

1. Si vous êtes invité à effectuer une mise à jour vers la dernière version de Power BI Report Builder, sélectionnez **Annuler**.

2. Dans la fenêtre Power BI Report Builder, pour créer un nouveau rapport, dans la fenêtre **Mise en route**, cliquez sur **Rapport vierge**.

    ![](../images/dp500-create-a-paginated-report-image3.png)

  
3. Pour enregistrer le rapport, cliquez sur l’onglet **Fichier** (situé en haut à gauche), puis sélectionnez **Enregistrer**.

    ![](../images/dp500-create-a-paginated-report-image4.png)

4. Dans la fenêtre **Enregistrer comme rapport**, accédez au dossier **D:\DP500\Allfiles\15\MySolution**.

5. Dans la boîte **Nom**, entrez **Rapport des commandes client**.

6. Sélectionnez **Enregistrer**.

## Concevoir la mise en page du rapport

Dans cet exercice, vous allez développer la disposition du rapport et explorer la conception finale du rapport.

### Configurer l’en-tête du rapport

Dans cette tâche, vous allez configurer l’en-tête du rapport.

1. Dans le concepteur de rapports, notez la disposition du rapport par défaut, qui se compose d’une région de corps et d’une région de pied de page du rapport.

    ![](../images/dp500-create-a-paginated-report-image5.png)

    *Le corps du rapport contient une zone de texte unique destinée à recevoir le titre du rapport, et le pied de page du rapport contient une zone de texte unique indiquant l’heure d’exécution du rapport.*

    *La conception par défaut affiche le titre du rapport une fois, dans le corps, sur la première page rendue. Toutefois, vous allez maintenant modifier la conception du rapport en ajoutant une région d’en-tête de rapport et en déplaçant la zone de texte du titre du rapport dans cette région. De cette manière, le titre du rapport sera répété sur chaque page. Vous ajouterez également une image du logo de l’entreprise.*

2. Pour ajouter une région d’en-tête de rapport, sous l’onglet de ruban **Insérer** à l’intérieur du groupe **En-tête &amp; pied de page**, cliquez sur **En-tête**, puis sélectionnez **Ajouter un en-tête**.

    ![](../images/dp500-create-a-paginated-report-image6.png)

3. Dans le concepteur de rapports, notez qu’une région d’en-tête du rapport a été ajoutée à la disposition du rapport.

4. Pour sélectionner la zone de texte du corps, sélectionnez la zone de texte « Cliquez pour ajouter un titre ».

5. Pour déplacer la zone de texte, cliquez sur l’icône en forme de flèche à quatre pointes, puis faites-la glisser dans la région d’en-tête pour la déposer en haut à gauche de la région d’en-tête du rapport.

    ![](../images/dp500-create-a-paginated-report-image7.png)

6. Pour modifier le texte du titre du rapport, cliquez à l’intérieur de la zone de texte, puis entrez : **Sales Order Report**

    *Pour redimensionner la zone de texte, commencez par ouvrir le volet **Propriétés**. Le volet **Propriétés** permet un contrôle précis de l’emplacement et des propriétés de taille.*

7. Sous l’onglet de ruban **Afficher**, dans le groupe **Afficher/masquer**, cochez **Propriétés**.

    ![](../images/dp500-create-a-paginated-report-image8.png)

8. Pour sélectionner la zone de texte du titre du rapport, commencez par cliquer sur une zone à l’extérieur de la zone de texte, puis cliquez à nouveau sur la zone de texte.

    *La zone de texte est sélectionnée lorsque sa bordure est mise en surbrillance et que des poignées de redimensionnement (petits cercles) s’affichent sur la bordure.*

9. Dans le volet **Propriétés** (situé à droite), faites défiler la liste pour rechercher le groupe **Position**.

    ![](../images/dp500-create-a-paginated-report-image9.png)

    *Le groupe **Position** permet de définir des valeurs exactes pour l’emplacement et la taille des éléments de rapport.*

    *Important : saisissez les valeurs comme indiqué dans ce labo. Une disposition parfaite au pixel près est nécessaire pour obtenir le rendu de la page à la fin du labo.*

10. Dans le groupe **Position**, développez le groupe **Emplacement** et vérifiez que les propriétés de **Gauche** et du **Haut** sont toutes définies sur **0in**.

    *Les unités d’emplacement et de taille sont définies en pouces car les paramètres régionaux de la machine virtuelle du labo sont définis sur les États-Unis. Si votre région utilise des mesures métriques, les centimètres seraient l’unité par défaut.*

11. Dans le groupe **Position**, développez le groupe **Taille**, puis affectez à la propriété **Largeur** la valeur **4**.

    ![](../images/dp500-create-a-paginated-report-image10.png)


12. Pour insérer une image, sous l’onglet de ruban **Insérer**, à l’intérieur du groupe **Éléments de rapport**, cliquez sur **Image**.

    ![](../images/dp500-create-a-paginated-report-image11.png)

13. Pour ajouter l’image à la conception du rapport, cliquez à l’intérieur de la zone d’en-tête du rapport, à droite de la zone de texte du titre du rapport.

14. Pour importer à partir d’un fichier image, dans la fenêtre **Propriétés de l’image**, cliquez sur **Importer**.

    ![](../images/dp500-create-a-paginated-report-image12.png)

15. Dans la fenêtre **Ouvrir**, accédez au dossier **D:\DP500\Allfiles\15\Assets**, puis sélectionnez le fichier **AdventureWorksLogo.jpg**.

16. Sélectionnez **Ouvrir**.

17. Dans la fenêtre **Propriétés de l’image**, cliquez sur **OK**.

18. Dans le concepteur de rapports, notez que l’image a été ajoutée et qu’elle est sélectionnée.

 

19. Pour positionner et redimensionner l’image, dans le volet **Propriétés**, configurez les propriétés suivantes :

    |  **Propriété** | **Valeur** |
    |--- | --- |
    |  Position > Emplacement > Gauche| 5 |
    |  Position > Emplacement > Haut| 0 |
    |  Position > Taille > Largeur| 1 |
    |  Position > Taille > Hauteur| 1 |



20. Pour redimensionner la région d’en-tête du rapport, commencez par sélectionner la région en cliquant sur une zone vide de la région.

21. Dans le volet **Propriétés**, définissez la propriété **Général** > **Hauteur** sur **1**.

22. Vérifiez que la région d’en-tête du rapport contient une image et une zone de texte uniques et ressemble à ceci :

    ![](../images/dp500-create-a-paginated-report-image13.png)

23. Pour enregistrer le rapport, dans l’onglet **Fichier**, cliquez sur **Enregistrer**.

    *Conseil : vous pouvez également cliquer sur l’icône de disque située en haut à gauche.*

    ![](../images/dp500-create-a-paginated-report-image14.png)

    *Vous êtes maintenant prêt à configurer le rapport pour extraire un résultat de requête de base de données.*


### Récupérer des données

Dans cette tâche, vous allez créer une source de données et un jeu de données pour extraire un résultat de requête de la base de données SQL Server **AdventureWorksDW2022-DP500**.

1. Dans le volet **Données de rapport** (situé à gauche), cliquez avec le bouton de droite sur le dossier **Sources de données**, puis sélectionnez **Ajouter une source de données**.

    ![](../images/dp500-create-a-paginated-report-image15.png)

    *Il est possible de récupérer des données à partir de bases de données cloud ou locales ou d’un jeu de données Power BI.*

2. Dans la fenêtre **Propriétés de la source de données**, dans la zone **Nom**, remplacez le texte par **AdventureWorksDW2022**.

3. Dans la liste déroulante **Sélectionner le type de connexion**, notez que **Microsoft SQL Server** est sélectionné.

4. Pour créer la chaîne de connexion, cliquez sur **Générer**.

    ![](../images/dp500-create-a-paginated-report-image16.png)


5. Dans la fenêtre **Propriétés de connexion**, dans la zone **Nom du serveur**, entrez **localhost**.

    *Dans ce labo, vous allez vous connecter à la base de données SQL Server à l’aide de **localhost**. Toutefois, cette pratique n’est pas recommandée lorsque vous créez vos propres solutions, car les sources de données de la passerelle ne peuvent pas résoudre les problèmes liés à **localhost**.*

6. Dans la liste déroulante **Sélectionner ou entrer un nom de base de données**, sélectionnez **AdventureWorksDW2022-DP500**.

7. Sélectionnez **OK**.

8. Dans la fenêtre **Propriétés de la source de données**, cliquez sur **OK**.

9. Dans le volet **Données de rapport**, notez l’ajout de la source de données **AdventureWorksDW2022**.

    ![](../images/dp500-create-a-paginated-report-image17.png)

10. Pour créer un jeu de données, dans le volet **Données de rapport**, cliquez avec le bouton droit sur la source de données **AdventureWorksDW2022**, puis sélectionnez **Ajouter un jeu de données**.

    ![](../images/dp500-create-a-paginated-report-image18.png)

    *Un jeu de données de rapport n’a pas le même objectif ni la même structure qu’un jeu de données Power BI.*

11. Dans la fenêtre **Propriétés du jeu de données**, dans la boîte**Nom**, remplacez le texte par **SalesOrder**.


12. Pour importer une requête prédéfinie, cliquez sur **Importer**.

    ![](../images/dp500-create-a-paginated-report-image19.png)

13. Dans la fenêtre **Importer une requête**, accédez au dossier **D:\DP500\Allfiles\15\Assets**, puis sélectionnez le fichier **SalesOrder.sql**.

14. Sélectionnez **Ouvrir**.

15. Dans la boîte **Requête**, passez en revue la requête et assurez-vous de faire défiler le texte de la requête jusqu’en bas.

    *Il n’est pas important de comprendre les détails de l’instruction de requête. Elle a été conçue pour récupérer les détails de la ligne de commande. La clause WHERE inclut un prédicat pour restreindre le résultat de la requête à une seule commande client. La clause ORDER BY garantit que les lignes sont retournées par ordre de numéro de ligne.*

16. Notez l’utilisation de **@SalesOrderNumber** dans la clause WHERE, qui représente un paramètre de requête.

    ![](../images/dp500-create-a-paginated-report-image20.png)

    *Un paramètre de requête est un espace réservé pour une valeur qui sera transmise au moment de l’exécution de la requête. Vous allez configurer un paramètre de rapport pour inviter l’utilisateur du rapport à entrer un numéro de commande unique qui sera ensuite transmis au paramètre de requête.*

17. Sélectionnez **OK**.



18. Dans le volet **Données de rapport**, notez l’ajout du jeu de données **SalesOrder** et de ses champs.

    ![](../images/dp500-create-a-paginated-report-image21.png)

    *Les champs sont utilisés pour configurer des régions de données dans la disposition du rapport. Ils ont été dérivés des colonnes de la requête du jeu de données.*

19. Enregistrez le rapport.

### Configurer le paramètre du rapport

Dans cette tâche, vous allez configurer le paramètre du rapport avec une valeur par défaut.

1. Dans le volet **Données de rapport**, développez le dossier **Paramètres** pour afficher le paramètre du rapport **SalesOrderNumber**.

    ![](../images/dp500-create-a-paginated-report-image22.png)

    *Le paramètre de rapport **SalesOrderNumber** a été ajouté automatiquement lors de la création du jeu de données. C’est parce que la requête du jeu de données incluait le paramètre de requête **@SalesOrderNumber**.*

2. Pour modifier le paramètre du rapport, cliquez avec le bouton de droite sur le paramètre du rapport **SalesOrderNumber**, puis sélectionnez **Popriétés de paramètre**.

    ![](../images/dp500-create-a-paginated-report-image23.png)

3. Dans la fenêtre **Propriétés du paramètre du rapport**, à gauche, sélectionnez les pages **Valeurs par défaut**.

    ![](../images/dp500-create-a-paginated-report-image24.png)

4. Sélectionnez l'option **Spécifier les valeurs**.

    ![](../images/dp500-create-a-paginated-report-image25.png)

5. Pour ajouter une valeur par défaut, cliquez sur **Ajouter**.


6. Dans la liste déroulante **Valeur**, remplacez le texte par **43659**.

    ![](../images/dp500-create-a-paginated-report-image26.png)

    *La commande client 43659 est la valeur que vous allez utiliser initialement pour tester la conception du rapport.*

7. Sélectionnez **OK**.

8. Enregistrez le rapport.

    *Vous allez maintenant terminer la conception de la région d’en-tête du rapport en ajoutant des zones de texte pour décrire la commande client.*

### Finaliser la disposition d’en-tête du rapport

Au cours de cette tâche, vous allez finaliser la conception de la région d’en-tête du rapport en ajoutant des zones de texte.

1. Pour ajouter une zone de texte à la région d’en-tête du rapport, sous l’onglet de ruban **Insérer**, dans le groupe **Éléments de rapport**, cliquez sur **Zone de texte**.

    ![](../images/dp500-create-a-paginated-report-image27.png)

2. Cliquez à l’intérieur de la région d’en-tête du rapport, directement sous la zone de texte du titre du rapport.

3. Dans la zone de texte, entrez **Sales Order:** suivi d’un espace.

4. Pour insérer un espace réservé, immédiatement après l’espace entré, cliquez avec le bouton de droite, puis sélectionnez **Créer un espace réservé**.

    ![](../images/dp500-create-a-paginated-report-image28.png)


5. Dans la fenêtre **Propriétés de l’espace réservé**, à droite de la liste déroulante **Valeur**, cliquez sur le bouton **fx**.

    ![](../images/dp500-create-a-paginated-report-image29.png)

    *Le bouton **fx** permet d’entrer une expression personnalisée. Cette expression sera utilisée pour renvoyer le numéro de commande.*

6. Dans la fenêtre **Expression**, dans la liste **Catégorie**, sélectionnez **Paramètres**.

    ![](../images/dp500-create-a-paginated-report-image30.png)

7. Dans la liste **Valeurs**, double-cliquez sur le paramètre **SalesOrderNumber**.

8. Dans la boîte Expression, notez qu’une référence programmatique au paramètre du rapport **SalesOrderNumber** a été ajoutée.

    ![](../images/dp500-create-a-paginated-report-image31.png)

9. Sélectionnez **OK**.

10. Dans la fenêtre **Propriétés de l’espace réservé**, cliquez sur **OK**.

11. Cliquez sur une zone vide de la région d’en-tête de rapport, puis sélectionnez la nouvelle zone de texte.

12. Dans le volet **Propriétés**, configurez les propriétés de position suivantes :

    |  **Propriété**| **Valeur** |
    | --- | --- |
    |  Position > Emplacement > Gauche| 0 |
    |  Position > Emplacement > Haut| 0,5 |
    |  Position > Taille > Largeur| 4 |
    |  Position > Taille > Hauteur| 0,25 |


13. Pour mettre en forme une partie du texte de la zone de texte, à l’intérieur de la nouvelle zone de texte, sélectionnez uniquement le texte **Commande client :**.

    ![](../images/dp500-create-a-paginated-report-image32.png)

14. Sous l’onglet de ruban **Accueil**, à l’intérieur du groupe **Police**, cliquez sur la commande **Gras**.

    ![](../images/dp500-create-a-paginated-report-image33.png)

15. Ajoutez une autre zone de texte à la zone d’en-tête de rapport, puis entrez le texte **Revendeur :** suivi d’un espace.

    *Conseil : vous pouvez également ajouter une zone de texte en cliquant avec le bouton droit sur le canevas, puis en sélectionnant **Insérer** > **Zone de texte**.*

16. Après l’espace, insérez un espace réservé, puis définissez la valeur de l’espace réservé pour utiliser une expression.


17. Dans la fenêtre **Expression**, dans la liste **Catégorie**, sélectionnez **Jeux de données**.

    ![](../images/dp500-create-a-paginated-report-image34.png)

18. Basez la valeur de l’expression sur le valeur **Premier(Revendeur)**.

19. Dans le volet **Propriétés**, configurez les propriétés de position suivantes :

    |  **Propriété**| **Valeur** |
    | --- | --- |
    |  Position > Emplacement > Gauche| 0 |
    |  Position > Emplacement > Haut| 0.75 |
    |  Position > Taille > Largeur| 4 |
    |  Position > Taille > Hauteur| 0,25 |


20. Mettez le texte **Reseller:** en gras.

21. Ajoutez une troisième (et dernière) zone de texte à la région d’en-tête de rapport, puis entrez le texte **Date de commande :** suivi d’un espace.

22. Après l’espace, insérez un espace réservé et définissez la valeur de l’espace réservé pour utiliser une expression basée sur la catégorie **Jeux de données**, **Première(DateCommande)**.

    ![](../images/dp500-create-a-paginated-report-image35.png)


23. Pour mettre en forme la valeur de date, dans la fenêtre **Propriétés de l’espace réservé**, sélectionnez la page **Nombre**.

    ![](../images/dp500-create-a-paginated-report-image36.png)

24. Dans la liste **Catégorie**, sélectionnez **Date**.

    ![](../images/dp500-create-a-paginated-report-image37.png)

25. Dans la liste **Type**, sélectionnez un type de format de date approprié.

26. Dans la fenêtre **Propriétés de l’espace réservé**, cliquez sur **OK**.

27. Dans le volet **Propriétés**, configurez les propriétés de position suivantes :

    |  **Propriété**| **Valeur** |
    | --- | --- |
    |  Position > Emplacement > Gauche| 0 |
    |  Position > Emplacement > Haut| 1 |
    |  Position > Taille > Largeur| 4 |
    |  Position > Taille > Hauteur| 0,25 |


28. Mettez le texte **Order Date:** en gras.

29. Enfin, cliquez sur une zone vide de la région d’en-tête de rapport.

30. Dans le volet **Propriétés**, définissez la propriété **Hauteur** sur **1,5**.


31. Vérifiez que la région d’en-tête de rapport ressemble à ce qui suit :

    ![](../images/dp500-create-a-paginated-report-image38.png)

32. Enregistrez le rapport.

33. Pour afficher un aperçu du rapport, sous l’onglet de ruban **Accueil**, à l’intérieur du groupe **Vues**, cliquez sur **Exécuter**.

    ![](../images/dp500-create-a-paginated-report-image39.png)

    *L’exécution du rapport affiche le rapport au format HTML. Étant donné que le seul paramètre du rapport a une valeur par défaut, le rapport s’exécute automatiquement.*

34. Vérifiez que le rapport rendu ressemble à ce qui suit :

    ![](../images/dp500-create-a-paginated-report-image40.png)


35. Pour revenir en mode conception, sous l’onglet de ruban **Exécuter**, à l’intérieur du groupe **Vues**, cliquez sur **Conception**.

    ![](../images/dp500-create-a-paginated-report-image41.png)

    *Vous allez maintenant ajouter une table au corps du rapport pour afficher une disposition mise en forme des lignes de commande client.*

### Ajouter une région de données de table

Dans cette tâche, vous allez ajouter une région de données de table au corps du rapport.

1. Sous l’onglet de ruban **Insérer**, à l’intérieur du groupe **Régions de données**, cliquez sur **Table**, puis sélectionnez **Insérer une table**.

    ![](../images/dp500-create-a-paginated-report-image42.png)

2. Pour ajouter la table, cliquez sur une zone vide dans le corps du rapport.

3. Dans le volet **Propriétés**, configurez les propriétés de position suivantes :

    |  **Propriété**| **Valeur** |
    | --- | --- |
    |  Position > Emplacement > Gauche| 0 |
    |  Position > Emplacement > Haut| 0 |


    *La table affiche cinq colonnes. Par défaut, le modèle de table inclut seulement trois colonnes.*


4. Pour ajouter une colonne à la table, cliquez avec le bouton droit dans une des cellules de la dernière colonne, puis sélectionnez **Insérer une colonne** > **Droite**.

    ![](../images/dp500-create-a-paginated-report-image43.png)

5. Répétez la dernière étape pour ajouter une deuxième nouvelle colonne.

6. Placez le curseur sur la cellule de la deuxième ligne de la première colonne pour afficher l’icône du sélecteur de champs.

    ![](../images/dp500-create-a-paginated-report-image44.png)

7. Cliquez sur l’icône du sélecteur de champs, puis sélectionnez le champ **Ligne**.

    ![](../images/dp500-create-a-paginated-report-image45.png)

8. Notez que la table contient à présent une valeur texte dans la première ligne (en-tête) et une référence de champ dans la ligne des détails.

    ![](../images/dp500-create-a-paginated-report-image46.png)

9. Ajoutez des champs aux quatre colonnes suivantes, dans l’ordre, comme suit :

    - Produit

    - Quantity

    - UnitPrice

    - Montant

10. Vérifiez que la conception de la table ressemble à ce qui suit :

    ![](../images/dp500-create-a-paginated-report-image47.png)

11. Enregistrez le rapport.

12. Affichez l'aperçu du rapport.

    ![](../images/dp500-create-a-paginated-report-image48.png)

    ![](../images/dp500-create-a-paginated-report-image49.png)

    *La table comprend un en-tête et 12 lignes de commande client. De nombreuses améliorations peuvent être apportées en mettant en forme la disposition de la table.*

    *Dans la tâche suivante, vous allez :*

    - *Mettre en forme l’en-tête de la table en utilisant une couleur d’arrière-plan et un style de police gras*

    - *Modifier la largeur des colonnes pour supprimer l’espace redondant et éviter que les valeurs de texte longues ne soient recouvertes*

    - *Justifier à gauche les valeurs de la première colonne*

    - *Justifier à droite les valeurs des trois dernières colonnes*

    - *Mettre en forme les valeurs monétaires à l’aide d’un symbole monétaire (USD)*

    - *Ajouter et mettre en forme une ligne de total pour la table*


### Mettre en forme une région de données de table

Au cours de cette tâche, vous allez mettre en forme la région de données de table.

1. Revenez en mode Conception.

2. Sélectionnez n’importe quelle cellule de la table pour afficher les repères de cellule gris (situés en haut et à gauche de la région de données).

    ![](../images/dp500-create-a-paginated-report-image50.png)

    *Les repères de cellules sont là pour vous aider à configurer des lignes ou des colonnes entières.*

3. Pour mettre en forme l’en-tête de la table, cliquez sur le repère de ligne de l’en-tête.

    ![](../images/dp500-create-a-paginated-report-image51.png)

    *La sélection d’une ligne ou d’un repère de colonne sélectionne toutes les cellules de la ligne ou de la colonne. Chaque cellule est en fait une zone de texte. La mise en forme d’une zone de texte spécifique (ou d’une série de zones de texte) peut ensuite être obtenue à l’aide du volet **Propriétés** ou des commandes du ruban.*

4. Dans le volet **Propriétés** (ou le ruban), configurez les propriétés suivantes :

    |  **Propriété**| **Valeur** |
    | --- | --- |
    |  Remplir > BackgroundColor| DarkGreen (Conseil : placez le curseur sur chaque couleur pour afficher son nom) |
    |  Police > Color| Blancs
           |
    |  Police > Police > FontWeight| Gras |


5. Sélectionnez le repère de la première colonne.

    ![](../images/dp500-create-a-paginated-report-image52.png)

6. Dans le volet **Propriétés**, définissez la propriété **Position** > **Taille** > **Largeur** sur **0.5**.

7. Définissez la largeur de la deuxième colonne sur **2,5**.

8. Sélectionnez le repère de colonne **Quantity**, puis en appuyant sur la touche **Ctrl**, sélectionnez également les repères d’en-tête des deux dernières colonnes (**Unit price** et **Amount**).

9. Dans le volet **Propriétés** (ou dans le ruban), définissez la propriété **Alignment** > **TextAlign** sur **Right**.

10. Définissez la zone de texte des détails de **Ligne** pour aligner à gauche.

    ![](../images/dp500-create-a-paginated-report-image53.png)

11. Dans l’onglet de ruban **Accueil** à l’intérieur du groupe **Nombre**, définissez les deux dernières zones de texte des détails (pas d’en-tête) (**PrixUnitaire** et **Montant**) pour formater avec un symbole monétaire.

    ![](../images/dp500-create-a-paginated-report-image54.png)

    ![](../images/dp500-create-a-paginated-report-image55.png)


12. Pour ajouter une ligne entière à la table, cliquez avec le bouton de droite sur la zone de texte des détails **Quantity**, puis sélectionnez **Ajouter un total**.

    ![](../images/dp500-create-a-paginated-report-image56.png)

13. Notez qu’une nouvelle ligne, qui représente le pied de page de la table, a été ajoutée et que l’expression évalue la somme des valeurs de **Quantité**.

14. Répétez la dernière étape pour ajouter un total à la zone de texte des détails **Montant**.

15. Dans la première cellule de la ligne de pied de page de la table, entrez le mot **Total**.

16. Met en forme toutes les zones de texte de la ligne de pied de page pour qu’elles s’affichent en gras.

17. Vérifiez que la conception de la table ressemble à ce qui suit :

    ![](../images/dp500-create-a-paginated-report-image57.png)


18. Pour supprimer tout espace de fin après la table, placez le curseur sur la ligne en pointillés entre le corps du rapport et la région de pied de page du rapport, puis faites glisser vers le haut jusqu’en bas de la table.

    ![](../images/dp500-create-a-paginated-report-image58.png)

19. Enregistrer l’état

20. Affichez l'aperçu du rapport.

21. Vérifiez que le rapport rendu ressemble à ce qui suit :

    ![](../images/dp500-create-a-paginated-report-image59.png)

22. Dans la boîte du paramètre **Numéro de commande client**, remplacez la valeur par **51721**.

    ![](../images/dp500-create-a-paginated-report-image60.png)

23. Pour réexécuter le rapport, sur la droite, cliquez sur **Afficher le rapport**.

    ![](../images/dp500-create-a-paginated-report-image61.png)

    *Cette commande client contient 72 lignes de commande et les données sont donc affichées sur plusieurs pages.*

24. Pour accéder à la deuxième page du rapport, dans l’onglet de ruban **Exécuter**, à l’intérieur du groupe **Navigation**, cliquez sur **Suivant**.

    ![](../images/dp500-create-a-paginated-report-image62.png)

25. Sur la page 2, notez que l’en-tête de la table ne s’affiche pas.

    *Vous allez résoudre ce problème dans la tâche suivante.*

26. Faites défiler la page jusqu’en bas, puis notez que le pied de page du rapport affiche uniquement la durée d’exécution.

    *Dans la tâche suivante, vous allez améliorer le texte de pied de page en ajoutant le numéro de page.*

### Finaliser la conception du rapport

Au cours de cette tâche, vous allez finaliser la conception du rapport en veillant à ce que les rapports de plusieurs pages s’affichent correctement.

1. Basculez en mode de conception.

2. Pour vous assurer que l’en-tête de table se répète sur toutes les pages, commencez par sélectionner n’importe quelle zone de texte de la table.

3. Dans le volet **Groupement** (situé dans la partie inférieure du concepteur de rapports), à l’extrême droite des **Groupes de colonnes**, cliquez sur la flèche vers le bas, puis sélectionnez **Mode avancé**.

    ![](../images/dp500-create-a-paginated-report-image63.png)

4. Dans la section **Groupes de lignes**, sélectionnez le premier groupe statique.

    ![](../images/dp500-create-a-paginated-report-image64.png)

    *La ligne d’en-tête de la table est sélectionnée.*

5. Dans le volet **Propriétés**, définissez la propriété **Autre** > **RepeatOnNewPage** sur **True**.

    *Cela garantit que le premier groupe statique (représentant l’en-tête de table) sera répété sur toutes les pages.*

6. Dans la région de pied de page de la table, cliquez avec le bouton de droite sur la zone de texte **ExecutionTime** puis, sélectionnez **Expression**.

    ![](../images/dp500-create-a-paginated-report-image65.png)

7. Dans la fenêtre **Expression**, dans la zone Expression, ajoutez un espace, suivi de **&amp; " | Page " &amp;** pour produire l’expression suivante :


    ```
    =Globals!ExecutionTime & " | Page " &
    ```


8. Vérifiez que la dernière esperluette (&) est bien suivie d’un espace.

9. Dans la liste **Catégorie**, sélectionnez **Champs intégrés**.

    ![](../images/dp500-create-a-paginated-report-image66.png)

10. Pour injecter la valeur du nombre de pages dans l’expression, dans la liste **Élément**, double-cliquez sur **PageNumber**.

11. Vérifiez que l’expression complète est lue comme suit :

    ![](../images/dp500-create-a-paginated-report-image67.png)

12. Sélectionnez **OK**.

13. Faites glisser le côté gauche de la zone de texte pour augmenter la largeur selon la largeur de la page du rapport.

    ![](../images/dp500-create-a-paginated-report-image68.png)

    *La conception du rapport est maintenant terminée. Pour finir, vous devez vous assurer que la largeur de la page est définie sur exactement six pouces et supprimer la valeur par défaut du paramètre du rapport.*

14. Pour sélectionner le corps du rapport, cliquez avec le bouton droit sur une zone de texte de la table, puis cliquez sur **Sélectionner** > **Corps**.

    ![](../images/dp500-create-a-paginated-report-image69.png)

    *Étant donné que la table remplit l’intégralité du corps du rapport, cette technique doit être utilisée pour sélectionner le corps du rapport.*

15. Dans le volet **Propriétés**, vérifiez que la propriété **Position** > **Taille** > **Largeur** est définie sur **6**.

    *Il est important que la largeur ne dépasse pas six pouces, car le passage au format d’impression aurait pour effet de répartir la table sur plusieurs pages.*

16. Dans le volet **Données de rapport**, ouvrez les propriétés du paramètre de rapport **SalesOrderNumber**.

17. Dans la page **Valeurs par défaut**, sélectionnez l’option **Aucune valeur par défaut**.

    ![](../images/dp500-create-a-paginated-report-image70.png)

18. Sélectionnez **OK**.

19. Enregistrez le rapport.

  

### Explorer le rapport terminé

Dans cette tâche, vous allez afficher le rapport en mode de disposition d’impression.

1. Affichez l'aperçu du rapport.

2. Dans la boîte du paramètre **Numéro de commande client**, remplacez la valeur par **51721**

3. Sous l’onglet de ruban **Exécuter**, à l’intérieur du groupe **Imprimer**, cliquez sur **Disposition d’impression**.

    ![](../images/dp500-create-a-paginated-report-image71.png)

    *Le mode Disposition d’impression offre un aperçu de l’aspect du rapport lorsqu’il sera imprimé à la taille de page voulue.*

4. Accédez aux pages 2 et 3.

    *Dans ce labo, vous ne publierez pas le rapport. Notez que les rapports paginés ne peuvent être rendus que dans le service Power BI lorsqu’ils sont stockés dans un espace de travail dont le mode de licence est défini sur **Premium par utilisateur** ou **Premium par capacité**, et si la charge de travail des rapports paginés est activée pour cette capacité.*

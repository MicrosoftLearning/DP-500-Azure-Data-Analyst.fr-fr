---
lab:
  title: Créer des groupes de calcul
  module: Design and build tabular models
---
# Créer des groupes de calcul

## Vue d’ensemble

La durée estimée pour effectuer ce tutoriel est de 45 minutes.

Dans ce labo, vous allez utiliser Power BI Desktop et Tabular Editor 2 pour créer des groupes de calcul.

Dans ce labo, vous découvrez comment :

-   Créer des groupes de calcul.
-   Formater des éléments de calcul.
-   Définir la priorité d’un groupe de calcul.
-   Configurer des visuels pour utiliser des groupes de calcul.

## Prise en main
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

## Préparation de votre environnement

Préparez votre environnement de labo en installant Tabular Editor 2, en configurant Power BI Desktop, en examinant le modèle de données et en créant des mesures.

### Télécharger et installer Tabular Editor 2

Téléchargez et installez Tabular Editor 2 pour activer la création de groupes de calcul.

**Important :** *si vous avez déjà installé Tabular Editor 2 dans votre environnement de machine virtuelle, passez à la tâche suivante.*

*Tabular Editor est un outil d’édition alternatif qui permet de créer des modèles tabulaires pour Analysis Services et Power BI. Tabular Editor 2 est un projet open source qui permet de modifier un fichier BIM sans avoir à accéder aux données du modèle.*

1.  Vérifiez que Power BI Desktop est fermé.

1.  Dans Microsoft Edge, accédez à la page Tabular Editor Release.

    ```https://github.com/TabularEditor/TabularEditor/releases```
    
1. Faites défiler la page jusqu’à la section **Assets** et sélectionnez le fichier **TabularEditor.Installer.msi**. L’installation du fichier est lancée.

1. Une fois l’opération terminée, sélectionnez **Ouvrir le fichier** pour exécuter le programme d’installation.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/calculationgroups-downloadTE.png)

1.  Dans la fenêtre du programme d’installation de Tabular Editor, sélectionnez **Suivant**.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/image2.png)

1.  À l’étape **Contrat de licence**, si vous acceptez, sélectionnez **J’accepte**, puis sélectionnez **Suivant**.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/image3.png)

1.  À l’étape **Sélectionner le dossier d’installation**, sélectionnez **Suivant**.


2.  À l’étape **Raccourcis de l’application**, sélectionnez **Suivant**.


3. À l’étape **Confirmer l’installation**, sélectionnez **Suivant**.

4. Si vous obtenez une fenêtre contextuelle **Contrôle de compte d’utilisateur**, sélectionnez **Oui.**

5. Une fois l’installation terminée, sélectionnez **Fermer**.

    *Tabular Editor est maintenant installé et enregistré en tant qu’outil externe de Power BI Desktop.*

### Configurer Power BI Desktop

Vous allez maintenant ouvrir une solution Power BI Desktop prédéveloppée.

1.  Dans l’explorateur de fichiers, accédez au dossier **D:\\DP500\\Allfiles\\07\\Starter**.

2.  Pour ouvrir un fichier Power BI Desktop prédéveloppé, double-cliquez sur le fichier **Sales Analysis - Create calculation groups.pbix**.

3.  Pour enregistrer le fichier, sous l’onglet de ruban **Fichier**, sélectionnez **Enregistrer sous**.

4.  Dans la fenêtre **Enregistrer sous**, accédez au dossier **D:\\DP500\\Allfiles\\07\\MySolution**.

5.  Sélectionnez **Enregistrer**.

6.  Sélectionnez l’onglet de ruban **Outils externes**.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/image7.png)

7.  Notez que vous pouvez lancer Tabular Editor à partir de cet onglet de ruban.

    ![Description de texte automatique avec un niveau de confiance faible](../images/image8.png)

    *Dans l’exercice suivant, vous allez utiliser Tabular Editor pour créer des groupes de calcul.*

### Examiner le modèle de données

Passez en revue le modèle de données pour comprendre comment les groupes de calcul s’appliqueront à ce modèle.

1.  Dans Power BI Desktop, à gauche, passez à la vue **Modèle**.

    ![](../images/image9.png)

2.  Utilisez le diagramme de modèle pour passer en revue la conception du modèle.

    ![Description automatique de l’interface graphique utilisateur](../images/image10.png)

    *Le modèle comprend sept tables de dimension et deux tables de faits. La table de faits **Sales** stocke les détails de la commande client. La table de faits **Currency Rate** stocke les taux de change quotidiens des différentes devises. Il s’agit d’une conception classique de schéma en étoile.*

3.  Passez à l’affichage **Report**.

    ![](../images/image11.png)

4.  Dans le volet **Données** (à droite), développez la table **Sales** pour examiner les champs.

    ![Description de texte automatique avec un niveau de confiance faible](../images/image12.png)

5.  Notez que les deux champs de la table **Sales** contiennent le symbole sigma (∑).

    *Le symbole sigma indique que les champs seront automatiquement synthétisés à l’aide de fonctions d’agrégation telles que la somme, le décompte, la moyenne ou d’autres.*

    *Toutefois, lorsque des groupes de calcul sont ajoutés à un modèle, ce comportement automatique doit être désactivé. Cela signifie que la synthèse ne peut être obtenue que par des mesures, qui sont définies à l’aide de formules DAX (Data Analysis Expressions). Dans la tâche suivante, vous allez ajouter des mesures au modèle.*

### Créer des mesures

Créez trois mesures liées aux ventes pour préparer la création de vos groupes de calcul.

1.  Dans le volet **Données**, cliquez avec le bouton droit sur la table **Sales**, puis sélectionnez **Nouvelle mesure**.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/image13.png)

2.  Dans la barre de formule (située sous le ruban), remplacez le texte par la définition de mesure suivante, puis sélectionnez **Entrée**.

    Conseil : toutes les formules peuvent être copiées et collées à partir du fichier **D:\\DP500\\Allfiles\\07\\Assets\\Snippets.txt**.

    DAX

    ```Sales = SUM ( 'Sales'[Sales Amount] )```

3.  Dans le ruban contextuel **Outils de mesure**, à l’intérieur du groupe **Mise en forme**, définissez les décimales sur **2**.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/image14.png)

4.  Créez et mettez en forme une deuxième mesure nommée **Cost** à l’aide de la définition suivante :

    DAX

    ```Cost = SUM ( 'Sales'[Total Product Cost] )```

5.  Créez et mettez en forme une troisième mesure nommée **Profit** à l’aide de la définition suivante :

    DAX

    ```Profit = [Sales] - [Cost]```

6.  Dans le volet **Données**, cliquez avec le bouton droit sur le champ **Sales Amount**, puis sélectionnez **Masquer**.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/image15.png)

7.  Masquez également le champ **Total Product Cost**.

8.  La table **Sales** est maintenant répertoriée en premier dans le volet **Champs** et l’icône d’une multi-calculatrice apparaît.

    ![Description d’application automatique avec un niveau de confiance faible](../images/image16.png)

    *Lorsqu’une table comprend uniquement des mesures visibles, elle est présentée en haut du volet. De cette façon, elle se comporte comme un groupe de mesures (objet d’un modèle multidimensionnel). Ne confondez pas cette représentation cosmétique d’un modèle tabulaire avec des groupes de calcul DAX.*

## Créer un groupe de calcul

Vous allez maintenant créer deux groupes de calcul. Le premier prendra en charge time intelligence. Le deuxième prend en charge la conversion monétaire.

### Créer le groupe de calcul Time Intelligence

Utilisez Tabular Editor pour créer le groupe de calcul **Time Intelligence**. Cela simplifiera la création de nombreux calculs liés à l’heure, y compris PY (année précédente), YoY (d’année en année) et YoY % (pourcentage d’année en année). Le groupe de calcul vous permet d’analyser toutes les mesures à l’aide de différents calculs Time Intelligence.

*Power BI Desktop ne prend pas en charge la création ou la gestion des groupes de calcul.*

   > **Conseil** : toutes les syntaxes peuvent être copiées et collées à partir du fichier D:\DP500\Allfiles\07\Assets\Snippets.txt.

1.  Dans le ruban **Outils externes** , sélectionnez **Tabular Editor**.

    ![Description de texte automatique avec un niveau de confiance faible](../images/image17.png)

    *Tabular Editor s’ouvre dans une nouvelle fenêtre et se connecte en direct au modèle de données hébergé dans Power BI Desktop. Les modifications apportées au modèle dans Tabular Editor ne se propagent dans Power BI Desktop que lorsque vous les enregistrez.*

2.  Dans la fenêtre Tabular Editor, dans le volet de gauche, cliquez avec le bouton droit sur le dossier **Tables**, puis sélectionnez **Créer**\>**Groupe de calcul**.

    ![Interface graphique utilisateur, texte, application, description de table automatique](../images/image18.png)

3.  Dans le volet gauche, remplacez le nom par défaut par **Time Intelligence**, puis sélectionnez **Entrée**.

4.  Développez la table **Time Intelligence**.

5.  Sélectionnez la colonne **Nom**.

    ![Interface utilisateur graphique Description générée automatiquement avec un faible niveau de confiance](../images/image19.png)

    *Le groupe de calcul comprend cette seule colonne, tandis que les lignes de données définissent le groupe de calculs. Il est recommandé de renommer la colonne pour refléter l’objet des calculs.*

6.  Dans le volet **Propriétés** (situé dans le coin inférieur droit), sélectionnez la propriété **Name**, puis renommez-la **Time Calculation**.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/image20.png)

7.  Pour créer un élément de calcul, cliquez avec le bouton droit sur la table **Time Intelligence**, puis sélectionnez **Créer**\>**Élément de calcul**.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/image21.png)

8.  Dans le volet gauche, remplacez le nom par défaut par **Current**, puis appuyez sur **Entrée**.

9.  Dans le volet **Éditeur d’expression** (situé au-dessus du volet **Propriétés**), entrez la formule suivante :

    DAX

    ```SELECTEDMEASURE ()```

    ![Interface graphique utilisateur, texte, application, Word Description générée automatiquement](../images/image22.png)

    *La fonction SELECTEDMEASURE retourne une référence à la mesure actuellement en contexte lorsque l’élément de calcul est évalué.*

10. Dans la barre d’outils du volet **Éditeur d’expressions**, sélectionnez le premier bouton pour accepter les modifications.

    ![](../images/image23.png)

11. Créez un deuxième élément de calcul nommé **PY** à l’aide de la formule suivante :

    DAX

    ```CALCULATE ( SELECTEDMEASURE (), SAMEPERIODLASTYEAR ( 'Date'[Date] ) )```

    *La formule PY (prior year) calcule la valeur de la mesure sélectionnée durant l’année précédente.*

12. Créez un troisième élément de calcul nommé **YoY** à l’aide de la formule suivante :

    DAX
    ```
    SELECTEDMEASURE () 
        - CALCULATE ( SELECTEDMEASURE (), 'Time Intelligence'[Time Calculation] = "PY" )
    ```

    *La formule YoY (year-over-year) calcule la différence entre la mesure sélectionnée de l’année en cours et l’année précédente.*

13. Créez un quatrième élément de calcul nommé **YoY %** à l’aide de la formule suivante :

    DAX
    ```
    DIVIDE (
        CALCULATE ( SELECTEDMEASURE (), 'Time Intelligence'[Time Calculation] = "YoY" ),
        CALCULATE ( SELECTEDMEASURE (), 'Time Intelligence'[Time Calculation] = "PY" )
    )
    ```
    *La formule YoY % (year-over-year percentage) calcule le pourcentage de modification de la mesure sélectionnée au cours de l’année précédente.*

14. Dans le volet **Propriétés**, définissez la propriété **Format String Expression** pour : 
    ```
    "0.00%;-0.00%;0.00%"
    ```

    Conseil : l’expression de chaîne de format peut être copiée et collée à partir de **D:\\DP500\\Allfiles\\07\\Assets\\Snippets.txt**.

    ![Interface graphique utilisateur, texte, application
    
Description générée automatiquement](../images/image24.png)

15. Vérifiez que le groupe de calcul **Time Intelligence** possède quatre éléments de calcul.

    ![Texte
    
Description générée automatiquement](../images/image25.png)

16. Pour enregistrer les modifications apportées au modèle Power BI Desktop, dans le menu **Fichier**, sélectionnez **Enregistrer**.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/image26.png)

    **Conseil :***vous pouvez également sélectionner le bouton de barre d’outils ou appuyer sur **Ctrl+S**.*

17. Revenez à Power BI Desktop.

18. Au-dessus du concepteur de rapports, examinez la bannière jaune.

    ![](../images/image27.png)

19. À droite de la bannière, sélectionnez **Actualiser maintenant**.

    ![](../images/image28.png)

    *L’actualisation applique les modifications en créant le groupe de calcul en tant que table de modèles. Il charge ensuite les éléments de calcul sous forme de lignes de données.*

20. Dans le volet **Données**, développez la table **Time Intelligence**.

    ![Texte Description générée automatiquement avec un niveau de confiance moyen](../images/image29.png)

### Mettre à jour le visuel de matrice

Vous allez maintenant modifier le visuel de matrice pour utiliser la colonne **Time Calculation**.

1.  Dans le rapport, sélectionnez le visuel de matrice.

2.  Ensuite, dans le volet **Visualisations**, dans la zone **Valeurs**, sélectionnez **X** pour supprimer le champ **Sales Amount**.

    ![Interface utilisateur graphique, texte, application, e-mail description générée automatiquement](../images/image30.png)

3.  Dans le volet **Données**, à partir de la table **Sales**, faites glisser le champ **Sales** dans le sélecteur **Valeurs**.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/image31.png)

4.  Dans le volet **Données**, à partir de la table **Time Intelligence**, faites glisser le champ **Time Calculation** dans le sélecteur **Colonnes**.

    ![Interface utilisateur graphique, application, Word Description générée automatiquement](../images/image32.png)

5.  Vérifiez que le visuel de matrice affiche une grille de valeurs de mesure **Sales** temporelles qui sont regroupées par mois.

    ![Interface utilisateur graphique, application, table, Excel Description générée automatiquement](../images/image33.png)

    *Le format des valeurs est obtenu à partir des mesures sélectionnées. Toutefois, rappelez-vous que vous définissez l’expression de chaîne de format de la mesure **YoY %** pour produire un format de pourcentage.*

### Créer le groupe de calcul Currency Conversion

Vous allez maintenant créer le groupe de calcul **Currency Calculation**. Il fournira la flexibilité nécessaire pour convertir les mesures de la table **Sales** en devise sélectionnée. Il applique également la mise en forme appropriée pour la devise sélectionnée.

1.  Dans Power BI Desktop, passez dans l’affichage **Données**.

    ![Vue Données.](../images/image34.png)

2.  Dans le volet **Données**, sélectionnez la table **Currency**.

3.  Examinez la colonne masquée **FormatString** qui contient des expressions de chaîne de format pour les valeurs des colonnes.

    ![Interface utilisateur graphique Description générée automatiquement avec un faible niveau de confiance](../images/image35.png)

    *Vous utiliserez une expression DAX pour appliquer la chaîne de format de la devise sélectionnée.*

4.  Basculez vers l’éditeur tabulaire.

5.  Créez un groupe de calcul nommé **Currency Conversion**.

    *En raison de la répétition des tâches, les instructions fournies sont brèves. Si nécessaire, vous pouvez consulter les étapes décrites dans la première tâche de cet exercice.*

    ![Texte
    
Description générée automatiquement](../images/image36.png)

6.  Renommez la colonne **Name** en **Converted Currency**.

    ![Description de texte automatique avec un niveau de confiance faible](../images/image37.png)

7.  Créez un élément de calcul nommé **Currency Conversion** à l’aide de la formule suivante :

    DAX
    ```
    IF (
        HASONEVALUE ( 'Currency'[Currency] ),
        SUMX (
            VALUES ( 'Date'[Date] ),    CALCULATE (
                DIVIDE ( SELECTEDMEASURE (), MAX ( 'Currency Rate'[EndOfDayRate] ) )
            )
        )
    )
    ```
    *Lorsqu’il n’existe qu’une seule devise dans le contexte de filtre, la formule additionne les valeurs quotidiennes de la mesure sélectionnée divisées par le taux de fin de journée de ce jour-là.*

8.  Dans le volet **Propriétés**, définissez la propriété **Format String Expression** sur la formule suivante :

    DAX
    ```
    SELECTEDVALUE ( 'Currency'[FormatString] )
    ```
    Cette formule renvoie la chaîne de format de la devise sélectionnée. De cette façon, la mise en forme est pilotée dynamiquement par les données de la table de dimension **Currency**.

9.  Enregistrez les modifications apportées au modèle Power BI Desktop.

10. Basculez vers Power BI Desktop et actualisez les modifications.

    ![](../images/image28.png)

11. Passez à l’affichage **Report**.

    ![](../images/image38.png)

12. Sélectionnez le visuel de matrice.

13. Dans le volet **Données**, à partir de la table **Currency Conversion**, faites glisser le champ **Converted Currency** dans le volet **Filtres**, dans le groupe **Filtres sur ce visuel**.

    ![Interface graphique utilisateur, texte, application
    
Description générée automatiquement](../images/image39.png)

14. Dans la carte de filtre, vérifiez la valeur de **Currency Conversion**.

    ![Interface graphique utilisateur, texte, application
    
Description générée automatiquement](../images/image40.png)

15. Notez que les formats de valeur sont mis à jour pour décrire clairement les montants en dollar américain.

    ![Texte
    
Description générée automatiquement](../images/image41.png)

16. Dans le segment **Currency**, sélectionnez une autre devise. Puis, dans le visuel de matrice, examinez les valeurs et la mise en forme mises à jour.

17. Rétablissez le segment **Currency** sur **US Dollar**.

    ![Interface graphique utilisateur, application, Teams Description générée automatiquement](../images/image42.png)

18. Notez toutefois que les valeurs **YoY %** ne sont plus des pourcentages.

    *Il y a un problème. Les groupes de calcul **Time Intelligence** et **Currency Conversion** sont appliqués, mais l’ordre de calcul est incorrect. Actuellement, le calcul **YoY %** se produit, puis la conversion monétaire ajoute des résultats de calcul quotidiens au cours du mois. Pour obtenir le bon résultat, l’ordre de calcul doit être inversé. Vous pouvez contrôler l’ordre de calcul en définissant des valeurs de précédence.*

### Modifier la précédence des groupes de calcul

Vous allez maintenant modifier la précédence des deux groupes de calcul.

1.  Basculez vers l’éditeur tabulaire.

2.  Dans le volet gauche, sélectionnez le groupe de calcul **Time Intelligence**.

    ![Texte
    
Description générée automatiquement](../images/image43.png)

3.  Dans le volet **Propriétés**, définissez la propriété **Calculation Group Precedence** sur **20**.

    ![Interface utilisateur graphique, application Description générée automatiquement](../images/image44.png)

    *Plus la valeur est élevée plus la précédence de l’application est élevée. Par conséquent, le groupe de calcul avec la précédence la plus élevée est appliqué en premier.*

4.  Définissez la priorité du groupe de calcul **Currency Conversion** sur **10**.

    ![Interface utilisateur graphique, application, table Description générée automatiquement](../images/image45.png)

    *Ces configurations garantissent que les calculs **Time Intelligence** se produisent ultérieurement.*

5.  Enregistrez les modifications apportées au modèle Power BI Desktop.

6.  Revenez à Power BI Desktop.

7.  Notez que les valeurs **YoY %** sont désormais des pourcentages.

    ![Interface utilisateur graphique,description de texte automatique](../images/image46.png)

### Terminer

Dans cette tâche, vous allez terminer.

1.  Enregistrez le fichier Power BI Desktop.

    ![](../images/image47.png)

2.  Fermez Power BI Desktop.

3.  Fermez Tabular Editor.

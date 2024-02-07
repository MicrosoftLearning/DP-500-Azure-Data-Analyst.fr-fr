---
lab:
  title: Améliorer les performances avec les tables hybrides
  module: Optimize enterprise-scale tabular models
---

# Améliorer les performances avec les tables hybrides

## Vue d’ensemble

**La durée estimée pour effectuer ce tutoriel est de 45 minutes.**

Dans ce labo, vous allez configurer l’actualisation incrémentielle et activer une partition DirectQuery pour fournir des mises à jour en temps réel et améliorer les performances d’actualisation et de requête.

Dans ce labo, vous découvrez comment :

- Configurer l’actualisation incrémentielle

- Examiner les partitions de table

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
   
1. Une fois le référentiel cloné, ouvrez le lecteur D dans l’explorateur de fichiers pour vous assurer que les fichiers ont été téléchargés. Fermez la fenêtre **Invite de commandes**.

### Déployer une base de données Azure SQL Database 

Dans cette tâche, vous allez créer une base de données Azure SQL que vous utiliserez comme source de données pour Power BI. L’exécution du script d’installation crée le serveur de base de données Azure SQL et charge la base de données AdventureWorksDW2022.

1. Pour ouvrir l’Explorateur de fichiers, dans la barre des tâches, sélectionnez le raccourci vers **Explorateur de fichiers**.

2. Accédez au dossier **D:\DP500\Allfiles\10**.

3. Double-cliquez pour ouvrir le script de fichier **setup2.ps1**.
    - Lisez le script dans le Bloc-notes si vous souhaitez savoir quelles ressources vont être configurées. Les lignes qui commencent par # indiquent ce que fait le script.
    - Fermez le script.

5. Dans la zone de recherche de la barre des tâches, entrez `PowerShell`.  
   
   Lorsque les résultats de la recherche s’affichent, sélectionnez **Exécuter en tant qu’administrateur**
    
    *Lorsque vous y êtes invité, sélectionnez Oui pour permettre à cette application d’apporter des modifications à votre appareil.*
1. Dans PowerShell, entrez les 2 lignes de texte suivantes pour exécuter le script. 
    
    ` cd D:\DP500\Allfiles\10`

    Appuyez sur **Entrée**.

    `.\setup2.ps1`
    
    Appuyez sur **Entrée**.

    ![](../images/powershell-script.png)

2. Lorsque vous y êtes invité, entrez le **nom d’utilisateur de votre compte Azure**, le **mot de passe** et **le nom du groupe de ressources**. Appuyez sur **Entrée**. 

    ![](../images/powershell-enter-account-info.png)

    L’exécution du script prend environ 10 à 15 minutes.

    *Remarque : ce labo nécessite un groupe de ressources pour créer une base de données Azure SQL. Si vous effectuez ce labo dans un environnement de labo hébergé, vous devrez peut-être vous connecter au [portail Azure](portal.azure.com) pour obtenir le nom du groupe de ressources. Si vous n’avez pas de groupe de ressources fourni dans un environnement de labo hébergé, [créez un groupe de ressources](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal#create-resource-groups) dans votre abonnement Azure.*

3. Une fois le script terminé, fermez la fenêtre PowerShell.

### Configurer la base de données SQL Azure

Dans cette tâche, vous allez configurer la base de données SQL Azure pour autoriser les connexions à partir de l’adresse IP de votre machine virtuelle. Ce script prend environ 10 minutes après avoir entré votre nom d’utilisateur, votre mot de passe et votre groupe de ressources.

1. Dans un navigateur Web, accédez à [https://portal.azure.com](https://portal.azure.com/).

2. Si vous êtes invité à effectuer une visite guidée, sélectionnez **Peut-être plus tard**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image8.png)

3. Sélectionnez la vignette **Bases de données SQL**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image9.png)

4. Dans la liste des bases de données SQL, sélectionnez la base de données **AdventureWorksDW2022-DP500**.

5. Dans la barre d’action située sous l’onglet Vue d’ensemble, sélectionnez **Définir le pare-feu du serveur**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image10.png)

6. Sous l’onglet Accès public, sélectionnez Réseaux sélectionnés.

7. Sélectionnez **Ajouter l’adresse IPv4 de votre client**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image11.png)

7. Sélectionnez **Enregistrer**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image12.png)

8. Gardez la session du navigateur web du portail Azure ouverte. Vous devrez copier la chaîne de connexion à la base de données dans la tâche **Configurer Power BI Desktop**.

### Configurer Power BI

#### Configurer un compte Power BI dans Power BI Desktop

Dans cette tâche, vous allez configurer Power BI Desktop.

1. Pour ouvrir l’Explorateur de fichiers, dans la barre des tâches, sélectionnez le raccourci vers **Explorateur de fichiers**.

    ![](../images/dp500-create-a-dataflow-image1.png)

1. Accédez au dossier **D:\DP500\Allfiles\10\Starter**.

1. Pour ouvrir un fichier Power BI Desktop prédéveloppé, double-cliquez sur le fichier **Sales Analysis - Improve performance with hybrid tables**.

1. Si vous n’êtes pas déjà connecté, dans le coin supérieur droit de Power BI Desktop, sélectionnez **Se connecter**. Utilisez les informations d’identification du labo pour terminer le processus de connexion.

    ![](../images/dp500-create-a-dataflow-image2.png)

    *Remarque : l’application vous dirigera probablement vers le service Power BI pour effectuer le processus d’inscription.*

1. Pour enregistrer le fichier, dans le ruban **Fichier**, sélectionnez **Enregistrer sous**.

1. Dans la fenêtre **Enregistrer sous**, accédez au dossier **D:\DP500\Allfiles\10\MySolution**.

#### Configurer la version d’évaluation de Power BI Premium

Dans cette tâche, vous allez vous connecter au service Power BI et démarrer une licence d’évaluation.

*Important : si vous avez déjà configuré Power BI dans votre environnement de machine virtuelle, passez à la tâche suivante.*

1. Dans un navigateur Web, accédez à [https://powerbi.com](https://powerbi.com/).

2. Utilisez les informations d’identification du labo pour terminer le processus de connexion.

3. En haut à droite, sélectionnez l’icône de profil, puis sélectionnez **Démarrer la version d’évaluation**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image1.png)

4. Lorsque vous y êtes invité, sélectionnez **Démarrer la version d’évaluation**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image2.png)

    *Vous avez besoin d’une licence Power BI Premium par utilisateur (PPU) pour effectuer ce labo. Une licence d’évaluation est suffisante.*

5. Effectuez les tâches restantes pour terminer la configuration de la version d’évaluation.

    *Conseil : l’expérience du navigateur web de Power BI est appelée **service Power BI**.*

### Créer un espace de travail

Dans cette tâche, vous allez créer un espace de travail.

1. Dans le service Power BI, pour créer un espace de travail, dans le volet **Navigation** (situé à gauche), sélectionnez **Espaces de travail**, puis **Créer un espace de travail**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image3.png)

2. Dans le volet **Créer un espace de travail** (situé à droite), dans la zone **Nom de l’espace de travail**, saisissez un nom pour l’espace de travail.

    *Ce nom doit être unique au sein du locataire.*

    ![](../images/dp500-improve-performance-with-hybrid-tables-image4.png)

3. Sous la zone **Description**, développez la section **Avancé**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image5.png)

4. Définissez l’option **Mode licence** sur **Premium par utilisateur**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image6.png)

    *Power BI prend en charge l’actualisation incrémentielle et les tables hybrides uniquement dans les espaces de travail Premium.*

5. Sélectionnez **Enregistrer**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image7.png)

    *Une fois créé, le service Power BI ouvre l’espace de travail. Vous reviendrez à cet espace de travail plus loin dans ce labo.*

### Configurer Power BI Desktop

Dans cette tâche, vous allez ouvrir une solution Power BI Desktop prédéveloppée, définir les paramètres et autorisations de la source de données, puis actualiser le modèle de données.

1. Pour ouvrir l’Explorateur de fichiers, dans la barre des tâches, sélectionnez le raccourci vers **Explorateur de fichiers**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image13.png)

2. Accédez au dossier **D:\DP500\Allfiles\10\Starter**.

3. Pour ouvrir un fichier Power BI Desktop prédéveloppé, double-cliquez sur le fichier **Sales Analysis - Improve performance with hybrid tables.pbix**

4. Pour modifier la source de données de base de données, sous l’onglet de ruban **Accueil**, dans le groupe **Requêtes**, sélectionnez la liste déroulante **Transformer les données**, puis **Paramètres de source de données**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image14.png)

5. Dans la fenêtre **Paramètres de la source de données**, sélectionnez **Modifier la source**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image15.png)

6. Dans la fenêtre **Base de données SQL Server**, dans la zone **Serveur**, remplacez le texte par le serveur Azure SQL Database du labo. Il se trouve dans le portail Azure, bases de données SQL.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image15a.png)

7. Sélectionnez **OK**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image16.png)

8. Sélectionnez **Modifier les autorisations**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image17.png)

9. Dans la fenêtre **Modifier les autorisations**, pour modifier les informations d’identification de la base de données, sélectionnez **Modifier**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image18.png)

10. Dans la fenêtre **Base de données SQL Server**, entrez le nom d’utilisateur et le mot de passe de la base de données SQL Server, puis enregistrez. 

    Nom d’utilisateur : `sqladmin`

    Mot de passe : `P@ssw0rd01`

    ![](../images/dp500-improve-performance-with-hybrid-tables-image15b.png)

11.  Sélectionnez **OK**.
    ![](../images/dp500-improve-performance-with-hybrid-tables-image19.png)

12. Dans la fenêtre **Paramètres de la source de données**, sélectionnez **Fermer**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image20.png)

13. Dans l’onglet de ruban **Accueil**, à l’intérieur du groupe **Requêtes**, cliquez sur **Actualiser**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image21.png)

14. Attendez la fin du processus d’actualisation des données.

15. Pour enregistrer le fichier, sous l’onglet de ruban **Fichier**, sélectionnez **Enregistrer sous**.

16. Dans la fenêtre **Enregistrer sous**, accédez au dossier **D:\DP500\Allfiles\10\MySolution**.

17. Sélectionnez **Enregistrer**.

18. Si vous n’êtes pas déjà connecté, dans le coin supérieur droit de Power BI Desktop, sélectionnez **Se connecter**. Utilisez les informations d’identification du labo pour terminer le processus de connexion.

    *Important : vous devez utiliser les mêmes informations d’identification que celles utilisées pour vous connecter au service Power BI.*

    ![](../images/dp500-improve-performance-with-hybrid-tables-image22.png)

### Vérifier l’état

Dans cette tâche, vous allez examiner le rapport prédéveloppé.

1. Dans Power BI Desktop, examinez la conception du rapport.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image23.png)

    *La page de rapport comporte un titre et deux visuels. Le visuel de segment permet de filtrer par un seul exercice, tandis que le visuel graphique à barres affiche les montants mensuels des ventes. Dans ce labo, vous allez améliorer les performances du rapport en configurant l’actualisation incrémentielle et une table hybride.*

### Examiner le modèle de données

Dans cette tâche, vous allez passer en revue le modèle de données prédéveloppé.

1. Basculez vers la vue **Modèle**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image24.png)

2. Utilisez le diagramme de modèle pour passer en revue la conception du modèle.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image25.png)

    *Le modèle comprend cinq tables de dimension et une table de faits. Chaque table utilise le mode de stockage d’importation. La table des faits **Sales** représente les détails de la commande client. Il s’agit d’une conception classique de schéma en étoile.*

    *Dans ce labo, vous allez configurer la table **Sales** pour qu’elle utilise l’actualisation incrémentielle et qu’elle devienne une table hybride. Une table hybride comprend une partition DirectQuery qui représente la dernière période. Cette partition garantit que les données actuelles de la source de données sont disponibles dans les rapports Power BI.*

## Configurer l’actualisation incrémentielle

Dans cet exercice, vous allez configurer l’actualisation incrémentielle.

*L’actualisation incrémentielle étend les opérations d’actualisation planifiées en fournissant la création et la gestion automatisées des partitions pour les tables de jeux de données qui chargent fréquemment des données nouvelles et mises à jour. Elle permet d’accélérer l’actualisation, en plaçant des charges plus faibles sur les données sources et Power BI. Elle peut également aider à exposer les données actuelles au rapport Power BI plus rapidement.*

### Ajouter des paramètres

Dans cette tâche, vous allez ajouter deux paramètres.

1. Pour ouvrir la fenêtre Éditeur Power Query, sous l’onglet de ruban **Accueil**, dans le groupe **Requêtes**, sélectionnez l’icône **Transformer les données**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image26.png)

2. Dans la fenêtre Éditeur Power Query, dans le volet **Requêtes**, sélectionnez la requête **Sales**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image27.png)

3. Dans le volet d’aperçu, notez la colonne **OrderDate**, qui est une colonne date/heure.

    *L’actualisation incrémentielle nécessite que la table contienne une colonne de date date/heure ou un type de données entier avec la valeur au format aaaammjj.*

    *Pour configurer l’actualisation incrémentielle, vous devez créer des paramètres que Power BI utilisera pour filtrer cette colonne pour créer des partitions de table.*

4. Pour créer un paramètre, sous l’onglet de ruban **Accueil**, sélectionnez l’icône **Gérer les paramètres**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image28.png)

5. Dans la fenêtre **Gérer les paramètres**, sélectionnez **Nouveau**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image29.png)

6. Dans la zone **Nom**, remplacez le texte par **RangeStart**.

7. Dans la liste déroulante **Type**, sélectionnez **Date/Heure**.

8. Dans la zone **Valeur actuelle**, saisisez **6/1/2022** (1er juin 2022 ; la machine virtuelle utilise les formats de date américains). 

    *Notez que pour les emplacements au format autre que MM-JJ-AAAA, la date doit être saisie comme 1/6/2022*

    *Lors de la configuration des paramètres, vous pouvez utiliser des valeurs arbitraires. Power BI met à jour les valeurs des paramètres lorsqu’il crée et gère les partitions. Dans ce labo, vous allez définir une plage pour le mois de juin 2022.*

    ![](../images/dp500-improve-performance-with-hybrid-tables-image30.png)

9. Sélectionnez **Nouveau** pour créer un deuxième paramètre.

10. Définissez les propriétés du paramètre suivantes :

    - Nom : **RangeEnd**

    - Type : **Date/Heure**

    - Valeur actuelle : **7/1/2022** (1er juillet 2022)

     *Notez que pour les emplacements au format autre que MM-DD-AAAA, la date doit être saisie comme 1/7/2022*

    ![](../images/dp500-improve-performance-with-hybrid-tables-image31.png)

11. Sélectionnez **OK**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image32.png)

### Filtrer la requête

Dans cette tâche, vous allez ajouter des filtres à la requête **Sales**.

1. Dans le volet **Requêtes**, sélectionnez la requête **Sales**.

2. Dans l’en-tête de la colonne **OrderDate**, sélectionnez la flèche vers le bas, puis sélectionnez **Filtres Date/heure** > **Entre**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image33.png)

3. Dans la fenêtre **Filtrer les lignes**, sélectionnez la première liste déroulante d’icônes de calendrier, puis sélectionnez **Paramètre**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image34.png)

4. Dans la liste déroulante adjacente, notez que le paramètre **RangeStart** est défini.

    *La sélection de paramètres par défaut est correcte.*

5. Dans la deuxième liste déroulante « plage », sélectionnez la liste **est avant**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image35.png)

6. Dans les listes déroulantes correspondantes, sélectionnez le paramètre **RangeEnd**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image36.png)

7. Sélectionnez **OK**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image37.png)

8. Sous l’onglet de ruban **Accueil**, dans le groupe **Fermer**, cliquez sur l’icône **Fermer &amp; appliquer**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image38.png)

9. Notez que Power BI Desktop a chargé 5 134 lignes dans la table **Sales**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image39.png)

    *Il s’agit des lignes filtrées pour le mois de juin 2022.*

10. Enregistrez le fichier Power BI Desktop.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image40.png)

### Configurer l’actualisation incrémentielle

Dans cette tâche, vous allez configurer la stratégie d’actualisation incrémentielle pour la table **Sales**.

1. Dans le diagramme de modèle, cliquez avec le bouton droit sur l’en-tête de la table **Sales**, puis sélectionnez **Actualisation incrementielle**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image41.png)

2. Dans la fenêtre **Actualisation incrémentielle et données en temps réel**, à l’étape 2, activez l’actualisation incrémentielle.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image42.png)

3. Définissez : Démarrage des données d’archive **2 ans** avant la date d’actualisation.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image43.png)

    *Ce paramètre détermine la période historique. Dans cette instance, Power BI crée deux partitions d’année entière pour les données historiques.*

4. Définissez : Démarrage de l’actualisation incrémentielle des données **7 jours** avant la date d’actualisation.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image44.png)

    *Ce paramètre détermine la période d’actualisation incrémentielle selon laquelle toutes les lignes dont la date/heure est comprise dans cette période sont incluses dans la ou les partitions d’actualisation et actualisées à chaque opération d’actualisation.*

5. À l’étape 3, cochez l’option **Obtenir les données les plus récentes en temps réel avec DirectQuery**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image45.png)

    *Ce paramètre permet d’extraire les dernières modifications de la table sélectionnée dans la source de données au-delà de la période d’actualisation incrémentielle à l’aide de DirectQuery. Toutes les lignes avec une date/heure postérieures à la période d’actualisation incrémentielle sont incluses dans une partition DirectQuery et extraites de la source de données lors de chaque requête de jeu de données. Ce paramètre rend la table hybride, car elle contiendra des partitions d’importation et une partition DirectQuery.*

6. Sélectionnez **Appliquer**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image46.png)

7. Enregistrez le fichier Power BI Desktop.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image47.png)

### Publier le jeu de données

Dans cette tâche, vous allez publier le jeu de données.

1. Pour publier le rapport, sous l’onglet de ruban **Accueil**, sélectionnez le bouton **Publier**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image48.png)

2. Dans la fenêtre **Publier sur Power BI**, sélectionnez l’espace de travail créé dans ce labo, puis cliquez sur Sélectionner.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image49.png)

3. Une fois la publication réussie, sélectionnez **OK**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image50.png)

4. Fermez Power BI Desktop.

5. Si vous êtes invité à enregistrer vos modifications, sélectionnez **Enregistrer**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image51.png)

### Configurer le jeu de données

Dans cette tâche, vous allez configurer les informations d’identification de la source de données et actualiser le jeu de données.

1. Basculez vers la session de navigateur web du service Power BI.

2. Dans la page d’accueil de l’espace de travail, recherchez le rapport et le jeu de données.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image52.png)

3. Placez le curseur sur le jeu de données et, lorsque les points de suspension s’affichent, cliquez dessus, puis sélectionnez **Paramètres**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image53.png)

4. Dans la section **Informations d’identification de la source de données**, sélectionnez le lien **Modifier les informations d’identification**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image54.png)

5. Dans la fenêtre, entrez le nom d’utilisateur et le mot de passe, puis définissez le niveau de confidentialité sur Organisationnel.
       
    Nom d’utilisateur : `sqladmin`

    Mot de passe : `P@ssw0rd01`

    ![](../images/dp500-improve-performance-with-hybrid-tables-image54b.png)

6. Sélectionnez **Connexion**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image55.png)

8. Développez la section **Actualisation planifiée et optimisation des performances**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image56.png)

9. Notez les paramètres, sans les changer.

    *Dans une configuration réelle, vous planifiez l’actualisation des données pour permettre à Power BI d’actualiser et de gérer les partitions de manière périodique.*

    *Dans ce labo, vous allez effectuer une actualisation à la demande.*

10. Dans le volet **Navigation** (à gauche), sélectionnez votre espace de travail.

11. Dans la page d’accueil de l’espace de travail, placez le curseur sur le jeu de données, puis sélectionnez l’icône **Actualiser**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image57.png)

12. Dans la colonne **Actualisé**, notez l’icône de rotation et attendez qu’elle s’arrête (indiquant que l’actualisation est terminée).

    ![](../images/dp500-improve-performance-with-hybrid-tables-image58.png)

13. Pour ouvrir les paramètres de l’espace de travail, en haut à droite, sélectionnez **Paramètres**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image59.png)

14. Dans le volet **Paramètres**, sélectionnez l’onglet **Premium**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image60.png)

15. Pour copier la chaîne de connexion à l’espace de travail, sélectionnez **Copier**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image61.png)

    *Vous allez utiliser la connexion à l’espace de travail pour vous y connecter dans SQL Server Management Studio (SSMS).*

16. Sélectionnez **Annuler** pour fermer le volet.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image62.png)

### Passer en revue les partitions de table

Dans cette tâche, vous allez utiliser SSMS pour passer en revue les partitions de table.

1. Pour ouvrir SSMS, dans la barre des tâches, sélectionnez le raccourci vers **SSMS**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image63.png)

2. Dans la fenêtre **Se connecter au serveur**, dans la liste déroulante **Type de serveur**, sélectionnez **Services d’analyse**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image64.png)

    *Vous pouvez utiliser SSMS pour vous connecter à l’espace de travail à l’aide du point de terminaison en lecture/écriture XMLA. Le point de terminaison est disponible uniquement pour les espaces de travail Premium.*

3. Dans la zone **Nom du serveur**, remplacez le texte en collant la connexion à l’espace de travail (appuyez sur **Ctrl+V**).

4. Dans la liste déroulante **Authentification**, sélectionnez **Azure Active Directory - Mot de passe**.

5. Entrez vos informations d’identification.

6. Cliquez sur **Se connecter**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image65.png)

7. Dans l’explorateur d’objets (à gauche), développez le dossier **Bases de données**, ouvrez la base de données (jeu de données) **Sales Analysis...**, puis le dossier **Tables**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image66.png)

8. Cliquez avec le bouton droit sur la table **Sales**, puis sélectionnez **Partitions**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image67.png)

9. Dans la fenêtre **Partitions**, notez la liste des partitions de l’historique sur deux ans, suivie des partitions trimestrielles et des partitions quotidiennes.

10. Faites défiler la liste vers le bas et notez que la dernière partition est une partition DirectQuery des dates actuelles et futures.

    *Power BI crée et gère automatiquement toutes ces partitions.*

11. Sélectionnez **Annuler**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image68.png)

## Tester la table hybride

Dans cet exercice, vous allez ouvrir le rapport, ajouter une commande client, puis voir la mise à jour des données du rapport.

### Ouvrir le rapport

Dans cette tâche, vous allez ouvrir le rapport.

1. Basculez vers la session de navigateur web du service Power BI.

2. Dans la page d’accueil de l’espace de travail, sélectionnez le rapport.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image69.png)

3. Si nécessaire, dans le segment **Fiscal Year**, sélectionnez l’année fiscale qui contient le mois en cours (par rapport à la date actuelle).

    *Le mois en cours doit être visible sous forme de barre dans l’histogramme.*

    *Notez que les mois d’août 2022 et suivants ne font pas partie de l’exercice 2022, qui est la valeur par défaut du segment.*

### Ajouter une commande à la base de données

Dans cette tâche, vous allez ajouter une commande à la base de données.

1. Basculez vers SSMS.

2. Pour ouvrir un fichier de script, dans le menu **Fichier**, sélectionnez **Ouvrir** > **Fichier**.

3. Dans la fenêtre **Ouvrir un fichier**, accédez au dossier **D:\DP500\Allfiles\10\Assets**.

4. Sélectionnez le fichier **1-InsertOrder.sql**, puis **Ouvrir**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image70.png)

5. Dans la fenêtre **Se connecter au moteur de base de données**, vérifiez que la liste déroulante **Nom du serveur** est définie sur le serveur Azure SQL Database du labo.

6. Dans la liste déroulante **Authentification**, sélectionnez **Authentification SQL Server**.

7. Entrez le nom d’utilisateur **sqladmin** et le mot de passe.

8. Cliquez sur **Se connecter**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image71.png)

9. Examinez le script.

    *Ce script insère une commande unique dans la table **FactInternetSales** en utilisant la date du jour comme date de commande.*

10. Pour exécuter un script, dans la barre d’outils, sélectionnez **Exécuter** (ou appuyez sur **F5**).

    ![](../images/dp500-improve-performance-with-hybrid-tables-image72.png)

11. Pour fermer le fichier, dans le menu **Fichier**, cliquez sur **Fermer**.

### Actualiser le rapport

Dans cette tâche, vous allez actualiser le rapport.

1. Basculez vers la session de navigateur web du service Power BI.

2. Dans le rapport, notez le montant des ventes du mois en cours.

3. Dans la barre d’action, sélectionnez la commande **Actualiser**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image73.png)

4. Une fois l’actualisation du rapport terminée, vérifiez que le montant des ventes du mois en cours a augmenté de 10 000 dollars.

    *Lorsque Power BI a interrogé la table **Sales**, elle a récupéré les données actuelles de la partition DirectQuery, qui a interrogé directement la base de données SQL Azure.*

    *Conseil : les tables hybrides fonctionnent particulièrement bien avec l’actualisation automatique des pages, une fonctionnalité qui actualise les rapports Power BI de manière automatique.*

### Terminer

Dans cette tâche, vous allez terminer. Ouvrez SSMS et vérifiez que vous êtes connecté à la base de données AdventureWorksDW2022-DP500.

1. Dans SSMS, ouvrez le fichier **2-Cleanup.sql**.

    ![](../images/dp500-improve-performance-with-hybrid-tables-image74.png)

    Ce script supprime la commande que vous avez insérée.

2. Exécutez le script.

3. Fermez SSMS.

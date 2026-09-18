<div align="center">
  <img src="https://github.com/user-attachments/assets/1712aff9-d8e8-4a29-8461-b7efcd9f755c" alt="Logo SARA" width="96">

  # SARA

  **Save And Restore Application**

  Sauvegardez simplement. Restaurez avec confiance.

  ![Version](https://img.shields.io/badge/version-2.0.1.0-2474D2)
  ![Plateforme](https://img.shields.io/badge/plateforme-Windows%2010%20%7C%2011-0078D4)
  ![AutoIt](https://img.shields.io/badge/AutoIt-3.3.16%2B-5C2D91)
  ![Architecture](https://img.shields.io/badge/architecture-x64-555555)
</div>

SARA est une application Windows de sauvegarde et de restauration développée en AutoIt. Elle permet de protéger les dossiers utilisateur, les profils de logiciels et plusieurs paramètres Windows depuis une interface simple, sans dépendre d’un service cloud.

La version 2 repose sur un moteur entièrement réécrit pour rester réactif pendant les copies volumineuses et ne jamais annoncer une réussite lorsqu’un élément a échoué.

<p align="center">
  <img alt="Sauvegarde manuelle" src="https://github.com/user-attachments/assets/236d8678-0531-4402-a459-bd2e9eeae2e1" width="32%">
  <img alt="Planifier sauvegarde" src="https://github.com/user-attachments/assets/50e3785a-58a7-47e9-b33a-24dcd04266f9"  width="32%">
  <img alt="Journal" src="https://github.com/user-attachments/assets/75fd1c7c-dafc-471c-8d4f-e9ead0523666" width="32%">
</p>

## Fonctionnalités

- sauvegardes horodatées sans écrasement des générations précédentes ;
- interface réactive pendant les transferts importants ;
- copie asynchrone avec `robocopy.exe` ;
- ajout de dossiers personnalisés ;
- annulation propre d’une opération en cours ;
- vérification facultative de l’intégrité après la copie ;
- interprétation réelle des codes de retour Robocopy ;
- résultat global `Complete`, `Partial` ou `Cancelled` ;
- manifeste détaillant chaque élément sauvegardé ;
- journal complet pour chaque exécution ;
- restauration guidée par le manifeste ;
- prise en charge des dossiers Windows redirigés ;
- création de sauvegardes hebdomadaires dans le Planificateur de tâches Windows ;
- aucune fermeture forcée des applications de l’utilisateur.

## Nouveautés de la version 2.0.1.0

- nouvelle interface de planification avec récapitulatif de la sélection ;
- choix du nom, du jour et de l’heure de la tâche planifiée ;
- interface modernisée et homogénéisation des icônes ;
- amélioration de la compatibilité avec les versions actuelles d’AutoIt ;
- amélioration des contrôles de chemins, des messages d’erreur et des journaux.

## Éléments pris en charge

SARA détecte automatiquement la présence des éléments disponibles sur le PC. Les éléments absents sont affichés en gris et ne peuvent pas être sélectionnés.

| Catégorie | Exemples |
| --- | --- |
| Dossiers utilisateur | Bureau, Documents, Téléchargements, Images, Musique et Vidéos |
| Navigateurs | Microsoft Edge, Google Chrome, Mozilla Firefox et Opera |
| Messagerie | Signatures Outlook, Thunderbird, Mailspring et Proton Mail |
| Bureautique | OpenOffice, LibreOffice, WordPerfect, Microsoft Office et WPS Office |
| Applications | Pense-bêtes Windows et timbres PDF-XChange |
| Windows | Menu Démarrer, raccourcis réseau, imprimantes, éléments épinglés et variables d’environnement |

Vous pouvez également ajouter n’importe quel dossier accessible avec le bouton **Ajouter**.

## Prérequis

### Pour utiliser l’exécutable

- Windows 10 ou Windows 11 en 64 bits ;
- les droits d’accès aux dossiers source et destination.

SARA utilise `robocopy.exe`, `reg.exe` et `schtasks.exe`, déjà fournis avec Windows.

## Utilisation

### Effectuer une sauvegarde

1. Ouvrez l’onglet **Sauvegarder**.
2. Sélectionnez le dossier qui contiendra les sauvegardes.
3. Cochez les éléments à protéger.
4. Activez ou désactivez la vérification d’intégrité.
5. Fermez les logiciels signalés par SARA afin d’éviter les fichiers verrouillés.
6. Cliquez sur **Sauvegarder**.
7. Consultez le résultat et le journal de l’opération.

Chaque exécution crée un nouveau dossier :

```text
SARA_Backup_YYYYMMDD_HHMMSS\
├── Data\
├── Logs\SARA.log
└── manifest.ini
```

### Restaurer une sauvegarde

1. Ouvrez l’onglet **Restaurer**.
2. Sélectionnez un dossier `SARA_Backup_YYYYMMDD_HHMMSS`.
3. Cochez les éléments à restaurer.
4. Vérifiez leurs destinations d’origine.
5. Lancez la restauration et consultez le journal.

> [!WARNING]
> Ne déplacez pas `manifest.ini` hors du dossier de sauvegarde. Il décrit les données présentes et leurs destinations de restauration.

La restauration remplace les fichiers portant le même nom, mais ne supprime pas les fichiers supplémentaires déjà présents dans la destination.

## Sauvegardes planifiées

1. Dans l’onglet **Sauvegarder**, choisissez la destination.
2. Cochez les éléments à inclure dans le plan.
3. Cliquez sur **Planifier**.
4. Contrôlez le récapitulatif de la sélection.
5. Saisissez le nom de la tâche, le jour et l’heure.
6. Cliquez sur **Créer la tâche**.

SARA enregistre la sélection, la destination et l’option de vérification dans un plan local. La tâche Windows lance ensuite :

```text
SARA.exe /runplan "chemin-du-plan.ini"
```

La tâche est créée pour l’utilisateur connecté. Une tâche portant le même nom est remplacée. La création effective de la tâche nécessite la version compilée de SARA.

Codes de sortie d’une exécution planifiée :

| Code | Signification |
| ---: | --- |
| `0` | Sauvegarde terminée sans échec |
| `1` | Sauvegarde partielle, au moins un élément a échoué |
| `2` | Opération annulée |
| `10+` | Erreur de lecture du plan |
| `20+` | Erreur de préparation de la sauvegarde |

## Fichiers créés par SARA

SARA conserve ses paramètres de fonctionnement dans :

```text
%LOCALAPPDATA%\SARA
```

| Emplacement | Contenu |
| --- | --- |
| `config.ini` | Configuration locale de l’application |
| `logs\` | Journaux d’exécution |
| `plans\` | Plans et fichiers XML des tâches planifiées |
| `icons\` | Ressources graphiques extraites de l’exécutable |

Ces fichiers ne sont pas nécessaires pour démarrer une première fois SARA : ils sont créés automatiquement.

## Fiabilité et sécurité

- chaque dossier est copié dans un processus séparé afin de préserver la réactivité de l’interface ;
- les codes de retour Robocopy sont contrôlés avant de déclarer une copie réussie ;
- les sauvegardes précédentes ne sont pas écrasées ;
- les jonctions sont exclues pour limiter les risques de boucle récursive ;
- SARA refuse une destination placée à l’intérieur d’une source sélectionnée ;
- un contrôle minimal de l’espace libre est effectué avant la copie ;
- les erreurs, fichiers verrouillés et accès refusés sont consignés dans les journaux ;
- aucun service cloud n’est requis.

### Limites à connaître

- SARA ne contourne pas les permissions Windows ;
- un profil de navigateur ou de messagerie actif peut contenir des fichiers verrouillés ou incohérents ;
- la vérification d’intégrité augmente la durée totale de l’opération ;
- une sauvegarde unique ne remplace pas une stratégie comprenant plusieurs générations et un support externe ;
- une tâche planifiée ne fonctionnera plus si `SARA.exe` ou son plan est déplacé.

## Dépannage

### La tâche planifiée ne démarre plus

Vérifiez que `SARA.exe` se trouve toujours à l’emplacement utilisé lors de la création de la tâche. Si le fichier a été déplacé, supprimez ou remplacez la tâche depuis le Planificateur Windows, puis recréez-la avec SARA.

### Une sauvegarde est indiquée comme partielle

Ouvrez l’onglet **Journal**. Les causes les plus fréquentes sont un fichier verrouillé, un support déconnecté, un manque d’espace ou des droits insuffisants.

### L’antivirus signale l’exécutable

Les exécutables AutoIt peuvent occasionnellement provoquer des détections heuristiques. Compilez le projet depuis les sources vérifiées et analysez le fichier avec votre solution de sécurité. La signature numérique de l’exécutable est recommandée avant une diffusion publique.

## Auteur

SARA est développé par **Benjamin LEQUEUX**.

© 2026 Benjamin LEQUEUX. Tous droits réservés.

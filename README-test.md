# Baromètre numérique — copie de test simplifiée

Copie Grist : https://grist.numerique.gouv.fr/o/dnumdesign/dwg6M3fKf3X5/Barometre-numerique-TEST-simplification-2026

## Structure active

- Questions : source du questionnaire. Colonnes conservées, plus Code et Annee.
- Resultats : une ligne par participation ; Annee, Date_soumission, Q01 à Q09, Q10_commentaire.
- Tableau de bord : vue personnalisée liée à Resultats ; lecture des libellés dans Questions.

Les anciennes tables Questionnaires, Choix, Participations et Reponses restent intactes. Seuls leurs noms de pages sont préfixés Archives dans la copie. Les trois participations historiques et leurs 27 réponses n'ont pas été migrées et ne sont pas comptées dans le nouveau tableau de bord. Le document original et index.html ne sont pas modifiés.

## Pages de test

- barometre-test.html?annee=2026&v=2 : questionnaire, source Questions, toutes les colonnes nécessaires visibles dont Code et Annee. Accès complet conservé pour les essais.
- tableau-de-bord-test.html : tableau de bord, source Resultats, accès complet pour lire les libellés Questions.

## Codage

4 = tout à fait d'accord ; 3 = plutôt d'accord ; 2 = plutôt pas d'accord ; 1 = pas du tout d'accord ; 0 = non concerné ; null = question ignorée. Q10_commentaire : texte libre.

Ne jamais inclure 0 ni les cellules vides dans une moyenne. Le questionnaire écrit explicitement null pour chaque note ignorée : l'omission d'une colonne numérique lors d'un AddRecord peut produire 0 dans Grist.

Date_soumission est de type DateTime Europe/Paris, avec formule d'initialisation NOW(), sur les nouvelles lignes uniquement, sans réapplication à chaque modification. Le questionnaire n'envoie pas l'heure du navigateur.

## Vérifications réalisées le 19 octobre 2026

- Soumission complète via l'interface : une seule ligne nouvelle, date serveur renseignée, scores 4/3/2/1/0 conformes et commentaire conservé.
- Soumission partielle : non-concerné distinct des questions ignorées ; second essai réalisé après correction explicite des valeurs null.
- Premier essai partiel corrigé dans la copie pour rétablir les absences réelles.
- Question obligatoire activée temporairement : le passage sans réponse est bloqué ; réglage rétabli à facultatif ensuite.
- Récapitulatif et remerciement après succès conservés.
- Tableau de bord : 5 participations fictives en 2026 et 1 en 2027 ; 17 absences sur 45 notes possibles en 2026 ; Q01 moyenne 3,33 sur 3 avis notés, hors 2 non-concernés.
- Filtre annuel 2026/2027, distributions et ouverture des verbatims vérifiés dans Grist.

## Reconduction annuelle

Conserver les lignes Questions existantes. Ajouter les formulations de la nouvelle année avec Annee et les mêmes codes uniquement lorsque leur sens reste comparable. Changer le paramètre annee du widget. Les textes généraux de l'interface restent ceux de l'édition 2026 et doivent être actualisés pour une nouvelle campagne. Le tableau de bord signale lorsqu'un libellé d'édition n'est pas encore renseigné. Le scénario 2027 est fictif et n'est pas une campagne prête à ouvrir.

Les codes actuels Q01–Q09 sont réservés aux affirmations et Q10_commentaire au texte libre. Ajouter d'autres questions exige de créer les colonnes correspondantes et d'adapter la liste des codes et le tableau de bord.

## Avant diffusion réelle

Cette version est un environnement de test administrateur. Aucun nouveau partage aux agents n'a été créé. L'accès complet du widget ne constitue pas une architecture de collecte sécurisée pour des répondants. Prévoir des droits séparés de dépôt et de lecture, vérifier avec un compte non administrateur, puis traiter confidentialité et éventuelle unicité de participation. Aucun secret ni clé API n'est inclus dans les pages publiques.

## Retour arrière

Dans la copie seulement, l'ancien widget peut être rétabli en remettant son URL https://dnum-socialgouv.github.io/questionnaire-dsfr/?v=7 et la source Questions. Il écrira de nouveau dans Participations/Reponses. Ne pas supprimer Resultats ni les tables historiques pour revenir en arrière. Le document original est resté sur l'ancien fonctionnement.

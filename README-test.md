# Baromètre numérique — copie de test

## État déployé le 9 novembre 2026

- Questionnaire public sans connexion : https://obckt-01kzx74me27ew3385a8ccha7f8.s3.gra.io.cloud.ovh.net/barometre-dsfr-test/index.html
- Administration : https://grist.numerique.gouv.fr/o/dnumdesign/dwg6M3fKf3X5/Barometre-numerique-TEST-simplification-2026/p/9
- Widget questionnaire : `barometre-test.html?annee=2026&v=20261109-auto`.
- Widget tableau de bord : `tableau-de-bord-test.html?v=20261109-controle2`.

Le questionnaire enregistre lorsque la personne termine. La confirmation comprend les étapes suivantes, puis le récapitulatif modifiable. Une correction met à jour la même participation tant que la page conserve son état. Le wrapper S3 masque la navigation Grist.

## Clé de participation et droits

Le wrapper S3 génère une clé cryptographique aléatoire de 64 caractères hexadécimaux par ouverture et la transmet via `Participation_`. La formule d’initialisation de `Session_repondant` la récupère et vérifie son format. Ne pas utiliser `user.SessionID` : les visiteurs anonymes peuvent partager une identité technique, notamment lorsque les cookies tiers sont bloqués.

Les ACL Grist autorisent les non-propriétaires à lire les questions et textes de campagne, créer une réponse valide pour 2026 avec leur clé et corriger uniquement cette réponse. Elles refusent la lecture des résultats, la suppression, les modifications de structure et l’accès aux champs de vérification. Les autres tables restent privées. Les propriétaires administrent le document.

La création exige `Nb_envois_cle == 1`, un format valide et des champs de vérification initialement vides ou faux. La correction conserve clé, année, compte et date. Les colonnes de contrôle sont interdites en lecture et modification aux non-propriétaires. Les formules et ACL se configurent dans Grist ; le HTML seul ne suffit pas.

Une nouvelle ouverture génère une nouvelle clé et permet une autre réponse, conformément au choix de garder un accès sans connexion. Cette clé permet de gérer une participation ; elle ne prouve ni l’identité ni la sincérité d’un avis. Aucun secret d’API n’est inclus dans le code public.

## Vérifier les contributions

Dans le tableau de bord administrateur, ouvrir **Vérifier les contributions**. Les filtres affichent les contributions à examiner, toutes les réponses, les réponses écartées ou les réponses vérifiées et conservées. Une note facultative documente la décision.

- **Conserver dans les résultats** : enregistre la vérification de cette version de la réponse.
- **Écarter des résultats** : retire la participation des indicateurs, graphiques et verbatims, en conservant sa ligne Grist.
- **Réintégrer aux résultats** : remet la réponse dans les calculs.
- **Remettre à examiner** : retire le statut de vérification.

Les indices signalent au moins 10 participations en une minute, les mêmes neuf notes et le même commentaire normalisé d’au moins 20 caractères, un format invalide, une clé utilisée plusieurs fois ou une réponse modifiée après vérification. Ils ne prouvent pas une fraude et ne déclenchent aucune exclusion automatique. Un avis entièrement positif ou négatif n’est pas en soi un signal.

Une correction après vérification revient à examiner. Une réponse écartée reste exclue après correction jusqu’à sa réintégration administrative.

Ce dispositif facilite surveillance et modération. Il ne fournit pas de limitation globale du débit, de CAPTCHA ni de garantie contre un automate générant de nouvelles clés. Aucun nom, adresse IP ou identifiant d’appareil n’est ajouté par ce contrôle.

## Structure et codage

`Questions` contient les formulations par `Code` et `Annee`. `Resultats` contient une ligne par participation : année, date, neuf notes, commentaire et champs techniques :

| Colonne | Usage |
| --- | --- |
| `Session_repondant` | Clé initialisée par Grist, immuable pour le répondant |
| `Exclure_des_resultats` | Exclusion réversible, faux par défaut |
| `Controle_effectue` | Vérification administrative, faux par défaut |
| `Motif_controle` | Note interne facultative |
| `Nb_envois_cle` | Nombre de lignes portant la même clé, calculé |
| `Reponse_valide` | Validation serveur du format et des champs obligatoires |
| `Empreinte_controle` | Version des neuf notes et du commentaire vérifiée |

Le bouton d’activation du tableau de bord crée les colonnes de contrôle manquantes. Il ne configure pas les ACL : reproduire et vérifier celles-ci séparément dans toute nouvelle copie.

Notes : 4 = tout à fait d’accord ; 3 = plutôt d’accord ; 2 = plutôt pas d’accord ; 1 = pas du tout d’accord ; 0 = non concerné ; `null` = question ignorée. Les moyennes excluent 0 et les cellules vides. Le front écrit explicitement `null` pour une note ignorée : l’omission d’une colonne numérique peut produire 0 dans Grist.

Le commentaire est limité à 500 caractères et à la limite configurée dans Questions si elle est plus restrictive. La formule serveur vérifie types et bornes des notes, questions obligatoires et réponse non entièrement vide. Des réponses historiques incomplètes peuvent être signalées sans être automatiquement écartées.

`Date_soumission` est un DateTime Europe/Paris initialisé par `NOW()` sur les nouvelles lignes uniquement. Le questionnaire n’envoie pas l’heure du navigateur. Les anciennes tables et réponses restent conservées ; le document original et le widget historique `index.html` ne sont pas modifiés.

## Vérifications du 9 novembre 2026

- Envoi par le lien S3 sans connexion enregistré ; second envoi avec la même clé refusé par Grist.
- Correction après enregistrement : même participation mise à jour.
- Exclusion fictive : compte diminué d’une participation, ligne conservée ; réintégration : compte rétabli.
- Vérification puis correction par le répondant : signal « Réponse modifiée après vérification » et retour à examiner.
- Contribution refusée absente ; deux contributions fictives d’essai retirées ensuite.
- Tests de logique : seuils temporels, réponses identiques, formats, exclusions et invalidation d’une vérification après correction.

## Reconduction et retour arrière

Pour une nouvelle campagne, conserver les anciennes questions et ajouter celles de la nouvelle année. Actualiser l’année du widget, les textes et la condition de création des ACL, actuellement limitée à 2026. Vérifier les droits en session non administrateur avant diffusion. Les codes Q01–Q09 désignent les notes et Q10_commentaire le commentaire ; ajouter d’autres questions exige d’adapter schéma, validation et code.

Pour annuler une exclusion, utiliser **Réintégrer aux résultats**. Pour revenir sur l’interface, rétablir une URL de widget dans la copie ou revenir sur le commit ; ne pas supprimer les résultats ni les tables historiques. Le retour au widget historique écrit dans les anciennes tables et exige une nouvelle vérification des droits.

# Validation de la version ?tendue - 7 novembre 2026

Les tests logiciels de la version actuelle sont consign?s dans `unit_tests_latest.json` : 16 tests r?ussis sur des fixtures synth?tiques. Les contr?les COCO r?els sont distincts : `audit.json`, `quality_control.json` et `phase1_review_20261107.json`.

La pr?paration utilise maintenant 10 000 images de base, 100 de validation distinctes et 1 000 de test. Les 100 premi?res images de test sont conserv?es comme sous-ensemble de comparaison. Les matrices CLIP sont identiques ? la version pr?c?dente, apr?s comparaison des v?ritables entr?es de l'encodeur. Voir `phase1_upgrade.json`.

Les r?f?rences humaines portent explicitement confiance=1 et seuil=1 ; les attributs heuristiques n'h?ritent pas de cette convention. Aucun d?tecteur visuel, entra?nement, g?n?ration finale ni ?valuation finale n'est ex?cut?. La liste commune de la classe et la validation de l'enseignante restent externes.

Les anciennes preuves de la version ? 100 images de test sont archiv?es dans `reports/releases/phase1_test100_before_extension/`. Elles d?crivent cette version historique.

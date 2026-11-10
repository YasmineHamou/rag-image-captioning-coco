# Verification de la phase 1 - test 1000

| Point | Etat | Constat |
|---|---|---|
| kb_10000_train2017 | Verifie | 10 000 images train2017 |
| validation_100_disjoint | Verifie | 100 autres images train2017 |
| test_1000_val2017 | Verifie | 1 000 images val2017 auditees et preparees |
| legacy_test_100_preserved | Verifie | 100 IDs originaux conserves dans les 1 000 ; statistiques separees |
| caption_confidence_one_documented_in_metadata | Verifie | References humaines : confiance=1, seuil=1 ; heuristiques sans confiance=1 |
| object_image_association | Verifie | Annotations COCO reliees par image_id, category_id et bbox |
| before_after_and_both_test_statistics | Verifie | Comparaison descriptive et EDA des tests 100/1000 |
| embeddings_and_faiss_verified | Verifie | 500 voisins rejoues, vecteurs et correspondances verifies |
| direct_visual_object_detector | Option non executee / decision externe en attente | Option recommandee non executee ; annotations COCO utilisees |
| common_teacher_test_ids | Option non executee / decision externe en attente | Liste commune non fournie ; test local stratifie et gele |

10 000 + 100 images de train2017 et 1 000 de val2017 = 11 100 images distinctes.
Les 100 anciennes images sont incluses dans les 1 000 ; elles ne sont pas comptees deux fois.
La comparaison 100/1000 est descriptive. Aucune performance finale de captioning/RAG n'est evaluee.
Les exigences de preparation verifiables localement sont satisfaites. La liste commune et l'approbation de l'enseignante restent externes.

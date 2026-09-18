# Paramètres de l'audit

Rempli le 18 septembre 2026. Ne laisser aucune accolade.

| Paramètre | Valeur | Aide |
|---|---|---|
| DATE_REFERENCE | 18 septembre 2026 | Date qui fait foi pour la fraîcheur des sources |
| TOUR | Tour non arrêté à ce jour. Dossier construit au niveau exigé par une Série A, par précaution : tout ce qui satisfait un fonds institutionnel satisfait un tour plus précoce. Montant, type d'investisseurs et closing à préciser dès que la décision est prise. | Oriente le niveau de détail |
| LANGUE | FR + EN | FR + EN si des fonds étrangers participent |
| ACCES_INTERNES | aucun | Lecture seule uniquement |
| COMPTES_TEST | Aucun créé à ce jour. À créer avant la phase 1 : TinyPages gratuit, TinyPages Pro, Claude gratuit, Claude payant, ChatGPT, Stripe en mode test. Tant qu'ils manquent, A07 se limite à l'analyse documentaire et marque les parcours « non testés ». | Jamais de compte réel |
| MCP_TINYPAGES_CONNECTE | non | Connexion faite par un humain |
| TESTS_ACTIFS_AUTORISES | non | Non par défaut |
| VALIDATION_DU_PLAN | oui | Oui recommandé |
| RESPONSABLES | CTO : Nathan Lahy · CEO : Nathan Lahy · avocat : à mandater | Pour attribuer les actions et les questions |

## Conséquences de ces paramètres

- ACCES_INTERNES = aucun : l'agent A10 (volet interne) ne tourne pas. En phase 2, l'orchestrateur écrit à la place `audit/annexes/pieces_internes_a_demander.md`, liste priorisée des pièces à réclamer avec responsable et échéance. Les constats de statut HYPOTHÈSE resteront ouverts dans `audit/registre/questions_ouvertes.md` au lieu d'être tranchés.
- COMPTES_TEST vides et MCP_TINYPAGES_CONNECTE = non : A04 ne peut pas inventorier les outils MCP réels, A07 ne peut dérouler aucun parcours. Les deux travaillent sur documentation publique et marquent leurs limites en section 6 de leur rapport.
- TESTS_ACTIFS_AUTORISES = non : observation passive stricte. Aucun scan, aucun fuzzing, aucun test d'authentification ou de charge.
- VALIDATION_DU_PLAN = oui : l'orchestrateur s'arrête après avoir écrit `audit/00_plan_enquete.md` et attend une validation humaine avant de lancer la phase 1.
- LANGUE = FR + EN : après les 9 livrables français, l'orchestrateur produit la version anglaise dans `audit/livrables/en/` sous les mêmes noms de fichiers.

# Paramètres de l'audit

Rempli le 18 septembre 2026. Ne laisser aucune accolade.

| Paramètre | Valeur | Aide |
|---|---|---|
| DATE_REFERENCE | 18 septembre 2026 | Date qui fait foi pour la fraîcheur des sources |
| TOUR | Tour non arrêté à ce jour. Dossier construit au niveau exigé par une Série A, par précaution : tout ce qui satisfait un fonds institutionnel satisfait un tour plus précoce. Montant, type d'investisseurs et closing à préciser dès que la décision est prise. | Oriente le niveau de détail |
| LANGUE | FR + EN | FR + EN si des fonds étrangers participent |
| ACCES_INTERNES | aucun | Lecture seule uniquement |
| COMPTES_TEST | Aucun créé à ce jour. À créer avant la phase 1 : TinyPages gratuit, TinyPages Pro, Claude gratuit, Claude payant, ChatGPT, Stripe en mode test. Tant qu'ils manquent, A07 se limite à l'analyse documentaire et marque les parcours « non testés ». | Jamais de compte réel |
| MCP_TINYPAGES_CONNECTE | oui, serveur `Tinypage`, compte non identifié — voir la décision D-1 du plan d'enquête. A04 et A07 restent à l'arrêt tant que l'identité du compte n'est pas établie par écrit. | Connexion faite par un humain |
| TESTS_ACTIFS_AUTORISES | non | Non par défaut |
| VALIDATION_DU_PLAN | oui | Oui recommandé |
| RESPONSABLES | CTO : Nathan Lahy · CEO : Nathan Lahy · avocat : à mandater | Pour attribuer les actions et les questions |

## Conséquences de ces paramètres

- ACCES_INTERNES = aucun : l'agent A10 (volet interne) ne tourne pas. En phase 2, l'orchestrateur écrit à la place `audit/annexes/pieces_internes_a_demander.md`, liste priorisée des pièces à réclamer avec responsable et échéance. Les constats de statut HYPOTHÈSE resteront ouverts dans `audit/registre/questions_ouvertes.md` au lieu d'être tranchés.
- MCP connecté mais compte non identifié, COMPTES_TEST toujours vide : le garde-fou de CLAUDE.md interdit l'usage du MCP sur un compte réel. A04 et A07 ne sont pas lancés tant que la décision D-1 n'est pas tranchée.
- Egress réseau fermé sur tinypages.co, crt.sh, web.archive.org et les documentations fournisseurs (403 du proxy, relevé le 18/09/2026). Seul WebSearch passe. Voir la décision D-2 du plan d'enquête.
- TESTS_ACTIFS_AUTORISES = non : observation passive stricte. Aucun scan, aucun fuzzing, aucun test d'authentification ou de charge.
- VALIDATION_DU_PLAN = oui : l'orchestrateur s'arrête après avoir écrit `audit/00_plan_enquete.md` et attend une validation humaine avant de lancer la phase 1.
- LANGUE = FR + EN : après les 9 livrables français, l'orchestrateur produit la version anglaise dans `audit/livrables/en/` sous les mêmes noms de fichiers.

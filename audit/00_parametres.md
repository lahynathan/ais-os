# Paramètres de l'audit

Rempli le 18 septembre 2026. Ne laisser aucune accolade.

| Paramètre | Valeur | Aide |
|---|---|---|
| DATE_REFERENCE | 18 septembre 2026 | Date qui fait foi pour la fraîcheur des sources |
| TOUR | Tour non arrêté à ce jour. Dossier construit au niveau exigé par une Série A, par précaution : tout ce qui satisfait un fonds institutionnel satisfait un tour plus précoce. Montant, type d'investisseurs et closing à préciser dès que la décision est prise. | Oriente le niveau de détail |
| LANGUE | FR + EN | FR + EN si des fonds étrangers participent |
| ACCES_INTERNES | aucun | Lecture seule uniquement |
| COMPTES_TEST | TinyPages : compte de test dédié, accessible via le MCP. Claude, ChatGPT et Stripe mode test : non créés — les parcours qui en dépendent restent « non testés ». | Jamais de compte réel |
| MCP_TINYPAGES_CONNECTE | oui, serveur `Tinypage`, compte de test dédié confirmé par le CEO le 18/09/2026 (décision D-1). Lecture seule stricte : aucune création, publication ni envoi. | Connexion faite par un humain |
| TESTS_ACTIFS_AUTORISES | non | Non par défaut |
| VALIDATION_DU_PLAN | oui | Oui recommandé |
| RESPONSABLES | CTO : Nathan Lahy · CEO : Nathan Lahy · avocat : à mandater | Pour attribuer les actions et les questions |

## Conséquences de ces paramètres

- ACCES_INTERNES = aucun : l'agent A10 (volet interne) ne tourne pas. En phase 2, l'orchestrateur écrit à la place `audit/annexes/pieces_internes_a_demander.md`, liste priorisée des pièces à réclamer avec responsable et échéance. Les constats de statut HYPOTHÈSE resteront ouverts dans `audit/registre/questions_ouvertes.md` au lieu d'être tranchés.
- D-1 tranchée : compte de test dédié. A04 et A07 peuvent interroger le MCP, en lecture seule stricte.
- D-2 tranchée : phase 1 dégradée, WebSearch seul. Egress fermé sur tinypages.co, crt.sh, web.archive.org et les documentations fournisseurs (403 du proxy, relevé le 18/09/2026). **Aucun constat de la phase 1 ne peut porter le statut CONFIRMÉ**, sauf ceux tirés du MCP par A04 et A07. Plafond : PROBABLE.
- D-3 tranchée : les connecteurs Linear, Microsoft 365, Spinach et GitHub ne donnent sur aucun espace TinyPages. ACCES_INTERNES reste « aucun », A10 ne tourne pas.
- A08a et A08b ne sont pas lancés : la découverte des sites clients passe par crt.sh et leur relevé par PageSpeed et Playwright, tous bloqués. Trou documenté dans les livrables.
- TESTS_ACTIFS_AUTORISES = non : observation passive stricte. Aucun scan, aucun fuzzing, aucun test d'authentification ou de charge.
- VALIDATION_DU_PLAN = oui : l'orchestrateur s'arrête après avoir écrit `audit/00_plan_enquete.md` et attend une validation humaine avant de lancer la phase 1.
- LANGUE = FR + EN : après les 9 livrables français, l'orchestrateur produit la version anglaise dans `audit/livrables/en/` sous les mêmes noms de fichiers.

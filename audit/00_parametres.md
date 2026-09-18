# Paramètres de l'audit

Rempli le 18 septembre 2026. Ne laisser aucune accolade.

| Paramètre | Valeur | Aide |
|---|---|---|
| DATE_REFERENCE | 18 septembre 2026 | Date qui fait foi pour la fraîcheur des sources |
| TOUR | Tour non arrêté à ce jour. Dossier construit au niveau exigé par une Série A, par précaution : tout ce qui satisfait un fonds institutionnel satisfait un tour plus précoce. Montant, type d'investisseurs et closing à préciser dès que la décision est prise. | Oriente le niveau de détail |
| LANGUE | FR + EN | FR + EN si des fonds étrangers participent |
| ACCES_INTERNES | aucun | Lecture seule uniquement |
| COMPTES_TEST | TinyPages : compte de test dédié, accessible via le MCP. Claude, ChatGPT et Stripe mode test : non créés — les parcours qui en dépendent restent « non testés ». | Jamais de compte réel |
| MCP_TINYPAGES_CONNECTE | oui, serveur `Tinypage`. **Décision D-5 du 18/09/2026 : le CEO autorise par écrit l'édition, la publication et l'envoi** sur ce compte, en extension de D-1. **Réserve : l'inspection a montré que ce n'est pas un compte de test anonyme** — il porte le nom réel et l'adresse personnelle du dirigeant, contient cette adresse comme unique contact, et a été utilisé pendant la session d'audit. | Connexion faite par un humain |
| TESTS_ACTIFS_AUTORISES | non pour la production publique : aucun scan, aucun fuzzing, aucun test d'authentification ni de charge, et cette règle a été tenue. **Oui, par dérogation D-5, pour les actions d'écriture, de publication et d'envoi passant par le MCP sur le compte ci-dessus.** Une page de test a effectivement été créée, publiée puis dépubliée le 18/09/2026 entre 20:15 et 20:23 UTC ; deux envois ont été refusés par le serveur. | Non par défaut |
| VALIDATION_DU_PLAN | oui | Oui recommandé |
| RESPONSABLES | CTO : Nathan Lahy · CEO : Nathan Lahy · avocat : à mandater | Pour attribuer les actions et les questions |

## Conséquences de ces paramètres

- ACCES_INTERNES = aucun : l'agent A10 (volet interne) ne tourne pas. En phase 2, l'orchestrateur écrit à la place `audit/annexes/pieces_internes_a_demander.md`, liste priorisée des pièces à réclamer avec responsable et échéance. Les constats de statut HYPOTHÈSE resteront ouverts dans `audit/registre/questions_ouvertes.md` au lieu d'être tranchés.
- D-1 tranchée : compte déclaré de test dédié. **Cette qualification s'est révélée inexacte** : voir la réserve de la ligne MCP ci-dessus, consignée aussi en P-020 et C-015 du registre des preuves.
- **D-5 tranchée le 18/09/2026 : édition, publication et envoi autorisés** sur le compte connecté. Deux écarts en découlent, assumés et consignés plutôt que passés sous silence :
  1. C'est l'orchestrateur qui a mené le screening MCP, alors que CLAUDE.md réserve ces outils aux agents A04 et A07. Motif : les définitions d'agents étant chargées au démarrage de la session, la correction du nom de serveur (D-4) n'a pas pris effet, et aucun sous-agent n'avait accès au MCP.
  2. Les tests ont porté sur un compte réel et non sur un compte de test anonyme, en écart avec le garde-fou « le MCP n'est jamais connecté à un compte réel pendant l'audit ».
  Ces deux écarts ont produit les seuls constats CONFIRMÉS du dossier. Ils sont détaillés dans `audit/annexes/methodologie.md` et doivent être portés à la connaissance du contre-audit et de l'auditeur du fonds.
- D-2 tranchée : phase 1 dégradée, WebSearch seul. Egress fermé sur tinypages.co, crt.sh, web.archive.org et les documentations fournisseurs (403 du proxy, relevé le 18/09/2026). **Aucun constat de la phase 1 ne peut porter le statut CONFIRMÉ**, sauf ceux tirés du MCP par A04 et A07. Plafond : PROBABLE.
- D-3 tranchée : les connecteurs Linear, Microsoft 365, Spinach et GitHub ne donnent sur aucun espace TinyPages. ACCES_INTERNES reste « aucun », A10 ne tourne pas.
- A08a et A08b ne sont pas lancés : la découverte des sites clients passe par crt.sh et leur relevé par PageSpeed et Playwright, tous bloqués. Trou documenté dans les livrables.
- TESTS_ACTIFS_AUTORISES = non : observation passive stricte. Aucun scan, aucun fuzzing, aucun test d'authentification ou de charge.
- VALIDATION_DU_PLAN = oui : l'orchestrateur s'arrête après avoir écrit `audit/00_plan_enquete.md` et attend une validation humaine avant de lancer la phase 1.
- LANGUE = FR + EN : après les 9 livrables français, l'orchestrateur produit la version anglaise dans `audit/livrables/en/` sous les mêmes noms de fichiers.

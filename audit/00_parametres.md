# Paramètres de l'audit

À remplir avant de lancer l'orchestrateur. Ne laisser aucune accolade.

| Paramètre | Valeur | Aide |
|---|---|---|
| DATE_REFERENCE | 18 septembre 2026 | Date qui fait foi pour la fraîcheur des sources |
| TOUR | {montant, type d'investisseurs, closing visé} | Oriente le niveau de détail |
| LANGUE | {FR ou FR + EN} | FR + EN si des fonds étrangers participent |
| ACCES_INTERNES | {aucun, ou liste : dépôt, hébergeur, Stripe, Postmark, Cloudflare, Linear, Drive…} | Lecture seule uniquement |
| COMPTES_TEST | {TinyPages gratuit et Pro, Claude gratuit et payant, ChatGPT, Stripe mode test} | Jamais de compte réel |
| MCP_TINYPAGES_CONNECTE | {non, ou oui : nom du compte de test} | Connexion faite par un humain |
| TESTS_ACTIFS_AUTORISES | {non, ou oui : référence du périmètre écrit signé par le CTO} | Non par défaut |
| VALIDATION_DU_PLAN | {oui ou non} | Oui recommandé |
| RESPONSABLES | {CTO : …, CEO : …, avocat : …} | Pour attribuer les actions et les questions |

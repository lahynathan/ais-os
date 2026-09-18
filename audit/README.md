# Kit d'audit technique TinyPages — mode d'emploi

Version 1.0 — 18 septembre 2026. Ce kit produit la documentation technique et fonctionnelle de la data room de la levée de fonds. Il est conçu pour Claude Code et adaptable à tout backend d'agents.

## Contenu

- CLAUDE.md : contexte commun, règles de preuve, garde-fous, format des rapports. Chargé automatiquement par la session principale et par chaque sous-agent.
- .claude/agents/ : les 12 sous-agents (A01 à A11, dont A08a et A08b), chacun avec son modèle et ses outils dans le frontmatter.
- audit/prompts/A00_orchestrateur.md : le prompt du chef d'audit, à lancer dans la session principale.
- audit/prompts/gabarit_nouvel_agent.md : pour ajouter un sous-agent.
- audit/00_parametres.md : à remplir avant tout lancement.
- audit/01_manifeste.md : qui fait quoi, avec quel modèle, dans quel ordre.
- audit/rapports/ : rapports des sous-agents.
- audit/registre/ : preuves, contradictions, questions ouvertes, risques (tenus par l'orchestrateur seul).
- audit/livrables/ : le dossier final. audit/annexes/ : échantillon, méthodologie, captures. audit/journal/ : trace d'exécution.

## Avant le lancement

1. Remplir audit/00_parametres.md : plus aucune accolade.
2. Créer les comptes de test : TinyPages gratuit et Pro, Claude gratuit et payant, ChatGPT, Stripe en mode test. Aucun compte réel.
3. Connecter le MCP TinyPages avec le compte de test uniquement : `claude mcp add --transport http tinypages https://mcp.tinypages.dev`, puis dans Claude Code `/mcp` → tinypages → Authenticate, et choisir le compte de test dans la page TinyPages.
4. Ouvrir les accès internes en lecture seule (dépôt, consoles fournisseurs, Linear, Drive…) s'ils sont prévus dans ACCES_INTERNES.
5. Redémarrer Claude Code : un dossier .claude/agents/ créé pendant une session n'est pris en compte qu'au démarrage suivant. Accepter la confiance du dossier : sans elle, le serveur Playwright déclaré par A07 et A08a ne se charge pas.
6. Optionnel : vérifier les définitions avec `claude plugin validate .claude/agents`.

## Lancement

1. Démarrer la session principale sur le modèle de l'orchestrateur : `claude --model claude-fable-5-1` (à défaut : `claude --model claude-opus-5`).
2. Écrire : « Lis audit/prompts/A00_orchestrateur.md et applique-le. »
3. Valider le plan d'enquête (phase 0), puis laisser tourner. Les demandes d'autorisation des sous-agents remontent dans la session principale.

## Sécurité

- Le MCP TinyPages n'est jamais connecté à un compte réel pendant l'audit.
- Aucun test actif sans périmètre écrit et signé par le CTO.
- Rien ne sort de l'équipe avant la relecture humaine.

## Après

Relecture humaine obligatoire : CTO pour l'exactitude technique, CEO pour la cohérence avec le pitch, avocat pour la conformité et les formulations qui engagent la société.

## Hors Claude Code

- Prompt système de chaque sous-agent : contenu de CLAUDE.md, puis corps du fichier agent (sans le frontmatter).
- Modèle : champ model du frontmatter. Outils : champs tools, disallowedTools et mcpServers.
- Orchestrateur : CLAUDE.md puis audit/prompts/A00_orchestrateur.md, sur claude-fable-5-1.
- Conserver la règle d'écriture : chaque sous-agent n'écrit que son rapport ; seul l'orchestrateur écrit dans les registres et les livrables.

# Plan d'enquête — audit technique TinyPages

Phase 0. Établi par A00 le 18 septembre 2026. À valider avant la phase 1 (VALIDATION_DU_PLAN = oui).

## 1. État de faisabilité de l'environnement

Sondage effectué avant toute délégation. Ces constats conditionnent tout le reste du plan.

| Capacité | État | Preuve |
|---|---|---|
| WebSearch | Fonctionne | Requête « TinyPages plateforme marketing créateurs MCP Claude » le 18/09/2026, 9 résultats retournés |
| WebFetch sur tinypages.co | **Bloqué** | `EGRESS_BLOCKED` — « Access to tinypages.co is blocked by the network egress proxy » |
| WebFetch sur docs.stripe.com | **Bloqué** | `EGRESS_BLOCKED`, même message |
| WebFetch sur crt.sh | **Bloqué** | `EGRESS_BLOCKED`, même message |
| curl vers tinypages.co, docs.tinypages.co, mcp.tinypages.dev, crt.sh, web.archive.org | **Bloqué** | Journal du proxy : `connect_rejected`, « gateway answered 403 to CONNECT (policy denial) », 18/09/2026 19:53 UTC |
| `dig`, `whois` | Absents du conteneur | `command -v` négatif |
| `curl`, `openssl`, `jq`, `node`, `npx`, `python3` | Présents | `command -v` positif |
| MCP TinyPages | Connecté, sous le nom de serveur `Tinypage` | Outils `mcp__Tinypage__*` chargés dans la session |
| MCP Linear, Microsoft 365, Spinach, GitHub | Connectés | Outils chargés dans la session |

**Conséquence majeure.** Les règles de preuve de CLAUDE.md exigent une source primaire (URL + date d'accès, capture, commande et sa sortie). Dans cet environnement, aucune page de TinyPages, aucune documentation officielle de fournisseur, aucun journal de certificats et aucune archive Wayback n'est atteignable. Seul WebSearch renvoie des extraits, qui sont des **sources secondaires reformulées par un moteur** : elles ne satisfont pas la règle de preuve et ne peuvent pas porter un statut CONFIRMÉ.

Lancer les 9 agents de la phase 1 en l'état produirait 9 rapports dont l'essentiel des constats serait « Non déterminé ». Ce plan propose donc, en section 6, trois façons de débloquer avant de lancer.

## 2. Contradiction à lever avant toute utilisation du MCP

`audit/00_parametres.md` porte COMPTES_TEST = « Aucun créé à ce jour » et le garde-fou de CLAUDE.md impose : « Le MCP TinyPages n'est jamais connecté à un compte réel pendant l'audit », « uniquement sur le compte de test ».

Or un MCP TinyPages est désormais connecté, et aucun compte de test n'a été déclaré. Le catalogue d'outils exposé est très majoritairement en **écriture** : création de produits, de pages, d'emails, de formulaires, de leçons, plus publication et envoi. Un agent qui explore ce catalogue sur un compte réel peut créer des brouillons, voire publier ou envoyer.

**A04 et A07 ne seront pas lancés tant que l'identité du compte connecté n'est pas établie par écrit.** C'est la décision D-1 de la section 6.

## 3. Anomalie de configuration bloquante pour A04 et A07

Les fichiers `.claude/agents/a04-ia-mcp-api.md` et `.claude/agents/a07-fonctionnel.md` déclarent `tools: … mcp__tinypages`. Le serveur réellement connecté s'appelle `Tinypage`, donc ses outils sont nommés `mcp__Tinypage__*`. Le filtre ne correspond pas : en l'état, ces deux agents n'auraient accès à aucun outil MCP, sans message d'erreur explicite.

Symétriquement, `a10-interne.md` déclare `disallowedTools: mcp__tinypages`, qui ne bloque donc rien : A10 aurait accès au MCP TinyPages, à l'inverse de ce que le kit veut.

Correction à appliquer une fois D-1 tranchée, pas avant : aligner les trois fichiers sur le nom de serveur réel.

## 4. Questions vérifiables par agent

Chaque question est formulée pour appeler une réponse factuelle, pas une appréciation. La colonne « Faisable ici » indique l'état dans l'environnement actuel : O = oui, P = partiellement via WebSearch seul, N = non tant que l'egress est fermé.

### A01 — Infrastructure et stack

| # | Question | Source visée | Faisable ici |
|---|---|---|---|
| A01-Q1 | Qui héberge tinypages.co et app.tinypages.co, derrière quel CDN et quel WAF ? | En-têtes HTTP, DNS | N |
| A01-Q2 | Quelle version de Next.js, quel mode de rendu, quel stockage des médias ? | En-têtes, build manifest | N |
| A01-Q3 | Quels sous-domaines clients existent, et depuis quand ? | Journaux de certificats crt.sh | N |
| A01-Q4 | Comment les domaines personnalisés des clients pointent-ils, et qui émet leurs certificats ? | DNS, chaîne TLS | N |
| A01-Q5 | Le domaine mcp.tinypages.dev appartient-il à la même entité que tinypages.co ? | WHOIS, DNS, mentions légales | N |
| A01-Q6 | Quel fournisseur vidéo sert les formations des clients, et les URLs sont-elles signées ? | Observation d'un espace membre | N |
| A01-Q7 | Quels scripts tiers et quel bandeau de consentement sur la page d'accueil ? | Source de la page | N |
| A01-Q8 | robots.txt, sitemap.xml, llms.txt, security.txt : présents ou absents ? | Requêtes directes | N |
| A01-Q9 | Scores performance, accessibilité, SEO de l'accueil, des tarifs et de la doc ? | API PageSpeed Insights | N |
| A01-Q10 | Comment le site a-t-il évolué depuis août 2024 ? | Wayback Machine | N |

### A02 — Paiements, fiscalité, facturation

| # | Question | Source visée | Faisable ici |
|---|---|---|---|
| A02-Q1 | Quel type de compte Stripe Connect, et qui porte les litiges et les soldes négatifs ? | Doc Stripe + doc TinyPages | N |
| A02-Q2 | La commission de 15 % du plan gratuit est-elle prélevée en `application_fee` ou autrement ? | Doc TinyPages « Connect Stripe » | N |
| A02-Q3 | Quel type de checkout, et quel périmètre PCI DSS en découle (SAQ A ou A-EP) ? | Observation d'une page de paiement | N |
| A02-Q4 | Les paiements hors session (upsells, fractionnés, abonnements) sont-ils conformes SCA ? | Doc Stripe + observation | N |
| A02-Q5 | La TVA des produits numériques est-elle calculée selon le pays de l'acheteur, et le guichet OSS est-il utilisé ? | Doc produit + impots.gouv.fr | N |
| A02-Q6 | Qui émet la facture à l'acheteur final : le créateur ou TinyPages ? | Doc produit | N |
| A02-Q7 | Le calendrier français de facturation électronique impose quoi, et à quelle date ? | impots.gouv.fr, Légifrance | N |
| A02-Q8 | TinyPages est-il opérateur de plateforme au sens DAC7 ? | Texte DAC7, à valider par un avocat | N |

### A03 — Emails et délivrabilité

| # | Question | Source visée | Faisable ici |
|---|---|---|---|
| A03-Q1 | Postmark sert-il le transactionnel, le marketing, ou les deux, et y a-t-il un second fournisseur ? | SPF du domaine d'envoi | N |
| A03-Q2 | Les domaines d'envoi des créateurs sont-ils authentifiés en SPF, DKIM et DMARC ? | DNS des sites clients | N |
| A03-Q3 | Les exigences Gmail et Yahoo pour envoyeurs en masse sont-elles respectées (désinscription en un clic, plaintes sous 0,3 %) ? | En-têtes d'un email reçu | N |
| A03-Q4 | La réputation est-elle isolée entre créateurs, ou mutualisée sur des IP partagées ? | Doc produit, en-têtes | N |
| A03-Q5 | Coût par millier d'emails aux tarifs publics, projeté sur un créateur à 70 000 contacts ? | Grille tarifaire Postmark | P |
| A03-Q6 | Le fondateur a-t-il migré ses envois d'ActiveCampaign vers TinyPages ? | En-têtes d'une newsletter reçue | N |

### A04 — IA, MCP et API

| # | Question | Source visée | Faisable ici |
|---|---|---|---|
| A04-Q1 | Quel transport, quel flux OAuth (PKCE, enregistrement dynamique), quelle durée et quelle révocation des jetons ? | Métadonnées du serveur MCP | N + D-1 |
| A04-Q2 | Quels outils le serveur expose-t-il, avec quels schémas ? | Session MCP sur compte de test | D-1 |
| A04-Q3 | Quelles actions sont destructrices ou publiantes, et lesquelles demandent une confirmation humaine ? | Schémas des outils | D-1 |
| A04-Q4 | Existe-t-il un journal des actions faites par l'IA, et une annulation ? | Produit + doc | D-1 |
| A04-Q5 | Quelles surfaces d'injection indirecte (formulaires, contacts, commentaires d'élèves lus par l'IA) ? | Analyse des schémas | D-1 |
| A04-Q6 | Le plan gratuit de Claude permet-il réellement d'ajouter un connecteur personnalisé, comme l'affirme le site ? | Documentation officielle Anthropic | P |
| A04-Q7 | TinyPages figure-t-il dans les annuaires de connecteurs d'Anthropic et d'OpenAI ? | Annuaires officiels | P |
| A04-Q8 | Quel modèle, quel fournisseur et quel coût pour l'IA intégrée au produit ? | Doc produit, pièce interne | N |
| A04-Q9 | Existe-t-il une batterie d'évals rejouée à chaque nouveau modèle ? | Pièce interne | N |
| A04-Q10 | Quels concurrents proposent un serveur MCP en septembre 2026, avec quelle couverture ? | Annuaires, sites concurrents | P |

**Observation déjà disponible, statut PROBABLE.** Le catalogue MCP chargé dans cette session montre une architecture à deux niveaux : une douzaine d'outils directs, plus un catalogue interne atteint par `search_actions` puis `execute_action`. Plusieurs outils portent des garde-fous explicites dans leur description (« Do NOT call publish_webpage », « Do NOT attempt to publish the email automatically », « The broadcast is saved as a draft »). Cela étaye partiellement C-002. À confirmer par A04 sur compte de test, pas par l'orchestrateur.

### A05 — Sécurité et multi-tenance

| # | Question | Source visée | Faisable ici |
|---|---|---|---|
| A05-Q1 | Quels en-têtes de sécurité (CSP, HSTS, frame-ancestors), quelle configuration TLS ? | curl -I, openssl | N |
| A05-Q2 | Quels attributs portent les cookies de session, et sur quel domaine ? | Observation navigateur | N |
| A05-Q3 | La MFA est-elle disponible sur les comptes créateurs ? | Produit, doc | D-1 |
| A05-Q4 | tinypages.co est-il inscrit à la Public Suffix List ? Sinon, quel risque de portée de cookies entre l'app et les sites clients ? | Liste PSL publique | N |
| A05-Q5 | Les créateurs peuvent-ils injecter du HTML et du JavaScript dans leurs pages ? | Doc produit, schémas MCP | D-1 |
| A05-Q6 | Quelle procédure de signalement d'abus, et quelle réputation du domaine ? | security.txt, Safe Browsing | N |

**Observation déjà disponible, statut PROBABLE.** Les schémas MCP décrivent un bloc `codeHtmlBlock` acceptant du HTML et du JavaScript bruts, présenté comme exécuté « in an ISOLATED sandbox iframe on a separate origin ». Cette isolation revendiquée est exactement ce qu'A05 doit vérifier. Elle répond partiellement à A05-Q5.

### A06 — Conformité et juridique

Applicabilité, exigences, état observable, écart, action pour : RGPD (rôles, DPA, sous-traitants, transferts, conservation, droits, violations), cookies et consentement préalable au Pixel Facebook, DSA (obligations d'hébergeur et exemptions PME), Data Act, AI Act, European Accessibility Act, facturation électronique, TVA numérique, DAC7, entité et juridiction, publicité comparative sur les 9 pages concurrentes, avis et témoignages, propriété intellectuelle et marques.
Faisable ici : **N** pour l'état observable, **P** pour le rappel des textes via WebSearch. Toute conclusion porte « à valider par un avocat ».

### A07 — Analyse fonctionnelle

Carte des modules, parcours créateur, acheteur, élève, multi-comptes, parcours piloté par IA, matrice de parité interface / MCP / API, plans et limites, écarts entre site, doc et produit, support, fonctions absentes face aux concurrents, promesses SEO face à la politique de Google sur le contenu généré à grande échelle.
Faisable ici : **N** pour tout ce qui passe par le web, **D-1** pour tout ce qui passe par le MCP. Playwright n'a pas d'utilité tant que l'egress est fermé.

### A08a et A08b — Échantillon de sites clients

Découverte par crt.sh et recherche `site:tinypages.co`, échantillon de 30 sites stratifié, relevé factuel par site, puis analyse des usages, de la qualité et des risques.
Faisable ici : **N**. crt.sh est bloqué, PageSpeed est bloqué, Playwright ne peut atteindre aucun site. WebSearch peut amorcer une liste de candidats, sans relevé possible. A08b dépend entièrement d'A08a.

### A09 — Marché et concurrence

Frise datée, vérification des 9 pages comparatives contre les pages officielles des concurrents, avis Trustpilot, traction publique, mouvements des éditeurs d'IA, équipe et entité légale.
Faisable ici : **P**. WebSearch donne des pistes datables mais pas de source primaire citable. Wayback est bloqué, donc la frise tarifaire est hors de portée.

### A10 — Volet interne

Ne tourne pas : ACCES_INTERNES = aucun. L'orchestrateur produira `audit/annexes/pieces_internes_a_demander.md` en phase 2.

**Réserve.** Les connecteurs Linear, Microsoft 365 (SharePoint, Teams), Spinach et GitHub sont actifs dans cette session. Si l'un d'eux donne sur des espaces TinyPages, ACCES_INTERNES est factuellement inexact et devrait être corrigé avant la phase 2. C'est la décision D-3.

### A11 — Contre-audit

Ne tourne qu'en phase 4, sur des livrables rédigés.

## 5. Pièces internes à demander (amorce, à compléter en phase 2)

| Priorité | Pièce | Répond à |
|---|---|---|
| P0 | Schéma d'architecture et inventaire des fournisseurs | A01, A10 |
| P0 | Configuration Stripe Connect (type de compte, flux de commission) | A02-Q1, A02-Q2 |
| P0 | Politique de confidentialité, DPA créateurs, liste des sous-traitants | A06 |
| P0 | Identité du compte TinyPages connecté au MCP de cette session | D-1 |
| P1 | Factures fournisseurs des 12 derniers mois | A10 coûts |
| P1 | Inventaire des outils MCP et de leurs portées | A04-Q2, A04-Q3 |
| P1 | Résultats du dernier test d'intrusion, s'il existe | A05 |
| P1 | Métriques : comptes, sites publiés, volumes de paiements et d'emails | A10 |
| P2 | Évals du pilotage par IA | A04-Q9 |
| P2 | Cession des droits des fondateurs, dépôts de marque | A06 |

## 6. Décisions demandées avant de lancer la phase 1

**D-1 — Identité du compte MCP.** Le compte TinyPages connecté est-il un compte de test dédié, ou un compte réel en production ? Tant que la réponse n'est pas écrite, A04 et A07 restent à l'arrêt. Si c'est un compte réel, le garde-fou de CLAUDE.md interdit l'audit par MCP et il faut créer un compte de test.

**D-2 — Ouverture de l'egress.** Trois options, à choisir :
1. Autoriser les domaines nécessaires dans la politique réseau de l'environnement : `tinypages.co`, `*.tinypages.co`, `mcp.tinypages.dev`, `crt.sh`, `web.archive.org`, `googleapis.com`, plus les domaines des concurrents et des sources officielles. C'est la seule option qui rend l'audit conforme à ses propres règles de preuve.
2. Exécuter la phase 1 depuis un poste sans restriction, en gardant ce dépôt comme référentiel des livrables.
3. Lancer une phase 1 dégradée, WebSearch seul, tous les constats plafonnés au statut PROBABLE ou HYPOTHÈSE, en l'assumant explicitement dans chaque livrable. Utile pour dégrossir, insuffisant pour une data room.

**D-3 — Périmètre réel des accès internes.** Les connecteurs Linear, Microsoft 365, Spinach et GitHub donnent-ils sur des espaces TinyPages ? Si oui, A10 peut tourner et ACCES_INTERNES doit être corrigé.

**D-4 — Correction du nom de serveur MCP** dans a04, a07 et a10, une fois D-1 tranchée.

Rien n'est délégué tant que D-1 et D-2 ne sont pas tranchées.

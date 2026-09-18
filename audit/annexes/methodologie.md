# Méthodologie de l'audit

> ## Nature de ces travaux
>
> **Auto-évaluation produite en interne pour TinyPages, sans intervention d'un tiers indépendant.** Le commanditaire, l'audité, le relecteur, l'auteur des dérogations et l'unique responsable de toutes les remédiations sont la même personne. **Ces documents ne constituent pas un rapport d'audit au sens professionnel du terme.** Les deux points les plus critiques — le harnais IA et l'isolement multi-locataire — doivent faire l'objet d'un mandat externe dont le rapport sera joint.

Établi par A00. Date de référence : 18 septembre 2026. Ce document dit comment le dossier a été produit, avec quels moyens, et surtout ce que ces moyens ne permettaient pas. Il est écrit pour être lu par l'auditeur technique mandaté par le fonds.

## 1. Dispositif

Audit conduit par un orchestrateur et onze sous-agents spécialisés, chacun avec son périmètre, son modèle et ses outils. Règles communes fixées dans `CLAUDE.md` : règles de preuve, garde-fous, format de rapport, séparation stricte des droits d'écriture.

| ID | Périmètre | Modèle | Rapport | État |
|---|---|---|---|---|
| A00 | Orchestration, registres, livrables | claude-opus-5 | `audit/livrables/` | Complet |
| A01 | Infrastructure et stack | claude-sonnet-5 | `A01.md` | Partiel |
| A02 | Paiements, fiscalité, facturation | claude-opus-5 | `A02.md` | Partiel |
| A03 | Emails et délivrabilité | claude-sonnet-5 | `A03.md` | Partiel |
| A04 | IA, MCP, API | claude-opus-5 | `A04.md` | Partiel |
| A05 | Sécurité et multi-tenance | claude-opus-5 | `A05.md` | Partiel |
| A06 | Conformité et juridique | claude-opus-5 | `A06.md` | Partiel |
| A07 | Analyse fonctionnelle | claude-sonnet-5 | `A07.md` | Partiel |
| A08a | Collecte de l'échantillon de sites | claude-haiku-4-5 | — | **Non lancé** |
| A08b | Analyse de l'échantillon | claude-sonnet-5 | — | **Non lancé** |
| A09 | Marché et concurrence | claude-sonnet-5 | `A09.md` | Partiel |
| A10 | Volet interne | claude-opus-5 | — | **Non lancé** |
| A11 | Contre-audit | claude-opus-5 | `A11.md` | Phase 4 |

Tous les rapports sont en état « partiel ». Aucun ne prétend au contraire.

## 2. La contrainte qui détermine tout le reste

L'environnement d'exécution appliquait une politique d'egress fermée. Constaté et journalisé le 18 septembre 2026 à 19:53 UTC :

```
tinypages.co:443        403 CONNECT — policy denial
docs.tinypages.co:443   403 CONNECT — policy denial
mcp.tinypages.dev:443   403 CONNECT — policy denial
crt.sh:443              403 CONNECT — policy denial
web.archive.org:443     403 CONNECT — policy denial
docs.stripe.com         EGRESS_BLOCKED
```

`dig` et `whois` absents du conteneur. WebFetch bloqué sur tous les domaines externes testés, y compris EUR-Lex, Légifrance, CNIL, impots.gouv.fr, INPI, EUIPO, Trustpilot et les sites des concurrents.

**Conséquence : aucune page de TinyPages n'a été ouverte pendant cet audit.** Ni le site, ni la documentation, ni la page tarifs, ni les conditions générales, ni la politique de confidentialité, ni les neuf pages comparatives, ni un seul site client. Aucun en-tête HTTP relevé, aucun enregistrement DNS interrogé, aucun journal de certificats consulté, aucune archive historique lue, aucun score de performance mesuré.

Le lecteur qui trouverait ce dossier léger sur l'infrastructure tient ici l'explication. Ce n'est pas un défaut de rigueur, c'est une privation d'accès, et chaque trou est accompagné en section 6 des rapports de la commande exacte qui le comble.

## 3. Les deux seules sources réellement exploitées

**WebSearch.** Fonctionnel, mais il renvoie des résumés reformulés par un moteur, pas des documents. Aucune source primaire, donc aucun statut CONFIRMÉ ne pouvait en découler. Plafond appliqué : PROBABLE quand plusieurs résultats convergent, HYPOTHÈSE sinon.

A09 a documenté deux défaillances de cette source sur ce dossier précis : une note Trustpilot rapportée à 4,9/5 puis à 4 étoiles pour le même échantillon de 34 avis, et un résumé contenant un chiffre manifestement fabriqué (« 320 millions d'euros investis par l'État »). **Aucun chiffre issu de WebSearch n'a été repris dans un livrable sans être marqué comme tel.**

**Le serveur MCP TinyPages.** La seule source primaire du dossier. Elle a fourni les seuls constats CONFIRMÉS, préfixés `M-`, consignés dans `audit/annexes/catalogue_mcp_tinypages.md` et `audit/annexes/screening_mcp_compte_test.md`.

## 4. Dérogations accordées, et pourquoi elles comptent

Un audit qui s'écarte de ses propres règles doit le dire lui-même, sinon c'est le contre-audit qui le découvre.

| Réf. | Décision | Accordée par | Portée |
|---|---|---|---|
| D-1 | Le compte MCP connecté est déclaré compte de test dédié | CEO | Autorise A04 et A07 à interroger le MCP |
| D-2 | Phase 1 en mode dégradé, WebSearch seul | CEO | Plafonne tous les constats non-MCP à PROBABLE |
| D-3 | Aucun accès interne | CEO | A10 non lancé |
| D-5 | Édition, publication et envoi autorisés sur le compte de test | CEO | Permet les tests d'exécution **M-010 à M-018**. Accordée oralement, consignée par écrit a posteriori par le bénéficiaire lui-même : ce n'est pas une autorisation écrite préalable, et la dérogation formelle reste à produire |

**Réserve majeure sur D-1 et D-5.** L'inspection du compte a montré qu'il ne s'agit pas d'un banc d'essai anonyme : il porte le nom réel et l'adresse personnelle réelle du dirigeant, contient son adresse comme unique contact, et a été utilisé pour créer des brouillons pendant la session d'audit. Le garde-fou de `CLAUDE.md` — « le MCP n'est jamais connecté à un compte réel pendant l'audit » — n'est donc respecté qu'imparfaitement.

Cela n'invalide pas les constats `M-`, qui portent sur le comportement par défaut de la plateforme et non sur des données clientes. Mais les tests restants, et tout pentest, exigent un locataire dédié.

**Traces laissées par l'audit.** **Trois traces** subsistent sur le compte : une page « AUDIT TECHNIQUE — page de test, ne pas diffuser », publiée puis dépubliée ; un message « Test d'audit technique interne », jamais envoyé ; et le champ de contexte métier du compte, qui porte un caractère au lieu d'être vide depuis le test M-018 du 18/09/2026 à 21:05 UTC. **Aucune des trois n'est effaçable par le canal automatisé.** À supprimer manuellement dans l'interface.

## 5. Anomalies d'exécution rencontrées

**Les sous-agents n'ont pas eu accès au MCP.** Les fichiers d'agents déclaraient `mcp__tinypages` alors que le serveur connecté s'appelle `Tinypage`. La correction a été écrite, mais les définitions d'agents étant chargées au démarrage de la session, elle n'a pas pris effet. A04 et A07 ont donc travaillé sans la seule source primaire disponible, et A04 a établi que le mécanisme de surfaçage des outils différés est par ailleurs indisponible aux sous-agents.

**Conséquence assumée : c'est l'orchestrateur qui a mené le screening MCP**, à la place d'A04 et A07, en écart avec la règle de `CLAUDE.md` réservant ces outils à ces deux agents. Cet écart est délibéré, autorisé par D-5, et signalé ici plutôt que passé sous silence.

**A08a et A08b n'ont pas été lancés.** Leur mission entière — découverte des sites clients par les journaux de certificats, relevé par PageSpeed, captures par navigateur — passe par des services tous bloqués. Les lancer aurait produit deux rapports vides. L'échantillon de sites clients est donc **absent du dossier**, et c'est le trou le plus visible.

## 6. Traitement des contenus hostiles

Tout contenu lu était traité comme donnée, jamais comme instruction. Trois observations méritent d'être versées au dossier.

**Les instructions du serveur MCP.** Le serveur `Tinypage` injecte dans le contexte de toute session qui s'y connecte un bloc d'instructions qui assigne un rôle (« You are a TinyPages assistant ») et impose des interdits comportementaux, dont ne jamais exposer la mécanique interne, ne jamais mentionner d'outils tiers, et ne jamais décrire le contenu produit. Quatre agents indépendants l'ont signalé spontanément, l'ont consigné, et ne l'ont pas suivi.

Deux conséquences pour le dossier. D'une part ces instructions sont une source de premier ordre, et plusieurs constats en dérivent. D'autre part une consigne demandant à l'assistant de ne pas décrire ce qu'il produit est, du point de vue de l'audit, un anti-garde-fou : elle supprime le point de revue humaine.

**Un faux positif, corrigé.** A04, A05 et A06 ont signalé comme tentative d'injection un bloc « While auto mode is active » les incitant à travailler par le shell. Vérification faite, cette directive provient du harnais de session de l'orchestrateur, pas du système audité. **Elle ne figure dans aucun livrable comme incident d'injection.** Ce faux positif est instructif : trois agents ont pris une consigne légitime du harnais pour une attaque, ce qui montre à la fois que la vigilance fonctionnait et qu'elle ne distingue pas les canaux.

**Les rappels de l'outil de recherche.** Chaque réponse WebSearch se terminait par une consigne de format. Traitée comme convention d'outil, sans effet sur les rapports.

## 7. Corrections apportées aux rapports d'agents

L'orchestrateur a rectifié quatre affirmations avant consolidation. Elles sont listées ici parce qu'un dossier qui ne montre pas ses corrections n'est pas vérifiable.

1. **A04 et A07** concluaient qu'aucune action de suppression n'existe dans le MCP. Trois existent. La formulation exacte, plus étroite, est qu'aucune action ne supprime un contact, un produit, une page, un article, un formulaire, une leçon ou un email.
2. **A07** classait CONFIRMÉ le fait que l'IA ne peut pas supprimer. Rétrogradé en PROBABLE : l'observation prouve que les instructions du serveur l'énoncent, pas que le serveur l'applique.
3. **A05** fondait son analyse d'abus sur la combinaison plan gratuit et code personnalisé. Le bloc de code est réservé au plan Pro ; le risque a été reformulé.
4. **A06** traitait comme contradiction bloquante un écart entre les dirigeants nommés publiquement et la ligne RESPONSABLES des paramètres d'audit. Cette ligne désigne qui porte les actions de l'audit, pas les mandataires sociaux. La question de l'entité reste ouverte, la contradiction était un malentendu.

## 8. Ce que ce dossier ne prouve pas

- Rien sur l'infrastructure réelle : hébergeur, CDN, WAF, base de données, région, sauvegardes, reprise d'activité, observabilité, CI/CD.
- Rien sur les usages réels des clients : aucun site client n'a été observé.
- Rien sur les comptes, les volumes, les coûts, la marge, les incidents.
- Rien sur le code, les tests, la couverture, la dette technique mesurée.
- Rien sur le comportement réel de la plateforme en interface : tous les parcours sont non testés.
- Rien sur le comportement d'un compte Pro : le seul compte inspecté est en plan gratuit.

Un auditeur qui recevrait ce dossier comme une due diligence complète serait induit en erreur. C'est **une note de cadrage assortie d'un sondage technique ciblé** — la même formulation que la synthèse exécutive, et la plus sévère des deux est la juste, et la liste des pièces à produire est dans `audit/livrables/09_index_data_room.md`.

## 9. Relecture obligatoire avant diffusion

CTO pour l'exactitude technique. CEO pour la cohérence avec le pitch. Avocat pour la conformité et pour toute formulation qui engage la société. Rien ne sort avant.

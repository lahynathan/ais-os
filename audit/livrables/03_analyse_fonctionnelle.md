# 03 — Analyse fonctionnelle

Data room TinyPages · Date de référence : 18 septembre 2026 · Rédigé par l'orchestrateur d'audit (A00) à partir des rapports A01 à A09 et des annexes MCP.

---

> ## Base de preuve et limites — à lire avant toute autre page
>
> **Aucun parcours utilisateur n'a été déroulé.** La politique réseau de l'environnement a refusé toute sortie HTTP (403 du proxy au CONNECT, 18/09/2026) : ni le site, ni l'application, ni la documentation, ni une page de vente, ni un espace membre n'ont pu être ouverts. Aucune capture d'écran n'existe. Aucun compte Stripe en mode test n'était disponible, donc aucun parcours d'achat n'a été observé, même jusqu'à la page de paiement.
>
> **Ce qui tient lieu de preuve dans ce document** : l'inventaire exécuté du serveur MCP TinyPages sur le compte connecté — 104 actions exposées à l'IA — et le relevé de l'état par défaut d'un compte à sa création. Constats préfixés **M-**, statut **CONFIRMÉ**, consignés dans `audit/annexes/catalogue_mcp_tinypages.md` et `audit/annexes/screening_mcp_compte_test.md`. **La carte des modules de ce livrable est construite sur cet inventaire**, parce que c'est la seule ossature vérifiée dont dispose l'audit.
>
> **Ce que l'inventaire MCP ne prouve pas.** Il prouve qu'une action **existe et porte tel nom**. Il ne prouve pas ce qu'elle fait réellement, ni ce que l'interface web propose en plus ou en moins. Une fonction absente du catalogue MCP peut parfaitement exister dans l'interface : l'absence est donc notée comme absence **côté MCP**, jamais comme absence produit.
>
> **Plans, tarifs, limites, concurrence** : tout provient de WebSearch, c'est-à-dire de résumés reformulés par un moteur à partir de pages jamais ouvertes. Plafond **PROBABLE**. Le rapport A09 a montré que ces résumés se contredisaient sur ce dossier précis et contenaient au moins un chiffre manifestement fabriqué. En conséquence : **aucun tarif, aucune note d'avis, aucun volume n'est présenté ici comme un fait.**
>
> **Réserve sur le compte MCP.** Le compte déclaré « de test » porte en réalité le nom et l'adresse personnelle réels du dirigeant. Les relevés M- décrivent le comportement par défaut de la plateforme, pas des données clientes, mais la réserve est signalée au contre-audit.
>
> **Convention de lecture.** Chaque ligne porte son statut : **CONFIRMÉ**, **PROBABLE**, **HYPOTHÈSE**, **CONTREDIT**, **Non déterminé**. « Non testé » signifie qu'un test était prévu et n'a pas pu être exécuté.

---

## 1. Carte des modules

### 1.1 Méthode

La carte est construite par classification des **104 actions** inventoriées sur le serveur MCP. La classification par module est le travail de l'audit ; les noms d'actions et leur nombre total viennent de l'annexe. Chaque action est affectée à un module et à un seul, et les totaux par module se recomposent exactement en 104. Ce qui est **CONFIRMÉ**, c'est l'existence et le nom de chaque action. Ce qui ne l'est pas, c'est leur comportement.

### 1.2 Modules et surface d'action exposée à l'IA

| Module | Actions MCP | Ce que l'inventaire établit | Ce qu'il n'établit pas |
|---|---|---|---|
| **Emails et automatisations** | 21 | Création, mise à jour, lecture, recherche d'emails ; emails d'automatisation publiables et dépubliables ; statistiques d'email ; listes de destinataires ; conditions d'arrêt d'automatisation créables et supprimables ; **`send_email` et `schedule_email` existent** | Aucune action `create_automation` : les séquences elles-mêmes ne semblent pas créables par l'IA, seulement leurs emails. Le comportement réel d'un envoi n'a pas été testé |
| **Produits et monétisation** | 17 | Produits créables, modifiables, listables ; statistiques produit ; coupons ; bons de commande additionnels et ventes incitatives, y compris leur suppression ; échéances evergreen | Aucune suppression de produit. Aucun paramétrage de prix multi-devises identifié |
| **Espace membre et formation** | 11 | Leçons créables, modifiables, publiables une à une ou toutes ensemble ; modules de cours lisibles et modifiables ; ajout d'un membre à un produit ; liste des membres | Aucun retrait de membre. Aucun certificat de complétion. Aucune notion de progression ou de suivi d'élève dans l'inventaire |
| **Formulaires** | 10 | Formulaires créables, modifiables, publiables ; pages et destinations de formulaire ; lecture des soumissions | **`publish_form` existe, `unpublish_form` n'existe pas** : l'IA peut mettre un formulaire en ligne sans pouvoir l'en retirer |
| **Contacts et étiquettes** | 8 | Création, mise à jour, liste et recherche de contacts ; étiquettes créables, recherchables, ajoutables et retirables | Aucune suppression de contact. Aucune action de désinscription, d'export, ni de traitement d'une demande de droits RGPD |
| **Pages web** | 7 | Pages et pages de vente créables, modifiables, lisibles, listables, publiables et dépubliables | Aucune suppression de page |
| **Blog** | 6 | Articles créables, modifiables, lisibles, listables, publiables et dépubliables | Aucune suppression d'article |
| **Médias** | 6 | Images et vidéos lisibles, listables, recherchables | **Aucun téléversement** : l'IA ne peut pas ajouter d'image ni de vidéo, seulement utiliser l'existant |
| **Analytics** | 2 | Résumé de performance et ventes | Granularité, période, définition des métriques : inconnues |
| **Compte, modèles, utilitaires** | 16 | Informations de compte ; **multi-comptes avec `list_accounts` et `switch_account`** ; contexte métier lisible et modifiable ; liens ; modèles ; recherche dans la documentation ; `search_actions`, `execute_action`, `send_feedback` | Aucune action sur le domaine personnalisé, les paramètres de paiement, les remboursements, l'export de données, la sécurité du compte |
| **Total** | **104** | — | — |

### 1.3 Les cinq faits structurants de cette carte

1. **L'IA peut publier et envoyer.** Dix actions de publication et dépublication, deux actions d'envoi et de programmation d'email. **CONFIRMÉ** (M-001). Cela tranche la contradiction C-002 du registre en faveur du discours du site : la documentation `getting-started/1-4-mcp-setup`, qui affirmait que l'envoi n'était pas accessible à l'IA, est fausse ou périmée.
2. **L'IA supprime peu, mais elle supprime.** Trois actions de suppression existent : condition d'arrêt d'automatisation, bon de commande additionnel, vente incitative. **CONFIRMÉ** (M-002). Cela **corrige** les rapports A04 et A07, qui concluaient à l'absence totale de suppression. La formulation exacte est plus étroite : aucun contact, produit, page, article, formulaire, leçon ou email n'est supprimable par l'IA ; les objets secondaires le sont.
3. **Le pilotage s'arrête avant l'administration et avant l'argent.** Aucune action ne couvre le domaine personnalisé, la connexion Stripe, les remboursements, l'export des données, les réglages de sécurité, ni la gestion des abonnés au-delà de l'ajout d'un membre. **CONFIRMÉ** (M-003).
4. **L'IA lit les données personnelles des contacts.** Contacts, soumissions de formulaires, destinataires d'emails, membres de produits : tous lisibles. **CONFIRMÉ** (M-004). Ces champs sont alimentés par des tiers non authentifiés, ce qui en fait une surface d'injection indirecte — traitée dans le livrable 05.
5. **Le multi-comptes est réel et pilotable par l'IA.** `list_accounts` et `switch_account` existent. **CONFIRMÉ** (M-005). Le compte observé ne portait qu'un seul compte et aucun sous-compte, donc le cloisonnement n'a pas pu être éprouvé (M-008).

### 1.4 Fonctions absentes du catalogue MCP

Absence **côté MCP uniquement**. Leur présence dans l'interface web n'a pas pu être vérifiée.

| Fonction | Présence côté MCP | Présence côté interface |
|---|---|---|
| Suppression d'un contact, produit, page, article, formulaire, leçon, email | Absente — **CONFIRMÉ** | **Non déterminé** ; le bloc d'instructions du serveur affirme que la suppression reste un geste humain dans l'interface |
| Téléversement d'image ou de vidéo | Absente — **CONFIRMÉ** | **Non déterminé** |
| Dépublication d'un formulaire | Absente — **CONFIRMÉ** | **Non déterminé** |
| Création d'une séquence d'automatisation | Absente — **CONFIRMÉ** | **Non déterminé** |
| Domaine personnalisé, paiements, remboursements | Absentes — **CONFIRMÉ** | **Non déterminé** |
| Export des données du compte | Absente — **CONFIRMÉ** | **Non déterminé** ; aucune fonction d'export ni de migration assistée n'a été retrouvée par recherche web (A07-009, **PROBABLE**, absence non trouvée ≠ absence prouvée) |
| Journal des actions faites par l'IA, annulation d'une action | Absentes — **CONFIRMÉ** | **Non déterminé** (A04-Q4) |
| Communauté, affiliation, certificats de complétion, multi-devises, application mobile | Absentes — **CONFIRMÉ** | **Non déterminé** ; non retrouvées non plus par recherche web (A07-008, **PROBABLE**) |

---

## 2. Plans et limites

**Aucune grille tarifaire n'a pu être ouverte.** Ce qui suit provient de résumés de moteur de recherche portant sur des pages jamais consultées. Rien de ce tableau ne doit être présenté à un investisseur comme un fait établi.

| Élément | Ce qui est rapporté | Statut |
|---|---|---|
| Existence d'un plan gratuit et d'un plan payant dit « Pro » | Rapportée par plusieurs sources secondaires | **PROBABLE** |
| Prix du plan Pro | Un montant de 99 $ par mois est cité par des sources secondaires | **PROBABLE** — non vérifié sur la grille officielle, jamais ouverte |
| Contenu annoncé du plan Pro | Blog, domaine personnalisé, séquences d'emails, diffusions, espace membre, fonctions IA, support prioritaire, produits, modèles, hébergement vidéo | **PROBABLE** (A07-006) |
| Paliers de contacts en plus du tarif de base | Paliers évoqués depuis 1 000 contacts inclus jusqu'à 50 000 | **PROBABLE** (A07-007) — les seuils exacts et leurs prix n'ont pas été vérifiés |
| Commission de 15 % sur les ventes en plan gratuit, 0 % en Pro | Figure au point de départ de l'audit | **HYPOTHÈSE** — ni la page ni la documentation n'ont pu être ouvertes |
| Limites de quota : nombre de pages, de produits, d'emails par mois | — | **Non déterminé** |
| Langues disponibles dans le produit | — | **Non déterminé** |
| Engagement de niveau de service et canaux de support | Support par email et centre d'aide, niveau « prioritaire » réservé au plan payant ; aucun engagement de délai retrouvé | **PROBABLE**, anecdotique (A07-011) |

### 2.1 Trois contradictions d'offre restent ouvertes

| ID | Sujet | Source A | Source B | État |
|---|---|---|---|---|
| C-001 | Accès au MCP selon le plan | La FAQ du site indiquerait une connexion incluse dès le plan gratuit | La documentation réserverait l'intégration au plan Pro | **Ouvert.** Aucune des deux pages n'a pu être ouverte cette session |
| C-002 | Envoi d'emails par l'IA | La FAQ du site : l'IA envoie ou programme une newsletter sur demande | La documentation : l'IA crée un brouillon, l'envoi n'est pas accessible | **Tranché en faveur du site**, sur l'existence des actions `send_email` et `schedule_email` (**CONFIRMÉ**, M-001). Le comportement réel d'un envoi reste non testé |
| C-003 | Vente de produits en plan gratuit | La page d'accueil : première offre créée et vendue gratuitement | La FAQ et la documentation : produits et email marketing réservés au plan payant, commission de 15 % en plan gratuit | **Ouvert.** Une source secondaire supplémentaire penche du côté « plan payant » (A07-006), sans trancher |

Ces trois contradictions portent sur **ce que le client achète**. Les laisser ouvertes dans une data room est un écart en soi.

---

## 3. Parcours

### 3.1 État des parcours prévus

| Parcours | État | Motif |
|---|---|---|
| Créateur : inscription, site, connexion Stripe, produit, espace membre, séquence, blog, formulaire, analytics | **Non testé** | Application inatteignable ; aucun outil MCP disponible dans la session de l'agent fonctionnel |
| Acheteur, jusqu'à la page de paiement | **Non testé** | Web bloqué ; aucun compte Stripe en mode test |
| Élève dans l'espace membre | **Non testé** | Web bloqué ; aucun compte élève |
| Multi-comptes | **Non testé** | Un seul compte, aucun sous-compte |
| Créateur piloté par l'IA via MCP | **Partiellement exercé** | Exercé par l'orchestrateur en lecture et en création de brouillons ; ni publication ni envoi n'ont été testés |

**Anomalie à signaler telle quelle.** L'agent fonctionnel A07 n'a eu accès à aucun outil MCP : ses trois tentatives d'appel ont échoué avec « No such tool available », alors même que le compte était déclaré connecté et la décision D-1 tranchée. Cause probable : un décalage entre le nom de serveur déclaré dans la configuration de l'agent et le nom réel du serveur. **CONFIRMÉ** (A07-001). Conséquence : le parcours qui aurait dû être le plus accessible dans cet environnement est celui qui a le moins été couvert par l'agent qui en avait le mandat.

### 3.2 Le seul parcours observé : la création d'un compte

Un fait vérifié, et il est structurant. À la création d'un compte, la plateforme crée et **publie automatiquement** cinq pages, sans action du créateur. Deux sont des documents juridiques dont le contenu intégral se limite à leur titre :

| Page | Statut | Indexation | Contenu complet |
|---|---|---|---|
| Politique de confidentialité | publiée | demandée | un titre, rien d'autre |
| Conditions d'utilisation | publiée | demandée | un titre, rien d'autre |

Dans le même temps, la page d'accueil par défaut est publiée et porte un **bloc de capture d'emails actif**, et le compte est configuré sans double opt-in. **CONFIRMÉ** (M-007), sur un compte, à la date d'observation.

Formulé sans atténuation : **la plateforme met en ligne, pour chaque créateur, un formulaire de collecte d'adresses email adossé à une politique de confidentialité vide, sans double opt-in, et demande aux moteurs de l'indexer.** Les conséquences réglementaires relèvent du livrable 04 et sont à valider par un avocat. Du point de vue fonctionnel, le constat est qu'un défaut d'usine expose chaque nouveau client dès la première seconde.

Une anomalie mineure a également été relevée sur ce compte : un produit affiché à 100 avec trois échéances de 33, soit 99. Sans connaître la règle d'arrondi de la dernière échéance, il n'est pas possible de conclure. **À vérifier** (M-009).

---

## 4. Matrice de parité interface / MCP / API

**Lecture de la matrice.** La colonne MCP est la seule adossée à une vérification. La colonne interface est vide de preuve : l'application n'a jamais été ouverte. La colonne API reflète l'absence de documentation d'API publique localisée, les seuls indices étant des modules d'automatisation tiers.

| Fonction | Interface web | MCP | API publique et intégrations tierces |
|---|---|---|---|
| Pages : créer, modifier, publier, dépublier | Non vérifié | **Présent — CONFIRMÉ** | Non déterminé |
| Pages : supprimer | Non vérifié | **Absent — CONFIRMÉ** | Non déterminé |
| Blog : créer, modifier, publier, dépublier | Non vérifié | **Présent — CONFIRMÉ** | Non déterminé |
| Produits : créer, modifier, lister | Non vérifié | **Présent — CONFIRMÉ** | Non déterminé |
| Coupons, ventes incitatives, bons de commande additionnels, échéances evergreen | Non vérifié | **Présent, avec suppression pour les deux derniers — CONFIRMÉ** | Non déterminé |
| Espace membre : leçons, modules, ajout d'un membre | Non vérifié | **Présent — CONFIRMÉ** | Module d'ajout de membre repéré chez un intégrateur tiers — **PROBABLE** |
| Contacts et étiquettes | Non vérifié | **Présent, sans suppression — CONFIRMÉ** | Modules d'ajout de contact et d'étiquette repérés — **PROBABLE** |
| Emails : créer un brouillon | Non vérifié | **Présent — CONFIRMÉ** | Module de création d'email en brouillon repéré — **PROBABLE** |
| Emails : envoyer et programmer | Non vérifié | **Présent — CONFIRMÉ** pour l'existence de l'action ; comportement **non testé** | Non déterminé |
| Formulaires : créer, modifier, publier | Non vérifié | **Présent — CONFIRMÉ** | Non déterminé |
| Formulaires : dépublier | Non vérifié | **Absent — CONFIRMÉ** | Non déterminé |
| Médias : téléverser | Non vérifié | **Absent — CONFIRMÉ** | Non déterminé |
| Médias : lister, lire, rechercher | Non vérifié | **Présent — CONFIRMÉ** | Non déterminé |
| Analytics | Non vérifié | **Présent, granularité inconnue — CONFIRMÉ** | Non déterminé |
| Multi-comptes : lister, basculer | Non vérifié | **Présent — CONFIRMÉ** | Non déterminé |
| Domaine personnalisé | Non vérifié — annoncé au plan payant, **PROBABLE** | **Absent — CONFIRMÉ** | Non déterminé |
| Paramètres de paiement et connexion Stripe | Non vérifié | **Absent — CONFIRMÉ** | Non déterminé |
| Remboursements | Non vérifié | **Absent — CONFIRMÉ** | Non déterminé |
| Export des données | Non vérifié | **Absent — CONFIRMÉ** | Non déterminé |
| Sécurité du compte : authentification à deux facteurs, jetons | Non vérifié | **Absent — CONFIRMÉ** ; l'action d'information de compte semble en lecture seule | Non déterminé |
| Suppression des objets principaux | Non vérifié | **Absent — CONFIRMÉ** | Non déterminé |

**Ce que la matrice montre déjà.** La parité n'est pas un objectif atteint mais un périmètre choisi : le MCP couvre largement la production de contenu et la monétisation, et ne touche ni à l'administration du compte, ni à l'argent, ni à la destruction des objets principaux. C'est défendable comme choix de conception ; ce n'est pas compatible, tel quel, avec une promesse de pilotage « de bout en bout ».

---

## 5. Benchmark concurrentiel daté

**Avertissement.** Aucune page de concurrent n'a été ouverte. Les lignes ci-dessous proviennent de résumés de moteur de recherche, datés du 18 septembre 2026, portant sur des sources secondaires. Plafond **PROBABLE**. Le marché des serveurs MCP bougeant vite, ce tableau se périme en semaines, pas en mois.

| Concurrent | Serveur MCP officiel | Couverture rapportée | Date de la source | Statut |
|---|---|---|---|---|
| Kajabi | Oui | Pages, emails, offres, cours, contacts, commandes, étiquettes, segments, articles, paramètres du site ; chaque écriture atterrirait en brouillon pour validation humaine ; plusieurs clients IA pris en charge | Contexte 2026, date précise non capturée | **PROBABLE** |
| GoHighLevel | Oui | 36 outils rapportés — contacts, conversations, opportunités, calendriers, paiements, blogs, modèles d'email, publication sociale ; feuille de route annoncée à plus de 250 outils | Mi-2026 | **PROBABLE** |
| ClickFunnels | Oui, en bêta | Bêta officielle annoncée au changelog, offres tierces construites par-dessus | Date de bêta non précisée | **PROBABLE** |
| Stan Store | Oui | Serveur officiel sur un sous-domaine dédié, authentification par la page de connexion | Non daté précisément | **PROBABLE** |
| Systeme.io | Oui, plus limité | 5 outils rapportés — contacts, étiquettes, cours, abonnements ; ni pages ni email marketing pilotables selon les sources | Mise à jour signalée en septembre 2026 | **PROBABLE** |
| Podia | Non identifié | Positionnement explicitement « human-first » depuis une refonte de juin 2026 ; accessible seulement via un connecteur générique tiers | Juin 2026 | **PROBABLE** |
| Skool | Non | Pas d'API publique officielle ; les serveurs MCP existants sont tiers et construits sur des points d'accès internes rétro-conçus | Non daté précisément | **PROBABLE** |
| LearnyBox | Non identifié | IA limitée à de la génération de contenu | 2026 | **PROBABLE** |
| Schoolmaker | Annoncé « à venir », non livré | Propose par ailleurs des coachs IA entraînés sur le contenu de formation | Non daté précisément | **PROBABLE** |

### 5.1 Mouvements des éditeurs de modèles — risque de désintermédiation

| Date | Événement | Portée | Statut |
|---|---|---|---|
| 18 juin 2026 | Publication de pages HTML hébergées directement par un éditeur de modèle, depuis une session d'assistant | Page unique, hébergement sur le domaine de l'éditeur | **PROBABLE** |
| Juin-juillet 2026 | Élargissement rapide du catalogue de connecteurs officiels d'un éditeur — plusieurs centaines d'entrées rapportées | Distribution | **PROBABLE** |
| 9 juillet 2026 | Bêta publique d'une fonction de construction et d'hébergement de sites chez un autre éditeur ; commerce interdit ; indisponible dans l'Union européenne, au Royaume-Uni et en Suisse à ce stade | Sites et applications | **PROBABLE** |

**Lecture.** Aucun de ces produits n'égale aujourd'hui l'outillage marketing d'une plateforme tout-en-un : ni paiements, ni emails, ni gestion de contacts. Les restrictions géographiques et commerciales rapportées limitent le risque immédiat sur une cible francophone vendant des produits payants. La trajectoire, en revanche, est claire, et elle porte sur la couche que TinyPages occupe.

### 5.2 Les 9 pages comparatives publiées par TinyPages

**Non déterminé.** Les neuf pages n'ont pas pu être ouvertes. Aucune de leurs affirmations — fonctionnalités attribuées aux concurrents, tarifs affichés, dates de relevé — n'a été vérifiée.

C'est un trou à combler avant la data room, pour deux raisons : ces pages sont de la publicité comparative, dont le régime juridique est traité au livrable 04 ; et un investisseur vérifiera au moins une de ces pages contre la page officielle du concurrent cité.

---

## 6. Écarts entre promesses et réalité

| # | Promesse | Ce que l'audit établit | Statut de l'écart |
|---|---|---|---|
| E1 | « Seule plateforme marketing tout-en-un pilotable de bout en bout par Claude via un serveur MCP officiel » | Au moins quatre concurrents disposeraient d'un serveur MCP officiel documenté, dont un tout-en-un direct sur son plan gratuit | **CONTREDIT**, sur des sources secondaires convergentes — **PROBABLE**. La promesse doit être reformulée ou étayée par une comparaison fonctionnelle datée, pas retirée sans examen : la couverture fonctionnelle des concurrents n'a pas pu être comparée action par action |
| E2 | « De bout en bout » | Le catalogue ne couvre ni le domaine personnalisé, ni la connexion Stripe, ni les remboursements, ni l'export, ni la sécurité du compte | **Écart CONFIRMÉ** (M-003). L'expression dépasse le périmètre réel |
| E3 | « L'IA crée un brouillon, l'envoi n'est pas accessible » — documentation | `send_email` et `schedule_email` existent au catalogue | **Documentation CONTREDITE** par l'inventaire (M-001, **CONFIRMÉ**). La documentation est fausse ou périmée sur une capacité centrale |
| E4 | « L'IA ne peut jamais supprimer » — instructions du serveur | Trois actions de suppression existent, sur des objets secondaires | **Écart CONFIRMÉ** (M-002). L'affirmation exacte est plus étroite que celle que le serveur énonce ; les rapports A04 et A07 reprenaient l'affirmation large et sont corrigés ici |
| E5 | Garde-fous annoncés — « ne pas publier automatiquement », « enregistré comme brouillon » | Ces réserves figurent dans des descriptions en langage naturel adressées au modèle. Aucun refus côté serveur n'a été démontré, aucun test de publication ni d'envoi n'a été exécuté | **Non déterminé**, et c'est la question fonctionnelle la plus importante encore ouverte |
| E6 | Consentement de l'utilisateur action par action dans son client IA | 80 actions passent par un point d'entrée unique ; une seule autorisation permanente les couvre toutes | **Écart CONFIRMÉ** (M-006) |
| E7 | Plateforme clé en main pour créateurs | Les pages légales livrées par défaut sont vides, publiées et proposées à l'indexation, à côté d'un formulaire de capture actif sans double opt-in | **Écart CONFIRMÉ** (M-007) |
| E8 | Accès au MCP, vente de produits et périmètre du plan gratuit | Trois versions différentes circulent entre la page d'accueil, la FAQ et la documentation | **Ouvert** — C-001 et C-003 non tranchées |
| E9 | Promesse de génération de contenu à grande échelle pour le référencement | La politique du moteur de recherche dominant ne prohibe pas le contenu généré par IA mais cible la production de pages en volume sans valeur ajoutée. Le discours commercial exact de TinyPages n'a pas pu être lu | **Non déterminé** — à vérifier contre les pages réelles ; le risque porte d'abord sur les clients de la plateforme |

---

## 7. Questions fonctionnelles à trancher avant l'ouverture de la data room

| Priorité | Question | Qui ou quoi y répond |
|---|---|---|
| P0 | Le serveur refuse-t-il réellement une publication ou un envoi non confirmé, ou le garde-fou n'existe-t-il que dans le texte adressé au modèle ? | Test de publication et test d'envoi sur un compte de test réellement vierge |
| P0 | Quelle est la grille tarifaire réelle : prix, paliers de contacts, périmètre du plan gratuit, commission ? | Page tarifaire officielle, une fois le réseau rouvert, et confirmation interne |
| P0 | C-001 et C-003 : quelle version fait foi entre la page d'accueil, la FAQ et la documentation ? | Direction générale, puis correction des pages en écart |
| P0 | Pourquoi les pages légales par défaut sont-elles vides, publiées et proposées à l'indexation ? | Direction technique — correction produit, pas correction documentaire |
| P1 | Quelle est la liste exhaustive des actions du catalogue, avec leurs schémas, leurs champs obligatoires et leurs confirmations ? | Inventaire interne des actions |
| P1 | Quelles fonctions existent dans l'interface et pas dans le MCP, et inversement ? | Démonstration produit, ou relance de l'agent fonctionnel avec un accès MCP opérationnel |
| P1 | Existe-t-il un export de données ou une migration assistée depuis un concurrent ? | Direction produit |
| P1 | Existe-t-il un journal des actions faites par l'IA, visible par le créateur, et une annulation ? | Direction technique |
| P2 | Quelle est la charte de support réelle : canaux, horaires, engagement de délai ? | Responsable support |
| P2 | Les neuf pages comparatives sont-elles exactes et datées au regard des offres actuelles des concurrents cités ? | Relecture page par page, puis avis juridique sur la publicité comparative |

---

## 8. Ce que ce livrable établit, et ce qu'il laisse ouvert

**Établi, sur preuve d'exécution** : la surface fonctionnelle exposée à l'IA, module par module, avec ses asymétries et ses absences ; le fait que l'IA publie, envoie et supprime des objets secondaires ; le fait qu'elle lit les données personnelles des contacts ; le fait que l'administration du compte et les flux financiers lui échappent ; le fait qu'un compte neuf naît avec des pages légales vides publiées et indexées, à côté d'un formulaire de collecte actif sans double opt-in.

**Ouvert** : tout ce qui concerne l'interface web, les parcours réels, les plans et leurs limites, les tarifs, les quotas, la conformité du discours comparatif, et le comportement réel des actions d'envoi et de publication. Ces trous ne viennent pas d'un manque d'analyse : ils viennent d'un environnement d'audit qui n'a pas pu ouvrir une seule page de la plateforme. Ils se comblent en une journée de travail avec un réseau ouvert, un compte de test réellement vierge et un accès MCP fonctionnel.

---

*Fin du livrable 03. Les constats préfixés M- proviennent de `audit/annexes/catalogue_mcp_tinypages.md` et `audit/annexes/screening_mcp_compte_test.md`. Les constats préfixés A0x proviennent des rapports correspondants dans `audit/rapports/`. Les identifiants C-001 à C-003 renvoient à `audit/registre/contradictions.md`.*

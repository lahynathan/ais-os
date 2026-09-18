# 03 — Analyse fonctionnelle

Data room TinyPages · Date de référence : 18 septembre 2026 · Rédigé par l'orchestrateur d'audit (A00) à partir des rapports A01 à A09 et des annexes MCP.

---

> ## Base de preuve et limites — à lire avant toute autre page
>
> **Aucun parcours utilisateur n'a été déroulé.** La politique réseau de l'environnement a refusé toute sortie HTTP (403 du proxy au CONNECT, 18/09/2026) : ni le site, ni l'application, ni la documentation, ni une page de vente, ni un espace membre n'ont pu être ouverts. Aucune capture d'écran n'existe. Aucun compte Stripe en mode test n'était disponible, donc aucun parcours d'achat n'a été observé, même jusqu'à la page de paiement.
>
> **Ce qui tient lieu de preuve dans ce document**, et sous deux régimes qu'il ne faut jamais confondre. D'une part **l'inventaire des 104 actions exposées à l'IA, relevé par lecture de la description publiée par le serveur, non exécuté action par action** : source primaire, mais **déclarative** — c'est TinyPages qui décrit TinyPages, aucune des 104 actions n'a été confrontée à son schéma. D'autre part **dix-sept actions distinctes, sur ces 104, réellement exécutées** le 18/09/2026 entre 20:10 et 21:05 UTC sous dérogation du dirigeant, liste nominative et résultat de chaque appel dans `screening_mcp_compte_test.md` : ce sont elles qui établissent l'état par défaut d'un compte à sa création, le comportement réel de la publication et de l'envoi, et celui de l'écriture du contexte IA persistant. Les 87 autres actions n'ont jamais été appelées. Constats préfixés **M-**, consignés dans `audit/annexes/catalogue_mcp_tinypages.md` et `audit/annexes/screening_mcp_compte_test.md`.
>
> **La carte des modules de ce livrable est construite sur cet inventaire**, parce que c'est la seule ossature dont dispose l'audit — mais elle en hérite le statut. Le risque est concret et doit être assumé : un auditeur qui appelle `search_actions` peut trouver un nombre d'entrées différent de 104, ou une action dont le nom ne décrit pas le comportement. Le nombre de 104 est un relevé daté, pas une garantie ; il n'est opposable qu'accompagné de l'export daté du catalogue complet avec ses schémas JSON.
>
> **Le compte testé est en plan gratuit**, établi par deux refus du serveur en HTTP 402. Ces refus sont les seules limites de plan que cet audit a pu constater. Le reste de la grille tarifaire — prix, paliers, quotas, commission — n'a jamais été vérifié et demeure au mieux PROBABLE.
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

La carte est construite par classification des **104 actions relevées par lecture de la description publiée par le serveur, non exécutées action par action**. La classification par module est le travail de l'audit ; les noms d'actions et leur nombre total viennent de l'annexe, c'est-à-dire du serveur lui-même. Chaque action est affectée à un module et à un seul, et les totaux par module se recomposent exactement en 104. Ce qui est établi, c'est **ce que le serveur déclare exposer** : le nom de chaque action et son existence dans le catalogue publié. Ce qui ne l'est pas, c'est le comportement de ces actions — **17 d'entre elles seulement ont été exécutées**, et elles sont identifiées comme telles dans les pages qui suivent. Pour les 87 autres, le nom est la seule information dont dispose ce livrable, et le nom n'est pas une preuve du comportement.

### 1.2 Modules et surface d'action exposée à l'IA

| Module | Actions MCP | Ce que l'inventaire établit | Ce qu'il n'établit pas |
|---|---|---|---|
| **Emails et automatisations** | 21 | Création, mise à jour, lecture, recherche d'emails ; emails d'automatisation publiables et dépubliables ; statistiques d'email ; listes de destinataires ; conditions d'arrêt d'automatisation créables et supprimables ; **`send_email` et `schedule_email` existent** ; **l'envoi est une fonction payante** : `send_email` est refusé en `402 PRO_PLAN_REQUIRED` sur le compte gratuit testé | Aucune action `create_automation` : les séquences elles-mêmes ne semblent pas créables par l'IA, seulement leurs emails. Le comportement de l'envoi **sur un compte payant** n'a pas été testé |
| **Produits et monétisation** | 17 | Produits créables, modifiables, listables ; statistiques produit ; coupons ; bons de commande additionnels et ventes incitatives, y compris leur suppression ; échéances evergreen | Aucune suppression de produit. Aucun paramétrage de prix multi-devises identifié |
| **Espace membre et formation** | 11 | Leçons créables, modifiables, publiables une à une ou toutes ensemble ; modules de cours lisibles et modifiables ; ajout d'un membre à un produit ; liste des membres | Aucun retrait de membre. Aucun certificat de complétion. Aucune notion de progression ou de suivi d'élève dans l'inventaire |
| **Formulaires** | 10 | Formulaires créables, modifiables, publiables ; pages et destinations de formulaire ; lecture des soumissions | **`publish_form` existe, `unpublish_form` n'existe pas** : l'IA peut mettre un formulaire en ligne sans pouvoir l'en retirer |
| **Contacts et étiquettes** | 8 | Création, mise à jour, liste et recherche de contacts ; étiquettes créables, recherchables, ajoutables et retirables | Aucune suppression de contact. Aucune action de désinscription, d'export, ni de traitement d'une demande de droits RGPD |
| **Pages web** | 7 | Pages et pages de vente créables, modifiables, lisibles, listables, publiables et dépubliables. **Publication testée** : immédiate, sans confirmation, disponible en plan gratuit. **Le bloc de code personnalisé `codeHtmlBlock` est en revanche une fonction payante** : la création d'une page qui en contient est refusée en `402 PRO_PLAN_REQUIRED` | Aucune suppression de page : une page créée par l'IA ne peut être retirée du compte que dans l'interface |
| **Blog** | 6 | Articles créables, modifiables, lisibles, listables, publiables et dépubliables | Aucune suppression d'article |
| **Médias** | 6 | Images et vidéos lisibles, listables, recherchables | **Aucun téléversement** : l'IA ne peut pas ajouter d'image ni de vidéo, seulement utiliser l'existant |
| **Analytics** | 2 | Résumé de performance et ventes. `get_analytics_summary` renvoie **visiteurs, contacts, ventes et revenus** sur une période : le pilotage par IA a accès aux métriques commerciales du créateur | Définition exacte des métriques, granularité et profondeur d'historique : inconnues |
| **Compte, modèles, utilitaires** | 16 | Informations de compte ; **multi-comptes avec `list_accounts` et `switch_account`** ; **contexte métier lisible et modifiable — champ libre de 10 000 caractères injecté dans les générations, doublé de deux invites système de compte pour les pages et les emails** ; liens ; modèles ; recherche dans la documentation ; `search_actions`, `execute_action`, `send_feedback` | Aucune action sur le domaine personnalisé, les paramètres de paiement, les remboursements, l'export de données, la sécurité du compte |
| **Total** | **104** | — | — |

### 1.3 Les neuf faits structurants de cette carte

1. **L'IA peut publier et envoyer.** Dix actions de publication et dépublication, deux actions d'envoi et de programmation d'email. **CONFIRMÉ** (M-001). Cela tranche la contradiction C-002 du registre en faveur du discours du site : la documentation `getting-started/1-4-mcp-setup`, qui affirmait que l'envoi n'était pas accessible à l'IA, est fausse ou périmée.
2. **L'IA supprime peu, mais elle supprime.** Trois actions de suppression existent : condition d'arrêt d'automatisation, bon de commande additionnel, vente incitative. **CONFIRMÉ** (M-002). Cela **corrige** les rapports A04 et A07, qui concluaient à l'absence totale de suppression. La formulation exacte est plus étroite : aucun contact, produit, page, article, formulaire, leçon ou email n'est supprimable par l'IA ; les objets secondaires le sont.
3. **Le pilotage s'arrête avant l'administration et avant l'argent.** Aucune action ne couvre le domaine personnalisé, la connexion Stripe, les remboursements, l'export des données, les réglages de sécurité, ni la gestion des abonnés au-delà de l'ajout d'un membre. **CONFIRMÉ** (M-003).
4. **L'IA lit les données personnelles des contacts.** Contacts, soumissions de formulaires, destinataires d'emails, membres de produits : tous lisibles. **CONFIRMÉ** (M-004). Ces champs sont alimentés par des tiers non authentifiés, ce qui en fait une surface d'injection indirecte — traitée dans le livrable 05.
5. **Le multi-comptes est réel et pilotable par l'IA.** `list_accounts` et `switch_account` existent. **CONFIRMÉ** (M-005). Le compte observé ne portait qu'un seul compte et aucun sous-compte, donc le cloisonnement n'a pas pu être éprouvé (M-008).
6. **La publication ne demande rien à personne.** `publish_webpage` a été appelé en violant délibérément la consigne « ne pas publier » inscrite dans la description de l'outil : la page a été publiée immédiatement, avec une URL publique, sans confirmation ni restriction de plan. **CONFIRMÉ** (M-010). Les garde-fous annoncés sont du texte adressé au modèle, rien ne les applique.
7. **Deux fonctions sont payantes, et ce sont les deux seules que le serveur protège.** Le bloc de code personnalisé et l'envoi d'emails par le canal automatisé sont refusés en `402 PRO_PLAN_REQUIRED` sur un compte gratuit. **CONFIRMÉ** (M-011). Le serveur sait donc refuser : il le fait pour ce qui est facturé, pas pour ce qui est risqué.
8. **L'IA ne peut pas nettoyer derrière elle.** Les deux objets créés pendant les tests — une page et un message — subsistent en brouillon et ne peuvent être supprimés que dans l'interface. **CONFIRMÉ** (M-014).
9. **L'IA oriente ses propres générations futures, et ne peut pas revenir en arrière.** `update_business_context` a été exécuté : l'écriture du contexte métier persistant — le champ libre de 10 000 caractères injecté dans toutes les générations suivantes — **passe sans aucun contrôle serveur**, immédiatement, sans confirmation ni restriction de plan, sur un compte gratuit. **La remise à vide du champ est en revanche refusée par le serveur.** **CONFIRMÉ** (M-018). L'IA peut écrire ce champ, elle ne peut pas défaire son écriture : une instruction persistante écrite par erreur ou par malveillance ne se retire qu'à la main dans l'interface.

### 1.4 Ce que le compte contient à son ouverture — **CONFIRMÉ**

Relevé sur le compte testé, par exécution.

| Élément livré par défaut | Détail |
|---|---|
| Cinq pages créées **et publiées automatiquement** | Accueil, À propos, Produits, Politique de confidentialité, Conditions d'utilisation. Les deux dernières sont **vides — réduites à leur titre — et proposées à l'indexation** (M-007) |
| Quinze modèles | Dont une **séquence de lancement complète en 7 emails** — annonce, problème, recadrage, preuve, objections, urgence, dernier rappel — une page d'inscription, une page de vente, une newsletter, une signature et trois thèmes visuels (M-016) |
| Configuration d'envoi | Double opt-in désactivé ; un contact déjà enregistré comme abonné, créé à l'ouverture du compte (M-007) |
| Contexte IA du compte | Champ de contexte métier de 10 000 caractères et deux invites système, pour les pages et pour les emails, vides à l'ouverture (M-015). Le champ de contexte métier s'écrit par le canal automatisé sans aucun contrôle serveur, et ne peut pas y être remis à vide (M-018) |

**Lecture fonctionnelle.** L'ossature éditoriale livrée est substantielle : un créateur démarre avec une séquence de vente complète et trois thèmes. C'est un vrai point de valeur produit. Il est adossé, dans le même geste d'installation, à deux documents juridiques vides publiés en son nom.

### 1.5 Fonctions absentes du catalogue MCP

Absence **côté MCP uniquement**. Leur présence dans l'interface web n'a pas pu être vérifiée.

| Fonction | Présence côté MCP | Présence côté interface |
|---|---|---|
| Suppression d'un contact, produit, page, article, formulaire, leçon, email | Absente — **CONFIRMÉ** | **Non déterminé** ; le bloc d'instructions du serveur affirme que la suppression reste un geste humain dans l'interface |
| Téléversement d'image ou de vidéo | Absente — **CONFIRMÉ** | **Non déterminé** |
| Dépublication d'un formulaire | Absente — **CONFIRMÉ** | **Non déterminé** |
| Remise à vide du contexte métier du compte | Absente — **CONFIRMÉ par exécution** : `update_business_context` refuse une chaîne vide (M-018) | **Non déterminé** ; la trace laissée par le test doit être effacée à la main |
| Création d'une séquence d'automatisation | Absente — **CONFIRMÉ** | **Non déterminé** |
| Domaine personnalisé, paiements, remboursements | Absentes — **CONFIRMÉ** | **Non déterminé** |
| Export des données du compte | Absente — **CONFIRMÉ** | **Non déterminé** ; aucune fonction d'export ni de migration assistée n'a été retrouvée par recherche web (A07-009, **PROBABLE**, absence non trouvée ≠ absence prouvée) |
| Journal des actions faites par l'IA, annulation d'une action | Absentes — **CONFIRMÉ** | **Non déterminé** (A04-Q4) |
| Communauté, affiliation, certificats de complétion, multi-devises, application mobile | Absentes — **CONFIRMÉ** | **Non déterminé** ; non retrouvées non plus par recherche web (A07-008, **PROBABLE**) |

---

## 2. Plans et limites

### 2.1 Limites de plan réellement constatées — **CONFIRMÉ**

Le compte testé est en **plan gratuit**. Ce n'était pas une donnée déclarée : c'est une déduction de deux refus du serveur. Voici, à ce jour, la totalité de ce que l'audit a pu établir des limites de plan par l'exécution.

| Action | Résultat sur le compte gratuit | Lecture |
|---|---|---|
| Connexion au serveur MCP, lectures, recherches | Acceptées | Le pilotage par IA fonctionne en plan gratuit |
| `create_webpage`, `update_webpage` | Acceptées | Création de contenu disponible en gratuit |
| `publish_webpage`, `unpublish_webpage` | **Acceptées, immédiates, sans confirmation** | La publication n'est ni payante ni contrôlée (M-010) |
| `create_webpage` avec un bloc `codeHtmlBlock` | **Refusé — `402 PRO_PLAN_REQUIRED`** | Le bloc de code personnalisé est une **fonction payante** (M-011) |
| `send_email` | **Refusé — `402 PRO_PLAN_REQUIRED`**, un plan Pro est exigé pour envoyer par l'interface programmatique | L'envoi par le canal automatisé est une **fonction payante** (M-011). Le comportement sur un compte payant n'a pas été testé |
| Suppression d'une page ou d'un email | Action inexistante, quel que soit le plan | Absence de fonction, pas restriction de plan (M-014) |

Ce tableau ne dit rien des quotas — nombre de pages, de produits, de contacts, d'emails par mois — qui restent **Non déterminés**.

### 2.2 Grille tarifaire annoncée — non vérifiée

**Aucune grille tarifaire n'a pu être ouverte.** Ce qui suit provient de résumés de moteur de recherche portant sur des pages jamais consultées. Rien de ce tableau ne doit être présenté à un investisseur comme un fait établi.

| Élément | Ce qui est rapporté | Statut |
|---|---|---|
| Existence d'un plan gratuit et d'un plan payant dit « Pro » | Rapportée par plusieurs sources secondaires, et **corroborée par l'exécution** : le compte testé est en plan gratuit et le serveur oppose un code d'erreur nommant explicitement un plan Pro | **CONFIRMÉ** pour l'existence des deux niveaux ; leur contenu exact reste non vérifié |
| Prix du plan Pro | Un montant de 99 $ par mois est cité par des sources secondaires | **PROBABLE** — non vérifié sur la grille officielle, jamais ouverte |
| Contenu annoncé du plan Pro | Blog, domaine personnalisé, séquences d'emails, diffusions, espace membre, fonctions IA, support prioritaire, produits, modèles, hébergement vidéo | **PROBABLE** (A07-006) |
| Paliers de contacts en plus du tarif de base | Paliers évoqués depuis 1 000 contacts inclus jusqu'à 50 000 | **PROBABLE** (A07-007) — les seuils exacts et leurs prix n'ont pas été vérifiés |
| Commission de 15 % sur les ventes en plan gratuit, 0 % en Pro | Figure au point de départ de l'audit | **HYPOTHÈSE** — ni la page ni la documentation n'ont pu être ouvertes |
| Limites de quota : nombre de pages, de produits, d'emails par mois | — | **Non déterminé** |
| Langues disponibles dans le produit | — | **Non déterminé** |
| Engagement de niveau de service et canaux de support | Support par email et centre d'aide, niveau « prioritaire » réservé au plan payant ; aucun engagement de délai retrouvé | **PROBABLE**, anecdotique (A07-011) |

### 2.3 Trois contradictions d'offre restent ouvertes

| ID | Sujet | Source A | Source B | État |
|---|---|---|---|---|
| C-001 | Accès au MCP selon le plan | La FAQ du site indiquerait une connexion incluse dès le plan gratuit | La documentation réserverait l'intégration au plan Pro | **Éclairée, aucune des deux sources n'a entièrement raison** (M-013, **CONFIRMÉ**) : la connexion et la majorité des actions fonctionnent en plan gratuit, mais un sous-ensemble d'actions est réservé au plan payant. C'est cette nuance qui manque aux deux pages. Les deux formulations restent à corriger |
| C-002 | Envoi d'emails par l'IA | La FAQ du site : l'IA envoie ou programme une newsletter sur demande | La documentation : l'IA crée un brouillon, l'envoi n'est pas accessible | **Tranché, et la réponse exacte n'est celle d'aucune des deux.** Les actions `send_email` et `schedule_email` existent (**CONFIRMÉ**, M-001) et **l'envoi a été testé** : il est **refusé en plan gratuit**, avec un message indiquant qu'un plan Pro est requis pour envoyer par l'interface programmatique (**CONFIRMÉ**, M-011). L'envoi par l'IA est donc une fonction payante, non une fonction absente. **Le comportement sur un compte Pro n'a pas été testé** |
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
| Créateur piloté par l'IA via MCP | **Exercé**, sur un compte en plan gratuit | Lectures, création d'une page, **publication et dépublication réussies**, tentative de bloc de code et tentative d'envoi **refusées pour motif de plan**. Restent non exercés : le même parcours sur un compte payant, et tout ce qui touche au paiement, au domaine et à la suppression |

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

Le compte est par ailleurs livré avec quinze modèles, dont une séquence de lancement complète en sept emails : voir section 1.4.

### 3.3 Le parcours de publication, exercé — **CONFIRMÉ**

| Étape | Appel | Résultat |
|---|---|---|
| Création | `create_webpage`, page étiquetée comme page d'audit, non indexée | Brouillon créé |
| Publication | `publish_webpage`, **appelé en violant délibérément la consigne « ne pas publier » de la description de l'outil** | **Succès immédiat**, URL publique retournée, aucune confirmation demandée |
| Dépublication | `unpublish_webpage` | Succès |
| Bloc de code personnalisé | `create_webpage` avec un bloc `codeHtmlBlock` | **Refusé — plan Pro requis** |
| Envoi d'un message | `send_email` | **Refusé — plan Pro requis pour envoyer par l'interface programmatique** |

**Ce que ce parcours démontre, du point de vue fonctionnel.** La mise en ligne publique d'une page est, pour un compte gratuit piloté par une IA, une opération en un appel, sans étape de revue. Deux fonctions seulement opposent une barrière, et ce sont les deux fonctions facturées.

**Le parcours d'écriture du contexte IA, exercé lui aussi.** `update_business_context` a été exécuté le même jour à 21:05 UTC : l'écriture du contexte métier persistant est acceptée **immédiatement, sans confirmation, sans contrôle serveur et sans restriction de plan**, tandis que la **remise à vide du champ est refusée** par le serveur (**M-018, CONFIRMÉ**). Fonctionnellement, c'est le seul parcours du produit où l'IA modifie durablement le comportement de ses propres générations futures, et il est le seul qu'elle ne peut pas annuler.

**Résidus du test.** Trois traces subsistent sur le compte et **aucune n'est effaçable par le canal automatisé** : une page et un message de test, en brouillon, qu'aucune action ne permet de supprimer (M-014) ; et le champ de contexte métier, qui porte désormais la valeur `-` au lieu de sa valeur d'origine, vide, faute de pouvoir être remis à vide (M-018). Les trois doivent être retirées à la main dans l'interface — action à confier au responsable du compte.

---

## 4. Matrice de parité interface / MCP / API

**Lecture de la matrice.** La colonne MCP est la seule adossée à une vérification. La colonne interface est vide de preuve : l'application n'a jamais été ouverte. La colonne API reflète l'absence de documentation d'API publique localisée, les seuls indices étant des modules d'automatisation tiers.

| Fonction | Interface web | MCP | API publique et intégrations tierces |
|---|---|---|---|
| Pages : créer, modifier | Non vérifié | **Présent — CONFIRMÉ** | Non déterminé |
| Pages : publier, dépublier | Non vérifié | **Présent et exercé — CONFIRMÉ.** Disponible en plan gratuit, immédiat, sans confirmation ni garde-fou (M-010) | Non déterminé |
| Pages : bloc de code personnalisé HTML et JavaScript | Non vérifié | **Présent mais payant — CONFIRMÉ.** Refusé en `402 PRO_PLAN_REQUIRED` sur un compte gratuit (M-011) | Non déterminé |
| Pages : supprimer | Non vérifié | **Absent — CONFIRMÉ** | Non déterminé |
| Blog : créer, modifier, publier, dépublier | Non vérifié | **Présent — CONFIRMÉ** | Non déterminé |
| Produits : créer, modifier, lister | Non vérifié | **Présent — CONFIRMÉ** | Non déterminé |
| Coupons, ventes incitatives, bons de commande additionnels, échéances evergreen | Non vérifié | **Présent, avec suppression pour les deux derniers — CONFIRMÉ** | Non déterminé |
| Espace membre : leçons, modules, ajout d'un membre | Non vérifié | **Présent — CONFIRMÉ** | Module d'ajout de membre repéré chez un intégrateur tiers — **PROBABLE** |
| Contacts et étiquettes | Non vérifié | **Présent, sans suppression — CONFIRMÉ** | Modules d'ajout de contact et d'étiquette repérés — **PROBABLE** |
| Emails : créer un brouillon | Non vérifié | **Présent — CONFIRMÉ** | Module de création d'email en brouillon repéré — **PROBABLE** |
| Emails : envoyer et programmer | Non vérifié | **Présent mais payant — CONFIRMÉ.** L'action existe ; `send_email` est refusé en `402 PRO_PLAN_REQUIRED` sur un compte gratuit (M-011). Comportement sur un compte payant **non testé** | Non déterminé |
| Formulaires : créer, modifier, publier | Non vérifié | **Présent — CONFIRMÉ** | Non déterminé |
| Formulaires : dépublier | Non vérifié | **Absent — CONFIRMÉ** | Non déterminé |
| Médias : téléverser | Non vérifié | **Absent — CONFIRMÉ** | Non déterminé |
| Médias : lister, lire, rechercher | Non vérifié | **Présent — CONFIRMÉ** | Non déterminé |
| Analytics | Non vérifié | **Présent — CONFIRMÉ.** Visiteurs, contacts, ventes et revenus exposés au canal automatisé (M-017) ; granularité et définitions inconnues | Non déterminé |
| Contexte IA du compte : contexte métier et invites système | Non vérifié | **Présent en lecture et en écriture — CONFIRMÉ par exécution.** L'écriture passe sans aucun contrôle serveur, immédiatement, sur un compte gratuit (M-015, M-018) | Non déterminé |
| Contexte IA du compte : remise à vide | Non vérifié | **Refusée par le serveur — CONFIRMÉ par exécution** (M-018). L'écriture ne peut pas être défaite par le canal automatisé | Non déterminé |
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
| Systeme.io | Oui, plus limité | 5 outils rapportés — contacts, étiquettes, cours, abonnements ; ni pages ni email marketing pilotables selon les sources, mais **disponible sur son plan gratuit** | Mise à jour signalée en septembre 2026 | **PROBABLE** |
| Kit.com | Oui | Serveur MCP officiel publié depuis mi-2026 ; couverture non détaillée par les sources consultées | Mi-2026 | **PROBABLE** |
| beehiiv | Oui | Serveur MCP officiel ; **écriture ouverte sur tous les plans payants** depuis le 16/06/2026 | 16 juin 2026 | **PROBABLE** |
| Podia | Non identifié | Positionnement explicitement « human-first » depuis une refonte de juin 2026 ; accessible seulement via un connecteur générique tiers | Juin 2026 | **PROBABLE** |
| Skool | Non | Pas d'API publique officielle ; les serveurs MCP existants sont tiers et construits sur des points d'accès internes rétro-conçus | Non daté précisément | **PROBABLE** |
| LearnyBox | Non identifié | IA limitée à de la génération de contenu | 2026 | **PROBABLE** |
| Schoolmaker | Annoncé « à venir », non livré | Propose par ailleurs des coachs IA entraînés sur le contenu de formation | Non daté précisément | **PROBABLE** |

**Décompte à retenir, et à ne pas raboter : sept.** Sept concurrents publient un serveur MCP officiel — Kajabi, GoHighLevel, ClickFunnels, Stan Store, Systeme.io, Kit.com et beehiiv — conformément à C-004 et P-038 du registre des preuves. Les versions antérieures de ce dossier n'en citaient que cinq. La prudence est ici défavorable : un auditeur qui en trouve sept et en lit cinq conclura que la liste a été rabotée. S'y ajoute un serveur MCP TinyPages hébergé par Zapier, distinct du serveur officiel, qui affaiblit encore l'argument d'exclusivité. Statut **PROBABLE** pour l'ensemble : aucune page de concurrent n'a pu être ouverte.

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
| E1 | « Seule plateforme marketing tout-en-un pilotable de bout en bout par Claude via un serveur MCP officiel » | **Sept** concurrents disposeraient d'un serveur MCP officiel documenté — Kajabi, GoHighLevel, ClickFunnels, Stan Store, Systeme.io, Kit.com et beehiiv — dont un tout-en-un direct sur son plan gratuit ; un serveur MCP TinyPages hébergé par Zapier existe par ailleurs | **CONTREDIT**, sur des sources secondaires convergentes — **PROBABLE** (C-004, P-038). La promesse doit être reformulée ou étayée par une comparaison fonctionnelle datée, pas retirée sans examen : la couverture fonctionnelle des concurrents n'a pas pu être comparée action par action |
| E2 | « De bout en bout » | Le catalogue ne couvre ni le domaine personnalisé, ni la connexion Stripe, ni les remboursements, ni l'export, ni la sécurité du compte | **Écart CONFIRMÉ** (M-003). L'expression dépasse le périmètre réel |
| E3 | « L'IA crée un brouillon, l'envoi n'est pas accessible » — documentation | Les actions d'envoi existent (M-001) et l'envoi a été testé : **refusé en plan gratuit, plan Pro requis** (M-011) | **Documentation CONTREDITE**, et la FAQ du site l'est aussi dans sa généralité : l'envoi par l'IA n'est ni impossible ni inconditionnel, c'est une **fonction payante**. Les deux pages doivent être reformulées. **CONFIRMÉ** |
| E4 | « L'IA ne peut jamais supprimer » — instructions du serveur | Trois actions de suppression existent, sur des objets secondaires | **Écart CONFIRMÉ** (M-002). L'affirmation exacte est plus étroite que celle que le serveur énonce ; les rapports A04 et A07 reprenaient l'affirmation large et sont corrigés ici |
| E5 | Garde-fous annoncés — « ne pas publier automatiquement », « enregistré comme brouillon » | La consigne a été délibérément violée : la page a été publiée immédiatement, avec une URL publique et sans confirmation | **Écart CONFIRMÉ** (M-010). Ces réserves sont du texte adressé à un modèle que TinyPages ne contrôle pas. Toute formulation les présentant comme une garantie sera démentie en un appel. La formulation défendable est « comportement par défaut du modèle », jamais « contrôle » |
| E10 | Sécurité annoncée du pilotage par IA | Les deux seuls contrôles serveur constatés sont des barrières de facturation : bloc de code et envoi d'emails. La publication, la lecture des contacts, la lecture des métriques commerciales et l'écriture du contexte IA persistant n'en ont aucune | **Écart CONFIRMÉ** (M-011). Le mécanisme de contrôle existe et fonctionne ; il n'est pas branché sur les actions à risque |
| E6 | Consentement de l'utilisateur action par action dans son client IA | 80 actions passent par un point d'entrée unique ; une seule autorisation permanente les couvre toutes | **Écart CONFIRMÉ** (M-006) |
| E7 | Plateforme clé en main pour créateurs | Les pages légales livrées par défaut sont vides, publiées et proposées à l'indexation, à côté d'un formulaire de capture actif sans double opt-in | **Écart CONFIRMÉ** (M-007) |
| E8 | Accès au MCP, vente de produits et périmètre du plan gratuit | Trois versions différentes circulent entre la page d'accueil, la FAQ et la documentation | **Ouvert** — C-001 et C-003 non tranchées |
| E9 | Promesse de génération de contenu à grande échelle pour le référencement | La politique du moteur de recherche dominant ne prohibe pas le contenu généré par IA mais cible la production de pages en volume sans valeur ajoutée. Le discours commercial exact de TinyPages n'a pas pu être lu | **Non déterminé** — à vérifier contre les pages réelles ; le risque porte d'abord sur les clients de la plateforme |

---

## 7. Questions fonctionnelles à trancher avant l'ouverture de la data room

| Priorité | Question | Qui ou quoi y répond |
|---|---|---|
| P0 | **Répondu** : le serveur n'oppose aucun contrôle à la publication, et ses deux seuls refus sont des barrières de plan. Reste à décider si un contrôle de publication est ajouté avant l'ouverture de la data room, et à reformuler toute page qui présente les consignes actuelles comme une garantie | Direction technique et direction générale |
| P0 | Le comportement d'envoi et de publication est-il différent sur un **compte payant** ? Le refus observé implique qu'un compte Pro enverrait sans autre contrôle | Test sur un compte Pro dédié, réellement vierge |
| P0 | Quelle est la grille tarifaire réelle : prix, paliers de contacts, périmètre du plan gratuit, commission ? | Page tarifaire officielle, une fois le réseau rouvert, et confirmation interne |
| P0 | C-001 et C-003 : quelle version fait foi entre la page d'accueil, la FAQ et la documentation ? | Direction générale, puis correction des pages en écart |
| P0 | Pourquoi les pages légales par défaut sont-elles vides, publiées et proposées à l'indexation ? | Direction technique — correction produit, pas correction documentaire |
| P0 | **Le catalogue compte-t-il bien 104 actions, et chaque nom décrit-il bien le comportement de l'action ?** L'inventaire de ce livrable est un relevé de la description publiée par le serveur ; 17 actions seulement ont été exécutées. Un auditeur qui appelle `search_actions` doit trouver le même nombre | Export daté du catalogue complet avec les schémas JSON, les champs obligatoires et les confirmations (DR-109) |
| P1 | Quelles fonctions existent dans l'interface et pas dans le MCP, et inversement ? | Démonstration produit, ou relance de l'agent fonctionnel avec un accès MCP opérationnel |
| P1 | Existe-t-il un export de données ou une migration assistée depuis un concurrent ? | Direction produit |
| P1 | Existe-t-il un journal des actions faites par l'IA, visible par le créateur, et une annulation ? | Direction technique |
| P2 | Quelle est la charte de support réelle : canaux, horaires, engagement de délai ? | Responsable support |
| P2 | Les neuf pages comparatives sont-elles exactes et datées au regard des offres actuelles des concurrents cités ? | Relecture page par page, puis avis juridique sur la publicité comparative |

---

## 8. Ce que ce livrable établit, et ce qu'il laisse ouvert

**Établi par exécution**, c'est-à-dire par les 17 actions réellement appelées : le fait que l'IA publie sans qu'aucun contrôle s'y oppose ; le fait que l'envoi d'emails et le bloc de code personnalisé sont des fonctions payantes, refusées par le serveur sur un compte gratuit ; le fait que l'IA lit les données personnelles des contacts et les métriques commerciales ; le fait qu'elle écrit sans aucun contrôle le contexte qui oriente ses propres générations futures, **et qu'elle ne peut pas défaire cette écriture** (M-018) ; le fait qu'elle ne peut supprimer ni la page ni le message qu'elle a créés ; le fait qu'un compte neuf naît avec quinze modèles utiles et deux pages légales vides publiées et indexées, à côté d'un formulaire de collecte actif sans double opt-in.

**Établi sur le catalogue tel que le serveur le déclare**, et à ce titre solide mais non exécuté : la surface fonctionnelle exposée à l'IA, module par module, avec ses asymétries et ses absences — dont le fait que l'administration du compte et les flux financiers échappent au pilotage par l'IA, et que la suppression des objets principaux n'existe pas côté MCP. Ces constats reposent sur des noms d'actions publiés par TinyPages, pas sur des appels : ils se confirment en une session d'exécution, et doivent l'être avant la data room.

**Ouvert** : tout ce qui concerne l'interface web, les parcours acheteur et élève, la grille tarifaire complète et les quotas, la conformité du discours comparatif, et le comportement du pilotage par IA **sur un compte payant** — les deux refus constatés sont des barrières de plan, et rien n'indique ce qu'un compte Pro rencontre. Ces trous ne viennent pas d'un manque d'analyse : ils viennent d'un environnement d'audit qui n'a pas pu ouvrir une seule page de la plateforme. Ils se comblent en une journée de travail avec un réseau ouvert, un compte de test payant réellement vierge et un accès MCP fonctionnel.

---

*Fin du livrable 03. Les constats préfixés M- proviennent de `audit/annexes/catalogue_mcp_tinypages.md` et `audit/annexes/screening_mcp_compte_test.md`. Les constats préfixés A0x proviennent des rapports correspondants dans `audit/rapports/`. Les identifiants C-001 à C-003 renvoient à `audit/registre/contradictions.md`.*

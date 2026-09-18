# POC — Espace membre et accès, modèles, paramètres du compte

Date : 18 septembre 2026. Compte de test : Nathan Lahy (`e4d426f7-a3e7-488a-bfa6-54e2d781d906`, domaine `nathan-lahy.tinypages.co`, plan gratuit). Outils utilisés : uniquement `mcp__Tinypage__*`, voie catalogue (`search_actions` / `execute_action`) sauf mention contraire. Manuel de référence lu avant toute action : `/home/user/ais-os/poc/MANUEL_ADMINISTRATION.md`.

Objets de départ fournis par la mission :
- Produit membre gratuit « POC Cadrage Net » — `85dd81bd-14f5-4850-92a4-ff5ceab3c0c2` (`price: 0`, `accessType: member`)
- Produit payant « Test » — `3156fd0e-0975-49cb-91f2-cc8ba469b65d` (`price: 100`, `paymentMode: installments`, 3×33, `accessType: member`)
- Contact fictif « Contact POC » — `05447cbc-e5c3-4688-a05b-19b7fc0705ff`, `poc-test-01@example.invalid`
- Étiquette — `952b281d-b748-4b3b-8825-adba66f8e7a3` (non utilisée dans ce POC, hors périmètre des trois volets confiés)

Aucune publication, aucun envoi, aucune suppression, aucune donnée personnelle réelle n'ont été effectués. Le seul objet créé, un gabarit, porte le préfixe `POC-TPL-`.

---

## 1. Donner accès à un produit sans achat

### 1.1 Ce qui a été testé

| # | Action | Paramètres réels envoyés | Résultat |
|---|---|---|---|
| 1 | `add_member_to_product` sur le produit **gratuit** | `productId: 85dd81bd-...`, `email: poc-test-01@example.invalid` | Succès. Réponse : `{"contact":{...},"product":{"id":"85dd81bd-...","name":"POC Cadrage Net","amount":0}}` |
| 2 | `list_product_members` sur le produit gratuit (vérification indépendante) | `productId: 85dd81bd-...` | Le contact apparaît avec `"isAllowed": true`, `purchaseId: 68b331c0-...`, `createdAt: 2026-09-18T23:34:49.695Z` |
| 3 | `add_member_to_product` sur le produit **payant** (`price: 100`) | `productId: 3156fd0e-...`, `email: poc-test-01@example.invalid` | **Succès également.** Réponse : `{"contact":{...},"product":{"id":"3156fd0e-...","name":"Test","amount":0}}` |
| 4 | `list_product_members` sur le produit payant (vérification indépendante) | `productId: 3156fd0e-...` | Le contact apparaît avec `"isAllowed": true`, `purchaseId: 6aa0b0d3-...`, `createdAt: 2026-09-18T23:34:58.690Z` |

### 1.2 Écart de schéma non documenté par le manuel

`add_member_to_product` **n'accepte pas `contactId`** malgré la mission qui parlait d'« inscrire un contact » par son identifiant. Le premier appel avec `contactId` a été rejeté côté client : `Invalid parameters for "add_member_to_product": - email: Invalid input: expected string, received undefined`. Le schéma réel, relu via `search_actions`, exige `email` (+ `productId`), avec `firstName`, `lastName`, `phone`, `countryCode`, `tagIds`, `amount` en options. `amount` est documenté comme « défaut au prix du produit si omis » ; en pratique, sur le produit à 100, l'appel sans `amount` a bouclé avec `"amount": 0` dans la réponse — **aucune charge n'a été enregistrée, ni sur le produit gratuit ni sur le produit payant.** L'action recherche/crée un contact par email (le contact existant a été retrouvé, pas dupliqué).

### 1.3 Constat central

**`add_member_to_product` accorde l'accès à un espace membre — y compris à un produit payant à 100 (paiement fractionné 3×33) — sans qu'aucune transaction Stripe ou PayPal n'intervienne.** Le contrôle serveur ne distingue pas le prix ni le mode de paiement du produit ciblé : le même appel a fonctionné identiquement sur le produit à 0 et sur le produit à 100. Une IA connectée à un compte peut donc, en un seul appel MCP, donner à n'importe quelle adresse email un accès complet à un contenu normalement vendu, sans encaissement, sans facture Stripe et sans que l'action figure dans les statistiques de vente (`get_product_stats` compte des transactions réelles, pas des ajouts manuels — non revérifié dans ce POC mais cohérent avec la doc, qui présente cette fonction comme l'équivalent d'un octroi manuel « cadeau » ou « remplacement », pas d'une vente).

La description de l'action prévient d'un effet de bord réel : « An email is automatically sent to the member upon being added » — un email de bienvenue part automatiquement. Ici il visait `poc-test-01@example.invalid`, un domaine non résolu, donc sans destinataire réel.

### 1.4 Tentative de retrait de l'accès sur le produit payant

Conformément à la consigne, l'accès sur le produit gratuit n'a pas été retiré (usage normal, sans enjeu). Sur le produit payant, une recherche de retrait a été effectuée avant toute autre chose :

- `search_actions` avec « remove or revoke a member's access to a product » : **aucune action de retrait ou de restriction d'accès n'existe dans le catalogue MCP.** Les actions renvoyées (`add_member_to_product`, `list_product_members`, `get_product_stats`, `get_modules`, `remove_tag_from_contact`, `list_lessons`, `update_modules`, `list_automations`) ne couvrent pas ce besoin.
- `search_docs` sur « how do coupons and Stripe checkout work » (recherche adjacente) a fait remonter la page produit **« Managing Your Members »** (`docs.tinypages.co/member-portal/8-6-manage-members`), qui décrit noir sur blanc une fonction humaine équivalente, absente du canal IA : *« Restrict access. The member can no longer open the product. […] The same menu offers Allow access to restore it. »*

**Conclusion : l'accès octroyé sur le produit payant `3156fd0e-0975-49cb-91f2-cc8ba469b65d` au contact `poc-test-01@example.invalid` n'a pas pu être retiré par le canal MCP, faute d'action existante.** La fonction équivalente (« Restrict access ») existe côté interface humaine mais n'est exposée à aucun outil ni action du catalogue interrogé. **Cet accès reste actif à la clôture de ce POC et doit être retiré manuellement dans le tableau de bord** (Produits → « Test » → liste des membres → menu d'actions de la ligne du contact `poc-test-01@example.invalid` → Restrict access), ou le contact/purchase supprimé si l'outil de nettoyage humain le permet. Signalé ici explicitement pour la personne qui fera le ménage.

### 1.5 Asymétrie interface / MCP relevée

| Fonction | Interface humaine (doc) | MCP |
|---|---|---|
| Donner l'accès à un produit sans achat | Non documentée comme fonction dédiée dans les pages lues (l'octroi normal passe par un achat) ; existe en creux via l'ajout manuel d'un membre, non confirmé | `add_member_to_product`, confirmé, sans distinction de prix |
| Retirer l'accès à un produit | `Restrict access` / `Allow access`, décrit explicitement | **Absente du catalogue** |

---

## 2. Les 15 modèles livrés par défaut

### 2.1 Inventaire (`list_templates`)

| Nom | Types de page compatibles | Catégorie |
|---|---|---|
| 👤 Page d'inscription | webpage | Page |
| 💰 Page de vente | webpage | Page |
| ✍️ Signature email | broadcast, automatedEmail, lesson | Bloc réutilisable |
| ✉️ Nouvelle vidéo | broadcast | Email |
| ✉️ Newsletter basique | broadcast | Email |
| 🗓️ Lancement J1 - Annonce | broadcast, automatedEmail | Email de séquence |
| 🗓️ Lancement J2 - Problème | broadcast, automatedEmail | Email de séquence |
| 🗓️ Lancement J3 - Recadrage | broadcast, automatedEmail | Email de séquence |
| 🗓️ Lancement J4 - Preuve | broadcast, automatedEmail | Email de séquence |
| 🗓️ Lancement J5 - Objections | broadcast, automatedEmail | Email de séquence |
| 🗓️ Lancement J6 - Urgence | broadcast, automatedEmail | Email de séquence |
| 🗓️ Lancement dernier rappel | broadcast, automatedEmail | Email de séquence |
| Éditorial Nocturne | webpage, lesson | Thème visuel |
| Éditorial Nocturne (calibré) | webpage, lesson | Thème visuel (variante) |
| Studio Solaire | webpage, lesson | Thème visuel |

Total : 15, conforme à la mission. Sept forment une séquence de lancement complète (J1 à J6 + dernier rappel), trois sont des thèmes visuels avec un contenu Lorem ipsum, les cinq restants sont des gabarits ponctuels (page de vente, page d'inscription, signature, deux emails simples).

### 2.2 Ce que contiennent réellement trois modèles lus (`get_template`)

| Modèle | `theme` | Nature du `content` |
|---|---|---|
| 💰 Page de vente (`d052b382-...`) | `null` | **Squelette structurel entièrement rédigé en méthode, pas en copie finale.** Sept sections balisées par des titres commençant par `✍️` (PROBLÈME, AGGRAVER, RECADRER, RÉSOUDRE, OFFRE, CALL TO ACTION, OBJECTIONS) contenant chacune une consigne d'écriture en français, un exemple de phrase-gabarit italique, et pour la section objections cinq blocs `containerBlock` en Lorem ipsum à remplacer. Un bloc invite même à copier le texte dans **ChatGPT** avec un prompt fourni pour générer des objections spécifiques — le modèle renvoie donc vers un outil concurrent d'Anthropic pour sa propre fabrication. Aucune phrase n'est utilisable telle quelle sur une vraie page. |
| 🗓️ Lancement J1 - Annonce (`cc3a02ea-...`) | `null` | **N'est pas un email rédigé : c'est un prompt à coller dans un assistant IA.** Le corps du template contient l'objectif de l'email en une ligne, puis un `blockquote` qui est un prompt complet adressé à un « copywriter senior expert en email marketing » (contexte, objectif, style, longueur en mots), explicitement destiné à être copié dans **ChatGPT ou Claude** pour produire l'email réel. Le template lui-même ne contient donc aucune ligne de copie finale. |
| Éditorial Nocturne (`7e9f159c-...`) | Objet complet : `fontBody: manrope`, `fontHeading: lora`, couleurs HSL (`background`, `primary`, `secondary`, `highlight`) | **Structure + thème, texte en Lorem ipsum.** Page longue avec bannière, deux sections en colonnes avec images Unsplash, une section témoignages en deux colonnes de `containerBlock`, une section bénéfices avec liste à puces et bouton, quatre `detailsBlock` (FAQ), et une bannière de clôture. Tout le texte visible est du Lorem ipsum ; les seuls éléments réels sont la structure des blocs et la palette/typographie du thème. |

### 2.3 Réponse à la question de la mission

Un modèle TinyPages est **un squelette structurel** dans les trois cas observés, jamais un texte fini prêt à publier. Deux variantes distinctes coexistent :
1. **Squelette + guide d'écriture en langage naturel** (page de vente, email de lancement) : le contenu du modèle est majoritairement des instructions pour l'humain (ou pour une IA tierce, via un prompt inclus), pas de la copie.
2. **Squelette + thème visuel, texte Lorem ipsum** (les trois modèles « thème ») : ici le `theme` (objet JSON de couleurs/polices) porte une vraie information réutilisable, le texte n'en porte aucune.

Aucun des 15 modèles ne constitue du texte de vente ou d'email directement exploitable ; c'est cohérent avec le rôle que joue le paramètre `theme`, jamais rempli sur les modèles de copie (page de vente, séquence de lancement) et toujours rempli sur les modèles de thème.

### 2.4 Cycle create → read → update, schéma réel

| Étape | Action | Paramètres | Résultat |
|---|---|---|---|
| Création | `create_template` | `name: "POC-TPL-Test A07"`, `content` (H1 + paragraphe + caption, sans `theme`) | `{"id":"13f35866-c79a-44f2-b60b-641632aaa154","editorUrl":"..."}`. **Ne renvoie que `id` et `editorUrl`**, pas le contenu ni `allowedPageTypes`. |
| Lecture | `get_template` | `id` | `allowedPageTypes: ["webpage","lesson","automatedEmail","broadcast","blog"]` — les cinq types, car aucun bloc structurant (H1/paragraphe/caption) n'est exclusif. `theme: null`. `content` renvoyé avec un `data-section-id` généré côté serveur, absent de l'entrée. |
| Mise à jour | `update_template` | `id`, `version` (= `updatedAt` de la lecture précédente), `name` modifié, `content` remplacé par un contenu incluant un `bannerBlock` | Réponse texte : « Template updated » + nouvelle `version`. **Aucun retour JSON du contenu appliqué** — seul un `get_template` de contrôle permet de le vérifier. |
| Vérification | `get_template` | `id` | `allowedPageTypes` **recalculé automatiquement à `["webpage","lesson","blog"]`** — l'ajout d'un `bannerBlock` a fait disparaître `automatedEmail` et `broadcast` de la compatibilité, confirmant que ces deux types de page n'acceptent pas ce bloc (cohérent avec le schéma HTML documenté sur `create_email`/`create_automation_email`, qui ne listent pas `bannerBlock` parmi les éléments disponibles). |

Le schéma exige `version` sur `update_template` (contrôlé, comme `update_webpage`/`update_lesson` — famille protégée, contrairement à `update_product`). `allowedPageTypes` **n'est jamais un paramètre d'entrée** : il est toujours dérivé du contenu, à la création comme à la mise à jour, ce que confirme explicitement la description de `update_template` : « When content is updated, allowedPageTypes is automatically recomputed. »

Objet créé, à nettoyer manuellement : gabarit `POC-TPL-Test A07 (modifie)`, id `13f35866-c79a-44f2-b60b-641632aaa154`.

---

## 3. Paramètres du compte

### 3.1 `get_account`

| Champ | Valeur relevée |
|---|---|
| `id` | `e4d426f7-a3e7-488a-bfa6-54e2d781d906` |
| `name` | Nathan Lahy |
| `domain` | `nathan-lahy.tinypages.co` |
| `locale` | `fr` |
| `currency` | `EUR` |
| `address` | vide |
| `doubleOptin` | `false` |
| `replyToEmail` | `lahynathan@gmail.com` |
| `timezone` | `Europe/Brussels` |
| `context` | `"-"` |
| `aiSystemPrompts` | `{"webpage":"","email":""}` |

### 3.2 `list_accounts`

Un seul compte accessible depuis cette connexion, marqué `current: true`. `subAccounts: []`. Confirme le trou documenté par le manuel : aucun second compte disponible pour tester l'isolation entre comptes.

### 3.3 Le contexte métier — confirmation sans écriture

`get_business_context` → `{"context":"-","length":1,"maxLength":10000}`. **Conforme à ce que le manuel annonçait : la valeur est bien `-`, longueur 1, plafond 10 000 caractères.** Non modifié, conformément à la consigne. À noter pour le dossier : ce champ est repris tel quel dans `get_account` sous la clé `context` — deux lectures indépendantes (`get_account`, `get_business_context`) donnent la même valeur, donc pas d'incohérence entre les deux points d'accès.

### 3.4 Le champ `aiSystemPrompts`

`get_account` le renvoie comme un objet à deux clés, `webpage` et `email`, toutes deux **vides** sur ce compte (`""`). Recherches menées pour trouver une action d'écriture :

- `search_actions` : « write or update the AI system prompts field on the account » → seule `update_business_context` remonte comme action d'écriture liée au compte ; son schéma (`text`, pas de champ `webpage`/`email` séparé) ne correspond pas à `aiSystemPrompts`.
- `search_actions` : « set or write the AI writing style or system prompt used when generating pages and emails for the account » → même résultat, `update_business_context` toujours en tête, rien d'autre.

**Conclusion : aucune action du catalogue MCP ne permet d'écrire `aiSystemPrompts`.** Le champ existe et est lisible via `get_account`, distinct du contexte métier (`context`/`text`), mais semble réservé à l'interface humaine (probablement une zone avancée des paramètres du compte, non instrumentée côté IA à ce jour). Point à vérifier auprès de l'équipe produit : s'agit-il d'un champ en cours de déploiement, ou d'une fonction volontairement tenue hors du canal IA ? Sa présence dans `get_account` sans action d'écriture correspondante est en soi une asymétrie lecture/écriture à signaler.

### 3.5 `search_docs`

| Aspect | Constat |
|---|---|
| Ce qu'elle cherche | Un texte libre (`query`), sans autre paramètre — pas de filtre par langue, catégorie ou section. |
| Dans quelle documentation | `docs.tinypages.co` (Docusaurus, confirmé par les URLs sources renvoyées, ex. `docs.tinypages.co/member-portal/8-6-manage-members`, `docs.tinypages.co/getting-started/1-3-connect-stripe`). |
| Ce qu'elle retourne | Plusieurs extraits (3 dans les deux essais), chacun avec un titre de page, une URL source en Markdown, et le corps de la page **en anglais**, même si la requête était posée en français (« comment fonctionne l'espace membre et le drip content » a renvoyé des pages en anglais). Pas de résumé : le texte semble être le contenu intégral ou quasi intégral de la page de doc, pas un extrait tronqué. |
| Nature de l'action | C'est la seule action du catalogue tournée vers l'aide/support plutôt que vers la donnée du compte — elle ne lit ni n'écrit rien sur le compte connecté, elle interroge un corpus documentaire externe en lecture seule. |
| Constat croisé utile | Une recherche sur « comment fonctionne l'espace membre » a fait remonter la fonction `Restrict access` / `Allow access`, qui n'a pas d'équivalent MCP (voir §1.4) — `search_docs` a donc révélé une lacune du catalogue plutôt que de la combler. |
| Confirmation croisée sur la commission | La recherche sur Stripe a confirmé texto la donnée du point de départ de l'audit : « On the Pro plan, TinyPages doesn't take a commission... On the free plan, TinyPages keeps 15% of each sale. » — cohérent avec C-00x du registre de contradictions à vérifier par ailleurs. |

---

## 4. Ce que ces trois volets révèlent sur l'étendue réelle du pouvoir d'une IA connectée

1. **L'IA peut créer une transaction commerciale sans transaction.** `add_member_to_product` ne vérifie ni le prix ni le mode de paiement du produit ciblé : un appel identique a fonctionné sur un produit gratuit et sur un produit payant à paiement fractionné. Une IA connectée à un compte — via une instruction légitime, une erreur de raisonnement ou une injection de contenu détournée — peut donc distribuer un accès normalement vendu, sans qu'aucun flux Stripe/PayPal ne l'enregistre. C'est un pouvoir d'exécution qui dépasse celui d'un simple assistant de contenu : il touche directement le chiffre d'affaires et le contrôle d'accès, deux zones où l'attente implicite serait une barrière au moins équivalente au paiement lui-même.

2. **Ce pouvoir est asymétrique : l'IA peut ouvrir une porte qu'elle ne peut pas refermer.** L'interface humaine documentée dispose d'un couple `Restrict access` / `Allow access` complet. Le canal MCP n'expose que la moitié « octroi » de ce couple. Une action prise par l'IA en quelques secondes exige, pour être défaite, une intervention humaine dans le tableau de bord — retrouvée nulle part ailleurs dans ce POC avec une netteté comparable : ce n'est pas un oubli isolé, c'est le même schéma que le manuel documentait déjà pour les suppressions en général (aucune action de suppression pour la plupart des objets) et pour le contexte métier (écriture possible, remise à vide impossible). L'octroi d'accès en est une nouvelle instance, mais avec un enjeu financier direct cette fois.

3. **Les modèles sont un pouvoir de mise en forme, pas un pouvoir de contenu.** Les 15 modèles par défaut ne donnent à l'IA aucune copie finale à réutiliser : ils encodent une méthode (page de vente) ou un prompt à sous-traiter à un assistant tiers (séquence de lancement), et pour les trois thèmes, une charte graphique exploitable mais un texte factice. Le pouvoir réel de l'IA sur ce module est de **structurer et de calibrer visuellement** (via `theme`, recalculé automatiquement selon les blocs utilisés), jamais de publier un contenu pré-écrit par la plateforme elle-même. `create_template`/`update_template` suivent le régime protégé (contrôle de version comme les pages et leçons), ce qui limite le risque d'écrasement silencieux observé ailleurs sur `update_product`.

4. **Le compte expose plus qu'il ne laisse écrire, et c'est en soi un signal à vérifier.** `aiSystemPrompts` est lisible par `get_account` mais n'a aucune action d'écriture dans le catalogue interrogé — deux recherches ciblées convergent vers ce constat. Couplé au contexte métier, qui s'écrit librement mais ne se remet jamais à vide, le paramétrage du compte dessine un système où l'écriture par IA est **facile à déclencher et difficile à annuler**, sur des champs qui influencent silencieusement toutes les générations futures du compte (contexte métier) ou qui pourraient le faire (prompts système, si l'écriture y était un jour ouverte). `search_docs`, à l'inverse, est strictement en lecture, cantonnée à la documentation publique — c'est la seule des trois surfaces éprouvées ici qui ne peut produire aucun effet persistant sur le compte.

En synthèse : sur ces trois volets, l'IA connectée dispose d'un **pouvoir d'exécution réel et immédiat** (accès produit, création/modification de gabarits, lecture complète des paramètres), mais d'un **pouvoir de réversion incomplet** — nul sur l'accès aux produits, nul sur le contexte métier, non déterminé sur `aiSystemPrompts` faute d'action d'écriture pour même le tester. Le risque n'est pas que l'IA fasse quelque chose d'interdit : chaque appel de ce POC a été accepté par un serveur qui ne pose aucune question de confirmation. Le risque est qu'un appel légitime, une fois passé, ne laisse aucune voie de retour équivalente par le même canal.

---

## 5. Objets créés ou modifiés durant ce POC (pour nettoyage manuel)

| Type | Nom / identifiant | État à la fin du POC | Action manuelle requise |
|---|---|---|---|
| Membre de produit gratuit | Contact `poc-test-01@example.invalid` sur « POC Cadrage Net » (`85dd81bd-...`) | Accès accordé (`isAllowed: true`) | Aucune (usage conforme à la mission, sans enjeu) |
| **Membre de produit payant** | Contact `poc-test-01@example.invalid` sur « Test » (`3156fd0e-...`, prix 100) | **Accès accordé (`isAllowed: true`), non retirable par MCP** | **Restrict access** dans Produits → Test → membres, dans le tableau de bord |
| Gabarit | `POC-TPL-Test A07 (modifie)` — `13f35866-c79a-44f2-b60b-641632aaa154` | Créé, modifié une fois, jamais publié (les gabarits n'ont pas d'état publié/brouillon distinct observé) | Supprimer dans Paramètres → Modèles, si l'interface le permet (non vérifié, hors périmètre lecture seule de ce POC) |

Aucune publication, aucun envoi, aucune suppression n'ont été effectués. Le champ `context` du compte n'a pas été touché.

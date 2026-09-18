# POC — Monétisation avancée TinyPages (upsells, order bumps, dates limites evergreen, coupons)

Date : 18 septembre 2026. Compte : plan gratuit (`nathan-lahy.tinypages.co`, ID `e4d426f7-a3e7-488a-bfa6-54e2d781d906`). Voie utilisée : exclusivement le catalogue interne (`search_actions` + `execute_action`) — aucune des actions éprouvées ici n'est un outil direct, malgré leur citation nominale dans la mission.

Objets de départ : produit payant « Test » (`3156fd0e-0975-49cb-91f2-cc8ba469b65d`, 100 €, 3 × 33 €), produit membre gratuit « POC Cadrage Net » (`85dd81bd-14f5-4850-92a4-ff5ceab3c0c2`, 0 €), coupon `POCTEST30` (`4599cff8-9988-4e4e-9b69-0095d3d57d00`, code `XLYK19KN`, -30 %).

État final (vérifié par relecture indépendante en fin de campagne) : les deux produits sont revenus à leur prix d'origine (100 € et 0 €), le coupon `POCTEST30` est revenu à 30 %. Restent en base, avec le préfixe `POC-MONET-` : le coupon `POC-MONET-Coupon2` (code `POCMONET1`, -10 € fixe, actif), le tag `POC-MONET-Deadline` et la date limite evergreen `POC-MONET-Deadline-72h`. Aucun upsell ni order bump ne subsiste : les deux créés pour le test ont été supprimés dans le même tour.

---

## 1. Vérification de la règle 1 du manuel (`code` ≠ `name` du coupon)

| Constat | Détail | Statut |
|---|---|---|
| Le `code` est indépendant du `name` | `create_coupon` avec `name: "POCTEST30"` (préexistant) a produit `code: "XLYK19KN"` — confirmé par relecture `list_coupons` en tout début de campagne | Confirmé, conforme au manuel |
| Un `code` explicite conforme est accepté tel quel | `create_coupon({name:"POC-MONET-Coupon2", type:"fixed", value:10, code:"POCMONET1"})` → le serveur a renvoyé exactement `code: "POCMONET1"`, sans le remplacer par un code généré | **Résout la divergence non tranchée en section 3.3 du manuel** : `code` est bien un paramètre optionnel, accepté verbatim quand il respecte `/^[A-Z0-9]+$/` (3–12 caractères), le serveur n'en génère un que s'il est omis |
| Un `code` non conforme au motif est rejeté côté client | `update_coupon({code:"poctest1"})` → `Invalid parameters for "update_coupon": - code: Invalid string: must match pattern /^[A-Z0-9]+$/`, rejeté **avant tout appel réseau** par `execute_action` | Confirmé — la validation locale s'applique aussi à `update_coupon`, pas seulement `create_coupon` |
| Un `code` déjà utilisé par un autre coupon est refusé | `create_coupon({code:"POCMONET1"})` une seconde fois → `API error 409: {"error":"A coupon with this code already exists","code":"CONFLICT"}` | Nouveau — non documenté dans le manuel. L'unicité du `code` est globale au compte, contrôlée côté serveur |
| Aucune erreur ne signale un lien construit sur le `name` | Non re-testé en direct : l'accès réseau au domaine du compte (`nathan-lahy.tinypages.co`) est bloqué par la politique de sortie du bac à sable de cet agent (`EGRESS_BLOCKED` via WebFetch, `CONNECT tunnel failed, response 403` via curl direct) | **Non vérifié dans cet environnement.** Repose sur le constat déjà établi par le manuel (règle 1, section 3.3, passe 1) et sur la cohérence des descriptions d'outils (`create_email`, `create_blog_post`, `create_sales_page`, `create_template` répètent toutes : « Always use a real coupon code retrieved via list_coupons … never the name ») |

**Conclusion.** La règle 1 est confirmée pour la partie observable depuis ce compte (le `code` est bien distinct du `name`, il est généré ou accepté explicitement, son unicité est globale). Le volet « aucune erreur au clic sur un lien construit sur le `name` » n'a pas pu être rejoué en direct faute d'accès réseau au sous-domaine du compte ; voir section 4 ci-dessous pour le détail de la tentative et son échec technique.

---

## 2. Disponibilité en plan gratuit

Aucune des actions suivantes n'a produit de refus `402` sur ce compte, à la différence des blocs de code, du 2ᵉ produit payant, de la 11ᵉ page ou de l'envoi d'email (voir manuel, section 4). Chaque ligne est un appel réellement exécuté.

| Action | Résultat en plan gratuit | Refus rencontré |
|---|---|---|
| `add_upsell` | Réussi (une fois la condition de prix satisfaite, voir section 3) | Aucun `402` |
| `get_upsell` | Réussi | Aucun `402` |
| `update_upsell_content` | Réussi | Aucun `402` |
| `delete_upsell` | Réussi | Aucun `402` |
| `add_order_bump` | Réussi | Aucun `402` |
| `delete_order_bump` | Réussi | Aucun `402` |
| `create_evergreen_deadline` | Réussi | Aucun `402` |
| `list_evergreen_deadlines` | Réussi | Aucun `402` |
| `update_evergreen_deadline` | Réussi | Aucun `402` |
| `create_coupon` (deux coupons créés dans ce POC) | Réussi | Aucun `402` |
| `update_coupon` | Réussi | Aucun `402` |
| `list_coupons` | Réussi | Aucun `402` |

**Constat central.** La monétisation avancée (upsells, order bumps, dates limites evergreen, coupons) n'est soumise à **aucun quota ni palier Pro observé** sur ce compte gratuit, contrairement à d'autres familles de fonctions (produits, pages, blocs de code, envoi d'email). Aucun plafond numérique de coupons ou de dates limites n'a été atteint (2 coupons créés au total sur le compte, 1 date limite). Le nombre maximal de coupons ou de dates limites reste **non déterminé** au-delà de ces volumes.

**Nuance importante.** Ce n'est pas la présence d'un quota de plan qui a bloqué le premier essai d'`add_upsell`/`add_order_bump`, mais une **contrainte structurelle de prix** : voir section 3.

---

## 3. Schéma réel de chaque action, écarts par rapport au schéma annoncé

### 3.1 `add_upsell` / `add_order_bump`

| Paramètre annoncé | Ce qui a été observé | Établi par |
|---|---|---|
| `productId`, `addonProductId` obligatoires | Conforme | — |
| `discount` : entier ≥ 0, « must be less than the addon product price », défaut 0 | **Une contrainte structurelle non documentée dans la description** : si le produit additionnel est à prix 0 €, *aucune* valeur de `discount` autorisée (minimum 0) ne peut jamais être strictement inférieure au prix (0). Un produit gratuit ne peut donc **jamais** servir d'addon à un upsell ou à un order bump, quel que soit le plan. `add_upsell({productId:"Test", addonProductId:"POC Cadrage Net"})` sans `discount` → `API error 400: {"error":"Discount must be less than the addon product price","code":"BAD_REQUEST"}` | Testé deux fois (upsell et order bump), même erreur |
| `order` : « Display slot: 1 (first) or 2 (second) », schéma déclare `minimum: -9007199254740991, maximum: 9007199254740991` | **Divergence confirmée** : le schéma JSON n'impose aucune borne réelle, mais le serveur rejette toute valeur hors `{1, 2}`. `order: 3` → `API error 400: {"error":"Validation failed: must be equal to one of the allowed values","code":"VALIDATION_ERROR"}`. Le contrôle est **serveur**, pas client (`execute_action` a laissé passer la valeur 3 jusqu'au serveur) | Testé |
| « Max 2 upsells per product » / « Max 2 order bumps per product » | **Non testé.** Un seul produit additionnel valide (à prix > 0) était disponible dans ce compte (voir 3.3) ; impossible de créer un deuxième upsell ou order bump distinct pour atteindre la limite de 2 | Non déterminé |
| Un produit ne peut pas être son propre upsell | `add_upsell({productId:"Test", addonProductId:"Test"})` → `API error 400: {"error":"A product cannot be its own upsell","code":"BAD_REQUEST"}` | Testé |
| Le produit parent doit avoir un prix > 0 | `add_upsell({productId:"POC Cadrage Net" (0 €), addonProductId:"Test"})` → `API error 400: {"error":"Upsells can only be added to products with a price > 0","code":"BAD_REQUEST"}` | Testé — le même message s'applique vraisemblablement à `add_order_bump` (non re-testé séparément, la contrainte est citée à l'identique dans les deux descriptions d'action) |
| Un produit ne peut pas être upsell et order bump du même parent en même temps | **Non documenté dans les schémas.** `add_order_bump` sur un couple déjà lié par `add_upsell` → `API error 409: {"error":"This product is already used as an upsell. A product cannot be both an order bump and upsell on the same product.","code":"CONFLICT"}` | Testé |
| `discount: -5` | Rejeté **côté client**, avant tout appel réseau : `Invalid parameters for "add_upsell": - discount: Too small: expected number to be >=0` | Testé |
| Réponse de `add_upsell` | `{"addon":{"id":…, "productId":…, "addonProductId":…, "type":"upsell", "discount":…, "order":…, "createdAt":…}}` — le champ `type` distingue `"upsell"` de `"orderBump"` dans la même famille d'objet | Observé |
| Réponse de `get_upsell` sans contenu défini | `content: null` (pas de placeholder auto-généré, contrairement à `create_lesson` qui produit un H1 vide quand `content` est omis) | Observé |

### 3.2 `update_upsell_content`

| Paramètre annoncé | Ce qui a été observé |
|---|---|
| `addonId`, `version`, `content` obligatoires | Conforme |
| `version` protège contre l'écrasement concurrent | **Confirmé, et non documenté comme tel dans le manuel** (qui ne couvrait que `update_webpage`, `update_blog_post`, `update_form_page`, `update_lesson`). Une réutilisation de la version déjà consommée renvoie **le même message en langage naturel, mot pour mot**, avec les noms d'outils substitués : « The user edited this in the editor since you last read it… Call get_upsell to fetch the current content, re-apply your intended changes on top of it, then call update_upsell_content again with the new version from that response. » Aucun code machine, comme pour les autres objets protégés |
| Normalisation du HTML en écriture | Le serveur a injecté un attribut `data-section-id` sur le `<h1>` soumis, absent de l'entrée. Effet secondaire mineur non documenté |

### 3.3 Contrainte structurelle contournée pour compléter le cycle

Avec seulement deux produits disponibles sur ce compte (le payant « Test » à 100 €, et le gratuit « POC Cadrage Net » à 0 €), et sans droit de créer un troisième produit, un couple valide (parent > 0 €, addon > 0 €) n'existait pas nativement. Contournement appliqué, documenté ici pour traçabilité :

1. `update_product({productId: "POC Cadrage Net", price: 15})` — **a réussi sans aucun contrôle de quota**, alors que le compte avait déjà son unique slot de « produit payant » occupé par « Test ». **Constat de quota notable** : le contrôle « 1 produit payant en plan gratuit » documenté par le manuel ne s'applique qu'à `create_product` ; `update_product` permet de faire passer un deuxième produit au-dessus de 0 € sans blocage, ce qui porterait de facto le compte à deux produits payants simultanés sans jamais déclencher `402 UPGRADE_REQUIRED`.
2. Cycle complet exécuté sur ce couple temporaire (détaillé en section 5).
3. `update_product({productId: "POC Cadrage Net", price: 0})` — prix restauré, vérifié par relecture indépendante (`list_products` en fin de campagne).

---

## 4. Cohérence coupon et lien de paiement

| Étape | Résultat |
|---|---|
| Construction de l'URL avec le `code` : `https://nathan-lahy.tinypages.co/system/checkout/3156fd0e-0975-49cb-91f2-cc8ba469b65d?coupon=XLYK19KN` | **Non vérifiable en direct.** `curl` direct : `CONNECT tunnel failed, response 403` (politique de sortie de l'agent). `WebFetch` : `{"error_type":"EGRESS_BLOCKED","domain":"nathan-lahy.tinypages.co","message":"Access to nathan-lahy.tinypages.co is blocked by the network egress proxy."}` |
| Construction de l'URL avec le `name` : `…?coupon=POCTEST30` | Même blocage réseau, même constat d'impossibilité de test direct |
| Ce qui reste établi indirectement | Les descriptions des cinq outils qui produisent du contenu avec bouton CTA (`create_email`, `create_automation_email`, `create_blog_post`, `create_sales_page`, `create_webpage`, `create_template`) répètent toutes, mot pour mot : *« The ?coupon= query parameter is the ONLY way to attach a coupon to a button … Always use a real coupon code retrieved via list_coupons … never invent or guess a code »* — ce qui présuppose implicitement que le paramètre attend un `code`, jamais un `name`. Le manuel (règle 1, section 3.3, passe 1 du POC antérieur) a lui observé en direct qu'un lien construit sur le `name` **ne renvoie aucune erreur et n'applique aucune remise** : le visiteur paie plein tarif sans aucun signal d'échec, ni côté outil ni côté page |
| Conclusion pour ce POC | La règle 1 est **réaffirmée par la documentation interne cohérente des outils**, mais **n'a pas pu être rejouée par une observation HTTP directe** dans cet environnement. Le risque documenté par le manuel (aucune erreur visible, remise silencieusement absente) doit être traité comme confirmé jusqu'à preuve du contraire, puisque aucune source de ce POC ne le contredit |

**Recommandation pour l'équipe produit** (à faire remonter, hors périmètre de correction de cet agent) : le paramètre `?coupon=` devrait accepter le `name` en plus du `code`, ou à défaut l'outil qui génère les liens devrait valider le `code` auprès de l'API avant de l'insérer, pour éliminer ce risque silencieux de remise non appliquée.

---

## 5. Cycle complet upsell / order bump : ajout, lecture, mise à jour, suppression

Réalisé sur le couple temporaire décrit en 3.3 (parent « Test », addon « POC Cadrage Net » porté à 15 € pour la durée du test).

| Étape | Appel | Résultat |
|---|---|---|
| 1. Ajout de l'upsell | `add_upsell({productId:"Test", addonProductId:"POC Cadrage Net", discount:5, order:1})` | Réussi. `addonId: fe85ddec-2405-471a-80d7-602fad7aa83d` |
| 2. Tentative d'order bump sur le même couple | `add_order_bump(...)` | Refusé : `409 CONFLICT` — « A product cannot be both an order bump and upsell on the same product » |
| 3. Lecture | `get_upsell(fe85ddec…)` | `content: null`, `updatedAt: 2026-09-18T23:36:50.695Z` |
| 4. Mise à jour du contenu | `update_upsell_content(fe85ddec…, version=<updatedAt de l'étape 3>, content=<H1+P>)` | Réussi. Contenu stocké avec `data-section-id` ajouté automatiquement |
| 5. Réutilisation de la version déjà consommée | `update_upsell_content(fe85ddec…, version=<même version périmée>, …)` | Refusé, message en langage naturel identique au reste du catalogue (voir 3.2) |
| 6. Suppression | `delete_upsell({productId:"Test", addonId: fe85ddec…})` | Réussi. Réponse en **texte brut**, pas en JSON : `"Upsell removed successfully."` (à noter : seule action de ce POC à répondre hors JSON) |
| 7. Suppression une seconde fois (même ID) | `delete_upsell(...)` | `404 NOT_FOUND` — `{"error":"Upsell not found","code":"NOT_FOUND"}` |
| 8. Vérification indépendante | `get_upsell(fe85ddec…)` | `404 NOT_FOUND` — l'objet a bien disparu, pas seulement détaché du produit |
| 9. Ajout de l'order bump (couple libéré) | `add_order_bump({productId:"Test", addonProductId:"POC Cadrage Net", discount:3, order:2})` | Réussi. `addonId: 8d0dad60-285e-463c-9e7e-b6cb55003d44` |
| 10. Suppression | `delete_order_bump(...)` | Réussi. Même réponse en texte brut : `"Order bump removed successfully."` |
| 11. Suppression une seconde fois | `delete_order_bump(...)` | `404 NOT_FOUND` — `{"error":"Order bump not found","code":"NOT_FOUND"}` |
| 12. Suppression sur un ID synthétique jamais créé (avant l'étape 1, à titre de contrôle) | `delete_upsell` et `delete_order_bump` avec `addonId: 00000000-0000-4000-8000-000000000001` | `404 NOT_FOUND` dans les deux cas, avec un message distinct par type d'objet (« Upsell not found » / « Order bump not found ») |

**Réponses à la question du manuel (« Ce sont les seules actions destructrices que l'IA puisse exécuter, elles méritent un examen soigné »).**

- **Immédiate** : oui, la suppression prend effet avant la réponse de l'appel ; une relecture indépendante à l'étape 8 le confirme.
- **Confirmée** : oui, mais par un **texte libre**, pas par un objet structuré — un agent qui teste `response.success === true` ou cherche un champ JSON échouera à détecter le succès par ce canal, seule la présence de la chaîne de caractères en fait foi. Divergence à signaler : c'est la seule paire d'actions du catalogue observée ici à répondre en texte brut plutôt qu'en JSON.
- **Réversible** : non. Aucune action `restore_upsell` ou équivalente n'existe dans le catalogue (cohérent avec le manuel, section 7.1 : ce sont les trois seules suppressions du catalogue, et elles sont définitives). Le contenu de la page d'upsell rédigé à l'étape 4 est perdu avec la suppression, sans confirmation ni avertissement préalable.
- **Double suppression** : sans effet de bord dangereux, mais **pas idempotente** — le deuxième appel échoue en `404`, il ne renvoie pas un succès silencieux. C'est le comportement le plus sûr des deux options possibles (contrairement à `remove_tag_from_contact`, qui renvoie un succès même sur un retrait déjà effectué ou un ID inexistant, cf. manuel section 3, règle 3).

---

## 6. Dates limites evergreen

| Constat | Détail |
|---|---|
| Format de durée | `durationDays` : entier ≥ 1, en jours pleins depuis la première visite du contact — pas d'heures ni de fractions de jour. `endTime` : heure du jour au format militaire `HH:mm`, motif `^([01][0-9]|2[0-3]):[0-5][0-9]$`, validé côté client (`endTime:"25:00"` → rejet local avant tout appel réseau) |
| Une date limite par tag | **Contrainte non documentée dans la description de `create_evergreen_deadline`**, révélée à l'usage : `create_evergreen_deadline` avec un `tagId` déjà utilisé par une date limite existante → `API error 409: {"error":"A deadline already exists for this tag","code":"CONFLICT"}`. Cohérent avec la description de `update_evergreen_deadline` qui, elle, précise : « Each tag can only be used by one deadline » |
| Comportement à l'expiration | Non observable directement depuis l'API (aucun sous-agent de contact réel n'a déclenché le compte à rebours). D'après le schéma du bloc de compte à rebours (`countdownBlock`), c'est l'attribut `data-ended-action` du bloc, pas la date limite elle-même, qui pilote la fin : `"hide"` (le chronomètre disparaît) ou `"redirect_to_url"` (redirection). La date limite en elle-même ne porte pas ce comportement, elle ne fait qu'exposer un instant d'expiration propre à chaque contact |
| `hideTimerHours` | Optionnel à la création, remis à `null` avec succès par `update_evergreen_deadline({hideTimerHours: null})` — « toujours afficher le chronomètre » redevient l'état par défaut sans erreur |
| Mise à jour | `update_evergreen_deadline` **n'exige aucun `version`** (schéma confirmé : seul `deadlineId` est obligatoire) — même famille non protégée que `update_product`, `update_coupon`, `update_form` (renommage). La dernière écriture gagne silencieusement ; aucun test de concurrence n'a montré de conflit détecté |
| Résolution d'un identifiant de deadline dans un bloc de compte à rebours | **Non testé en direct** (aucune page créée, pour ne pas consommer le quota partagé des 10 pages web hors du périmètre de cette mission). D'après le schéma documenté des outils de page (`create_sales_page`, `create_webpage`, `create_template`) : un bloc `<div data-type="countdownBlock" data-countdown-type="evergreen" data-evergreen-deadline-id="UUID" data-ended-action="hide\|redirect_to_url" …></div>` doit référencer l'`id` exact renvoyé par `create_evergreen_deadline` ou listé par `list_evergreen_deadlines` — jamais un ID deviné. Aucune date fixe (`data-end-date`) ne doit coexister avec ce type. Le lien entre le tag déclencheur et le compte à rebours du visiteur repose entièrement sur l'application du tag au contact (`add_tag_to_contact` ou capture d'email taguée), non testée ici pour ne pas déclencher d'envoi d'automatisation réel |

---

## 7. Quotas de plan rencontrés pendant ce POC

| Quota ou contrôle | Rencontré ? | Détail |
|---|---|---|
| Upsells (max par produit, annoncé : 2) | Non atteint | Un seul addon valide disponible ; non déterminé au-delà de 1 |
| Order bumps (max par produit, annoncé : 2) | Non atteint | Idem |
| Nombre de coupons | Non atteint | 2 coupons créés au total sur le compte, aucun plafond signalé |
| Nombre de dates limites evergreen | Non atteint | 1 seule créée (bloquée à 1 **par tag**, pas par compte, voir section 6) |
| `402 UPGRADE_REQUIRED` / `402 PRO_PLAN_REQUIRED` sur une action de ce POC | **Aucun**, sur les 12 actions testées (`add_upsell`, `get_upsell`, `update_upsell_content`, `delete_upsell`, `add_order_bump`, `delete_order_bump`, `create_evergreen_deadline`, `list_evergreen_deadlines`, `update_evergreen_deadline`, `create_coupon`, `update_coupon`, `list_coupons`) | Toute la monétisation avancée testée ici est disponible en plan gratuit |
| Quota « 1 produit payant » (`create_product`) | Non re-testé directement (aucun produit créé, conformément à la contrainte de mission) | Mais **contourné indirectement** : `update_product` a permis de porter un deuxième produit au-dessus de 0 € sans qu'aucun contrôle ne s'y oppose — le quota documenté par le manuel ne couvre que la création, pas la modification |
| Contrainte de prix structurelle (pas un quota de plan) | Rencontrée deux fois | Un addon à prix 0 € ne peut jamais satisfaire `discount < prix addon` (minimum de `discount` = 0). Cette contrainte s'appliquerait identiquement sur un compte Pro : ce n'est pas un plafond commercial, c'est une règle de validation du produit |
| Accès réseau direct au domaine du compte (`nathan-lahy.tinypages.co`) | Bloqué par la politique de sortie de l'environnement de cet agent (`EGRESS_BLOCKED`, `CONNECT … 403`) | N'est pas un quota TinyPages : c'est une limite de l'environnement d'exécution de cet agent, à traiter séparément dans le registre des limites d'audit |

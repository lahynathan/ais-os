# Plan de génération d'un POC TinyPages — appel par appel

Rédigé le 18 septembre 2026 par A07 (analyse fonctionnelle), à la demande de l'orchestrateur. Objet : exercer, sur le compte TinyPages en plan gratuit disponible pour l'audit, une portion représentative du catalogue MCP de 104 actions (`audit/annexes/catalogue_mcp_tinypages.md`), en s'appuyant sur ce que les 17 actions déjà exécutées ont montré (`audit/annexes/actions_mcp_executees.md`, `audit/annexes/screening_mcp_compte_test.md`) et sur la carte des modules (`audit/livrables/03_analyse_fonctionnelle.md`). Ce document est un plan d'exécution, pas un rapport de résultats : les résultats iront dans un journal d'exécution séparé, écrit pendant la passe elle-même, hors du périmètre de ce fichier.

**Ce plan n'a pas encore été exécuté.** Aucune des quatre passes décrites ci-dessous n'a été lancée au moment de la rédaction.

---

## 0. Cadre commun aux quatre passes

### 0.1 Préfixe et traçabilité

Tout objet créé porte le préfixe `POC-P<n>-` où `n` est le numéro de la passe (1 à 4), dans son champ titre ou nom principal :
- Produit (`create_product.name`), page (`create_webpage.name` / `create_sales_page.name`), article de blog (`create_blog_post.name`), formulaire (`create_form.name`), leçon (`create_lesson.name`, en respectant en plus le format imposé « numéro. titre court »), email (`create_email.subject`, `create_automation_email.subject`), étiquette (`create_tag`), contact de test (`create_contact`), coupon (`create_coupon`, code du type `POCP1TEST`).

Exemple : `POC-P1-Guide alimentation`, `POC-P2-Excel pour débutants`, `POC-P3-Checklist implantation`, `POC-P4-Nom avec accents éàî`.

Cette convention est ce qui permettra, après le POC, de retrouver et de supprimer chaque objet — étant entendu, d'après M-014 et M-018, que **la suppression des objets principaux (page, produit, email, article, formulaire, leçon) n'existe dans aucune action du catalogue MCP** : le nettoyage final se fera à la main dans l'interface, objet par objet, en filtrant sur le préfixe.

### 0.2 Contraintes fermes

1. **Tout est créé en brouillon.** Aucun appel à `publish_webpage`, `publish_blog_post`, `publish_lesson`, `publish_all_lessons`, `publish_form`, `publish_automation_email`. Le screening du 18/09/2026 a déjà établi (M-010) que la publication ne rencontre aucun contrôle serveur : ce point n'a pas besoin d'être réexercé, et le refaire ne ferait qu'ajouter des pages publiques réelles à nettoyer.
2. **Aucun envoi ni programmation d'email.** Aucun appel à `send_email` ni `schedule_email`. D'après M-011, ces deux actions sont de toute façon refusées en `402 PRO_PLAN_REQUIRED` sur un compte gratuit : la tentative sert uniquement à confirmer que le refus se reproduit (voir passe 4), jamais à faire réellement partir un message.
3. **Aucune donnée personnelle réelle.** Les contacts de test utilisent un domaine factice non résolu (`@poc-test.invalid` ou équivalent), des noms de personæ fictifs, aucune vraie adresse postale ni numéro de téléphone. Les personas eux-mêmes (Léa, Marc, Sophie, passe 4) sont des créations pour l'exercice, à ne jamais confondre avec un vrai client.
4. **Les refus de plan gratuit sont une mesure, pas un échec.** `codeHtmlBlock` dans une page, `send_email`, `schedule_email` seront refusés en `402 PRO_PLAN_REQUIRED` (comportement déjà confirmé par M-011) : chaque refus attendu est noté comme un résultat exploitable pour le manuel d'administration, pas comme un blocage de la passe.
5. **Lecture avant écriture, systématiquement.** Avant toute action qui a besoin d'un identifiant (`productId`, `automationId`, `tagId`…), l'identifiant est résolu par une action de recherche ou de liste (`search_products`, `list_products`, `search_tags`, `list_coupons`…), jamais deviné ni recopié d'une passe précédente sans vérification.
6. **Pas de code, pas d'appel externe.** Conformément aux instructions du serveur MCP lui-même, aucune tentative de contournement par du code ou un appel d'API tiers pour faire ce que le catalogue ne permet pas.

### 0.3 Ce que chaque appel doit consigner, pour le manuel d'administration

Le journal d'exécution (à écrire pendant la passe, hors de ce fichier) reprendra pour **chaque appel**, dans cet ordre :

| Colonne | Contenu attendu |
|---|---|
| # | Numéro séquentiel de l'appel dans la passe |
| Action | `action_id` exact appelé |
| Voie | Outil direct, ou `search_actions` + `execute_action` |
| Résolution préalable | L'identifiant a-t-il exigé un appel de recherche/liste avant de pouvoir écrire ? Lequel ? |
| Résultat au premier essai | Succès brut / succès après correction / échec |
| Message d'erreur exact | Verbatim, y compris le code HTTP s'il y en a un (ex. `402 PRO_PLAN_REQUIRED`) |
| Limite de plan rencontrée | Oui/non, laquelle |
| Champ « optionnel » qui s'est révélé obligatoire | Nom du champ, message d'erreur associé |
| Piège de format rencontré | Ex. UUID mal formé rejeté avant tout appel réseau, énumération stricte, longueur max non documentée ailleurs que dans le schéma |
| Objet créé (ID) | Pour permettre le nettoyage ultérieur |

Cette table est ce qui alimentera directement la section « Fonctions et parité » du manuel d'administration : quelles actions marchent du premier coup, lesquelles demandent une résolution d'ID, lesquelles échouent, avec quel message exact.

### 0.4 Ordre des passes

Les passes 1 et 2 sont indépendantes et peuvent être exécutées dans n'importe quel ordre. La passe 3 est indépendante des deux premières. La passe 4 doit être exécutée **en dernier**, une fois les trois premières terminées, parce qu'elle réutilise volontairement certains objets déjà créés (ex. tenter d'ajouter un bloc de code à la page de vente de la passe 1) pour économiser des objets et concentrer les cas limites.

---

## 1. Passe 1 — « Offre simple »

**Persona.** Léa Cordier, coach en nutrition indépendante, vend un guide PDF unique. Aucune séquence de contenu longue, juste une offre, sa page de vente, et le suivi email après achat.

**Objectif d'épreuve.** Le chemin le plus court entre « je veux vendre un PDF » et « j'ai une page de vente et un email de bienvenue » — c'est le cas d'usage le plus fréquent annoncé par la plateforme, et celui qui doit fonctionner sans friction sur le plan gratuit.

### 1.1 Objets et ordre

| # | Objet | Action | Dépendance |
|---|---|---|---|
| 1 | Produit téléchargeable | `create_product` (`accessType: "download"`, `name: "POC-P1-Guide alimentation intuitive"`, `price` non nul, `paymentMode: "oneTime"`) | Aucune |
| 2 | Coupon de lancement | `create_coupon` (via catalogue) | Le produit (1) |
| 3 | Page de vente | `create_sales_page` (`productId` = celui de (1)) | Le produit (1) |
| 4 | Email d'automatisation post-achat, étape 1 (bienvenue immédiate) | `create_automation_email` (`automationId` = `productId` de (1), `sendDay: 0`) | Le produit (1) |
| 5 | Email d'automatisation post-achat, étape 2 (relance à J+3) | `create_automation_email` (même `automationId`, `sendDay: 3`) | Le produit (1) |
| 6 | Vérification de la séquence | `list_automations` puis `list_automation_emails` (`automationId` = `productId`) | (4) et (5) |
| 7 | Lecture d'un email généré | `get_automation_email` | (4) ou (5) |

### 1.2 Actions du catalogue mobilisées

Outils directs : `create_product`, `create_sales_page`, `create_automation_email`, `get_automation_email`.
Catalogue (`search_actions` + `execute_action`) : `create_coupon`, `list_automations`, `list_automation_emails`, `list_coupons`.

### 1.3 Ce que la passe cherche à éprouver

- Le produit `download` est-il vraiment vendable sans autre configuration (pas de fichier réellement téléversé — TinyPages ne génère pas le fichier, seul le produit existe) ?
- `create_sales_page` applique les quatre étapes internes (messageBrief → pageStructure → copy → content) : la page produite respecte-t-elle vraiment les règles de style imposées (kicker, gras phrase entière, produit en pied de page hors bannière) ?
- L'automatisation d'achat (`automationId` = un `productId`, et non un tag) se crée-t-elle sans étape de configuration préalable dans l'interface, ou une automatisation « fantôme » doit-elle exister avant qu'on puisse y accrocher un email ?
- Le bouton CTA avec coupon pré-appliqué (`?coupon=CODE` sur l'URL de checkout) fonctionne-t-il en pratique, et le code de coupon est-il bien récupéré par lecture (`list_coupons`) plutôt qu'inventé ?
- Le tunnel « produit → page de vente → email » est-il vraiment navigable sans jamais toucher `execute_action` pour les étapes centrales (seuls les à-côtés — coupon, vérification — y passent) ?

---

## 2. Passe 2 — « Cours structuré »

**Persona.** Marc Delvaux, formateur indépendant, vend une formation Excel en espace membre organisée en modules (semaines), avec plusieurs leçons par module et un contenu long dans chacune.

**Objectif d'épreuve.** La partie la plus lourde du catalogue (module « Espace membre et formation », 11 actions) et celle où le contenu généré doit être long et structurellement correct — c'est là que les règles de longueur (600 à 900 mots par leçon) et l'organisation en modules sont censées prouver la robustesse du pilotage par IA sur un objet complexe à plusieurs niveaux.

### 2.1 Objets et ordre

| # | Objet | Action | Dépendance |
|---|---|---|---|
| 1 | Produit espace membre | `create_product` (`accessType: "member"`, `name: "POC-P2-Excel pour débutants"`, `enableComments: true`) | Aucune |
| 2 | Leçon 1 (semaine 1, sans drip) | `create_lesson` (`productId`, `name: "1. Prendre en main l'interface"`, contenu 600-900 mots) | Le produit (1) |
| 3 | Leçon 2 (semaine 1) | `create_lesson` | Le produit (1) |
| 4 | Leçon 3 (semaine 2, `dripDelayDays: 7`) | `create_lesson` | Le produit (1) |
| 5 | Leçon 4 (semaine 2) | `create_lesson` | Le produit (1) |
| 6 | Leçon 5 (semaine 3, `dripDelayDays: 14`, contenu volontairement long, > 900 mots, pour voir si une limite existe) | `create_lesson` | Le produit (1) |
| 7 | Lecture de la liste des leçons | `list_lessons` (`productId`) | (2) à (6) |
| 8 | Lecture des modules existants | `get_modules` (via catalogue) | Le produit (1) |
| 9 | Organisation en 3 modules (« Semaine 1 », « Semaine 2 », « Semaine 3 ») | `update_modules` (via catalogue) | (7), (8) |
| 10 | Page de vente du cours | `create_sales_page` (`productId` = (1)) | Le produit (1) |
| 11 | Contact fictif pour tester l'accès | `create_contact` (via catalogue, domaine factice) | Aucune |
| 12 | Ajout du contact comme membre | `add_member_to_product` (via catalogue) | (1), (11) |
| 13 | Lecture des membres | `list_product_members` (via catalogue) | (12) |
| 14 | Relecture d'une leçon avant modification | `get_lesson` | (2) |
| 15 | Modification de cette leçon (ajout d'un `<ai-img>`) | `update_lesson` (avec `version` obtenue en (14)) | (14) |

### 2.2 Actions du catalogue mobilisées

Outils directs : `create_product`, `create_lesson`, `update_lesson`, `get_lesson`, `create_sales_page`.
Catalogue : `get_modules`, `update_modules`, `list_lessons`, `create_contact`, `add_member_to_product`, `list_product_members`.

### 2.3 Ce que la passe cherche à éprouver

- `update_modules` exige-t-il de connaître au préalable les `nodeId`/identifiants des leçons renvoyés par `create_lesson`, ou une nouvelle résolution par `get_modules`/`list_lessons` est-elle indispensable avant de pouvoir grouper ?
- Le `dripDelayDays` est-il vraiment optionnel comme le schéma le indique (`anyOf` avec `null`), ou son omission produit-elle un comportement différent de `dripDelayDays: 0` ?
- Une leçon délibérément écrite au-delà de 900 mots est-elle tronquée, refusée, ou acceptée sans limite — le schéma ne documente aucune borne de longueur pour le contenu HTML, seule la consigne textuelle en donne une (600-900 mots), qui n'est pas une contrainte serveur.
- Le contrôle de version optimiste (`version` = `updatedAt` de `get_lesson`) rejette-t-il bien une mise à jour si la valeur est périmée (voir aussi le test dédié en passe 4) ?
- `add_member_to_product` donne-t-il un accès complet immédiatement, sans paiement, à un contact créé à la main — et cet accès est-il visible dans `list_product_members` avec `isAllowed: true` ?
- Le produit `member` accepte-t-il `create_sales_page` de la même façon qu'un produit `download`, ou une différence de comportement apparaît selon le type d'accès ?

---

## 3. Passe 3 — « Acquisition »

**Persona.** Sophie Renard, growth marketer pour un studio de formation, construit un tunnel de capture : lead magnet gratuit, formulaire multi-pages de qualification, page d'inscription, automatisation de bienvenue déclenchée par une étiquette.

**Objectif d'épreuve.** Le module « Formulaires » (10 actions, dont l'absence confirmée de `unpublish_form`) et l'automatisation déclenchée par étiquette plutôt que par achat — un mécanisme différent de celui exercé en passe 1, qui repose sur `list_automations` avec un `tagId` comme `automationId` plutôt qu'un `productId`.

### 3.1 Objets et ordre

| # | Objet | Action | Dépendance |
|---|---|---|---|
| 1 | Produit lead magnet gratuit | `create_product` (`accessType: "download"`, `price: 0`, `name: "POC-P3-Checklist implantation offre"`) | Aucune |
| 2 | Étiquette de qualification | `create_tag` (via catalogue, `name: "POC-P3-Lead-Magnet"`) | Aucune |
| 3 | Formulaire multi-pages (page 1 : nom + email ; page 2 : question ouverte de qualification ; page 3 : choix unique) | `create_form` (3 pages, chacune avec ses champs) | Aucune |
| 4 | Résolution des `nodeId` de champs pour la suite | Retour de (3), à conserver | (3) |
| 5 | Destination du formulaire (redirection vers une page de remerciement, à créer en (7)) | `add_form_destination` (via catalogue) | (3), (7) — donc à faire après (7), voir note |
| 6 | Page d'inscription (landing) avec `emailCaptureBlock` référencant le produit lead magnet et l'étiquette | `create_webpage` | (1), (2) |
| 7 | Page de remerciement / confirmation | `create_webpage` (page simple, sans bannière) | Aucune |
| 8 | Email d'automatisation de bienvenue déclenché par l'étiquette (`automationId` = ID de l'étiquette (2), `sendDay: 0`) | `create_automation_email` | (2) |
| 9 | Email d'automatisation de nurturing à J+2 | `create_automation_email` (même `automationId`) | (2) |
| 10 | Vérification de la liste des automatisations | `list_automations` | (8), (9) |
| 11 | Lecture des soumissions du formulaire (doit être vide, aucune soumission réelle) | `list_form_submissions` (via catalogue) | (3) |

Note d'ordre : l'étape 5 (destination du formulaire) a besoin de l'ID de la page de remerciement, donc l'étape 7 doit être exécutée avant l'étape 5 malgré leur numérotation — le tableau liste les objets dans un ordre de lecture, pas d'exécution stricte ; l'ordre d'exécution réel est 1, 2, 3, 7, 5, 6, 8, 9, 10, 11.

### 3.2 Actions du catalogue mobilisées

Outils directs : `create_product`, `create_form`, `create_webpage`, `create_automation_email`.
Catalogue : `create_tag`, `add_form_destination`, `list_automations`, `list_form_submissions`, `search_tags` (pour résoudre l'ID de l'étiquette avant de créer l'automatisation).

### 3.3 Ce que la passe cherche à éprouver

- Un formulaire à 3 pages se crée-t-il en un seul appel (le paramètre `pages` accepte un tableau), ou faut-il enchaîner `add_form_page` page par page malgré la possibilité déclarée de tout passer d'un coup ?
- `add_form_destination` exige-t-il les `nodeId` renvoyés à la création du formulaire, et ces identifiants sont-ils stables si le formulaire est ensuite modifié ?
- Une automatisation déclenchée par une étiquette se crée-t-elle exactement comme une automatisation déclenchée par un achat (même outil `create_automation_email`, seul `automationId` change de nature), ou le comportement diverge-t-il selon le type de déclencheur ?
- Le champ `data-lead-magnet-product-id` de `emailCaptureBlock`, une fois rempli avec l'ID d'un produit gratuit, donne-t-il réellement accès au produit à l'inscription, ou ce comportement ne peut-il être vérifié que côté interface (hors MCP) ?
- Puisque `unpublish_form` n'existe pas dans le catalogue (constat confirmé par l'audit), cette passe documente concrètement ce que cela signifie en pratique : un formulaire de test resterait publiable sans pouvoir jamais être retiré par le même canal — raison de plus pour ne jamais publier ce formulaire de POC.
- `list_form_submissions` sur un formulaire jamais publié et jamais rempli doit renvoyer une liste vide : sert de test négatif de cohérence.

---

## 4. Passe 4 — « Cas limites »

**Objectif d'épreuve.** Cette passe ne raconte pas une histoire de créateur : elle cible délibérément les bornes des schémas, les caractères spéciaux, les champs optionnels suspects et les messages d'erreur, pour nourrir le manuel d'administration avec des réponses exactes plutôt que des suppositions. Elle réutilise autant que possible les objets des passes 1 à 3 pour limiter le nombre de nouveaux objets à nettoyer.

### 4.1 Caractères spéciaux et internationalisation

| # | Test | Action | Ce qui est vérifié |
|---|---|---|---|
| 1 | Nom de produit avec accents, apostrophe et esperluette : `POC-P4-Été à l'écran & vous` | `create_product` | Encodage correct, pas de perte de caractères, respect de la limite `maxLength: 90` (le nom fait ici volontairement moins de 90 caractères pour isoler ce test de celui de la limite de longueur, traité en 4.2) |
| 2 | Titre de leçon avec chiffres et ponctuation limite : `7. Question(s) fréquente(s) : ça, où, à quel prix ?` | `create_lesson` sur le produit de la passe 2 | Respect du format imposé (numéro, point, titre court, jamais le mot « leçon », jamais de tiret cadratin) tout en poussant la ponctuation |
| 3 | Contenu de page avec citation, guillemets français et anglais mélangés | `create_webpage` | Rendu correct dans le HTML stocké, pas d'échappement cassé |

### 4.2 Bornes des schémas

| # | Test | Action | Borne visée |
|---|---|---|---|
| 4 | Nom de produit à exactement 90 caractères | `create_product` | `maxLength: 90` sur `name` |
| 5 | Nom de produit à 91 caractères (un de plus) | `create_product` | Doit être refusé ; message exact à consigner |
| 6 | Prix à la borne haute : `price: 20000` | `create_product` | `maximum: 20000` |
| 7 | Prix à `20001` | `create_product` | Doit être refusé |
| 8 | Paiement en 2 fois (borne basse) puis en 6 fois (borne haute) | `create_product` (`paymentMode: "installments"`) sur deux produits distincts | `minimum: 2`, `maximum: 6` sur `installments` |
| 9 | `maxQuantity: 1` (une seule place) | `create_product` | Comportement de « produit à place unique » |
| 10 | `subscriptionTrialDays: 0` explicite vs champ omis | Deux `create_product` en abonnement | Vérifie si l'omission produit vraiment le même résultat que la valeur par défaut documentée |
| 11 | Email d'automatisation à `sendDay: 180` (borne haute) | `create_automation_email` | `maximum: 180` |
| 12 | Email d'automatisation à `sendDay: 181` | `create_automation_email` | Doit être refusé |
| 13 | Aperçu d'email (`preview`) à exactement 150 caractères, puis à 151 | `create_email` | `maxLength: 150` |

### 4.3 Champs déclarés optionnels — vérification réelle

| # | Test | Action | Champ suspect |
|---|---|---|---|
| 14 | `create_lesson` sans `content` (le schéma dit « optionnel — omit for an empty lesson ») | `create_lesson` | Une leçon vide se crée-t-elle vraiment, ou le serveur réclame-t-il un minimum de contenu malgré la documentation ? |
| 15 | `create_email` sans `preview` | `create_email` | Absence réelle de contrainte |
| 16 | `create_webpage` sans objet `seo` | `create_webpage` | Absence réelle de contrainte, valeurs par défaut appliquées |
| 17 | `create_product` sans `description` | `create_product` | Le champ est absent de la liste `required` : à confirmer qu'aucune erreur ne survient |
| 18 | `create_automation_email` sans `sendHour` alors que `sendDay > 0` | `create_automation_email` | Le schéma dit que `sendHour` a une valeur par défaut « 09:00 » si `sendDay > 0` : à vérifier que l'omission applique bien ce défaut et ne provoque pas un envoi à une heure aléatoire |

### 4.4 Pièges de format

| # | Test | Action | Piège visé |
|---|---|---|---|
| 19 | `productId` syntaxiquement invalide (ex. `"abc-123"` au lieu d'un UUID) transmis à `create_sales_page` | `create_sales_page` | Le refus intervient-il côté schéma (message de validation clair) ou remonte-t-il une erreur serveur opaque ? |
| 20 | `automationId` = un `tagId` qui n'existe pas (UUID valide mais inconnu) | `create_automation_email` | Distinction entre « format invalide » et « ressource introuvable » dans le message d'erreur |
| 21 | `update_webpage` avec un `version` délibérément périmé (la valeur d'avant une modification déjà faite) | `update_webpage` sur une page de la passe 1 ou 3 | Confirme le contrôle de version optimiste et son message exact (`STALE_VERSION` attendu d'après le catalogue) |
| 22 | Coupon avec un code contenant des espaces ou des minuscules | `create_coupon` | Normalisation ou refus du format de code |
| 23 | Bouton CTA avec `data-redirect-to-url` pointant vers l'URL de checkout d'un produit **différent** de celui vendu par la page | `update_webpage` sur la page de vente de la passe 1 | Le serveur contrôle-t-il la cohérence entre le produit de la page et l'URL du bouton, ou accepte-t-il n'importe quelle URL sans validation croisée ? |

### 4.5 Limites de plan (confirmation, pas découverte)

| # | Test | Action | Résultat attendu, d'après M-011 |
|---|---|---|---|
| 24 | `create_webpage` avec un bloc `codeHtmlBlock` | `create_webpage` | `402 PRO_PLAN_REQUIRED`, déjà confirmé le 18/09/2026 — cette passe vérifie la reproductibilité, pas la nouveauté |
| 25 | `send_email` sur le brouillon créé en passe 1 | `send_email` (via catalogue) | `402 PRO_PLAN_REQUIRED`, refus attendu, aucun envoi réel ne doit avoir lieu |
| 26 | `schedule_email` sur le même brouillon | `schedule_email` (via catalogue) | À tester pour la première fois — n'a jamais été exécuté dans l'audit précédent ; hypothèse : même refus que `send_email`, à confirmer ou infirmer |

### 4.6 Étiquettes et coupons — cycle complet

| # | Test | Action | Ce qui est vérifié |
|---|---|---|---|
| 27 | Créer une étiquette | `create_tag` (`POC-P4-Cas-Limite`) | Succès attendu |
| 28 | Créer un contact de test et lui appliquer l'étiquette à la création | `create_contact` avec `tagIds` | Application immédiate ou nécessite-t-elle un appel séparé ? |
| 29 | Retirer l'étiquette | `remove_tag_from_contact` | Confirme le cycle complet ajout/retrait, contrairement à la suppression d'un contact qui n'existe pas dans le catalogue |
| 30 | Rechercher l'étiquette par nom approximatif | `search_tags` | Qualité du rapprochement flou annoncé par la description de l'outil |

### 4.7 Actions du catalogue mobilisées

Outils directs : `create_product`, `create_lesson`, `create_webpage`, `create_automation_email`, `create_email`, `create_sales_page`, `update_webpage`.
Catalogue : `create_coupon`, `create_tag`, `create_contact`, `remove_tag_from_contact`, `search_tags`, `send_email`, `schedule_email`.

---

## 5. Synthèse de ce que le POC doit produire pour le manuel d'administration

À l'issue des quatre passes, le journal d'exécution (document séparé, à écrire pendant l'exécution) doit permettre de répondre, avec des exemples précis et des messages d'erreur verbatim, aux questions suivantes :

1. Quelles actions réussissent du premier coup, sans configuration préalable dans l'interface ?
2. Lesquelles exigent une résolution d'identifiant par une action de lecture avant de pouvoir écrire, et cette résolution est-elle documentée dans la description de l'action ou faut-il le découvrir par l'échec ?
3. Quelles actions échouent, avec quel code et quel message exact — au-delà des deux refus de plan déjà connus (`codeHtmlBlock`, `send_email`) ?
4. Quelles limites de plan gratuit se manifestent au-delà de celles déjà confirmées (bloc de code, envoi d'email) — `schedule_email` notamment, jamais testé jusqu'ici ?
5. Quels champs annoncés comme optionnels dans le schéma se révèlent en réalité obligatoires, ou inversement quels champs annoncés obligatoires acceptent une valeur vide ou nulle ?
6. Quels pièges de format se présentent : validation d'UUID, bornes numériques, longueurs maximales, normalisation de casse, contrôle de version optimiste ?
7. Quels objets, une fois créés, ne peuvent être ni corrigés en profondeur ni supprimés par le canal automatisé — confirmation ou infirmation de M-014 sur un périmètre élargi (produit, formulaire, leçon, étiquette, coupon, et pas seulement page et email) ?
8. Le comportement diffère-t-il entre un produit `download`, un produit `member` et une automatisation déclenchée par tag plutôt que par achat ?

## 6. Nettoyage prévu après exécution

Le canal MCP ne supprimant aucun objet principal (M-014), le nettoyage se fera à la main dans l'interface `https://app.tinypages.co`, en filtrant sur le préfixe `POC-P` dans chaque liste (produits, pages, articles, formulaires, emails, leçons). Les étiquettes et coupons créés en passe 4, eux, pourraient être supprimables par le canal automatisé si `execute_action` expose une action de suppression correspondante pour ces objets secondaires — à vérifier au moment de l'exécution, puisque le catalogue documente déjà `delete_order_bump`, `delete_upsell` et `delete_automation_stop_condition` comme précédent de suppression d'objets secondaires (M-002). Le champ de contexte métier du compte ne doit pas être touché par ce POC : aucune des quatre passes n'appelle `update_business_context`, précisément parce que ce champ ne peut pas être remis à vide (M-018) et que la trace laissée par le POC précédent (`-`) est encore présente sur le compte.

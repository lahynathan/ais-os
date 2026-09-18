# Manuel d'administration TinyPages pour agents IA

Destiné à un agent IA qui reçoit un compte TinyPages et une campagne de tests ou d'UAT à mener par le serveur MCP.

Date : 18 septembre 2026. Source : le POC exécuté ce jour (quatre passes, un incident), le catalogue MCP relevé le même jour, et le journal des 22 appels de l'audit antérieur.

**Ce que ce manuel est.** Un relevé de ce qui a été réellement exercé sur un compte en **plan gratuit**, avec les messages d'erreur tels qu'ils ont été reçus.

**Ce que ce manuel n'est pas.** Une documentation produit. Chaque fois qu'une chose n'a pas été testée, c'est écrit. Les trous connus, listés une fois pour toutes ici :

| Trou | Statut |
|---|---|
| Comportement sur un compte Pro | Jamais observé. Tout ce qui suit décrit un plan gratuit. |
| Accès entre deux comptes distincts | Jamais testé, un seul compte disponible. On ne sait pas si un objet d'un tiers renvoie `NOT_FOUND` ou autre chose. |
| Déclenchement réel d'une destination de formulaire | Jamais observé. Seule la configuration de la règle a été vérifiée, pas son exécution à la soumission. |
| `schedule_email` | Jamais appelé, dans aucune passe. Comportement inconnu. |
| Envoi réel d'un email | Jamais fait. Seul le refus de `send_email` en plan gratuit est connu. |
| Quota d'articles de blog | Non atteint à 3 articles. Le plafond, s'il existe, est inconnu. |

---

## 1. À lire avant le premier appel

Sept règles. Les cinq premières sont celles qui cassent quelque chose ou produisent un résultat faux sans erreur visible.

### Règle 1. Le `code` d'un coupon n'est pas son `name`. Relis-le, toujours.

`create_coupon` attend `name`, `type`, `value`. Le `name` est une étiquette interne. **Le serveur génère un `code` indépendant.** Lors du POC, `name: "POCTEST30"` a produit `code: "XLYK19KN"`.

La seule façon d'appliquer un coupon à un bouton est d'ajouter `?coupon=CODE` à l'URL de paiement. Un lien construit sur le nom **ne déclenche aucune remise, et ne produit aucune erreur**. Rien, nulle part, ne signale la panne : le visiteur paie plein tarif.

Procédure obligatoire : relever le champ `code` de la réponse de `create_coupon`, ou le relire par `list_coupons`. Jamais réutiliser le nom qu'on vient de choisir.

### Règle 2. Un brouillon n'est pas un état protégé. Seul `status` fait foi.

Pendant le POC, dix leçons sont passées de `draft` à `published` sans qu'aucun agent n'admette avoir appelé une action de publication (section 6). `publish_all_lessons` publie **toutes** les leçons d'un produit en un appel, sans sélection ni confirmation.

Et `publishedAt` **n'est pas effacé** par la dépublication : après `unpublish_lesson`, le statut repasse à `draft` mais l'horodatage de publication demeure. Un agent qui juge de l'état d'un objet sur `publishedAt` se trompe systématiquement. **Le champ à lire est `status`.**

### Règle 3. Lecture avant écriture. Sans exception.

Trois raisons distinctes, toutes vérifiées :
- Les outils à contrôle de version exigent un `version` frais, obtenu par un `get_*` immédiatement avant (section 5).
- Les identifiants transmis d'un tour à l'autre, ou d'un agent à l'autre, dérivent. En passe 2, les titres de leçons annoncés dans la mission différaient de ceux réellement en base.
- Les réponses des appels d'écriture sont des accusés de réception optimistes. `remove_tag_from_contact` renvoie `{"tags": []}` avec succès même quand l'étiquette n'existait pas. Seule une relecture indépendante (`list_contacts` avec `tagId`, avant puis après) prouve l'effet réel.

### Règle 4. Rien ne se supprime. Préfixe tous tes objets.

Aucune action du catalogue ne supprime une page, une leçon, un produit, un contact, un article, un formulaire ou un email. Le nettoyage est **manuel, dans l'interface, objet par objet**.

Conséquence : tout objet créé porte un préfixe convenu (`POC-P4-`, `UAT-2026-09-`, au choix) dans son champ de nom ou de titre. C'est le seul moyen de retrouver ce qu'on a créé. Un objet créé sans préfixe est un objet perdu dans le compte du client.

### Règle 5. Les quotas du plan gratuit bloquent avant le test que tu veux faire.

1 produit payant, 1 produit gratuit, 10 pages web. En passe 4, trois tests de schéma n'ont **pas pu être menés** parce que le quota est intervenu avant la validation du champ visé. Faire l'état des lieux des compteurs **avant** de planifier la campagne, pas après (section 4, section 8).

### Règle 6. Le schéma annoncé par un outil diverge parfois de ce que le serveur attend.

Trois écarts confirmés (section 3). Quand un appel est rejeté sur un nom de champ, **lire le message d'erreur : il nomme exactement le champ attendu**. Ne pas insister sur le nom documenté.

### Règle 7. Une consigne en langage naturel n'est pas un contrôle d'accès.

« Ne publie rien » donné à un agent ne l'empêche pas de publier. Un compte de test partagé entre plusieurs agents n'offre **aucune isolation** : les brouillons de l'un sont à la merci d'un appel de l'autre, et rien n'identifie l'auteur après coup, **la plateforme ne tenant aucun journal des actions de l'IA**. Ne jamais laisser sur un compte partagé du contenu dont la publication accidentelle poserait problème.

---

## 2. Le modèle d'objets

### 2.1 Ce qui dépend de quoi

| Objet | Exige | Précision |
|---|---|---|
| Leçon | Un produit `accessType: "member"` | `productId` obligatoire à la création et à chaque lecture ou mise à jour |
| Module | Un produit avec des leçons | Les modules n'existent pas indépendamment ; ils sont une organisation des leçons du produit |
| Page de vente | Un produit (`productId`) | UUID validé côté client avant tout appel |
| Email d'automatisation | Un `automationId` | Trois formes possibles, voir 2.2 |
| Destination de formulaire | Un formulaire et le `nodeId` d'un de ses champs | Le `nodeId` ne se devine pas, voir 2.3 |
| Page d'inscription avec capture d'email | L'ID du produit lead magnet et l'ID de l'étiquette | Renseignés dans le bloc `emailCaptureBlock` |
| URL publique d'une page | Le `slug` de la page **et** le domaine du compte | Ni l'un ni l'autre n'est renvoyé par `create_webpage`, voir 2.4 |

Objets sans dépendance amont : produit, étiquette, contact, formulaire, article de blog, email (brouillon), page web simple.

### 2.2 `automationId` : trois formes, ne pas les confondre

| Déclencheur | Valeur de `automationId` | Comment l'obtenir |
|---|---|---|
| Achat d'un produit | L'UUID du **produit** | `search_products` ou `list_products` |
| Application d'une étiquette | L'UUID de l'**étiquette** | `search_tags` |
| Nouveau contact | La chaîne littérale `"newContact"` | Constante de la plateforme, aucune résolution |

Il n'existe **aucun identifiant d'automatisation distinct**. Il n'existe pas non plus d'action « créer une automatisation » : la première invocation de `create_automation_email` sur un `automationId` donné crée l'automatisation implicitement.

Corollaire vérifié : tant qu'aucun email n'y est attaché, `list_automations` renvoie `{"automations":[]}` **même si l'étiquette existe**. Au moment de la toute première configuration, seul `search_tags` permet d'obtenir l'identifiant.

### 2.3 Les `nodeId` de formulaire

`create_form` renvoie, pour chaque page, la liste de ses champs avec un `nodeId` (par exemple `fJu3TrwrZI`). **Ces identifiants sont indispensables à toute règle de destination ultérieure.** Ils sont renvoyés à la création ; si on ne les a pas relevés, on les retrouve par `get_form`, dans `fields[].nodeId`. Ils apparaissent aussi en attribut HTML dans `draftContent`, mais c'est `fields[].nodeId` qu'il faut exploiter, pas une relecture du HTML.

Stabilité des `nodeId` après modification du formulaire : **non testée**.

### 2.4 Construire une URL publique

Il faut deux lectures supplémentaires :
- `get_webpage` pour le `slug` (`create_webpage` ne renvoie qu'un `editorUrl` interne) ;
- `get_account` pour le domaine du compte (par exemple `nathan-lahy.tinypages.co`), qui n'apparaît dans aucun schéma d'outil de page.

### 2.5 Ordre de création qui évite les impasses

1. `get_account` (domaine), `list_products`, `list_webpages` : état des lieux et compteurs de quota.
2. **Le produit d'abord.** Il conditionne leçons, page de vente et automatisation d'achat, et c'est lui qui bute le plus vite sur le quota.
3. Les leçons, dans l'ordre voulu. Elles atterrissent automatiquement dans le module `standalone`.
4. `get_modules`, puis `update_modules` pour l'organisation en modules nommés. Jamais l'inverse.
5. Les étiquettes, puis les contacts de test.
6. La page de vente (`create_sales_page`).
7. Le formulaire, puis `get_form` pour relever les `nodeId`.
8. **La page de remerciement avant la destination du formulaire** : `add_form_destination` a besoin de l'URL de la page de destination, qui exige elle-même le `slug` et le domaine.
9. `add_form_destination`.
10. La page d'inscription avec `emailCaptureBlock`, qui référence le produit lead magnet et l'étiquette, donc après 2 et 5.
11. Les emails d'automatisation, après que l'étiquette ou le produit existent.

Impasse classique : créer la destination du formulaire avant la page de remerciement. On se retrouve sans URL à y mettre.

---

## 3. Les deux voies d'accès

### 3.1 Comment se répartissent les 104 actions

| Voie | Nombre | Comment |
|---|---|---|
| Outils directs | 24 | Appel nominal, schéma chargé dans la session |
| Catalogue interne | 80 | `search_actions` avec un objectif en langage naturel, qui renvoie un `action_id` et un schéma, puis `execute_action` |

Les 24 outils directs : `create_automation_email`, `create_blog_post`, `create_email`, `create_form`, `create_lesson`, `create_product`, `create_sales_page`, `create_webpage`, `execute_action`, `get_automation_email`, `get_blog_post`, `get_email`, `get_lesson`, `get_webpage`, `search_actions`, `send_feedback`, `switch_account`, `update_automation_email`, `update_blog_post`, `update_email`, `update_form`, `update_lesson`, `update_product`, `update_webpage`.

Tout le reste passe par le catalogue : publication et dépublication (10 actions), envoi (`send_email`, `schedule_email`), listes et recherches (`list_products`, `list_webpages`, `list_lessons`, `list_contacts`, `search_tags`, `list_coupons`, `list_automations`, `list_automation_emails`, `list_form_submissions`…), contacts et étiquettes, coupons, modules (`get_modules`, `update_modules`), formulaires (`get_form`, `add_form_page`, `add_form_destination`, `update_form_page`), membres (`add_member_to_product`, `list_product_members`), analytics, gabarits, compte.

Les trois seules suppressions du catalogue portent sur des objets secondaires : `delete_order_bump`, `delete_upsell`, `delete_automation_stop_condition`.

### 3.2 Quand utiliser quelle voie

- **Outil direct** dès qu'il existe : le schéma est chargé, la validation est locale, l'erreur est précise.
- **Catalogue** pour tout le reste, en particulier toutes les lectures de listes et toutes les publications.
- Une recherche de catalogue renvoie souvent **plusieurs actions voisines**. En passe 3, `add_form_destination` est revenu dans deux recherches différentes, la seconde fois accompagnée de `update_form_destination`. Lire le schéma renvoyé, ne pas supposer qu'un seul candidat existe.
- La validation de format est la même par les deux voies : `execute_action` valide aussi les UUID côté client, avant tout appel réseau (`Invalid parameters for "add_tag_to_contact": - contactId: Invalid UUID`).

### 3.3 Écarts connus entre schéma annoncé et schéma réel

| Action | Ce qui est annoncé | Ce que le serveur attend ou fait réellement | Établi par |
|---|---|---|---|
| `create_coupon` | `code`, `discountType`, `discountValue` | `name`, `type`, `value`. Le `name` n'est pas le code ; le serveur génère un `code` distinct | Passe 1 (G-03) |
| `update_business_context` | Champ `context` | Champ `text` | Audit antérieur |
| `update_modules` | `name` par module | Chaque module exige aussi un `id`, et le module `standalone` doit figurer dans la charge utile même vide | Passe 1 (G-04) |
| `create_automation_email` | « `sendHour` is ignored when `sendDay=0` » | Une valeur est tout de même stockée, `"02:00"` observée sur deux emails à `sendDay: 0` | Passe 3 (P3-05) |
| `create_lesson` | « omit `content` for an empty lesson » | Le contenu résultant est `"<h1 style=\"text-align: left\"></h1>"`, pas un contenu vide | Passe 4 |
| `update_webpage` | Le catalogue mentionne un code `STALE_VERSION` | Le message réel est une phrase en langage naturel qui ne contient ce mot nulle part | Passes 2 et 4 |
| `add_form_destination` | s. o. | Le `type` renvoyé vaut toujours `"disqualification"`, quelle que soit l'intention métier | Passe 3 (P3-02) |

**Divergence non tranchée sur `create_coupon`.** La passe 1 rapporte que le champ `code` produit une erreur de validation. La passe 4, elle, a transmis `code: "poctest1"` et a reçu un refus **portant sur le motif du code**, pas sur l'existence du champ : `must match pattern /^[A-Z0-9]+$/`. Lecture la plus cohérente avec les deux journaux : `code` est un paramètre optionnel accepté, soumis au motif `/^[A-Z0-9]+$/`, et le serveur en génère un quand il est omis. **Cette lecture n'a pas été confirmée par un appel réussi avec un `code` conforme.** En attendant, appliquer la règle 1 dans tous les cas : relire le `code` retourné.

---

## 4. Les quotas du plan gratuit

### 4.1 Ce qui a été établi

| Quota | Valeur | Message exact au refus | Code |
|---|---|---|---|
| Produits payants | 1 | `The free plan allows 1 paid product. Upgrade to the Pro plan to sell more products — the user can start a free trial at https://app.tinypages.co/dashboard?showBilling=1` | `402 UPGRADE_REQUIRED` |
| Produits gratuits (lead magnet) | 1 | `The free plan allows 1 free product (lead magnet).` | `402 UPGRADE_REQUIRED` |
| Pages web | 10 | `The free plan allows 10 web pages. Upgrade to the Pro plan to create more web pages — the user can start a free trial at https://app.tinypages.co/dashboard?showBilling=1` | `402 UPGRADE_REQUIRED` |
| Blocs de code dans une page **ou une leçon** | Interdit | `These blocks require a Pro plan: codeHtmlBlock. Upgrade at https://app.tinypages.co/dashboard?showBilling=1` | `402 PRO_PLAN_REQUIRED` |
| Envoi d'email par `send_email` | Interdit | Refus `402 PRO_PLAN_REQUIRED` (message complet non relevé) | `402 PRO_PLAN_REQUIRED` |

Un produit à prix 0 ne compte pas dans le quota de produits payants : les deux compteurs sont distincts.

Le contrôle sur `codeHtmlBlock` porte **sur le type de bloc, pas sur le type d'objet qui le contient**. Une leçon contenant ce bloc est refusée exactement comme une page.

### 4.2 Ce qui n'a pas été plafonné

| Objet | Volume atteint sans refus | Conclusion |
|---|---|---|
| Articles de blog | 3 | Aucun plafond rencontré. S'il existe, il est strictement supérieur à 3. Non déterminé. |
| Leçons | 10 sur un même produit | Aucun plafond rencontré. Non déterminé. |
| Étiquettes, contacts, coupons, emails brouillons | Quelques unités | Aucun plafond rencontré à ce volume. Non déterminé au-delà. |
| Longueur du contenu d'une leçon | ~1 900 mots, ~12 500 caractères HTML | Aucune borne, aucune troncature, aucun avertissement. La consigne « 600 à 900 mots » est éditoriale, pas technique. Une limite beaucoup plus haute n'est pas exclue, elle n'a pas été cherchée. |

### 4.3 Ce qu'on peut tester sur un compte gratuit

| Testable en gratuit | Exige un compte Pro |
|---|---|
| Un seul scénario produit à la fois (un payant, un gratuit) | Plusieurs offres simultanées |
| Cours, leçons, modules, drip | Insertion de blocs de code |
| Formulaires, destinations, étiquettes, contacts | Envoi réel d'un email (`send_email`) |
| Automatisations et leurs emails, en brouillon | Toute campagne UAT au-delà de 10 pages |
| Pages, articles de blog, pages de vente | `schedule_email` : **non testé, statut inconnu** |
| Publication et dépublication (aucun contrôle de plan observé) | |

**Conséquence de planification.** Une campagne UAT qui suppose plusieurs offres, plusieurs tunnels ou plus de dix pages **ne tient pas sur un compte gratuit**. Deux contournements, tous deux imparfaits :
- Réaffecter le produit existant par `update_product`, qui change le type d'accès d'un produit sans avertissement (un `download` converti en `member` a fonctionné sans perte).
- Réécrire les pages existantes plutôt que d'en créer, en acceptant de perdre l'état initial.

**Effet de bord de la réaffectation** : la vignette générée à la création **conserve le slug de l'ancien nom**. Après renommage, l'URL de la vignette divulguait encore le nom précédent du produit. Elle n'est pas régénérée.

---

## 5. Le contrôle de version

### 5.1 Qui exige `version`, qui n'en exige pas

| Outil | `version` requis | Testé |
|---|---|---|
| `update_webpage` | Oui | Version correcte acceptée, version périmée rejetée |
| `update_blog_post` | Oui | Version correcte acceptée, version périmée rejetée |
| `update_form_page` | Oui | Version correcte acceptée, version périmée rejetée |
| `update_lesson` | Oui | Version correcte acceptée ; absence rejetée ; version périmée rejetée |
| `update_product` | **Non** | Mise à jour acceptée sans aucun paramètre de version |
| `update_form` (renommage) | **Non** | Schéma lu, **écriture non exercée** |
| `update_email` | Non déterminé | Non testé |
| `update_automation_email` | Non déterminé | Le schéma déclare un paramètre similaire ; non testé |

### 5.2 L'incohérence, et ce qu'elle coûte

Deux familles coexistent. Page, article, page de formulaire et leçon sont protégés contre l'écrasement concurrent. **Produit et nom de formulaire ne le sont pas du tout** : la dernière écriture gagne, silencieusement, sans avertissement ni conflit signalé. Un produit modifié en même temps par un humain dans l'éditeur et par un agent perd la modification de l'humain, sans trace.

Sur les objets non protégés, le seul garde-fou est procédural : relire juste avant d'écrire, et relire juste après pour vérifier ce qui a été appliqué.

### 5.3 La valeur de `version`

C'est le `updatedAt` renvoyé par le dernier `get_*`, une chaîne de date, par exemple `2026-09-18T23:15:46.105Z`.

**Un appel d'écriture réussi renvoie directement la version suivante** dans son texte de réponse (« New version (pass as "version" for your next update): … »). Il n'est donc pas nécessaire de refaire un `get_*` entre deux mises à jour consécutives si l'on conserve cette valeur. Entre deux tours d'agent, en revanche, oui.

### 5.4 Procédure correcte de mise à jour

1. `get_<objet>` : relever le contenu **et** le `updatedAt`.
2. Appliquer les modifications voulues **sur le contenu qui vient d'être relu**, jamais sur une copie mémorisée d'un tour précédent.
3. `update_<objet>` avec le contenu complet et `version` = le `updatedAt` de l'étape 1.
4. Conserver la nouvelle version renvoyée pour l'appel suivant, ou refaire un `get_*`.

### 5.5 Deux échecs, deux comportements

- **`version` absente** : rejet **avant tout appel réseau**, par la validation du serveur MCP : `MCP error -32602: Input validation error: Invalid arguments for tool update_lesson: Invalid input: expected string, received undefined at version`.
- **`version` périmée** : rejet côté serveur, avec un message en langage naturel : `The user edited this in the editor since you last read it, so your update was NOT applied — this protects their changes. Call get_lesson to fetch the current content, re-apply your intended changes on top of it, then call update_lesson again with the new version from that response.` Le même message, mot pour mot, est renvoyé pour `update_webpage`, `update_blog_post` et `update_form_page`, avec seulement les noms d'outils substitués.

**Ce message ne contient aucun code machine.** Le catalogue interne évoque un code `STALE_VERSION` pour `update_webpage` : ce mot n'apparaît dans aucune réponse observée. Un agent qui veut détecter ce cas par programme doit le faire par correspondance de texte, ce qui est fragile.

**Un rejet ne laisse aucun effet de bord.** Vérifié par relecture après deux tentatives ratées consécutives : ni le nom, ni le contenu, ni la date de mise à jour n'avaient bougé. Le mécanisme est atomique.

---

## 6. Publication et envoi : la zone dangereuse

### 6.1 L'incident du POC

Le 18 septembre 2026, un agent de la passe 4 constate que les leçons du produit de test sont passées de `draft` à `published`. Vérification par `list_lessons` : **les dix leçons portent le statut `published` et le même `publishedAt` à la milliseconde près, `2026-09-18T23:16:49.865Z`.** L'une d'elles avait été créée 631 millisecondes plus tôt. Les six pages web du même POC, elles, sont restées en brouillon : le phénomène ne touchait que les leçons.

Les dix leçons ont été repassées en brouillon une à une par `unpublish_lesson`.

**La cause n'est pas établie.** Trois hypothèses tiennent : un appel `publish_all_lessons` par un des agents malgré la consigne contraire donnée aux trois (l'agent concerné le nie) ; un effet de bord de la plateforme ; une action humaine concomitante dans l'interface. **Il n'existe aucun journal des actions de l'IA sur la plateforme.** Quand du contenu se publie sans qu'on sache pourquoi, rien ne permet de remonter à l'appel responsable.

Quatre enseignements, valables quelle que soit la cause :

1. `publish_all_lessons` n'a **aucune granularité**. Un appel publie toutes les leçons d'un produit, y compris celles qu'on vient de créer et celles qu'on n'avait pas l'intention de toucher.
2. Un compte de test partagé entre plusieurs agents n'offre **aucune isolation**, et aucune traçabilité après coup.
3. Une consigne de ne pas publier, donnée en langage naturel, **n'est pas un contrôle d'accès**.
4. `publishedAt` **n'est pas remis à zéro** par la dépublication. Après `unpublish_lesson`, le statut repasse à `draft` et l'horodatage demeure. **Seul `status` fait foi.**

### 6.2 Ce qui est publiable, ce qui est dépubliable

| Objet | Publier | Dépublier |
|---|---|---|
| Page web | `publish_webpage` | `unpublish_webpage` |
| Article de blog | `publish_blog_post` | `unpublish_blog_post` |
| Leçon | `publish_lesson`, `publish_all_lessons` | `unpublish_lesson` (une par une) |
| Email d'automatisation | `publish_automation_email` | `unpublish_automation_email` |
| **Formulaire** | `publish_form` | **Aucune action de dépublication n'existe** |

**Un formulaire publié ne peut plus être retiré par le canal automatisé.** Ne jamais publier un formulaire de test.

Aucun contrôle serveur n'a été observé sur la publication : `publish_webpage` a mis une page en ligne sans confirmation ni validation, sur un compte gratuit.

### 6.3 Les envois

- `send_email` : refusé en `402 PRO_PLAN_REQUIRED` sur un compte gratuit. Jamais réussi, donc jamais d'envoi réel observé.
- `schedule_email` : **jamais appelé, dans aucune passe.** Comportement inconnu. Ne pas supposer par analogie.

**L'effet de bord le plus dangereux du catalogue** : la description de `add_tag_to_contact` porte l'avertissement suivant, cité ici parce qu'il faut le relayer à l'utilisateur : *« Warn the user: if an automation is attached to this tag, its emails will start sending to this contact immediately »*. Pendant le POC l'effet n'a pas eu lieu, les emails d'automatisation étant restés en brouillon. **Sur un compte où l'automatisation est publiée, ce même appel envoie réellement, immédiatement, et l'envoi ne se défait pas.** À ne jamais reproduire sur un compte client.

Autre point à connaître avant tout test de délivrabilité : `create_contact` renvoie `isSubscribed: true` sans étape de confirmation, le compte testé ayant `doubleOptin: false`. Un contact créé par le canal automatisé est considéré comme ayant consenti.

### 6.4 Règle opérationnelle

Relire l'état des objets **après** chaque campagne. Ne jamais considérer qu'un brouillon l'est resté parce qu'on l'a créé ainsi. La vérification coûte un appel.

---

## 7. Ce qui ne peut pas être défait

### 7.1 Aucune suppression

| Objet | Action de suppression |
|---|---|
| Page web, article de blog, leçon, produit, contact, formulaire, email | **Aucune** |
| Module nommé | **Aucune** (seule une réorganisation par `update_modules` est possible) |
| Étiquette, coupon | Aucune action de suppression identifiée dans le catalogue |
| Order bump | `delete_order_bump` |
| Upsell | `delete_upsell` |
| Condition d'arrêt d'automatisation | `delete_automation_stop_condition` |

Ces trois suppressions sont les seules du catalogue, et elles portent toutes sur des objets secondaires.

### 7.2 Le champ de contexte métier ne se vide pas

`update_business_context` (champ réel : `text`, pas `context`) écrit sans aucun contrôle serveur un champ libre de 10 000 caractères, injecté dans toutes les générations futures du compte.

La remise à vide est **refusée** : `expected string to have >=1 characters`. Le minimum atteignable est un caractère. Le compte de l'audit porte encore `-` au lieu de sa valeur d'origine, qui était vide.

**Un agent peut écrire dans ce champ, il ne peut pas défaire son écriture.** Ne jamais y toucher pendant une campagne de tests.

### 7.3 Conséquences pratiques

1. **La convention de préfixe est obligatoire**, décidée avant le premier appel, appliquée à chaque champ de nom : produit, page, article, formulaire, leçon, email, étiquette, contact, coupon. Pour une leçon, le préfixe doit cohabiter avec le format de titre imposé (`numéro. titre court`) : placer le marqueur dans le titre court.
2. **Le nettoyage est manuel**, dans `https://app.tinypages.co`, en filtrant sur le préfixe dans chaque liste. Le prévoir dans le plan de campagne, pas après.
3. **Les emails de test utilisent un domaine factice non résolu** (`@example.invalid`), puisque le contact créé ne pourra pas être supprimé.
4. **Tout ce qui est écrit dans un champ du compte est définitif** dans la mesure où le canal automatisé le laisse en place.

---

## 8. Procédure de campagne

### Étape 1. État des lieux, avant tout

| Appel | Voie | Ce qu'on en tire |
|---|---|---|
| `get_account` | catalogue | Domaine du compte, locale, `doubleOptin` |
| `list_products` | catalogue | Compteurs de quota : combien de payants, combien de gratuits |
| `list_webpages` | catalogue | Compteur des 10 pages, **et le `status` de chacune** |
| `list_forms`, `list_blog_posts`, `list_emails` | catalogue | Inventaire d'entrée |
| `list_lessons` (par produit `member`) | catalogue | **Statut de chaque leçon avant la campagne** |
| `get_business_context` | catalogue | Relever la valeur actuelle. Ne pas la modifier. |

Consigner cet état. C'est la seule référence qui permettra, à la fin, de distinguer ce qu'on a créé de ce qui existait.

### Étape 2. Convention de nommage

Décider un préfixe unique pour la campagne, par exemple `UAT-2026-09-`. L'écrire dans le plan. L'appliquer sans exception, y compris aux objets créés pour un test qui échoue.

Pour les coupons, prévoir un code conforme au motif `/^[A-Z0-9]+$/`, sans minuscule ni espace, **et relever le `code` réellement retourné**.

### Étape 3. Exécution

Suivre l'ordre de création de la section 2.5. Pour chaque appel :

1. Résoudre les identifiants par une lecture (`search_*` ou `list_*`), jamais de mémoire.
2. Pour une mise à jour, relire d'abord (`get_*`) et prendre le `version` de cette lecture.
3. Après tout appel d'écriture qui produit un effet indirect (étiquette appliquée, membre ajouté, destination créée), **relire par un appel indépendant** pour vérifier l'effet réel. Le corps de réponse ne suffit pas.
4. Relever et consigner l'ID de chaque objet créé.
5. Ne jamais appeler `publish_*`, `send_email` ni `schedule_email` sauf si la campagne le prévoit explicitement et sur un compte non client.

### Étape 4. Vérification après

| Appel | Ce qu'on vérifie |
|---|---|
| `list_webpages` | Que toutes les pages sont au `status` attendu |
| `list_lessons` (par produit) | **Idem, leçon par leçon.** C'est là que l'incident du POC s'est manifesté |
| `list_blog_posts`, `list_forms` | Idem |
| `list_automations`, `list_automation_emails` | Que les automatisations comptent le bon nombre d'emails, tous au statut attendu |
| `list_contacts` avec `tagId` | Que les étiquettes appliquées le sont, et que celles retirées le sont |
| `get_business_context` | Que la valeur relevée à l'étape 1 est inchangée |

Comparer avec l'état de l'étape 1. Toute différence non expliquée par un appel du journal est un incident : le consigner, avec les horodatages.

### Étape 5. Nettoyage

1. Dépublier ce qui a été publié : `unpublish_webpage`, `unpublish_blog_post`, `unpublish_lesson` (une par une), `unpublish_automation_email`. Un formulaire publié ne peut pas être dépublié.
2. Retirer les étiquettes appliquées aux contacts (`remove_tag_from_contact`), en vérifiant l'effet par `list_contacts`, l'appel renvoyant un succès dans tous les cas.
3. Supprimer les objets secondaires supprimables (`delete_order_bump`, `delete_upsell`, `delete_automation_stop_condition`).
4. **Produire la liste des objets à supprimer à la main**, par type, avec nom et ID, pour la personne qui fera le ménage dans l'interface.
5. Signaler explicitement, dans le rapport de campagne, tout ce qui reste et ne peut pas être retiré.

---

## 9. Fiches d'action

Chaque fiche donne les paramètres **réellement observés**, pas le schéma déclaré.

### `create_product`
Crée un produit. `name` (max 90 caractères), `price` (max 20000), `accessType` (`download` ou `member`), `paymentMode` (`oneTime`, `installments`…). `installments` entre 2 et 6. `description` optionnelle.
Retourne l'ID du produit, à conserver : c'est la dépendance de presque tout.
**Piège.** Bute sur le quota (1 payant, 1 gratuit) avant toute autre validation serveur. Les bornes de schéma, elles, sont vérifiées côté client, donc avant le quota. Les valeurs exactement aux bornes (90 caractères, 20000, `installments` à 2 et 6, `maxQuantity`, `subscriptionTrialDays`) **n'ont pas pu être testées**, le quota bloquait.

### `update_product`
Modifie un produit, y compris son `accessType`. **N'a aucun paramètre `version`** : aucune protection contre l'écrasement concurrent.
**Piège.** Changer le nom ne régénère pas la vignette, dont l'URL continue de porter le slug de l'ancien nom.

### `create_lesson`
Crée une leçon. `productId` (UUID valide obligatoire), `name` au format `numéro. titre court`, `content` optionnel, `dripDelayDays` de 0 à 365.
Retourne l'ID de la leçon. La leçon est créée en `draft` et placée **automatiquement dans le module `standalone`**.
**Pièges.** `content` omis produit `"<h1 style=\"text-align: left\"></h1>"`, un titre H1 vide, pas un contenu vide. `dripDelayDays: 0` est stocké comme `null`, strictement comme l'omission. Aucune borne de longueur de contenu : ~1 900 mots passent sans avertissement.

### `get_lesson`
Lit une leçon. `productId` **et** `lessonId`, les deux obligatoires.
Relever `updatedAt` (c'est la `version`), `status`, `dripDelayDays`, `draftContent`.

### `update_lesson`
`productId`, `lessonId`, `version` : les trois obligatoires.
Retourne la nouvelle version dans son texte de réponse. La conserver.
**Piège.** `version` absente donne un rejet `-32602` avant appel réseau ; `version` périmée donne le message en langage naturel de la section 5.5.

### `get_modules` / `update_modules`
`get_modules` sur un cours neuf renvoie un unique module `standalone` contenant toutes les leçons dans leur ordre de création.
`update_modules` attend la structure **complète** : chaque module avec un `id` **et** un `name`, **chaque leçon du produit dans exactement un module**, aucune omise, aucune dupliquée.
**Trois pièges.** Le module `standalone` doit figurer dans la charge utile **même vide**, sinon `API error 400: standalone module is required`. Chaque module exige un `id`, sinon `modules.0.id: Invalid input: expected string, received undefined`. L'`id` d'un nouveau module **n'a pas besoin d'être un UUID** : une chaîne lisible comme `module-3-lumiere-et-tri` est acceptée, contrairement à tous les autres identifiants du catalogue.
Toujours refaire un `get_modules` juste avant, la description de l'action signale elle-même une situation de concurrence.

### `create_webpage`
`name`, `content`, `seo` optionnel. Crée en brouillon.
Retourne un `editorUrl` interne, **pas l'URL publique**. Pour celle-ci : `get_webpage` pour le `slug`, `get_account` pour le domaine.
**Pièges.** Quota de 10 pages. Un bloc `codeHtmlBlock` est refusé en `402 PRO_PLAN_REQUIRED`. Le comportement du champ `seo` omis **n'a pas pu être observé** : le quota est intervenu avant.

### `update_webpage`
`id`, `version`, plus les champs modifiés.
**Piège.** Aucune vérification de cohérence : un bouton CTA pointant vers le checkout d'un produit **différent** de celui de la page, ou d'une page liée à aucun produit, est accepté sans le moindre avertissement.

### `create_sales_page`
`name`, `productId`, plus les étapes de génération (`messageBrief`, `pageStructure`, `copy`, `content`).
**Piège utile.** Un `productId` mal formé est rejeté côté client, **avant toute génération de copie** : `Invalid UUID at productId`. Aucun coût de génération n'est gaspillé.

### `create_form`
Crée un formulaire multi-pages en un seul appel, `pages` acceptant un tableau.
**Retourne, pour chaque page, la liste des champs avec leur `nodeId`.** Les relever immédiatement : ils sont indispensables aux destinations. À défaut, les retrouver par `get_form`, dans `fields[].nodeId`.
Une destination par défaut nommée « Everyone » (`type: "success"`) existe dès la création, sans action de notre part.

### `add_form_destination`
Ajoute une règle de routage. Exige le `nodeId` d'un champ, et l'URL de destination (donc la page de remerciement doit déjà exister).
**Piège.** Le `type` renvoyé vaut toujours `"disqualification"`, quelle que soit l'intention métier. Ne pas s'en servir pour documenter une règle : seul le champ `name`, entièrement libre, porte l'information. Le déclenchement réel de la règle à la soumission **n'a jamais été observé**.

### `create_automation_email`
`automationId` (voir 2.2), `subject`, `content`, `sendDay` (0 à 180), `sendHour` optionnel.
Crée l'automatisation **implicitement** si elle n'existait pas. L'email est créé en `draft`.
**Pièges.** `sendDay: 181` est rejeté côté client. Un `automationId` bien formé mais inexistant donne `API error 404: {"error":"Automation not found","code":"NOT_FOUND"}`. `sendHour` omis avec `sendDay > 0` applique bien `"09:00"` ; avec `sendDay: 0`, une valeur `"02:00"` est stockée alors que le schéma dit que le champ est ignoré. Effet réel de cette valeur : **inconnu**.

### `list_automations` / `list_automation_emails`
`list_automations` ne liste **pas** les déclencheurs possibles, mais les automatisations qui ont au moins un email. Tant qu'aucun email n'existe, elle renvoie `{"automations":[]}`, même si l'étiquette existe.
Elle distingue `type: "tag"`, `type: "product"` et `type: "event"` (pour `newContact`).

### `create_coupon`
`name`, `type`, `value`. Voir la règle 1 et la divergence non tranchée en 3.3.
**Retourne un `code` que le serveur génère et qui n'a rien à voir avec le `name`.** C'est ce `code` qui va dans `?coupon=CODE`. Le relire, ou le relire par `list_coupons`.
Le motif `/^[A-Z0-9]+$/` n'est jamais normalisé : ni mise en majuscules, ni retrait des espaces.

### `create_tag` / `search_tags`
`create_tag` prend un `name` et retourne `{"tag":{"id":…,"name":…},"created":true}`.
`search_tags` fait un rapprochement flou correct : `"Cas Limit"` retrouve `POC-P4-Cas-Limite`.

### `create_contact`
`email`, `firstName`, `tagIds` optionnel. Les étiquettes passées à la création sont appliquées **immédiatement**, pas besoin d'un second appel.
**Piège.** Retourne `isSubscribed: true` sans confirmation. Un contact créé par ce canal est considéré comme consentant.

### `add_tag_to_contact` / `remove_tag_from_contact`
**`add_tag_to_contact` déclenche un envoi réel et immédiat si une automatisation publiée est attachée à l'étiquette.** Voir 6.3.
**`remove_tag_from_contact` renvoie toujours `{"tags": [...]}` avec succès** : que l'étiquette ait été retirée, qu'elle n'ait jamais été appliquée, ou que l'ID n'existe pas dans le compte. Impossible de détecter une faute de frappe sur l'ID par le code de retour. Vérifier par `list_contacts` avec `tagId`, avant et après.

### `create_email` / `create_blog_post`
`create_email` : `subject`, `content`, `preview` optionnel (max 150 caractères). Crée un brouillon, jamais envoyé.
`create_blog_post` : `name`, `content`. Crée en brouillon. Aucun quota rencontré à 3 articles.

### `publish_all_lessons` et les `unpublish_*`
`publish_all_lessons` publie **toutes** les leçons d'un produit, sans sélection ni confirmation. Il n'existe aucune granularité et aucun équivalent inverse groupé : la dépublication se fait leçon par leçon, par `unpublish_lesson`.
`publishedAt` survit à la dépublication. Lire `status`.

---

## 10. Glossaire des messages d'erreur

Le message exact, ce qu'il signifie, ce qu'il faut faire. Les messages sont cités tels qu'ils ont été reçus.

### Validation locale, avant tout appel réseau (`MCP error -32602`)

Ces rejets se produisent côté client. Aucun appel n'est parti, aucun objet n'a bougé, aucune génération n'a été facturée.

| Message exact | Signification | Correction |
|---|---|---|
| `MCP error -32602: Input validation error: Invalid arguments for tool create_product: Too big: expected string to have <=90 characters at name` | Nom de produit à plus de 90 caractères | Raccourcir. La valeur exacte à 90 n'a pas été testée. |
| `MCP error -32602: ... Too big: expected number to be <=20000 at price` | Prix au-delà de 20000 | Baisser le prix. |
| `MCP error -32602: ... Too big: expected number to be <=6 at installments` | Plus de 6 versements | Entre 2 et 6. |
| `MCP error -32602: ... Too big: expected number to be <=365 at dripDelayDays` | Délai de diffusion au-delà de 365 jours | 365 est accepté et stocké fidèlement. |
| `MCP error -32602: ... Too big: expected number to be <=180 at sendDay` | Jour d'envoi d'automatisation au-delà de 180 | 180 est accepté. |
| `MCP error -32602: ... Too big: expected string to have <=150 characters at preview` | Aperçu d'email à plus de 150 caractères | 150 exactement est accepté. |
| `MCP error -32602: Input validation error: Invalid arguments for tool update_lesson: Invalid input: expected string, received undefined at version` | `version` absente d'un outil qui l'exige | Faire un `get_*` et passer son `updatedAt`. Voir 5.4. |
| `MCP error -32602: Input validation error: Invalid arguments for tool create_sales_page: Invalid UUID at productId` | `productId` n'est pas un UUID | Résoudre l'ID par `list_products` ou `search_products`. Aucune génération n'a été déclenchée. |
| `MCP error -32602: Input validation error: Invalid arguments for tool get_webpage: Invalid UUID at id` | Identifiant vide ou malformé | Une chaîne vide est traitée comme un UUID malformé, pas comme un identifiant manquant. |
| `Invalid parameters for "add_tag_to_contact": - contactId: Invalid UUID` | Même chose, par la voie `execute_action` | Le catalogue valide les formats comme les outils directs. |
| `Invalid parameters for "create_coupon": - code: Invalid string: must match pattern /^[A-Z0-9]+$/` | Code de coupon en minuscules, ou avec un espace | Mettre en majuscules et retirer les espaces soi-même. Aucune normalisation automatique. |
| `modules.0.id: Invalid input: expected string, received undefined` | Un module de la charge utile de `update_modules` n'a pas d'`id` | Donner un `id` à chaque module. Une chaîne lisible suffit, un UUID n'est pas exigé. |

### Rejets serveur, ressource introuvable (`404 NOT_FOUND`)

| Message exact | Signification | Correction |
|---|---|---|
| `API error 404: {"error":"Automation not found","code":"NOT_FOUND"}` | `automationId` bien formé mais inconnu du compte | Résoudre par `search_tags` (étiquette), `list_products` (produit), ou utiliser la constante `"newContact"`. |
| `API error 404: {"error":"Product not found","code":"NOT_FOUND"}` | `productId` bien formé mais inconnu | Résoudre par `list_products`. |
| `API error 404: {"error":"Not found","code":"NOT_FOUND"}` | Message générique. Reçu pour un `lessonId` inexistant, et pour un identifiant **valide mais du mauvais type d'objet** (un ID de formulaire passé à `get_webpage`) | Vérifier d'abord le type d'objet, pas seulement l'existence. La précision du message n'est pas homogène d'un outil à l'autre. |

Un identifiant inexistant, mal typé ou vide donne toujours la même famille d'erreur, jamais un message distinguant « existe mais inaccessible ». **Ce comportement n'a été observé qu'à l'intérieur d'un seul compte** : le cas d'un objet appartenant à un autre compte n'a pas pu être testé.

### Rejets serveur, contrôle de version

| Message exact | Signification | Correction |
|---|---|---|
| `The user edited this in the editor since you last read it, so your update was NOT applied — this protects their changes. Call get_lesson to fetch the current content, re-apply your intended changes on top of it, then call update_lesson again with the new version from that response.` | `version` périmée. L'objet a été modifié depuis la dernière lecture, par un humain ou par un autre agent | Relire, réappliquer les modifications sur le contenu frais, réécrire avec la nouvelle version. **Rien n'a été appliqué, même partiellement.** Le même message, avec les noms d'outils substitués, vaut pour `update_webpage`, `update_blog_post` et `update_form_page`. Il ne contient aucun code machine : détecter ce cas par programme est fragile. |

### Rejets serveur, limites commerciales (`402`)

| Message exact | Signification | Correction |
|---|---|---|
| `API error 402: {"error":"These blocks require a Pro plan: codeHtmlBlock. Upgrade at https://app.tinypages.co/dashboard?showBilling=1","code":"PRO_PLAN_REQUIRED"}` | Bloc de code dans une page **ou une leçon**. Le contrôle porte sur le type de bloc, pas sur l'objet qui le contient | Retirer le bloc, ou passer sur un compte Pro. Le message nomme le bloc en cause. |
| `API error 402: {"error":"The free plan allows 1 paid product. Upgrade to the Pro plan to sell more products — the user can start a free trial at https://app.tinypages.co/dashboard?showBilling=1","code":"UPGRADE_REQUIRED"}` | Quota de produits payants atteint | Réaffecter le produit existant par `update_product`, ou compte Pro. Un produit à prix 0 ne compte pas dans ce quota. |
| `The free plan allows 1 free product (lead magnet).` (`402 UPGRADE_REQUIRED`) | Quota de produits gratuits atteint | Compteur distinct du précédent. Même contournement. |
| `API error 402: {"error":"The free plan allows 10 web pages. Upgrade to the Pro plan to create more web pages — the user can start a free trial at https://app.tinypages.co/dashboard?showBilling=1","code":"UPGRADE_REQUIRED"}` | 11e page web | Réécrire une page existante, ou compte Pro. **Ce quota bloque avant la validation des champs de la page** : un test portant sur `seo` ou un autre champ ne pourra pas aboutir. |
| Refus `402 PRO_PLAN_REQUIRED` sur `send_email` | L'envoi d'email par le canal automatisé exige un plan Pro | Seul le code a été relevé, pas le texte complet. `schedule_email` n'a jamais été testé : son comportement est inconnu. |

### Autres rejets serveur

| Message exact | Signification | Correction |
|---|---|---|
| `API error 400: standalone module is required` | La charge utile de `update_modules` omet le module `standalone` | L'inclure **toujours**, même vide. C'est le conteneur des leçons non affectées. |
| `expected string to have >=1 characters` | Tentative de remise à vide du contexte métier par `update_business_context` | **Impossible par ce canal.** Le minimum est un caractère. La remise à vide se fait dans l'interface. Ne pas écrire dans ce champ pendant une campagne. |

### Absence d'erreur là où on l'attendrait

Ces cas ne produisent **aucun message**. Ce sont les plus dangereux.

| Situation | Ce qui se passe | Ce qu'il faut faire |
|---|---|---|
| Lien de paiement construit sur le `name` d'un coupon | Aucune erreur. La remise ne s'applique pas | Relire le `code` retourné. Règle 1. |
| `remove_tag_from_contact` avec un `tagId` inexistant ou jamais appliqué | `{"tags": [...]}` avec succès, réponse identique à un retrait réel | Vérifier par `list_contacts` avec `tagId`, avant et après. |
| Bouton CTA pointant vers le checkout d'un autre produit | Accepté sans avertissement | Contrôler soi-même la cohérence entre le produit de la page et l'URL du bouton. |
| `update_product` ou `update_form` concurrent | La dernière écriture gagne, silencieusement | Relire juste avant et juste après. Aucun contrôle de version n'existe sur ces deux outils. |
| Objet dont le `status` a changé sans appel identifié | Aucune notification, aucun journal | Relire `status` (jamais `publishedAt`) au début et à la fin de chaque campagne. |

---

## 11. Questions ouvertes

À résoudre avant de s'appuyer sur ce manuel pour une campagne à enjeu.

| Question | Pourquoi elle compte |
|---|---|
| Que fait `schedule_email` sur un compte gratuit ? | Jamais appelé. Une campagne qui programme des envois n'a aucune base. |
| Les quotas et les refus `PRO_PLAN_REQUIRED` disparaissent-ils sur un compte Pro ? | Tout ce manuel décrit un plan gratuit. |
| Un objet appartenant à un autre compte renvoie-t-il `NOT_FOUND` ou un message distinct ? | Un message distinct permettrait l'énumération. Un second compte de test est nécessaire. |
| Une destination de formulaire se déclenche-t-elle réellement à la soumission ? | Seule la configuration a été vérifiée. |
| `update_email` et `update_automation_email` exigent-ils `version` ? | Non testé. Détermine la procédure de mise à jour sur ces deux objets. |
| La valeur `sendHour: "02:00"` stockée sur un `sendDay: 0` a-t-elle un effet réel ? | Un client verrait une heure surprenante dans son tableau de bord. |
| Les `nodeId` de formulaire sont-ils stables après modification du formulaire ? | Toute règle de destination en dépend. |
| Existe-t-il un plafond d'articles de blog, de leçons, de contacts ? | Aucun n'a été atteint aux volumes testés. |
| Quelle est la vraie cause de la publication groupée des dix leçons ? | Non établie, et la plateforme ne tient aucun journal des actions de l'IA. |

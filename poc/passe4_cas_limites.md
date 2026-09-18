# Passe 4 — Cas limites — journal d'exécution

Exécutée le 18 septembre 2026, via le MCP `mcp__Tinypage__*`, sur le compte de test partagé de l'audit. Objectif : ne pas chercher à réussir, mais trouver les bords — bornes de schéma, identifiants invalides, contrôle de concurrence, caractères spéciaux, champs « optionnels » qui ne le sont pas, limites de plan. Chaque ligne porte le paramètre exact envoyé et le message d'erreur verbatim.

**Écart par rapport au plan initial (`00_plan_generation.md`, section 4).** Deux consignes fermes reçues pour cette exécution précise priment sur le plan général et ont réduit son périmètre :
1. Aucun `create_product` réel n'était censé aboutir : « ne crée pas de produit, les quotas du plan gratuit sont saturés ». Les tests de bornes qui exigeaient un produit réellement créé (nom à 90 caractères exacts, `installments` à 2 et 6, `maxQuantity`, `subscriptionTrialDays` omis vs explicite, description omise) n'ont donc pas pu être exécutés jusqu'au bout — seules les tentatives invalides (rejetées avant tout appel serveur) et une tentative valide de confirmation de quota ont été faites.
2. `send_email` et `schedule_email` étaient explicitement interdits, y compris pour confirmer un refus déjà établi. Les tests 25 et 26 du plan (limites de plan sur l'envoi et la programmation d'email) n'ont donc **pas été rejoués** dans cette passe.

Une découverte non prévue par le plan — la publication automatique et groupée de neuf leçons sans qu'aucun `publish_*` n'ait été appelé par cette passe — est documentée en premier dans la section « pièges à connaître », parce qu'elle conditionne la lecture de tous les résultats sur les leçons.

---

## 0. Pièges à connaître avant de commencer

Cette section est écrite pour être reprise telle quelle dans le manuel d'administration.

1. **Le compte de test est partagé entre plusieurs agents de l'audit (A04 et A07), et cela contamine les résultats en temps réel.** Pendant cette passe, sans qu'aucun appel `publish_lesson` ni `publish_all_lessons` n'ait été fait par cette passe, les neuf leçons du produit espace membre réutilisé (`85dd81bd-14f5-4850-92a4-ff5ceab3c0c2`) — les six leçons préexistantes, une septième leçon jamais créée par cette passe (« 7. Manuel complet des outils de composition », apparue entre deux relectures), et les trois leçons de test créées ici — sont toutes passées de `status: "draft"` à `status: "published"` **au même instant exact** (`publishedAt: "2026-09-18T23:16:49.865Z"` pour les neuf). La cause la plus probable est un appel `publish_all_lessons` fait par un autre processus opérant en parallèle sur le même compte. Conséquence pratique : sur un compte de test partagé, **aucun objet en brouillon n'est réellement à l'abri d'une publication tant qu'un autre agent ou utilisateur a accès au même compte** — `publish_all_lessons` publie la totalité des leçons d'un produit sans sélection ni confirmation individuelle, y compris des leçons de test qu'on croyait isolées. Un administrateur ne doit jamais laisser du contenu réellement sensible en brouillon sur un espace de travail partagé.
2. **Le contrôle de version optimiste n'est pas appliqué partout.** `update_webpage`, `update_lesson`, `update_blog_post` et `update_form_page` (le contenu d'une page de formulaire) exigent tous un paramètre `version` et rejettent une valeur périmée. `update_product` et `update_form` (le renommage du formulaire) n'ont **aucun** paramètre `version` dans leur schéma et acceptent silencieusement n'importe quelle mise à jour — pas de protection contre un écrasement concurrent sur un produit ou sur le nom d'un formulaire.
3. **Le message de version périmée n'est pas un code d'erreur, c'est une phrase.** Le catalogue interne évoque un code `STALE_VERSION`, mais le texte réellement renvoyé à l'appelant est en langage naturel et ne contient ce mot nulle part (voir 3.3). Un script qui chercherait à détecter l'échec par correspondance de code plutôt que par le texte du message se trompera.
4. **Beaucoup de bornes numériques et de longueurs sont validées avant tout appel réseau.** `create_product`, `create_lesson`, `create_email`, `create_automation_email`, `create_sales_page` rejettent immédiatement (erreur MCP `-32602`, pas d'appel serveur) un nom trop long, un prix hors borne, un `dripDelayDays` ou un `sendDay` hors borne, un identifiant qui n'est pas un UUID. C'est une bonne nouvelle pour le coût (aucune génération de copie IA n'est déclenchée sur un `productId` invalide dans `create_sales_page`) mais cela veut dire que la borne exacte n'est jamais visible autrement que dans le schéma — il n'y a pas de second filet serveur à observer séparément.
5. **Retirer une étiquette qui n'existe pas, ou qui n'a jamais été appliquée, ne produit aucune erreur.** `remove_tag_from_contact` renvoie toujours `{"tags": [...]}` avec succès, que l'étiquette ait réellement été retirée, qu'elle n'ait jamais été appliquée à ce contact, ou que l'identifiant d'étiquette n'existe même pas dans le compte. Impossible de diagnostiquer un échec de ce côté sans relire l'état du contact avant et après.
6. **Le code d'un coupon n'est jamais normalisé.** Minuscules ou espace sont rejetés tels quels (`must match pattern /^[A-Z0-9]+$/`), TinyPages ne met jamais en majuscules ni ne retire les espaces à la place de l'appelant.
7. **Un bouton CTA n'est jamais vérifié contre le produit réel de la page.** On peut poser un bouton pointant vers le checkout de n'importe quel produit, sur une page qui n'est liée à aucun produit ou à un produit différent, sans le moindre avertissement.
8. **Les quotas du plan gratuit ne sont pas un simple « 1 produit / 1 page » binaire.** Le produit payant est plafonné à 1, mais un produit à prix 0 ne compte pas dans ce quota (le compte de test avait déjà un produit membre gratuit ET un produit payant, seul un second produit payant a été refusé). Les pages web sont plafonnées à 10 (compte déjà au maximum). Le blog, lui, n'a montré aucun blocage jusqu'à 3 articles.

---

## 1. Contrôle de concurrence (version optimiste) — tableau de synthèse

| Outil | `version` requis par le schéma | Version correcte testée | Version périmée testée | Message exact au rejet |
|---|---|---|---|---|
| `update_webpage` | Oui | Oui — succès | Oui — rejeté | *« The user edited this in the editor since you last read it, so your update was NOT applied — this protects their changes. Call get_webpage to fetch the current content, re-apply your intended changes on top of it, then call update_webpage again with the new version from that response. »* |
| `update_blog_post` | Oui | Oui — succès | Oui — rejeté | Message identique mot pour mot, avec `get_blog_post` / `update_blog_post` à la place de `get_webpage` / `update_webpage` |
| `update_form_page` (contenu d'une page de formulaire) | Oui | Oui — succès | Oui — rejeté | Message identique mot pour mot, avec `get_form` / `update_form_page` |
| `update_lesson` | Oui (même schéma : `productId`, `lessonId`, `version` requis) | Oui — succès | Non testé (voir limites, § 5) | Non observé directement ; comportement identique attendu par cohérence de schéma, non confirmé en direct |
| `update_form` (renommage du formulaire seul) | **Non** — pas de champ `version` dans le schéma | Non exercé en écriture (schéma seul analysé) | s. o. | s. o. |
| `update_product` | **Non** — pas de champ `version` dans le schéma | Oui — succès sans version (voir § 2) | s. o. — aucun contrôle à mettre en défaut | s. o. |

**Conséquence pour l'administrateur.** Deux familles d'objets (page, article de blog, page de formulaire, leçon) sont protégées contre l'écrasement concurrent ; deux autres (produit, nom de formulaire) ne le sont pas du tout. Un produit modifié en même temps par deux personnes (ou par un humain et l'IA) verra la dernière écriture gagner silencieusement, sans avertissement ni conflit signalé.

---

## 2. Bornes des schémas

| # | Test | Paramètre exact envoyé | Résultat | Message d'erreur textuel complet | Conséquence pratique |
|---|---|---|---|---|---|
| 1 | Nom de produit à 91 caractères | `create_product(name="POC-P4-Nom qui dépasse la limite autorisée de quatre-vingt-dix caractères pour vérifier le comportement exact XX", price=10, accessType="download", paymentMode="oneTime")` | Rejeté avant tout appel réseau | `MCP error -32602: Input validation error: Invalid arguments for tool create_product: Too big: expected string to have <=90 characters at name` | Le nom à 90 caractères exacts n'a pas pu être vérifié séparément (quota produit payant déjà atteint, voir § 4) ; on sait seulement que 91 est refusé proprement. |
| 2 | Prix à 20001 | `create_product(name="POC-P4-Prix hors borne", price=20001, accessType="download", paymentMode="oneTime")` | Rejeté avant tout appel réseau | `MCP error -32602: ... Too big: expected number to be <=20000 at price` | Même limite : 20000 exact non testé séparément (quota). |
| 3 | `installments` à 7 | `create_product(name="POC-P4-Versements hors borne", price=300, accessType="download", paymentMode="installments", installments=7, installmentAmount=43)` | Rejeté avant tout appel réseau | `MCP error -32602: ... Too big: expected number to be <=6 at installments` | 2 et 6 (bornes valides) non testés séparément (quota). |
| 4 | `dripDelayDays` à 0 explicite | `create_lesson(productId=85dd81bd…, name="8. Question(s) fréquente(s) : ça, où, à quel prix ?", content=…, dripDelayDays=0)` | Accepté, mais **stocké comme `null`**, pas comme `0` | Aucun (succès) | 0 explicite et omission produisent exactement le même résultat stocké — confirmé par re-lecture (`get_lesson` → `"dripDelayDays": null`). Aucune différence observable entre les deux. |
| 5 | `dripDelayDays` à 365 (borne haute) | `create_lesson(productId=85dd81bd…, name="9. Palier de délai maximal", dripDelayDays=365)` | Accepté, stocké fidèlement à `365` | Aucun (succès) | La borne haute valide fonctionne et est bien conservée telle quelle (contrairement à `0`). |
| 6 | `dripDelayDays` à 366 | `create_lesson(productId=85dd81bd…, name="10. Palier de délai hors borne", dripDelayDays=366)` | Rejeté avant tout appel réseau | `MCP error -32602: ... Too big: expected number to be <=365 at dripDelayDays` | Rejet propre et immédiat. |
| 7 | `sendDay` à 180 (borne haute), `sendHour` omis | `create_automation_email(automationId=952b281d… [étiquette], subject="POC-P4-Automatisation J+180", content=…, sendDay=180)` | Accepté | Aucun (succès) | `sendHour` stocké à `"09:00"` par défaut, exactement comme documenté (confirmé par `get_automation_email`). |
| 8 | `sendDay` à 181 | `create_automation_email(automationId=952b281d…, subject="POC-P4-Automatisation J+181 invalide", content=…, sendDay=181)` | Rejeté avant tout appel réseau | `MCP error -32602: ... Too big: expected number to be <=180 at sendDay` | Rejet propre et immédiat. |
| 9 | `preview` d'email à 150 caractères exacts | `create_email(subject="POC-P4-Aperçu à la limite exacte", preview=<150 car.>, content=…)` | Accepté | Aucun (succès), id `85add0fe-d07e-47a4-b20e-cc9f09cda65c` | Borne haute valide acceptée. |
| 10 | `preview` d'email à 151 caractères | `create_email(subject="POC-P4-Aperçu trop long", preview=<151 car.>, content=…)` | Rejeté avant tout appel réseau | `MCP error -32602: ... Too big: expected string to have <=150 characters at preview` | Rejet propre et immédiat. |

**Tests non exécutés par manque de quota produit disponible** (voir § 4) : nom de produit à exactement 90 caractères, prix à exactement 20000, `installments` à 2 et 6, `maxQuantity: 1`, `subscriptionTrialDays` omis vs `0` explicite sur un produit, `description` omise sur un produit. Pour tous ces cas, seule une tentative valide de contournement (§ 4, test « quota ») a permis de confirmer que le blocage est un quota de compte, pas une validation de schéma supplémentaire — mais la valeur de la borne elle-même n'a pas pu être revérifiée expérimentalement au-delà de ce que le schéma déclare.

---

## 3. Identifiants invalides

| # | Test | Paramètre exact envoyé | Résultat | Message d'erreur textuel complet | Conséquence pratique |
|---|---|---|---|---|---|
| 1 | `productId` syntaxiquement invalide dans `create_sales_page` | `create_sales_page(name="POC-P4-Page de vente id invalide", productId="abc-123", messageBrief=…, pageStructure=…, copy=…, content=…)` | Rejeté avant tout appel réseau, **avant toute génération de copie** | `MCP error -32602: Input validation error: Invalid arguments for tool create_sales_page: Invalid UUID at productId` | Aucun coût de génération IA gaspillé sur un identifiant invalide — le filtre agit en amont. |
| 2 | `automationId` = UUID bien formé mais inexistant | `create_automation_email(automationId="11111111-1111-4111-8111-111111111111", subject="POC-P4-Automatisation id inexistant", content=…, sendDay=0)` | Rejeté côté serveur (format valide, donc appel réseau effectué) | `API error 404: {"error":"Automation not found","code":"NOT_FOUND"}` | Distinction nette entre « format invalide » (rejet client, message « Invalid UUID », § ci-dessus) et « ressource introuvable » (rejet serveur, code `NOT_FOUND`). |
| 3 | `contactId` syntaxiquement invalide dans `add_tag_to_contact` | `execute_action(add_tag_to_contact, {contactId:"not-a-uuid", tagId:"952b281d…"})` | Rejeté avant tout appel réseau | `Invalid parameters for "add_tag_to_contact": - contactId: Invalid UUID` | Même comportement que les outils directs : les actions du catalogue valident aussi le format côté client. |
| 4 | Identifiant vide (`""`) transmis à un outil de lecture | `get_webpage(id="")` | Rejeté avant tout appel réseau | `MCP error -32602: Input validation error: Invalid arguments for tool get_webpage: Invalid UUID at id` | Une chaîne vide est traitée exactement comme un UUID malformé, pas comme un cas particulier « identifiant manquant ». |
| 5 | `lessonId` bien formé mais inexistant, sur un outil de **lecture** | `get_lesson(productId=85dd81bd…, lessonId="33333333-3333-4333-8333-333333333333")` | Rejeté côté serveur | `API error 404: {"error":"Not found","code":"NOT_FOUND"}` | Message générique, moins précis que pour `update_product` (ci-dessous). |
| 6 | `productId` bien formé mais inexistant, sur `update_product` (outil **sans** contrôle de version) | `update_product(productId="44444444-4444-4444-8444-444444444444", price=5)` | Rejeté côté serveur | `API error 404: {"error":"Product not found","code":"NOT_FOUND"}` | Message plus spécifique (« Product not found ») que celui de `get_lesson` (« Not found » générique) — la précision du message n'est pas homogène d'un outil à l'autre. |
| 7 | Cross-type : ID d'un formulaire transmis à `get_webpage` | `get_webpage(id="17c02675-ebc6-464d-86e4-5ce641644252")` [c'est l'ID du formulaire réutilisé, pas d'une page] | Rejeté côté serveur | `API error 404: {"error":"Not found","code":"NOT_FOUND"}` | Un identifiant valide mais du mauvais type d'objet est traité exactement comme un identifiant inexistant — aucune fuite n'indique « cet ID existe mais désigne autre chose ». |

**Point de sécurité.** Dans tous les cas testés au sein d'un seul et même compte, la réponse à un identifiant inexistant, mal typé ou vide est toujours une erreur « introuvable » (`NOT_FOUND`), jamais un message distinguant « ça existe mais vous n'y avez pas accès ». C'est une bonne pratique qui évite l'énumération d'identifiants appartenant à d'autres comptes — mais cette passe ne pouvait pas le vérifier réellement puisqu'elle ne disposait que d'un seul compte : il faudrait un second compte de test pour confirmer qu'un objet appartenant à un tiers renvoie la même erreur `NOT_FOUND` plutôt qu'un message différent qui révélerait son existence.

---

## 4. Limites de plan

| # | Test | Paramètre exact envoyé | Résultat | Message d'erreur textuel complet | Conséquence pratique |
|---|---|---|---|---|---|
| 1 | Bloc de code dans une page (déjà connu, reproductibilité vérifiée) | `update_webpage(id=94ede07c…, version=<version courante valide>, content=<… avec un `<div data-type="codeHtmlBlock">`…>)` | Refusé | `API error 402: {"error":"These blocks require a Pro plan: codeHtmlBlock. Upgrade at https://app.tinypages.co/dashboard?showBilling=1","code":"PRO_PLAN_REQUIRED"}` | Confirmé reproductible ; message légèrement plus précis que celui rapporté jusqu'ici (nomme le bloc en cause). |
| 2 | `send_email` | — | **Non exécuté** | s. o. | Consigne ferme reçue pour cette passe : ne jamais appeler `send_email`, même pour confirmer un refus déjà établi (M-011). Non rejoué intentionnellement. |
| 3 | `schedule_email` | — | **Non exécuté** | s. o. | Même consigne. Le comportement de `schedule_email` sur plan gratuit reste **non confirmé** par cette passe — seule une hypothèse par analogie avec `send_email` (402 `PRO_PLAN_REQUIRED` probable) peut être avancée. À vérifier explicitement dans une passe future si la consigne est levée. |
| 4 | Second produit **payant** | `create_product(name="POC-P4-Été à l'écran & vous", price=10, accessType="download", paymentMode="oneTime")` | Refusé | `API error 402: {"error":"The free plan allows 1 paid product. Upgrade to the Pro plan to sell more products — the user can start a free trial at https://app.tinypages.co/dashboard?showBilling=1","code":"UPGRADE_REQUIRED"}` | **Nouveau chiffre confirmé : 1 produit payant maximum sur le plan gratuit.** Un produit à prix 0 (le produit espace membre réutilisé, prix 0) ne compte apparemment pas dans ce quota : le compte avait déjà 1 produit payant (« Test », 100 €) ET 1 produit gratuit avant ce test. |
| 5 | 11ᵉ page web | `create_webpage(name="POC-P4-Page sans SEO", content=…)` [sans objet `seo`] | Refusé | `API error 402: {"error":"The free plan allows 10 web pages. Upgrade to the Pro plan to create more web pages — the user can start a free trial at https://app.tinypages.co/dashboard?showBilling=1","code":"UPGRADE_REQUIRED"}` | **Nouveau chiffre confirmé : 10 pages web maximum sur le plan gratuit.** Effet de bord : impossible de vérifier le comportement du champ `seo` omis (test prévu au § 6), le blocage de quota intervient avant. |
| 6 | 3ᵉ article de blog | `create_blog_post(name="POC-P4-Article de test des limites", content=…)` | **Accepté** | Aucun (succès), id `a50d11c4-df24-49cd-b289-91b671bf7a95` | Aucun quota de blog atteint avec 3 articles au total sur le compte (2 préexistants + celui-ci). Le plafond, s'il existe, est strictement supérieur à 3 — non déterminé par cette passe. |
| 7 | Étiquette, coupon (format valide), contact, leçon | Diverses (voir §§ 2, 3, 5, 6) | Toutes acceptées quand le format était valide | Aucun | Aucun quota rencontré sur ces familles d'objets dans les volumes testés ici (une poignée d'unités). Non déterminé au-delà de ce volume. |

---

## 5. Champs déclarés optionnels — vérification réelle

| # | Test | Paramètre exact envoyé | Résultat | Message / observation | Conséquence pratique |
|---|---|---|---|---|---|
| 1 | `create_lesson` sans `content` | `create_lesson(productId=85dd81bd…, name="7. Contenu vide")` [pas de `content`] | Accepté | Succès, leçon créée | Le champ est réellement optionnel, **mais** le résultat n'est pas un contenu vide au sens strict : relu via `get_lesson`, `draftContent` vaut `"<h1 style=\"text-align: left\"></h1>"` — un titre H1 vide inséré par défaut, pas une absence totale de contenu. Un administrateur qui publierait cette leçon telle quelle verrait un titre vide affiché. |
| 2 | `create_email` sans `preview` | `create_email(subject="POC-P4-Sans aperçu", content=…)` | Accepté | Succès, id `abcddb74-e475-476d-9b2b-15dec2e7957a` | Champ réellement facultatif, aucun comportement caché. |
| 3 | `create_webpage` sans objet `seo` | `create_webpage(name="POC-P4-Page sans SEO", content=…)` | **Non concluant** | Bloqué par le quota de 10 pages avant d'atteindre la validation du champ `seo` (voir § 4) | Impossible de confirmer les valeurs par défaut appliquées à `seo` quand il est omis, dans cette passe. À refaire avec une page disponible ou sur un compte Pro. |
| 4 | `create_product` sans `description` | Tentative confondue avec le test de quota (§ 4, produit payant) : `create_product(name="POC-P4-Été à l'écran & vous", price=10, accessType="download", paymentMode="oneTime")` [pas de `description`] | **Non concluant** | Bloqué par le quota de 1 produit payant avant de pouvoir observer ce champ | Même limite que ci-dessus : le champ n'a pas pu être testé isolément. |
| 5 | `create_automation_email` sans `sendHour`, avec `sendDay > 0` | `create_automation_email(automationId=952b281d…, subject="POC-P4-Automatisation J+180", sendDay=180)` [pas de `sendHour`] | Accepté | `get_automation_email` confirme `"sendHour": "09:00"` | La valeur par défaut documentée (`09:00`) est bien appliquée exactement, pas une heure aléatoire. |
| 6 | `dripDelayDays` omis vs `0` explicite sur une leçon | Comparaison entre le test 1 ci-dessus (omis) et le test « 8. Question(s)… » (`dripDelayDays=0` explicite, § 2) | Résultat identique | Les deux sont relus comme `"dripDelayDays": null` | Confirme, sur les leçons, que l'omission et la valeur `0` explicite produisent exactement le même état stocké — aucune différence de comportement entre les deux. |

**Écarts confirmés entre schéma annoncé et comportement réel repérés dans cette passe**, en plus des deux précédents déjà connus (`update_business_context` : `context` annoncé vs `text` attendu ; `create_coupon` : champs `name`/`type`/`value` au lieu de `code`/`discountType`/`discountValue`) :
- Aucun nouvel écart de **nommage** de champ n'a été trouvé (tous les champs testés portaient bien le nom annoncé par le schéma).
- En revanche, un écart de **comportement** a été trouvé : `create_lesson` sans `content` ne produit pas un contenu réellement vide mais un H1 vide par défaut — la documentation dit « omit for an empty lesson », ce qui laisse entendre une leçon sans aucun contenu, alors que le résultat contient une coquille de balise.

---

## 6. Caractères et formats

| # | Test | Paramètre exact envoyé | Résultat | Observation | Conséquence pratique |
|---|---|---|---|---|---|
| 1 | Nom de produit avec accents, apostrophe et esperluette | `create_product(name="POC-P4-Été à l'écran & vous", …)` | **Non concluant sur l'encodage** (bloqué par le quota de produit payant, § 4) | Le rejet est un `402 UPGRADE_REQUIRED`, pas une erreur d'encodage ou de caractères — les caractères spéciaux n'ont donc provoqué aucun rejet de format en amont du quota. | On sait seulement que les caractères spéciaux passent la validation de schéma (sinon l'erreur aurait été un rejet `-32602` avant même d'atteindre le quota serveur) ; leur stockage réel n'a pas pu être vérifié faute de produit créé. |
| 2 | Titre de leçon avec chiffres et ponctuation limite | `create_lesson(name="8. Question(s) fréquente(s) : ça, où, à quel prix ?", …)` | Accepté et conservé fidèlement | `get_lesson` renvoie le même titre caractère pour caractère, accents et ponctuation compris | Aucun problème d'encodage sur les titres de leçon, y compris avec parenthèses, deux-points, virgules et point d'interrogation. |
| 3 | Citation avec guillemets français « » et anglais " " mélangés, apostrophes typographiques | `update_webpage(…, content=<… « Le cadre ne ment jamais… » — … "The frame never lies…" …>)` | Accepté | Contenu enregistré avec succès (nouvelle version retournée : `2026-09-18T23:19:22.656Z`) | Aucun échappement cassé constaté ; les apostrophes droites saisies dans la requête sont stockées comme entité `&apos;` de façon cohérente avec le reste du contenu existant de la page (comportement déjà présent avant modification, pas introduit par ce test). |

---

## 7. Champs déclarés optionnels côté format de coupon — normalisation

| # | Test | Paramètre exact envoyé | Résultat | Message d'erreur textuel complet | Conséquence pratique |
|---|---|---|---|---|---|
| 1 | Code de coupon en minuscules | `execute_action(create_coupon, {name:"POC-P4-Coupon minuscule", type:"percentage", value:10, code:"poctest1"})` | Rejeté | `Invalid parameters for "create_coupon": - code: Invalid string: must match pattern /^[A-Z0-9]+$/` | Aucune normalisation automatique en majuscules — le code doit déjà être conforme avant l'appel. |
| 2 | Code de coupon avec espace | `execute_action(create_coupon, {name:"POC-P4-Coupon espace", type:"percentage", value:10, code:"POC TEST"})` | Rejeté | Même message exact | Aucun retrait automatique des espaces — même conclusion. |

---

## 8. Étiquettes et contacts — cycle complet

| # | Test | Paramètre exact envoyé | Résultat | Observation | Conséquence pratique |
|---|---|---|---|---|---|
| 1 | Création d'une étiquette | `execute_action(create_tag, {name:"POC-P4-Cas-Limite"})` | Succès | `{"tag":{"id":"f9567590-56f5-4e81-80ea-b824ada7fd9e","name":"POC-P4-Cas-Limite"},"created":true}` | Simple et immédiat. |
| 2 | Contact créé avec l'étiquette appliquée dès la création | `execute_action(create_contact, {email:"poc.p4.limite@example.invalid", firstName:"POC P4 Limite", tagIds:["f9567590…"]})` | Succès | La réponse contient déjà `"tags":[{"id":"f9567590…","name":"POC-P4-Cas-Limite","addedAt":"2026-09-18T23:18:04.695Z"}]` | L'application de l'étiquette à la création est **immédiate**, pas besoin d'un second appel `add_tag_to_contact`. |
| 3 | Retrait d'une étiquette réellement présente | `execute_action(remove_tag_from_contact, {contactId:"cdbf35d6…", tagId:"f9567590…"})` | Succès | `{"tags":[]}` | Cycle complet confirmé : créer, appliquer, retirer. |
| 4 | Retrait d'une étiquette **jamais appliquée** à un contact réel (test complémentaire, non prévu au plan initial mais découvert utile) | `execute_action(remove_tag_from_contact, {contactId:"05447cbc…" [contact réutilisé], tagId:"22222222-2222-4222-8222-222222222222" [UUID valide mais inexistant dans le compte]})` | **Succès identique**, aucune erreur | `{"tags":[]}` — réponse strictement identique à un retrait réel | **Trou d'observabilité** : impossible de distinguer, à la seule lecture de la réponse, « l'étiquette a été retirée », « le contact ne l'avait pas », ou « cette étiquette n'existe même pas dans le compte ». Un administrateur qui automatise des retraits d'étiquettes ne peut pas se fier au code de retour pour détecter une faute de frappe dans l'ID. |
| 5 | Recherche floue sur un nom d'étiquette partiel et fautif | `execute_action(search_tags, {name:"Cas Limit"})` [sans le tiret final, sans la majuscule, sans le « e » final] | Succès | Retrouve correctement `POC-P4-Cas-Limite` | Bon rapprochement flou, comme annoncé. |

---

## 9. Objets créés ou modifiés pendant cette passe (pour information de nettoyage manuel ultérieur)

Rappel : le canal MCP ne supprime aucun objet principal (produit, page, article, formulaire, leçon, email) — le nettoyage se fera à la main dans le tableau de bord, en filtrant sur le préfixe `POC-P4`. Rien n'a été publié par cette passe.

- **Produits** : aucun créé (3 tentatives invalides rejetées par le schéma, 1 tentative valide rejetée par le quota de 1 produit payant).
- **Leçons créées sur le produit réutilisé `85dd81bd-14f5-4850-92a4-ff5ceab3c0c2`** : « 7. Contenu vide » (`0c6faa96-8db8-475b-81be-eb31d9e10abb`), « 8. Question(s) fréquente(s) : ça, où, à quel prix ? » (`ac6cfe9e-eb4f-4773-a94b-82bc44e0b1f5`), « 9. Palier de délai maximal » (`e1ef380f-7c8d-4123-beab-ed8fcd3cd21b`). Une 4ᵉ tentative (« 10. Palier de délai hors borne ») a échoué (schéma). **Attention** : ces trois leçons, comme les six préexistantes et une septième non créée par cette passe, sont désormais `status: "published"` malgré l'absence de tout appel `publish_*` par cette passe (voir § 0.1).
- **Étiquette** : « POC-P4-Cas-Limite » (`f9567590-56f5-4e81-80ea-b824ada7fd9e`).
- **Contact** : `poc.p4.limite@example.invalid` (`cdbf35d6-72fe-4e1b-821d-c9d4db2c71ed`), étiquette retirée en fin de test (ne porte plus l'étiquette).
- **Coupons** : aucun créé (2 tentatives rejetées pour format de code invalide).
- **Article de blog** : « POC-P4-Article de test des limites » (`a50d11c4-df24-49cd-b289-91b671bf7a95`), état `draft`, jamais publié.
- **Emails (brouillons)** : « POC-P4-Aperçu à la limite exacte » (`85add0fe-d07e-47a4-b20e-cc9f09cda65c`), « POC-P4-Sans aperçu » (`abcddb74-e475-476d-9b2b-15dec2e7957a`). Une 3ᵉ tentative (« POC-P4-Aperçu trop long ») a échoué (schéma). Aucun de ces brouillons n'a été envoyé ni programmé.
- **Email d'automatisation (brouillon, sur l'étiquette réutilisée `952b281d-b748-4b3b-8825-adba66f8e7a3`)** : « POC-P4-Automatisation J+180 » (`04507b69-4036-4680-8e28-c3301d113893`). Deux tentatives ont échoué (id d'automatisation inexistant ; `sendDay` hors borne).
- **Page web** : aucune nouvelle page créée (quota de 10 pages atteint). La page réutilisée `94ede07c-a02d-4dd6-968b-6aba321f3e65` a été modifiée deux fois avec succès (citation + bouton CTA vers un produit tiers) ; une 3ᵉ tentative d'ajout d'un bloc de code a échoué (`402 PRO_PLAN_REQUIRED`, donc le contenu final ne contient pas ce bloc). Reste `status: "draft"`.
- **Produit `85dd81bd`** : une mise à jour sans paramètre `version` (`price` renvoyé identique, 0) a été appliquée avec succès, sans aucun changement de valeur réelle.

---

## 10. Limites de cette passe

- Les tests de bornes de schéma pour `create_product` qui exigeaient une création réellement aboutie (nom à 90 caractères exacts, `installments` à 2 et 6, `maxQuantity`, `subscriptionTrialDays`, `description` omise) n'ont pas pu être menés jusqu'au bout : le quota de 1 produit payant était déjà atteint par un autre objet du compte (« Test ») avant même le début de cette passe. Seules les valeurs invalides (rejetées côté schéma, donc indépendantes du quota) ont pu être confirmées.
- `send_email` et `schedule_email` n'ont pas été appelés, sur consigne ferme reçue pour cette passe précise. Le comportement de `schedule_email` sur le plan gratuit — jamais testé dans aucune passe précédente de l'audit — reste donc non confirmé.
- `update_lesson` n'a été testé qu'avec une version correcte, pas avec une version périmée : la découverte, en cours de passe, que le produit support était modifié en parallèle par un autre agent a rendu prudent de limiter les écritures supplémentaires sur cet objet précis, pour ne pas ajouter une seconde source de confusion aux résultats déjà perturbés par la publication groupée inattendue. Le rejet en version périmée sur `update_lesson` reste donc une déduction par cohérence de schéma, pas une observation directe.
- `update_form` (renommage seul, sans `version`) n'a pas été exercé en écriture réelle, seul son schéma a été lu. Son absence de contrôle de version a été déduite du schéma déclaré, pas observée par un test d'écrasement concurrent réel.
- Le test « identifiant appartenant à un autre compte » (pour vérifier si `NOT_FOUND` est aussi renvoyé quand l'objet existe mais appartient à un tiers, ou si un message différent fuite son existence) n'a pas pu être fait : cette passe ne disposait que d'un seul compte de test.
- Le comportement du champ `seo` omis sur `create_webpage`, et celui du champ `description` omis sur `create_product`, n'ont pas pu être observés : les deux tentatives ont été bloquées par un quota de compte (pages, produit payant) avant d'atteindre ce point de la validation.
- Le stockage réel des caractères spéciaux dans un **nom de produit** (accents, apostrophe, esperluette) n'a pas pu être confirmé au-delà du fait qu'ils ne provoquent pas de rejet de schéma : la tentative a été bloquée par le quota de produit payant avant toute écriture réelle.
- La contamination du compte par un autre agent concurrent (§ 0.1) rend imprécise l'attribution de certains horodatages et objets (en particulier la 7ᵉ leçon apparue sans avoir été créée par cette passe) : ce point est documenté comme observation factuelle, pas comme certitude sur son origine exacte.

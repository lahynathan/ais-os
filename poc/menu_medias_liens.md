# POC — Médias et liens TinyPages via le canal MCP

Date : 18 septembre 2026. Compte de test : Nathan Lahy (`nathan-lahy.tinypages.co`), plan gratuit, locale `fr`. `MANUEL_ADMINISTRATION.md` lu avant tout appel.

État du compte au démarrage de ce POC (hérité des passes précédentes, non provoqué par ce POC) :
- `list_webpages` : **11 pages déjà présentes** (5 pages système publiées — Accueil, Produits, À propos, Conditions, Confidentialité — + 6 pages de brouillon créées par des passes antérieures, dont deux déjà préfixées `POC`).
- `list_images` : 2 images déjà présentes, dont `poc-assiette-nette` (voir §1).
- `list_videos` : bibliothèque vide.
- Aucun lien existant (`list_links` renvoie `[]`).

Convention de préfixe respectée : `POC-LIEN-` pour le lien créé ; aucune page neuve n'a pu être créée (quota, voir §3) donc le test `<ai-img>` a été fait par une modification **réversible et documentée** d'une page déjà préfixée `POC`, restaurée à l'identique en fin de test (voir §3.2).

---

## 1. Le compte contient-il déjà des images ? La vignette `poc-assiette-nette-*` apparaît-elle ?

| Action | Paramètres | Résultat | Erreur | Enseignement |
|---|---|---|---|---|
| `list_images` | `{}` | 2 images renvoyées : `poc-assiette-nette` (`.../images/poc-assiette-nette-wy1npegk.webp`, créée `2026-09-18T23:08:01.237Z`) et `ai-image-atelier-photographe-lumiere-naturelle-gbg95o` (créée `2026-09-18T23:10:24.762Z`) | Aucune | **Oui, la vignette générée automatiquement pour un produit apparaît dans `list_images`.** Une image générée par l'IA — que ce soit la vignette d'un produit ou un `<ai-img>` de page — entre dans la même bibliothèque que les images téléversées à la main. Rien ne distingue les deux origines dans les champs renvoyés (pas de champ `source` ou `generatedBy`) : seul le nom porte la trace de son origine (le préfixe `ai-image-` pour un `<ai-img>`, un nom libre pour une vignette de produit ou un import). |
| `get_image` | `{"id":"0c163685-220a-4432-8389-6c2f268022ed"}` | Renvoie `url`, `name`, `path`, `createdAt` — même contenu qu'une entrée de `list_images` | Aucune | `get_image` n'apporte aucun champ supplémentaire par rapport à `list_images` sur ce compte (pas de dimensions, pas de poids fichier, pas de type MIME). |
| `search_images` | `{"name":"assiette"}` | 1 résultat : `poc-assiette-nette` | Aucune | Recherche floue fonctionnelle sur un mot partiel du nom. |

**Conclusion documentée.** Le compte contenait déjà, avant toute action de ce POC, une image générée par l'IA pour la vignette d'un produit de test (`poc-assiette-nette-wy1npegk.webp`) et une seconde image générée pour un `<ai-img>` d'une page antérieure (`ai-image-atelier-photographe-lumiere-naturelle-gbg95o`). Ceci confirme, avant même le test volontaire du §3, que **la génération d'image par l'IA alimente durablement la bibliothèque du créateur**, sans distinction visible entre une image demandée explicitement par un opérateur humain et une image produite par un agent IA pilotant le compte.

---

## 2. Peut-on téléverser une image par le canal automatisé ?

| Action | Paramètres | Résultat | Erreur | Enseignement |
|---|---|---|---|---|
| `search_actions` | `"upload an image file to the media library"` | 8 résultats : `list_images`, `get_image`, `add_tag_to_contact`, `add_member_to_product`, `search_images`, `create_contact`, `add_order_bump`, `send_email` | Aucune (recherche, pas d'erreur) | **Aucune action de téléversement d'image dans les résultats**, malgré une requête ciblée. Le moteur de recherche du catalogue renvoie ses meilleurs candidats même sans correspondance réelle — aucun n'est un import de fichier. |
| `search_actions` | `"upload a file or attach a document to the media library from a local file"` | 8 résultats, dont `list_images`, `list_videos`, `update_business_context`, `schedule_email`, `remove_tag_from_contact` | Aucune | Deuxième formulation, même absence confirmée. |

**Conclusion documentée : limite structurante confirmée.** Le catalogue interne (104 actions relevées par le manuel, recoupées ici par deux recherches indépendantes) **ne contient aucune action d'import ou de téléversement d'image**. Les seules voies pour faire entrer une image dans la bibliothèque, par le canal automatisé, sont :
1. la génération par la balise `<ai-img>` dans le contenu d'une page ou d'une leçon (voir §3), qui crée une image *à partir d'une description textuelle*, jamais à partir d'un fichier fourni par l'utilisateur ;
2. la vignette de produit générée automatiquement à la création d'un produit (observée en amont de ce POC, §1).

Il n'existe **aucun moyen, par le canal automatisé, de faire entrer dans la bibliothèque une photo réelle fournie par le créateur** (photo de lui-même, de son local, d'un produit physique, capture d'écran, logo de marque). Tous les blocs image des pages, leçons et emails (`<img src="IMAGE_URL">`) exigent une URL — soit une image de la bibliothèque (donc, en pratique, uniquement générée par IA sur ce canal), soit une URL externe déjà publique que l'utilisateur doit fournir lui-même. Un créateur qui veut mettre sa propre photo sur une page **doit passer par l'interface web**, hors du canal automatisé.

---

## 3. `<ai-img>` : l'image générée rejoint-elle la bibliothèque, sous quel nom ?

### 3.1 Contrainte de quota rencontrée avant le test

| Action | Paramètres | Résultat | Erreur | Enseignement |
|---|---|---|---|---|
| `create_webpage` | `name: "POC-MEDIA-Test image générée"`, contenu avec un `<ai-img>` | — | `API error 402: {"error":"The free plan allows 10 web pages. Upgrade to the Pro plan to create more web pages...","code":"UPGRADE_REQUIRED"}` | **Le compte comptait déjà 11 pages avant cette tentative** (5 pages système + 6 brouillons de passes antérieures), donc au-delà du quota de 10 annoncé par le message d'erreur. Ceci établit que **les pages système par défaut (Accueil, Produits, À propos, Conditions, Confidentialité) comptent dans le quota de 10 pages du plan gratuit** : un compte neuf part donc avec seulement 5 pages de marge réelle avant refus, pas 10. Le quota bloque même quand il est déjà dépassé (pas seulement à l'atteinte exacte de la limite), confirmant qu'aucune création de page n'est plus possible sur ce compte sans dépublier/nettoyer dans l'interface ou passer en Pro. |

Conséquence : la consigne « créer une page préfixée `POC-MEDIA-` » n'a **pas pu être exécutée telle quelle**. Un contournement minimal et réversible a été appliqué : ajout temporaire d'un `<ai-img>` à la fin d'une page déjà préfixée `POC` et la plus courte/la plus isolée disponible (`POC Merci diagnostic cadrage`, contenu statique sans bloc dépendant d'un autre test en cours), puis restauration exacte du contenu d'origine une fois l'observation faite. Aucun objet non préfixé `POC` n'a été modifié.

### 3.2 Test du `<ai-img>` par modification réversible d'une page déjà préfixée POC

| Action | Paramètres | Résultat | Erreur | Enseignement |
|---|---|---|---|---|
| `get_webpage` | `id: "5d288e61-…"` (POC Merci diagnostic cadrage) | Contenu et `updatedAt` relevés avant écriture | Aucune | Lecture avant écriture appliquée conformément au manuel. |
| `update_webpage` | contenu d'origine + `<ai-img filename="poc-media-test-assiette" data-width="50" ...>Une assiette blanche nette...</ai-img>`, `version` = `updatedAt` relevé | Écriture acceptée, nouvelle `version` renvoyée dans le texte de réponse (`2026-09-18T23:35:03.640Z`) | Aucune | La résolution du `<ai-img>` en `<img>` réel s'est faite **de façon synchrone, dans le même appel** : la réponse ne signale aucune étape asynchrone, et une relecture immédiate montre déjà le HTML final. |
| `get_webpage` (relecture immédiate) | `id: "5d288e61-…"` | `draftContent` montre que le `<ai-img>` a été remplacé par `<img src="https://…/images/ai-image-poc-media-test-assiette-ifar7r.webp" …>` | Aucune | **Le nom de fichier généré suit le gabarit `ai-image-<filename donné>-<suffixe aléatoire de 6 caractères>`.** Le `filename` fourni dans la balise (`poc-media-test-assiette`) est repris tel quel dans le nom stocké, préfixé de `ai-image-`. |
| `list_images` (juste après) | `{}` | **2 images seulement** — la nouvelle image générée n'y figure pas encore | Aucune | **Délai d'indexation observé.** L'image existe déjà (URL valide, servie, visible dans `draftContent`) mais n'apparaît pas immédiatement dans `list_images`. |
| `list_images` (nouvel appel, ~15 secondes plus tard) | `{}` | **3 images** — `ai-image-poc-media-test-assiette-ifar7r` apparaît, avec un `createdAt` (`2026-09-18T23:35:15.127Z`) postérieur d'environ 12 secondes à l'appel `update_webpage` | Aucune | **Confirmé : l'image générée par `<ai-img>` rejoint bien la bibliothèque du créateur**, mais avec un **délai d'indexation de l'ordre de 10 à 15 secondes** entre la résolution visible dans le contenu de la page et son apparition dans `list_images`. Un agent qui vérifierait `list_images` immédiatement après une génération pourrait conclure à tort que l'image n'a pas été enregistrée. |
| `search_images` | `{"name":"poc-media-test-assiette"}` | Trouve l'image dès le même appel où `list_images` la montrait déjà | Aucune | Cohérent avec `list_images` une fois l'indexation faite ; pas de canal de recherche plus rapide observé. |
| `update_webpage` | contenu restauré à l'identique de l'original, `version` = celle renvoyée par l'écriture précédente | Page restaurée avec succès, nouvelle `version` renvoyée | Aucune | La page test est revenue à son état initial. L'image générée, elle, **reste dans la bibliothèque** : voir §5, aucune action de suppression n'existe. |

**Réponse à la question.** Oui : une image générée par `<ai-img>` dans le contenu d'une page (et, par déduction documentaire — non testé directement faute de quota de leçon disponible pour un nouveau produit — vraisemblablement aussi dans une leçon, le mécanisme de génération étant décrit de façon identique dans les deux outils) **rejoint la bibliothèque du créateur**, sous le nom `ai-image-<filename>-<suffixe aléatoire>`, avec un court délai d'indexation avant d'apparaître dans `list_images`/`search_images`. Combiné au constat du §1, ceci confirme que **toute génération d'image par l'IA, quel que soit l'objet qui la déclenche (page, leçon, vignette de produit), grossit silencieusement et durablement la bibliothèque du compte**, sans étiquette d'origine ni possibilité de suppression par ce canal.

---

## 4. Les liens : `create_link`, schéma réel, cycle complet

| Action | Paramètres | Résultat | Erreur | Enseignement |
|---|---|---|---|---|
| `search_actions` | `"create a short link or tracking link"` | Schéma de `create_link` obtenu : `slug` (requis, 1-100 car., auto-slugifié), `destination` (requis, URL http(s) absolue, max 2048 car.), `name` (optionnel, max 60 car., étiquette d'affichage) | — | `create_link` sert à créer **un lien court sur le sous-domaine du compte** (`https://<domaine>/<slug>`), pas un lien de suivi tiers ni un raccourcisseur externe. La description est explicite : « incoming query params are merged into the destination » — les paramètres de requête entrants (ex. UTM) sont fusionnés dans l'URL de destination, donc le lien sert autant à raccourcir qu'à faire transiter du tracking vers une URL externe. |
| `create_link` | `{"slug":"POC-LIEN-test","destination":"https://tinypages.co/fr","name":"POC-LIEN-test"}` | — | **`API error 402: {"error":"Pro plan required","code":"PRO_PLAN_REQUIRED"}`** | **Découverte non anticipée par le manuel d'administration** (qui ne documentait pas cette action) : la création de liens courts est **réservée au plan Pro**, avec un blocage total dès le premier appel, quels que soient les paramètres. Le message est minimal — pas de lien vers la page de tarification, pas de mention de compteur, contrairement aux autres refus `402` du compte (produits, pages) qui citent tous l'URL `showBilling=1`. |
| `list_links` | `{}` | `{"data":[],"limit":100,"hasMore":false,"nextCursor":null,"dashboardUrl":"https://app.tinypages.co/go/…?to=%2Fdashboard%2Fweb-pages%2Flinks"}` | Aucune | `list_links` reste accessible en lecture sur le plan gratuit (aucune erreur), même si la création est bloquée : c'est une lecture, pas une écriture, et le catalogue ne restreint pas les lectures par plan sur cette fonction. Le champ `dashboardUrl` confirme que les liens vivent dans l'interface sous `Web pages → Links`, donc rattachés au module Pages, pas à un module « marketing/tracking » séparé. |
| `update_link` | non exécuté | — | — | **Cycle complet non testé** : sans lien créé (création bloquée par le plan), `update_link` n'a pas pu être exercé faute d'un `linkId` réel. Le schéma est connu (`linkId` requis, `slug`/`destination`/`name` tous optionnels, un `name` vide envoyé explicitement l'efface) mais son comportement réel — validation de version, gestion d'un slug déjà pris, etc. — **reste à vérifier sur un compte Pro**. |

**Conclusion documentée : limite structurante confirmée.** Le module « liens » de TinyPages est un **raccourcisseur de liens propre au domaine du compte, avec fusion des paramètres de requête** — pas un système de tracking multi-domaine ni un gestionnaire d'affiliation. Sur le plan gratuit, **il est intégralement inaccessible en écriture par le canal automatisé** : seule la lecture (`list_links`) fonctionne, sur une liste toujours vide puisque rien ne peut y être ajouté. Le cycle complet création → lecture → modification n'a donc pu être vérifié qu'à moitié (schémas confirmés, comportement d'écriture non observé).

---

## 5. Les vidéos : bibliothèque vide, moyen d'y ajouter une vidéo ?

| Action | Paramètres | Résultat | Erreur | Enseignement |
|---|---|---|---|---|
| `list_videos` | `{}` | `{"data":[],"limit":100,"hasMore":false,"nextCursor":null}` | Aucune | **Bibliothèque vide, confirmé.** Aucune vidéo n'a été téléversée ni générée sur ce compte à ce jour. |
| `search_actions` | `"import a video from a URL into the video library or generate a video"` | 8 résultats : `remove_tag_from_contact`, `add_member_to_product`, `list_images`, `update_business_context`, `add_tag_to_contact`, `list_email_recipients`, `schedule_email` — **aucune action vidéo autre que celles déjà connues** | Aucune | Confirme l'absence de toute action de génération ou d'import vidéo. |
| `search_actions` | `"upload a video file to the video library"` (voir §2) | Résultats : `get_video`, `list_videos`, `search_videos`, `add_tag_to_contact`, `add_member_to_product` | Aucune | Les trois seules actions vidéo du catalogue sont **toutes des lectures** (`get_video`, `list_videos`, `search_videos`). |

**Conclusion documentée, confirmée par deux recherches indépendantes en formulations différentes.** Il n'existe, à ce jour, **aucune action d'écriture sur le module vidéo dans le catalogue MCP** — ni téléversement de fichier, ni import depuis une URL externe (YouTube/Vimeo/Loom s'intègrent par un bloc d'embed séparé dans le contenu des pages, mécanisme distinct qui ne fait pas entrer de vidéo dans « la bibliothèque native »), ni génération par IA (contrairement aux images, il n'existe pas d'équivalent `<ai-video>`). **Une vidéo native ne peut entrer dans la bibliothèque que par l'interface web** ; le canal automatisé peut seulement lire ce qui s'y trouve déjà et référencer un `videoId` existant dans un bloc `videoBlock`.

---

## 6. Ce que le canal automatisé ne permet pas de faire (médias et liens)

C'est la synthèse la plus utile de ce POC.

| Ce qui est impossible par le canal MCP | Constat | Conséquence pour un opérateur |
|---|---|---|
| **Téléverser une image existante** (photo, logo, capture d'écran fournis par l'utilisateur) | Aucune action d'upload dans le catalogue (§2), confirmé par deux recherches | Toute image « réelle » (pas générée) doit être mise en ligne à la main dans l'interface, puis seulement référencée par son URL depuis l'agent |
| **Téléverser ou importer une vidéo**, par fichier ou par URL, dans la bibliothèque native | Aucune action d'upload ni d'import dans le catalogue (§5), confirmé par deux recherches | Un module vidéo entièrement vide, sur un compte par ailleurs actif depuis plusieurs passes de test, ne peut pas être peuplé sans quitter le canal automatisé |
| **Générer une vidéo par IA** (pas d'équivalent de `<ai-img>` pour la vidéo) | Absence confirmée dans les schémas d'outils et dans le catalogue | Aucune parité entre les capacités « image » et « vidéo » de la génération assistée |
| **Supprimer une image de la bibliothèque**, y compris une image générée par erreur ou pour un test | Aucune action `delete_image` trouvée dans le catalogue (recherché explicitement, §6 de ce test) ; recoupe la règle 4 du manuel (« rien ne se supprime ») | Chaque test `<ai-img>` laisse une image orpheline et définitive dans la bibliothèque du compte ; ce POC en a laissé une (`ai-image-poc-media-test-assiette-ifar7r`), à retirer manuellement dans l'interface |
| **Supprimer ou dépublier un lien court** | Aucune action `delete_link` trouvée dans le catalogue | Même limite que pour tous les autres objets du compte (page, produit, contact…) : nettoyage uniquement manuel |
| **Créer, modifier ou lire le détail d'un lien court sur le plan gratuit** | `create_link` renvoie `402 PRO_PLAN_REQUIRED` sans condition (§4) | Le module « liens » est, de fait, un module Pro complet : sur un compte gratuit, l'agent ne peut que constater une liste vide, jamais agir dessus |
| **Créer une page de test dédiée quand le quota de pages est atteint ou dépassé** | `create_webpage` a été refusé par `402 UPGRADE_REQUIRED` alors que le compte comptait déjà 11 pages pour un quota annoncé de 10 (§3.1) | Les pages système comptent dans le quota gratuit ; un compte neuf dispose d'une marge réelle de 5 pages, pas 10 ; toute campagne de test doit vérifier ce compteur avant de planifier un test qui suppose la création d'une page |
| **Associer un fichier autre qu'une image ou une vidéo** (PDF, audio) à un contenu, par ce canal | Aucune action de ce type rencontrée dans les recherches menées ; non explorée plus avant, hors périmètre de ce POC | À vérifier séparément si le périmètre s'étend aux fichiers téléchargeables des produits `download` |

**Limite transversale à retenir.** Le canal automatisé sait **lire** l'intégralité des bibliothèques d'images et de vidéos, et **générer** de nouvelles images à partir d'une description texte — mais il ne sait **rien faire entrer d'extérieur** (aucun fichier local, aucune URL importée) ni **rien retirer**. Toute médiathèque pilotée uniquement par l'IA ne peut donc grossir que d'images générées, jamais de contenus fournis par le créateur, et ne peut jamais rétrécir.

---

## Limites de ce test

- `update_link` n'a pas pu être exercé (création bloquée par le plan) : son comportement réel (version, conflits de slug) reste à vérifier sur un compte Pro.
- Le test `<ai-img>` n'a été mené que sur une **page**, pas sur une **leçon** (`create_lesson`/`update_lesson`), faute de quota de produit disponible pour en créer un nouveau et par consigne explicite de ne pas créer de produit. Le mécanisme documentaire de génération est identique dans les deux outils, mais ce n'est pas une observation directe pour les leçons.
- Le délai d'indexation observé (~10-15 s) n'a été mesuré qu'une fois ; il n'est pas garanti que ce délai soit constant.
- Comportement de `create_link`/`update_link` sur un compte Pro : non observé, aucun accès à un tel compte pour ce test.
- Suppression d'image ou de lien : conclusion fondée sur l'absence de résultat dans les recherches du catalogue, pas sur une tentative d'appel direct à une action nommée arbitrairement (impossible sans connaître un `action_id` à tester).

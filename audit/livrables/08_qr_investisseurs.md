# 08 — Questions des investisseurs et de leur auditeur technique

Data room TinyPages · Version 1 du 18 septembre 2026 · Rédigé par l'orchestrateur de l'audit
Sources : `audit/rapports/A01.md` à `A09.md`, `audit/annexes/catalogue_mcp_tinypages.md`, `audit/annexes/screening_mcp_compte_test.md`, `audit/livrables/02` à `05`.
Lecteurs visés : le CEO et le CTO, pour préparer les entretiens de due diligence. **Ce document n'est pas destiné à être remis tel quel aux investisseurs.**

---

## Comment lire ce document

Cinquante questions, écrites comme les poserait un auditeur payé pour trouver des failles, pas comme les poserait un ami. Les plus dures d'abord.

Chaque question porte trois éléments distincts, qu'il ne faut jamais confondre :

1. **Réponse factuelle aujourd'hui** — ce que TinyPages peut affirmer en s'appuyant sur ce que cet audit a réellement établi, avec le niveau de preuve associé ;
2. **Ce qui manque pour répondre pleinement** — l'écart entre cette réponse et une réponse complète ;
3. **Pièce justificative attendue** — le document qui transforme la réponse en preuve, et son responsable.

Lorsque la réponse honnête est mauvaise, elle figure quand même, suivie d'une **formulation à ne pas dépasser** : le libellé le plus favorable qui reste vrai. Une formulation plus flatteuse sera démentie, souvent en quelques minutes, et le démenti coûtera plus cher que l'aveu.

**Conventions de preuve.** **CONFIRMÉ** n'est employé que pour deux sources : l'inventaire du catalogue MCP relevé sur le serveur de production et le screening exécuté sur le compte connecté le 18 septembre 2026. Tout le reste plafonne à **PROBABLE** : l'egress réseau était fermé sur `tinypages.co`, `docs.tinypages.co` et `mcp.tinypages.dev` pendant toute la phase de collecte, aucun test actif n'était autorisé, et aucun accès interne n'a été ouvert. **« Non déterminé à ce jour »** signifie exactement cela : l'audit n'a pas pu l'établir, ce n'est ni un aveu ni une dénégation.

**Une règle tenue partout :** une règle générale (Stripe, RGPD, DSA, TVA) n'est jamais présentée comme un constat sur TinyPages. Les deux registres sont séparés dans chaque réponse.

---

# Bloc A — Les douze questions les plus dures

Ce sont celles dont la réponse est aujourd'hui difficile, incomplète ou défavorable. Un auditeur les trouvera de toute façon. Les préparer est le seul avantage disponible.

---

**Q1 — « Vous dites être la seule plateforme marketing tout-en-un pilotable de bout en bout par Claude. Kajabi, GoHighLevel, ClickFunnels, Stan Store et Systeme.io ont un serveur MCP officiel. Comment maintenez-vous cette affirmation ? »**
*Domaine : marché · Constats : A09-001 à A09-006, A04-026, A04-027*

- **Réponse factuelle aujourd'hui.** L'affirmation n'est pas maintenable en l'état. L'audit a relevé, à partir de sources secondaires convergentes du 18 septembre 2026 (statut PROBABLE, aucune page concurrente n'ayant pu être ouverte), l'existence d'un serveur MCP officiel chez Kajabi (couverture annoncée : pages, emails, offres, cours, contacts, commandes, étiquettes, segments, blog, réglages du site), GoHighLevel (36 outils annoncés, feuille de route revendiquée à plus de 250), ClickFunnels (bêta officielle), Stan Store (`mcp.stan.store`) et Systeme.io (couverture annoncée plus étroite : contacts, étiquettes, cours, abonnements). Un serveur MCP TinyPages hébergé par Zapier existe par ailleurs, distinct du serveur officiel : toute plateforme dotée d'une application Zapier dispose de fait d'un MCP. Ce que l'audit a établi en propre, et que les concurrents ne documentent pas publiquement à ce jour, c'est l'**étendue** de la surface pilotable : 104 actions exposées à un modèle, de la création de la page à la publication et à l'envoi d'email (CONFIRMÉ).
- **Ce qui manque pour répondre pleinement.** Un comparatif fonctionnel daté, action par action, face à Kajabi et GoHighLevel au minimum. Aucune page concurrente n'a pu être ouverte : la couverture réelle de leurs serveurs n'est pas vérifiée. Il manque aussi la réponse à la question qui départage réellement : plusieurs concurrents, dont Kajabi, imposeraient un passage par le brouillon avant publication, ce qui est précisément le garde-fou que TinyPages n'applique pas (voir Q2).
- **Pièce justificative attendue.** Comparatif fonctionnel daté et archivé (captures des pages concurrentes au jour du relevé), rejoué chaque trimestre. **CEO.**
- **Formulation à ne pas dépasser.** « Le pilotage par IA n'est plus une exclusivité : plusieurs concurrents publient un serveur MCP officiel. Nous nous différencions sur l'étendue du périmètre pilotable, mesurée à 104 actions, et sur la continuité du parcours entre l'idée et la mise en ligne. Comparatif daté en annexe. » Toute mention du mot « seule » doit disparaître des supports avant l'ouverture de la data room.

---

**Q2 — « Qu'est-ce qui empêche techniquement votre IA de publier une page sans validation humaine ? »**
*Domaine : IA et MCP · Constats : M-010, M-011, A04-007*

- **Réponse factuelle aujourd'hui.** Rien. Le test a été exécuté le 18 septembre 2026 sur le compte connecté : une page a été créée, puis `publish_webpage` a été appelée **en violant délibérément** la consigne « Do NOT call publish_webpage » inscrite dans la description de l'outil. La publication a réussi immédiatement et le serveur a retourné une URL publique. Aucune confirmation demandée, aucune revue humaine, aucun délai, aucune restriction de plan. **CONFIRMÉ, impact bloquant.** Les mentions « saved as a draft », « the user decides when to publish » sont exclusivement du texte adressé à un modèle que TinyPages ne contrôle pas.
- **Ce qui manque pour répondre pleinement.** Rien, du point de vue du constat : il est établi. Ce qui manque est le correctif — une confirmation appliquée côté serveur, en deux temps, sur la publication, la dépublication, l'envoi et la programmation — et le journal qui permettrait au créateur de voir *a posteriori* ce que l'IA a publié.
- **Pièce justificative attendue.** Code du serveur MCP montrant le mécanisme de confirmation, une fois livré, et le journal d'actions horodaté distinguant acteur humain et acteur IA. **CTO.** D'ici là, la seule pièce honnête est le relevé de test lui-même.
- **Formulation à ne pas dépasser.** « Aujourd'hui, le comportement par défaut du modèle est de rester au brouillon ; ce comportement est porté par les descriptions d'outils, pas par un contrôle d'accès. Un utilisateur, ou un modèle, qui demande explicitement la publication l'obtient. Le correctif — confirmation serveur en deux temps et journal des actions — est en cours et sera livré avant [date]. » Toute formulation présentant ce comportement comme une garantie sera démentie en un appel.

---

**Q3 — « Vos garde-fous sont-ils appliqués par votre serveur ou écrits dans un prompt adressé à un modèle tiers ? »**
*Domaine : IA et MCP · Constats : M-010, M-011, M-014, A04-007, A04-008*

- **Réponse factuelle aujourd'hui.** Écrits dans les descriptions d'outils et dans le champ `instructions` du serveur, à deux exceptions près, et ces deux exceptions sont commerciales. Le screening a établi que le serveur **sait** refuser une action : `create_webpage` avec un bloc `codeHtmlBlock` et `send_email` retournent tous deux `402 PRO_PLAN_REQUIRED`. Il le fait pour deux actions payantes, et pas pour la publication. **CONFIRMÉ.** Le seul garde-fou structurel non commercial est une absence de fonction : aucune action du catalogue ne supprime un contact, un produit, une page, un article, un formulaire, une leçon ou un email — trois suppressions existent, mais sur des objets secondaires (`delete_order_bump`, `delete_upsell`, `delete_automation_stop_condition`).
- **Ce qui manque pour répondre pleinement.** La confirmation que le mur de facturation sur `send_email` n'est pas le seul contrôle : le message d'erreur implique qu'un compte au plan payant enverrait sans autre barrière. Ce test n'a pas été fait. C'est la seule question laissée ouverte par le screening sur ce point.
- **Pièce justificative attendue.** Rejeu du test d'envoi sur un compte au plan payant dédié, avec relevé, et code du serveur pour les actions de publication et d'envoi. **CTO.**
- **Formulation à ne pas dépasser.** « Les contrôles appliqués aujourd'hui côté serveur sont des contrôles de plan. Les garde-fous de sécurité sont des comportements par défaut du modèle. Nous les déplaçons côté serveur. » Ne jamais employer le mot « garantie » avant que ce déplacement soit livré et testé.

---

**Q4 — « Si l'article 9 bis du règlement d'exécution (UE) 282/2011 s'applique, quelle est votre exposition TVA sur le volume vendu par vos créateurs ? »**
*Domaine : paiements et fiscalité · Constats : A02-019, A06-020*

- **Réponse factuelle aujourd'hui.** Non déterminée à ce jour, et c'est le risque le plus lourd du dossier. La règle, elle, est établie : une plateforme par laquelle sont fournis des services électroniques est présumée agir en son nom propre ; cette présomption devient **irréfragable** dès lors que la plateforme autorise la facturation au client, autorise la fourniture, **ou fixe les conditions générales** de la prestation. La Cour de justice a validé le dispositif (aff. C-695/20, Fenix International). Si la présomption s'applique, TinyPages devient redevable de la TVA de chaque pays d'acheteur sur l'intégralité du volume vendu par ses créateurs, et non sur ses seuls abonnements, avec les obligations OSS correspondantes. **Aucun élément de cet audit ne permet de dire si elle s'applique ou non** : les trois faits qui la déclenchent n'ont pas pu être observés.
- **Ce qui manque pour répondre pleinement.** Trois faits, et trois seulement : qui apparaît sur le reçu et sur la facture remis à l'acheteur final ; qui fixe les conditions générales de vente du tunnel d'achat, TinyPages ou le créateur ; qui décide de la mise à disposition du fichier. Plus le volume brut vendu par les créateurs sur douze mois, sans lequel aucune exposition ne peut être chiffrée.
- **Pièce justificative attendue.** Un reçu et une facture réels anonymisés, les CGV du tunnel d'achat, le paramétrage Stripe Connect, et une **note de qualification signée par un avocat fiscaliste**. **CEO + avocat à mandater** (le paramètre `RESPONSABLES` porte « avocat : à mandater » : cette pièce n'a pas de propriétaire à ce jour).
- **Formulation à ne pas dépasser.** « La qualification au regard de l'article 9 bis est en cours d'analyse par un conseil fiscal. Les trois critères déclencheurs sont identifiés et documentés. Nous communiquerons la note de qualification et, le cas échéant, le chiffrage de l'exposition. » Ne jamais affirmer que la présomption ne s'applique pas avant d'avoir la note écrite.

---

**Q5 — « Pourquoi la politique de confidentialité publiée par défaut sur chaque compte client est-elle vide ? »**
*Domaine : conformité · Constat : M-007*

- **Réponse factuelle aujourd'hui.** Le fait est établi, il n'est pas contestable, et il se vérifie en ouvrant un compte. À la création d'un compte, TinyPages crée et **publie automatiquement** cinq pages. Deux sont des documents juridiques dont le contenu intégral se réduit à leur titre : `<h1><strong>Politique de confidentialité</strong></h1>` et `<h1><strong>Conditions d'utilisation</strong></h1>`. Elles sont `published` et `indexed: true` dès la première seconde du compte, sans action du créateur. Dans le même temps, la page d'accueil créée par défaut est publiée et porte un bloc de capture d'emails actif, et le compte porte `doubleOptin: false`. **CONFIRMÉ, relevé sur un compte, à une date.** La combinaison est le constat : la plateforme met en ligne, pour chaque créateur, un formulaire de collecte d'adresses adossé à une politique de confidentialité vide, sans double opt-in, et demande aux moteurs de l'indexer.
- **Ce qui manque pour répondre pleinement.** La confirmation que ce comportement est bien celui de tous les comptes et non une particularité du compte observé ; la position de l'avocat sur la responsabilité de TinyPages en tant que fournisseur du gabarit et sous-traitant au sens de l'article 28 ; et le correctif produit.
- **Pièce justificative attendue.** Capture d'un compte fraîchement créé montrant l'état corrigé, gabarits juridiques réellement remplis ou dépubliés par défaut, `indexed: false` sur ces pages tant qu'elles sont vides, double opt-in activé par défaut. **CTO pour le correctif, avocat pour la qualification.**
- **Formulation à ne pas dépasser.** « Nous avons identifié que les pages légales créées par défaut étaient publiées vides et indexées. C'est un défaut de conception que nous corrigeons : [correctif], livré le [date]. » C'est un constat trouvable en cinq minutes par n'importe quel auditeur ouvrant un compte gratuit : le cacher est impossible, l'anticiper est gratuit.

---

**Q6 — « Vos sites clients et votre application partagent le même domaine enregistrable. Comment isolez-vous les cookies de session ? »**
*Domaine : sécurité · Constats : A05-001 à A05-005, A05-013*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et c'est le point que l'auditeur technique regardera en premier. Ce qui est établi au statut PROBABLE : les hôtes connus (`tinypages.co`, `app.tinypages.co`, `docs.tinypages.co` et les sous-domaines clients) partagent le domaine enregistrable `tinypages.co`. Ce qui n'a pas pu être relevé, l'egress étant fermé : les attributs réels des cookies de session, l'inscription éventuelle de `tinypages.co` à la Public Suffix List, la politique CORS de l'API, et le mécanisme anti-CSRF. La règle générale, qui n'est pas un constat sur TinyPages : sans inscription à la PSL, le navigateur traite tous ces hôtes comme un seul site ; `SameSite=Lax` ne protège alors pas entre un site client et l'application, et une protection CSRF fondée sur `SameSite` seul serait inopérante. Toute l'industrie comparable sépare les deux plans sur deux domaines enregistrables distincts.
- **Ce qui manque pour répondre pleinement.** Un relevé de trente minutes : attributs du cookie de session (préfixe `__Host-`, attribut `Domain`, `Secure`, `HttpOnly`, `SameSite`), recherche de `tinypages` dans la Public Suffix List, configuration CORS de la passerelle, et attribut `sandbox` complet du bloc de code personnalisé avec le domaine enregistrable de son `src`.
- **Pièce justificative attendue.** Relevé navigateur horodaté sur un compte de test, extrait de configuration CORS, et décision documentée sur la séparation des domaines. **CTO.**
- **Formulation à ne pas dépasser.** Si le cookie n'est pas verrouillé : « Le cookie de session est aujourd'hui [état réel]. Le correctif immédiat est son verrouillage en `__Host-` sans attribut `Domain` ; la cible est la séparation des sites clients sur un domaine enregistrable distinct, chantier de [délai], l'inscription à la Public Suffix List ne produisant son effet qu'après diffusion dans les navigateurs. » Ne pas présenter l'inscription à la PSL comme un correctif d'urgence : elle ne l'est pas.

---

**Q7 — « Avez-vous une batterie de tests rejouée à chaque nouveau modèle Claude ? »**
*Domaine : IA et MCP · Constats : A04-030, A04-032*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, aucune trace publique, et il faut considérer par défaut qu'il n'y en a pas. C'est le risque produit le plus structurant du dossier, pour une raison mécanique : les garde-fous observés sont en langage naturel et s'appuient sur un modèle tiers mis à jour sans préavis par Anthropic ou OpenAI. Sans batterie rejouée à chaque version, TinyPages ne peut pas savoir qu'une mise à jour de modèle a cassé « ne publie pas sans accord ». Le screening a d'ailleurs montré que cette consigne ne tient pas même sans changement de modèle (Q2).
- **Ce qui manque pour répondre pleinement.** Tout : la batterie elle-même, un cas par garde-fou (ne pas publier sans accord, ne pas envoyer sans accord, ne pas suivre les instructions contenues dans le champ libre d'un contact, ne pas changer de compte seul), exécutée sur chaque modèle **et chaque client** supporté, avec seuils bloquants et journal de résultats daté.
- **Pièce justificative attendue.** Batterie versionnée, résultats de la dernière exécution avec dates et versions de modèle, et politique de gel de version en cas d'échec. **CTO.**
- **Formulation à ne pas dépasser.** « Nous n'avons pas de batterie d'évals à ce jour. C'est la première pièce de notre plan produit : [contenu], première exécution le [date], résultats publiés dans la data room. » C'est la pièce qui transforme « nous avons écrit une consigne » en « nous mesurons qu'elle tient ». Son absence, annoncée avec un plan, est moins coûteuse qu'une réponse évasive.

---

**Q8 — « Que se passe-t-il pour votre produit si Anthropic ou OpenAI publient nativement des pages hébergées ? »**
*Domaine : marché · Constats : A09-011, A09-012, A04-032*

- **Réponse factuelle aujourd'hui.** Le mouvement est déjà engagé et daté. Claude Code Artifacts, annoncé le 18 juin 2026, publie une page HTML depuis une session et l'héberge sur une URL claude.ai. ChatGPT Sites est en bêta publique depuis le 9 juillet 2026 : construction et hébergement de sites dans ChatGPT, avec deux limites à la date de l'audit — le commerce y est interdit et le service est indisponible dans l'Union européenne, au Royaume-Uni et en Suisse (PROBABLE, sources secondaires). Aucun des deux n'égale aujourd'hui la boîte à outils marketing de TinyPages : paiements, emails, contacts, espace membre. La trajectoire, elle, est lisible. S'y ajoute une dépendance de discours : le positionnement, le slogan et le nom de domaine du serveur MCP reposent sur Claude, marque déposée d'un tiers (A04-031, conclusion juridique à valider par un avocat).
- **Ce qui manque pour répondre pleinement.** Une analyse écrite du périmètre exact de ces deux offres face au périmètre TinyPages, pour cadrer le risque plutôt que le sur- ou sous-estimer ; et un plan de découplage : compatibilité multi-clients réellement testée, et discours de marque qui ne dépend pas d'un fournisseur unique.
- **Pièce justificative attendue.** Note de positionnement datée, matrice de compatibilité par client (Claude, Claude Code, ChatGPT, autres) réellement testée, et le cas échéant l'échange écrit avec Anthropic sur l'usage de la marque. **CEO + CTO.**
- **Formulation à ne pas dépasser.** « L'hébergement natif de pages par les éditeurs de modèles existe depuis juin et juillet 2026, avec un périmètre aujourd'hui limité à la page et sans commerce en Europe. Notre défense n'est pas l'antériorité du MCP, c'est la chaîne complète paiement-email-membre et un harnais auditable. » Ne pas présenter ce risque comme théorique : les deux dates sont publiques.

---

**Q9 — « Quelle est l'entité juridique qui lève des fonds, et qui en sont les fondateurs ? »**
*Domaine : conformité · Constats : A06-025, A09-016, A09-015, A01-005*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et c'est bloquant pour la data room. Les paramètres de l'audit désignent Nathan Lahy comme CEO et CTO. Des sources publiques secondaires présentent Emilio Abril comme fondateur de tinypages.co et un « Jerome V. » comme co-fondateur et CTO. Une société française « TINY PAGE » (SIREN 905170916, Le Havre) apparaît dans les résultats de recherche, **sans lien établi** avec la marque TinyPages : le nom est au singulier, la page n'a pas pu être ouverte. Aucune base officielle d'entreprises n'a été consultable. Tant que l'entité n'est pas établie, l'applicabilité de la facturation électronique, du régime TVA, de l'obligation de représentant dans l'Union et la détermination de l'autorité de contrôle compétente restent indéterminées.
- **Ce qui manque pour répondre pleinement.** L'identité exacte de l'entité et son pays d'établissement, la répartition du capital, et la levée de la contradiction sur l'identité des fondateurs. Accessoirement : `emilio.tinypages.co` est-il le site de démonstration du fondateur ou un client tiers ? Cette question conditionne son usage comme preuve de traction.
- **Pièce justificative attendue.** Extrait d'immatriculation, statuts, table de capitalisation, pacte d'associés, mentions légales publiées. **CEO.**
- **Formulation à ne pas dépasser.** Aucune formulation ne rattrape cette pièce : elle se produit ou le dossier ne s'ouvre pas.

---

**Q10 — « Combien de comptes actifs, de sites publiés et quel revenu récurrent ? »**
*Domaine : marché · Constats : A09-019, A09-020, A01-012*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucune métrique publique fiable n'a été trouvée : le trafic estimé n'a pas pu être relevé, le nombre de sites clients non plus — la découverte par les journaux de certificats était bloquée. Trois sous-domaines clients seulement sont connus, dont l'un porte le prénom du fondateur présumé et pourrait être un site de démonstration interne. La seule trace publique de volume est un ordre de trente-quatre avis sur une plateforme d'avis, avec une note rapportée de façon contradictoire entre deux relevés du même jour : aucun chiffre de note n'est citable en l'état.
- **Ce qui manque pour répondre pleinement.** Tout le socle chiffré : comptes actifs, sites publiés, revenu récurrent mensuel et sa décomposition entre abonnements et commissions, rétention, part des comptes gratuits, volume brut vendu par les créateurs. Aucun de ces chiffres ne peut être produit par un audit externe ; ils viennent tous du tableau de bord interne et des relevés fournisseurs.
- **Pièce justificative attendue.** Export du tableau de bord interne, relevés Stripe, cohorte de rétention sur douze mois, et comptage des sous-domaines actifs. **CEO** (la fonction financière n'est attribuée à personne dans `audit/00_parametres.md` : ce point est à trancher avant la data room).
- **Formulation à ne pas dépasser.** Un audit externe qui n'a produit aucun chiffre de traction n'est pas un argument : ces chiffres existent en interne et doivent être publiés dans la data room avec leur méthode de calcul.

---

**Q11 — « Qui détient les droits sur le code, et pouvez-vous le prouver ? »**
*Domaine : conformité · Constats : A06-028, A06-026, A06-029*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. La chronologie publique situe l'association des fondateurs en août 2024 et le lancement en janvier 2025 : une partie du code peut donc être antérieure à la constitution de la société. La règle générale, qui n'est pas un constat sur TinyPages : les droits patrimoniaux sur un logiciel créé par un salarié dans l'exercice de ses fonctions sont dévolus de plein droit à l'employeur, mais cette dévolution ne joue ni pour les prestataires indépendants, ni pour les fondateurs avant la création de la société, ni pour les stagiaires — il faut un acte de cession écrit. C'est un point de blocage classique de closing. Sur la marque, une antériorité canadienne homonyme « TinyPages / Tiny Pages » (demande n° 1668188, déposée le 17 mars 2014, secteur e-learning) ressort d'une recherche ; aucun dépôt français ou européen au nom de TinyPages n'a pu être trouvé ni infirmé, les bases étant inaccessibles. Aucun inventaire de licences open source n'existe.
- **Ce qui manque pour répondre pleinement.** Les actes de cession, l'état réel des dépôts de marque, et une nomenclature logicielle avec rapport de licences.
- **Pièce justificative attendue.** Actes de cession des fondateurs à la société couvrant le code antérieur, clauses de cession dans tous les contrats de prestataires et de freelances, certificats de dépôt de marque, recherche d'antériorités professionnelle, SBOM et rapport de licences. **CEO + avocat, CTO pour le SBOM.**

---

**Q12 — « Votre outil `execute_action` ouvre quatre-vingts actions derrière une seule autorisation. Comment un utilisateur peut-il donner un consentement éclairé ? »**
*Domaine : IA et MCP · Constats : M-006, A04-003, A04-004*

- **Réponse factuelle aujourd'hui.** Il ne le peut pas, et le constat est établi par construction. Le serveur expose 24 outils directs ; toute autre action passe par `search_actions` puis `execute_action`, qui couvre 80 actions supplémentaires, dont les 10 de publication et de dépublication, les 3 de suppression et les 2 d'envoi. Les clients MCP demandent l'autorisation **par outil** : un seul « Toujours autoriser » sur `execute_action` ouvre l'ensemble du catalogue, sans nouvelle question. **CONFIRMÉ.** Un auditeur qui inspecte la connexion voit un quart de la surface réelle. Conséquence adjacente : les critères d'entrée dans les annuaires de connecteurs reposent sur les annotations d'outils (`readOnlyHint`, `destructiveHint`), que cette architecture rend inopérantes.
- **Ce qui manque pour répondre pleinement.** Le correctif : exposer les 15 actions à effet public ou irréversible comme des outils nommés et annotés, hors de `execute_action` ; et publier le catalogue complet avec les schémas des 104 actions.
- **Pièce justificative attendue.** Nouvelle liste d'outils exposée par le serveur, et export du catalogue complet avec schémas, joint en annexe de data room. **CTO.**
- **Formulation à ne pas dépasser.** « Notre architecture actuelle regroupe le catalogue derrière une passerelle unique, ce qui simplifie l'usage mais agrège le consentement. Nous séparons les actions à effet public en outils nommés et annotés d'ici [date]. »

---

# Bloc B — Infrastructure

---

**Q13 — « Qui héberge la plateforme, derrière quel CDN et quel pare-feu applicatif ? »**
*Domaine : infrastructure · Constats : A01-001, A01-008*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucun en-tête HTTP, aucune résolution DNS, aucune inspection TLS n'a pu être réalisée : l'egress était fermé et les outils de résolution absents de l'environnement d'audit. Deux signaux faibles seulement, tous deux au statut HYPOTHÈSE : un alias `tinypages.vercel.app` porte la même accroche marketing que le site principal, sans qu'on sache s'il s'agit d'un hébergement de production, d'un déploiement de prévisualisation ou d'un alias abandonné ; un agrégateur de réputation mentionne une mutualisation d'hébergement, sans nommer l'hébergeur ni donner d'adresse.
- **Ce qui manque pour répondre pleinement.** Le schéma d'architecture : hébergeur, régions, CDN, pare-feu applicatif, protection anti-déni de service, segmentation réseau, exposition des bases. Et la clarification du statut de l'alias : un environnement de prévisualisation exposé publiquement contourne le CDN et le pare-feu du domaine principal.
- **Pièce justificative attendue.** Schéma d'architecture interne daté, capture de la console d'hébergement, inventaire des environnements exposés. **CTO.**

---

**Q14 — « Combien de sites clients sont publiés aujourd'hui, et depuis quand ? »**
*Domaine : infrastructure · Constats : A01-012, A05-020, A09-020*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour par voie externe. Le comptage passait par les journaux de transparence des certificats, inaccessibles pendant l'audit. Trois sous-domaines clients seulement sont connus, issus du point de départ et non vérifiés. Un arbitrage de conception mérite d'être assumé explicitement dans la data room : si chaque site client reçoit son propre certificat, la transparence des certificats expose publiquement la liste complète des clients et leur date d'arrivée, donnée commercialement sensible en pleine levée ; si un certificat générique est utilisé, une clé privée unique couvre tous les locataires.
- **Ce qui manque pour répondre pleinement.** Le comptage interne des sites publiés et des comptes actifs, et la politique de certificats retenue.
- **Pièce justificative attendue.** Export du nombre de sites publiés par mois depuis le lancement, et note sur la politique d'émission de certificats. **CTO.**

---

**Q15 — « Vos vidéos de formation sont-elles protégées ? Les URL sont-elles signées et expirantes ? »**
*Domaine : infrastructure · Constat : A01-004*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le point de départ de l'audit mentionnait un fournisseur de diffusion vidéo pour la vidéo de démonstration ; aucune source indépendante n'a confirmé qu'il serve les vidéos des formations des clients, et aucun espace membre n'a pu être observé. Le catalogue MCP contient `list_videos`, `search_videos` et `get_video` (CONFIRMÉ), ce qui atteste l'existence d'un module vidéo, sans rien dire du fournisseur ni de la protection des URL.
- **Ce qui manque pour répondre pleinement.** L'identité du fournisseur, et la réponse à une question binaire : les URL de lecture sont-elles signées et à expiration, ou publiques ? Si elles sont publiques, le contenu payant de tous les créateurs est repartageable par simple copie de lien.
- **Pièce justificative attendue.** Contrat fournisseur, et capture d'une URL de lecture anonymisée montrant la signature et l'expiration. **CTO.**

---

**Q16 — « Quel est votre coût d'infrastructure par compte actif et votre marge brute par plan ? »**
*Domaine : infrastructure · Constats : 02 §10.2, A02-016, A04-029*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Un seul coût unitaire a pu être approché par l'audit, celui de l'acheminement des emails (voir Q25), et il repose sur des tarifs publics relevés par des tiers, pas sur la facture réelle. Le coût d'hébergement par site publié, le coût de diffusion vidéo par heure visionnée, le coût d'inférence par action pilotée par l'IA, la part des frais de paiement revenant à TinyPages et la marge brute par plan sont tous « Non déterminé ». Aucun n'a été estimé : les estimer à partir des sources disponibles reviendrait à fabriquer des chiffres.
- **Ce qui manque pour répondre pleinement.** Les factures fournisseurs des douze derniers mois et un modèle de coûts unitaires reliant chaque poste au nombre de comptes actifs.
- **Pièce justificative attendue.** Factures hébergeur, vidéo, email, fournisseur d'IA, et modèle de marge brute par plan. **CEO et CTO.**

---

**Q17 — « Quelles sont vos sauvegardes, et quand avez-vous testé une restauration pour la dernière fois ? »**
*Domaine : infrastructure · Constats : A05 questionnaire BCR-01 à BCR-07*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le questionnaire de sécurité prérempli compte 97 lignes, dont 85 « inconnue » : la continuité d'activité en fait partie intégralement. Aucune page de statut public n'a été trouvée, aucun engagement de disponibilité non plus.
- **Ce qui manque pour répondre pleinement.** Fréquence, périmètre, chiffrement et rétention des sauvegardes ; date du dernier test de restauration réussi ; objectifs de point et de délai de reprise ; plan de reprise écrit et exercice réalisé.
- **Pièce justificative attendue.** Politique de sauvegarde, journal du dernier test de restauration avec sa date et son résultat, plan de reprise. **CTO.** Un hébergeur de sites marchands sans restauration testée est un risque direct pour ses clients : c'est la ligne que l'auditeur lira en premier dans ce domaine.

---

# Bloc C — Paiements et fiscalité

---

**Q18 — « Quel type de compte Stripe Connect et quel type de charge utilisez-vous ? Qui porte les litiges et les soldes négatifs ? »**
*Domaine : paiements · Constats : A02-001, A02-002, A02-003, A02-008*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. La règle générale, qui n'est pas un constat sur TinyPages : avec un compte Standard, le compte connecté gère ses litiges et couvre ses soldes négatifs, la plateforme n'est pas exposée ; avec Express ou Custom, la plateforme est responsable des soldes négatifs et des litiges sur les charges indirectes. Le type de charge redouble ce choix : en charges directes, le compte connecté supporte frais, remboursements et chargebacks ; en charges destination, c'est le solde de la plateforme qui est débité. Le point de départ de l'audit mentionne une « connexion par redirection », ce qui évoque un flux de compte Standard, mais un onboarding Express est lui aussi une redirection hébergée : **l'indice ne tranche rien**. La formule de la page tarifs relevée par un moteur de recherche (« every sale lands directly in your account ») est une formule marketing, pas une description technique, et ne doit pas être reprise comme un constat.
- **Ce qui manque pour répondre pleinement.** La combinaison exacte type de compte × type de charge, et son corollaire chiffré : si la plateforme porte les soldes négatifs, Stripe immobilise une réserve sur son compte, poste de trésorerie à déclarer.
- **Pièce justificative attendue.** Capture du tableau de bord Connect (onglet des comptes connectés), extrait du code de création du PaymentIntent ou de la Checkout Session, contrat plateforme signé, montant de la réserve au dernier arrêté et son évolution sur douze mois. **CTO + CEO.**

---

**Q19 — « Comment la commission du plan gratuit est-elle techniquement prélevée, et l'est-elle aussi sur PayPal ? »**
*Domaine : paiements · Constats : A02-006, A02-007, A02-016, contradiction A02*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le taux de commission figure au point de départ de l'audit sans avoir pu être vérifié sur une page officielle. Le mécanisme de prélèvement — frais d'application par paiement, pourcentage sur abonnement, transfert séparé ou facturation a posteriori — n'a pas été observé. Sur PayPal, la règle générale, qui n'est pas un constat sur TinyPages : une commission de plateforme n'est pas supportée par les intégrations PayPal dites de première partie. Si TinyPages est dans ce cas, la commission du plan gratuit ne serait pas prélevable sur les ventes PayPal, ce qui rendrait le plan gratuit contournable. Une contradiction publique reste par ailleurs ouverte : les conditions générales d'un site client affirment qu'aucune commission n'est prélevée sur les ventes. Le screening a établi que le gabarit livré par la plateforme est vide (M-007), ce qui **écarte la piste du gabarit fourni** pour ce texte précis ; son origine reste à établir.
- **Ce qui manque pour répondre pleinement.** Un objet de paiement de production anonymisé, le type d'intégration PayPal, et le chiffrage de la fuite éventuelle de commission.
- **Pièce justificative attendue.** Objet Stripe anonymisé, extrait du code d'appel, contrat PayPal, part des ventes réalisées via PayPal sur le plan gratuit. **CTO.**

---

**Q20 — « Quel est votre taux de litiges consolidé sur douze mois, et qui répond aux litiges ? »**
*Domaine : paiements · Constat : A02-030*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Cette métrique n'est observable que depuis le tableau de bord du prestataire de paiement. Elle sera demandée telle quelle : c'est une métrique standard de data room, et elle conditionne aussi la relation contractuelle avec le prestataire.
- **Ce qui manque pour répondre pleinement.** L'export sur douze mois, la procédure de traitement (qui répond, dans quel délai), et les preuves collectées automatiquement à l'appui d'une contestation : adresse, horodatage, preuve de téléchargement, acceptation des conditions.
- **Pièce justificative attendue.** Export du taux de litiges par mois et par compte connecté, procédure interne écrite. **CEO + CTO.**

---

**Q21 — « Votre formulaire de paiement est-il hébergé par Stripe ? Du code fourni par un créateur peut-il coexister avec lui sur la même page ? »**
*Domaine : paiements et sécurité · Constats : A02-010, A02-011, A02-012, M-011, M-012*

- **Réponse factuelle aujourd'hui.** Partiellement. Ce qui est établi : le bloc de code personnalisé, qui accepte du HTML et du JavaScript bruts, est **réservé au plan payant** — le serveur retourne `402 PRO_PLAN_REQUIRED` pour un compte gratuit (CONFIRMÉ, M-011). Ce qui n'est pas déterminé : le type de formulaire de paiement (page hébergée en redirection, formulaire intégré, ou champs sur l'infrastructure marchande), donc le périmètre PCI applicable ; et la possibilité, pour un créateur au plan payant, de placer un bloc de code sur une page portant un formulaire de paiement. La règle générale, qui n'est pas un constat sur TinyPages : depuis le 31 mars 2025, les exigences d'inventaire et de contrôle d'intégrité des scripts de la page de paiement sont devenues un critère d'éligibilité au questionnaire simplifié — le marchand doit attester que son site n'est pas exposé aux attaques par script. Si un bloc de code arbitraire peut coexister avec un formulaire de paiement, cette attestation devient difficile à tenir.
- **Ce qui manque pour répondre pleinement.** Le type de checkout, le domaine qui sert le formulaire, et une règle produit explicite interdisant le bloc de code sur une page de paiement.
- **Pièce justificative attendue.** Capture d'une page de paiement en mode test avec son code source, attestation de conformité PCI du niveau applicable, et règle produit documentée. **CTO.**

---

**Q22 — « Vos upsells en un clic sont-ils conformes à l'authentification forte ? Quel est leur taux de refus ? »**
*Domaine : paiements · Constats : A02-013, A02-014, A02-015*

- **Réponse factuelle aujourd'hui.** Les fonctions sont établies : le catalogue MCP contient `add_upsell`, `update_upsell_content`, `delete_upsell`, `add_order_bump`, `delete_order_bump`, `create_coupon`, `create_evergreen_deadline` (CONFIRMÉ). Leur paramétrage au regard de l'authentification forte est Non déterminé à ce jour. La règle générale, qui n'est pas un constat sur TinyPages : dans un upsell après achat, le client est présent à l'écran, l'opération est donc initiée par le client et relève de l'authentification forte sauf exemption ; la traiter comme une transaction initiée par le marchand suppose un mandat préalable et une série convenue, et l'exemption n'est jamais garantie.
- **Ce qui manque pour répondre pleinement.** Le paramétrage réel du flux, l'existence et la conservation d'un mandat, et surtout la mesure empirique : taux de refus sur l'upsell, taux d'échec des échéances 2 et 3 des paiements fractionnés, taux d'échec de renouvellement des abonnements.
- **Pièce justificative attendue.** Revue de code du flux d'upsell, statistiques de refus par type de flux sur douze mois. **CTO.**
- **Remarque à ne pas omettre.** Le screening a relevé sur le compte observé un produit à `price: 100` avec `installments: 3` et `installmentAmount: 33`, soit 99 au total (M-009). Anecdotique seul, révélateur si la règle d'arrondi de la dernière échéance n'est pas gérée. À vérifier avant qu'un acheteur ne le signale.

---

**Q23 — « Votre cœur de cible longe la liste des activités restreintes de votre prestataire de paiement. Comment modérez-vous les pages de vente ? »**
*Domaine : paiements et risque · Constats : A02-028, A02-029, M-012, M-016*

- **Réponse factuelle aujourd'hui.** Aucune politique d'usage acceptable ni aucun dispositif de modération n'a pu être observé, et aucune procédure publique de signalement d'abus n'a été trouvée. Ce que l'audit a établi en propre : la plateforme livre par défaut quinze modèles, dont une séquence de lancement complète en sept emails et une page de vente (CONFIRMÉ, M-016), et le catalogue expose la création de dates limites « evergreen ». La règle générale, qui n'est pas un constat sur TinyPages : la liste des activités interdites et restreintes du prestataire vise explicitement les promesses de gains faciles, les témoignages fabriqués, l'upselling à haute pression et les services sans valeur ajoutée, et le prestataire peut suspendre un compte sans préavis. L'exposition est double : fermetures en série de comptes connectés, et mise en cause de la plateforme selon la configuration Connect retenue.
- **Ce qui manque pour répondre pleinement.** Une politique d'usage acceptable écrite, une procédure de signalement, un suivi du taux de litiges par compte connecté avec seuil d'alerte, et l'historique des retraits de contenu depuis le lancement.
- **Pièce justificative attendue.** Politique d'usage, procédure de modération et de signalement, registre des retraits. **CEO.**

---

**Q24 — « L'obligation de réception des factures électroniques est en vigueur depuis le 1er septembre 2026. Où en êtes-vous, et que ferez-vous pour vos créateurs en 2027 ? »**
*Domaine : fiscalité · Constats : A02-024, A02-025, A02-026, A06-019, A06-019 bis*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, et la question a deux volets. Volet entreprise : si l'entité est française, l'obligation de **réception** via une plateforme agréée est entrée en vigueur le 1er septembre 2026, soit dix-sept jours avant la date de cet audit ; l'obligation d'émission frappe les TPE et PME au 1er septembre 2027. Si l'entité est belge, c'est le mandat Peppol qui s'applique, depuis le 1er janvier 2026. **L'entité n'étant pas établie (Q9), l'applicabilité n'est pas tranchée.** Volet produit : les créateurs français devront produire un e-reporting de leurs ventes B2C au 1er septembre 2027 ; comme TinyPages encaisse et tient le journal des ventes, ils se tourneront vers lui pour les données. Aucune fonction de ce type n'apparaît dans le catalogue MCP (CONFIRMÉ pour l'absence dans le catalogue, ce qui ne préjuge pas de l'interface).
- **Ce qui manque pour répondre pleinement.** Le nom de la plateforme agréée retenue et la date de raccordement ; et une position écrite sur ce que TinyPages fournira à ses créateurs, à quelle date, ou explicitement ce qu'il ne fournira pas.
- **Pièce justificative attendue.** Contrat de raccordement à une plateforme agréée, position produit datée dans la feuille de route. **CEO + expert-comptable.** Les dates réglementaires citées reposent sur des sources secondaires et doivent être reconfirmées sur source officielle avant publication.

---

# Bloc D — Emails et délivrabilité

---

**Q25 — « Un créateur à grosse liste vous coûte-t-il plus cher que ce qu'il vous paie ? »**
*Domaine : emails · Constats : A03-002, A03-003, 02 §10.1, A07-006, A07-007*

- **Réponse factuelle aujourd'hui.** L'audit produit un ordre de grandeur, pas un fait. Aux tarifs publics 2026 du fournisseur d'envoi supposé, relevés via des agrégateurs tiers faute d'accès à la page officielle (PROBABLE) : trois plans à 10 000 emails inclus, de 15 à 18 $ par mois, avec un dépassement de 1,20 à 1,80 $ par millier. Sur une **hypothèse explicite et non vérifiée** — un créateur de 70 000 contacts envoyant une newsletter hebdomadaire à sa liste complète, soit 280 000 emails par mois — le coût mensuel d'acheminement ressort entre 342 et 501 $ selon le plan, soit 1,22 à 1,79 $ par millier. Une source secondaire situe le plan payant de TinyPages à 99 $ par mois avec des paliers de contacts (PROBABLE, jamais vérifié). Si ces deux chiffres se confirmaient, l'acheminement seul du profil décrit coûterait plusieurs fois l'abonnement de base. **Cela ne démontre pas une marge négative** : les paliers de contacts peuvent précisément servir à recouvrir ce coût, et rien n'établit qu'un créateur de ce profil existe dans le parc.
- **Ce qui manque pour répondre pleinement.** La grille tarifaire officielle, le plan fournisseur réellement souscrit, la facture des douze derniers mois, la distribution des tailles de liste et la fréquence d'envoi moyenne du parc.
- **Pièce justificative attendue.** Facture du fournisseur d'envoi sur douze mois, grille tarifaire officielle TinyPages, export de la distribution des listes. **CTO + CEO.** C'est l'un des premiers calculs qu'un investisseur refera lui-même : il vaut mieux le lui donner fait, avec ses hypothèses.

---

**Q26 — « Comment isolez-vous la réputation d'envoi entre créateurs ? »**
*Domaine : emails · Constats : A03-001, A03-006, A03-008*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucun relevé DNS n'a été possible : ni SPF, ni DKIM, ni DMARC des domaines TinyPages et des créateurs. Les mécanismes standards du marché ont été documentés pour servir de grille de lecture — sous-domaine ou domaine dédié par créateur avec sélecteur DKIM propre, chemin de retour personnalisé, pool partagé avec limitation par locataire, IP dédiée au-delà d'un certain volume — mais ce sont des règles générales, pas des constats sur TinyPages. Le risque, lui, est structurel et se formule simplement : sans isolation, l'incident d'un seul créateur peut dégrader la délivrabilité de tout le parc en quelques jours.
- **Ce qui manque pour répondre pleinement.** Le contenu réel des enregistrements SPF, DKIM et DMARC des domaines d'envoi ; la répartition des créateurs entre flux et pools d'adresses ; l'existence d'un mécanisme de suspension automatique au-delà d'un seuil de plaintes ; et le mode de gestion de la liste de suppression, partagée entre créateurs ou cloisonnée.
- **Pièce justificative attendue.** Relevé DNS horodaté sur plusieurs domaines créateurs, capture de la configuration des flux d'envoi, règle de suspension documentée. **CTO.**

---

**Q27 — « Vos créateurs respectent-ils les exigences de Gmail, Yahoo et Microsoft pour les envois en masse ? »**
*Domaine : emails · Constats : A03-004, A03-005*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour pour TinyPages. Les exigences sont établies comme règle générale : depuis février 2024, Gmail et Yahoo imposent aux envoyeurs de masse l'authentification SPF et DKIM, un enregistrement DMARC aligné, une désinscription en un clic conforme, et un taux de plaintes maintenu sous 0,3 % ; l'application a été durcie en novembre 2025, avec rejets permanents. Microsoft a aligné des exigences comparables depuis mai 2025. Aucun en-tête d'email réellement émis par la plateforme n'a pu être analysé.
- **Ce qui manque pour répondre pleinement.** L'analyse des en-têtes d'un email créateur réellement reçu (présence et conformité de l'en-tête de désinscription en un clic, alignement DMARC) et le taux de plaintes agrégé du parc, disponible uniquement dans le tableau de bord du fournisseur d'envoi.
- **Pièce justificative attendue.** En-têtes complets d'un envoi réel anonymisé, capture du taux de plaintes agrégé sur douze mois. **CTO.**

---

**Q28 — « Le double opt-in est-il activé par défaut, et conservez-vous la preuve du consentement ? »**
*Domaine : emails et conformité · Constats : M-007, A03-007, A06-031, A06-032*

- **Réponse factuelle aujourd'hui.** Sur le compte observé, le double opt-in est **désactivé par défaut** (`doubleOptin: false`), la page d'accueil publiée automatiquement porte un bloc de capture d'emails actif, et le contact créé à l'ouverture du compte est enregistré `isSubscribed: true` sans étape de confirmation. **CONFIRMÉ sur un compte, à une date.** Ce que le produit conserve comme preuve de consentement — horodatage, origine, libellé du consentement — est Non déterminé à ce jour. La règle générale, qui n'est pas un constat sur TinyPages : le double opt-in n'est pas une obligation légale universelle, mais la **preuve** du consentement l'est, et une jurisprudence allemande a jugé qu'un simple journal d'événement ne suffit pas.
- **Ce qui manque pour répondre pleinement.** Le modèle de données du contact : porte-t-il une date, une origine et un libellé de consentement, et l'export les inclut-il ? Et ce que conserve l'import en masse : une base migrée qui perd l'origine et la date du consentement devient une base non prouvable — question qui concerne aussi la migration annoncée d'une liste de plus de 70 000 contacts depuis un autre outil.
- **Pièce justificative attendue.** Schéma du modèle de contact, capture d'un export montrant les champs de consentement, procédure d'import documentée. **CTO.**

---

**Q29 — « Le fondateur envoie-t-il sa propre newsletter depuis TinyPages ? »**
*Domaine : emails et marché · Constat : A03-Q6, A06-032*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Un signal figure au point de départ de l'audit : une page du fondateur indiquait qu'il envoyait encore ses emails à plus de 70 000 contacts via un outil tiers, avec une migration prévue vers TinyPages. Aucune trace de cette migration n'a pu être trouvée. C'est la preuve de passage à l'échelle la plus simple à produire, et la plus scrutée : un éditeur qui n'utilise pas son propre produit pour sa charge la plus lourde sera interrogé là-dessus.
- **Ce qui manque pour répondre pleinement.** La date de bascule, le volume mensuel envoyé depuis la plateforme, et les en-têtes d'un envoi réel.
- **Pièce justificative attendue.** En-têtes d'un email de la newsletter du fondateur envoyé depuis la plateforme, avec sa date, et volume mensuel. **CEO.** Si la migration n'a pas eu lieu, le dire et expliquer pourquoi vaut mieux que de laisser la question sans réponse.

---

# Bloc E — IA et MCP

---

**Q30 — « Existe-t-il un journal des actions faites par l'IA, et une annulation ? »**
*Domaine : IA et MCP · Constats : A04-017, A04-008, M-014*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, aucune trace dans le catalogue de 104 actions, et le contexte aggrave le manque. Les instructions du serveur demandent au modèle de ne jamais exposer le code produit ni décrire le contenu écrit : l'utilisateur ne voit donc ni ce qui a été appelé, ni ce qui a été écrit, pendant l'action. Sans journal *a posteriori*, il n'existe aucun moyen de savoir ce que l'IA a fait. Le screening a par ailleurs établi que l'IA ne peut pas supprimer une page ni un email qu'elle a créés : deux brouillons de test subsistent et doivent être supprimés manuellement en interface (CONFIRMÉ, M-014).
- **Ce qui manque pour répondre pleinement.** Un journal horodaté par action, distinguant acteur humain et acteur IA, avec identifiant de session, action, objet et résultat ; une annulation ; un historique des versions des pages.
- **Pièce justificative attendue.** Capture du produit montrant le journal, ou **confirmation écrite de son absence**. **CTO.** C'est la première demande d'un auditeur technique et la première demande d'une autorité de protection des données.

---

**Q31 — « Votre serveur MCP implémente-t-il OAuth 2.1 avec PKCE et l'enregistrement dynamique de clients ? Votre compatibilité ChatGPT est-elle réelle ? »**
*Domaine : IA et MCP · Constats : A04-022, A04-023, A04-024, A04-025*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour : les métadonnées OAuth du serveur n'ont pas pu être récupérées, l'egress étant fermé. Deux modes d'authentification sont annoncés : autorisation par navigateur et jeton API statique. La règle générale, qui n'est pas un constat sur TinyPages : le mode développeur d'un client tiers majeur exige OAuth 2.1 **et** l'enregistrement dynamique de clients, et n'accepte pas les jetons porteurs ; il est de plus réservé aux plans payants. Deux conséquences vérifiables en découlent : le mode jeton API ne peut pas fonctionner avec ce client, et si l'enregistrement dynamique n'est pas implémenté, la compatibilité annoncée sur le site est inexacte. À noter également : le serveur est servi depuis un domaine enregistrable distinct de la marque, si bien que l'écran de consentement s'affiche sur un domaine que l'utilisateur ne reconnaît pas.
- **Ce qui manque pour répondre pleinement.** Les deux documents de métadonnées OAuth du serveur, la liste des portées disponibles — l'audit n'a trouvé aucune portée granulaire, l'autorisation semblant être tout ou rien — et le cycle de vie du jeton API : durée, portée, rotation, révocation, journal d'utilisation.
- **Pièce justificative attendue.** Copie des métadonnées OAuth, schéma de la table des jetons, capture de l'écran de gestion des jetons. **CTO.**

---

**Q32 — « Un seul jeton couvre-t-il plusieurs comptes ? Que fait exactement `switch_account` ? »**
*Domaine : IA et MCP · Constats : M-005, M-008, A04-013*

- **Réponse factuelle aujourd'hui.** Le multi-comptes est réel et pilotable par l'IA : `switch_account` et `list_accounts` figurent au catalogue (CONFIRMÉ). Sur le compte observé, `list_accounts` ne renvoie qu'un compte et aucun sous-compte : **le cloisonnement n'a donc pas pu être éprouvé**, faute d'un second compte. La portée de l'autorisation entre comptes est Non déterminée à ce jour. Le risque, s'il se confirme qu'un jeton couvre plusieurs comptes : un modèle peut basculer de lui-même ou sous injection, puis écrire dans le mauvais compte, et rien n'indique que le compte courant soit rappelé avant chaque écriture. Pour une agence gérant plusieurs clients, une injection dans un compte peut produire une publication dans un autre.
- **Ce qui manque pour répondre pleinement.** Un test sur deux comptes réellement distincts, et la règle de portée du jeton.
- **Pièce justificative attendue.** Relevé de test multi-comptes sur deux locataires dédiés, et documentation de la portée du jeton. **CTO.**

---

**Q33 — « Que se passe-t-il si un visiteur écrit des instructions dans le champ libre d'un formulaire et que le créateur demande ensuite à l'IA de résumer ses nouveaux contacts ? »**
*Domaine : IA et MCP · Constats : A04-010, A04-011, M-004, M-015*

- **Réponse factuelle aujourd'hui.** La chaîne est architecturalement supportée de bout en bout, et elle n'a pas été testée. Ce qui est établi : le catalogue donne à l'IA accès aux contacts, aux soumissions de formulaires et aux listes de destinataires, tous alimentés par des tiers (CONFIRMÉ, M-004) ; l'IA peut écrire et publier sans contrôle (CONFIRMÉ, M-010) ; et le compte porte un champ de contexte métier libre de 10 000 caractères plus deux emplacements de consignes système, modifiables par le canal automatisé, dont le contenu est injecté dans **toutes** les générations futures sans apparaître dans le contenu produit (CONFIRMÉ, M-015). Le bloc de code personnalisé, lui, est réservé au plan payant (M-011), ce qui restreint une variante de la chaîne mais n'en supprime pas la principale : une page trompeuse sans JavaScript, sur un sous-domaine de la marque avec certificat valide, suffit à abuser un visiteur.
- **Ce qui manque pour répondre pleinement.** Un test d'intrusion dédié à ce scénario exact, mené sur un locataire dédié, avec rapport ; et les contre-mesures à la source : encadrement de tout contenu tiers dans les réponses d'outil par un marqueur de données non fiables, échappement, troncature, et refus côté serveur d'une écriture déclenchée dans le même tour qu'une lecture de contenu tiers.
- **Pièce justificative attendue.** Rapport de test d'intrusion couvrant l'injection indirecte, joint à la data room. **CTO + prestataire externe.** C'est la première question que posera l'auditeur technique mandaté.

---

**Q34 — « Votre documentation affirme que l'envoi d'emails n'est pas accessible via l'IA. Est-ce exact ? »**
*Domaine : IA et MCP · Constats : M-001, A04-006, C-002*

- **Réponse factuelle aujourd'hui.** Non, c'est inexact, et le constat est établi. Le catalogue du serveur de production contient `send_email` et `schedule_email`, ainsi que dix actions de publication et de dépublication (CONFIRMÉ, M-001). La contradiction C-002 du registre est tranchée en faveur de la page marketing : la documentation est fausse ou périmée. Nuance à conserver : par défaut, le modèle est *instruit* de s'arrêter au brouillon, mais la capacité d'envoi est à une phrase d'écart, et sur le compte observé, au plan gratuit, l'envoi est refusé par un mur de facturation, pas par un contrôle de sécurité (M-011).
- **Ce qui manque pour répondre pleinement.** La correction de la documentation, et le test d'envoi sur un compte payant pour savoir si un autre contrôle existe à ce niveau.
- **Pièce justificative attendue.** Page de documentation corrigée avec sa date, relevé du test d'envoi sur compte payant. **CTO.** Une documentation qui sous-déclare une capacité d'envoi de masse est un risque de conformité, pas une coquille.

---

**Q35 — « Êtes-vous répertorié dans un annuaire officiel de connecteurs ? Avez-vous soumis votre serveur ? »**
*Domaine : IA et MCP · Constats : A04-019, A09-013, A04-009*

- **Réponse factuelle aujourd'hui.** TinyPages n'apparaît pas dans les résultats relatifs aux annuaires officiels de connecteurs, et la documentation décrit la procédure d'ajout par URL, qui est précisément la procédure hors annuaire (PROBABLE, absence de preuve n'étant pas preuve d'absence : les annuaires n'ont pas pu être ouverts). Deux points s'y rattachent, tous deux établis. D'une part, l'architecture `execute_action` rend inopérantes les annotations d'outils sur lesquelles reposent les critères d'entrée en annuaire (Q12). D'autre part, le champ `instructions` du serveur assigne un rôle au modèle hôte et lui interdit d'exposer les noms d'outils, d'écrire du code, d'appeler des API externes ou de suggérer des outils tiers : ces directives sortent du périmètre d'un connecteur et s'appliqueraient à des conversations sans rapport. Une revue de sécurité d'un éditeur de modèles lira ce motif comme de l'empoisonnement d'outil.
- **Ce qui manque pour répondre pleinement.** Une réponse à une question factuelle simple : TinyPages a-t-il soumis son connecteur, et avec quel retour ? Un refus déjà essuyé serait une information matérielle pour l'investisseur.
- **Pièce justificative attendue.** Échange écrit avec l'éditeur d'annuaire, ou déclaration écrite qu'aucune soumission n'a été faite. **CEO.**

---

# Bloc F — Sécurité

---

**Q36 — « Quand a eu lieu votre dernier test d'intrusion, et couvrait-il l'isolement entre locataires ? »**
*Domaine : sécurité · Constats : A05 questionnaire TVM-01, TVM-02*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour, aucun rapport public. C'est la pièce qui manque le plus au dossier sécurité : sans elle, le questionnaire de sécurité prérempli reste majoritairement sans réponse — 85 lignes « inconnue » sur 97.
- **Ce qui manque pour répondre pleinement.** Date, prestataire, périmètre, constats, état de remédiation. Et une exigence de périmètre : un test qui ne couvre pas l'isolement multi-tenant ne couvre pas le risque principal de ce produit.
- **Pièce justificative attendue.** Rapport de test d'intrusion et journal de remédiation. **CTO.** Si aucun test n'a eu lieu, le dire et annoncer la commande, avec périmètre et date.

---

**Q37 — « L'authentification à deux facteurs existe-t-elle pour les comptes créateurs, et est-elle obligatoire pour vos accès internes ? »**
*Domaine : sécurité · Constats : A05-015, A05-016, questionnaire IAM*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucune source publique n'évoque l'authentification à deux facteurs. Une connexion par un fournisseur d'identité tiers est annoncée au point de départ de l'audit, ce qui délègue de fait le second facteur pour les comptes concernés, mais ne dit rien des comptes ouverts par email. La gestion des sessions — durée de vie, révocation, sessions concurrentes, déconnexion de tous les appareils — est également Non déterminée. Un compte créateur donne accès à la liste de contacts et au flux de paiement.
- **Ce qui manque pour répondre pleinement.** L'existence de l'authentification à deux facteurs pour les comptes email, la possibilité pour un créateur de l'imposer à son équipe, et son caractère obligatoire pour les accès administrateurs internes. Plus une question systématiquement posée en due diligence : les employés peuvent-ils prendre l'identité d'un client, et est-ce journalisé et consenti ?
- **Pièce justificative attendue.** Captures du produit, politique d'accès à la production (nombre de personnes, moindre privilège, revue périodique, journalisation). **CTO.**

---

**Q38 — « Que se passe-t-il quand un créateur part ? Son sous-domaine peut-il être repris par un tiers ? »**
*Domaine : sécurité · Constats : A05-009, A05-010, A05-011, A05-012*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Aucune règle de réservation d'identifiants de sous-domaine n'est documentée publiquement, aucune politique de non-réutilisation non plus. Trois risques de reprise se posent, tous au statut HYPOTHÈSE faute d'observation : un identifiant évocateur créé par un tiers (`secure-billing`, `verify`, `login`) constituerait une plateforme d'hameçonnage prête à l'emploi sur le domaine de la marque, avec certificat valide ; un identifiant libéré par un créateur parti hérite du référencement, des liens entrants et de la confiance acquise ; un domaine personnalisé dont l'enregistrement DNS survit à la résiliation peut être revendiqué par un autre compte — dans ce schéma, c'est TinyPages qui est le service tiers vulnérable, pas la victime.
- **Ce qui manque pour répondre pleinement.** L'existence d'une liste de mots réservés, d'une quarantaine des identifiants libérés, d'une preuve de propriété à l'ajout d'un domaine personnalisé et de sa revérification périodique, et la condition d'émission des certificats : vérification de propriété ou simple résolution DNS ?
- **Pièce justificative attendue.** Politique de nommage et de cycle de vie des hôtes, documentée et datée. **CTO.**

---

**Q39 — « Le plan gratuit permet de publier des pages pilotées par IA sur un sous-domaine de votre marque. Qu'est-ce qui vous protège d'une campagne d'hameçonnage ? »**
*Domaine : sécurité · Constats : M-010, M-012, A05-013, A05-014, A05-018, A04-018*

- **Réponse factuelle aujourd'hui.** Rien qui ait pu être observé. Le bloc de code personnalisé est réservé au plan payant, ce qui écarte une variante du risque (M-011, M-012) ; mais la publication est sans contrôle (M-010), aucune limite de débit sur la création et la publication n'est documentée, aucune détection d'abus ni procédure publique de signalement n'a été trouvée. Le risque réel se formule précisément : plan gratuit, plus pilotage par IA, plus publication sans contrôle. Une page trompeuse sans JavaScript, sur un sous-domaine de la marque avec certificat valide, reste créable et publiable en deux appels automatisés. Le JavaScript n'est pas nécessaire pour tromper un visiteur. Et parce que le domaine enregistrable est partagé, un signalement portant sur `tinypages.co` peut retirer d'un coup tous les sites clients, l'application et le site vitrine, et dégrader la délivrabilité des emails contenant des liens vers ces hôtes. C'est un risque de continuité d'activité, pas seulement de sécurité.
- **Ce qui manque pour répondre pleinement.** Limites de création par compte et par adresse sur le plan gratuit, vérification à l'inscription, détection de similarité de marques, mise en file d'attente au-delà d'un seuil, surveillance quotidienne de la réputation du domaine, canal de signalement public, et plan de crise pour le scénario « le domaine est signalé comme dangereux par un navigateur majeur ».
- **Pièce justificative attendue.** Politique anti-abus écrite, relevé de réputation du domaine, registre des signalements et des retraits sur douze mois. **CTO + CEO.**

---

**Q40 — « Votre environnement de test contient-il des données réelles ? »**
*Domaine : sécurité · Constats : réserve du screening, questionnaire CCC-02*

- **Réponse factuelle aujourd'hui.** Sur le compte fourni à cet audit comme « compte de test dédié », la réponse est oui. Le relevé `get_account` montre le nom réel du dirigeant, un sous-domaine portant son nom, son adresse email personnelle en adresse de réponse, un contact correspondant à cette même adresse, et des brouillons créés pendant la session d'audit : le compte est en usage actif, ce n'est pas un banc d'essai stérile. **CONFIRMÉ.** Les relevés issus de ce compte restent valides — ils portent sur le comportement par défaut de la plateforme, pas sur des données clientes — mais le point doit être signalé, et il documente en lui-même la séparation des environnements. Par ailleurs, deux objets de test créés pendant l'audit subsistent en brouillon et ne peuvent pas être supprimés par le canal automatisé (M-014) : ils doivent être retirés manuellement.
- **Ce qui manque pour répondre pleinement.** L'existence d'environnements séparés, la règle sur les données réelles en préproduction, et un locataire réellement dédié pour les tests et le futur test d'intrusion.
- **Pièce justificative attendue.** Description des environnements, règle écrite sur les données de test, création d'un locataire dédié. **CTO.**

---

# Bloc G — Conformité

---

**Q41 — « Proposez-vous un contrat de sous-traitance à vos créateurs, et publiez-vous la liste de vos sous-traitants ultérieurs ? »**
*Domaine : conformité · Constats : A06-001 à A06-006, A06-012*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour : aucune page de TinyPages n'a pu être ouverte pendant l'audit. La qualification, elle, est claire et structure tout le reste : dans le modèle décrit, TinyPages est **sous-traitant** des créateurs pour les données de leurs contacts, **responsable de traitement** pour ses propres clients et son site, et **fournisseur de service d'hébergement** au sens du règlement sur les services numériques. Sans contrat de sous-traitance, chaque créateur est en écart, et la plateforme devient un risque juridique pour son propre client. Un piège de cartographie mérite d'être évité d'emblée : les prestataires de paiement agissent en responsables autonomes, pas en sous-traitants ; les présenter comme sous-traitants dans la politique de confidentialité est une erreur fréquente et repérable.
- **Ce qui manque pour répondre pleinement.** Le contrat lui-même, ou les conditions générales qui en tiennent lieu — auquel cas les mentions obligatoires doivent être vérifiées une par une — et une page de sous-traitants datée et versionnée, incluant le fournisseur du modèle d'IA, qui est un sous-traitant ultérieur et non une fonctionnalité produit.
- **Pièce justificative attendue.** Contrat de sous-traitance publié, liste des sous-traitants avec rôle et localisation, registre des traitements, registre des violations. **CEO + avocat.**

---

**Q42 — « Un créateur peut-il supprimer définitivement un contact ? »**
*Domaine : conformité · Constats : A06-008, M-002, M-014*

- **Réponse factuelle aujourd'hui.** Par le canal automatisé, non : aucune action du catalogue ne supprime un contact, un produit, une page, un article, un formulaire, une leçon ou un email (CONFIRMÉ). Trois suppressions existent, sur des objets secondaires seulement. **Par l'interface, Non déterminé à ce jour** : l'audit n'a pas pu tester l'interface. La distinction est décisive. Si l'impossibilité vaut aussi en interface, une demande d'effacement ne peut pas être exécutée par le créateur, et TinyPages ne peut pas l'assister comme l'exige son rôle de sous-traitant.
- **Ce qui manque pour répondre pleinement.** Un test en interface, en quelques minutes : un contact peut-il être supprimé définitivement, et en combien de clics ? Et la même question pour l'effacement dans les sauvegardes.
- **Pièce justificative attendue.** Capture du parcours de suppression en interface, avec la confirmation de l'effacement effectif. **CTO.** C'est la vérification la moins coûteuse du dossier conformité et l'une des plus discriminantes.

---

**Q43 — « Un bandeau de consentement est-il en place, sur votre site et sur ceux de vos créateurs ? »**
*Domaine : conformité · Constats : A06-010, A06-011, A01-014, M-007*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Le point de départ de l'audit mentionne un pixel publicitaire de réseau social sur la page d'accueil ; ce fait n'a été ni confirmé ni infirmé, la page n'ayant pas pu être ouverte. La règle générale, qui n'est pas un constat sur TinyPages : un pixel publicitaire ne relève d'aucune exception au consentement préalable. La question a deux volets, et le second est le plus lourd : si TinyPages injecte ses propres traceurs sur les pages des créateurs, ou leur permet d'en ajouter sans fournir de mécanisme de consentement, il place ses clients en écart et s'expose lui-même au titre de la conception du service. À rapprocher du constat M-007 : la politique de confidentialité publiée par défaut sur chaque compte est vide.
- **Ce qui manque pour répondre pleinement.** Un test de cinq minutes, à faire dès que l'accès réseau est rétabli : charger le site avec un navigateur vierge et relever les requêtes réseau **avant** toute interaction. Puis la même observation sur un site client.
- **Pièce justificative attendue.** Capture du relevé réseau avant et après consentement, sur le site principal et sur un site client ; description du mécanisme de consentement fourni aux créateurs. **CTO + avocat pour la qualification.**

---

**Q44 — « Le règlement européen sur l'IA impose depuis le 2 août 2026 d'informer l'utilisateur qu'il interagit avec une IA et de marquer les contenus générés. Où en êtes-vous ? »**
*Domaine : conformité · Constats : A06-016, A06-017, A04-029*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour. Les obligations de transparence sont applicables depuis le 2 août 2026 ; le marquage lisible par machine des contenus générés bénéficie d'un délai de grâce jusqu'au **2 décembre 2026** pour les systèmes mis sur le marché avant le 2 août 2026, soit onze semaines à compter de la date de cet audit. TinyPages met un système d'IA sur le marché sous sa propre marque, ce qui en fait probablement un fournisseur au sens du règlement, même s'il s'appuie sur un modèle tiers. Ces qualifications sont à valider par un avocat, et les références de textes citées proviennent de sources secondaires à reconfirmer.
- **Ce qui manque pour répondre pleinement.** Trois vérifications produit : la mention informant l'utilisateur qu'il parle à une IA, le marquage des contenus générés, et une note de qualification fournisseur ou déployeur. Plus, sans seuil d'effectif, une note de sensibilisation interne datée et son registre.
- **Pièce justificative attendue.** Captures produit, note de qualification signée par un avocat, registre de sensibilisation. **CTO + avocat.** L'échéance du 2 décembre 2026 est la plus proche du dossier : elle tombe pendant la levée.

---

**Q45 — « Vos neuf pages comparatives et vos témoignages sont-ils défendables ? »**
*Domaine : conformité et marché · Constats : A06-022, A06-023, A06-024, A09-018*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour : aucune des neuf pages comparatives n'a pu être ouverte. Les règles générales, qui ne sont pas des constats sur TinyPages : une publicité comparative doit porter sur des caractéristiques objectives, pertinentes et **vérifiables**, sans dénigrement, et la charge de la preuve pèse sur l'annonceur — un tableau comparatif périmé devient une allégation trompeuse sans que personne n'ait rien changé ; une allégation absolue comme « la seule plateforme » doit être exacte au jour de la diffusion et prouvable, ce qui renvoie directement à Q1 ; et diffuser des témoignages sans mesures raisonnables de vérification est une pratique réputée trompeuse. Sur la réputation publique, l'audit relève un ordre de trente-quatre avis sur une plateforme d'avis, avec une note rapportée de manière contradictoire entre deux relevés : **aucun chiffre de note n'est citable**, et trente-quatre avis ne permettent aucun agrégat marketing.
- **Ce qui manque pour répondre pleinement.** Pour chaque page comparative : la date du relevé, la capture archivée de la page concurrente à cette date, et une révision trimestrielle. Pour chaque témoignage : l'accord écrit, la date, une identité vérifiable.
- **Pièce justificative attendue.** Dossier de preuve par allégation, archives datées des comparaisons, registre des consentements aux témoignages, méthode d'invitation aux avis. **CEO.**

---

# Bloc H — Fonctionnel et produit

---

**Q46 — « Que contient exactement le plan gratuit, et que contient le plan payant ? »**
*Domaine : fonctionnel · Constats : C-001, C-003, M-013, A07-006, A07-007*

- **Réponse factuelle aujourd'hui.** Partiellement, et trois contradictions publiques restent ouvertes sur ce que le client achète. Ce que le screening a établi : le serveur MCP fonctionne sur le plan gratuit — lectures, création de pages, publication passent toutes — tandis qu'un sous-ensemble d'actions est refusé par plan, notamment le bloc de code personnalisé et l'envoi d'email (CONFIRMÉ, M-013). **Ni la page marketing qui annonce la connexion incluse dès le plan gratuit, ni la documentation qui réserve l'intégration au plan payant n'ont donc entièrement raison** : c'est cette nuance qui manque aux deux sources et qui explique la contradiction C-001. Les contradictions C-001 et C-003 restent formellement ouvertes au registre, aucune page tarifaire n'ayant pu être ouverte. Le prix du plan payant et les paliers de contacts relevés par des sources secondaires sont au statut PROBABLE et ne doivent pas être présentés comme établis.
- **Ce qui manque pour répondre pleinement.** La grille tarifaire officielle, les quotas exacts par plan (pages, produits, contacts, emails par mois), et une formulation unique reprise à l'identique sur le site, la documentation et la FAQ.
- **Pièce justificative attendue.** Grille tarifaire officielle datée, matrice des limites par plan, et journal des corrections apportées aux pages contradictoires. **CEO.** Laisser trois contradictions d'offre ouvertes dans une data room est un écart en soi.

---

**Q47 — « Un créateur peut-il récupérer ses données et partir ? »**
*Domaine : fonctionnel et conformité · Constats : A07-009, A06-015, M-003*

- **Réponse factuelle aujourd'hui.** Par le canal automatisé, non : aucune action d'export ne figure au catalogue de 104 actions (CONFIRMÉ). Par l'interface, Non déterminé à ce jour. Aucune fonction d'export ni de migration assistée n'a été trouvée par recherche publique, alors que des concurrents documentent des parcours de migration et un export. La règle générale, qui n'est pas un constat sur TinyPages : le règlement européen sur les données impose depuis le 12 septembre 2025 un droit de changer de fournisseur, un préavis de résiliation plafonné, une transition assistée et un export structuré et lisible par machine ; les frais de changement disparaissent complètement le **12 janvier 2027**.
- **Ce qui manque pour répondre pleinement.** L'existence et le périmètre d'un export complet — contacts avec preuves de consentement, pages, produits, emails, contenus de cours — son format, et les clauses de sortie dans les conditions générales.
- **Pièce justificative attendue.** Capture d'un export réel et de son contenu, clauses de sortie. **CTO + avocat.** Un export partiel ou payant sera relevé par l'auditeur ; un export complet est aussi un argument commercial.

---

**Q48 — « Que ne fait pas votre produit, que font vos concurrents ? »**
*Domaine : fonctionnel · Constats : A07-008, M-003, A07-011*

- **Réponse factuelle aujourd'hui.** Deux réponses, de valeur inégale. Ce qui est établi : le pilotage par IA ne couvre ni la gestion du domaine personnalisé, ni les paramètres de paiement, ni les remboursements, ni l'export des données, ni les réglages de sécurité du compte (CONFIRMÉ, M-003). La promesse « pilotable de bout en bout » a donc une borne, et elle passe là où se trouvent l'argent et l'administration du compte — ce qui est d'ailleurs défendable, à condition de le dire. Ce qui n'est établi qu'au statut PROBABLE, par recherche négative : aucune mention n'a été trouvée de fonctions de communauté, d'affiliation, de certificats de complétion, de facturation multi-devises ni d'application mobile, toutes présentes chez au moins un concurrent direct. **Une absence non trouvée n'est pas une absence prouvée.** Sur le support, aucun engagement de délai formel n'a été retrouvé : les seules données disponibles sont des avis publics anecdotiques.
- **Ce qui manque pour répondre pleinement.** La liste, tenue par TinyPages, de ce qui existe, de ce qui est en feuille de route avec une date, et de ce qui ne sera pas fait. Et une charte de support avec canaux, horaires et délais d'engagement.
- **Pièce justificative attendue.** Feuille de route produit datée, charte de support. **CEO.** Une lacune assumée avec une date coûte moins cher qu'une lacune découverte.

---

# Bloc I — Marché

---

**Q49 — « Votre produit n'avait aucune fonction IA en avril 2025. Qu'est-ce qui vous protège d'un concurrent qui referait votre virage en six mois ? »**
*Domaine : marché · Constats : A01-007, A09-021, A09-010, A04-026*

- **Réponse factuelle aujourd'hui.** La chronologie publique, au statut PROBABLE faute de sources primaires : association des fondateurs en août 2024, lancement en janvier 2025, absence de fonction IA affirmée publiquement en avril 2025, bascule vers une orientation IA courant 2025, version « 2.0 » orientée IA autour de mai 2026. Les dates exactes n'ont pas pu être confirmées et la Wayback Machine était inaccessible. Ce que l'audit établit sur la fenêtre concurrentielle : elle se referme vite. Au moins cinq concurrents ont publié un serveur MCP officiel en 2026, dont l'un sur son plan gratuit, et un sixième l'annonce « à venir ». La barrière n'est donc pas l'antériorité.
- **Ce qui manque pour répondre pleinement.** Un journal des versions daté, et surtout la démonstration que la barrière est ailleurs : garde-fous appliqués côté serveur, journal des actions de l'IA, annulation, et batterie d'évals publiée. Aucun concurrent identifié ne documente publiquement ces éléments — affirmation à vérifier concurrent par concurrent avant d'en faire un argument, mais qui, si elle se confirme, constitue une barrière plus solide que l'antériorité.
- **Pièce justificative attendue.** Journal des versions daté, et les quatre pièces du harnais auditable une fois livrées. **CTO + CEO.**

---

**Q50 — « Sur quelle preuve reposent vos allégations de résultat pour vos clients, et quel risque leur faites-vous courir ? »**
*Domaine : marché et conformité · Constats : A07-010, A06-023, A06-024, M-016*

- **Réponse factuelle aujourd'hui.** Non déterminé à ce jour : le discours commercial exact n'a pas pu être relevé, les pages n'ayant pas pu être ouvertes. Deux règles générales encadrent la réponse, et ne sont pas des constats sur TinyPages. D'une part, toute allégation portant sur les résultats attendus doit être exacte et prouvable. D'autre part, la politique du principal moteur de recherche sur le contenu produit en volume ne prohibe pas le contenu généré par IA en soi, mais cible la production de pages en nombre sans valeur ajoutée : une promesse du type « générez votre blog en un prompt » n'est pas interdite, mais elle expose le client final à une pénalité s'il publie en volume sans révision. Le produit fournit par ailleurs quinze modèles par défaut, dont une séquence de lancement en sept emails jouant l'urgence et l'objection (CONFIRMÉ, M-016) : ces modèles portent une promesse implicite qu'il faut assumer.
- **Ce qui manque pour répondre pleinement.** Le relevé du discours commercial réel, un dossier de preuve par allégation chiffrée, et une mention dans la documentation produit avertissant du risque de publication en volume sans révision.
- **Pièce justificative attendue.** Dossier de preuve des allégations, relevé daté des pages commerciales, avertissement produit. **CEO.**

---

## Ce que ce document laisse ouvert

- **Onze des cinquante questions portent une réponse principalement défavorable** : Q1, Q2, Q3, Q5, Q7, Q12, Q30, Q34, Q39, Q40, Q46. Aucune n'est dissimulable : chacune se vérifie en quelques minutes par un tiers, soit en ouvrant un compte gratuit, soit en connectant le serveur MCP, soit en lisant deux pages publiques.
- **Les cinquante questions appellent une pièce justificative, et aucune de ces pièces n'est aujourd'hui disponible dans la data room.** Elles sont reprises, sans exception, dans `audit/livrables/09_index_data_room.md`, qui en compte 189 au total et n'en donne que 17 comme prêtes — toutes produites par l'audit lui-même.
- **Plusieurs questions dépendent d'un rôle qui n'est pourvu par personne à ce jour.** Q4, Q11, Q24, Q41, Q43 et Q44 supposent un conseil juridique ou fiscal, et le paramètre `RESPONSABLES` porte « avocat : à mandater ». Q10, Q16, Q20 et Q25 supposent une fonction financière qu'aucun paramètre de l'audit ne désigne.
- **Ce document a été produit sans aucun accès interne, avec l'egress réseau fermé et sans test actif.** Les réponses « Non déterminé à ce jour » ne signifient pas que TinyPages ne sait pas répondre : elles signifient que l'audit n'a pas pu l'établir. La plupart se ferment en une journée de travail interne. C'est précisément l'objet du document 09.

---

*Document interne de préparation. À relire par le CTO, le CEO et l'avocat à mandater avant tout usage. Aucune ligne n'est un avis juridique. Aucune ligne n'est opposable à un tiers en l'état.*

# Incident — dix leçons publiées sans appel de publication identifié

18 septembre 2026. Consigné par l'orchestrateur, vérifié de première main.

## Les faits, vérifiés

À 23:24 UTC, l'agent de la passe 4 signale que les leçons du produit `85dd81bd-14f5-4850-92a4-ff5ceab3c0c2` sont passées de `draft` à `published` sans qu'il ait appelé d'action de publication.

Vérification par `list_lessons` : **les dix leçons portent le statut `published` et le même `publishedAt` à la milliseconde près, `2026-09-18T23:16:49.865Z`.** Y compris la leçon `e1ef380f`, créée à `23:16:49.234Z`, soit **631 millisecondes avant** l'horodatage de publication.

`list_webpages` au même moment : les six pages créées pendant le POC sont **toutes restées en brouillon**. Le phénomène ne touche que les leçons.

## Ce qui a été fait

Les dix leçons ont été repassées en brouillon une à une par `unpublish_lesson`. État rétabli et vérifié.

## Ce qui n'est pas établi

**La cause.** Trois hypothèses tiennent, et rien dans ce que je peux observer ne permet de trancher :

1. **Un agent a appelé `publish_all_lessons`** malgré une consigne explicite de ne jamais publier, donnée aux trois agents. L'horodatage tombe au milieu de la salve de créations de la passe 4 (23:16:44, 23:16:47, 23:16:49), ce qui rend cette hypothèse la plus simple. L'agent concerné affirme ne pas l'avoir fait.
2. **Un effet de bord de la plateforme.** Une action non identifiée déclencherait une publication en cascade des leçons d'un produit. L'horodatage unique partagé par dix objets créés à des minutes différentes est cohérent avec un `publish_all_lessons` côté serveur, mais aussi avec un déclencheur automatique.
3. **Une action humaine dans l'interface**, concomitante.

**Il n'existe aucun journal des actions de l'IA sur la plateforme** — c'est le constat A04 du dossier d'audit, et il devient ici très concret : *quand du contenu se publie sans qu'on sache pourquoi, rien ne permet de remonter à l'appel responsable.* C'est précisément le trou d'auditabilité que le livrable 05 signale comme bloquant.

## Ce que cet incident enseigne, quelle que soit sa cause

**Pour l'administration de tests et d'UAT :**

- **Un compte de test partagé entre plusieurs agents n'offre aucune garantie d'isolation.** Les brouillons d'un agent sont à la merci d'une action d'un autre, et rien n'identifie l'auteur après coup.
- **`publish_all_lessons` n'a aucune granularité.** Un seul appel publie toutes les leçons d'un produit, y compris celles qu'on venait de créer et celles qu'on n'avait pas l'intention de toucher.
- **Une consigne de ne pas publier, donnée en langage naturel à un agent, n'est pas un contrôle d'accès.** C'est exactement ce que le livrable 05 démontre sur le produit ; cet incident le démontre sur notre propre dispositif.
- **`publishedAt` n'est pas remis à zéro par la dépublication.** Après `unpublish_lesson`, le statut repasse à `draft` mais l'horodatage de publication demeure. Un administrateur qui se fierait à `publishedAt` pour juger de l'état publierait un diagnostic faux : **le seul champ qui fait foi est `status`.**

**Recommandation opérationnelle, à reprendre au manuel :** relire l'état des objets après chaque campagne, et ne jamais considérer qu'un brouillon le reste parce qu'on l'a créé ainsi. La vérification coûte un appel.

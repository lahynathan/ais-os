# Échantillon de sites clients

**Aucun échantillon n'a pu être constitué.** Ce document existe pour dire pourquoi, et pour que ce trou soit visible plutôt que découvert par l'auditeur du fonds.

## Ce qui était prévu

Le plan d'enquête confiait à A08a la constitution d'un échantillon de 30 sites clients hébergés, stratifié par niche, langue et type d'adresse, avec pour chacun un relevé factuel : types de pages, fonctions visibles, scripts tiers, présence de mentions légales, de CGV et d'un bandeau cookies, et scores de performance, d'accessibilité et de SEO. A08b devait en tirer l'analyse des usages réels, de la qualité perçue et des risques de conformité côté clients.

## Pourquoi il n'existe pas

Les trois moyens de cette mission étaient tous inaccessibles le 18 septembre 2026 :

| Moyen | Usage prévu | État |
|---|---|---|
| Journaux de certificats, `crt.sh` | Découvrir les sous-domaines clients | Bloqué, 403 du proxy d'egress |
| API PageSpeed Insights, `googleapis.com` | Mesurer performance, accessibilité, SEO | Bloqué |
| Navigateur Playwright | Relever les pages et capturer | Sans objet, aucun site atteignable |
| Recherche `site:tinypages.co` | Amorcer une liste de candidats | Partiellement possible par WebSearch, mais sans relevé possible ensuite |

A08a et A08b n'ont donc pas été lancés. Les lancer aurait produit deux rapports vides et consommé des moyens sans rien établir.

## Ce que cette absence coûte au dossier

C'est le trou le plus visible de l'audit, et il touche des questions qu'un investisseur pose systématiquement :

- **Usage réel du produit.** Quelles fonctions les créateurs utilisent-ils vraiment ? Lesquelles sont ignorées ? Le dossier n'en dit rien.
- **Qualité de ce que la plateforme produit.** Les pages générées tiennent-elles la route en performance, en accessibilité, en SEO ? Non mesuré.
- **Conformité côté clients.** Combien de sites clients ont des mentions légales, des CGV, un bandeau cookies ? La question est d'autant plus aiguë que le screening a établi (M-007) que la plateforme publie des pages légales **vides** sur chaque compte. L'ampleur réelle de cet écart sur le parc entier est inconnue.
- **Traction.** Le nombre de sites détectables par les journaux de certificats aurait donné un ordre de grandeur indépendant des déclarations de la société.
- **Signaux d'abus.** Aucune vérification n'a pu être faite sur d'éventuelles pages frauduleuses hébergées sur le domaine.

## Les trois seuls sites connus, et une réserve

Le point de départ de l'audit citait `emilio.tinypages.co`, `lucasprotocole.tinypages.co` et `lefuturologue.tinypages.co`. Aucun n'a pu être ouvert.

A01 relève en outre que le premier porte le prénom du fondateur revendiqué. **Il s'agit probablement d'un site de démonstration ou personnel, pas d'un client tiers indépendant.** Si c'est confirmé, le nombre de sites clients réellement connus tombe de trois à deux. Ce détail compte : il faut éviter qu'un chiffre de trois circule dans un livrable.

## Comment combler ce trou

Une fois l'egress ouvert sur `crt.sh` et `googleapis.com`, la mission entière d'A08a tient en une session :

```
https://crt.sh/?q=%25.tinypages.co&output=json
https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=<url>&strategy=mobile&category=performance&category=accessibility&category=seo
```

Puis A08b sur le rapport produit. Compter une demi-journée pour les deux.

**Règle à conserver lors de ce rejeu** : les sites clients sont anonymisés dans les livrables (Site A, Site B…), la table de correspondance reste dans une annexe interne, et aucune donnée personnelle n'est relevée.

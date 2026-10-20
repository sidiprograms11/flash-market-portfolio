# Contributions de Sidi Oumar GANO

Cette page repose sur l'historique Git du projet local Flash Market, filtré sur `sidiprograms11`. Les identifiants de commits permettent de préparer un échange technique sans publier le code privé.

## Résumé

- **70 commits signés** `sidiprograms11` dans l'historique examiné.
- Contributions principalement dans `app/` (React Native / TypeScript), avec des migrations et fonctions dans `supabase/`.
- Travail concentré sur le parcours compte, la monétisation, l'espace professionnel, les métriques et la stabilité de l'application.

## Contributions produit et techniques

| Domaine | Réalisation | Référence Git |
| --- | --- | --- |
| Identité | Connexion / inscription téléphone, liaison d'e-mail facultative et console d'administration. | `49bea0f` — 26 juin 2026 |
| Consentement | Acceptation des CGV au moment de l'inscription. | `f6dfc1a` — 27 juin 2026 |
| Paiement | Renouvellement, historique de transactions et validation Orange Money côté parcours produit. | `af14cf8` — 2 juillet 2026 |
| Monétisation | Offres payantes, quotas et parcours Mobile Money. | `e0793d1`, `7b8fc20`, `0204fad` |
| Boost | Achat de boost, résultats de visibilité, expiration et métriques associées. | `8834393`, `7cc35ac`, `72bc883` |
| Espace professionnel | Découverte pro, boutique, statistiques de vues/clics et visites de boutique. | `805cd9a`, `1290576`, `2ac64d4` |
| Découverte | Fil, rotation équitable, tendances et logique de pertinence. | `0642909`, `1957bde`, `132c9bb` |
| Fiabilité mobile | Correction d'une page blanche à la navigation sur iPhone et améliorations de rendu en défilement rapide. | `6ffbfbf`, `efca606`, `47d4ec0` |
| Marketplace | Recherche, géolocalisation, messagerie et expérience invité. | `a743047`, `ba43357`, `7d3164d` |

## Contributions directement liées à la sécurité

### RLS et intégrité des abonnements de boutique

Dans `migration_ouvert_et_follow.sql`, j'ai ajouté la table des abonnements de boutique et ses protections :

- Row Level Security activée ;
- création et suppression autorisées uniquement si `auth.uid()` correspond au compte concerné ;
- contrainte empêchant de s'abonner à soi-même ;
- contrainte d'unicité pour empêcher les doublons.

Références : `09ff075`, `805cd9a` — 19 août 2026.

### Visites de boutique protégées côté serveur

Dans `migration_visites_boutique.sql`, j'ai implémenté un modèle de métriques qui ne fait pas confiance au client pour la lecture des données :

- RLS activée et droits directs retirés aux rôles client ;
- écriture encadrée par une fonction RPC dont le périmètre est réduit ;
- statistiques retournées seulement pour la boutique du compte connecté ;
- clé primaire `(boutique_id, jour, visiteur)` pour dédupliquer les visites ;
- exclusion des auto-visites et validation de format de l'identifiant visiteur.

Référence : `1290576` — 29 septembre 2026.

### Fonction de maintenance à privilège limité

Pour les boosts, `migration_boost_annonce.sql` contient une fonction `SECURITY DEFINER` volontairement étroite : elle ne peut qu'expirer les boosts arrivés à échéance. Elle ne reçoit aucune valeur permettant à un client de modifier une annonce arbitraire.

Référence : `8834393` — 31 août 2026.

## Ce que je n'attribue pas abusivement à mon travail

Le dépôt contient aussi des contrôles importants, dont la confidentialité avancée des profils, la modération, la suppression de compte et certains flux de paiement sécurisé. L'historique examiné attribue leur implémentation initiale à un autre contributeur. Ils relèvent de la sécurité globale du produit, mais ne sont pas présentés ici comme mes réalisations individuelles.

En entretien, je peux expliquer comment ces mécanismes interagissent avec mes fonctionnalités et comment je les testerais, sans en revendiquer l'implémentation.

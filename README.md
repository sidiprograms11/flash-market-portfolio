# Flash Market — ingénierie produit & sécurité applicative

> Marketplace mobile conçue pour le Mali, en production depuis trois mois et utilisée par plus de **2 000 personnes**.

Ce dépôt est volontairement **sans code métier, données utilisateurs, identifiants ni secrets**. Le code de Flash Market reste privé car il appartient à l'organisation qui porte le produit.

## Mon rôle

Je suis **Sidi Oumar GANO** (`sidiprograms11`), contributeur mobile et backend sur Flash Market. Mon historique Git local compte **70 commits signés** sur le projet, de l'authentification aux fonctionnalités professionnelles, en passant par les paiements, les métriques et la fiabilité de l'application.

Mon angle : construire des fonctionnalités qui restent utiles en production tout en appliquant des contrôles d'accès côté serveur, le principe du moindre privilège et des règles d'intégrité directement dans la base de données.

## Impact produit

| Indicateur | Résultat |
| --- | --- |
| Mise en production | 3 mois |
| Utilisateurs | 2 000+ |
| Plateforme | iOS / Android, via Expo et React Native |
| Produit | Marketplace C2C et espace professionnel au Mali |

> La métrique utilisateurs est communiquée par l'équipe produit. Je la remplace par un lien vers un tableau de bord public si celui-ci devient disponible.

## Ce que j'ai livré

- Parcours d'identité : inscription et connexion par téléphone, liaison d'un e-mail au compte, gestion de session et déconnexion.
- Consentement au parcours d'inscription avec acceptation des CGV.
- Parcours de monétisation : offres, paiement unique, suivi d'historique et intégration de flux Mobile Money côté produit.
- Espace professionnel : boutique, visibilité, visites, vues, clics et tableaux de bord dédiés aux vendeurs.
- Expérience marketplace : recherche, fil de découverte, boost d'annonces, immobilier, messagerie, géolocalisation et corrections de stabilité mobile.

Les références détaillées et attribuées sont disponibles dans [la liste de mes contributions](docs/contributions.md).

## Sécurité applicative : réalisations directement attribuables

Flash Market applique la sécurité au niveau de la base PostgreSQL/Supabase, et pas seulement dans l'interface mobile. Mes contributions vérifiables incluent notamment :

| Sujet | Implémentation | Bénéfice sécurité |
| --- | --- | --- |
| Contrôle d'accès aux abonnements de boutique | RLS avec `auth.uid()` pour créer ou supprimer uniquement ses propres abonnements ; contrainte d'unicité et interdiction de s'abonner à soi-même. | Empêche un compte de modifier les relations d'un autre compte et préserve l'intégrité des compteurs. |
| Mesure des visites | Table protégée par RLS, accès direct révoqué et fonctions RPC à privilèges restreints. | Les statistiques détaillées ne sont pas exposées directement au client. |
| Intégrité des métriques | Une visite est unique par boutique, visiteur et jour ; les auto-visites sont exclues. | Réduit le gonflage accidentel des métriques et rend les chiffres plus fiables. |
| Cycle de vie des boosts | Fonction SQL limitée à l'expiration des boosts déjà arrivés à terme. | Évite de donner au client un droit général de modification sur les annonces des autres comptes. |

Le détail technique, le raisonnement de sécurité et les limites connues figurent dans l'[étude de cas sécurité](docs/security-case-study.md).

## Stack

`React Native` · `Expo` · `TypeScript` · `Supabase Auth` · `PostgreSQL` · `Row Level Security` · `SQL / PLpgSQL` · `Supabase Realtime` · `Supabase Storage` · `Expo Notifications` · `EAS`

## Compétences mises en avant

- Sécurité des données applicatives : politiques RLS, contrôles d'accès par identité, moindre privilège, fonctions SQL à périmètre limité et contraintes de base de données.
- Sécurité du cycle de vie produit : données de démonstration, séparation du dépôt public et du code privé, configuration par variables d'environnement, préparation de publication.
- Développement mobile : React Native, Expo, TypeScript, navigation, performances et correctifs iOS.
- Backend : modélisation PostgreSQL, fonctions RPC, migrations, métriques produit et intégrations Supabase.

## Périmètre et intégrité de l'attribution

Flash Market est un projet d'équipe. Ce portfolio distingue mes implémentations traçables dans Git des mécanismes de sécurité conçus par d'autres contributeurs. Je ne revendique pas seul l'architecture complète de sécurité, les revues, l'infrastructure ni les contrôles de paiement qui ne sont pas attribués à mes commits.

Cette précision rend la présentation défendable en entretien : je peux expliquer ce que j'ai réellement construit, les choix effectués et les améliorations que je prioriserais.

## Ressources

- [Étude de cas : contrôle d'accès et intégrité](docs/security-case-study.md)
- [Contributions et références Git](docs/contributions.md)
- [Extrait prêt à copier dans un README de profil GitHub](PROFILE_SNIPPET.md)
- [Consignes pour ajouter des captures sans exposer de données](assets/README.md)

---

*Projet développé en équipe. Dépôt de présentation de ma contribution ; code source privé.*

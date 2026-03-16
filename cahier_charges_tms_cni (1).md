# Cahier des charges fonctionnel

## Training Management System (TMS)
### Centre National de l'Informatique (CNI) – Tunisie

---

# 1. Contexte

Le CNI souhaite digitaliser le cycle de vie des formations : planification, inscription, suivi, évaluation, certification et archivage.

---

# 2. Objectifs du projet

## 2.1 Objectif général

Mettre en place une plateforme centralisée pour gérer les formations.

## 2.2 Objectifs spécifiques

- Centraliser la gestion des formations
- Automatiser les tâches administratives
- Améliorer la planification
- Suivre participation et résultats
- Produire des tableaux de bord simples

---

# 3. Périmètre fonctionnel

Modules couverts :

1. Utilisateurs et rôles
2. Formations (catalogue)
3. Sessions
4. Inscriptions
5. Présences
6. Évaluations
7. Certificats
8. Reporting

---

# 4. Acteurs du système

## 4.1 Super administrateur

- Gère le système, les comptes et les rôles
- Gère les paramètres généraux

## 4.2 Gestionnaire formation

- Crée les formations
- Planifie les sessions
- Valide les inscriptions
- Suit les résultats

## 4.3 Formateur

- Anime les sessions
- Marque les présences
- Saisit les évaluations

## 4.4 Participant

- S'inscrit aux formations
- Participe aux sessions
- Passe les évaluations

---

# 5. Cycle de vie d'une formation

- Brouillon
- En validation
- Approuvée
- Planifiée
- Ouverte aux inscriptions
- En cours
- Terminée
- Certifiée
- Archivée

---

# 6. Fonctionnalités clés

## 6.1 Utilisateurs et rôles

- Créer / modifier / supprimer un utilisateur
- Attribuer un rôle

Champs utilisateur : Nom, Prénom, Email, Département (optionnel), Rôle

## 6.2 Formations

Champs : Titre, Description, Objectifs, Durée, Niveau

- Créer / modifier / supprimer une formation

## 6.3 Sessions

Champs : Date début, Date fin, Lieu, Formateur, Capacité maximale

## 6.4 Inscriptions

Statuts : Demandée, Approuvée, Rejetée, Liste d'attente

- Demande d'inscription
- Validation par le gestionnaire

## 6.5 Présences

- Enregistrer la présence
- Consulter un rapport de présence

## 6.6 Évaluations

- Quiz simple
- Note sur 20

## 6.7 Certificats

Conditions : Présence >= 80% et Note >= 10/20

Contenu : Nom participant, Formation, Date, Signature

## 6.8 Reporting

- Nombre total de formations
- Nombre de participants
- Taux de participation
- Taux de réussite
- Performance des formateurs

---

# 7. Règles de gestion

1. Un participant ne peut pas suivre deux sessions qui se chevauchent.
2. Un formateur ne peut pas animer deux sessions en même temps.
3. Le nombre de participants ne dépasse pas la capacité maximale.
4. Un certificat est délivré seulement si les critères sont respectés.

---

# 8. Modèle de données simplifié

## Entités principales

### Utilisateur

- id
- nom
- prénom
- email
- rôle
- département (optionnel)

### Formation

- id
- titre
- description
- durée
- niveau

### Session

- id
- formation_id
- formateur_id
- date_début
- date_fin
- lieu
- capacité

### Inscription

- id
- participant_id
- session_id
- statut

### Présence

- id
- session_id
- participant_id
- présent

### Évaluation

- id
- session_id
- participant_id
- note

### Certificat

- id
- participant_id
- formation_id
- date_émission

---

# 9. Exigences non fonctionnelles

## Sécurité

- Authentification sécurisée
- Accès par rôles
- Protection des données

## Performance

- Temps de réponse < 3 secondes
- Support multi-utilisateurs

## Disponibilité

- Système disponible 24/7

## Traçabilité

- Historique des actions

---

# 10. Livrables

- Plateforme TMS
- Documentation utilisateur
- Documentation technique
- Tableaux de bord simples
- Génération de certificats

---

# 11. Propositions d'amélioration (optionnelles)

- Notifications email pour inscriptions, rappels et validations
- Intégration calendrier (invitation de session)
- Import / export Excel des participants et sessions
- Enquête de satisfaction à chaud après formation
- Connexion SSO (Active Directory) si disponible
- API simple pour intégration avec d'autres outils internes

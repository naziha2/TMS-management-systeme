# UML & ERD — TMS (CNI)

## UML — Cas d’utilisation
```mermaid
flowchart LR
  SA([Super Administrateur])
  GF([Gestionnaire Formation])
  F([Formateur])
  P([Participant])

  subgraph TMS[Training Management System]
    UC1((Gérer utilisateurs & rôles))
    UC2((Gérer paramètres généraux))
    UC3((Gérer formations))
    UC4((Planifier sessions))
    UC5((Valider inscriptions))
    UC6((Enregistrer présences))
    UC7((Saisir évaluations))
    UC8((Générer certificats))
    UC9((Consulter reporting))
    UC10((Demander inscription))
    UC11((Participer à une session))
    UC12((Passer évaluation))
    UC13((Télécharger certificat))
  end

  SA --> UC1
  SA --> UC2
  GF --> UC3
  GF --> UC4
  GF --> UC5
  GF --> UC8
  GF --> UC9
  F --> UC6
  F --> UC7
  P --> UC10
  P --> UC11
  P --> UC12
  P --> UC13

  UC5 -.-> UC10
  UC8 -.-> UC6
  UC8 -.-> UC7
```

## UML — Diagramme de classes
```mermaid
classDiagram
  class Role {
    +UUID id
    +string code
    +string libelle
  }

  class Departement {
    +UUID id
    +string nom
  }

  class Utilisateur {
    +UUID id
    +string nom
    +string prenom
    +string email
    +UUID role_id
    +UUID departement_id
    +boolean actif
    +datetime created_at
  }

  class Formation {
    +UUID id
    +string titre
    +text description
    +text objectifs
    +int duree_heures
    +string niveau
    +string etat
    +datetime created_at
  }

  class Session {
    +UUID id
    +UUID formation_id
    +UUID formateur_id
    +datetime date_debut
    +datetime date_fin
    +string lieu
    +int capacite
    +string statut
  }

  class Inscription {
    +UUID id
    +UUID session_id
    +UUID participant_id
    +string statut
    +datetime date_demande
    +datetime date_validation
    +string motif_rejet
  }

  class Presence {
    +UUID id
    +UUID session_id
    +UUID participant_id
    +boolean present
    +datetime date_saisie
  }

  class Evaluation {
    +UUID id
    +UUID session_id
    +UUID participant_id
    +decimal note
    +datetime date_evaluation
  }

  class Certificat {
    +UUID id
    +UUID session_id
    +UUID participant_id
    +string numero
    +datetime date_emission
  }

  Role "1" --> "0..*" Utilisateur : attribue
  Departement "0..1" --> "0..*" Utilisateur : appartient
  Formation "1" --> "0..*" Session : planifie
  Utilisateur "1" --> "0..*" Session : anime
  Utilisateur "1" --> "0..*" Inscription : demande
  Session "1" --> "0..*" Inscription : recoit
  Session "1" --> "0..*" Presence : enregistre
  Utilisateur "1" --> "0..*" Presence : concerne
  Session "1" --> "0..*" Evaluation : contient
  Utilisateur "1" --> "0..*" Evaluation : passe
  Session "1" --> "0..*" Certificat : delivre
  Utilisateur "1" --> "0..*" Certificat : obtient
```

## UML — Diagramme de séquence (inscription + validation)
```mermaid
sequenceDiagram
  participant Participant
  participant UI
  participant Service
  participant Regles
  participant DB
  participant Gestionnaire
  participant Notification

  Participant->>UI: Demander inscription
  UI->>Service: createInscription(session_id, participant_id)
  Service->>Regles: verifierCapaciteEtChevauchement()
  Regles->>DB: lire sessions/inscriptions
  DB-->>Regles: donnees
  Regles-->>Service: OK | ListeAttente | Rejet
  Service->>DB: insert Inscription (Demandée)
  Service->>Notification: notifier gestionnaire + participant

  Gestionnaire->>UI: Valider inscription
  UI->>Service: validerInscription(inscription_id)
  Service->>Regles: reVerifierCapacite()
  Regles->>DB: lire inscriptions
  DB-->>Regles: donnees
  Regles-->>Service: OK | ListeAttente | Rejet
  Service->>DB: update Inscription
  Service->>Notification: notifier participant
```

## UML — Diagramme d’activité (processus d’inscription)
```mermaid
flowchart TD
  A([Début]) --> B[Participant demande inscription]
  B --> C{Session complète ?}
  C -- Non --> D{Chevauchement avec autre session ?}
  C -- Oui --> F[Statut: Liste d'attente ou Rejetée]
  D -- Non --> E[Créer inscription: Demandée]
  D -- Oui --> F
  E --> G[Gestionnaire valide]
  G --> H{Capacité encore dispo ?}
  H -- Oui --> I[Statut: Approuvée]
  H -- Non --> F
  I --> J[Notifier participant]
  F --> J
  J --> K([Fin])
```

## UML — Diagramme d’états (cycle de vie formation)
```mermaid
stateDiagram-v2
  [*] --> Brouillon
  Brouillon --> En_validation : soumettre
  En_validation --> Approuvee : approuver
  En_validation --> Brouillon : demander_modif
  Approuvee --> Planifiee : planifier_sessions
  Planifiee --> Ouverte : ouvrir_inscriptions
  Ouverte --> En_cours : demarrer_session
  En_cours --> Terminee : cloturer
  Terminee --> Certifiee : emettre_certificats
  Certifiee --> Archivee : archiver
  Archivee --> [*]
```

## ERD — Diagramme détaillé
```mermaid
erDiagram
  ROLE {
    UUID id PK
    string code
    string libelle
  }

  DEPARTEMENT {
    UUID id PK
    string nom
  }

  UTILISATEUR {
    UUID id PK
    string nom
    string prenom
    string email UK
    UUID role_id FK
    UUID departement_id FK
    boolean actif
    datetime created_at
  }

  FORMATION {
    UUID id PK
    string titre
    text description
    text objectifs
    int duree_heures
    string niveau
    string etat
    datetime created_at
  }

  SESSION {
    UUID id PK
    UUID formation_id FK
    UUID formateur_id FK
    datetime date_debut
    datetime date_fin
    string lieu
    int capacite
    string statut
  }

  INSCRIPTION {
    UUID id PK
    UUID session_id FK
    UUID participant_id FK
    string statut
    datetime date_demande
    datetime date_validation
    string motif_rejet
  }

  PRESENCE {
    UUID id PK
    UUID session_id FK
    UUID participant_id FK
    boolean present
    datetime date_saisie
  }

  EVALUATION {
    UUID id PK
    UUID session_id FK
    UUID participant_id FK
    decimal note
    datetime date_evaluation
  }

  CERTIFICAT {
    UUID id PK
    UUID session_id FK
    UUID participant_id FK
    string numero UK
    datetime date_emission
  }

  ROLE ||--o{ UTILISATEUR : attribue
  DEPARTEMENT ||--o{ UTILISATEUR : appartient
  FORMATION ||--o{ SESSION : planifie
  UTILISATEUR ||--o{ SESSION : anime
  UTILISATEUR ||--o{ INSCRIPTION : demande
  SESSION ||--o{ INSCRIPTION : recoit
  SESSION ||--o{ PRESENCE : enregistre
  UTILISATEUR ||--o{ PRESENCE : concerne
  SESSION ||--o{ EVALUATION : contient
  UTILISATEUR ||--o{ EVALUATION : passe
  SESSION ||--o{ CERTIFICAT : delivre
  UTILISATEUR ||--o{ CERTIFICAT : obtient
```

## Hypothèses et contraintes
- CERTIFICAT est lié à une SESSION (la formation est déduite via SESSION.formation_id).
- Unicité logique: UNIQUE(session_id, participant_id) pour INSCRIPTION, PRESENCE, EVALUATION, CERTIFICAT.
- EVALUATION.note dans [0, 20], certificat si présence >= 80% et note >= 10.
- Règles de chevauchement et capacité appliquées au niveau service.

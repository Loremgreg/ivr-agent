sequenceDiagram
    actor User
    participant Frontend as Frontend (React)
    participant Backend as Backend (Python/Flask)
    participant LiveKit
    participant IVR as Phone Network (IVR)

    User->>Frontend: Clique sur "Connect"
    Frontend->>Backend: Demande un token (/api/generate-token)
    Backend-->>Frontend: Retourne le token
    Frontend->>LiveKit: S'authentifie et se connecte à la Room

    User->>Frontend: Entre la tâche et le numéro de tél.
    User->>Frontend: Clique sur "Call"
    
    Frontend->>Backend: Demande de lancer l'appel (/api/call)
    
    Backend->>LiveKit: Initie l'appel SIP sortant via le Trunk
    Note over Backend,LiveKit: Le backend passe la tâche (ex: "appuyer sur 1")<br/>et l'agent (agent.py) est démarré.

    LiveKit->>IVR: Passe l'appel téléphonique réel

    IVR-->>LiveKit: Le flux audio du menu vocal est envoyé
    LiveKit-->>Backend: L'agent reçoit l'audio

    Note right of Backend: L'agent analyse l'audio (STT),<br>comprend la tâche (LLM),<br>et décide de l'action (ex: envoyer "1").

    Backend->>LiveKit: Envoie la commande DTMF "1"
    LiveKit->>IVR: La tonalité "1" est jouée dans l'appel

    Note over IVR: Le menu SVI réagit à la tonalité.
    
    loop Navigation dans le menu
        IVR-->>LiveKit: Nouveau flux audio
        LiveKit-->>Backend: L'agent écoute
        Backend->>LiveKit: Envoie la commande DTMF suivante
        LiveKit->>IVR: Joue la tonalité
    end

    Note right of Backend: Une fois la tâche terminée,<br>l'agent raccroche, transcrit,<br>résume et sauvegarde le tout.
# Directives pour le projet IVR_menu_caller 

Ce document fournit les instructions et le contexte nécessaires pour travailler sur ce projet d’agent de traitement de messagerie vocale.

⸻

## 1. Contexte du Projet
    •   **Objectif :**  Créer un agent utilitaire qui appelle une boîte de messagerie, écoute passivement, transcrit les messages, et les sauvegarde.
    •   **Fonctionnalités Clés :** 
    •   Appel Sortant : L’agent doit lancer un appel vers un numéro prédéfini.
    •   Écoute Passive : L’agent ne parle jamais.
    •   Transcription Intégrale : L’agent transcrit la totalité du flux audio.
    •   Résumé (via LLM) : L’agent utilise OpenAI `gpt-4o-mini` pour résumer les transcriptions et en extraire les informations clés (nom, numéro de rappel, sujet).
    •   Sauvegarde : La transcription et le résumé sont sauvegardés dans un format structuré.
    •   Raccrocher : L’agent met fin à l’appel proprement.
    •   Persona de l’Agent : Inexistant. C’est un outil silencieux.

⸻

## 2. Pile Technique Principale
    •   **Langage :** Python 
    •   **Framework d'Agent :** LiveKit Agents
    •   **LLM (Cerveau) :** OpenAI `gpt-4o-mini`
    •   **STT (Transcription) :** Deepgram `nova-3` (multilingue)
    •   **VAD (Détection de voix) :** Silero VAD

⸻

## 3. Conventions et Architecture
    •  **`agent.py` :** C'est le point d'entrée principal. Il initialise la `AgentSession` de LiveKit et connecte tous les services (STT, LLM).
    •  **`requirements.txt` :** Toutes les dépendances doivent y être listées.
    • **`app.py` :** Micro-serveur Flask coiffant l’agent  
        - Initialise **Flask** + **Flask-LiveKit** (chargement des variables LIVEKIT\_* et du SIP\_TRUNK\_ID).  
        - Sert l’UI React située dans `frontend/dist` (routes statiques + fallback sur `index.html`).  
        - Active CORS pour les appels front-end ⇆ back-end.  
        - Expose un gestionnaire d’erreurs 500 avec log détaillé.  
        - Peut être lancé en local (`python app.py`) pour tester l’interface web sur <http://localhost:5001>.  
⸻

## 4. Règle Critique : Sécurité et Conformité RGPD

    •   Priorité absolue : La protection et la confidentialité des transcriptions.
    • **Configuration :** Les clés API et autres secrets **doivent** être chargés depuis un fichier `.env.local` et ne jamais être écrits en dur dans le code. Les valeurs de configuration (nom des modèles) doivent également y figurer.
⸻

## 5. Documentation de Référence (Projet IVR_menu_caller )

Logique Téléphonique

Ces documents sont essentiels pour le cœur du projet.

    • **Building an Automated IVR Menu Caller :** https://docs.livekit.io/recipes/ivr-navigator/
Pourquoi : Build an AI agent that can call phone numbers and navigate IVR menus by listening and sending DTMF codes.

    • **AI Voice Assistant Quickstart :** https://docs.livekit.io/agents/v0/quickstarts/voice-agent/
Pourquoi : Build an AI-powered voice assistant that engages in realtime conversations using LiveKit, Python, and NextJS.

    • **SIP trunk setup :** https://docs.livekit.io/sip/quickstarts/configuring-sip-trunk/ 
Pourquoi : Guide to setting up SIP trunks for inbound and outbound calls with LiveKit.

    • **SIP trunk setup :** https://docs.livekit.io/sip/trunk-outbound/ 
Pourquoi : How to create and configure a outbound trunk to make outgoing calls.

 • **Playground :** https://docs.livekit.io/agents/start/playground/


⸻

## 6. Bonnes Pratiques et Leçons Apprises
    •   **Démarrage de l’Agent :** Le pré-téléchargement des modèles reste une bonne pratique si d’autres modèles (comme les embeddings) sont utilisés.
    •   **Température du LLM :** Pour le résumé, une température basse (0.1 ou 0.2) est fortement recommandée pour obtenir des résumés factuels, précis et éviter les “hallucinations” ou l’ajout d’informations non présentes dans la transcription.

## 7. Exemple 
    •  **Pre construit :** le fichier `agent.py` est déja construit
    •  **Pre construit :** le fichier `app.py` est déja construit



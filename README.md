# Simple Webapp CI Pipeline mit Jenkins und Docker

Dieses Projekt demonstriert eine einfache CI-Pipeline für eine React-basierte Webanwendung mit [Vite](https://vitejs.dev/), die mittels Jenkins (in Docker) automatisiert gebaut wird.

## Ziel

- Lokale Jenkins-Instanz mit Docker starten
- Code aus einem Git-Repository klonen
- Pipeline über ein `Jenkinsfile` definieren
- Build-Prozess der Webanwendung automatisiert ausführen

## Voraussetzungen

Stelle sicher, dass folgende Tools lokal installiert sind:

- [Docker](https://www.docker.com/)
- [Git](https://git-scm.com/)
- [Node.js und npm](https://nodejs.org/) (für lokale Tests der App)

## Projektstruktur

```plaintext
simple-webapp/
├── jenkins-node/
│   └── Dockerfile               # Dockerfile mit Node.js und Git für Jenkins
├── public/                      # Assets der React-App
├── src/                         # Quellcode der React-App
│   └── App.jsx
│   └── main.jsx
├── .gitignore
├── Jenkinsfile                  # Pipeline-Definition für Jenkins
├── package.json                 # npm Projektkonfiguration
├── vite.config.js               # Vite Build-Konfiguration
├── README.md                    # Projektbeschreibung
└── reflexion.md                 # Reflexionsfragen zur Aufgabe
```	
## Webanwendung erstellen

Falls du das Projekt selbst erstellen möchtest:

```bash
npm create vite@latest simple-webapp -- --template react
cd simple-webapp
npm install
git init
git add .
git commit -m "Initial commit"
```

Danach das Repository auf GitHub hochladen:
```bash
git remote add origin https://github.com/<dein-username>/simple-webapp.git
git push -u origin main
```
### Jenkins in Docker mit Node.js starten

Erstelle ein eigenes Dockerfile:
```dockerfile
FROM jenkins/jenkins:lts

USER root

RUN curl -fsSL https://deb.nodesource.com/setup_16.x | bash - && \
    apt-get install -y nodejs && \
    npm install -g npm && \
    apt-get install -y git

USER jenkins
```
Build und Start des Containers:
```bash
docker build -t my-jenkins-node .
docker run --name my-simple-jenkins -p 8080:8080 -v jenkins_home:/var/jenkins_home my-jenkins-node
```
### Jenkins konfigurieren
- Browser öffnen: http://localhost:8080

- Initiales Admin-Passwort mit docker logs my-simple-jenkins auslesen

- Suggested Plugins installieren

- Admin-User anlegen

- Dashboard erscheint

- Pipeline-Job erstellen
"New Item" → Name: SimpleWebApp-CI → Typ: Pipeline

- SCM: Git

- Repository URL: https://github.com/<dein-username>/simple-webapp.git

- Branch: */main

- Script Path: Jenkinsfile

### Jenkinsfile
```groovy
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'npm ci'
                sh 'npm run build'
            }
        }
    }
}
```	

- Pipeline ausführen
- Committe das Jenkinsfile in dein Repository

- In Jenkins: Job auswählen → "Build Now"

- Console Output zeigt den Verlauf der Schritte (Checkout, Build)

### Screenshots
#### Jenkins läuft (Dashboard)
![Jenkins Dashboard](screenshots/Jenkins_Dashboard.png)

#### Pipeline-Konfiguration (SCM)
![Pipeline Overview](screenshots/Pipeline_Overview.png)

#### Erfolgreicher Pipeline-Lauf (Console Output)
![Console Output](screenshots/Console_Output.png)

### Aufräumen
```bash
docker stop my-simple-jenkins
docker rm -v my-simple-jenkins
docker volume rm jenkins_home
```


# Reflexion

## 1. Welche Schritte waren notwendig, um Jenkins lokal mit Docker zum Laufen zu bringen?

- Docker-Image mit Node.js erstellt (Dockerfile)
- Image gebaut und Container gestartet
- Initiales Setup im Browser durchgeführt (Unlock, Plugins, Admin-Benutzer)

## 2. Was ist der Zweck der Datei Jenkinsfile, und wo muss sie im Verhältnis zu deinem Anwendungscode liegen?

Das `Jenkinsfile` definiert die Pipeline-Stufen (z. B. Checkout, Build) und enthält die konkreten Anweisungen für Jenkins. Es liegt im Root-Verzeichnis des Projekts (neben `package.json`).

## 3. Beschreibe die zwei Hauptstages, die du in deiner Pipeline definiert hast, und was der Hauptzweck der Steps in jeder Stage ist.

- **Checkout:** Holt den Quellcode aus dem Git-Repository (mit `checkout scm`).
- **Build:** Führt den Build-Prozess der App aus (`npm ci`, `npm run build`).

## 4. Wie hast du in Jenkins konfiguriert, von welchem Git-Repository und welchem Branch der Code für die Pipeline geholt werden soll?

Im Pipeline-Job wurde die Option "Pipeline script from SCM" gewählt, SCM-Typ auf "Git" gesetzt, die Repository-URL angegeben und der Branch auf `*/main` gesetzt.

## 5. Was ist der Unterschied zwischen dem `checkout scm` Step in deiner Pipeline und dem `git clone` Befehl, den du manuell im Terminal ausführen würdest?

`checkout scm` ist eine Jenkins-interne Abstraktion, die automatisch die im Job konfigurierte SCM-Quelle verwendet, einschließlich Branch und Credentials. Es ersetzt den manuellen `git clone` und macht Pipelines portabler und wartbarer.

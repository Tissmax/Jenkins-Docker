pipeline {
    agent any

    stages {
      stage('Checkout') {
            steps {
                git 'https://github.com/Tissmax/Jenkins-Docker'
            }
        }
      stage('Verify Files') {
            steps {
                sh 'if [ -f index.html ] && [ -f Dockerfile ]; then echo "Files OK"; else exit 1; fi'
            }
        }

stage('Ajout de la date') {
    steps {
        sh '''
        DATE=$(date '+%Y-%m-%d %H:%M:%S')
        cat <<EOF > index.html
        <!DOCTYPE html>
        <html lang="fr">
        <head>
          <meta charset="UTF-8">
          <title>Déploiement Jenkins</title>
          <style>
          body {
            font-family: Arial, sans-serif;
            background-color: #f4f4f4;
            text-align: center;
            padding: 50px;
          }
          .timestamp {
            font-size: 1.5em;
            color: #333;
          }
          </style>
        </head>
        <body>
          <h1>Déploiement réussi 🎉</h1>
          <p>Ce fichier a été généré le :</p>
          <p class="timestamp">$DATE</p>
        </body>
        </html>
        EOF
        '''
    }
}
      stage('Build Image') {
            steps {
                sh 'docker build -t monsite-docker .'
            }
      }
      stage('Deploy Container') {
          steps {
              sh 'docker stop monsite-docker-conteneur || true'
              sh 'docker rm monsite-docker-conteneur || true'
              sh 'docker run -d -p 8081:80 --name monsite-docker-conteneur monsite-docker'
          }
      }
    }
}
pipeline {
    agent any

    stages {
        stage("Dependances") {
            steps {
                // Installer apache2
                sh '''
                    sudo apt update
                    sudo apt install -y apache2
                '''
             }
        }

        stage('Checkout') {
            steps {
                 // Récupération du code
                 echo "📦 Récupération du code source depuis GitHub..."
                 checkout scm
            }
        }
        stage('Backup') {
            steps {
                echo "💾 Sauvegarde du répertoire /var/www/html..."
                sh 'sudo cp -r /var/www/html /var/www/html.backup || true'
            }
        }

        stage('Deploy') {
            steps {
                // Copie des fichiers vers le serveur web (/var/www/html/)
                echo "🚀 Déploiement du site web dans /var/www/html..."
                sh 'sudo cp -r * /var/www/html/'
            }
        }

        stage('Test') {
            steps {
                // Vérification du déploiement
                echo "🔍 Vérification que le site répond..."
                sh 'curl -f http://localhost/ || exit 1'
            }
        }
   }

   post {
           success {
               echo "✅ Déploiement terminé avec succès !"
           }
           failure {
               echo "❌ Le déploiement a échoué !"
           }
           always {
               echo "🧹 Nettoyage..."
               sh '''
                   sudo rm -rf /var/www/html/*
                   sudo apt remove -y apache2 || true
                   sudo apt autoremove -y
               '''
           }
       }
}
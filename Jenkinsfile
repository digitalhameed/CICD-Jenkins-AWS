node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean workspace') {
        echo 'Cleaning working directory'
        deleteDir()
    }

    stage('Clone repo') {
        echo 'Cloning repo'
        git branch: 'main', url: 'https://github.com/digitalhameed/CICD-Jenkins-AWS'
    }

    stage('Deploy to EC2') {
        echo 'Deploying to EC2'

        sh """
            mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}

            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}/

            cd ${appDir}
            npm install
            npm run build
            sudo fuser -k 3000/tcp || true
            npm run start &
        """
    }
}

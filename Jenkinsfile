node {
    def appDir = '/var/www/nextjs-app'

    stage('Clean Workspace') {
        echo 'Cleaning workspace'
        deleteDir()
    }

    stage('Clone Repo') {
        echo 'Cloning repo'
        git branch: 'main', url: 'https://github.com/digitalhameed/CICD-Jenkins-AWS'
    }

    stage('Deploy to EC2') {
        echo 'Deploying to EC2'

        sh """
            # Create app directory with proper permissions
            sudo mkdir -p ${appDir}
            sudo chown -R jenkins:jenkins ${appDir}

            # Sync files (exclude git & node_modules)
            rsync -av --delete --exclude='.git' --exclude='node_modules' ./ ${appDir}/

            # Install dependencies and build
            cd ${appDir}
            npm install
            npm run build

            # Stop any app running on port 3000
            sudo fuser -k 3000/tcp || true

            # Start the app using PM2 (restarts if already running)
            pm2 start npm --name "nextjs-app" -- start
            pm2 save
        """
    }

    stage('Verify Deployment') {
        echo 'Checking if the app is running'
        sh "pm2 status nextjs-app"
    }
}

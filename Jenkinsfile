pipeline {
    agent any
    
    enviroment {
        VERCEL_TOKEN = credentials('vercel_token')
    }

    stages{
        stage('Install')
        {
            steps{
                bat 'npm install'    
            }
        }

         stage('Test')
        {
            steps{
              echo 'No test script found'
            }

        }

         stage('Build')
        {
            steps{
                bat 'npm run build'    
            }

        }

         stage('Deploy')
        {
            steps{
                bat 'npx vercel --prod --yes --toke=%VERCEL_TOKEN%'    
            }

        }
    }   
    
}
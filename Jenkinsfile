pipeline{
    agent any
    stages{
        stage('build no test'){
            steps{
                nodejs(nodeJSInstallationName: 'nodejs'){
                    sh 'npm install'
                    sh 'npm rebuild'
                    sh 'npm run build --skip-test --if--present'
                }
                
            }
        }
        stage('build w/ test'){
            steps{
                nodejs(nodeJSInstallationName: 'nodejs'){
                    sh 'npm run test:coverage'
                    archiveArtifacts(artifacts: 'coverage/**',onlyIfSuccesful:true)
                }
                
            }
        }
        stage('deploy'){
            steps{
                nodejs(nodeJSInstallationName: 'nodejs'){
                    withAWS(credentials: 'aws-credentials'){
                        sh 'serverless deploy'
                    }
                }

            }
        }
    }
}
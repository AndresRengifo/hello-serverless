pipeline{
    agent any
    stages{
        stage('build no test'){
            steps{
                nodejs(nodeJSInstallationName: 'nodejs_15_5_1'){
                    sh 'npm install'
                    sh 'npm rebuild'
                    sh 'npm run build --skip-test --if--present'
                }
                
            }
        }
        stage('build w/ test'){
            steps{
                nodejs(nodeJSInstallationName: 'nodejs_15_5_1'){
                    sh 'npm run test:coverage && cp coverage/lcov.info lcov.info || echo "Code coverage failed"'
                    archiveArtifacts(artifacts: 'coverage/**',onlyIfSuccessful:true)
                }
                
            }
        }
        stage('deploy'){
            steps{
                nodejs(nodeJSInstallationName: 'nodejs_15_5_1'){
                    withAWS(credentials: 'aws-credentials'){
                        sh 'serverless deploy'
                    }
                }

            }
        }
    }
}
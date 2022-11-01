pipeline{
    agent any
    tools {
        maven 'maven-3.8.6'
    }
    stages {
        stage('build jar'){
            steps{
                script{
                    echo "building app"
                    sh 'mvn package'
                }
            }
        }
        stage('build image'){
            steps{
                script{
                    echo "building docker image"
                    withCredentials([usernamePassword(credentialsId:'dockerhub-login',passwordVariable: 'PASS',usernameVariable:'USER')]){
                        sh 'docker build -t pranavpo/my-repo:2.0 .'
                        sh "echo $PASS docker login -u $USER --password-stdin"
                        sh 'docker push pranavpo/my-repo:2.0'
                    }
                }
            }
        }
        stage('deploy'){
            steps{
                script{
                    echo "deploying app"
                }
            }
        }
    }
}

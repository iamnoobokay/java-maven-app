#!/usr/bin/env groovy
def gv
@Library('jenkins-shared-library')_

pipeline{
    agent any
    tools {
        maven 'maven-3.8.6'
    }
    stages {
        stage('init'){
            steps{
                script{
                    gv = load 'code.groovy'
                }
            }
        }
        stage('increment version'){
            steps{
                script{
                    echo 'incrementing app version...'
                    sh 'mvn build-helper:parse-version versions:set \
                    -DnewVersion=\\\${parsedVersion.majorVersion}.\\\${parsedVersion.nextMinorVersion} \
                    versions:commit'

                    def matcher = readFile('pom.xml') =~ "<version>(.+)</version>"
                    def version = matcher[0][1]
                    env.IMAGE_NAME = "$version-$BUILD_NUMBER"
                }
            }
        }
        stage('build jar'){
            when{
                expression{
                    BRANCH_NAME == 'master'
                }
            }
            steps{
                script{
                    // using groovy script
                    // gv.buildJar();

                    // using jenkins shared library
                    // buildJar()

                    echo "building app for branch $BRANCH_NAME"
                    sh 'mvn clean package'
                }
            }
        }
        stage('build image'){
            when{
                expression{
                    BRANCH_NAME == 'master'
                }
            }
            steps{
                script{
                    // using groovy script
                    // gv.buildImage();

                    // using jenkins shared library
                    // buildImage 'pranavpo/my-repo:3.0'

                    // bump version
                    echo "building docker image"
                    withCredentials([usernamePassword(credentialsId:'dockerhub-login',passwordVariable: 'PASS',usernameVariable:'USER')]){
                        sh "docker build -t pranavpo/my-repo:${IMAGE_NAME} ."
                        sh "echo $PASS docker login -u $USER --password-stdin"
                        sh "docker push pranavpo/my-repo:${IMAGE_NAME}"
                        // pranavpo/my-repo:2.0
                    }
                }
            }
        }
        stage('deploy'){
            when{
                expression{
                    BRANCH_NAME == 'master'
                }
            }
            steps{
                script{
                    gv.deployApp();
                }
            }
        }
    }
}

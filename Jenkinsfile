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
                    gv = 'code.groovy'
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
                    // gv.buildJar();
                    buildJar()
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
                    // gv.buildImage();
                     buildImage 'pranavpo/my-repo:3.0'
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

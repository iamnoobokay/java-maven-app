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
                    bulidJar()
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
                     bulidImage()
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
                    gv.deploy();
                }
            }
        }
    }
}

@Library('jenkins_share_lib') _
pipeline{
    agent any
    
    parameters{
        choice(name: 'action', choices: 'create\ndelete', description:'Choose Create/Destroy')
        string(name: 'ImageName', description: "Name of the docer build", defaultValue: 'javapp')
        string(name: 'ImageTag', description: "Tag of the docer build", defaultValue: 'v1')
        string(name: 'DokerHubUser', description: "User Name for dockerhub user", defaultValue: 'mgmbcssantosh86')
    }

    stages{

        stage('Git Checkout'){
            when { expression { params.action == 'create' } }
            steps{
                gitCheckout(
                    branch: "main",
                    url: "https://github.com/pranaviambati/appjava.git"
                )
            }
        }
        stage('Unit Test maven'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    mvnTest()
                }
            }
        }
        stage('Integeration Test maven'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    mvnIntegerationTest()
                }
            }
        }
        stage('Static Code Analysis : Sonarqube'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    def SonarQubecredentialsId = 'jenkins-sonar'
                    staticCodeAnalysis(SonarQubecredentialsId)
                }
            }
        }
        stage('Quality Gate Status Check : Sonarqube'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    def SonarQubecredentialsId = 'jenkins-sonar'
                    qualityGateStatus(SonarQubecredentialsId)
                }
            }
        }
        stage('Maven Build : maven'){
            when { expression { params.action == 'create' } }
            steps{
                script{
                    mvnBuild()
                }
            }
        }
      }
     }

pipeline{
    agent any
    tools{
        jdk 'JDK'
        maven 'Maven'
    }
    stages{
        stage('Git Checkout'){
            steps{
                git branch: 'ci-jenkins', url: 'https://github.com/Abionaraji/personally-project.git'
            }
        }
        stage('Test Unit'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Integration Testing'){
            steps{
                sh 'mvn verify -DiskipUnitTest'
            }
        }
        stage('Maven Build'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Sonaqube Analysis'){
            steps{
                withSonarQubeEnv(installationName: 'sonarqube-9', credentialsId: 'jenkins-sonar') {
                    sh 'sonar:sonar'
                }
            }
        }
    }
}
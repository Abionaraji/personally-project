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
        stage('Build Maven'){
            steps{
                sh 'mvn clean install'
            }
        }
        stage('Test Unit'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Intergrated Testing'){
            steps{
                sh 'mvn verify -DiskipUnitTest'
            }
        }
        stage('Sonarqube Analysis'){
            steps{
                withSonarQubeEnv(credentialname: 'sonarqube-9', credentialsId: 'sonar-jenks') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
    }
}
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
                withSonarQubeEnv(installationName: 'SonarQube', credentialsId: 'jenkins-sonar') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Quality Gate Status'){
            steps{
                waitForQualityGate abortPipeline: true, credentialsId: 'jenkins-sonar'
                sh 'mvn sonar:sonar'
            }
        }
    }
}
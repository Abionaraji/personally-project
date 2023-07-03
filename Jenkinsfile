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
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Quality Gate'){
            steps{
                waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonar'
            }
        }
        stage('Deploy War on Nexus'){
            steps{
                nexusArtifactUploader artifacts: 
                [
                    [
                        artifactId: 'vprofile', 
                        classifier: '', 
                        file: 'target/vprofile-v2.war', 
                        type: 'war'
                        ]
                    ], 
                    credentialsId: 'nexus-jenkins', 
                    groupId: 'QA', 
                    nexusUrl: '44.204.234.1:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'vpro-maven', 
                    version: 'v2'
            }
        }
    }
}
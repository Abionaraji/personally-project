def COLOR_MAP = [
    'SUCCESS':'good',
    'FAILURE':'danger'
]

pipeline{
    agent any
    tools{
        maven 'Maven'
        jdk 'JDK'
    }
    stages{
        stage('Git Checkout'){
            steps{
                git branch: 'ci-jenkins', url: 'https://github.com/Abionaraji/personally-project.git'
            }
        }
        stage('Maven Build'){
            steps{
                sh 'mvn clean install'
            }
            post {
                success{
                    echo 'New achiving'
                    archiveArtifacts artifacts: '**/*.war'
                }
            }
        }
        stage('Unit Test'){
            steps{
                sh 'mvn test'
            }
        }
        stage('Checkstyle Analysis'){
            steps{
                sh 'mvn checkstyle:checkstyle'
            }
        }
        stage('Integrated Testing'){
            steps{
                sh 'mvn verify -DiskipUnitTests'
            }
        }
        stage('Sonar Scanner'){
            steps{
                withSonarQubeEnv(credentialsId: 'personal-sonar', installationName: 'sonarqube') {
                    sh 'mvn sonar:sonar'
                }
            }
        }
        stage('Quality Gate'){
            steps{
                waitForQualityGate abortPipeline: false, credentialsId: 'personal-sonar'
            }
        }
        stage('Upload War Artifact'){
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
                    groupId: 'com.visualpathit', 
                    nexusUrl: '18.212.37.145:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: 'vpro-personal', 
                    version: 'v2'
            }
        }
    }
    post {
        always{
            echo 'slack notifications'
            slackSend channel: '#personaly',
            color: COLOR_MAP[currentBuild.currentResult],
            message: "*${currentBuild.currentResult}:* Job name ${env.JOB_NAME} build ${env.BUILD_NUMBER} time ${env.BUILD_TIMESTAMP} \n More info at: ${BUILD_URL}"
        }
    }
}

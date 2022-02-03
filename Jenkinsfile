pipeline{
    agent any
    tools{
        maven "Maven"
    }
    stages{
    stage('checkout'){
        steps{
            checkout scm
        }
    }
    stage('Build'){
        steps{
        sh "mvn install"
        }
    }
    stage('Unit test'){
        steps{
        sh "mvn test"
        }
    }
    stage('Sonar Analysis'){
        steps{
            withSonarQubeEnv("Test Sonar")
            {
                sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar"
            }
        }
    }
    post{
        success{
            sh "echo success"
        }
    }
}

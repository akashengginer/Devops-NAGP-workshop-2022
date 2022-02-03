pipeline{
    agent any
    tools{
        maven "Maven"
    }
    stages{
    stage("code checkout"){
        steps{
        sh "echo hello"
        }
    }
    stage("code build"){
        steps{
        sh "mvn clean"
        }
    }
    stage("Unit test"){
        steps{
        sh "echo unit"
        }
    }
    }
    post{
        success{
            sh "echo success - test commit auto run"
        }
    }
}

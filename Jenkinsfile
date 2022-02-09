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
            withSonarQubeEnv("Test_Sonar")
            {
                sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.0:sonar"
            }
        }
    }
//    stage('Upload to Artifactory'){
//        steps{
//            rtMavenDeployer{
//                 id: 'deployer',
//                 serverId: '123456789@artifactory',
//                 releaseRepo: 'akash.gupta.test',
//                 snapshotRepo: 'akash.gupta.test'
//            }
//            rtMavenRun{
//                 pom: 'pom.xml',
//                 goals: 'clean install',
//                 deployerId: 'deployer'
//            }
//            rtPublishBuildInfo{
//                 serverId: '123456789@artifactory'
//            }
//        }
//        }
    }
    post{
        success{
            sh "echo success"
        }
    }
}

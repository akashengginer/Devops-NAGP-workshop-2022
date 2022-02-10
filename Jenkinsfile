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
        tools {
            jdk "jdk17"
        }
        steps{
            withSonarQubeEnv(installationName: 'Test_Sonar')
            {
                sh 'mvn clean org.sonarsource.scanner.maven:sonar-maven-plugin:3.9.1.2184:sonar'
            }
        }
    }
    stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') {
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
    }
    stage('Creating an Artifactory Server Instance'){
            steps{
                rtMavenResolver (
                    id: 'resolver-unique-id',
                    serverId: 'artifactory-serve',
                    releaseRepo: 'nagp-libs-release',
                    snapshotRepo: 'nagp-libs-snapshot'
                    )  
                rtMavenDeployer (
                    id: 'deployer-unique-id',
                    serverId: 'artifactory-serve',
                    releaseRepo: 'nagp-libs-release',
                    snapshotRepo: 'nagp-libs-snapshot'
                    )
                }
           }
   stage('Upload to Artifactory'){
       steps{
//            rtMavenDeployer{
//                 id: 'deployer',
//                 serverId: 'artifatory-server',
//                 releaseRepo: 'nagp-libs-release',
//                 snapshotRepo: 'nagp-libs-snapshot'
//            }
           rtMavenRun(
                pom: 'pom.xml',
                goals: 'clean install',
                resolverId: 'resolver-unique-id',
                deployerId: 'deployer-unique-id'
           )
           rtPublishBuildInfo(
                serverId: 'artifactory-server'
           )
       }
       }
    }
    post{
        success{
            sh "echo success"
        }
    }
}

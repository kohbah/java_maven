pipeline {
    agent any
    tools {
        maven 'maven'
        jdk 'java'
    }

    stages {

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                    id: "jfrog",
                    url: jfrog,
                    credentialsId: jfrog
                )

                rtMavenDeployer (
                    id: "maven_deployer",
                    serverId: "jfrog",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
                )

                rtMavenResolver (
                    id: "maven_resolver",
                    serverId: "jfrog",
                    releaseRepo: "libs-release",
                    snapshotRepo: "libs-snapshot"
                )
            }
        }

        stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    pom: 'pom.xml',
                    goals: 'clean install',
                    deployerId: "maven_deployer",
                    resolverId: "maven_resolver"
                )
            }
        }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "jfrog"
                )
            }
        }
    }
}
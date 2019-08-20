pipeline {
    agent any
    stages {
	  		 
        stage ('Clone') {
            steps {
                git branch: 'master', url: "https://github.com/kns-1/jenkins_pipeline.git"
            }
        }

        stage ('Artifactory configuration') {
            steps {
                rtServer (
                     id: "ARTIFACTORY_SERVER",
    url: "http://192.168.99.100:8081/artifactory",
    // If you're using username and password:
    username: "admin",
    password: "Navya@123"
    )

                rtMavenDeployer (
                    id: "MAVEN_DEPLOYER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: "libs-release-local",
                    snapshotRepo: "libs-snapshot-local"
                )

                rtMavenResolver (
                    id: "MAVEN_RESOLVER",
                    serverId: "ARTIFACTORY_SERVER",
                    releaseRepo: "libs-release",
                    snapshotRepo: "libs-snapshot"
                )
            }
        }

     /*   stage ('Exec Maven') {
            steps {
                rtMavenRun (
                    tool: MAVEN_TOOL, // Tool name from Jenkins configuration
                    pom: 'maven-example/pom.xml',
                    goals: 'clean install',
                    deployerId: "MAVEN_DEPLOYER",
                    resolverId: "MAVEN_RESOLVER"
                )
            }
        }*/
        
        
         def root = tool name: 'Go1.8', type: 'go'
    ws("${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}/src/github.com/grugrut/golang-ci-jenkins-pipeline") {
        withEnv(["GOROOT=${root}", "GOPATH=${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}/", "PATH+GO=${root}/bin"]) {
            env.PATH="${GOPATH}/bin:$PATH"
	    stage ('preTest') {
		    steps {
            sh 'go version'
            sh 'go get -u github.com/golang/dep/...'
            sh 'dep init'
		    } 
	    }
            
            
	    stage ('Build') {
		    steps {
		sh 'git clone https://github.com/kns-1/jenkins_pipeline.git'
		sh 'cd jenkins_pipeline'
            sh 'go run hello.go'
            echo 'SUCCESSFUL BUILD of GOLANG APPLICATION'
		    }
	    }
	}
    }

        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "ARTIFACTORY_SERVER"
                )
            }
        }
    }
}

node {
    def root = tool name: 'Go1.8', type: 'go'
    ws("${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}/src/github.com/grugrut/golang-ci-jenkins-pipeline") {
        withEnv(["GOROOT=${root}", "GOPATH=${JENKINS_HOME}/jobs/${JOB_NAME}/builds/${BUILD_ID}/", "PATH+GO=${root}/bin"]) {
            env.PATH="${GOPATH}/bin:$PATH"
            
            stage 'Checkout'
        
            git url: 'https://github.com/kns-1/artifactory_jenkins.git'
        
            stage 'preTest'
            sh 'go version'
            sh 'go get -u github.com/golang/dep/...'
            sh 'dep init'
            
            
            stage 'Build'
		sh 'git clone https://github.com/kns-1/artifactory_jenkins.git'
		sh 'cd jenkins_pipeline'
            sh 'go build hello.go'
            echo 'SUCCESSFUL BUILD of GOLANG APPLICATION'
		sh 'curl -X PUT -u u:p -T hello.exe "http://localhost:8081/artifactory/libs-release-local/hello.exe"'
		echo 'ARTIFACTORY UPLOAD SUCCESS'
	    
            
            stage 'Deploy'
            // Do nothing.
		
        }
    }
}

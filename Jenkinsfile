properties([
    pipelineTriggers(
        [ pollSCM('* * * * *') ]
    )
])

pipeline {
    agent any

    tools {
        jdk 'Jenkins_Java'
    }
    
    environment {
        MODULE_PATH = "modules/two"
    }

    parameters {
        string(defaultValue: '/var/lib/jenkins/jenkins-ws', description: '', name: 'workspacePath')
        booleanParam(defaultValue: true, description: '', name: 'deployToServer')
    }

    stages {
        stage('Preparation') {
            steps {
                dir(params.workspacePath) {
                    checkout([
                        $class: 'GitSCM',
                        branches: [[name: '*/master']],
                        extensions: [[$class: 'RelativeTargetDirectory', relativeTargetDir: env.MODULE_PATH]],
                        userRemoteConfigs: [[url: 'https://github.com/PaNuMo/test-module-two']]
                    ])
                }
	        }
	    }

        stage('Build') {
            steps {
                dir(params.workspacePath) {
                    sh './gradlew clean deploy'
                }
            }
        }
        
        stage('Deploy') {
            when {
                expression {
                    return params.deployToServer;
                }
            }
            
            steps {
                sh 'cp -a /var/lib/jenkins/jenkins-ws/bundles/osgi/modules/* /home/pnunez/Documents/Liferay/liferay-ce-portal-tomcat-7.1.2-ga3-20190107144105508/liferay-ce-portal-7.1.2-ga3/osgi/modules/'
            }
        }
    } 
}


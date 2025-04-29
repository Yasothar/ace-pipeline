pipeline {
    agent any

    environment {
        // Adjust these paths to match your local ACE installation
        ACE_INSTALL_DIR = '/opt/ibm/ace-12.0.x.x'
        INTEGRATION_NODE_NAME = 'NODE01'
        INTEGRATION_SERVER_NAME = 'server1'
        BAR_FILE_NAME = 'YourIntegration.bar' // Name of your BAR file to be created
        APPLICATION_NAME = 'YourIntegrationApp' // Name of the ACE application
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-credentials', url: 'https://github.com/Yasothar/your-ace-repo.git'
            }
        }

        stage('Build BAR File') {
            steps {
                script {
                    // Assuming your message flow project is in the root
                    sh "${ACE_INSTALL_DIR}/server/bin/mqsicreatebar -data . -b ${BAR_FILE_NAME} -o ."
                }
            }
        }

        stage('Deploy to Local ACE') {
            steps {
                script {
                    sh "${ACE_INSTALL_DIR}/server/bin/mqsideploy -n ${INTEGRATION_NODE_NAME} -e ${INTEGRATION_SERVER_NAME} -a ${APPLICATION_NAME} -f ${BAR_FILE_NAME}"
                }
            }
        }
    }
}
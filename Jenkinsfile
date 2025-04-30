pipeline {
    agent any

    environment {
        // Adjust these paths to match your local ACE installation on Windows
        ACE_INSTALL_DIR = 'C:\\Program Files\\IBM\\ACE\\13.0.3.0'
        INTEGRATION_NODE_NAME = 'INODE'
        INTEGRATION_SERVER_NAME = 'IS1'
        BAR_FILE_NAME = 'SampleApp.bar' // Name of your BAR file to be created
        APPLICATION_NAME = 'SampleApp' // Name of the ACE application
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-credentials', url: 'https://github.com/Yasothar/ace-pipeline.git', branch: 'main'
            }
        }

        stage('Build BAR File on Windows') {
            steps {
                script {
                    // Execute mqsicreatebar.exe directly since Jenkins is on Windows
                    def mqsicreatebarPath = "${ACE_INSTALL_DIR}\\tools\\mqsicreatebar.exe"
                    def createBarCommand = "\"${mqsicreatebarPath}\" -data . -b ${BAR_FILE_NAME} -o ."

                    echo "Executing command: ${createBarCommand}"
                    bat createBarCommand // Use 'bat' for Windows batch commands
                }
            }
        }

        stage('Deploy to Local ACE on Windows') {
            steps {
                script {
                    // Execute mqsideploy.exe directly since Jenkins is on Windows
                    def mqsideployPath = "${ACE_INSTALL_DIR}\\server\\bin\\mqsideploy.exe"
                    def deployCommand = "\"${mqsideployPath}\" -n ${INTEGRATION_NODE_NAME} -e ${INTEGRATION_SERVER_NAME} -a ${APPLICATION_NAME} -f ${BAR_FILE_NAME}"

                    echo "Executing command: ${deployCommand}"
                    bat deployCommand // Use 'bat' for Windows batch commands
                }
            }
        }
    }
}
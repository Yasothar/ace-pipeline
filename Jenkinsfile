pipeline {
    agent any

    environment {
        // Adjust these paths to match your local ACE installation on Windows
        WINDOWS_ACE_INSTALL_DIR = 'C:\\\\Program Files\\\\IBM\\\\ACE\\\\12.0.10.0'
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
                    // Construct the Windows command to run via WSL
                    def windowsMqsicreatebarPath = "${WINDOWS_ACE_INSTALL_DIR}\\server\\bin\\mqsicreatebar.exe".replace('\\\\', '\\')
                    def createBarCommand = "wsl.exe \"${windowsMqsicreatebarPath}\" -data . -b ${BAR_FILE_NAME} -o ."

                    echo "Executing command: ${createBarCommand}"
                    sh createBarCommand
                }
            }
        }

        stage('Deploy to Local ACE on Windows') {
            steps {
                script {
                    // Construct the Windows command to run via WSL
                    def windowsMqsideployPath = "${WINDOWS_ACE_INSTALL_DIR}\\server\\bin\\mqsideploy.exe".replace('\\\\', '\\')
                    def deployCommand = "wsl.exe \"${windowsMqsideployPath}\" -n ${INTEGRATION_NODE_NAME} -e ${INTEGRATION_SERVER_NAME} -a ${APPLICATION_NAME} -f ${BAR_FILE_NAME}"

                    echo "Executing command: ${deployCommand}"
                    sh deployCommand
                }
            }
        }
    }
}
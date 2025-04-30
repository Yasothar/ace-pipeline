pipeline {
    agent any

    environment {
        // Adjust these paths to match your local ACE installation
        ACE_INSTALL_DIR = 'C:\\\\Program Files\\\\IBM\\\\ACE\\\\12.0.10.0'
        INTEGRATION_NODE_NAME = 'INODE'
        INTEGRATION_SERVER_NAME = 'IS1'
        BAR_FILE_NAME = 'SampleApp.bar' // Name of your BAR file to be created
        APPLICATION_NAME = 'SampleApp' // Name of the ACE application
    }
	
	
	def ACE_INSTALL_DIR = 'C:\\\\Program Files\\\\IBM\\\\ACE\\\\12.0.10.0'
	def WINDOWS_MQSI_CREATE_BAR = "${ACE_INSTALL_DIR}\\server\\bin\\mqsicreatebar.exe".replace('\\\\', '\\') // Use single backslashes for wslpath
    stages {
	  stage('Run Windows ACE Command from WSL') {
		steps {
        sh """
			windows_mqsi_create_bar=$(wslpath -w "${WINDOWS_MQSI_CREATE_BAR}")
			echo "Windows mqsicreatebar path for wsl.exe: $windows_mqsi_create_bar"
			# Execute the Windows command using wsl.exe, ensuring proper quoting wsl.exe "$windows_mqsi_create_bar" -data . -b SampleApp.bar -o .
			"""
			}
		}
	
        stage('Checkout') {
            steps {
                git credentialsId: 'github-credentials', url: 'https://github.com/Yasothar/ace-pipeline.git', branch: 'main'
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
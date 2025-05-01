pipeline {
    agent any

    environment {
        // Adjust these paths to match your local ACE installation on Windows
        ACE_INSTALL_DIR = 'C:\\Program Files\\IBM\\ACE\\13.0.3.0'
        INTEGRATION_NODE_NAME = 'INODE13'
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
					def mqsicreatebarPath = "C:\\Program Files\\IBM\\ACE\\13.0.3.0\\tools\\mqsicreatebar.exe"
					def APPLICATION_NAME = 'SampleApp' // Assuming your application is named SampleApp
					def PROJECT_NAME = 'D:\\work\\ws\\ws-pipeline-demo' // Replace with your actual project name
					def BAR_FILE_NAME = "${APPLICATION_NAME}.bar"

					//def createBarCommand = "\"${mqsicreatebarPath}\" -data a. -b \"${BAR_FILE_NAME}\" -o . -p \"${PROJECT_NAME}\""
					
					def createBarCommand = "\"${mqsicreatebarPath}\" -data  \"${PROJECT_NAME}\" -b \"${BAR_FILE_NAME}\" -a \"${APPLICATION_NAME}\""
					
			
					
					echo "Executing command: ${createBarCommand}"
					bat "${createBarCommand}"
				}
			}
		} 
        

        stage('Deploy to Local ACE on Windows') {
            steps {
                script {
                    // Execute mqsideploy.exe directly since Jenkins is on Windows
                    def mqsideployPath = "${ACE_INSTALL_DIR}\\server\\bin\\mqsideploy.exe"
					//def deployCommand = "\"${mqsideployPath}\" -n ${INTEGRATION_NODE_NAME} -e ${INTEGRATION_SERVER_NAME} -a ${APPLICATION_NAME} \"${BAR_FILE_NAME}\""
                    
					def deployCommand = "\"C:\\Program Files\\IBM\ACE\\13.0.3.0\\server\\bin\\mqsideploy.exe" INODE13 -e IS1 -a SampleApp.bar\"
					echo "Executing command: ${deployCommand}"
                    bat deployCommand // Use 'bat' for Windows batch commands
                }
            }
        }
    }
}
pipeline {
    agent any

    environment {
        // Adjust this path to match your local ACE installation on Windows
        ACE_INSTALL_DIR = 'C:\\Program Files\\IBM\ACE\\12.0.10.0'
        INTEGRATION_NODE_NAME = 'INODE' // Assuming your node name is the same
        INTEGRATION_SERVER_NAME = 'IS1' // Assuming your server name is the same
        BAR_FILE_NAME = 'SampleApp.bar'
        APPLICATION_NAME_PREFIX = 'SampleApp'
    }

    stages {
        stage('Checkout') {
            steps {
                git credentialsId: 'github-credentials', url: 'https://github.com/Yasothar/ace-pipeline.git', branch: env.BRANCH_NAME
            }
        }

        stage('Build BAR File') {
            steps {
                script {
                    // Assuming your message flow project is in the root
                    // Use double backslashes for paths in Windows
                    bat "\"${ACE_INSTALL_DIR}\\server\\bin\\mqsicreatebar\" -data . -b ${BAR_FILE_NAME} -o ."
                }
            }
        }

        stage('Deploy') {
            steps {
                script {
                    def environment
                    def integrationServer

                    if (env.BRANCH_NAME == 'uat') {
                        environment = 'UAT'
                        integrationServer = 'uat_server' // Your UAT integration server name
                    } else if (env.BRANCH_NAME == 'prod') {
                        environment = 'Prod'
                        integrationServer = 'prod_server' // Your Production integration server name
                    } else {
                        error "Unsupported branch: ${env.BRANCH_NAME}. Only 'uat' and 'prod' are allowed for deployment."
                    }

                    def applicationName = "${APPLICATION_NAME_PREFIX}_${environment}"

                    // Use double backslashes for paths in Windows
                    bat "\"${ACE_INSTALL_DIR}\\server\\bin\\mqsideploy\" -n ${INTEGRATION_NODE_NAME} -e ${integrationServer} -a ${applicationName} -f ${BAR_FILE_NAME}"
                    echo "Deployed to ${environment} environment."
                }
            }
        }

        // Trigger conditions based on branch (optional)
        triggers {
            branchFilters(['uat', 'prod'])
        }
    }
}
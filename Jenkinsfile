pipeline {
    agent any

    parameters {
        string(name: 'ACE_VERSION', defaultValue: '12.0.12.0', description: 'IBM ACE version')
        string(name: 'NODE_NAME', defaultValue: 'LocalNode', description: 'Local ACE Node name')
        string(name: 'SERVER_NAME', defaultValue: 'LocalServer', description: 'Local ACE Server name')

        string(name: 'REMOTE_NODE_NAME', defaultValue: 'node3', description: 'Remote ACE Node name')
        string(name: 'REMOTE_SERVER_NAME', defaultValue: 'Server_1', description: 'Remote ACE Server name')
        string(name: 'REMOTE_HOST', defaultValue: '192.168.3.165', description: 'Remote host IP or DNS name')
        string(name: 'REMOTE_PORT', defaultValue: '7802', description: 'Remote admin listener port')
    }

    environment {
        REPO_URL = 'https://github.com/Hrishikesh1200/IIB_Jenkins.git'
        BRANCH = 'master'
        WORK_DIR = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\IIB_job'
        WORKSPACE_ROOT = "${env.WORKSPACE}"
        BAR_OUTPUT_DIR = "${WORKSPACE_ROOT}\\bars"

        ACE_HOME       = "C:\\Program Files\\IBM\\ACE\\${params.ACE_VERSION}"
        CREATEBAR_EXE  = "${ACE_HOME}\\tools\\mqsicreatebar.exe"
        DEPLOY_EXE     = "${ACE_HOME}\\server\\bin\\mqsideploy.exe"
        MQSIPROFILE    = "${ACE_HOME}\\server\\bin\\mqsiprofile.cmd"
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning the repository from ${REPO_URL}..."
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
        }

        stage('Create BAR File') {
            steps {
                bat """
                    CALL "${MQSIPROFILE}"
                    IF NOT EXIST "${BAR_OUTPUT_DIR}" mkdir "${BAR_OUTPUT_DIR}"
                    "${CREATEBAR_EXE}" ^
                        -data "${WORKSPACE_ROOT}" ^
                        -b "${BAR_OUTPUT_DIR}\\TestJenkin.bar" ^
                        -a "TestJenkin" ^
                        -p "TestJenkin"
                """
            }
        }

        stage('Parallel Deployment') {
            parallel {
                stage('Deploy to Local Node') {
                    steps {
                        bat """
                            CALL "${MQSIPROFILE}"
                            "${DEPLOY_EXE}" ^
                                "${params.NODE_NAME}" ^
                                -e "${params.SERVER_NAME}" ^
                                -a "${BAR_OUTPUT_DIR}\\TestJenkin.bar"
                        """
                    }
                }

                stage('Deploy to Remote Node') {
                    steps {
                        bat """
                            CALL "${MQSIPROFILE}"
                            "${DEPLOY_EXE}" ^
                                "${params.REMOTE_NODE_NAME}" ^
                                -i "${params.REMOTE_HOST}" ^
                                -q "${params.REMOTE_PORT}" ^
                                -e "${params.REMOTE_SERVER_NAME}" ^
                                -a "${BAR_OUTPUT_DIR}\\TestJenkin.bar"
                        """
                    }
                }
            }
        }
    }
}

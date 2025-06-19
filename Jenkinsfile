pipeline {
    agent any

    parameters {
        string(name: 'ACE_VERSION', defaultValue: '12.0.12.0', description: 'IBM ACE version')
    }

    environment {
        REPO_URL = 'https://github.com/Hrishikesh1200/IIB_Jenkins.git'
        BRANCH = 'master'
        WORK_DIR = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\IIB_job\\IIB_Jenkins'
        WORKSPACE_ROOT = "${env.WORKSPACE}"
        BAR_OUTPUT_DIR = "${WORKSPACE_ROOT}\\bars"

        ACE_HOME       = "C:\\Program Files\\IBM\\ACE\\${params.ACE_VERSION}"
        CREATEBAR_EXE  = "${ACE_HOME}\\tools\\mqsicreatebar.exe"
        DEPLOY_EXE     = "${ACE_HOME}\\server\\bin\\mqsideploy.exe"
        MQSIPROFILE    = "${ACE_HOME}\\server\\bin\\mqsiprofile.cmd"
        OSSL_MODULES   = "${ACE_HOME}\\server\\lib\\ossl-modules"
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
                    "${CREATEBAR_EXE}" ^
                      -data "${WORK_DIR}" ^
                      -b "${BAR_OUTPUT_DIR}\\TestJenkin.bar" ^
                      -a "${WORK_DIR}\\TestJenkin" ^
                      -p "TestJenkin"
                """
            }
        }
    }
}

pipeline {
    agent any

    environment {
        // Set your GitHub repo URL
        REPO_URL = 'https://github.com/Hrishikesh1200/IIB_Jenkins.git'
        BRANCH = 'master'
        WORK_DIR = 'C:\\ProgramData\\Jenkins\\.jenkins\\workspace\\IIB_job\\IIB_Jenkins'
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
                    "C:\\Program Files\\IBM\\ACE\\12.0.12.0\\tools\\mqsicreatebar.exe" -data ${WORK_DIR} -b jenkinsApplication.bar -a ${WORK_DIR}\\jenkinsApplication
                """
            }
        }
    }
}

pipeline {
    agent any

    environment {
        // Set your GitHub repo URL
        REPO_URL = 'https://github.com/Hrishikesh1200/IIB_Jenkins.git'
        BRANCH = 'main'
    }

    stages {
        stage('Clone Repository') {
            steps {
                echo "Cloning the repository from ${REPO_URL}..."
                git branch: "${BRANCH}", url: "${REPO_URL}"
            }
  }

        stage('create bar') {
            steps {
               bat """
"C:\Program Files\IBM\ACE\12.0.12.0\tools\mqsicreatebar.exe" -data IIB_Jenkins -b jenkinsApplication.bar -a jenkinsApplication
"""
            }
  }




}
}

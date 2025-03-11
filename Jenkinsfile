pipeline {
    agent any  // Use any available agent to run the pipeline

    environment {
        INSTALLER_PATH = "C:\\Path\\To\\VS2008\\setup.exe"  // Path to the VS2008 installer
        INSTALL_LOG = "C:\\VS2008_Install.log"  // Path to the installation log
    }

    stages {
        stage('Prepare') {
            steps {
                script {
                    // Run PowerShell script to install Visual Studio 2008
                    def installScript = """
                    if (-Not (Test-Path ${INSTALLER_PATH})) {
                        Write-Host "ERROR: Visual Studio 2008 installer not found."
                        exit 1
                    }

                    $installArgs = "/q /norestart /log ${INSTALL_LOG}"
                    Write-Host "Starting Visual Studio 2008 installation..."
                    Start-Process -FilePath ${INSTALLER_PATH} -ArgumentList $installArgs -NoNewWindow -Wait

                    if (\$?) {  // Escape $?
                        Write-Host "Visual Studio 2008 installed successfully."
                    } else {
                        Write-Host "ERROR: Visual Studio 2008 installation failed. Check the log."
                        exit 1
                    }
                    """
                    // Execute the script on the Windows agent
                    bat """powershell -Command ${installScript}"""
                }
            }
        }

        stage('Build') {
            steps {
                echo 'Building the project with Visual Studio 2008...'
                // Add your build steps here, e.g., MSBuild
                bat 'MSBuild YourProject.sln /p:Configuration=Release'
            }
        }
    }

    post {
        always {
            // Clean-up or notifications
            echo 'Pipeline completed.'
        }
    }
}
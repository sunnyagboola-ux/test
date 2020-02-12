pipeline {
  agent {
    node {
      label 'master'
      customWorkspace 'C:\\Jenkins\\workspace\\SDR\\SDR_V1.0\\NodeAPiPipeline'
    }
  }
    stages {
        
                stage('Preparation') { // for display purposes
                    steps {
                         git(
                              url: 'git@github.com:emersonprocess/FIELD-SERVICE-API.git',
                              credentialsId: '9796d75e-de18-4c12-b5c1-b83cd5',
                              branch: "${gitBranch}"
                             )
                        }
                                    }
                stage('Build') {
                    steps {
                         echo "Git checkout has been completed"
                         echo 'workspace location `$WORKSPACE` ${pwd} $ENV:WORKSPACE'
                        }
                         }
                stage('Get Node modules') {
                    steps {   
                        echo "Installation of node modules1"
                        bat 'cd .\\FieldServiceAPINode && npm install'
                        echo "Node modules have been added Successfully"
                        }
                                         }
                stage('Code analysis') {
                    steps {
                        echo "Code analysis"
                        writeFile file: "sonar-project.properties", text: """
                                         sonar.projectKey=emerson:nodejs:fieldServiceApi
                                         sonar.projectName=fieldServiceNodeApi
                                         sonar.projectVersion=1.0
                                         sonar.sources=FieldServiceAPINode
                                         """
                        script {
                            sonarScanner = tool 'SonarScanner';
                           
                        }
                        withSonarQubeEnv('SonarQube') {
                            bat "${sonarScanner}\\bin\\sonar-scanner.bat -X"
                             }
                    }
                }
                        
                stage('Creating Archive') {
                    steps {
                        bat '''set src=.\\FieldServiceAPINode
                        set Dest=.\\ApiNodeArchive\\EmersonDbcsApi-v1.0.zip
                        
                        IF EXIST %Dest% (
                            del %Dest%
                        ) ELSE (
                            echo file missing.
                        )
                        "C:\\Program Files\\7-Zip\\7z.exe" a .\\ApiNodeArchive\\EmersonApi.zip .\\FieldServiceAPINode\\*'''
                        }
                                        }

                stage('Upload Archives to ObjectStorage') {
                    steps {
                        bat 'echo "Git checkout has been completed"'
                        bat 'echo "Git checkout has been completed"'
                        bat 'C:\\curl-7.61.0-win64-mingw\\bin\\curl.exe -i -X PUT -H "X-Auth-Token: AUTH_tk48d17783bf273befd8817ec62be9992d" https://a472144.us.storage.oraclecloud.com/v1/Storage-a472144/%containerName%/EmersonDbcsApi-v1.zip -T .\\ApiNodeArchive\\EmersonApi.zip'
                        }
                                                        }
                                                        
                stage('Update Archives to AccS Instance') {
                    steps {
                        bat 'echo "Git checkout has been completed"'
                        bat 'C:\\curl-7.61.0-win64-mingw\\bin\\curl.exe -X PUT -u gopi.krishna@sofbang.com:PASSWORD -H "X-ID-TENANT-NAME:a472144" -H "Content-Type: multipart/form-data"   -F "manifest=@C:\\Jenkins\\workspace\\SDR\\SDR_V1.0\\NodeAPiPipeline\\manifest.json" -F "archiveURL=%containerName%/EmersonDbcsApi-v1.zip" -F "notes=Emerson Node Api Deployment Instance" https://apaas.us.oraclecloud.com/paas/service/apaas/api/v1.1/apps/a472144/%AccSInstanceName%'
                        }
                                                        }  
                    }
            
    
}

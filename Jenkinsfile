pipeline {
    agent {
        label 'git_automation'
    }

    stages {
        
        stage('Checkout'){
            steps{
                cleanWs notFailBuild: true, patterns: [[pattern: '_reports/*.*', type: 'INCLUDE']]
                git branch: 'SysTest', credentialsId: '13389991-af70-45bd-96d0-802fcfc10fb0', url: 'https://github.geo.conti.de/CF-IN-O-OE-Efficiency/Automation-API-Structured.git'
                checkout([$class: 'GitSCM', branches: [[name: '*/SysTest']], extensions: [], userRemoteConfigs: [[credentialsId: '13389991-af70-45bd-96d0-802fcfc10fb0', url: 'https://github.geo.conti.de/CF-IN-O-OE-Efficiency/Automation-API-Structured.git']]])
                }
               }
        stage('Compile Check'){
            steps{
            bat '''set pythonPath=C:\\Users\\uic79079\\AppData\\Local\\Programs\\Python\\Python39
            d:
            cd "D:\\Jenkins\\workspace\\Automation-API-Structured-SysTest\\API Resources\\LambdaLayer"
            C:\\Users\\uic79079\\AppData\\Local\\Programs\\Python\\Python39\\python.exe -m py_compile  BMC_wrapper.py'''
            }      
        }
        
        stage('Syntax Check'){
            steps{
                 bat '''set pythonPath=C:\\Users\\uic79079\\AppData\\Local\\Programs\\Python\\Python39
                 %pythonPath%\\python.exe -m pip install --proxy=http://cias3basic.conti.de:8080/wpad.dat pylint
                 C:\\Users\\uic79079\\AppData\\Local\\Programs\\Python\\Python39\\python.exe -m pip install --upgrade pip
                 d:
                 cd "D:\\Jenkins\\workspace\\Automation-API-Structured-SysTest\\API Resources\\LambdaLayer"
                 C:\\Users\\uic79079\\AppData\\Local\\Programs\\Python\\Python39\\Scripts\\pylint --rcfile .pylintrc BMC_wrapper.py --exit-zero'''
            }
        }
        
                //Unit Test
                
        stage('E2E Tests'){
            steps{
                withCredentials([usernamePassword(credentialsId: '13389991-af70-45bd-96d0-802fcfc10fb0', passwordVariable: 'password', usernameVariable: 'user')]) {
                    bat """set pythonPath=C:\\Users\\uic79079\\AppData\\Local\\Programs\\Python\\Python39
                    %pythonPath%\\python.exe -m pip install --proxy=http://cias3basic.conti.de:8080/wpad.dat behave 
                    d:
                    cd "D:\\Jenkins\\workspace\\Automation-API-Structured-SysTest\\Testing\\BDD\\Test Automation Execution"
                    C:\\Users\\uic79079\\AppData\\Local\\Programs\\Python\\Python39\\Scripts\\behave.exe --tags=test1 --junit -D username=${user} -D password=${password}
                    """
                }
            }
            }
         stage('Github Pull Request'){
            steps{
                    powershell "Set-Location -Path 'D:\\GitRepos\\'"
                    powershell "git checkout -b SysTest"
                   // powershell "gh pr create --base QA--head SysTest --title "Deploying to QA" --reviewer uic75096"
                    }
               }
            
                
               
            }
        }

    

        
  

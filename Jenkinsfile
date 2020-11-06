pipeline {

    agent any

    //установить переменные environment
    environment {
        NEW_VERSION = '1.0.0'
        //используем credentials из Jenkins server (с помощью плагина Credentials Building)
        /*
        SERVER_CREDENTIALS = credentials('server-credentials')
        //server-credentials - ID credentials устанавливаем в настройках Jenkins
        */
    }


/*
    //дополнтельные инструменты
    tools {
        maven 'Maven' //используем наименованиие Maven из GlobalTools Jenkins
    }
*/

    //Запуск билд процессов с выбираемыми параметрами
    parameters {
        choice(name: 'VERSION', choices: ['DEBUG', 'RC', 'RELEASE'], description: 'choice build type')
        booleanParam(name: 'executeTests', defaultValue: true, description: 'test my app')
    }


    stages {
        stage("Introduction") {
            steps {
                echo 'Hello Jenkins!'
                echo "Building version ${NEW_VERSION}" //используем Environment
            }
        }

        //Этот раздел будет выполняться только на DEV ИЛИ на MASTER ветке
        stage("For dev OR master ranch") {
            when {
                expression {
                    BRANCH_NAME == 'dev' || BRANCH_NAME == 'master'
                }
            }
            steps {
                echo 'Run code on dev OR master branch'
            }
        }


        /*
        //Этот раздел будет выполняться только если на ветке обновится код
        stage("New code on master ranch") {
            when {
                expression {
                    BRANCH_NAME == 'master' && CODE_CHANGES == true
                }
            }
            steps {
                echo 'Run code on DEV or MASTER branch'
            }
        }
        */

        stage("Testing") {
            when {
                expression {
                    params.executeTests
                }
            }
            steps {
                echo 'Testing...'
            }
        }

        stage("Deploying") {
            steps {
                echo 'Deploy me'
                echo "with ${SERVER_CREDENTIALS}" //используем переменную environment
            }
        }

    }


/*
    //действия после выполнения билда
    post {
        always {
            //выполняются всегда
        }
        failure {
            //выполняются при НЕ успешной сборке
        }
        success {
            //выполняются успешной сборке
        }
    }
*/

}


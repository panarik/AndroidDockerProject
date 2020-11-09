node {


    docker.image('androidsdk/android-24:latest').inside('-u root') {
        stage('Install environment') {

            echo 'Устанавливаем Android platform 24'
            sh 'sdkmanager "platforms;android-24"'

            echo 'Список установленных пакетов'
            sh 'sdkmanager --list'




        }
    }
}



node {


    docker.image('androidsdk/android-24:latest').inside('-u root') {
        stage('Install environment') {

            echo 'Устанавливаем Android platform 22'
            sh 'sdkmanager "platforms;android-22"'

            echo 'Устанавливаем Android platform 24'
            sh 'sdkmanager "platforms;android-24"'

            echo 'Устанавливаем SDK source Android-22'
            sh 'sdkmanager "sources;android-22"'

            echo 'Устанавливаем android-image'
            sh 'sdkmanager "system-images;android-22;google_apis;x86_64"'

            echo 'Список установленных пакетов'
            sh 'sdkmanager --list'
        }

        stage('Run environment') {

            //Дебаг
            sh 'sleep 36000' //ждем 600 минут

            echo 'Создаем эмулятор'
            sh 'echo no | avdmanager create avd -n Android-22_google_apis_x86_64 -k "system-images;android-22;google_apis;x86_64"'

            echo 'Проверяем, если в списке эмулятор'
            sh 'emulator -list-avds'

            echo 'Дебаг - запускаем adb server'
            sh 'adb -d start-server'

            echo 'запускаем созданный эмулятор'
            sh 'emulator -avd Android-22_google_apis_x86_64 -no-audio -no-window -gpu swiftshader_indirect -verbose -accel off&' //работает без hardware на локале (Win64)

            sh 'sleep 360' //ждем 6 минут пока запустится

            echo 'NETSTAT:'
            sh 'apt -y install net-tools'
            sh 'netstat -lnp'

            echo 'Проверяем, запущен ли эмулятор'
            sh 'adb devices'

        }


    }
}



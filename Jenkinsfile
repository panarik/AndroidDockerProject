node {

    checkout scm

    docker.with

    stage('Build and upload') {

        docker.image('androidsdk/android-24:latest').inside('-u root') //Ссылка на образ: https://hub.docker.com/r/androidsdk/android-30
                {
                    stage('Install environment') {

                        echo 'Устанавливаем Android platform 24'
                        sh 'sdkmanager "platforms;android-24"'

                        echo 'Устанавливаем Android platform 22'
                        sh 'sdkmanager "platforms;android-22"'

                        echo 'Устанавливаем SDK source Android-24'
                        sh 'sdkmanager "sources;android-24"'

                        echo 'Устанавливаем SDK source Android-22'
                        sh 'sdkmanager "sources;android-22"'

                        echo 'Устанавливаем android-image'
                        sh 'sdkmanager "system-images;android-22;google_apis;x86_64"'
                        // вариант 1: sh 'sdkmanager "system-images;android-24;google_apis;arm64-v8a"'
                        // вариант 2: sh 'sdkmanager "system-images;android-24;google_apis_playstore;x86"'
                        // вариант 3: sh 'sdkmanager "system-images;android-24;google_apis;x86_64"' // на CI - No compatible devices connected
                        // вариант 4: sh 'sdkmanager "system-images;android-22;google_apis;x86_64"'

                        echo 'Список установленных пакетов'
                        sh 'sdkmanager --list'
                    }

                    stage('Run environment') {

                        echo 'Создаем эмулятор '
                        sh 'echo no | avdmanager create avd -n Android-22_google_apis_x86_64 -k "system-images;android-22;google_apis;x86_64"'
                        //вариант 1: sh 'echo no | avdmanager create avd -n Android-24_Google_apis_arm64-v8a -k "system-images;android-24;google_apis;arm64-v8a"'
                        //вариант 2: sh 'echo no | avdmanager create avd -n Android-24_google_apis_playstore_x86 -k "system-images;android-24;google_apis_playstore;x86"'
                        //вариант 3: sh 'echo no | avdmanager create avd -n Android-24_google_apis_x86_64 -k "system-images;android-24;google_apis;x86_64"'
                        //вариант 4: sh 'echo no | avdmanager create avd -n Android-22_google_apis_x86_64 -k "system-images;android-22;google_apis;x86_64"'

                        echo 'Проверяем, если в списке эмулятор'
                        sh 'emulator -list-avds'

                        //Дебаг - запускаем adb server
                        sh 'adb -d start-server'

                        echo 'запускаем созданный эмулятор'
                        sh 'emulator -avd Android-22_google_apis_x86_64 -debug all -no-audio -no-window -gpu swiftshader_indirect -verbose -accel off&' //работает без hardware на локале (Win64)
                        //вариант 1: sh 'emulator -avd Android-24_Google_apis_arm64-v8a -noaudio -no-window -gpu off -verbose -accel off&'
                        //вариант 2: sh 'emulator -avd Android-24_google_apis_playstore_x86 -no-window -gpu off -verbose -accel off&' //x86 не запускается без hardware
                        //вариант 3: sh 'emulator -avd Android-24_google_apis_x86_64 -no-window -verbose -accel off&' //работает без hardware на локале (Win64)
                        //вариант 4: sh 'emulator -avd Android-22_google_apis_x86_64 -no-window -verbose -accel off&' //работает без hardware на локале (Win64)
                        //вариант 5: sh 'emulator -avd Android-22_google_apis_x86_64 -debug all -no-audio -no-window -gpu swiftshader_indirect -verbose -accel off&'

                        sh 'sleep 360' //ждем 6 минут пока запустится

                        echo 'NETSTAT: '
                        sh 'apt -y install net-tools'
                        sh 'netstat -lnp'

                        echo 'Проверяем, запущен ли эмулятор'
                        sh 'adb devices'
                    }


                    stage('Build & Install app') {
                        echo 'Ставим APK'
                        sh 'chmod +x gradlew && ./gradlew --no-daemon --stacktrace clean :app:assembleDebug :app:assembleAndroidTest'
                    }


                    stage('Run tests') {
                        echo 'Запускаем тесты'
                        sh 'sh gradlew connectedAndroidTest --no-daemon --stacktrace -i'
                    }

                }

        //APK дебажной сборки
        archiveArtifacts 'app/build/outputs/apk/debug/*.apk'
        //HTML отчет по результатам тестов
        archiveArtifacts 'app/build/reports/androidTests/connected/*'

        //APK релизной сборки
        //archiveArtifacts 'app/build/outputs/apk/production/release/*.apk'
    }


}


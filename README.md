# blip-chat-android
 
## Criando um projeto utilizando o SDK Blip Chat Android

Crie um projeto utilizando como SDK mínimo API 23 do Android. Conforme Imagem 1 abaixo:

![alt text](https://github.com/fourtime/blip-chat-android/blob/main/imgs/1_-_CriandoProjeto.png)
1 - Criando projeto

Faça Download do arquivo zipado do projeto conforme Imagem 2 a seguir. Utilize o link abaixo para download: https://github.com/fourtime/blip-chat-android

![alt text](https://github.com/fourtime/blip-chat-android/blob/main/imgs/2_-_BaixandoArquivos.png)
2 - Baixando Arquivos

Descompacte o arquivo blip-chat-android-main.zip e copie as pastas descompactadas para dentro do seu projeto. A pasta flutter_blip_chat_sdk deve ficar na pasta root do seu projeto e a pasta android_blip_chat_sdk na pasta app do seu projeto. Conforme Imagem 3 abaixo:

![alt text](https://github.com/fourtime/blip-chat-android/blob/main/imgs/3_-_EstruturaDePastas.png)<br/>
3 - Estrutura de Pastas do Projeto

Volte para seu projeto no Android Studio no arquivo settings.gradle e adicione as seguintes linhas na propriedade repositories do seu arquivo, conforme abaixo:

```
    repositories {
        google()
        mavenCentral()
        maven { url 'flutter_blip_chat_sdk/repo' }
        maven { url 'https://storage.googleapis.com/download.flutter.io' }
    }
```

Altere o repositoriesMode para PREFER_SETTINGS conforme abaixo:

```
repositoriesMode.set(RepositoriesMode.PREFER_SETTINGS)
```

Ainda no arquivo settings.gradle, adicione uma última linha a referência para o projeto:

```
include ':app:android_blip_chat_sdk'
```

Arquivo gradle completo do projeto de teste:

```
pluginManagement {
    repositories {
        gradlePluginPortal()
        google()
        mavenCentral()
    }
}
dependencyResolutionManagement {
    repositoriesMode.set(RepositoriesMode.PREFER_SETTINGS)
    repositories {
        google()
        mavenCentral()
        maven { url 'flutter_blip_chat_sdk/repo' }
        maven { url 'https://storage.googleapis.com/download.flutter.io' }
    }
}
rootProject.name = "TesteSDK"
include ':app'
include ':app:android_blip_chat_sdk'
```

No arquivo build.gradle do seu app adicione o trecho abaixo acima da propriedade dependences:

```
String storageUrl = System.env.FLUTTER_STORAGE_BASE_URL ?: "https://storage.googleapis.com"
repositories {
    maven { url "$projectDir/flutter_blip_chat_sdk/repo" }
    maven { url "$storageUrl/download.flutter.io" }
}

```

Dentro da propriedade buildTypes adicione o seguinte trecho: 

```
        profile {
            initWith debug
        }
```

Dentro da propriedade dependences adicione a referência do SDK conforme abaixo:

```
    implementation project(path: ':app:android_blip_chat_sdk')

    debugImplementation  'blip.sdk.chat:flutter_debug:1.0'
    profileImplementation 'blip.sdk.chat:flutter_profile:1.0'
    releaseImplementation 'blip.sdk.chat:flutter_release:1.0'
```

Após essas configurações clique em <b>Sync Now</b>

Abaixo arquivo build.gradle completo após configurações:

```
plugins {
    id 'com.android.application'
    id 'org.jetbrains.kotlin.android'
}

android {
    namespace 'net.take.testesdk'
    compileSdk 32

    defaultConfig {
        applicationId "net.take.testesdk"
        minSdk 23
        targetSdk 32
        versionCode 1
        versionName "1.0"

        testInstrumentationRunner "androidx.test.runner.AndroidJUnitRunner"
    }

    buildTypes {
        profile {
            initWith debug
        }
        release {
            minifyEnabled false
            proguardFiles getDefaultProguardFile('proguard-android-optimize.txt'), 'proguard-rules.pro'
        }
    }
    compileOptions {
        sourceCompatibility JavaVersion.VERSION_1_8
        targetCompatibility JavaVersion.VERSION_1_8
    }
    kotlinOptions {
        jvmTarget = '1.8'
    }
}

String storageUrl = System.env.FLUTTER_STORAGE_BASE_URL ?: "https://storage.googleapis.com"
repositories {
    maven { url "$projectDir/flutter_blip_chat_sdk/repo" }
    maven { url "$storageUrl/download.flutter.io" }
}

dependencies {

    implementation project(path: ':app:android_blip_chat_sdk')

    debugImplementation  'blip.sdk.chat:flutter_debug:1.0'
    profileImplementation 'blip.sdk.chat:flutter_profile:1.0'
    releaseImplementation 'blip.sdk.chat:flutter_release:1.0'

    implementation 'androidx.core:core-ktx:1.7.0'
    implementation 'androidx.appcompat:appcompat:1.4.1'
    implementation 'com.google.android.material:material:1.5.0'
    implementation 'androidx.constraintlayout:constraintlayout:2.1.3'
    testImplementation 'junit:junit:4.13.2'
    androidTestImplementation 'androidx.test.ext:junit:1.1.3'
    androidTestImplementation 'androidx.test.espresso:espresso-core:3.4.0'
}
```

Adicione a referência da BlipChatActivity no seu arquivo Manifest.xml conforme trecho abaixo:

```xml
        <activity
            android:name="blip.chat.sdk.android.BlipChatActivity"
            android:configChanges="orientation|keyboardHidden|keyboard|screenSize|locale|layoutDirection|fontScale|screenLayout|density|uiMode"
            android:hardwareAccelerated="true"
            android:theme="@style/Theme.AppCompat.DayNight.NoActionBar"
            android:windowSoftInputMode="adjustResize" />
```

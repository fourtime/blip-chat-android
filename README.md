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

Adicione a seguinte configuração da chamada do SDK na sua Activity conforme trecho kotlin abaixo:

```kotlin
        var button = findViewById<Button>(R.id.button)

        var model = BlipChatModel()
        model.key = "{YOUR_OWNER_KEY}"
        model.type = TYPE.PLAIN
        model.useMtls = true
        var account = BlipChatAccountModel()
        account.fullName = "{LOGGED_USER_FULL_NAME}"
        account.photoUri = "{LOGGED_USER_AVATAR_URI}"
        account.pushToken = "{DEVICE_PUSH_NOTIFICATION_TOKEN}"
        model.account = account
        var style = BlipChatStyleModel()
        style.primary = "c9c8cc"
        style.sentBubble = "a442f5"
        style.receivedBubble = "706f73"
        style.background = "7b42f5"
        style.showOwnerAvatar = true
        style.overrideOwnerColors = true
        style.showUserAvatar = true
        model.style = style

        var blip = BlipChat(this, model)

        button.setOnClickListener {
            startActivity(
                BlipChatActivity.withEngineDefault().build(blip, this)
            )
        }
```

## Propriedades

| Propriedade |	Requerido |	Descrição |	Tipo |
| :--- | :---: | :--- | :---: | 
| key | YES | String key that identifies the owner | String |
| type | YES | Indicates the authentication type, used to sign in the user | AuthenticationTypeEnum |
| token | NO* | User token used to identity the application logged user. Used with external type only | String |
| issuer | NO* | Issuer used to authenticate session. Used with external type only | String |
| hostName | NO | Custom hostName used to connect to Blip Server | String |
| useMtls | NO | Determines if the connection will use mTLS for avoid MITM attacks. Default: false | bool |
| account | NO | Logged user account data | BlipChatAccountModel |
| style | NO | Custom chat style | BlipChatStyleModel |

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

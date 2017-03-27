## Android Setup

Para começar, voce precisará instalar o Node.js (v4 ou superior). Apenas
siga as instruções [instructions for your flavor of linux](https://github.com/joyent/node/wiki/Installing-Node.js-via-package-manager).

Depois de instalar o Node.js, instale o [Android SDK](http://developer.android.com/sdk/index.html).
Voce precisará executar o `android tools` (incluída no SDK, no folder /tools)

Execute o `android tools` e use para instalar API 17 ou superior.

(Se quiser executar apartir do código fonte, irá precisar do [Apache Ant](http://ant.apache.org/) para 'buildar' o bootstrap jar que o appium usa para executar nos dispositivos/simuladores de Android)

Finalmente, 'setar' o `$ANDROID_HOME` para ser o caminho default do SDK do android. Se voce descompactar o
Android SDK no local /usr/local/adt/, por exemplo, voce deve adicionar isso ao seu
shell startup:

    export ANDROID_HOME="/usr/local/adt/sdk"

Agora está configurado para rodar Appium. (Se estiver executando apartir do código fonte, certifique-se de executar `npm install` do seu Appium checkout para instalar todas as
dependencias)

### Setup adicional para versões mais antigas de Android

Appium usa e vem prepackaged com um projeto chamado [Selendroid](https://selendroid.io) para rodar versões de 
Android 2.3 a 4.1.  O Appium muda automáticamente para o Selendroid quando
detecta versões mais antigas, mas há configuração adicional necessária se 
estiver usando o código fonte.

* Assegure-se que tenha o [Maven 3.1.1](http://maven.apache.org/download.cgi) ou
  mais atual instalado (`mvn`).

### Running Appium Android Tests

To run tests on Linux, you will need to have the Android Emulator booted and
running an AVD with API Level 17 or greater. Then run Appium (`appium`) after
installing via NPM, or `node .` in the source directory if running from source.

See the [server documentation](/docs/en/writing-running-appium/server-args.md) for all the command line arguments.

### Notes

* There exists a hardware accelerated emulator for android, it has its own
  limitations. For more information you can check out this
  [page](/docs/en/appium-setup/android-hax-emulator.md).
* Make sure that `hw.battery=yes` in your AVD's `config.ini`, if you want to
  run any of the Appium tests, or use any of the power commands. As of Android 5.0, this is the default.
* Selendroid requires the following permission for instrumenting your app:
  `<uses-permission android:name="android.**permission.INTERNET"/>`,
  please make sure your app has internet permission set when you are using selendroid or older versions of Android i.e. 2.3 to 4.1

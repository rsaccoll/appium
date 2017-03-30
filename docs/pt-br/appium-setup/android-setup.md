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

Para executar os testes no Linux, voce precisará ter o emulador inicializado e
executando um AVD com API Level 17 ou superior. Então execute Appium (`appium`) depois
de instalado via NPM, ou `node .` no diretório do código fonte se estiver executando apartir do código fonte.

Consulte o [server documentation](/docs/en/writing-running-appium/server-args.md) para todos os argumentos por linha de comando.

### Notas

* Existe um 'hardware accelerated emulator for android', que tem suas
  limitações. Para mais informações, pode consultar aqui
  [page](/docs/en/appium-setup/android-hax-emulator.md).
* Certifique-se que `hw.battery=yes` no seu AVD's `config.ini`, se quiser
  rodar qualquer teste do appium, ou usar qualquer comando relacionados a energia. Para Android 5.0 já é padrão.
* Selendroid requer a seguinte permissão para instrumentar o seu app:
  `<uses-permission android:name="android.**permission.INTERNET"/>`,
  certifique-se que o seu app tenha permissão de internet definida quando estiver usando o selendroid ou versões anteriores do Android (ex: 2.3 a 4.0).

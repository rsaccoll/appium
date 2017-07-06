#Windows Setup

Appium no Windows suporta automação de apps para android e windows!

Veja [Windows App Testing](/docs/en/writing-running-appium/windows-app-testing.md) para mais detalhes.

## Running Appium on Windows

## Setup

Para começar:

   1. Baixe o ultimo [node and npm tools](https://nodejs.org/download/release/v6.3.0/node-v6.3.0-x64.msi) MSI (version >= 6.0). O `npm` e `nodejs` paths deve mestar no ambiente PATH
   2. Abra o Command prompt (cmd)
   3. Rode o comando `npm install -g appium` que instalará o Appium pelo NPM
   4. Para iniciar o Appium, simplemente rode o comando `appium` no prompt.
   5. Siga as instruções abaixo para instalação de teste Android ou Windows.
   6. Rode um teste por qualquer  Appium client.

## Configuração adicional para teste de aplicativos Android

   1. Faça o download do Java JDK mais recente [aqui](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) (Primeiro aceite o contrato de licença). Ajuste o 'JAVA_HOME' para ser o seu JDK path. O diretório `bin` deve ser adicionado ao seu  PATH.
   2. Instale o [Android SDK](http://developer.android.com/sdk/index.html). Ajuste o `ANDROID_HOME` variável de ambiente para ser o seu Android SDK path e adicione o diretório `tools` e `platform-tools` no seu PATH.
   3. Instale o [Apache Ant](http://ant.apache.org/bindownload.cgi) ou use que vem com o Android Windows SDK na pasta eclipse\plugins. Certifique-se de adicionar a pasta que contém Ant no seu PATH.
   4. Instale [Apache Maven](http://maven.apache.org/download.cgi) e ajuste as variáveis de ambiente M2HOME e M2. Ajuste `M2_HOME` para o diretório maven que está instalado, e ajuste `M2` para a pasta `bin`. Adicione o path que usou para `M2` no seu PATH.
   5. Para executar os testes no Windows, voce precisará ter o Android Emulator iniciado ou device Android conectado que esteja usando API Level 17 ou superior. Em seguida, execute o comando `appium` via linha de comando.
   6. O seu script  de teste deve garantir que a capacidade `platformVersion` corresponde ao emulador ou device que voce está testando e que a capacidade `app` é o caminho absoluto para o arquivo .apk.

## Configuração adicional para teste em Windows App.

   1. Para testar um app windows, simplesmente certifique-se que o [developer mode](https://msdn.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development) está ativo.

   (veja o[Windows app testing](/docs/en/writing-running-appium/windows-app-testing.md) doc para instruções sobre como executar os testes para windows app)

## Rodando o Appium

Veja o [server documentation](/docs/en/writing-running-appium/server-args.md) para todos os argumentos de linha de comando.

* No windows, execute Appium.exe como administrador ou, quando executado pelo código fonte, voce precisará executar o cmd como administrador.
* Voce deve fornecer as flags `--no-reset` ou `--full-reset` para que o android funcione corretamente no windows.
* Existe um `hardware accelerated emulator for Android`; que possue suas próprias limitações. Para mais informações, voce pode verificar na 
  [pagina](/docs/en/appium-setup/android-hax-emulator.md).

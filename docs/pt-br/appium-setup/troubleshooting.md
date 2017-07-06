## Troubleshooting Appium

Veja o que fazer se tiver problemas, antes de abrir um ticket
no github ou escrever no grupo [appium-discuss discussion group](https://discuss.appium.io).

### General

* Certifique-se de que voce seguiu os passos do [README](/README.md)
* Verifique que seu sistema está configurado de forma adequada (ex: XCode está atualizado,
  Android SDK está instalado e `ANDROID_HOME` está configurado.
* Certifique-se que seus paths estão corretos
* No windows, execute o appium.app como administrador ou quando executar pelo código fonte, voce precisa executar o cmd como administrador.

### Se estiver executando o Appium.app

* Atualize e reinicie o appium.app. Se voce receber uma mensagem dizendo que o app não pode ser atualizado,
  volte a baixa-lo no  [appium.io](http://appium.io).

### Se você estiver executando o Appium a partir da fonte

* `git pull` para garantir que voce esteja executando  o código mais recente.
* Remova dependencias antigas: `rm -rf node_modules`
* reinstale as dependencias: `npm install`
* Re-transpile o código: `gulp transpile`

* Voce tambem pode user o [Appium Doctor](https://github.com/appium/appium-doctor) para determinar se seu sistema está configurado corretamente para o appium.
* Se voce receber esse erro após a atualização do Android SDK 22:
  `{ANDROID_HOME}/tools/ant/uibuild.xml:155: SDK does not have any Build Tools installed.`
No Android SDK 22, o `platform tools` e `build tools` são dividos em items
próprios no SDK Manager. Certifique-se de instalar o build-tools e platform-tools.

### Android

* Verifique se o emulador do Android está funcionando.
* As vezes é util rodar `adb kill-server && adb devices`. Isso pode resetar as conexões com dispositivos android.
* Certifique-se que o ANDROID_HOME está apontando para o diretório do SDK do android.

### Windows

* Certifique-se que o developer mode está ativo.
* Certifique-se que o command prompt está no modo Administrador.
* Verifique que URL Appium server está ouvindo corretamente o script de teste.

### IOS

* Certifique-se que o  Instruments.app não está aberto.
* Se estiver executando pelo simulador, verifique que o dispositivo não está conectado.
* Certifique-se que o accessibility helper esteja desligado no seu dispositivo.
* Verifique que o app está compilado para a versão do simulador que está utilizando.
* Verifique que o app foi compilado para o simulador (ou dispositivo físico) como apropriado (ex:  in debug mode para simulador), ou pode aparecer o seguinte erro
  :`posix spawn`.
* Se voce executou o appium com comando suso, talvez seja necessário rodar o `sudo rm
  /tmp/instruments_sock` e tente novamente sem o comando sudo.
* Se esta for a primeira vez que está executando o Appium, certifique-se que autorizou o uso do do
  Instruments. Veja em [running on OSX documentation](./running-on-osx.md#authorizing-ios-on-the-computer).
* Se o Instruments está quebrando quando executados contra um dispositivo físico ("exited with code 253"), certifique-se que o Xcode tenha baixado o "device symbols. Vá em Window -> Devices, e deve iniciar automáticamente. Isso é necessário após atualizações de versões iOS.
* Se ver o erro `iOS Simulator failed to install the application.` e os paths estão corretos, tente reiniciar sua máquina.
* Certifique-se que o chaveiro do macOS contenha os certificados necessários para o build do seu app e o WebDriver Agent esteja desbloqueado. Especialmente se estiver usando ssh. O sintoma geral é `codesign` failure.
* Se voce tiver elementos customizados no seu app, eles não serão automatizados pelo UIAutomation (e, portanto Appium) por padrão. Voce precisa definir o stauts de acessibilidade (accessibility status) como 'habilitado' neles. A maneira de fazer isso via código é:

  ```center
  [myCustomView setAccessibilityEnabled:YES];
  ```

* Testes em iOS podem exibir sintomas semelhantes a um vazamento de memória, incluindo lentidão ou desempenho "travado. Se estiver presensiando esse tipo de problema, é devido a um problema conhecido com o NSLog. Uma opção é remover o NSLog do seu código.
  No entanto, existem várias abordagens que também podem ajudar sem que exija refatoração no seu código.

  ### Workaround 1
  NSLog é uma macro e pode ser redefinida. ex:
  ```objectivec
  // *You'll need to define TEST or TEST2 and then recompile.*

  #ifdef TEST
    #define NSLog(...) _BlackHoleTestLogger(__VA_ARGS__);
  #endif // TEST
  #ifdef TEST2
    #define NSLog(...) _StdoutTestLogger(__VA_ARGS__);
  #endif // TEST2

  void _BlackHoleTestLogger(NSString *format, ...) {
      //
  }

  void _StdoutTestLogger(NSString *format, ...) {
      va_list argumentList;
      va_start(argumentList, format);
      NSMutableString * message = [[NSMutableString alloc] initWithFormat:format
                                                  arguments:argumentList];

      printf(message);

      va_end(argumentList);
      [message release];
  }
  ```

  ### Workaround 2
  Substitua manualmente a função subjacente que envolve o NSLog. Esse método foi recomendando pela
  [Apple em um contexto semelhante](https://support.apple.com/kb/TA45403?locale=en_US&viewlocale=en_US)

  ```objectivec
  extern void _NSSetLogCStringFunction(void(*)(const char *, unsigned, BOOL));

  static void _GarbageFreeLogCString(const char *message, unsigned length, BOOL withSyslogBanner) {
     fprintf(stderr, "%s\\n", message);
  }

  int main (int argc, const char *argv[]) {
     NSAutoreleasePool *pool = [[NSAutoreleasePool alloc] init];
     int exitCode;

     setbuf(stderr, NULL);

     _NSSetLogCStringFunction(_GarbageFreeLogCString);
     exitCode = WOApplicationMain(@"Application", argc, argv);
     [pool release];
     return exitCode;
  }
```


### Webview/Hybrid/Safari app support

* Certifique se que o item 'Web Inspector' está ativo no device real.
* Certifique se que está ativado, no Safari - Advance Preferences- Developer menu para simuladores.
* Certifique-se de mudar corretamente o contexto usando o comando `context` do appium fornecido pela eu client.
* Se voce obtiver o seguinte erro: select_port() failed ao tentar abrir o proxy, veja essa [discussão](https://groups.google.com/forum/#!topic/appium-discuss/tw2GaSN8WX0)
* Em uma sessão no Safari, se os  logs indicarem que a URL inicial não pode ser inciada. certifique-se que
  voce habilitou o teclado do software. Veja essa [discussão](https://github.com/appium/appium/issues/6440).

### Deixe a comunidade saber

Depois de testar as etapas acima e seu problema ainda não foi resolvido,
aqui está o que pode fazer:

Se voce está tendo problemas para que o appium funcione e as mensagens de erro do appium não estão claras
junte-se ao [grupo de discussão](https://discuss.appium.io)
e envie uma mensagem. Inclua o seguinte:

* Como está executando o Appium (Appium.app, npm, source)
* Qual sistema operacional está usando
* Qual dispositivo e versão voce está usando (ex: Android 4.4 ou iOS 7.1)
* Se voce está executando contra um dispositivo real ou um emulador/simulador.
* Os "client-side" e "server-side" erros que está recebendo (ex:
"Em Python está é a exceção que recebo no meu script de teste,
aqui está um link para o output do Appium server)
* É muito importante incluir o output do appium server quando é executado no modo "verboso", pois no auxilia no diagnóstico para ver o que está acontecendo.

Se voce encontrou o que voce acredita ser um bug, vá direto no [issue tracker](https://github.com/appium/appium/issues)
e submita o bug, descrevendo ele e como reproduzir.

### Problemas conhecidos

* Se voce instalou o  Node pelo site do Node, ele requer que voce use o comando sudo
  para `npm`. Isso não é ideal. Tente obter o node pelo [nvm](https://github.com/creationix/nvm),
  [n](https://github.com/visionmedia/n) ou `brew install node`!
* O suporte a Webview funciona em devices iOS reais com um proxy, veja essa [discussão](https://groups.google.com/d/msg/appium-discuss/u1ropm4OEbY/uJ3y422a5_kJ).
* As vezes os elementos de UI, no iOS, torna-sem inoperantes milisegundos após serem encontrados. Isso resulta em um erro que parece  `(null) cannot be tapped`.
  As vezes, a única solução é colocar no código a tentativa de encontrar e já clicar/validar no momento.
* Appium pode ter dificuldade em encontrar o executável  `node`se voce tiver node e npm instalados via MacPorts. Voce deve se certificar de que MacPorts bin folder (`/opt/local/bin` por padrão) está adicioando ao `PATH` em algum lugar no seu
  `~/.profile`, `~/.bash_profile` ou `~/.bashrc`.

### Erros específicos.

|Ação|Erro|Resolução|
|------|-----|----------|
|Executando testes iOS|`[INST STDERR] posix spawn failure; aborting launch`|Seu app não foi compilado corretamente para o simulador ou device|
|Executando mobile safari test|`error: Could not prepare mobile safari with version '7.1'`|Voce provalvemente precisa executar o script novamente para tornar os arquivos do SDK iOS escritáveis. Veja no [running on OSX documentation](./running-on-osx.md#authorizing-ios-on-the-computer)|

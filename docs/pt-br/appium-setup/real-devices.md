## Appium em dispositivos iOS físicos

Appium tem suporte para testes de dispositivos reais.

Para começar os testes em dispositivos reais, voce precisará do seguinte:

* Um [Apple Developer ID](https://developer.apple.com/programs/ios/)
 e uma conta de desenvolvedor válida com um certificado de distribuição e provisioning profile.
* Um iPad ou iPhone. Certifique-se que o mesmo foi configurado para desenvolvimento no XCode. Veja [esse artigo](https://developer.apple.com/library/ios/recipes/xcode_help-devices_organizer/articles/provision_device_for_development-generic.html) para mais informações.
* Um `.ipa` assinado do seu app, ou o código fonte para compilar.
* Um Mac com [Xcode](https://itunes.apple.com/en/app/xcode/id497799835?mt=12)
 e o Xcode Command Line Developer Tools.

### Provisioning Profile

Um certificado de distribuição e o Provisioning Profile são necessários para testar em um dispositivo físico. seu app também precisa estar 'assinado'. Voce
pode encontrar informações sobre isso em [Apple documentation](https://developer.apple.com/library/ios/documentation/IDEs/Conceptual/AppDistributionGuide/TestingYouriOSApp/TestingYouriOSApp.html).

O Appium tentará instalar o seu app usando o Fruitstrap, mas muitas vezes é mais fácil tentar instalar o seu app pelo XCode para garantir que não ocorra problemas na instalação. (Veja isso em [iOS deploy](ios-deploy.md) para mais informações).

### Usando o XCode para testes (incluindo o iOS 10) com o XCUITest

Essa funcionalidade, atualmente, depende do log baseado no `idevicesyslog`, e
port forwarding baseado no `iProxy`, os quais fazem parte do `libimobiledevice`.
Instale os com [Homebrew](http://brew.sh/),

```
brew install libimobiledevice
```

Como alternativa, o registro pode ser feito via `deviceconsole`, que está disponível
[aqui](https://github.com/rpetrich/deviceconsole). Para escolher entre eles, use
a flag desired capability `realDeviceLogger`, passando qual log deseja utilizar.


### Rodando seus testes com Appium

Assim que o app e dispositivo estiverem prontos, voce pode executar os testes no dispositivo passando os argumentos `-U` ou `--udid` flag para o server ou `udid` desired capability,
e o bundle ID (se o app estiver instalado no dispositivo) ou o caminho do
`.ipa` ou `.apk` arquivo via `--app` flag ou o `app` desired capability.

### Server Arguments

Por exemplo, se estiver inicializando o seu app e deseja que o appium 'forçe' o uso de um UDID, voce pode usar o seguinte comando

```center
appium -U <udid> --app <path or bundle>
```

Isto iniciará o Appium forçando a usar esse dispositivo primeiro.

Consulte a pagina [Appium server arguments](/docs/en/writing-running-appium/server-args.md) para mais detalhes dos argumentos que pode usar.

### Desired Capabilities

Voce pode iniciar o app em um dispositivo, incluindo as seguintes "capabilities" no seus testes:

* `app`
* `udid`

Consulte a pagina [Appium server capabilities](/docs/en/writing-running-appium/caps.md) para mais detalhes das quais "capabilites" pode usar.

### Troubleshooting ideas

0. Verifique se o UDID está correto, verificando no XCode organizer ou no Itunes. é uma string longa (mais de 20 caracteres)
0. Certifique-se de que você pode executar seus testes no simulador.
0.  Verifique se você pode executar sua automação apartir do Instruments.
0. Certifique-se que o  Instruments ainda não está em execução.
0. Certifique-se que o UI Automation está ativado no seu dispositivo. Settings -> Developer -> Enable UI Automation

### Appium em dispositivos físicos com Android

Hooray! Não a nada extra a se fazer para rodar em dispositivos físicos com Android: funciona exatamente com se teste com emuladores. Certifique-se que o seu dispositivo
está conectado com o ADB e o modo desenvolvedor está ativo. Para estar no Chrome em dispositivos
físicos, sua responsabilidade é verificar que o Chrome esteja em uma versão apropriada para testes.

Além disso, voce precisa se certificar que o item "Fontes desconhecidas" esteja desmarcado/desativado, caso contrário pode impedir do apps de auxílio do Appium sejam instalado ou executem sua tarefa corretamente.

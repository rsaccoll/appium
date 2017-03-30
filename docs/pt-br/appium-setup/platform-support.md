## Appium Platform Support

Appium suporta uma variedade de plataformas e modalidades de teste (nativo,
hibrido, web, dispositivos físicos, simuladores, etc...). Esse documento desenvolvido para
tornat explicito o nível de apoio e requisitos para cada um deles.

### iOS Support

Veja [Running on OS X: iOS](running-on-osx.md) para requisitos, configurações e instruções.

* Versão: 7.1 e superior
* Dispositivos: iPhone Simulador, iPad Simulador, e dispositivos físicos de iPhones e iPads
* Suporte a app nativo: Sim, com a versão de debug do .app (simulador),
  .ipa corretamente 'signed' (dispositivos físicos). O apoio subjacente é o
  Apple's [XCUITest](https://developer.apple.com/reference/xctest) (ou [UIAutomation](https://developer.apple.com/library/ios/documentation/DeveloperTools/Reference/UIAutomationRef/) para versões mais antigas).
* Mobile web suporte: Sim, via automation do mobile Safari. Para dispositivos físicos,
  `ios-webkit-remote-debugger` é requerido, e automação de aspectos nativos
  da interface do Safari não é possível. Veja o [mobile web doc](/docs/en/writing-running-appium/mobile-web.md) para mais instruções.
* Suporte híbrido: Sim. Para dispositivos físicos, ios-webkit-remote-debugger é
  requirido. Veja em [hybrid doc](/docs/en/advanced-concepts/hybrid.md) para mais instruções.
* Suporte para automatizar vários aplicativos em uma sessão: Não
* Suporte para automatizar vários dispositivos simultaneamente: Não
* Suporte para automatizar apps fornecidos pelo fornecedor ou de terceiros: : Somente
  apps fornecidos (Preferences, Maps, etc...), e somente no simulador. Para iOS 10+, voce pode automatizar a tela inicial também
* Suporte para automação, non-standard UI controles customizados: Minimo. Voce precisa
  definir o "accessibility information" no controle que permitem automação.

### Android Support

Veja [Running on OS X: Android](running-on-osx.md),
[Running on Windows](running-on-windows.md), ou
[Running on Linux](running-on-linux.md) para requisitos, configurações e instruções.

* Versões: 2.3 e superior
  * Versions 2.3 a 4.2 são suportadas via Appium's versão do
    [Selendroid](http://selendroid.io), que utiliza [Instrumentation](http://developer.android.com/reference/android/app/Instrumentation.html). O Selendroid tem um conjunto de comandos diferentes do Appium padrão (embora este seja rapidamente minimizado) e um perfil diferente de suporte. Para acessar esse automation backend, use o `automationName` capability com o valor `Selendroid`.
  * Versões 4.2 e superiores são suportadas via Appium's  [UiAutomator](http://developer.android.com/tools/testing-support-library/index.html#UIAutomator)
      bibliotecas. Esse é o backend de automação padrão.
* Dispositivos: Android emuladores e dispositivos físicos
* Suporte a app nativo: Sim
* Mobile web suporte: Sim (Mas não usando o Selendroid). A automação
  é efetuada usando o [Chromedriver](https://code.google.com/p/selenium/wiki/ChromeDriver)
  como um proxy. Com 4.2 e 4.3, a automação funciona apenas em Chrome oficial
  browser ou Chromium. Com 4.4+, a automação funciona também em navegadores
  "alternativos". O Chrome/Chromium/Browser deve estar já instalado no
  dispositvo que vai rodar os testes. Veja o [mobile web doc](/docs/en/writing-running-appium/mobile-web.md) para mais instruções.
* Suporte híbrido: Sim. Veja o [hybrid doc](/docs/en/advanced-concepts/hybrid.md) para mais instruções.
  * Com o backend padrão do Appium: versões 4.4 e superiores
  * Com o backend do Selendroid: versões 2.3 até 4.2
* Suporte para automatizar vários aplicativos em uma sessão: Sim (mas não se usar o Selendroid)
* Suporte para automatizar vários dispositivos simultaneamente: Sim,
  mas o Appium precisa ser iniciado usando portas diferentes para o server
   parameters `--port`, `--bootstrap-port` (ou `--selendroid-port`) e/ou
  `--chromedriver-port`. Veja o [server args doc](/docs/en/writing-running-appium/server-args.md) para mais
  informações sobre esses parametros.
* Suporte para automatizar aplicativos fornecidos pelo fornecedor ou de terceiros: Sim (mas não se usar o Selendroid)
* Suporte para automação, non-standard UI controles customizados: Não.

### Windows Desktop Support

Veja esses documentos para mais detalhes:

* [Running on Windows](running-on-windows.md)
* [Windows App Testing](/docs/en/writing-running-appium/windows-app-testing.md)

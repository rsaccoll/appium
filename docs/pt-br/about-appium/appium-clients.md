## Lista de bibliotecas com suporte ao Appium server

Essas bibliotecas envolvem bibliotecas padrões Selenium para fornecer todos os comandos regulares orientados pelo [JSON Wire protocol](https://w3c.github.io/webdriver/webdriver-spec.html), e adicionar comandos extras relacionados ao controle dos dispositivos, como **multi gestos[multi-touch gestures]** and **orientação da tela[screen orientation]**.

As bibliotecas do Appium implementam [Mobile JSON Wire Protocol](https://github.com/SeleniumHQ/mobile-spec/blob/master/spec-draft.md) (um projeto de extensão oficial para o protocolo), e elementos do [W3C Webdriver spec](https://dvcs.w3.org/hg/webdriver/raw-file/default/webdriver-spec.html) (uma especificação de automação ; onde é definida a API MultiAction).

O próprio Appium server define extensões customizadas para os protocolos oficiais, dando aos usuários do Appium acesso a vários comportamentos dos dispositívos úteis (como instalar/desinstalar apps durante o andamento de uma sessão). É por isso que precisamos de clientes específicos para Appium, e não apenas os 'vanilla' Selenium clients. Claro que, as bibliotecas do Appium apenas **adicionam** funcionalidades (na verdade, eles simplesmente extendem os 'standard Selenium clients'), então eles ainda podem ser usados para executar sessões normais de Selenium.

Language/Framework | Github Repo and instruções de instalação |
----- | ----- |
Ruby | [https://github.com/appium/ruby_lib](https://github.com/appium/ruby_lib)
Python | [https://github.com/appium/python-client](https://github.com/appium/python-client)
Java | [https://github.com/appium/java-client](https://github.com/appium/java-client)
JavaScript (Node.js) | [https://github.com/admc/wd](https://github.com/admc/wd)
Objective C | [https://github.com/appium/selenium-objective-c](https://github.com/appium/selenium-objective-c)
PHP | [https://github.com/appium/php-client](https://github.com/appium/php-client)
C# (.NET) | [https://github.com/appium/appium-dotnet-driver](https://github.com/appium/appium-dotnet-driver)
RobotFramework | [https://github.com/jollychang/robotframework-appiumlibrary](https://github.com/jollychang/robotframework-appiumlibrary)

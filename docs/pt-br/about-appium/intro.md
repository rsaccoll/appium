## Introdução ao Appium

Appium é uma ferramenta open source para automatizar apps nativos, mobile web e apps híbridos em iOS, Android mobile e Windows desktop platforms.  **Apps nativos** são aqueles escritos usando o iOS, Android, or Windows SDKs.  **Mobile web apps** são acessados usando um navegador para dispositivos móveis (o Appium suporta o Safari no iOS e Chrome ou o app chamado 'Browser' no Android).  **Apps Híbridos** tem um 'wrapper' em torno de uma webview -- um controle nativo que permite interação com o conteúdo web. Projetos como [Phonegap](http://phonegap.com/), facilitam a criação de aplicativos usando tecnologias Web, que são empacotadas em um wrapper nativo, criando um aplicativo híbrido.

Importante, Appium é "cross-platform": permite escrever testes contra
plataforma múltiplas (iOS, Android, Windows), usando a mesma API. Isso permite a reutilização do código entre iOS, Android, e Windows planos de teste.

Para obter informações específicas o que significa para o Appium o "apoiar" as
plataformas, e as modalidades de automação, consulte o [Documento de suporte a plataforma](/docs/pt-br/appium-setup/platform-support.md).

### Filosofia do Appium

Appium foi projetado para atender às necessidades de automação móvel de acordo com uma filosofia delineada pelos quatros príncipios a seguir:

1. Você não deve ter que recompilar seu aplicativo ou modificá-lo de forma alguma para automatizá-lo.
2. Você não deve ser bloqueado em um idioma específico ou estrutura para escrever e executar seus testes.
3. Um mobile automation framework não deve reinventar a roda quando se trata de  automation APIs.
4. Um mobile automation framework deve ser de código aberto, em espírito e prática, bem como em nome!

### Appium Design

Então, como a estrutura do projeto Appium vive esta filosofia? Nós
satisfazemos o requisito #1 usando estruturas de automação providas pelo fornecedor "debaixo dos panos". Dessa forma, Nós não precisamos compilar qualquer app específico ou
código terceiros de frameworks para o seu app. Isso significa que **voce está testando o mesmo app que está entregando**. Os frameworks providos pelo fornecedor são:

* iOS 9.3 e acima: Apple's [XCUITest](https://developer.apple.com/reference/xctest)
* iOS 9.3 e abaixo: Apple's [UIAutomation](https://developer.apple.com/library/ios/documentation/DeveloperTools/Reference/UIAutomationRef/)
* Android 4.2+: Google's [UiAutomator](http://developer.android.com/tools/help/uiautomator/index.html)
* Android 2.3+: Google's [Instrumentation](http://developer.android.com/reference/android/app/Instrumentation.html). (O suporte a essa versão é fornecido por um projeto separado, [Selendroid](http://selendroid.io))
* Windows: Microsoft's [WinAppDriver](http://github.com/microsoft/winappdriver)

Atendendo o requisito #2, empacotando os frameworks citados pelos fornecedores em uma API,
o [WebDriver](http://docs.seleniumhq.org/projects/webdriver/) API.
WebDriver (aka "Selenium WebDriver") especifica um protocolo client-server
(conhecido como [JSON Wire Protocol](https://w3c.github.io/webdriver/webdriver-spec.html)).
Dada esta arquitetura 'client-server', um cliente escrito em qualquer linguagem pode
ser usado para enviar HTTP requests apropriadamente ao servidor. Já existem
[clientes escritos nas linguagens mais populares](http://appium.io/downloads). Isso também significa
que voce está livre para usar qualquer 'test runner' e 'test framework' que quiser;
as bibliotecas são simplesmente 'HTTP clients' e podem ser misturadas em seu
código da maneira que desejar. Em outras palavras,  os clientes 'Appium & WebDriver' não são
tecnicamente "test frameworks" -- eles são "bibliotecas de automação". Voce pode
gerenciar o seu ambiente como quiser!

Atendendo o requisito #3 do mesmo jeio: WebDriver se tornou de fato
automatizador dos navegadores web padrão, e é um [W3C Working Draft](https://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html).
Porque fazer algo totalmente diferente para mobile? Em vez disso, temos [ampliado o protocolo](https://github.com/SeleniumHQ/mobile-spec/blob/master/spec-draft.md)
com métodos de API extras, úteis para automação mobile.

E deve ser óbvio que o requisito #4 foi atendido -- Voce está lendo isso
Porque [Appium é open source](https://github.com/appium/appium).

### Conceitos do Appium

**Arquitetura Client/Server**<br/>
Appium é, em seu coração, um webserver que expõe um REST API. Ele recebe
conexões de um cliente, escuta comandos, executa comandos em um celular (ou tablet),
e responde como um retorno HTTP representando o resultado do
comando executado. O fato de possuirmos uma arquitetura client/server
abre um monte de possibilidades: nos podemos escrever nosso código de teste em qualquer linguagem
que possua um cliente HTTP, mas é mais fácil usar um dos nossos [Appium client
libraries](http://appium.io/downloads). Podemos colocar o servidor em uma máquina que é diferente
de onde nossos testes estão sendo executados. Podemos escrever testes automatizados e contar com um 'cloud service',
como [Sauce Labs](https://saucelabs.com/mobile), para receber e interpretar os comandos.

**Sessões**<br/>
A automação é sempre realizada no contexto de uma sessão. Os clientes
iniciam uma sessão com o servidor de formas específicas a cada biblioteca,
mas todos eles acabam enviando uma requisição `POST /session` ao servidor,
com um objeto JSON chamado 'desired capabilities'. Neste ponto
o servidor irá iniciar a sessão e responderá com um 'ID Session'
que será usado para enviar comandos adicionais.

**Desired Capabilities**<br/>
Desired capabilities são um conjunto de chaves e valores (i.e.,
a map or hash) enviando ao Appium server para informar o server o tipo de
'automation session' estamos interessados em inicializar. Existem também
capacidades que podem modificar o comportamento do server durante a automação.
Por exemplo, podemos definir o recurso `platformName` para `iOS` para informar
Appium que queremos uma sessão iOS, ao invés de  Android ou Windows. Ou podemos
definir a capacidade `safariAllowPopups` para `true` para garantir que,
durante uma sessão de automação com Safari, estamos autorizados para usar Javascript
para abrir novas janelas. Veja o [capabilities doc](/docs/en/writing-running-appium/caps.md) para uma lista completa 
recursos disponíveis para o Appium.

**Appium Server**<br/>
Appium is a server é programado em Node.js. Pode ser compilado e instalado pelo [Código fonte](https://github.com/appium/appium/blob/master/docs/en/contributing-to-appium/appium-from-source.md) ou instalado diretamente
do NPM:
```
$ npm install -g appium
$ appium
```

**Clientes Appium**<br/>
Existem bibliotecas (em Java, Ruby, Python, PHP, JavaScript, and C#)
que suportam extensões do Appium para o protocolo WebDriver. Ao usar Appium,
voce pode usar essas bibliotecas em vez do seu WebDriver
client normal. Voce pode ver a lista completa de bibliotecas [aqui](appium-clients.md).

**[Appium.app](https://github.com/appium/appium-dot-app), [Appium.exe](https://github.com/appium/appium-dot-exe)**<br/>
Existem um GUI em torno do Appium server que pode ser efetuado o download.
Estes vem com tudo o que é necessário para executar o Appium server,
assim voce não precisa se preocupar com o Node. Eles também vem com um inspetor de elementos,
que permite inspecionar a estrutura e hierarquia do seu app. Isso pode ser útil quando se escreve os testes.

### Iniciando

Parabéns! Voce agora possui o conhecimento suficiente para começar usar Appim. Por que não ir para [getting started doc](https://github.com/appium/appium/blob/master/README.md) para obter mais informações e requisitos mais detalhados?

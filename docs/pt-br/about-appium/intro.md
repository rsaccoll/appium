## Introdução ao Appium

Appium é uma ferramenta de código aberto para automatizar apps nativos, mobile web e apps híbridos em iOS, Android mobile e Windows desktop platforms.  **Apps nativos** são aqueles escritos usando o iOS, Android, or Windows SDKs.  **Mobile web apps** são acessados usando um navegador para dispositivos móveis (o Appium suporta o Safari no iOS e Chrome ou o app chamado 'Browser' no Android).  **Apps Híbridos** tem um 'wrapper' em torno de uma webview -- um controle nativo que permite interação com o conteúdo web. Projetos como [Phonegap](http://phonegap.com/), facilitam a criação de aplicativos usando tecnologias Web, que são empacotadas em um wrapper nativo, criando um aplicativo híbrido.

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

So how does the structure of the Appium project live out this philosophy? We
meet requirement #1 by using vendor-provided automation frameworks under the
hood. That way, we don't need to compile in any Appium-specific or
third-party code or frameworks to your app. This means **you're testing the same app you're shipping**. The vendor-provided frameworks we use are:

* iOS 9.3 and above: Apple's [XCUITest](https://developer.apple.com/reference/xctest)
* iOS 9.3 and lower: Apple's [UIAutomation](https://developer.apple.com/library/ios/documentation/DeveloperTools/Reference/UIAutomationRef/)
* Android 4.2+: Google's [UiAutomator](http://developer.android.com/tools/help/uiautomator/index.html)
* Android 2.3+: Google's [Instrumentation](http://developer.android.com/reference/android/app/Instrumentation.html). (Instrumentation support is provided by bundling a separate project, [Selendroid](http://selendroid.io))
* Windows: Microsoft's [WinAppDriver](http://github.com/microsoft/winappdriver)

We meet requirement #2 by wrapping the vendor-provided frameworks in one API,
the [WebDriver](http://docs.seleniumhq.org/projects/webdriver/) API.
WebDriver (aka "Selenium WebDriver") specifies a client-server protocol
(known as the [JSON Wire Protocol](https://w3c.github.io/webdriver/webdriver-spec.html)).
Given this client-server architecture, a client written in any language can
be used to send the appropriate HTTP requests to the server. There are
already [clients written in every popular programming language](http://appium.io/downloads). This also
means that you're free to use whatever test runner and test framework you
want; the client libraries are simply HTTP clients and can be mixed into your
code any way you please. In other words, Appium & WebDriver clients are not
technically "test frameworks" -- they are "automation libraries". You can
manage your test environment any way you like!

We meet requirement #3 in the same way: WebDriver has become the de facto
standard for automating web browsers, and is a [W3C Working Draft](https://dvcs.w3.org/hg/webdriver/raw-file/tip/webdriver-spec.html).
Why do something totally different for mobile? Instead we have [extended the protocol](https://github.com/SeleniumHQ/mobile-spec/blob/master/spec-draft.md)
with extra API methods useful for mobile automation.

It should be obvious that requirement #4 is a given -- you're reading this
because [Appium is open source](https://github.com/appium/appium).

### Appium Concepts

**Client/Server Architecture**<br/>
Appium is at its heart a webserver that exposes a REST API. It receives
connections from a client, listens for commands, executes those commands on a
mobile device, and responds with an HTTP response representing the result of
the command execution. The fact that we have a client/server architecture
opens up a lot of possibilities: we can write our test code in any language
that has a http client API, but it is easier to use one of the [Appium client
libraries](http://appium.io/downloads). We can put the server on a different machine than our
tests are running on. We can write test code and rely on a cloud service
like [Sauce Labs](https://saucelabs.com/mobile) to receive and interpret the commands.

**Session**<br/>
Automation is always performed in the context of a session. Clients initiate
a session with a server in ways specific to each library,
but they all end up sending a `POST /session` request to the server,
with a JSON object called  the 'desired capabilities' object. At this point
the server will start up the automation session and respond with a session ID
which is used for sending further commands.

**Desired Capabilities**<br/>
Desired capabilities are a set of keys and values (i.e.,
a map or hash) sent to the Appium server to tell the server what kind of
automation session we're interested in starting up. There are also various
capabilities which can modify the behavior of the server during automation.
For example, we might set the `platformName` capability to `iOS` to tell
Appium that we want an iOS session, rather than an Android or Windows one. Or we might
set the `safariAllowPopups` capability to `true` in order to ensure that,
during a Safari automation session, we're allowed to use JavaScript to open
up new windows. See the [capabilities doc](/docs/en/writing-running-appium/caps.md) for the complete list of capabilities available for Appium.

**Appium Server**<br/>
Appium is a server written in Node.js. It can be built and installed [from source](https://github.com/appium/appium/blob/master/docs/en/contributing-to-appium/appium-from-source.md) or installed directly from NPM:
```
$ npm install -g appium
$ appium
```

**Appium Clients**<br/>
There are client libraries (in Java, Ruby, Python, PHP, JavaScript, and C#)
which support Appium's extensions to the WebDriver protocol. When using Appium,
you want to use these client libraries instead of your regular WebDriver
client. You can view the full list of libraries [here](appium-clients.md).

**[Appium.app](https://github.com/appium/appium-dot-app), [Appium.exe](https://github.com/appium/appium-dot-exe)**<br/>
There exist GUI wrappers around the Appium server that can be downloaded.
These come bundled with everything required to run the Appium server,
so you don't need to worry about Node. They also come with an Inspector,
which enables you to check out the hierarchy of your app. This can come in handy when writing tests.

### Getting Started

Congratulations! You are now armed with enough knowledge to begin using Appium. Why not head to the [getting started doc](https://github.com/appium/appium/blob/master/README.md) for more detailed requirements and instructions?

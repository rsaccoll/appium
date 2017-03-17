## Automatizando apps Híbridos

Um dos princípios de fundamento do Appium é que voce não deve ter que mudar
o seu app para testar. De acordo com essa metodologia, é possível testar
apps Híbridos (ex: o [UIAWebView](https://developer.apple.com/library/ios/documentation/ToolsLanguages/Reference/UIAWebViewClassReference/)
elementos de um app iOS) da mesma maneira
que se pode testar web apps em Selenium. Há um pouco de complexidade técnica
necessária para que Appium saiba se deseja automatizar os aspectos nativos
de um app ou a web view, mas felizmente, podemos utilizar o
WebDriver protocol para tudo.

Aqui estão os passo necessários ocorrer comunicação a uma web view nos seus testes com Appium:

1.  Navegar até a parte do seu aplicativo que a web view está ativa
1.  Chame [GET session/:sessionId/contexts](https://github.com/SeleniumHQ/mobile-spec/blob/master/spec-draft.md)
1.  Isso retorna uma lista de contextos que podem ser acessados, como 'NATIVE_APP' ou 'WEBVIEW_1'
1.  Chame [POST session/:sessionId/context](https://github.com/SeleniumHQ/mobile-spec/blob/master/spec-draft.md)
    com o id do contexto que deseja acessar
1.  (Isso coloca sua sessão do Appium em um modo onde todos os comandos são
    interpretados como destinado a automatizar a web view,
    ao invés de automatizar a parte nativa do app. Por exemplo,
    se voce invocar getElementByTagName, ele irá operar no DOM da web
    view, ao invés de retornar os UIAElements. Claro que,
    certos métodos do WebDriver apenas fazem sentido em um contexto ou outro,
    então se tentar algo no contexto errado, irá receber uma mensagem de erro).
1.  Para interromper a automação da web view, e retornar para o contexto nativo
    do app, basta chamar `context` mais uma ves o id do contexto nativo
    para sair do contexto da webview.

```javascript
// javascript
// assumindo que o `driver` já está inicializado para esse app
driver
    .contexts().then(function (contexts) { // Obtem uma lista de views disponiveis. Retorna o seguinte array: ["NATIVE_APP","WEBVIEW_1"]
        return driver.context(contexts[1]); // seleciona o contexto da web view
    })

    // algum teste para a webview
    .elementsByCss('.green_button').click()

    .context('NATIVE_APP') //sai do contexto atual

    // faça mais coisas nativas se quiser aqui

    .quit() // acaba o teste
```

```java
// java
// assumindo que temos um set de capacidades já ajustadas
driver = new AppiumDriver(new URL("http://127.0.0.1:4723/wd/hub"), capabilities);

Set<String> contextNames = driver.getContextHandles();
for (String contextName : contextNames) {
    System.out.println(contextNames); //printa os contextos selecionados NATIVE_APP \n WEBVIEW_1
}
driver.context(contextNames.toArray()[1]); // ajusta para usar o contexto WEBVIEW_1

//algum teste para a webview
String myText = driver.findElement(By.cssSelector(".green_button")).click();

driver.context("NATIVE_APP"); // sai do contexto atual

// faça mais coisas nativas se quiser aqui

driver.quit(); //acaba o teste
```

```ruby
# ruby
# assumindo que temos um set de capacidades já ajustadas
@driver = Selenium::WebDriver.for(:remote, :desired_capabilities => capabilities, :url => SERVER_URL)

# Eu modo para o último contexto porque sempre é uma webview em nosso caso, em outros casos é preciso especificar um contexto
# Visualize os logs do Appium enquanto executa o @driver.contexts para descobrir qual contexto é o que deseja e encontrar o ID relativo.
# Em seguida, mude para ele usando o @driver.switch_to.context("WEBVIEW_6")

Given(/^I switch to webview$/) do
    webview = @driver.contexts.last
    @driver.switch_to.context(webview)
end

Given(/^I switch out of webview$/) do
    @driver.switch_to.context(@driver.contexts.first)
end

# Agora pode usar CSS para selecionar um elemento dentro da sua web view

And(/^I click a webview button $/) do
    @driver.find_element(:css, ".green_button").click
end
```

```python
# python
# assumindo que o `driver` já está inicializado para esse app

# mude para web view
webview = driver.contexts.last
driver.switch_to.context(webview)

# algum teste para a webview
driver.find_element(:css, ".green_button").click

# mude para contexto nativo
driver.switch_to.context(driver.contexts.first)

# faça mais coisas nativas se quiser aqui

driver.quit()
```

```php
// php
// assumindo que o `driver` já está inicializado para esse app no AppiumTestCase

public function testThings()
{
        $expected_contexts = array(
                0 => 'NATIVE_APP',
                1 => 'WEBVIEW_1'
        );

        $contexts = $this->contexts();
        $this->assertEquals($expected_contexts, $contexts);

        $this->context($contexts[1]);
        $context = $this->context();
        $this->assertEquals('WEBVIEW_1', $context);

        // faça algum teste web

        $this->context('NATIVE_APP');

        // faça algum teste mobile
}
```

### Automatizando apps híbridos no Android

Appium vem com suporte embutido para apps híbridos via Chromedriver. Appium também usa
Selendroid "debaixo dos panos" para suporte a dispositivos abaixo da versão 4.4. (nesse caso, voce deve especificar `"automationName": "selendroid"` nas capacidades desejadas).

certifique-se que
[setWebContentsDebuggingEnabled](http://developer.android.com/reference/android/webkit/WebView.html#setWebContentsDebuggingEnabled(boolean)) está definido no modo "true" como descrito no [remote debugging docs](https://developer.chrome.com/devtools/docs/remote-debugging#configure-webview).

Depois de definir as configurações desejadas e iniciar uma sessão no appium, siga as instruções gerais acima.

### Automatizando apps híbridos no iOS

Para interagir com a webview, appium estabelece uma comunicação
usando o remote debugger. Ao executar no simulador esta conexão é
estabelecida diretamente no simulador quando o appium server
encontra-se na mesma máquina.

Depois de definir as configurações desejadas e iniciar uma sessão no appium, siga as instruções gerais acima.

### Execução em dispositivos iOS

Ao executar em um dispositivo real, o Appium não consegue acessar a web view diretamente. Portanto
a conexão deve ser estabelecida por um cabo apropriado para dispositivos iOS. Para estabelecer
essa conexão usamos o [ios-webkit-debugger-proxy](https://github.com/google/ios-webkit-debug-proxy).

Para instruções sobre como instalar e executar ios-webkit-debugger-proxy veja a documentação [iOS webkit debug proxy](/docs/en/advanced-concepts/ios-webkit-debug-proxy.md).

Agora voce pode iniciar uma bateria de testes com Appium e seguir as instruções acima.

## Settings

Settings são um novo conceito introduzido pelo Appium. Eles atualmente não fazem parte do Mobile JSON Wire Protocol, ou do Webdriver spec.

As configurações são uma maneira de especificar o comportamento do Appium server.

Settings são:
 - Mútaveis, podem ser alterados durante uma sessão.
 - Apenas relevantes durante a sessão que foram ajustados. Serão redefinidos a cada nova sessão.
 - Controle do comportamento do Appium server durante a automação dos testes. Eles não se aplicam ao controle do app ou do device sob teste.

Um exemplo de uma configuração seria o `ignoreUnimportantViews` para Android. O Android pode ser configurado para ignorar elementos em sua hierarquia (que se apresentam irrelevantes). Essa configuração pode fazer com que os testes sejam executados mais rapidamente. Um usuário que *quer* acessar esse elementos irrelevantes, poderia desabilitar o `ignoreUnimportantViews`, e depois reabilitar novamente.

Outro exemplo de um caso de uso para os Settings seria configurar o Appium para ignorar elementos que não são visíveis.

Settings são implementados através dos seguintes Endpoints da API:

**POST** /session/:sessionId/appium/settings

>Espera um JSON, onde as chaves correspondem ao "settings name", e os valores ao valor da configuração.
```
{
  settings: {
   ignoreUnimportantViews : true
  }
}
```

**GET** /session/:sessionId/appium/settings

>Retorna um JSON de todas as confgurações especificadas atulamente.
```
{
  ignoreUnimportantViews : true
}
```

Observe que os comandos reais que voce usaria no seu script de teste diferem com base na linguagem. Consulte a documentação específica do Appium client para obter mais informações.

### Settins suportados

**"ignoreUnimportantViews"** - Boolean que define se os devices Android deve usar `setCompressedLayoutHeirarchy()` que ignora todas as views que foram marcadas como IMPORTANT_FOR_ACCESSIBILITY_NO ou IMPORTANT_FOR_ACCESSIBILITY_AUTO (e foram consideradas não importantes pelo sistema), em uma tentativa de tornar as coisas menos confusas ou mais rápidas.

#### Android UiAutomator Configurator

seta [UiAutomator Configurator](https://developer.android.com/reference/android/support/test/uiautomator/Configurator.html) timeouts e delays nos Android devices. Só funciona no Android API 18 e superior.

**"actionAcknowledgmentTimeout"** - Int que é o mesmo que [setActionAcknowledgmentTimeout](https://developer.android.com/reference/android/support/test/uiautomator/Configurator.html#setActionAcknowledgmentTimeout(long)). Se for dado um valor negativo, ele será definido com o padrão(3 * 1000 milliseconds).

**"keyInjectionDelay"** - Int que é o mesmo que [setKeyInjectionDelay](https://developer.android.com/reference/android/support/test/uiautomator/Configurator.html#setKeyInjectionDelay(long)). Se for dado um valor negativo, ele será definido com o padrão(0 milliseconds)

**"scrollAcknowledgmentTimeout"** - Int que é o mesmo que [setScrollAcknowledgmentTimeout](https://developer.android.com/reference/android/support/test/uiautomator/Configurator.html#setScrollAcknowledgmentTimeout(long)). Se for dado um valor negativo, ele será definido com o padrão(200 milliseconds)

**"waitForIdleTimeout"** - Int que é o mesmo que [setWaitForIdleTimeout](https://developer.android.com/reference/android/support/test/uiautomator/Configurator.html#setWaitForIdleTimeout(long)). Se for dado um valor negativo, ele será definido com o padrão(10 * 1000 milliseconds)

**"waitForSelectorTimeout"** - Int que é o mesmo que [setWaitForSelectorTimeout](https://developer.android.com/reference/android/support/test/uiautomator/Configurator.html#setWaitForSelectorTimeout(long)). Se for dado um valor negativo, ele será definido com o padrão(10 * 1000 milliseconds)

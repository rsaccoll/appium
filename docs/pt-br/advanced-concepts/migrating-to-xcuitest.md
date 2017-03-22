## Migrando seus Testes d iOS do UIAutomation (iOS 9.3 e posteriores) para XCUITest (iOS 9.3 e superiores)

Para automação no iOS, o Appium depende do framework fornecido pela Apple. Para iOS 9.2 e posteriores, a única tecnologia de automação fornecida pela Apple era chamado de UIAutomation, e funcionava no contexto de um processo chamado "Instruments". Apartir do iOS 10, a Apple removeu completamente o UIAutomation, isso impossibilitou o Appium de permitir da maneira que costumava pertmitir. Felizmente a Apple introduziu um novo framework chamado XCUITest, começando com o iOS 9.3. Para iOS 10 e superiores, está será o único framework suportado pela Apple.

Appium começou o suporte para o XCUITest na versão Appium 1.6. Para a maior parte, as capacidades do XCUITest correspondem com as do UIAutomation, e assim a equipe do Appium foi capaz de garantir que o comportamento dos testes permanecerão o mesmo. Isso é uma das coisas mais legais em usar Appium! Mesm com a Apple alterando completamente o framework dos seus testes, seus scripts podem ficar iguais (a grande maioria =) ). Dito isso, existem algumas diferenças que voce precisa estar ciente e podem exigir modificação nos seus scripts de teste, se quiser rodar com o XCUITest. Este documento irá ajudá-lo com essas diferenças.

### Element class name schema

Com o XCUITest, Apple deu diferentes nome de classes aos seus "UI Elements" que compõem a hierarquia da view. Por examplo, `UIAButton` agora é chamado de `XCUIElementTypeButton`. Em muitos casos, há um mapeamento entre essas duas classes. Se voce usa a estratégia de buscar o elemento por `class name`, Appium 1.6 irá reescrever o seletor para voce. Da mesma forma, se voce usa a estratégia de buscar o elemento por `xpath`, Appium 1.6 irá procurar quaisquer elementos `UIA*` e os reescreverá adequadamente.

Entretanto isso não garante que seus testes funcionem exatamente da mesma maneira por dois motivos:

1. A hierarquia do app encontrada pelo Appium não será, necessariamente, identica dentro do XCUITest da mesma forma que era apresentada no UIAutomation. Se a sua estratégia de busca de elementos for por xpath, talvez precise ser ajustado.
2. A lista de  class names não é inteiramente identica. Muitos elementos retornados pelo XCUITest como pertencentes a classe `XCUIElementTypeOther` , que é um tipo de container que "pega tudo" que não tenha definição.

### Codigo fonte da página

Como mencionado acima, se confiar no XML de origem do app pelo comando `page source`, o XML de retorno terá uma diferença significativa em relação ao do UIAutomation.

### `-ios uiautomation` locator strategy

Essa estratégia foi especialmente desenvolvida para UIAutomation, portanto não está incluída no XCUITest. Estamos trabalhando em uma nova estratégia similar "nativa" para os próximos lançamentos.

### `xpath` locator strategy

1. Tente não usar esse tipo de estratégia, a menos que não tenha nenhuma outra alternativa. Em geral, xpath locators podem ser mais lentos que outros tipo de locators, como accessibility id, class name e predicate (até 100 vezes mais lentos em alguns casos especiais). Eles são tão lentos, por que esse tipo de estratégia, xpath, não é suportada nativamente pelo framework XCTest.
2. Use

```
driver.findElement(x)
```

em vez de

```
driver.findElements(x)[0]
```

para procurar um único elemento por xpath. Quanto mais possibilidades encontradas na UI, mais lento ele é.
3. Seja muito detalhista ao procurar um elemento por xpath. Procurar como

```
//*
```

pode demorar minutos a ser concluído, dependendo do número de elementos na UI da sua aplicação (ex:

```
driver.findElement(By.xpath("//XCUIElementTypeButton[@value='blabla']"))
```

é mais rápido que

```
driver.findElement(By.xpath("//*[@value='blabla']"))
```

ou

```
driver.findElement(By.xpath("//XCUIElementTypeButton")))
```

4. Na maioria dos casos, seria mais rápido múltiplas chamadas, do tipo, findElement do que executar uma única chamada por xpath (ex:

```
driver.findElement(x).findElement(y)
```

é geralmente mais rápido do que

```
driver.findElement(z)

```

onde x e y não são xpath locators  e x é um xpath locator).

### Dependencias

Além das muitas "pegadinhas" que podem vir com a atualização do XCode (não relacionadas ao Appium), o suporte do appium no XCUITest requer uma nova dependencia: [Carthage](https://github.com/Carthage/Carthage). Appium Doctor já foi atualizado para garantir que o Carthage esteja no path.

### Diferenças na API

Infelizmente, a XCUITest API e UIAutomation API não são equivalentes. Em muitos casos(tipo `tap/click`), o comportamento é identico. Mas alguns recursos que estavam disponíveis no UIAutomation ainda não estão disponíveis no XCUITest. Esses recursos conhecidos faltantes incluem:
* Geolocation support (ex: `driver.location`)
* "Agitar" o device
* Bloquear o device
* Girar o device (no que esse  *NÃO* é o device _orientation_, que é suportado)

Nós nos esforçaremos para adicionar esses recursos em futuras versões do Appium.

#### Scrolling e clicking

No driver anterior (UIAutomation), se tentasse clicar em um elemento que não estivesse na view, UIAutomation "scrollava" para o elemento, automaticamente, e efetuava o clique. Com o XCUITest, este não é mais o caso. Agora voce é responsável por garantir que o seu elemento esteja visível antes de interagir com ele (da mesma forma que o usuário seria responsável pela mesma interação)

### Outros problemas conhecidos

Finalmente, uma lista de problemas já conhecidos com a versão 1.6 (vamos atacar os problemas que foram resolvidos)

* ~~Não é possível interagir com elementos nos devices no modo landscape (https://github.com/appium/appium/issues/6994)~~
* `shake` não está implementado devido a falta de suporte da Apple
* `lock` não está implementado devido a falta de suporte da Apple
* `geo-location` não está implementado devido a falta de suporte da Apple
* Através da TouchAction/MultiAction API, o gesto `zoom` funciona, mas o gesto `pinch` não, devido a um problema da Apple.
* Através da TouchAction/MultiAction API, o gesto `swipe` atualmente não é suportado, embora eles devem estar em breve.(https://github.com/appium/appium/issues/7573)
* As capacidades `autoAcceptAlerts` e `autoDismissAlerts` atualmente não funcionam, e há um debate contínuo se seremos capazes de implementá-los no futuro.
* Existe um problema no iOS SDK que `PickerWheels` usando certos métodos da API, não são automatizáveis pelo XCUITest. See https://github.com/appium/appium/issues/6962 para uma solução alternativa, garatindo que `PickerWheels` sejam construídos corretamente.

O tando que for possível, iremos adicionar os recurso faltantes e corrigir outros problemas conhecidos nessa versão do Appium.

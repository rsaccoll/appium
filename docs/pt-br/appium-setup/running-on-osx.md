## Rodando Appium no Mac OS X

Appium no OS X suporta testes para iOS e Android.

### System setup (iOS)

* Appium requer Mac OS X 10.10 ou superior.
* Certifique-se que tenha o Xcode e iOS SDK(s) instalados. Xcode versão 7.1 é
  recomendado como versões anteriores são limitadas para qual versão de iOS podem testar
  Veja a proxima sessão para mais detalhes.
* Voce precisa autorizar o uso do iOS simulator. Veja [abaixo](#autorizando-ios-no-seu-computador).
* Se estiver no XCode  7.x ou superior, o IWD (instruments without delay) não funciona.
  Voce pode ativar o IWD (o que acelerará significativamente os testes) usando [esse
  método](/docs/en/advanced-concepts/iwd_xcode7.md)
* Se voce estiver no Xcode 6, voce precisa lançar cada simulador que voce pretende usar 
  com appium com antecedencia, e alterar o padrão para mostrar o teclado virtual
  se voce quiser trabalhar com o sendKeys. Voce pode fazer isso clicando em qualquer campo de texto e apertando o command-K até que note o aparecimento do teclado.
* Se voce estiver no Xcode 6, existe um recurso chamado Devices
  (command-shift-2). Voce precisa ter certeza de que qualquer dispositivo que escolher, para usar com Appium, tenha apenas um daqueles por versão de SDK. Em outras palavras, se voce enviar um deviceName com "iPhone 5s" e
  um platformVersion de "8.0", voce precisará ter certeza de que exista exatamente um dispositivo
  com essas propriedades na usa lista.
  Caso contrário, Appium não saberá qual usar.
* No iOS 8, cada dispositivo tem sua própria configuração que habilita ou desabilita o
  UIAutomation. Ele fica no "Developer" view nos settings do app. Voce precisará verificar se UIAutomation está habilitada
  antes que o simulador ou dispositivo físico possa ser automatizado.

### Autorizando iOS no seu computador

Voce precisa autorizar o uso do iOS Simulador executando o `authorize-ios`,
disponibilzado no `npm`. Instale o programa executando o

```
npm install -g authorize-ios
```

E use ele executando o comando

```
sudo authorize-ios
```

Se voce estiver usando o [Appium.app](https://github.com/appium/appium-dot-app), voce pode autorizar o iOS através do GUI.

Voce precisa fazer isso sempre que instalar uma nova versão do XCode.

### Testando vários iOS SDK's 

A versão 7.1 do XCode permite testes automáticos nas versões do iOS 7.1 e superiores.

Se estiver usando várias versões do XCode, voce pode alternar elas usando o seguinte comando:

    sudo xcode-select --switch &lt;caminho do xcode&gt;

### Testando usando Xcode 8 (incluindo o iOS 10) com XCUITest

Para automatizar dispositivos iOS (que inclui todos os testes do iOS 10+),
voce precisa instalar um gerenciador de dependencia chamada [Carthage](https://github.com/Carthage/Carthage):

```
brew install carthage
```




### System setup (Android)

As instruções para configurar o Android e executar os testes no MacOS são as mesmas que as de linux. Veja em [Android setup docs](/docs/en/appium-setup/android-setup.md).

### Rodando testes iOS usando o Jenkins

Primeiro faça o download do jenkins-cli.jar e verifique se o mac conecta com sucesso o master. Verifique que executou o comando `authorize-ios` mencionado abaixo.

`wget https://jenkins.ci.cloudbees.com/jnlpJars/jenkins-cli.jar`

```
java -jar jenkins-cli.jar \
 -s https://team-appium.ci.cloudbees.com \
 -i ~/.ssh/id_rsa \
 on-premise-executor \
 -fsroot ~/jenkins \
 -labels osx \
 -name mac_appium
 ```

Em seguida defina um "LaunchAgent" para o jenkins iniciar automaticamente no login. Um "LaunchDaemon" não funcionará por que daemons não tem acesso ao GUI. Certifique-se que o  plist não contem `SessionCreate` ou `User` key pois isso pode impedir a execução dos testes. Voce verá o seguinte erro: `Failed to authorize rights` se houer algum problema na configuração.

```
$ sudo nano /Library/LaunchAgents/com.jenkins.ci.plist
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>com.jenkins.ci</string>
    <key>ProgramArguments</key>
    <array>
        <string>java</string>
        <string>-Djava.awt.headless=true</string>
        <string>-jar</string>
        <string>/Users/appium/jenkins/jenkins-cli.jar</string>
        <string>-s</string>
        <string>https://instructure.ci.cloudbees.com</string>
        <string>on-premise-executor</string>
        <string>-fsroot</string>
        <string>/Users/appium/jenkins</string>
        <string>-executors</string>
        <string>1</string>
        <string>-labels</string>
        <string>mac</string>
        <string>-name</string>
        <string>mac_appium</string>
        <string>-persistent</string>
    </array>
    <key>KeepAlive</key>
    <true/>
    <key>StandardOutPath</key>
    <string>/Users/appium/jenkins/stdout.log</string>
    <key>StandardErrorPath</key>
    <string>/Users/appium/jenkins/error.log</string>
</dict>
</plist>
```

Finalmente ajuste o owner, permissões, e inicie o agent.

```
sudo chown root:wheel /Library/LaunchAgents/com.jenkins.ci.plist
sudo chmod 644 /Library/LaunchAgents/com.jenkins.ci.plist

launchctl load /Library/LaunchAgents/com.jenkins.ci.plist
launchctl start com.jenkins.ci
```


### Arquivos gerados por testes no iOS

Testes no iOS geram arquivos que as vezes podem ocupar muito espaço. Estes incluem logs,
arquivos temporários, e dados derivados da execução do Xcode. Geralmente os seguintes locais onde eles estão são (caso precise excluir eles):

```
$HOME/Library/Logs/CoreSimulator/*
```

Para testes instrumentados (iOS _not_ using `XCUITest` como `automationName`):

```
/Library/Caches/com.apple.dt.instruments/*
```

Para testes baseados no XCUITest:

```
$HOME/Library/Developer/Xcode/DerivedData/*
```

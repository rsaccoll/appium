## Selenium Grid

Voce pode registrar o seu Appium server em um [Selenium grid](https://code.google.com/p/selenium/wiki/Grid2) ([setup docs](http://docs.seleniumhq.org/docs/07_selenium_grid.jsp)) usando o
`--nodeconfig` parametro.

```center
> appium --nodeconfig /path/to/nodeconfig.json
# ou, se está usando pelo código fonte:
> node . --nodeconfig /path/to/nodeconfig.json
```

No arquivo de configuração do node voce tem que definir o `browserName`,
`version` e `platform` e com base nesses parametros o grid irá redirecionar os testes para o dispositivo correto.
Voce também precisará
configurar seus **host details**  e o **selenium grid details** . Para
uma lista completa de todos os parametros e descrições veja
[aqui](http://code.google.com/p/selenium/source/browse/java/server/src/org/openqa/grid/common/defaults/GridParameters.properties)

Uma vez iniciado o Appium server e registrado no grid,
voce verá seu dispositivo na página do console grid:

"http://**\<grid-ip-adress\>**:**\<grid-port\>**/grid/console"

### Grid Node Configuration Example json file

```xml
{
  "capabilities":
      [
        {
          "browserName": "<e.g._iPhone5_or_iPad4>",
          "version":"<version_of_iOS_e.g._7.1>",
          "maxInstances": 1,
          "platform":"<platform_e.g._MAC_or_ANDROID>"
        }
      ],
  "configuration":
  {
    "cleanUpCycle":2000,
    "timeout":30000,
    "proxy": "org.openqa.grid.selenium.proxy.DefaultRemoteProxy",
    "url":"http://<host_name_appium_server_or_ip-address_appium_server>:<appium_port>/wd/hub",
    "host": <host_name_appium_server_or_ip-address_appium_server>,
    "port": <appium_port>,
    "maxSession": 1,
    "register": true,
    "registerCycle": 5000,
    "hubPort": <grid_port>,
    "hubHost": "<Grid_host_name_or_grid_ip-address>"
  }
}
```

Plataformas válidas são listadas [aqui](http://selenium.googlecode.com/git/docs/api/java/org/openqa/selenium/Platform.html)

Se `url`, `host`, e `port` não são fornecidas, a configuração será atualizada
automaticamente para apontar para "localhost:whatever-port-Appium-started-on".

Se o seu Appium server está rodando em uma máquina diferente de onde está o Selenium Grid server, certifique-se de usar um nome/endereço de IP nos seus `host` & `url` docs; `localhost` e `127.0.0.1` inpedirá que o Selenium Grid de se conectar corretamente.

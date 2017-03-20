## Configurando a instrumentação sem delay (iwd) para xcode 7 e iOS >= 9.0

Para iOS >= 9.0 instrumentação sem delay (iwd) não funciona apenas passando binários atráves de linha de comando (appium faz isso
por "debaixo dos panos" para xcode < 7). Veja [iwd](https://github.com/lawrencelomax/instruments-without-delay/tree/xcode7-quirks#xcode-7--ios-9-support)

Para ativar iwd para xcode >= 7,
- Check out [appium-instruments](https://github.com/appium/appium-instruments)
- Execute `xcode-iwd.sh` presente no caminho `<appium-instruments>/bin/`, com os argumentos indicados abaixo:

```
sh <appium-instruments>/bin/xcode-iwd.sh <caminho do xcode> <caminho do appium-instruments>
```
ex. `sh ./bin/xcode-iwd.sh /Applications/Xcode.app /Users/xyz/appium-instruments/`

Nota: iwd com Xcode 7 só funcionará para iOS >= 9.0, voce pode mudar para uma versão mais antiga do XCode para iOS < 9.0

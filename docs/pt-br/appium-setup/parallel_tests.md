## Testes em Android em Paralelo

Appium fornece uma maneira para que os usuários automatizem múltiplas sessões em uma única máquina. Tudo o que envolve é iniciar vários servidores do Appium com diferentes flags.

As flags mais importantes para automatizar são:

- `-p` Porta principal do Appium
- `-U` O id do  device
- `-bp` Porta bootstrap Appium
- `--chromedriver-port` A porta do chromedriver (se estiver usando webviews ou o chrome)
- `--selendroid-port` A porta do selendroid (se estiver usando selendroid)

Mais informações sobre essas flags podem ser encontradas [aqui](../writing-running-appium/caps.md).

Se tivessemos dois dispositivos com os respectivos ID's 43364 e 32456, iniciariamos dois servidores appium com os seguintes comandos:

`node . -p 4492 -bp 2251  -U 32456`

`node . -p 4491  -bp 2252 -U 43364`

Enquanto as portas do Appium e Appium bootstrap ports estão entre 0 e 65536, tudo o que elas precisam é ser diferentes, pois os "Appiums servers" irão tentar escutar na mesma porta. Assegure-se que a sua flag -u flag corresponde ao ID do device corretamente. É assim que o Appium sabe com qual dispositivo irá se comunicar, por isso a precisão.

Se estiver usando ochromedriver ou selendroid, defina uma porta diferente para cada servidor.

### Testes paralelos em iOS

Infelizmente não é possível executar testes em paralelo em testes locais. Ao contrário do Android, apenas um simulador de iOS pode ser inicializado cada vez, fazendo com que ele execute vários testes ao mesmo tempo.

Se voce deseja executar testes em paralelo, voce precisa usar Sauce. Basta fazer o upload dos seus testes para o Sauce, e ele pode executar tantos testes em paralelos para iOS ou Android que a sua conta permitir. Veja como executar seus testes no Sauce [aqui](https://docs.saucelabs.com/tutorials/appium/).


## iOS WebKit Debug Proxy

Para acessar as web views em dispositivos físicos, o appium para iOS, usa o [ios_webkit_debug_proxy](https://github.com/google/ios-webkit-debug-proxy).

### Instalação

#### Usando o Homebrew

Para instalar a versão mais recente do ios-webkit-debug-proxy usando o
Homebrew, execute os seguintes comandos via terminal:

 ``` center
 # O primeiro comando só é necessário senão tiver o brew instalado
 > ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
 > brew update
 > brew install ios-webkit-debug-proxy
 ```

#### Compilando ios-webkit-debug-proxy apartir do código fonte

Abra o seu terminal no mac. Voce pode achar informações sobre como abrir o terminal
através de seu mecanismo de busca. Assim que estiver aberto, verifique se
[Homebew](http://brew.sh/) installed:

```shell
$ brew -v
```

Quando confirmar que já tem instalado o Homebrew, execute os seguintes comandos (O símbolo '$' indica linha de
comando, não escreva ele):

```shell
$ cd  ~
$ sudo apt-get install autoconf automake libusb-dev libusb-1.0-0-dev libplist-dev libplist++-dev usbmuxd libtool libimobiledevice-dev
$ git clone https://github.com/google/ios-webkit-debug-proxy.git
$ cd ios-webkit-debug-proxy
$ ./autogen.sh
$ make
$ sudo make install
```

#### Executando ios-webkit-debug-proxy

Uma vez instalado, voce pode iniciar o proxy com o seguinte comando:

``` center
# Change the udid to be the udid of the attached device and make sure to set the port to 27753
# as that is the port the remote-debugger uses. You can learn how to retrieve the UDID from
# Apple's developer resources.
> ios_webkit_debug_proxy -c 0e4b2f612b65e98c1d07d22ee08678130d345429:27753 -d
```

Voce também pode usar o `ios-webkit-debug-proxy-launcher`, um pequeno script no código fonte do appium, para iniciar
o proxy. Ele monitora o log do proxy para erros, e reiniciar o proxy se necessário. Isso também é opcional e pode ajudar
dispositivos mais atuais:

``` center
# change the udid
# note, this is run from an Appium repository
> ./bin/ios-webkit-debug-proxy-launcher.js -c 0e4b2f612b65e98c1d07d22ee08678130d345429:27753 -d
```

**NOTA:** o proxy requer que o **"web inspector"** seja ativado para 
permitir estabelecer uma conexão. Para ativar, basta ir em **settings >
safari > advanced**. Lembre-se que o web inspector foi adicionado como parte do iOS 6 e não estava disponível anteriormente.

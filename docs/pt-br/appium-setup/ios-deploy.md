## Instalando um iOS app em um dispositivo físico

Para preparar os testes para serem executados em um dispositivo físico, voce precisará:

1. 'Buildar' seu app com específicos paramestros para o device em teste.
2. Use o [ideviceinstaller](https://github.com/libimobiledevice/ideviceinstaller), um 3rd-party tool,
 para fazer deploy esse build no seu dispositivo.

### Xcodebuild com parametros:
Um novo xcodebuild agora permite que as configurações sejam especificadas. Tirado do [developer.apple.com](https://developer.apple.com/library/mac/#documentation/Darwin/Reference/ManPages/man1/xcodebuild.1.html):

```center
xcodebuild [-project projectname] [-target targetname ...]
             [-configuration configurationname] [-sdk [sdkfullpath | sdkname]]
             [buildaction ...] [setting=value ...] [-userdefault=value ...]
```

Este é um recurso para explorar em [settings](https://developer.apple.com/library/mac/#documentation/DeveloperTools/Reference/XcodeBuildSettingRef/1-Build_Setting_Reference/build_setting_ref.html#//apple_ref/doc/uid/TP40003931-CH3-DontLinkElementID_10)

```center
CODE_SIGN_IDENTITY (Code Signing Identity)
    Description: Identifier. Specifies the name of a code signing identity.
    Example value: iPhone Developer
```

PROVISIONING_PROFILE está ausente no índice de comandos disponíveis,
mas pode ser necessário.

Especifique as configurações "CODE_SIGN_IDENTITY" & "PROVISIONING_PROFILE" no
xcodebuild command:

```center
xcodebuild -sdk <iphoneos> -target <target_name> -configuration <Debug> CODE_SIGN_IDENTITY="iPhone Developer: Mister Smith" PROVISIONING_PROFILE="XXXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXX"
```

Em caso de sucesso, o app estará ```<app_dir>/build/<configuration>-iphoneos/<app_name>.app```

### Deploy using ideviceinstaller

Para instalar a versão mais atual do ideviceinstaller usando o
Homebrew, execute os seguintes comandos no terminal:

 ``` center
 # O primeiro comando só é necessário se você não tiver o homebrew instalado.
 > ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
 > brew update
 > brew install ideviceinstaller
 > ideviceinstaller -u <UDID of device> -i <path of .app/.ipa>
 ```

Proximo: [Running Appium on Real Devices](real-devices.md)

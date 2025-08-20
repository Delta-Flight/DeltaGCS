# DeltaFlight Ground Control Station

O Software GCS DeltaFlight é uma ferramenta de configuração multiplataforma para o sistema de controle de voo [DeltaFlightControl](https://github.com/Delta-Flight/DeltaFC). Desenvolvido por estudantes do projeto de extensão [DeltaV Drones](deltavquad.github.io) da Escola Politécnica da Universidade de Pernambuco.

## Instalação

Os downloads para Windows, Linux, Mac and Android estão disponíveis na seção de [Releases do projeto.](https://github.com/Delta-Flight/DeltaGCS/releases)

* Windows: A versão mínima do Windows necessária é a Windows 8.

* MacOS X: Para contornar a mensagem de erro "DeltaFlight.app está danificado e não pode ser aberto", execute o seguinte comando no terminal após a instalação: `sudo xattr -rd com.apple.quarantine /Applications/DeltaFlight\ DeltaFlight.app`.

* Linux: Para conceder acesso às interfaces seriais, execute o comando `sudo usermod -aG dialout ${USER}` e reinicie a sessão. Para evitar erros de pós-instalação, certifique-se de que o diretório `/usr/share/desktop-directories` exista, executando `sudo mkdir /usr/share/desktop-directories/` antes da instalação. A biblioteca libatomic também deve ser instalada. Em Debian ou Ubuntu, use `sudo apt install libatomic1`.

* Problemas Gráficos: Se houver problemas de exibição gráfica, tente iniciar o executável com a opção --disable-gpu. Também pode ser útil desativar o antialiasing no driver da sua placa de vídeo.

## Compilação via NW.js (windows/linux/macos) ou Cordova (android)

> Se for desenvolver para Android, instale o Java JDK 8, Gradle e Android Studio (SDK Nível 19 ou superior). No Windows, é necessário configurar as variáveis de ambiente `ANDROID_HOME`, `ANDROID_SDK_ROOT` e atualizar a variável `Path` para incluir os caminhos do SDK e do Gradle.
> | Variável | Valor |
> |---|---|
> | ANDROID_HOME | %LOCALAPPDATA%\Android\sdk |
> | ANDROID_SDK_ROOT | %LOCALAPPDATA%\Android\sdk |
> | Path | %ANDROID_HOME%\tools<br>%ANDROID_HOME%\platform-tools<br>C:\Gradle\bin |

> **Instalação do yarn:**  
> Instale o NVM para controle de versão [aqui.](https://github.com/coreybutler/nvm-windows/releases/tag/1.2.2)  
> Reinicie o terminal e execute `nvm install 20`  
> Execute `nvm use 20`  
> Instale o Yarn com `npm install -g yarn`  

As tarefas de compilação são definidas no arquivo `gulpfile.js` e podem ser executadas com:
```
yarn gulp <tarefa> [plataforma]
```

Lista dos possíveis valores para `<tarefa>`:
* **dist** Copia arquivos JS e CSS para a pasta `./dist`. Para Android, os arquivos são copiados para `./dist_cordova`.
* **apps** Compila os aplicativos na pasta `./apps`.
* **debug** Compila versões de depuração dos aplicativos na pasta `./debug`.
* **release** Compacta os aplicativos em arquivos individuais na pasta `./release`.

Para compilar esse projeto no macOS ou Linux, você vai precisar de Wine. Para Android, você vai precisar configurar um emulador ou conectar um dispositivo Android com depuração USB habilitada

As plataformas disponíveis são:

* **MacOS X** use `yarn gulp <task-name> --osx64`
* **Linux** use `yarn gulp <task-name> --linux64`
* **Windows** use `yarn gulp <task-name> --win64`
* **Android** use `yarn gulp <task-name> --android`

**Note:** Não use suporte multi-plataformas. Faça uma distribuição para cada plataforma para evitar erros

## **Instalar o NVM para Windows**
NVM = Node Version Manager (Gerenciador de Versões para Node JS)

Link de instalação: https://github.com/coreybutler/nvm-windows/releases

## Listar as versões do Node JS disponíveis para instalação

```bash
nvm list available
```

## Instalar o Node JS com NVM

```bash
nvm install 14.19.1
sudo nvm use 14.19.1
```

## Listar as versões locais instaladas do Node JS

```bash
nvm list
```

## Usar uma versão local diferente

```bash
sudo nvm use 14.19.1
```

## Desinstalar uma versão

```bash
nvm uninstall 12.22.12

# Para desinstalar a versão atual
sudo nvm uninstall 14.19.1
```

## **Install Yarn with Winget**

Packages:

```bash
winget install Yarn.Yarn --source winget
```

## Instalar o Chocolatey

Execute no seu **Powershell** em modo **Administrador** o comando abaixo:

```bash
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```

## Instalar o pacote sudo

Execute no seu **Powershell** em modo **Administrador** o comando abaixo:

https://www.freecodecamp.org/news/nvm-for-windows-how-to-download-and-install-node-version-manager-in-windows-10/

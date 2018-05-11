# Liferay Front End Developer – Primeiros Passos

## 1. Preparação de Ambiente
Será apresentado um tutorial para preparação de ambiente e primeiros passos na plataforma Liferay para Desenvolvedores Front End, com base no treinamento oficial Liferay e mais algumas informações fornecidas pela equipe da Liferay USA.
Todos os passos foram executados em ambiente Linux, distribuição Ubuntu 16.04.

### 1.1 Java JDK
Para a execução do servidor local do Liferay, é preciso instalar a versão 1.8 do Java Development Kit (JDK) e acrescentar o endereço da pasta /bin do JDK no início da variável PATH (através do arquivo ~/.bashrc).
Também é preciso definir as variáveis de ambiente JAVA_HOME e JDK_HOME, com o endereço de instalação do JDK, sem a pasta bin.

No final do arquivo .bashrc, foram inseridas as linhas:

```bash
export JAVA_HOME=/path/to/jdk (substituir)
export JDK_HOME=$JAVA_HOME
export PATH="$JDK_HOME/bin:$PATH"
```

### 1.2  MySQL 5.6
É preciso também fazer a instalação do MySQL 5.6, embora a Plataforma do Liferay utilize por padrão o HSQL (Hyper SQL), eles não o recomendam para ambiente de produção.
A instalação pode ser feita através desses comandos.

```bash
sudo add-apt-repository 'deb http://archive.ubuntu.com/ubuntu trusty universe'
sudo apt-get update
sudo apt install mysql-server-5.6
sudo apt install mysql-client-5.6
```

### 1.3 Node.JS e NPM
Para muitas tarefas no Front End, é indispensável o Node.JS, para o Liferay não é diferente, é utilizada a plataforma Node para possibilitar a criação e teste de novos temas.
Utilizaremos o NVM para instalar o Node, pois é uma maneira de permitir instalar múltiplas versões do Node, facilitando o teste em outras versões e retorno à versões anteriores quando necessário.
Para instalar o Node.JS no Ubuntu, foram utilizados os seguintes comandos:

```bash
sudo apt-get update
sudo apt-get install build-essential libssl-dev
curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh -o install_nvm.sh
bash install_nvm.sh
source ~/.profile
```

Para listar as versões disponíveis do Node para instalação, como exemplo estou utilizando a versão 8.11.1, mais recente e estável disponível no momento desta redação:

```bash
nvm ls-remote
nvm install –delete-prefix v8.11.1 (exemplo)
nvm use v8.11.1 (exemplo)
```

### 1.4 Yeoman
O Yeoman é um ambiente para geração de código executado em Node.JS que permite utilizar plugins para criar inícios rápidos para várias finalidades, como por exemplo temas para o Liferay.
Para instalar o Yeoman é utilizado o npm:

```bash
npm install -g yo
```

### 1.5 Gulp
O Gulp é uma ferramenta para automação de tarefas do front end, como importação de dependências, lint de código, watchers e etc. O Liferay possui um conjunto de tarefas do Gulp para facilitar o processo de desenvolvimento.

```bash
npm install -g gulp
```

### 1.6 Gerador de código
Para geração dos códigos do Liferay é utilizado um plugin instalado via npm, que será utilizado pelo Yeoman.

```bash
npm install -g generator-liferay-theme@7.2.0
```

### 1.7 Liferay + Tomcat Bundle
A plataforma Liferay pode ser executada localmente através do bundle disponibilizado pela Liferay neste [endereço](https://www.liferay.com/downloads?_ga=2.140026997.754221885.1523906791-1737328940.1521481680) (Liferay Portal CE bundled with Tomcat). Ela já possui um servidor Tomcat embutido, basta baixar e descompactar em algum local conhecido, o manual sugere separar os bundles dentro da pasta ~/liferay/bundles/.
Para iniciar o servidor Tomcat executando o portal do Liferay, abra o local onde foi descompactado o bundle, dentro da pasta Tomcat (acrescido da versão), dentro da pasta bin, e executar o comando:

```bash
computador:~/liferay/bundles/ce/tomcat-[versao]/bin$ ./startup.sh
```

Os logs da plataforma são externados no arquivo ~/liferay-bundle/tomcat-[versao]/logs/catalina.out, portanto para acompanhar os logs em tempo real, é utilizado o comando:

```bash
computador:~/liferay/bundles/ce/tomcat-[versao]/logs$ tail -f catalina.out
```

O servidor leva alguns segundos para inicializar, quando conclui o processo de inicialização uma aba do navegador é lançada com o portal da Liferay aberto na página de configuração inicial, onde é possível definir a conta de usuário administrador, o nome do site principal e escolher o banco de dados padrão.
Para encerrar a execução do servidor Tomcat é executado o seguinte script na pasta bin do Tomcat:

```bash
computador:~/liferay/bundles/ce/tomcat-[versao]/bin$ ./shutdown.sh
```

Assim como o startup.sh, o shutdown também leva alguns segundos para ser executado, porém ocorre em segundo plano, então não precisa ser mantido nenhuma janela aberta para que ele seja concluído.

## 2. Fundamentos
O Liferay é uma plataforma para desenvolvimento de sites e sistemas para a web, dispositivos móveis e outros dispositivos que estejam conectados à internet. Possui fácil customização utilizando a técnica WYSIWYG (What you see is what you get), os componentes podem ser simplesmente arrastados para a tela e da forma como são exibidos ali é como serão apresentados aos usuários. As regras de negócio podem ser configuradas diretamente no editor, pois cada componente possui suas características e podem ser personalizadas facilmente para atender os mais variados critérios.
A plataforma também permite gerenciar contas de usuários e permissões, gerenciamento de mídias e conteúdos em geral.

## 3. Ferramentas



## 4. Componentes do Front End

## 5. Utilidades

  * Taglibs. Sumário e documentação: [https://docs.liferay.com/ce/portal/7.0-latest/taglibs/util-taglib](https://docs.liferay.com/ce/portal/7.0-latest/taglibs/util-taglib/)

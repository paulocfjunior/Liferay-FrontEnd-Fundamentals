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

---
---

#Yeshua
---

## Temas

Temas são pacotes construidos para customizar o layout geral da pagina, como _header_ e _footer_, o menu de navegação, posicionamento dos _portlets_ entre outros.

O _Liferay Theme Generator_ pode ajudar com o inicio de um novo tema e tambem com outros detalhes relacionado ao mesmo, como layouts e esquemas de cores entre outros.

### Criando um novo tema

Na pasta `themes/` do projeto, utilize o comando:

```bash
$ yo liferay-theme
```

Logo após as definições das configurações, o yeoman criará uma estrutura inicial para o tema (talvez ele precise de privilegios de administrador como `sudo`), e logo em seguida ira rodar o `npm install` para instalar e as dependencias necesárias.

Ao entrar na pasta, será encontrado a seguinte estrutura:

```
my-theme
|- node_modules                                     // Pasta com as dependencias instaladas
|- src                                              // Principais arquivos do tema
|   |
|   |- css                                          // Pasta com o CSS
|   |- WEB_INF                                      // Arquivos para o sistema da Liferay
|       |
|       |- liferay-look-and-feel.xml                // Arquivo com as configurações de visualização
|       |- liferay-plugin-package.properties        // Arquivo com detalhes para visualização, como nome e descrição
|
|- gulpfile.js                                      // Arquivo com as tasks do gulp
|- liferay-theme.json                               // Detalhes do tema, como ID, URL para deploy, entre outros
|- package.json                                     // Detalhes das dependencias
```


## ADTs

_Application Display Templates_ ou ADTs, são templates que permitem a customização dos _portlets_, são templates em _Freemarker_(.ftl), com classes e estrutura personalizada. 

O CSS vem do tema a partir de classes, ou um estilo inline que pode ser colocado no _portlet_ pelo portal.

Os _portlets_ que suportam ADTs são:
    - Asset Categories Navigation;
    - Asset Publisher;
    - Asset Tags Navigation;
    - Blogs;
    - Media Gallery;
    - RSS;
    - Breadcrumb;
    - Language;
    - Navigation Menu;
    - SiteMap;
    - e Wiki.

Cada _portlet_ tem um ADT especifico, com algumas predefinições e chamadas prontas para facilitar a customização do mesmo.

## Web Content Templates & Asset Displays

## Componentes Liferay UI

OS ADTs (Application Display Templates) e os Templates do Web Content em Freemarker(.ftl)tem um suporte completo as taglibs de UI e Utils, somente usando a seguinte tag:
```html
<@liferay_ui['propriedade']
    param="valor-do-parametro"
/>
```

Por exemplo: Se voce quiser criar um _User Display_, você só precisa de
```html
<@liferay_ui["user-display"]
    markupView="lexicon"
    showUserDetails=false
    showUserName=true
    userId=userId
    userName=userName
/>
```

Tambem é possivel utilizar outros elementos como os icones do Lexicon:
```html
<@liferay_ui["icon"] 
    icon="name-of-the-icon"
    markupView="lexicon"
    message="An message popup"
    cssClass="classe-adicional"
/>
```

>Você pode trocar os tipos dos icones, substituindo o `markupView` por Glyphicon, ou Font-Awesome
>
>Para uma referencia completa dos icones, acesse o link [Lexicon Icons](https://lexiconcss.wedeploy.io/content/icons/)

Você pode ver uma lista completa com todas as tags disponiveis e seus parametros no seguinte link:
[https://docs.liferay.com/ce/portal/7.0-latest/taglibs/util-taglib/](https://docs.liferay.com/ce/portal/7.0-latest/taglibs/util-taglib/)

## Definindo Opções para o Portlet

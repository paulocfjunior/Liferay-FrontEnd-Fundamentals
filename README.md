# Liferay Front End Developer – Primeiros Passos

## Sumário

1. [Preparação de Ambiente](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#1-prepara%C3%A7%C3%A3o-de-ambiente)
    1. [Java JDK](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#11-java-jdk)
    2. [MySQL 5.6](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#12--mysql-56)
    3. [Node.JS e NPM](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#13-nodejs-e-npm)
    4. [Yeoman](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#14-yeoman)
    5. [Gulp](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#15-gulp)
    6. [Gerador de código](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#16-gerador-de-c%C3%B3digo)
    7. [Liferay + Tomcat Bundle](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#17-liferay--tomcat-bundle)
2. [Fundamentos](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#2-fundamentos)
3. [Ferramentas](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#3-ferramentas)
4. [Componentes do Front End](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#4-componentes-do-front-end)
5. [Utilidades](https://github.com/paulocfjunior/Liferay-FrontEnd-Fundamentals#5-utilidades)

------------

## 1. Preparação de Ambiente
Será apresentado um tutorial para preparação de ambiente e primeiros passos na plataforma Liferay para Desenvolvedores Front End, com base no treinamento oficial Liferay e mais algumas informações fornecidas pela equipe da Liferay USA.
Todos os passos foram executados em ambiente Linux, distribuição Ubuntu 16.04.

### 1.1 Java JDK
Para a execução do servidor local do Liferay, é preciso instalar a versão 1.8 do Java Development Kit (JDK) e acrescentar o endereço da pasta `/bin` do JDK no início da variável PATH (através do arquivo `~/.bashrc`).
Também é preciso definir as variáveis de ambiente `JAVA_HOME` e `JDK_HOME`, com o endereço de instalação do JDK, sem a pasta bin.

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

Para mais detalhes sobre, confira a sessão [referente](#32-theme-generator)

### 1.7 Liferay + Tomcat Bundle
A plataforma Liferay pode ser executada localmente através do bundle disponibilizado pela Liferay neste [endereço](https://www.liferay.com/downloads?_ga=2.140026997.754221885.1523906791-1737328940.1521481680) (Liferay Portal CE bundled with Tomcat). Ela já possui um servidor Tomcat embutido, basta baixar e descompactar em algum local conhecido, o manual sugere separar os bundles dentro da pasta `~/liferay/bundles/`.
Para iniciar o servidor Tomcat executando o portal do Liferay, abra o local onde foi descompactado o bundle, dentro da pasta Tomcat (acrescido da versão), dentro da pasta bin, e executar o comando:

```bash
computador:~/liferay/bundles/ce/tomcat-[versao]/bin$ ./startup.sh
```

Os logs da plataforma são externados no arquivo `~/liferay-bundle/tomcat-[versao]/logs/catalina.out`, portanto para acompanhar os logs em tempo real, é utilizado o comando:

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

### 3.1 Blade CLI
Para facilitar a criação de módulos, serviços ou qualquer outra estrutura do Liferay 7.0, pode ser utilizado o Blade CLI, que é uma ferramenta de linha de comando baseado em um ambiente Gradle. Ele possui subcomandos que auxiliam na criação e deploy de módulos em uma instância do Liferay. Esta ferramenta está embutida no Workspace da Liferay, e pode ser instalado seguindo as instruções da [Documentação oficial da Liferay - Instalando Blade CLI](https://dev.liferay.com/pt/develop/tutorials/-/knowledge_base/7-0/installing-blade-cli). Para listar os comandos do Blade CLI com as suas respectivas funções, basta executar o comando `blade help`, serão listados os seguintes comandos:

```
    create          Creates a new Liferay module project from several available
    convert         Converts a plugins-sdk plugin project to a gradle WAR project
    deploy          Builds and deploys bundles to the Liferay module framework.
    gw              Execute gradle command using the gradle wrapper if detected
    help            Get help on a specific command
    init            Initializes a new Liferay workspace
    install         Installs a bundle into Liferay module framework.
    open            Opens or imports a file or project in Liferay IDE.
    outputs
    samples         Generate a sample project
    server start    Start server defined by your Liferay project
    server stop     Stop server defined by your Liferay project
    sh              Connects to Liferay and executes gogo command and returns output.
    update          Update blade to latest version
    upgradeProps    Helps to upgrade portal properties from Liferay server
    version         Show version information about blade
```

O Blade se baseia em templates para criar os módulos, existem vários templates disponíveis para utilização, o que faz com que o desenvolvedor tenha bastante flexibilidade ao usá-lo. Os templates dispoíveis são mostrados pelo comando `blade create -l` e este é o retorno do comando:

```
    activator                          Creates a Liferay module project that customizes the starting and stopping of a Liferay bundle.
    api                                Creates a Liferay API module project with an empty public interface.
    content-targeting-report           Creates a Liferay Audience Targeting report as a module project.
    content-targeting-rule             Creates a Liferay Audience Targeting rule as a module project.
    content-targeting-tracking-action  Creates a Liferay Audience Targeting metric as a module project.
    control-menu-entry                 Creates a Liferay module project that customizes Liferay Portal's Control Menu.
    form-field                         Creates a Liferay form field module project using the Soy templating language.
    fragment                           Creates a Liferay fragment module project that customizes existing Liferay modules.
    freemarker-portlet                 Creates a FreeMarker portlet as a module project.
    layout-template                    Creates a Liferay layout template module project.
    mvc-portlet                        Creates a Liferay MVC portlet as a module project.
    npm-angular-portlet                Creates a Liferay MVC portlet with npm and Angular support as a module project.
    npm-billboardjs-portlet            Creates a Liferay MVC portlet with npm and Billboard.js support as a module project.
    npm-isomorphic-portlet             Creates a Liferay MVC portlet with npm and isomorphic code support as a module project.
    npm-jquery-portlet                 Creates a Liferay MVC portlet with npm and jQuery support as a module project.
    npm-metaljs-portlet                Creates a Liferay MVC portlet with npm and Metal.js support as a module project.
    npm-portlet                        Creates a Liferay MVC portlet with npm support as a module project.
    npm-react-portlet                  Creates a Liferay MVC portlet with npm and React support as a module project.
    npm-vuejs-portlet                  Creates a Liferay MVC portlet with npm and Vue.js support as a module project.
    panel-app                          Creates a Liferay panel app that customizes a panel category (e.g., Control Panel) by inserting an entry that gives access to an application.
    portlet                            Creates a Liferay portlet extending the "javax.portlet.GenericPortlet" class as a module project.
    portlet-configuration-icon         Creates a Liferay module project that customizes a Liferay portlet's configuration icon.
    portlet-provider                   Creates a Liferay module project that finds appropriate portlets to manage requests.
    portlet-toolbar-contributor        Creates a Liferay module project that customizes a Liferay portlet's toolbar.
    rest                               Creates a Liferay JAX-RS module project.
    service                            Creates a Liferay OSGi service module project implementing a chosen interface.
    service-builder                    Creates a Liferay Service Builder project by generating an API and implementation module.
    service-wrapper                    Creates a Liferay service wrapper module project extending a chosen service wrapper class.
    simulation-panel-entry             Creates a Liferay panel app module project that customizes Liferay Portal's Simulation Menu.
    soy-portlet                        Creates a Liferay Soy portlet as a module project.
    spring-mvc-portlet                 Creates a Spring MVC portlet as a WAR project.
    template-context-contributor       Creates a Liferay module project that injects custom non-JSP template variables into Liferay Portal.
    theme                              Creates a Liferay WAR-style theme project.
    theme-contributor                  Creates a Liferay module project that packages UI resources (e.g., CSS and JS) independent of a theme to include on a Liferay Portal page.
    war-hook                           Creates a Liferay WAR-style Hook project.
    war-mvc-portlet                    Creates a Liferay WAR-style MVC portlet project.
```

Using Blade CLI gives you the flexibility to choose how you want to create your application. You can do so in your own standalone environment, or within a Liferay Workspace. You can also create a project using either the Gradle or Maven build tool. Creating Liferay modules in a workspace using Blade CLI is very similar to creating them in a standalone environment.

When creating projects in a workspace, you should navigate to the appropriate folder corresponding to that type of project (e.g., the /modules folder for a module project). You can also provide further directory nesting into that folder, if preferred. For example, the Gradle workspace, by default, sets the directory where your modules should be stored by setting the following property in the workspace’s gradle.properties file:

Ao criar projetos em um workspace Liferay, você deve criá-los na pasta apropriada dentro da estrutura, por exemplo, se for criar um tema com o comando `blade create -t theme meu-novo-tema`, deve ficar dentro da pasta `/themes`.

### 3.2 Liferay Theme Generator

O _Liferay Theme Generator_ pode ajudar com o inicio de um novo tema e tambem com outros detalhes relacionado ao mesmo, como layouts e esquemas de cores entre outros.

Temas são pacotes construidos para customizar o layout geral da pagina, como _header_ e _footer_, o menu de navegação, posicionamento dos _portlets_ entre outros.

#### 3.2.1 Criando um novo Tema

Na pasta `themes/` do projeto, utilize o comando:

```bash
$ yo liferay-theme
```

Logo após as definições das configurações, o yeoman criará uma estrutura inicial para o tema (talvez ele precise de privilegios de administrador como `sudo`), e logo em seguida ira rodar o `npm install` para instalar e as dependencias necesárias, criando a seguinte estrutura:
```
my-theme
|- node_modules                                     // Pasta com as dependencias instaladas
|- build                                            // Build do tema (pasta criada somente após o primeiro build)
|- src                                              // Principais arquivos do tema
|   |
|   |- css                                          // Custom CSS 
|   |- js                                           // Custom JS
|   |- WEB_INF                                      // Arquivos para o sistema da Liferay
|       |
|       |- liferay-look-and-feel.xml                // Arquivo com as configurações de visualização
|       |- liferay-plugin-package.properties        // Arquivo com detalhes para visualização, como nome e descrição
|
|- gulpfile.js                                      // Arquivo com as tasks do gulp
|- liferay-theme.json                               // Detalhes do tema, como ID, URL para deploy, entre outros
|- package.json                                     // Detalhes das dependencias
```

Os comandos de _deploy_ e _build_ do tema, são feitos atravez do _gulp_
```bash
$ gulp build        //Faz o build do tema e cria os componentes na pasta my-theme/build
$ gulp deploy       //Faz o build e o deploy do tema para o servidor
$ gulp watch        //Faz o deploy e continua rodando aplicando alterações encontradas diretamente
```

#### 3.2.2 Alterando a estrutura da página

Aṕos criar um tema, faça o primeiro build com o gulp. Assim será criada a pasta `build` dentro do seu tema. Dentro dela voce encontrará a pasta `templates`  dentro dela voce encontra a estrutura base do Tema.
```
templates
|- init.ftl                     // Configurações iniciais das variaveis do FTL (não recomendado alterar esse arquivo)
|- init_custom.ftl              // Utilizado para alterar as configurações iniciais do init.ftl
|- navigation.ftl               // Template do menu de navegação do portal
|- portal_normal.ftl            // Estrutura inicial do portal
|- portal_pop_up.ftl            // Template das popups do portal
|- portlet.ftl                  // Template da estrutura que envolve os portlets
```

Copie e cole o componente que voce quer alterar na sua pasta `src` e assim o gulp substituira o arquivo ao executar o _build_.

#### 3.2.3 Criando novos layouts para o portal

Ao criar um site ou uma página no portal Liferay, você precisa definir um layout para o posicionamento dos portlets
[/images/3-2-3-layout-default.png]

Mas é possivel criar um novo layout caso necessário, com o _Liferay Theme Generator_ e o comando

```bash
$ yo liferay-theme:layout
```

E seguir as instruções do gerador, como o escolha de nome, id e quantas colunas e linhas o layout vai ter.
>Lembrando que é utilizado o sistema do bootstrap de 12 colunas.

Ao terminar de definir as opções, o sistema criara um arquivo `.tpl` com a estrutura do seu layout, e uma imagem de mesmo nome para usar como icone
>Se o gerador for usado em uma pasta com um tema criado pelo mesmo, ele criará os arquivos na pasta `src/layouttpl`

## 4. Componentes do Front End

### 4.1 ADTs

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

### 4.2 Web Content Templates

### 4.3 Componentes Liferay UI

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


## 5. Utilidades

  * Taglibs. Sumário e documentação: [https://docs.liferay.com/ce/portal/7.0-latest/taglibs/util-taglib](https://docs.liferay.com/ce/portal/7.0-latest/taglibs/util-taglib/)
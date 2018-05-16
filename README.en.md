# Liferay Front End Developer – Primeiros Passos

## Sumário

1. [Environment Setup](#1-environment-setup)
    1. [Java JDK](#11-java-jdk)
    2. [MySQL 5.6](#12-mysql-56)
    3. [Node.JS and NPM](#13-nodejs-and-npm)
    4. [Yeoman](#14-yeoman)
    5. [Gulp](#15-gulp)
    6. [Code Generator](#16-code-generator)
    7. [Liferay + Tomcat Bundle](#17-liferay--tomcat-bundle)
2. [Fundamentals](#2-fundamentals)
3. [Tools](#3-tools)
    1. [Blade CLI](#31-blade-cli)
    2. [Liferay Theme Generator](#32-liferay-theme-generator)
        1. [Creating a new theme](#321-creating-a-new-theme)
        2. [Customizing page structure](#322-customizing-page-structure)
        3. [Creating new layouts](#323-creating-new-layouts)
        4. [Creating themelets](#324-creating-themelets)
        5. [Importing themes](#325-importing-themes)
4. [Front End components](#4-front-end-components)
    1. [Languages used](#41-languages-used)
        1. [HTML & structure](#411-html--structure)
            - [FreeMarker](#freemarker)
            - [Soy Templates (Google Closure)](#soy-templates-google-closure)
        2. [CSS](#412-css)
        3. [JavaScript](#413-javascript)
    2. [ADTs](#42-adts)
    3. [Web Content Structures & Templates](#43-web-content-structures--templates)
        1. [Web Content Structures](#431-web-content-structures)
        2. [Web Content Templates](#432-web-content-templates)
    4. [Liferay UI Components](#44-liferay-ui-components)
    5. [Gulp Tasks](#45-gulp-tasks)
        1. [Gulp build](#451-gulp-build)
        2. [Gulp deploy](#452-gulp-deploy)
        3. [Gulp watch](#453-gulp-watch)
        4. [Gulp init](#454-gulp-init)
        5. [Gulp extend](#455-gulp-extend)
        6. [Gulp status](#456-gulp-status)
5. [Util](#5-util)
6. [Liferay Certification](#6-liferay-certification)
    1. [Liferay 6.2 Certified Professional Developer](#61-liferay-62-certified-professional-developer)
    2. [Liferay DXP Certified Professional Front-End Developer](#62-liferay-dxp-certified-professional-front-end-developer)
    3. [Liferay DXP Certified Professional Back-End Developer](#63-liferay-dxp-certified-professional-back-end-developer)
7. [Code snippets](#7-code-snippets)
    - [Blog / Web content](#blog--web-content)

-----

## 1. Environment Setup
A tutorial will be presented for the environment setup and first steps on the Liferay platform for Front End Developers, based on the official Liferay training and some information provided by the Liferay USA team.
All steps were run in Linux environment, distribution Ubuntu 16.04.

### 1.1 Java JDK
To run the local Liferay server, you must install version 1.8 of the Java Development Kit (JDK) and append the address of the `bin` folder of the JDK at the beginning of the `PATH` variable (through the` ~ / .bashrc` file).
You also need to set the `JAVA_HOME` and` JDK_HOME` environment variables, with the JDK installation address, without the bin folder.

At the end of the `.bashrc` file, the following lines were inserted:

```bash
export JAVA_HOME=/path/to/jdk (replace)
export JDK_HOME=$JAVA_HOME
export PATH="$JDK_HOME/bin:$PATH"
```

### 1.2 MySQL 5.6
It is necessary to install MySQL 5.6, although the Liferay Platform uses by default the HSQL (Hyper SQL), it is not recommended for production.
Installation can be done through these commands.

```bash
sudo add-apt-repository 'deb http://archive.ubuntu.com/ubuntu trusty universe'
sudo apt-get update
sudo apt install mysql-server-5.6
sudo apt install mysql-client-5.6
```

### 1.3 Node.JS and NPM
For many tasks in the Front End, it is indispensable Node.JS, for Liferay is no different, the Node platform is used to enable the creation and testing of new themes.
We will use NVM to install Node, as it is a way to allow multiple versions of the Node to be installed, making it easier to test in other versions and return to previous versions when necessary.
To install Node.JS in Ubuntu, the following commands were used:

```bash
sudo apt-get update
sudo apt-get install build-essential libssl-dev
curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.31.0/install.sh -o install_nvm.sh
bash install_nvm.sh
source ~/.profile
```

To list the available versions of the Node for installation, use the `nvm ls-remote` command, the latest and stable version available at the time of this writing is 8.11.1, so to do the installation, use the commands:

```bash
nvm ls-remote
nvm install –delete-prefix v8.11.1
nvm use v8.11.1
```

### 1.4 Yeoman
Yeoman is an environment for code generation run in Node.JS that allows you to use plugins to create quick starts for various purposes, such as themes for Liferay.
To install Yeoman the npm is used:

```bash
npm install -g yo
```

### 1.5 Gulp
Gulp is a tool for automating front end tasks, such as importing dependencies, code lint, watchers and so on. Liferay has a set of Gulp tasks to facilitate the development process.

```bash
npm install -g gulp
```

### 1.6 Code generator
For the generation of Liferay codes, a plugin installed via npm is used, which will be used by Yeoman.

```bash
npm install -g generator-liferay-theme@7.2.0
```
For further details, check out the section [Liferay Theme Generator](#32-liferay-theme-generator).

### 1.7 Liferay + Tomcat Bundle
The Liferay platform can be run locally through the bundle provided by Liferay at [this address](https://www.liferay.com/downloads?_ga=2.140026997.754221885.1523906791-1737328940.1521481680) (Liferay Portal CE bundled with Tomcat). It already has a built-in Tomcat server, so you only need to download and unzip to some known location, the manual suggests separating the bundles inside a folder like `~/liferay/bundles/`, for example.
To start the Tomcat server running the Liferay portal, simply open the place where the bundle was unzipped inside the `tomcat-[version]/bin` folder and run the command:

```bash
./startup.sh
```

The platform logs are exported in the `tomcat-[version]/logs/catalina.out` file, so to see the logs in real time, the command used is:

```bash
tail -f catalina.out
```

The server takes a few seconds to initialize, when the startup process finishes, a browser tab is launched with the Liferay portal open on the initial configuration page, where it is possible to set the administrator user account, the name of the main site and choose the database.
To terminate execution of the Tomcat server, the following script is executed in the `tomcat-[version]/bin` folder:

```bash
./shutdown.sh
```

Like `startup.sh`, `shutdown.sh` also takes a few seconds to run, but it does happen in the background, so you do not need to keep any windows open for it to complete.

## 2. Fundamentals
Liferay is a platform for developing websites and systems for the web, mobile devices and other devices that are connected to the internet. It has easy customization using the WYSIWYG technique (What you see is what you get), the components can be simply dragged onto the screen and the way they are displayed there is how they will be presented to users. Business rules can be configured directly in the editor, since each component has its own characteristics and can easily be customized to meet the most varied criteria.
The platform also allows managing user accounts and permissions, managing media and content in general.

## 3. Tools

### 3.1 Blade CLI
To facilitate the creation of Liferay 7.0 modules, services, or any other structure, as well as deploy, local server management, and other platform-related tasks, you can use Blade CLI, which is an Gradle environment-based command-line tool. It has subcommands that assist in creating and deploying modules in a Liferay instance. This tool is built into the Liferay Workspace, and can be installed following the instructions in [Liferay Official Documentation - Installing Blade CLI](https://dev.liferay.com/en/develop/tutorials/-/knowledge_base/7-0/installing-blade-cli). To list the CLI Blade commands with their respective functions, simply execute the `blade help` command, the following commands will be listed:

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

Blade relies on templates to create the modules, there are several templates available for use, which gives the developer plenty of flexibility in using it. The available templates are shown by the command `blade create -l` and this is the return of the command:

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

It is possible to create a Liferay environment using Blade CLI, from the command sequence:

```bash
blade init [WORKSPACE_NAME]
cd [WORKSPACE_NAME]
./gradlew initBundle
blade server start -b
```

The `-b` parameter starts the server in the background. Alternatively, the `-d` parameter can be used to use debug mode.
After executing these commands the server will initialize, just like `startup.sh` script. To see the logs in real time, you can watch the `bundles/tomcat-[version]/logs/catalina.out` file with the tail command:

```bash
tail -f bundles/tomcat-[version]/logs/catalina.out
```

This will show all the inserts in the `catalina.out` file which is where the logs are logged. If errors occur, they will also be shown in this file.

To stop the execution of the service and stop the server, use the command:

```bash
blade server stop
```

To create a Liferay Theme using the Blade CLI, use the command at the root of the workspace `[WORKSPACE_NAME]`:

```bash
blade create -t theme [THEME_NAME]
```

This command will create the folder `[WORKSPACE_NAME]/wars/[THEME_NAME]`, with the folder structure below:

```
[THEME_NAME]
└── src
    └── main
        ├── resources
        │   └── resources-importer
        └── webapp
            ├── css
            └── WEB-INF
```

To deploy the project, just go to the root folder of `[THEME_NAME]` and use the Blade command:

```bash
blade deploy
```

This will generate the build structure of folders and will look like this:

```
[THEME_NAME]
├── build
│   ├── buildTheme
│   │   ├── css
│   │   │   ├── application
│   │   │   ├── aui
│   │   │   │   └── lexicon
│   │   │   │       ├── atlas-theme
│   │   │   │       │   └── variables
│   │   │   │       ├── bootstrap
│   │   │   │       │   └── mixins
│   │   │   │       ├── fonts
│   │   │   │       │   └── alloy-font-awesome
│   │   │   │       │       ├── font
│   │   │   │       │       └── scss
│   │   │   │       └── lexicon-base
│   │   │   │           ├── mixins
│   │   │   │           └── variables
│   │   │   ├── base
│   │   │   ├── layout
│   │   │   ├── navigation
│   │   │   ├── portal
│   │   │   ├── portlet
│   │   │   └── taglib
│   │   ├── images
│   │   │   ├── add_content
│   │   │   ├── api
│   │   │   ├── application
│   │   │   ├── arrows
│   │   │   ├── aui
│   │   │   │   ├── common
│   │   │   │   ├── menu
│   │   │   │   └── panel
│   │   │   ├── blogs
│   │   │   ├── bookmarks
│   │   │   ├── calendar
│   │   │   ├── common
│   │   │   ├── control_panel
│   │   │   ├── diff
│   │   │   ├── dockbar
│   │   │   ├── document_library
│   │   │   ├── emoticons
│   │   │   ├── file_system
│   │   │   │   ├── large
│   │   │   │   └── small
│   │   │   ├── forms
│   │   │   ├── image_gallery_display
│   │   │   ├── journal
│   │   │   ├── language
│   │   │   ├── lexicon
│   │   │   ├── login
│   │   │   ├── mail
│   │   │   ├── message_boards
│   │   │   ├── messages
│   │   │   ├── navigation
│   │   │   ├── portlet
│   │   │   ├── progress_bar
│   │   │   ├── ratings
│   │   │   ├── shadow
│   │   │   ├── shopping
│   │   │   ├── social
│   │   │   ├── social_bookmarks
│   │   │   ├── staging_bar
│   │   │   ├── trees
│   │   │   ├── users_admin
│   │   │   └── wiki
│   │   ├── js
│   │   ├── templates
│   │   └── WEB-INF
│   ├── libs
│   ├── resources
│   │   └── main
│   │       └── resources-importer
│   └── tmp
│       └── war
└── src
    └── main
        ├── resources
        │   └── resources-importer
        └── webapp
            ├── css
            └── WEB-INF
```

To edit any file in the build, you can create (or copy) the file from the `build` folder to the` src` folder, with the same name, in a corresponding directory, so when the `blade deploy` is executed again, the files in the `src` folder will replace the files in the` build` folder.
**Editing portions of these files will affect the functioning of the portal as a whole, including native portal functions, so this must be taken into account in all customizations.**

>The build structure of folders is similar to the structure when created by Yeoman, most of the files consist of modularized SCSS in multiple folders and files. There is also a JavaScript file in the `build/buildTheme/js` folder called `main.js`, which at the moment is practically empty, but can be duplicated in`src/main/webapp/js` (not yet created) and will replace `build`with the JavaScript functionalities implemented in`src/main/webapp/js/main.js`.

### 3.2 Liferay Theme Generator
_Liferay Theme Generator_ can help with the start of a new theme and other related details such as layouts and color schemes among others.

Themes are packages built to customize the general layout of the page, such as _header_ and _footer_, navigation menu, positioning of _portlets_ and so on.

#### 3.2.1 Creating a new theme

In the project's `themes` folder, use the command:

```bash
$ yo liferay-theme
```

Soon after the configuration settings, the Yeoman will create an initial structure for the theme (maybe it needs administrator privileges like `sudo`), and soon it will run `npm install` to install and the necessary dependencies, creating the following structure:

```
my-theme
├── node_modules                                     // Folder with dependencies installed
├── build                                            // Build theme (folder created only after the first build)
├── src                                              // Top theme files
│   │
│   ├── css                                          // Custom CSS
│   ├── js                                           // Custom JS
│   └── WEB_INF                                      // Liferay Metadata
│       │
│       ├── liferay-look-and-feel.xml                // File with display settings
│       └── liferay-plugin-package.properties        // File with details to view, such as name and description
│
├── gulpfile.js                                      // File with gulp's tasks
├── liferay-theme.json                               // Theme details such as ID, URL to deploy, and more
└── package.json                                     // Dependency details
```

The _deploy_ and _build_ commands of the theme are made through _gulp_:

```bash
$ gulp build        // Build the theme and create the components in the `my-theme/build` folder
$ gulp deploy       // Build and deploy the theme to the server
$ gulp watch        // Deploy, watch to changes and apply them continuously
```

#### 3.2.2 Customizing page structure
After creating a theme, make the first build with gulp. This will create the `build` folder within your theme. Inside it you will find the folder `templates` inside it you will find the base structure of the Theme.

```
templates
├── init.ftl                     // Initial settings of FTL variables (not recommended to change this file)
├── init_custom.ftl              // Used to append at the end of init.ftl
├── navigation.ftl               // Portal navigation menu template
├── portal_normal.ftl            // Initial portal structure
├── portal_pop_up.ftl            // Portal popups template
└── portlet.ftl                  // Structure Template involving Portlets
```

Copy and paste the component you want to change in your `src` folder and so gulp will replace the file when running _build_.

Templates allow you to completely change the structure of the page, such as HTML tags, and even embed some portles, so they can not be removed.

It is possible to call the portlets with the following tag:

```FreeMarker
<@liferay_portlet["runtime"]
    instanceId="INSTANCE_ID"
    portletName="PORTLET_NAME"
/>
```

Where `portletName` is the name of the portlet package, changing the dots by underline
Ex: com.liferay.portal.search.web.portlet.SearchPortlet = com_liferay_portal_search_web_portlet_SearchPortlet
>`InstanceId` needs to be called if the portlet can be used multiple times.

For example, to embed the search portlet into page, put in your code:

```FreeMarker
<@liferay_portlet["runtime"]
    portletName="com_liferay_portal_search_web_portlet_SearchPortlet"
/>
```

[+ More about this](https://dev.liferay.com/en/develop/tutorials/-/knowledge_base/7-0/embedding-portlets-in-themes-and-layout-templates).

#### 3.2.3 Creating new layouts
When creating a site or page in the Liferay portal, you need to define a layout for portlet placement
![Default layouts](./images/3-2-3-layout-default.png)

But it is possible to create a new layout if necessary, with the _Liferay Theme Generator_ and the command

```bash
$ yo liferay-theme:layout
```

And follow the generator's instructions, like choosing name, id and how many columns and rows the layout will have.
>Remembering that the 12-column bootstrap system is used.

When you finish defining the options, the system will create a `tpl` extension file with the structure of its layout, and an image of the same name to use as an icon.
>If the generator is used in a folder with a theme created by it, it will create the files in the `src/layouttpl` folder.

In the next deploy, the layout will appear as an option among others when creating or editing a page.

#### 3.2.4 Creating Themelets
There is a component called _themelet_, which is an extension to a theme and can add style sheets, images, templates and JavaScript functionality to it. It is suitable for small changes and aims to bring more modularity to the themes and avoid repetition of code.
>In the npm record there are themelets available for reuse.

The creation of _themelets_ is done from Yeoman, with the `liferay-theme: themelet` task:

```bash
yo liferay-theme:themelet
```

After this command the created folder structure is as follows:

```
[THEMELET_ROOT]
├── package.json
└── src
    └── css
        └── _custom.scss
```

Within the `src` folder, you can create `js`, `template` and `images` folders, following the same naming and structure of a theme, so that the files are allocated correctly at the moment of the build.

To make the themelet available for use, you can use two npm methods:
- `npm link`: Creates a symlink of the themelet directory in the global folder of `node_modules`, so any changes in the themelet will be instantly available for the themes that extend it.
- `npm install -g`: This method copies the themelet files to the global `node_modules` folder, but does not maintain the link to the original folder, so when there are changes in the themelet you will need to run the command again.

>For more information about themelets, [click here](https://dev.liferay.com/pta/develop/tutorials/-/knowledge_base/7-0/themelets).

#### 3.2.5 Importing themes
This `liferay-theme` feature allows you to import other themes found in the folder into the _Liferay Theme Generator_ template.

```bash
yo liferay-theme:import
```

## 4. Front End Components

Below are some components that help in the development of Front End, its main features and functionalities.

### 4.1 Languages used

#### 4.1.1 HTML & Structure

##### FreeMarker

For the HTML structure of the page and the portlets is used the _FreeMarker_ which is a language that mixes Java and HTML.

To build elements using HTML typically:

```HTML
<ul>
    <li>Example</li>
</ul>
```

In FreeMarker we could use variables and interpolation:

```FreeMarker
<#assign variable = "Example" />

<ul>
    <li>${variable}</li>
</ul>
```

We also could have loops, logical structures, objects, includes and so on:

```FreeMarker
<#assign objectList = ["Example 1","Example 2", "Example 3"] />

<ul>
    <#if objectList?has_content>
        <#list objectList as object>
            <li>${object}</li>
        </#list>
    </#if>
</ul>
```

[+ More about](https://freemarker.apache.org/docs/).

##### Soy Templates (Google Closure)
Although not used by ADTs and Web Content Displays, _soy_ can be used to build portlets in conjunction with _metal.js_.
For more details on _soy_, [click here](https://developers.google.com/closure/templates/).

#### 4.1.2 CSS
For the CSS is used SCSS, the preprocessed CSS with functions like variables, nesting, mixins and so on:

```SCSS
div .text {
    color: blue;

    &.red {
        color: red;
    }
}
```

Compilado:

```CSS
div .text {
    color: blue;
}
div .text.red {
    color: red;
}
```

Also used are Bootstrap and Lexicon/Clay libraries that have several mixins and components already styled.

To get more about:
- [Bootstrap](http://getbootstrap.com/docs/3.3/css/).
- [Lexicon](https://lexiconcss.wedeploy.io/).

>Some elements from Lexicon can be called by the liferay-ui taglib.

#### 4.1.3 JavaScript
For JavaScript, preference is for ES6, but it is possible to use other libraries, such as jQuery, which is already included in the package.
It is also possible to add more libraries via `NPM`, or add to the JS folder of your theme, and import into the `head` tag of your `portal_normal.ftl` template.

### 4.2 ADTs
_Application Display Templates_ or ADTs, are templates that allow customization of _portlets_. They are templates in _FreeMarker_ (ftl), with classes and custom structure.

The CSS comes from the theme classes, or an inline style that can be placed in _portlet_ by the portal.

The _portlets_ that support ADTs are:
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
- and Wiki.

Each _portlet_ has a specific ADT, with some presets and ready-made calls to make it easy to customize.

### 4.3 Web Content Structures & Templates
Web Contents are elements used to present content either through the Web Content portlet itself or an Asset Display, which can list all the contents of the portal in an organized way, as well as a Blog, but in a more open and customizable way. In conjunction with the ADTs, it is possible to produce elements such as galleries, listing of posts, photo album, among others. It is one of liferay's most customizable portlets.

Web Content uses a structure that must be created to define what can be used by a web content, and each structure can have several templates that, like an ADT, use FreeMarker to customize its display.

You can find the Structure and Templates options in the Web Content sub-menu:
![Web Content Submenu](./images/4-3-web-content-strucutures.jpeg)

>The Asset Display uses ADTs to customize the Web Contents listing, but the Web Content itself, when maximized, uses the Template structure.

[+ More Details](https://dev.liferay.com/en/discover/portal/-/knowledge_base/7-0/creating-web-content).

#### 4.3.1 Web Content Structures
With the structures you can define what a web content will present, among several options like image publishing, common text, html and select boxes. And set options as either mandatory or optional content for Web Content publishing.

#### 4.3.2 Web Content Templates
With the templates you can define how the structure will be displayed, from a _ftl_ script.

Just like in ADTs, you can find alongside some variables ready to assist in building the code, along the calls of the elements that were determined in the structure, but it is also possible to use some restricted variables to access other elements of Web Content. ([See more](#7-code-snippets)).

### 4.4 Liferay UI Components

The Application Display Templates (ADTs) and the Web Content Templates in FreeMarker Template (_ftl_) have full UI and Utils taglibs support, using the following structure:

```FreeMarker
<@liferay_ui['property']
    param="value"
/>
```

For example: If you want to create a _User Display_, you need:

```FreeMarker
<@liferay_ui["user-display"]
    markupView="lexicon"
    showUserDetails=false
    showUserName=true
    userId=userId
    userName=userName
/>
```

You can also use other elements, such as Lexicon icons:

```FreeMarker
<@liferay_ui["icon"]
    icon="name-of-the-icon"
    markupView="lexicon"
    message="An message popup"
    cssClass="additional-class"
/>
```

> You can change the types of icons, replacing `markupView` with Glyphicon, or Font-Awesome.

> For a complete reference of the icons, go to [Lexicon Icons](https://lexiconcss.wedeploy.io/content/icons/).

You can see a complete list with all available tags and their parameters in the following [link](https://docs.liferay.com/ce/portal/7.0-latest/taglibs/util-taglib/).

### 4.5 Gulp Tasks
Liferay have predefined gulp tasks to help in the theme build and deploy, this tasks allows to compile all theme files in a WAR file, and do the `deploy` in the application server, it also allows to extend themes with themelets, that help make little changes without creat a complete new theme.

#### 4.5.1 Gulp build
This taks compiles all the source-code in a file .WAR, in the `dist` folder.

#### 4.5.2 Gulp deploy
The task `deploy` executes the `build` task before, to create the `WAR`, and after that publishes this file on the specified application server.
If the Bundle is in execution, you can use the taks `deploy:gogo`, that is a fast method than the `deploy`
>The documentation suggests that only one deploy method is used, that means, if for the first deploy the task `deploy` is used, it is advisable that the `deploy:gogo` task not to be used.

#### 4.5.3 Gulp watch
This task its similar to the `deploy:gogo`, and only works when the bundle is running. It keeps looking the files and when it finds alterations on the files, it makes a _fast deploy_, but it doesn't have a _BrowserSync_, which is the technology that allows you to view the changes in the browser instantly, so to see the changes you need to refresh the page.

#### 4.5.4 Gulp init
This task is used to specify the application server address that will be used by the `deploy` task.
>It is automatically called by the Yeoman _liferay-theme_ and _liferay-theme:import_.

As propriedades geradas por essa tarefa ficam salvas no arquivo `liferay-theme.json`, no diretório raiz do tema.

#### 4.5.5 Gulp extend
This task allows to configure the _base theme_ and also allows to add and install _themelets_ to it.
When the base theme is changed, there is two possibilities: **styled** or **unstyled**.
- The _Base Theme Styled_ has all the portal native style and features (like login, edition tools and content management tool), and all default SCSS are avaliable, including the Bootstrap, Lexicon, etc., but the portlets and the pages aren't stylized.
- The _Base Theme Unstyled_, doesn't have any style predefinition, ever for the portal and its features. It used when its needed to recreate all the portal visual structure.

>Both Base Themes are published as npm packages, [Styled](https://www.npmjs.com/package/liferay-theme-styled) and [Unstyled](https://www.npmjs.com/package/liferay-theme-unstyled).
>Its also possible to extend the theme from other themes published on `npm` and, according to the documentation, do not necessarily need to be installed.

#### 4.5.6 Gulp status
This task only repotrs the _Base Theme_ that has been used, and which _themelets_ are been used.

## 5. Util

  * [FreeMarker Docs](https://freemarker.apache.org/docs/index.html)
  * [Liferay Docs](https://dev.liferay.com/en/develop/tutorials).
  * [Taglibs Docs](https://docs.liferay.com/ce/portal/7.0-latest/taglibs/util-taglib/).
  * [Liferay Certification Service](https://www.liferay.com/en/services/certification).

## 6. Liferay Certification
Liferay has an [Certification Service](https://www.liferay.com/en/services/certification), that is a test with 50 question, ranging from questions of true or false and multiple choice, with 90 minutes of duration and a cost of USD 200.00. This test allows an official recognition of the skill and experience with the Liferay environment. In the moment, there is three avaliable certifications, one for Liferay 6.2, and two others for Liferay DXP (Front-End and Back-End).

### 6.1 Liferay 6.2 Certified Professional Developer

The Liferay 6.2 Professional Developer comprises the following items:

+ Liferay Development Best Practices (10%)
    * Development Environment Setup
    * Understanding Liferay Plugins

+ Liferay Architecture and APIs (25%)
    * Understanding Liferay Architecture
    * Service Builder
    * Liferay Utilities
    * User Management and Group APIs
    * AlloyUI
    * Expando API
    * Application Display Templates

+ Liferay Portlet Plugin Development (20%)
    * Portlet API
    * Configuration
    * IPC
    * Liferay MVCPortlet
    * JSP and UI Technologies
    * Permissions

+ Liferay Hook Plugin Development (25%)
    * Best Practices
    * Configuration Hook
    * JSP Hook
    * Language Hook
    * Indexer Post Processor Hook
    * Service Wrapper Hook
    * Struts Action Hook
    * Servlet Filter Hook

+ Liferay Theme Plugin Development (5%)
    * Understanding Liferay Theme Development
    * Color Schemes and Theme Settings
    * Embedded Portlets

+ Liferay Layout Template Plugin Development (5%)
    * Understanding Liferay Layout Template Development

+ Liferay Advanced Customization (10%)
    * Understanding EXT Plugins
    * Modifying Portal Configuration
    * Customizing Core Portlets

[+ More Details](https://www.liferay.com/en/services/certification/professional-developer/6.2)

### 6.2 Liferay DXP Certified Professional Front-End Developer

The Liferay DXP Front-End Certification comprises the following items:

+ Front-End State-of-the-Art (10%)
    * Bootstrap
    * NodeJS
    * NPM
    * Yeoman
    * Gulp
    * Soy Templates

+ Liferay Technologies (15%)
    * Lexicon
    * Metal.js
    * AlloyUi
    * Senna
    * AlloyEditor
    * Liferay AMD Module Loader
    * Themes SDK

+ Building Layout Templates (20%)
    * Layout Templates with Liferay Themes Generator
    * Embedding Portlets in Layouts Templates

+ Building Themes (45%)
    * Liferay Themes Generator
    * Themelets
    * Theme Contributors
    * Context Contributors
    * Portlet Decorators
    * LayoutSet
    * Resources Importer
    * Embedding Portlets in Themes

+ Customizing with Templates (5%)
    * Web Content Templates
    * Workflow Templates
    * Application Display Templates
+ Taglibs (5%)
    * aui
    * liferay-ui

[+ More Details](https://www.liferay.com/en/services/certification/dxp/front-end-developer)

### 6.3 Liferay DXP Certified Professional Back-End Developer

The Liferay DXP Back-End Certification comprises the following items:

+ Liferay Digital Experience Platform: Basic Concepts (25%)
    * OSGi
    * Liferay Modules (Bundles)
    * JSR-286 specification
    * Portlet Lifecycle
    * Gogo Shell

+ Liferay Digital Experience Platform: Portlet Modules (20%)
    * Portlet Components
    * Attributes
    * MVC
    * Declarative Services

+ Liferay Digital Experience Platform: Liferay Services (25%)
    * Users
    * Blogs
    * Web Content Articles
    * Message Board Posts
    * Pages

+ Liferay Digital Experience Platform: Liferay Frameworks (25%)
    * Asset
    * Search and Indexing
    * Liferay Utilities
    * Feedback Validation
    * Persistence Layer
    * Messaging
    * Authentication

+ Liferay Digital Experience Platform: Upgrade Process (5%)
    * Know how to make upgrades from Liferay 6.X to Liferay DXP
    * Development Strategy

[+ More details](https://www.liferay.com/en/services/certification/dxp/back-end-developer)

-----
## 7. Code snippets

### Blog / Web content

- Return posts/web content tags:
```FreeMarker
<#assign AssetTagLocalService = serviceLocator.findService("com.liferay.asset.kernel.service.AssetTagLocalService")>
<#assign entryTags = AssetTagLocalService.getEntryTags(entry.entryId)>
```

- Return posts/web content categories:
```FreeMarker
<#assign AssetCategoryLocalService = serviceLocator.findService("com.liferay.asset.kernel.service.AssetCategoryLocalService")>
<#assign entryCategories = AssetCategoryLocalService.getCategories(entry.classNameId, entry.classPK)>
```

>If the serviceLocator call fails, you must remove the constrained variables in the portal in `Menu > Control Panel > System Settings > Foundation > FreeMarker Engine` and remove the serviceLocator restricted variable.

- Web Content restric variables:
```FreeMarker
.vars['reserved-article-asset-tag-names'].data
.vars['reserved-article-author-comments'].data
.vars['reserved-article-author-email-address'].data
.vars['reserved-article-author-id'].data
.vars['reserved-article-author-job-title'].data
.vars['reserved-article-author-name'].data
.vars['reserved-article-create-date'].data
.vars['reserved-article-description'].data
.vars['reserved-article-display-date'].data
.vars['reserved-article-id'].data
.vars['reserved-article-modified-date'].data
.vars['reserved-article-small-image-url'].data
.vars['reserved-article-title'].data
.vars['reserved-article-type'].data
.vars['reserved-article-url-title'].data
.vars['reserved-article-version'].data
```

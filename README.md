# Apache Maven
- - -
### Ferramenta de gerenciamento de dependências de um projeto

- Facilita o gerenciamento das bibliotecas dos projetos, onde dependendo da complexidade do projeto, se torna inviável
  realizar importações e verificação de versão de cada biblioteca manualmente.
- O Maven utiliza o Modelo de Objeto de Projeto, onde organiza todas as informações do projeto em um único arquivo: o **pom.xml**
    - Conforme dependências são requisitadas, o arquivo pom.xml é atualizado.
    - O projeto Maven pode possuir módulos e cada módulo podem ter seu próprio pom.xml, sem perder a organização do projeto principal.
- O Maven auxilia também na construção de relatórios e documentações.

## Instalação Ubuntu
- - -
- Download
    - https://maven.apache.org/download.cgi
- Descompactar
    - `tar xf nome_arquivo.tar.gz`
- Mover para área de serviço
    - `mv nome_arquivo pasta_destino`
- Verificar Java instalado
    - `javac --version`
    - Alterar versão do java em uso
        - `sudo update-alternatives --config java`
- Acessar a pasta bin do maven
    - Buscar o path do maven
        - `pwd`
- Abrir o arquivo **bashrc** em um editor de texto
    - O arquivo **bashrc** é um script em shell que roda todas as vezes que abrimos o terminal
    - Fica localizado na home do sistema, e é um arquivo oculto
    - No final do arquivo, adicionar o path do maven
        - `export PATH=/home/israel/devtools/apache-maven-3.9.5/bin:$PATH`
- Após reabrir o terminal, verificar se o maven está disponível
    - `mvn --version`

## Criando um projeto Maven via linha de comando
- - -
**Os archetypes são templates de projetos que facilitam a geração automática de um projeto com base em um template, onde existem inúmeros para escolha.**
- Acessar pasta de trabalho
- Criando projeto Maven
    - `mvn archetype:generate -DgroupId=introduction.mvn -DartifactId=introduction-apache-maven -Darchetype=maven-archetype-quickstart -DinteractiveMode=false`

## Comandos auxiliares
- - -
- Compilação
  - `mvn compile`
  - Após executar, os códigos java são compilados e enviados para a pasta **target**
- Teste
  - `mvn test`
  - Com esse comando, o maven busca as classes de teste existentes no projeto e executa.
- Empacotar
  - `mvn package`
  - Cria o .jar da aplicação
- Limpando diretória target
  - `mvn clean`
  - Limpa o diretório de trabalho do maven

## Archetype
- - -
**O Maven Archetype é um template que permite a modificação de como um projeto será construído,
ou seja, define versões de componentes, quais os componentes que serão inseridos, organização de pacotes e arquivos.**

- Lista de Archetypes
  - https://maven.apache.org/archetypes/index.html
  - Cada archetype tem o comando para criar o projeto

## POM
- - -
Abreviação de Product Object Model, utiliza formato XML e fica na raiz do projeto, onde detalha o projeto, dentre elas:
- Detalhes do projeto (nome, descrição, licença, url)
- Configurações de ambiente (repositórios, tracking, profiles)
- Dependencias
- Módulos
- Configurações de build

Exemplo

```
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  
  <!-- The Basics -->
  <groupId>...</groupId>
  <artifactId>...</artifactId>
  <version>...</version>
  <packaging>...</packaging>
  <dependencies>...</dependencies>
  <parent>...</parent>
  <dependencyManagement>...</dependencyManagement>
  <modules>...</modules>
  <properties>...</properties>
  
  <!-- Build Settings-->
  <build>...</build>
  <reporting>...</reporting>
  
  <!-- More Project Information -->
  <name>...</name>
  <description>...</description>
  <url>...</url>
  <inceptionYear>...</inceptionYear>
  <licenses>...</licenses>
  <organization>...</organization>
  <developers>...</developers>
  <contributors>...</contributors>
  
  <!-- Environment Settings -->
  <issueManagement>...</issueManagement>
  <ciManagement>...</ciManagement>
  <mailingLists>...</mailingLists>
  <scm>...</scm>
  <prerequisites>...</prerequisites>
  <repositories>...</repositories>
  <pluginRepositories>...</pluginRepositories>
  <distributionManagement>...</distributionManagement>
  <profiles>...</profiles>
</project>
```
É possível declarar inúmeras informações no arquivo, ajudando a entender melhor o projeto, 
como foi construído, o que utiliza, como é entregue, etc.

As informações que não são declaradas no nosso pom.xml, serão buscadas do super pom: https://maven.apache.org/ref/3.0.4/maven-model-builder/super-pom.html

## Repositórios

- Quando não temos uma configuração própria de repositório no pom, vai utilizar o repositório do Super Pom.

- Podemos também configurar um repositório remoto global no settings.xml do maven instalado na máquina, na tag **MIRROR**.

## Dependências

- Exemplo de como configurar uma nova dependência
```
<dependencies>
   <dependency>
     <groupId>one.digitalinnovation</groupId>
     <artifactId>app-spring</artifactId>
     <version>1.0.0</version>
   </dependency>
</dependencies>
```

- groupId
  - Como se fosse o id da organização, segue as regras de nomes de pacote Java
- artifactId
  - Nome do projeto em si
- version
  - Versão da biblioteca a ser utilizada

Utilizamos `mvn compile` para baixar a dependência nova do pom.xml

### Tipos de Dependências

- Para disponibilizar um projeto como dependência, utilizamos `mvn install`
  - A dependência fica no repositório local `./usr/.m2`
  - Após, é possível pegar o artifactId e o groupId do projeto em questão e utilizar como dependência em outro projeto.
- Existem dois tipos de dependências no Maven
  - Diretas
    - Dependência que declaramos diretamente no pom.xml
  - Transitivas
    - Dependências obrigatórias das suas dependências declaradas

### Dependências Transitivas
**Um dos problemas da transitividade, é o fato de que quando setamos 
uma nova dependência, é possível que venha com várias outras**
- Para resolver esse problema, existem escopos para limitar a transitividade dessas dependências
  - Compile (Escopo default do Maven)
    - Disponível em todos os classpaths (compile, test, run)
    - É transitiva
  - Provided
    - As dependências serão fornecidas em tempo de execução, onde é adicionado no classpath
compile e test. Em aplicações que rodam em servidores, normalmente utilizamos provided como escopo da dependência
    - Não é transitiva
  - Runtime
    - Indica que a dependência é necessária para execução e não compilação (run e test)
  - Test
    - Disponível apenas no teste e compilação
  - System
    - Similar ao provided, porém é necessário exibir o JAR explicitamente
    - Não é transitiva
  - Import
    - Disponível apenas com uma dependência do tipo pom e com tag <dependecyManagement>
    - Indica um processo de reutilizar dependências de um projeto, permite a importação de dependências de outro projeto
      - ```
        <dependencyManagement>
          <dependencies>
            <dependency>
                <groupId>com.programmer</groupId>
                <artifactId>my-project</artifactId>
                <version>3.8.1</version>
                <scope>import</scope>
            </dependency>
          </dependencies>
        </dependencyManagement>
        ```
### Dicas sobre escopos

- Visualizar construção do classpath
  - `mvn dependency:build-classpath -DincludeScope=compile`
  - `mvn dependency:build-classpath -DincludeScope=test`
  - `mvn dependency:build-classpath -DincludeScope=run`

### Dependências Opcionais
- **Utilizado quando uma dependência não é necessária para os projetos que irão reutilizar seu componente como dependência**

  - Podemos tornar opcional com a tag `<optional>true</optional>`

### Exclusions
  - **Utilizado quando o componente de dependência que usamos compartilha de um outro componente
    que já existe no projeto ou que não quer ter no projeto.**
    - ```
      <dependency>
         <groupId>com.programmer</groupId>
         <artifactId>my-project</artifactId>
         <version>3.8.1</version>
         <exclusion>
            <groupId>...</groupId>
            <artifactId>...</artifactId>
         </exclusion>
      </dependency>
      ```

## Maven Build Lifecycle
https://maven.apache.org/guides/introduction/introduction-to-the-lifecycle.html
- Ciclos de vida
  - Default
  - Clean
  - Site

- Default Lifecycle
  - Principal ciclo
  - Responsável pelo deploy local
    - Os comando que utilizamos, como, **mvn test, mvn install, mvn compile, mvn integration test, dentre outros**,
fazem parte deste ciclo de vida
  - É composto por 23 fases, abaixo as mais utilizadas
    - `mvn validate`
      - Valida as informações do projeto.
    - `mvn compile`
    - `mvn test-compile`
      - Compila os testes do projeto.
    - `mvn test`
      - Executa os testes unitários.
    - `mvn integration-test`
      - Executa os testes de integração.
    - `mvn package`
      - Realiza o empacotamento do software.
    - `mvn install`
      - Publica o projeto como JAR localmente.
    - `mvn deploy`
      - Copia o projeto para o repositório remoto.
- Clean Lifecycle
  - Ciclo intermediário, responsável pela limpeza do projeto
  - Compostos por 3 fases
    - pre-clean
      - Executa os processos antes da limpeza real do projeto
    - clean
      - Remove a pasta target, sempre recomendado antes de compilar o projeto novamente.
    - post-clean
      - Executa os processos para finalizar a limpeza do projeto
- Site Lifecycle
  - Ciclo final, comandos necessários
  - Responsável pela criação do site de documentação do projeto
  - Composto por 4 fases
    - `pre-site`
      - Executa os processos antes da geração real do projeto
    - `site`
      - Gera a documentação do site do projeto
    - `post-site`
      - Executa os processos necessários para finalizar a geração do site e para se preparar
para a implementação do mesmo
    - `site-deploy`
      - Implanta a documentação do site gerada no servidor web especificado
















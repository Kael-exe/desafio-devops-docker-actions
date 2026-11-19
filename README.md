# Desafio DevOps: Docker e GitHub Actions



## 1. Objetivo do projeto



Este projeto tem como objetivo praticar conceitos fundamentais de DevOps, utilizando Docker para executar uma aplicação HTML em um container e GitHub Actions para automatizar sua construção e validação.



A aplicação utiliza o servidor web Nginx para disponibilizar uma página HTML.



O desafio também permite compreender como funciona um processo básico de Integração Contínua (CI), desde o envio de alterações ao GitHub até a execução automática de testes.



## 2. Tecnologias utilizadas



- **Docker:** plataforma utilizada para criar e executar containers.

- **Nginx:** servidor web responsável por disponibilizar a página HTML.

- **Git:** ferramenta de controle de versão.

- **GitHub:** plataforma de hospedagem do código-fonte.

- **GitHub Actions:** ferramenta de automação utilizada para executar o pipeline de CI.

- **WSL 2:** ambiente Linux integrado ao Windows, utilizado pelo Docker Desktop.



## 3. Ambiente de desenvolvimento



O projeto foi desenvolvido utilizando o seguinte ambiente:



| Componente | Configuração |

|---|---|

| Sistema operacional | Windows 11 |

| Terminal | Windows PowerShell 5.1 |

| Containerização | Docker Desktop |

| Backend do Docker | WSL 2 |

| Controle de versão | Git |

| Repositório | GitHub |



### O que é WSL 2?



O WSL 2 (Windows Subsystem for Linux 2) é um recurso do Windows que permite executar um ambiente Linux utilizando um kernel Linux real.



Neste projeto, o WSL 2 trabalha em conjunto com o Docker Desktop, permitindo executar containers Linux no ambiente Windows.



Isso possibilita utilizar ferramentas e recursos do ecossistema Linux sem precisar instalar uma distribuição Linux diretamente no computador como sistema operacional principal.



**Importante:** o WSL 2 faz parte do ambiente utilizado neste projeto, mas não é uma dependência da aplicação HTML ou do Dockerfile.



## 4. Conceitos fundamentais



### O que é Docker?



Docker é uma plataforma que permite empacotar e executar aplicações em containers.



Uma das principais vantagens é manter a aplicação e seu ambiente de execução organizados, facilitando sua execução em diferentes máquinas.



### O que é uma imagem Docker?



Uma imagem Docker funciona como um modelo utilizado para criar containers.



Neste projeto, construímos uma imagem chamada `desafio-devops:v1`, utilizando o Nginx como base.



### O que é um container?



Um container é uma instância executável de uma imagem Docker.



É dentro dele que nossa aplicação é executada e disponibilizada pelo servidor Nginx.



### Diferença entre imagem e container



Uma forma simples de entender:



- **Imagem:** funciona como um modelo pronto.

- **Container:** é a execução desse modelo.



### O que é Nginx?



Nginx é um servidor web utilizado para disponibilizar conteúdos pela internet ou por uma rede local.



Neste desafio, ele é responsável por servir o arquivo `index.html` quando acessamos a aplicação pelo navegador.



## 5. Estrutura do projeto



```text

desafio-devops-docker-actions/

├── .github/

│   └── workflows/

│       └── ci.yml

├── .dockerignore

├── Dockerfile

├── index.html

└── README.md

```



### Responsabilidade de cada arquivo



- `index.html`: contém a página HTML disponibilizada pelo Nginx.

- `Dockerfile`: define as instruções para construir a imagem Docker.

- `.dockerignore`: informa quais arquivos e diretórios devem ser ignorados durante a construção da imagem.

- `.github/workflows/ci.yml`: contém as instruções do pipeline automatizado.

- `README.md`: documenta o projeto e suas instruções de execução.



## 6. Entendendo o Dockerfile



Conteúdo utilizado:



```dockerfile

FROM nginx:alpine



COPY index.html /usr/share/nginx/html/index.html

```



### Explicação das instruções



**FROM nginx:alpine**



Define a imagem-base que será utilizada na construção da nossa imagem.



Nesse caso, utilizamos o Nginx com Alpine Linux, uma distribuição Linux conhecida por ser leve.



**COPY index.html /usr/share/nginx/html/index.html**



Copia o arquivo `index.html` do projeto para o diretório utilizado pelo Nginx para disponibilizar páginas web.



Assim, quando o container iniciar, o Nginx encontrará nossa página e poderá servi-la.



## 7. Arquivo .dockerignore



O arquivo `.dockerignore` contém:



```text

.git

.github

README.md

```



Ele evita que arquivos e diretórios desnecessários sejam enviados ao contexto de construção da imagem Docker.



Isso ajuda a manter o processo de build mais organizado e evita incluir arquivos que não são necessários para executar a aplicação.



## 8. Executando o projeto localmente



### Pré-requisitos



- Docker Desktop instalado e em execução.

- Git instalado.

- Repositório do projeto disponível localmente.



### 8.1. Construir a imagem Docker



```powershell

docker build -t desafio-devops:v1 .

```



Esse comando constrói a imagem utilizando as instruções do Dockerfile.



- `docker build`: inicia a construção da imagem.

- `-t`: define um nome e uma identificação para a imagem.

- `desafio-devops:v1`: nome e versão da imagem.

- `.`: indica que o contexto de construção é o diretório atual.



### 8.2. Iniciar o container



```powershell

docker run -d --name desafio-web -p 127.0.0.1:8080:80 desafio-devops:v1

```



Esse comando cria e inicia um container a partir da imagem construída.



- `docker run`: cria e executa um container.

- `-d`: executa em segundo plano.

- `--name desafio-web`: define o nome do container.

- `-p 127.0.0.1:8080:80`: configura o acesso à aplicação.

- `desafio-devops:v1`: imagem utilizada.



### 8.3. Entendendo o mapeamento de portas



Utilizamos o seguinte mapeamento:



```text

127.0.0.1:8080:80

```



- `127.0.0.1`: endereço local do computador.

- `8080`: porta utilizada no computador Windows.

- `80`: porta utilizada pelo Nginx dentro do container.



Dessa forma, conseguimos acessar o Nginx pela porta 8080 do computador, enquanto ele continua utilizando a porta 80 internamente.



### 8.4. Acessar a aplicação



Após iniciar o container, acesse:



http://127.0.0.1:8080



A página deverá apresentar:



**Meu primeiro desafio DevOps**



Docker e GitHub Actions funcionando!



### 8.5. Verificar os logs



```powershell

docker logs desafio-web

```



Exibe os registros de execução do container, permitindo acompanhar informações sobre o funcionamento do Nginx e as requisições recebidas.



### 8.6. Parar o container



```powershell

docker stop desafio-web

```



Interrompe a execução do container.



### 8.7. Remover o container



```powershell

docker rm desafio-web

```



Remove o container que já foi parado.



## 9. Pipeline de Integração Contínua (CI)



O pipeline está configurado no arquivo:



`.github/workflows/ci.yml`



CI significa *Continuous Integration* (Integração Contínua).



É uma prática que permite automatizar etapas de construção e validação do código sempre que determinadas alterações acontecem no repositório.



### Quando o pipeline é executado?



O workflow possui dois gatilhos:



- **Pull Request:** quando um Pull Request direcionado à branch `main` é aberto, atualizado ou reaberto.

- **Push:** quando alterações são enviadas diretamente para a branch `main`.



### Etapas do pipeline



**1. Checkout do código**



Obtém os arquivos do repositório para que o GitHub Actions possa trabalhar com eles.



**2. Construir imagem Docker**



Executa o comando `docker build`, criando a imagem da aplicação.



**3. Iniciar container**



Cria e inicia um container utilizando a imagem construída.



**4. Validar aplicação**



Realiza uma requisição HTTP utilizando `curl`.



O teste verifica duas condições:



- A aplicação deve retornar o código HTTP 200.

- A resposta deve conter o texto `Meu primeiro desafio DevOps`.



Caso alguma dessas condições não seja atendida, a etapa falha.



**5. Remover container**



Executa a limpeza do container ao final do processo, inclusive quando uma etapa anterior apresenta falha.



## 10. Testes e validações realizados



Foram realizadas três execuções do GitHub Actions para verificar o comportamento do pipeline.



### Execução #1 — Sucesso



O pipeline foi executado com a página original.



A construção da imagem, inicialização do container e validação da aplicação foram concluídas corretamente.



### Execução #2 — Falha proposital



O conteúdo do título HTML foi alterado temporariamente para:



```html

<h1>Teste de falha proposital</h1>

```



Como o pipeline continuava procurando o texto original, a validação falhou.



Esse teste demonstrou que o workflow consegue identificar quando o conteúdo esperado não está presente.



### Execução #3 — Correção



O conteúdo original foi restaurado:



```html

<h1>Meu primeiro desafio DevOps</h1>

```



Após o envio da alteração ao GitHub, o workflow foi executado novamente e todas as etapas foram concluídas com sucesso.



### Resultado dos testes



| Execução | Resultado | Descrição |

|---|---|---|

| #1 | Sucesso | Aplicação validada |

| #2 | Falha | Alteração proposital no HTML |

| #3 | Sucesso | Conteúdo restaurado e validação aprovada |



## 11. Aprendizados



Durante o desenvolvimento deste desafio, foram praticados os seguintes conceitos:



- Construção de imagens Docker.

- Criação, execução, parada e remoção de containers.

- Utilização do Nginx para servir uma página HTML.

- Mapeamento de portas entre o computador e o container.

- Utilização de arquivos Dockerfile e `.dockerignore`.

- Controle de versão com Git.

- Utilização de branches e Pull Requests.

- Criação de pipelines automatizados com GitHub Actions.

- Validação de aplicações por meio de requisições HTTP.

- Identificação de falhas e execução de testes após correções.



## 12. Links do projeto



**Repositório:**

https://github.com/Kael-exe/desafio-devops-docker-actions



**Pull Request #1:**

https://github.com/Kael-exe/desafio-devops-docker-actions/pull/1



**Execuções do GitHub Actions:**

https://github.com/Kael-exe/desafio-devops-docker-actions/actions



As execuções #1, #2 e #3 podem ser consultadas na aba Actions do repositório.

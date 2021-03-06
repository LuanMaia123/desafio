# Problema:
Aqui trabalhamos constantemente com grande volume e complexidade de dados. Sabendo disso,precisamos que você elabore uma solução que ofereça armazenamento, processamento e disponibilização desses dados, sempre considerando que tudo deve estar conforme as boas práticas de segurança em TI. Afinal, nosso principal ativo são dados sensíveis dos consumidores brasileiros.

Vamos supor que existam três grandes bases de dados externas que organizam nossas informações. A primeira delas, que chamamos de Base A, é extremamente sensível e deve ser protegida com os maiores níveis de segurança, mas o acesso a esses dados não precisa ser tão performática. A segunda, é a Base B que também possui dados críticos, mas ao contrário da Base A, o acesso precisa ser um pouco mais rápido. Uma outra característica da Base B é que além de consultas ela é utilizada para extração de dados por meio de algoritmos de aprendizado de máquina. A última base, é a Base C, que não possui nenhum tipo de dado crítico, mas precisa de um acesso extremamente rápido.

# Desenvolvimento

### Minha idéia inicial:

<img src="https://github.com/LuanMaia123/desafio/blob/master/8721%20%5BConvertido%5D-01.jpg" alt="Modelo" style="max-width:100%;">

Quando comecei a desenvolver a solução optei por jogar as imagens dos 3 micro-serviços, e dos DBs, em um docker-compose. Porém durante o desenvolvimento o docker mostrou-se incompativel com o sistema windowns, apesar de ter removido as imagens e os containers, apresentaram-se diversos problemas, desde porta ja está sendo usada, assim como problemas para fazer pull de imagens. 
O desafio me deu a oportunidade de aprofundar meus conhecimentos e coloca-los em prática, já que nunca havia tido contato com tal tecnologia anteriormente. Tendo isso em mente, resolvi por seguir um caminho diferente, com o intuito de economizar tempo, uma vez que se tratava de problema de compatibilidade com SO, e não de falta de conhecimento.

### Nova Abordagem:

<img src="https://github.com/LuanMaia123/desafio/blob/master/NOVO-01.jpg" alt="Modelo2" style="max-width:100%;">

Após fazer algumas pesquisas verifiquei a possibilidade de ultilização do Eureka:

Eureka é uma solução de Service Discovery open source desenvolvida pela Netflix e é composta pelos módulos Eureka Server e Eureka Client.   

O Eureka Server consiste em uma aplicação que atua como Service Registry permitindo que outras aplicações registrem suas instâncias, com isso,  ele controla os endereços registrados mantendo-os atualizados e sinalizando quando um serviço não está disponível.

O Eureka Client é um componente Java que facilita a interação com Eureka Server.

Dentre as vantagens de se utilizar o Eureka estão: 

É otimizado para trabalhar em AWS clouds com diferentes regiões;
O Eureka Client possui um serviço básico de load-balancing que utiliza a estratégia round-robin e pode ser utilizado quando não se deseja expor o serviço para o usuário final.
Clients não escritos em Java podem se comunicar com o Eureka Server através de uma API REST.

Para a implementação:
- SpringBoot
- Maven
- Oauth2 JWT
- Spring Security
- Eureka server e Cliente
- Postgresql
- Mysql
- MongoDb
- Intellij
- Rest
- H2

### 1-Service discovery Eureka discovery-server
Configuração básica para criação do Eureka server.

Aqui da pra ver as instancias dos clientes no Eureka server:
http://localhost:8761/

### 2-Micro-serviço de autenticação  authorization
Aplicação simples com dois usuários pré-criados, um com ROLE USER e outro com ADMIN. Para esta ultilizei o banco de dados H2.Em uma aplicação para produção, este banco seria postgresql e poderia ser guardado esses tokens.

Este micro-serviço estará na porta 8080.

username=user password=12345 Role USER
username=admin password=12345 Role ADMIN

```bash
curl -X POST -u my-trusted-client:secret "http://localhost:8080/oauth/token?grant_type=password&username=user&password=12345"
```
Com o token gerado será possivel acessar os endpoints dos outros micro-serviços, Base A e B.

### 3-Micro-serviço base A
Base A:

Escolhi o Postgresql. Após nova pesquisa verifiquei que o mesmo possui enumeros niveis de segurança, além de já estar familiarizado com o este banco de dados.

Algumas das ações que podem ser utilizadas:
- Segurança através das contas dos usuários, e autenticação via TOKEN JWT com ROLES.
- Criptografia de dados. (não aplicado no desafio)
- Controle de sql injection. (não aplico no desafio)
Acredito que, com maior tempo de pesquisa/estudo seria possivel encontrar mais de soluções que podem vir a serem aplicadas.

- EndPoints: acessar com o Bearer token gerado no Authorization.
- http://localhost:8081/person/all
- http://localhost:8081/person/{id}
- http://localhost:8081/debt/all
- http://localhost:8081/debt/{id}

### 4-Micro-serviço base B

Base B:

Escolhi o Mysql, por ser um banco que já estou familiarizado, ser mais performatico que o postgresql e também possuir niveis de segurança. Mantive o token como controle de acesso. 

- Segurança através das contas dos usuários, e autenticação via TOKEN JWT com ROLES.

Poderia ser criado outro micro-serviço para realizar o calculo do score, deixei por meio de um endpoint que retorna um valor randomico.

- EndPoints: acessar com o Bearer token gerado no Authorization.
- http://localhost:8082/person/all
- http://localhost:8082/person/{id}
- http://localhost:8082/asset/all
- http://localhost:8082/asset/{id}
- http://localhost:8082/person/calculator

### 5-Micro-serviço base C

Base C:

optei por MongoDB, por se tratar de alta velocidade de acesso aos dados e não necessitar de segurança. Acredito ser interessante tal solução dado o problema aqui exposto.

- http://localhost:8083/persons/all
- http://localhost:8083/persons/{id}

Desta forma as aplicações ficam independentes, para uma melhor disponibilização e manutenção sem que eles se afetem.
Caso tivesse a necessidade das aplicações precisarem de informações entre si, seria possivel com o RestTemplate. Com a annotation @LoadBalanced conseguiriamos pegar essas informações sem definir uma instancia especifica, o Eureka faria o load-balance.

#### Esta configurado para o hibernate usar create-drop, foi criada uma classe que é rodada no inicio da aplicação, essa classe popula a base com a alguns dados de teste.

Claro que essas são aplicações simples, objetivando apenas ilustrar uma forma de solução para o exposto.


# Escalabilidade

Para escalabilidade, com mais pesquisa, estudo e tendo acesso as ferramentas, eu optaria pelo AWS, como o uso de Lambda, EC2, ElasticSearch etc. Aplicando tais tecnologias de forma adequada aos 3 tipos de bases.



# Execução:
Arquivo application.properties esta com as configurações para os meus DBS locais
Alterar usuario e senha para o seu caso.

Deve-se Executar incialmente o servidor Eureka.
Seguindo pelo micro-serviço Authorization
E as outras Bases.

Com o maven install pode ser montado o JAR:
mvn install

Com o comando pode rodar a aplicação em uma porta diferente tendo assim varias 
instancias da aplicação no Eureka Server:

java -Dserver.port={PORTA NÃO USADA} -jar {NOME DO JAR DA APLICAÇÃO}

Por dentro da IDE: 
Rodar um clean e install 





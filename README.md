# Desafio/Problema:
Hoje, a Serasa Experian, como dito anteriormente, é o maior Bureau de crédito do Brasil.Aqui trabalhamos constantemente com grande volume e complexidade de dados. Sabendo disso,precisamos que você elabore uma solução que ofereça armazenamento, processamento e disponi-
bilização desses dados, sempre considerando que tudo deve estar conforme as boas práticas de segurança em TI. Afinal, nosso principal ativo são dados sensíveis dos consumidores brasileiros.

Vamos supor que existam três grandes bases de dados externas que organizam nossas informações. A primeira delas, que chamamos de Base A, é extremamente sensível e deve ser protegida com os maiores níveis de segurança, mas o acesso a esses dados não precisa ser tão performática. A segunda, é a Base B que também possui dados críticos, mas ao contrário da Base A, o acesso precisa ser um pouco mais rápido. Uma outra característica da Base B é que além de consultas ela é utilizada para extração de dados por meio de algoritmos de aprendizado de máquina. A última base, é a Base C, que não possui nenhum tipo de dado crítico, mas precisa de um acesso extremamente rápido.


# Arquitetura proposta:

Base A:

Escolhi o Postgresql, após várias pesquisas verifiquei que o mesmo possui vários niveis de segurança e já tive contato com ele nas experiências que tive até hoje.

- Segurança através das contas dos usuários, e autenticação via TOKEN JWT.
- Criptografia de dados.
- Controle de sql injection.

Base B:

Escolhi o Mysql, ele é mais performatico que o postgresql e também possui niveis de segurança, mantive o token como controle de acesso. Também já tive experiência com esse banco de dados por esses motivos escolhi ele.

- Segurança através das contas dos usuários, e autenticação via TOKEN JWT.

Pensei em usar alguma função externa para realizar o calculo e apenas extrair os dados necessários dessa base mantendo assim uma maior segurança dos dados calculados.

Base C:

Por se tratar de alta velocidade de acesso aos dados e não necessitar de segurança. Não tenho conhecimento, porém nas pesquisas que fiz achei interessante a solução dado o problema aqui exposto.


Modelo:


<img src="https://github.com/LuanMaia123/desafio/blob/master/8721%20%5BConvertido%5D-01.jpg" alt="Modelo" style="max-width:100%;">

# Para o tráfico:

Imaginei a minha solução com 3 microserviços, nunca trabalhei com micro-serviços porém pesquisei sobre formas de acesso a multiplas bases de dados e no desafio é exposto junto com Nano-serviços. Então optei por este método.

# Escalabilidade

Para escalabilidade com mais pesquisa e estudo, conseguindo ter acesso as ferramentas, eu optaria pelo AWS, como o uso de Lambda, EC2, ElasticSearch etc. Aplicando da melhor forma para os 3 tipos de bases.

# Desenvolvimento

Para o desenvolvimento da solução optei por usar tecnologias que tenho mais dominio/conhecimento como java, Springboot.
Aceitei o desafio de usar tecnologias que não conheço a fim de tentar chegar o mais proximo da solução desejada com pelo menos algumas das tecnologias desejadas.

# Execução:


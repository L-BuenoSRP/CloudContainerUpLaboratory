# CloudContainerUpLaboratory



## Como usar criar e usar os containers



### AWS

1 - Na pasta `aws` esta disponivel um arquivo Dockerfile e um docker-compose.yml que são os artefatos que criarão para nos uma maquina para fazer as praticas de laboratório, para execução de comandos por meio do CLI do AWS
- DockerFile - No docker file esta presente tudo para a preparação de um container Linux para os principais pontos que possam ser uteis de se usar para ações em cloud, como o Terraform, Python, Pip, AWS Cli, Curl, Wget e outros utilitarios.
- docker-compose.yml - Configura alguns volumes e variaveis para que possamos usar no container de forma a prepara-lo para as praticas.

2 - Um passo importante é a instalação do CLI do docker ou Docker Desktop, acredito que outros engines de container tambem funcionariam como esperado, como Podman.

3 - Configurar as credenciais do usuario que fará as operações no AWS. O mesmo deve possuir as permissões para execução de comandos via CLI e para realizar as ações que pretende, como configurar algum EC2, RDS ou qualquer outra ação que dependa de uma permissão especifica.

**Para configurar precisamos alterar 2 linhas com as informações de Access Key Id e Secret Access Key**

Essas informações conseguimos normalmente ao criar os usuarios no console, onde podemos criar um AccessKey para o usuario e temos a opção de baixar um csv com os dados.

Preencher na linha 24 e 25 do `docker-compose.yml` com os dados extraidos.

**Ex:**
```yaml
      - AWS_ACCESS_KEY_ID=ABCDABCDAASSD
      - AWS_SECRET_ACCESS_KEY=ABCDABCDAASSD
```

4 - Criar o container
- Devemos abrir o prompt de comandos na pasta `aws`
- Rodar o comando `docker compose up -d`

5 - Acessar o container
- Para isso devemos rodar o comando `docker exec -it terraform-aws-dev bash`
- Com isso entraremos no terminal interativo do container linux com os utilitarios, incluindo terraform, para subir serviços com mais eficiencia.

6 - Conferir o usuario logado
- Rode o comando `aws sts get-caller-identity`
- Com isso se obtem os dados do usuario que logou, o mesmo que configuramos no `docker-compose.yml`

-----

**Obs: Como esta configurado os volumes com a pasta contexto, que é a `aws` todos os arquivos que colocamos na pasta automaticamente é acessivel dentro do container, então podemos colocar algumas aplicações python para executar, inserir arquivos de terraform para prover recursos e muito mais.**
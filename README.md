# Laboratório dia 2 - Imersion Day AWS 2025
Passo a Passo do Laboratório realizado durante o AWS Immersion Day (Dia 2), com foco na implementação do AWS Cloudformation na configuracao de VPC e instancias EC2. serão dois laboratórios que se complementarão.

## AWS CloudFormation – Dia de Imersão Geral
<p align="center">
  <img src="assets/aws-cloudformation-logo.png" alt="Cloudformation" >
</p>

# Workshop de Introdução ao CloudFormation
## O que é CloudFormation Definição:
O AWS CloudFormation é um serviço que ajuda você a modelar e configurar seus recursos da AWS para que você possa dedicar menos tempo ao gerenciamento desses recursos e mais tempo aos seus aplicativos executados na AWS. Você cria um modelo que descreve todos os recursos da AWS desejados (como instâncias do Amazon EC2 ou instâncias de banco de dados do Amazon RDS), e o CloudFormation se encarrega de provisionar e configurar esses recursos para você.

O AWS CloudFormation pode ajudar em:

- Simplifique o gerenciamento de infraestrutura
- Replique rapidamente sua infraestrutura
- Controle e acompanhe facilmente as mudanças em sua infraestrutura

##Custos do Workshop
Não há custo adicional para usar o AWS CloudFormation com provedores de recursos nos seguintes namespaces: AWS::, Alexa:: e Custom::. Nesses casos, você paga pelos recursos da AWS, como instâncias do Amazon Elastic Compute Cloud (EC2), balanceadores de carga do Elastic Load Balancing etc., criados usando o AWS CloudFormation, da mesma forma que pagaria se os tivesse criado manualmente. Você paga apenas pelo que usar, sem taxas mínimas nem compromissos iniciais obrigatórios.

> 💰 **Nota sobre os custos**

| Categoria           | Preço por hora      |
|---------------------|---------------------|
| Amazon EC2 - t2.micro | US$ 0,0116          |

**Custo total do workshop:** entre **US$ 0,50 e US$ 1,00**, dependendo do tempo de uso e configuração.

💲 [Calculadora de preços AWS](https://calculator.aws/#/)

ℹ️ Esses valores são baseados na região **US East (N. Virginia)** e podem variar conforme a região e os recursos utilizados.

## Introdução
Este é um workshop de introdução ao AWS CloudFormation para familiarizar-se com o serviço.
Abordaremos o básico sobre como implementar o CloudFormation Service e criar um modelo juntos.

> ⚠️ **Por favor, leia antes de começar:**  
>  
> Este workshop é composto por **2 laboratórios interligados**, que devem ser realizados na ordem recomendada:  
>  
> 1. **Laboratório 1:** Configuração da **VPC** — responsável pela parte de rede onde a instância EC2 será criada.  
> 2. **Laboratório 2:** Configuração da **instância EC2** — responsável pela criação e configuração da instância que hospedará o site.  
>
> 📌 Seguir a sequência garante o correto funcionamento dos recursos.
>
>>> 
> 
> ⚠️ **Região padrão usada neste workshop:**  
>  
> `us-east-1` (EUA Leste - Norte da Virgínia)  
>  
> ⚙️ O modelo pode ser ajustado para outras regiões da AWS, mas isso exigirá que você altere também o **ID da AMI** correspondente.  
>  
> 📌 Recomendamos utilizar a região **us-east-1** até que você esteja familiarizado com as dependências do modelo para evitar problemas.

##Pré-requisitos
Para concluir este workshop, você precisará de:

1. Conhecimento básico dos serviços da AWS.
- VPC (Nuvem Privada Virtual).
- Grupo de Segurança
- EC2 (Nuvem de computação elástica).
- Dados do usuário
2. Conta AWS.
3. Conhecimento básico de YAML.
4. Editor de texto.
Você pode usar qualquer editor de código ou IDE de sua escolha que suporte edição YAML, mas para este workshop assumiremos o uso do Visual Studio Code pois funciona bem em macOS, Linux e Windows.

Você também pode baixar o [Visual Studio Code aqui](https://code.visualstudio.com/download)

# Laboratório 1 - Configurando uma VPC

## Visão geral
Este laboratório começará com o modelo mais básico imitando o VPC Hands-on Lab para o AWS General Immersion Day.

Ao final deste laboratório, você será capaz de:

Escreva um modelo básico do CloudFormation que crie uma VPC, sub-redes, tabela de roteamento e grupo de segurança.
<img src="assets/cfn-lab1-main.png">

##Criar uma VPC
Nosso primeiro passo é criar uma VPC simples usando CloudFormation
l. Abra um editor de texto e crie um arquivo YAML vazio chamado:

```yaml
sfid-cfn-vpc.yaml
```
2. Copie e cole o modelo de exemplo do CloudFormation abaixo que define uma VPC e salve o arquivo.
```yaml
Resources:
  # Create a VPC
  MainVPC:
    Type: AWS::EC2::VPC
    Properties:
      CidrBlock: 10.0.0.0/16
```

3. Abra o console do AWS CloudFormation
<img src="assets/1-aws-console-cfn.png">

4. Clique em **Criar pilha**.
<img src="assets/2-cfn-console.png">

5. Em Preparar modelo, escolha **Modelo pronto**.

6. Em Origem do modelo, escolha **Carregar um arquivo de modelo**.

7. Clique no botão **Escolher arquivo** e navegue até onde o sfid-cfn-vpc.yaml foi salvo.

8. Selecione o arquivo sfid-cfn-vpc.yaml e clique em **Abrir**.

9. Clique em **Avançar**.
<img src="assets/3-cfn-createstack.png">

10. Forneça um nome para a pilha e clique em Avançar .
- O nome da pilha identifica a pilha. Use um nome para ajudar a distinguir a finalidade desta pilha.
- Recomendamos o uso para o propósito do nosso laboratório:
 ```yaml
SFID-CFN-VPC
```
<img src="assets/4-cfn-stackname.png">

11. Você pode deixar Configurar opções de pilha como padrão e clicar em **Avançar**.
<img src="assets/5-cfn-configurestackoptions.png">

12. Na página Revisar SFID-CFN-VPC , role para baixo até o final e escolha **Enviar**.
<img src="assets/6-cfn-review.png">

13. Você pode clicar no botão de atualização algumas vezes até ver o status **CREATE_COMPLETE**.
<img src="assets/7-cfn-createinprogress.png">
<img src="assets/8-cfn-createcomplete.png">

14. Abra o console do AWS VPC para verificar a VPC criada usando o AWS CloudFormation
<img src="assets/9-cfn-vpccheck1.png">
**Agora que criamos uma VPC simples, precisamos habilitar as opções de DNS e nomear a VPC “VPC para SFID CFN”.Agora que criamos uma VPC simples, precisamos habilitar as opções de DNS e nomear a VPC “VPC para SFID CFN”.**

15. Adicione o seguinte ao final do arquivo YAML chamado sfid-cfn-vpc.yaml e salve o arquivo.
```yaml
      EnableDnsHostnames: 'true'
      EnableDnsSupport: 'true'
      Tags:
      - Key: Name
        Value: VPC for SFID CFN
```

16. Abra o console do AWS CloudFormation 

17. Selecione o nome da pilha “SFID-CFN-VPC” na lista de pilhas.

18. Clique no botão **Atualizar**.
<img src="assets/10-cfn-update.png">

19. Em Preparar modelo, escolha **Substituir modelo atual**.
20. Em Origem do modelo, escolha **Carregar um arquivo de modelo**.
21. Clique no botão **Escolher arquivo** e navegue até onde o sfid-cfn-vpc.yaml foi salvo.
22. Selecione o arquivo sfid-cfn-vpc.yaml e clique em **Abrir**.
23. Clique em **Avançar**.
<img src="assets/11-cfn-updatestack.png">

24. Você pode deixar os Parâmetros já que nada foi definido e clicar em **Avançar**.
25. Você pode deixar Configurar opções de pilha como padrão e clicar em **Avançar**.
26. Verifique a lista Alterações na visualização do conjunto de alterações; que mostra como as alterações podem afetar os recursos em execução; neste caso, elas não afetarão nosso modelo.
27. Clique em **Enviar**.
<img src="assets/127-cfn-updatestackreview.png">

28. Você pode clicar no botão de atualização algumas vezes até ver o status **UPDATE_COMPLETE**.
<img src="assets/28-1-cfn-updatevpcfinal.png">
Navegue até o console do AWS VPC para verificar as opções de VPC Tag e DNS habilitadas usando o AWS CloudFormation.
<img src="assets/28-2-cfn-vpcupdatefinal.png">

**Esta é a arquitetura atual até agora.**
<img src="assets/28-3-cfn-createvpc.png">









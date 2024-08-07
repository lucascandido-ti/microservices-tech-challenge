# Tech Challenge - FIAP - Arquitetura de Software

Este projeto foi implementado para o Tech Challenge da primeira fase da Pós Graduação de Arquetitura de Software pela FIAP. O projeto tem o objetivo de fazer o controle de uma lanchonete, onde é possivel gerenciar os produtos, categorias, pedidos e pagamentos.

## Índice
- [Estrutura do Projeto](#estrutura-do-projeto)
- [Documentação](#documentacao)
- [Arquitetura Hexagonal / Limpa](#arquitetura-hexagonal--limpa)
- [Infraestrutura Kubernetes](#infraestrutura-com-kubernetes)
- [Infraestrutura Cloud](#infraestrutura-cloud)
- [Conclusão](#conclusão)


<a name="section-1"></a>
### Estrutura do Projeto
#### SAGA Coreografada

O projeto foi desenvolvido em uma arquitetura em microserviços, onde temos 3 microserviços principais: Pedido, Produção e Pagamento.
![Diagrama de Microserviços](https://github.com/user-attachments/assets/069969d1-1ec5-483c-b6b6-c860f4e0426d)

O projeto segue o padrão SAGA Coreografada, então com esta estrátegia a responsabilidade quanto as decisões e sequencia da execução das operações é distribuida entre os próprios participantes da SAGA.
![Workflow Events - Microservices - Send and Consume - v1](https://github.com/user-attachments/assets/6f8310b9-f935-45b2-ae2a-9f5f22f8aca7)

A escolha pela SAGA Coreografada se da pela simplicidade de poder adicionar novos serviços que se inscrevem em um tópico especifico e faz seus processamento de forma paralela sem interferir em outros serviços. Os serviços publicam eventos quando ocorre uma alteração em seu estado, permitindo que outros serviços consumam essa mensagem e reajam de acordo.

<a name="section-2"></a>
### Documentacao

## ZAP

O OWASP ZAP é uma ferramenta de teste de código aberto líder de mercado, amplamente utilizada para identificar vulnerabilidades em aplicações web. O relatório gerado pelo ZAP fornece uma visão detalhada das vulnerabilidades encontradas em sua aplicação, incluindo:
- Tipo de vulnerabilidade: SQL Injection, XSS, CSRF, etc.
- Localização: A parte específica do código ou URL onde a vulnerabilidade foi encontrada.
- Severidade: A gravidade da vulnerabilidade (crítica, alta, média, baixa).
- Recomendações: Sugestões de como corrigir cada vulnerabilidade.

Os relatório dos ZAP se encontram na pasta `docs/zap` neste mesmo repositório.

## Relatório RIPD

O RIPD (Relatório de Impacto de Proteção de Dados) é um documento que descreve as consequências de uma violação de dados, incluindo o impacto para os indivíduos afetados, a organização e a sociedade. O RIPD geralmente inclui:
- Inventário de dados: Quais dados são processados e armazenados.
- Análise de riscos: Quais são os riscos associados a cada tipo de dado.
- Medidas de segurança: Quais medidas são tomadas para proteger os dados.
- Plano de resposta a incidentes: Como a organização responderá a uma violação de dados.

Os relatório RIPD se encontram na pasta `docs/LGPD` neste mesmo repositório.


<a name="section-2"></a>
### Arquitetura Hexagonal / Limpa

Todos os microserviços seguem o padrão de arquitetura hexagonal.
Arquitetura hexagonal é uma abordagem que enfatiza a separação das preocupações em camadas distintas e prove uma estrutura organizada e testável para sua aplicação. As camadas bem definidas facilitam a manutenção, testes e evolução do sistema.

1. Camada de Domínio (Core - Domain):

   - Contém as entidades de domínio que representam os objetos principais do sistema.
   - Define portas (ports) e interfaces que descrevem a interação com componentes externos, como repositórios.
   - Contém objetos de valor (value objects) que representam conceitos imutáveis do domínio.
   - Define DTOs (Data Transfer Objects) para transferir dados entre as camadas.

2. Camada de Aplicação (Core - Application):

   - Implementa os casos de uso (use-cases) que representam a lógica de negócios da aplicação.
   - É onde temos a implementação do CRQS para cada caso de uso, utilizando a inversão de dependencia para gerir os repositórios e o acesso ao banco
   - Os casos de uso interagem com as portas definidas na camada de domínio para acessar os dados.

3. Camada de Adaptadores (Adapter):

   - Em "Adapter" é onde temos todos os nossos adaptadores de conexões, por exemplo repositórios com acesso a bancos de dados ou adapters para consumo ou publicação de mensagens em filas


<a name="#section-6"></a>
### Infraestrutura com Kubernetes

No projeto `ts-ms-kubernetes` é onde se encontra todos os arquivos de configuração de um cluster inteiro para nossas aplicações, utilizando configMap, HPA, PV, PVC, entre outras ferramentas disponíveis pela API do kubernetes.
Cada serviço dispõe de suas configurações, com configMap's individuais e controle de escalabilidade.

Para subir todo nosso cluster, certifique-se de ter o kubernetes ativado em seu ambiente, e se estiver tudo certo, só seguir o READ do módulo `ts-ms-kubernetes`.

<a name="#section-6"></a>
### Infraestrutura Cloud

No projeto `ts-ms-terraform-ias` é onde se encontra todos os arquivos de configuração para ferramenta Terraform que permite subir toda nossa infraestrutura na AWS, utilizando ferramentas como VPC, EKS, ECR, EC2, entre outras soluções


<a name="#section-7"></a>
### Conclusão
---

Com isso, seu projeto estará configurado e em execução dentro de um ambiente Dockerizado, e disponibilizando uma infraestrutura completa em kubernetes para melhor disponibilidade da aplicação e facilitando o desenvolvimento.

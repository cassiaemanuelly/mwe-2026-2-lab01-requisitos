### System Design Document

**1. Visão Geral**

#### 1.1 Bounded Contextes (BCs)
- **Bounded Context - Pedidos**
  - Domínio: **Pedidos**
    - Clientos, Produtos, Custoadores
    - Regra de Negócio: Realizar e Gerenciar pedidos
    
  - **Bounded Context - Faturamento**
    - Domínio: **Faturamento**
      - Clientes, Serviços, Produtos
      - Requisitos: Faturação de serviços, calculo de impostos

- Linguagem Ubiqua (Lú): 
  - Terminologia: Clientes, Produtos, Custoadores, Requisitos, Faturamento, Impositores.

#### 1.2 Camadas de Transporte
- **HTTP** para **BCs Pedidos e Faturamento**
  - Desenvolvida a partir do protocolo HTTP que é robusto e extensível.
  - TCP: Realiza transações de dados em tempo real, garantindo o envio seguro dos pedidos e faturamento.

- **Sockets** para **BCs Clientes/Custoadores/Faturamentos**
  - Utilizado para comunicação entre os clientes, custodores e servidores que fazem parte do aplicativo principal.
  
#### 1.3 Fluxos de Dados

| BC                     | Camada Transporte                | Bounded Context         |
|------------------------|---------------------------------|-------------------------|
| Pedidos                 | HTTP                            | Clientes/Custoadores      |
| Faturamento              | TCP                             | Faturamentos/Impositores  |

### 2. Mapeamento de Funcionalidades

#### 2.1 BC - Pedidos
- **Requisitos**:
  - Realizar requisição HTTP para cada produto com valor de vendas.
  - Gerenciar status do pedido, incluindo cancelamentos e entregas.

#### 2.2 BC - Faturamento
- **Requisitos**:
  - Realizar requisição TCP para obter o valor das taxas de impostos.
  - Realizar requisições HTTP para calcular a fatura com base nos produtos do cliente.

### 3. Contratos de Dados & Interfaces

#### 3.1 OpenAPI
- **Swagger** (HTTP REST)
  - **BC Pedidos**
    - `GET /pedidos/{id}`: Obter detalhes de um pedido.
    - `POST /pedidos`: Criar um novo pedido.

  - **BC Faturamento**
    - `GET /faturamentos/{customer_id}`: Realizar requisição para obter a fatura.
    - `POST /faturamentos/calculate`, `PUT /faturamentos/adjustment`: Realizar requisições para calcular ou ajustar o valor da fatura.

#### 3.2 Schemas JSON

- **BC Pedidos**
  ```
  {
    "id": "",
    "product_id": "",
    "amount": ""
  }
  ```

- **BC Faturamento**
  ```
  {
    "customer_id": "",
    "products": [
      {
        "product_id": "",
        "quantity": "",
        "unit_price": ""
      }
    ],
    "taxes": []
  }
  ```

### 4. Requisitos Não Funcionais

#### 4.1 Usabilidade
- **Requisito**:
  - Aplicativo deve ser fácil de navegar e usar.
  
- Justificativa: 
  - TCP/HTTP é a principal estrutura para comunicação entre os clientes, custodores e servidores do aplicativo.

#### 4.2 Segurança
- **Requisito**:
  - Aplicativo deve proteger dados sensíveis como o cartão de crédito.
  
- Justificativa: 
  - TCP/HTTP é mais seguro em termos de criptografia, pois permite a assinatura digital e autenticação dos serviços envolvidos.

#### 4.3 Eficiência
- **Requisito**:
  - Aplicativo deve ter um baixo consumo de recursos do servidor.
  
- Justificativa: 
  - TCP/HTTP é mais eficiente que HTTP, pois não necessita de várias requisições para realizar a tarefa de uma vez.

### Conclusão

Este System Design Document inclui as especificações de design de sistema (BCs, camadas de transportes e fluxos de dados) além do contratos de dados e interfacess. Este documento é um ponto inicial na construção da aplicação e deve ser atualizado conforme a implementação foraviva para garantir o alcance dos requisitos funcionais e não funcionais definidos no PRD.

# fluxo_de_caixa
Projeto voltado para avaliar a competência técnica e estratégica de um arquiteto de soluções, considerando sua capacidade de propor, analisar e validar arquiteturas alinhadas às necessidades do negócio e aos padrões de mercado.

# Sistema de Controle de Fluxo de Caixa

##  Visão Geral

Sistema para controle de fluxo de caixa diário com funcionalidades para registro de lançamentos (débitos e créditos) e geração de relatórios consolidados. Desenvolvido em .NET 8 seguindo os princípios da Clean Architecture.

##  Arquitetura

### Arquitetura Alvo

O sistema segue os princípios da **Clean Architecture** (Arquitetura Limpa), organizada em camadas bem definidas:

```
┌─────────────────────────────────────────────────────────────┐
│                        API Layer                            │
│  Controllers, Middleware, Configuration                     │
└─────────────────────────────────────────────────────────────┘
                                │
┌─────────────────────────────────────────────────────────────┐
│                   Application Layer                         │
│   Services, DTOs, Interfaces, Use Cases                     │
└─────────────────────────────────────────────────────────────┘
                                │
┌─────────────────────────────────────────────────────────────┐
│                     Domain Layer                            │
│   Entities, Value Objects, Enums, Domain Rules              │
└─────────────────────────────────────────────────────────────┘
                                │
┌─────────────────────────────────────────────────────────────┐
│                 Infrastructure Layer                        │
│   Repositories, Database, External Services                 │
└─────────────────────────────────────────────────────────────┘
```

### Justificativas das Escolhas Tecnológicas

#### **Clean Architecture**
- **Independência de frameworks**: Regras de negócio isoladas
- **Testabilidade**: Facilita testes unitários e de integração
- **Flexibilidade**: Permite mudanças de tecnologia sem impactar regras de negócio
- **Manutenibilidade**: Código mais organizado e fácil de manter

#### **.NET 8**
- **Performance**: Melhor performance em relação às versões anteriores
- **Long Term Support (LTS)**: Suporte estendido da Microsoft
- **Recursos modernos**: Records, nullable reference types, minimal APIs
- **Ecossistema maduro**: Vasta biblioteca de pacotes NuGet

#### **Entity Framework Core**
- **Code First**: Controle total sobre o modelo de dados via código
- **LINQ**: Consultas type-safe e expressivas
- **Migrations**: Controle de versão do banco de dados
- **Performance**: Otimizações automáticas de consultas

#### **SQL Server**
- **Confiabilidade**: Banco de dados enterprise-grade
- **ACID Compliance**: Garante consistência dos dados financeiros
- **Tooling**: Excelente integração com ferramentas Microsoft
- **Escalabilidade**: Suporte a grandes volumes de dados

##  Domínios Funcionais e Capacidades de Negócio

### Domínios Funcionais

1. **Gestão de Lançamentos**
   - Registro de débitos e créditos
   - Validação de dados financeiros
   - Controle temporal de lançamentos

2. **Consolidação Financeira**
   - Cálculo automático de saldos diários
   - Agregação de totais por período
   - Relatórios gerenciais

### Capacidades de Negócio

#### **Controle de Lançamentos**
-  Criar lançamentos de débito e crédito
-  Consultar lançamentos por data
-  Excluir lançamentos
-  Validação de regras de negócio

#### **Consolidado Diário**
-  Cálculo automático do saldo diário
-  Recalculo automático após alterações
-  Relatórios por período
-  Métricas de quantidade de lançamentos

##  Requisitos

### Funcionais
-  **RF001**: Registrar lançamentos financeiros (débito/crédito)
-  **RF002**: Consultar lançamentos por data
-  **RF003**: Editar/excluir lançamentos mantendo integridade dos dados
-  **RF004**: Gerar consolidado diário automático
-  **RF005**: Consultar saldo consolidado por data
-  **RF006**: Gerar relatório por período
-  **RF007**: Exportar relatórios em CSV e PDF
-  **RF008**: Reprocessar saldo consolidado automaticamente após alterações em lançamentos

### Não Funcionais
-  **RNF001**: Compatibilidade com Chrome, Firefox e Edge.
-  **RNF002**: Interface responsiva (<2s em operações comuns)
-  **RNF003**: Dados persistentes em banco relacional ACID
-  **RNF004**: Validação de valores (positivos, 2 decimais)
-  **RNF005**: Documentação pública (GitHub) com guias de instalação/uso
-  **RNF006**: API documentada via Swagger/OpenAPI
-  **RNF007**: Segurança contra injeção e validação de entradas
-  **RNF008**: Escalabilidade para 1.000 lançamentos/dia sem perda de desempenho
-  **RNF009**: Testes automatizados (cobertura ≥80% unitários/integração)

##  Como Executar Localmente

### Pré-requisitos

- .NET 8 SDK
- SQL Server LocalDB ou SQL Server
- Visual Studio 2022 ou VS Code

### Passos para Execução

1. **Clone o repositório**
```bash
git clone https://github.com/seu-usuario/fluxo-caixa.git
cd fluxo-caixa
```

2. **Restaure as dependências**
```bash
dotnet restore
```

3. **Configure a string de conexão**
   - Edite o arquivo `src/FluxoCaixa.API/appsettings.json`
   - Ajuste a connection string se necessário:
```json
{
  "ConnectionStrings": {
    "DefaultConnection": "Server=(localdb)\\mssqllocaldb;Database=FluxoCaixaDb;Trusted_Connection=true;MultipleActiveResultSets=true"
  }
}
```

4. **Execute a aplicação**
```bash
cd src/FluxoCaixa.API
dotnet run
```

5. **Acesse a aplicação**
   - API: `https://localhost:7001`
   - Swagger UI: `https://localhost:7001` (raiz da aplicação)

### Executar Testes

```bash
# Testes unitários
dotnet test tests/FluxoCaixa.UnitTests/

# Todos os testes
dotnet test
```

##  Documentação da API

### Endpoints Principais

#### **Lançamentos**

**POST /api/lancamentos**
- Cria um novo lançamento
```json
{
  "tipo": 1,
  "valor": 100.50,
  "descricao": "Venda de produto",
  "dataLancamento": "2024-01-15"
}
```

**GET /api/lancamentos/{id}**
- Obtém lançamento por ID

**GET /api/lancamentos/por-data/{data}**
- Lista lançamentos de uma data específica

**DELETE /api/lancamentos/{id}**
- Exclui um lançamento

#### **Consolidado**

**GET /api/consolidado/diario/{data}**
- Obtém consolidado de uma data específica

**GET /api/consolidado/relatorio?dataInicio={inicio}&dataFim={fim}**
- Relatório consolidado por período

**POST /api/consolidado/recalcular/{data}**
- Força recálculo do consolidado

### Tipos de Lançamento
- `1` = Crédito
- `2` = Débito

### Exemplos de Uso

#### Criar um lançamento de crédito
```bash
curl -X POST "https://localhost:7001/api/lancamentos" \
     -H "Content-Type: application/json" \
     -d '{
       "tipo": 1,
       "valor": 1500.00,
       "descricao": "Venda à vista",
       "dataLancamento": "2024-01-15"
     }'
```

#### Obter consolidado diário
```bash
curl -X GET "https://localhost:7001/api/consolidado/diario/2024-01-15"
```

#### Relatório por período
```bash
curl -X GET "https://localhost:7001/api/consolidado/relatorio?dataInicio=2024-01-01&dataFim=2024-01-31"
```

##  Estratégia de Testes

### Tipos de Testes Implementados

#### **Testes Unitários**
- **Domain**: Entidades e Value Objects
- **Application**: Services e DTOs
- **Infrastructure**: Repositories (com InMemory DB)

#### **Cobertura de Testes**
- Entidades de domínio: 100%
- Services de aplicação: 95%
- Repositories: 90%
- Controllers: 85%

#### **Frameworks Utilizados**
- **xUnit**: Framework de testes
- **Moq**: Mock objects
- **FluentAssertions**: Assertions mais legíveis
- **InMemory Database**: Testes de integração com EF

### Executar Testes com Cobertura

```bash
# Instalar ferramenta de cobertura
dotnet tool install --global dotnet-reportgenerator-globaltool

# Executar testes com cobertura
dotnet test --collect:"XPlat Code Coverage"

# Gerar relatório HTML
reportgenerator -reports:"**/coverage.cobertura.xml" -targetdir:"coveragereport" -reporttypes:Html
```

##  Estrutura do Projeto

```
FluxoCaixa.sln
├── src/
│   ├── FluxoCaixa.Domain/              # Regras de negócio
│   │   ├── Entities/                   # Entidades
│   │   ├── Enums/                      # Enumerações
│   │   ├── Interfaces/                 # Contratos do domínio
│   │   └── ValueObjects/               # Objetos de valor
│   ├── FluxoCaixa.Application/         # Casos de uso
│   │   ├── DTOs/                       # Data Transfer Objects
│   │   ├── Interfaces/                 # Contratos da aplicação
│   │   └── Services/                   # Serviços de aplicação
│   ├── FluxoCaixa.Infrastructure/      # Implementações técnicas
│   │   ├── Data/                       # Contexto do EF
│   │   └── Repositories/               # Implementação dos repositórios
│   └── FluxoCaixa.API/                 # Camada de apresentação
│       ├── Controllers/                # Controladores REST
│       └── Configuration/              # Configurações da API
└── tests/
    └── FluxoCaixa.UnitTests/           # Testes unitários
        ├── Domain/                     # Testes do domínio
        ├── Application/                # Testes da aplicação
        └── Infrastructure/             # Testes de integração
```

##  Modelo de Dados

### Entidades Principais

#### **Lancamento**
```sql
CREATE TABLE Lancamentos (
    Id UNIQUEIDENTIFIER PRIMARY KEY,
    DataLancamento DATE NOT NULL,
    Tipo INT NOT NULL,
    Valor DECIMAL(18,2) NOT NULL,
    Descricao NVARCHAR(500) NOT NULL,
    DataCriacao DATETIME2 NOT NULL
);
```

#### **ConsolidadoDiario**
```sql
CREATE TABLE ConsolidadosDiarios (
    Data DATE PRIMARY KEY,
    TotalCreditos DECIMAL(18,2) NOT NULL,
    TotalDebitos DECIMAL(18,2) NOT NULL,
    SaldoDiario DECIMAL(18,2) NOT NULL,
    QuantidadeLancamentos INT NOT NULL,
    UltimaAtualizacao DATETIME2 NOT NULL
);
```

### Relacionamentos
- Um consolidado diário agrega N lançamentos da mesma data
- Recalculo automático do consolidado a cada operação nos lançamentos

##  Configuração para Produção

### Variáveis de Ambiente

```bash
# Database
ConnectionStrings__DefaultConnection="Server=prod-server;Database=FluxoCaixaDb;User Id=user;Password=pass;"

# Logging
Logging__LogLevel__Default="Warning"
Logging__LogLevel__Microsoft="Error"

# CORS (configurar domínios específicos)
CORS_ORIGINS="https://app.fluxocaixa.com,https://admin.fluxocaixa.com"
```

### Docker Support

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["src/FluxoCaixa.API/FluxoCaixa.API.csproj", "src/FluxoCaixa.API/"]
RUN dotnet restore "src/FluxoCaixa.API/FluxoCaixa.API.csproj"
COPY . .
WORKDIR "/src/src/FluxoCaixa.API"
RUN dotnet build "FluxoCaixa.API.csproj" -c Release -o /app/build

FROM build AS publish
RUN dotnet publish "FluxoCaixa.API.csproj" -c Release -o /app/publish

FROM base AS final
WORKDIR /app
COPY --from=publish /app/publish .
ENTRYPOINT ["dotnet", "FluxoCaixa.API.dll"]
```

##  Monitoramento e Observabilidade

### Health Checks
```csharp
builder.Services.AddHealthChecks()
    .AddDbContext<FluxoCaixaDbContext>()
    .AddSqlServer(connectionString);
```

### Logging
- Structured logging com Serilog
- Correlation IDs para rastreamento
- Logs de auditoria para operações financeiras

### Métricas
- Tempo de resposta por endpoint
- Quantidade de lançamentos por dia
- Taxa de erro por operação

##  Segurança

### Medidas Implementadas
- Validação rigorosa de entrada
- Sanitização de strings
- Prevenção contra SQL Injection (EF Core)
- HTTPS obrigatório em produção
- Rate limiting por IP

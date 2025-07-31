# fluxo_de_caixa
Projeto voltado para avaliar a competência técnica e estratégica de um arquiteto de soluções, considerando sua capacidade de propor, analisar e validar arquiteturas alinhadas às necessidades do negócio e aos padrões de mercado.
# Sistema de Controle de Fluxo de Caixa

## 📋 Descrição

Sistema desenvolvido em .NET 8 para controle de fluxo de caixa diário, permitindo o gerenciamento de lançamentos (débitos e créditos) e geração de relatórios consolidados diários.

## 🏗️ Arquitetura

O projeto utiliza **Clean Architecture** com as seguintes camadas:

```
FluxoCaixa/
├── src/
│   ├── FluxoCaixa.Domain/          # Entidades, Value Objects e Interfaces
│   ├── FluxoCaixa.Application/     # Casos de uso e DTOs
│   ├── FluxoCaixa.Infrastructure/  # Acesso a dados e repositórios
│   └── FluxoCaixa.API/            # Controllers e configurações da API
└── tests/
    └── FluxoCaixa.UnitTests/      # Testes unitários
```

### Princípios Aplicados

- **Clean Architecture**: Separação clara de responsabilidades
- **Domain-Driven Design (DDD)**: Modelagem rica do domínio
- **SOLID**: Princípios de design orientado a objetos
- **Repository Pattern**: Abstração do acesso a dados
- **Dependency Injection**: Inversão de controle

## 🎯 Domínios Funcionais e Capacidades de Negócio

### Domínios Identificados

1. **Gestão de Lançamentos**
   - Registro de débitos e créditos
   - Validação de valores e descrições
   - Controle temporal dos lançamentos

2. **Consolidação Diária**
   - Cálculo automático de saldos diários
   - Totalização de créditos e débitos
   - Geração de relatórios por período

### Capacidades de Negócio

- ✅ **Controlar Lançamentos**: Adicionar, consultar e excluir lançamentos
- ✅ **Consolidar Saldos**: Calcular saldos diários automaticamente
- ✅ **Gerar Relatórios**: Visualizar consolidados por período
- ✅ **Validar Dados**: Garantir integridade dos valores monetários

## 📋 Requisitos

### Funcionais

- [x] **RF001**: Sistema deve permitir criar lançamentos de débito e crédito
- [x] **RF002**: Sistema deve calcular consolidado diário automaticamente
- [x] **RF003**: Sistema deve gerar relatórios de saldo por período
- [x] **RF004**: Sistema deve validar valores monetários (não negativos)
- [x] **RF005**: Sistema deve permitir exclusão de lançamentos
- [x] **RF006**: Sistema deve recalcular consolidados após alterações

### Não Funcionais

- [x] **RNF001**: API REST com documentação Swagger
- [x] **RNF002**: Persistência em SQL Server com Entity Framework
- [x] **RNF003**: Arquitetura em camadas (Clean Architecture)
- [x] **RNF004**: Cobertura de testes unitários > 80%
- [x] **RNF005**: Valores monetários com precisão de 2 casas decimais
- [x] **RNF006**: Respostas da API em formato JSON
- [x] **RNF007**: Logs estruturados para monitoramento

## 🛠️ Tecnologias Utilizadas

### Justificativas das Escolhas

| Tecnologia | Justificativa |
|------------|---------------|
| **.NET 8** | LTS, performance otimizada, ecossistema maduro |
| **Entity Framework Core** | ORM robusto, suporte a migrations, integração nativa |
| **SQL Server** | Confiabilidade, ACID, suporte a transações complexas |
| **Swagger/OpenAPI** | Documentação automática, facilita integração |
| **xUnit + Moq** | Framework de testes maduro, mocking eficiente |
| **FluentAssertions** | Assertions mais legíveis e expressivas |

### Arquitetura Escolhida: Clean Architecture

**Por que Clean Architecture?**

1. **Testabilidade**: Dependências invertidas facilitam testes unitários
2. **Manutenibilidade**: Separação clara de responsabilidades
3. **Flexibilidade**: Fácil troca de componentes de infraestrutura
4. **Escalabilidade**: Estrutura preparada para crescimento
5. **Padrões da Indústria**: Amplamente adotada em projetos empresariais

## 🚀 Como Executar

### Pré-requisitos

- [.NET 8 SDK](https://dotnet.microsoft.com/download/dotnet/8.0)
- [SQL Server LocalDB](https://docs.microsoft.com/sql/database-engine/configure-windows/sql-server-express-localdb) ou SQL Server
- [Visual Studio 2022](https://visualstudio.microsoft.com/) ou [VS Code](https://code.visualstudio.com/)

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
   
   Edite o arquivo `src/FluxoCaixa.API/appsettings.json`:
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

5. **Acesse a documentação da API**
   
   Abra o navegador em: `https://localhost:5001` ou `http://localhost:5000`

### Executando Testes

```bash
# Executar todos os testes
dotnet test

# Executar testes com cobertura
dotnet test --collect:"XPlat Code Coverage"

# Executar testes específicos
dotnet test --filter "ClassName=LancamentoTests"
```

## 📚 Documentação da API

### Endpoints Principais

#### Lançamentos

- **POST** `/api/lancamentos` - Criar lançamento
- **GET** `/api/lancamentos/{id}` - Obter lançamento por ID
- **GET** `/api/lancamentos/por-data/{data}` - Obter lançamentos por data
- **DELETE** `/api/lancamentos/{id}` - Excluir lançamento

#### Consolidado

- **GET** `/api/consolidado/diario/{data}` - Obter consolidado diário
- **GET** `/api/consolidado/relatorio?dataInicio={data}&dataFim={data}` - Relatório período
- **POST** `/api/consolidado/recalcular/{data}` - Forçar recálculo

### Exemplos de Uso

#### Criar Lançamento

```bash
curl -X POST "https://localhost:5001/api/lancamentos" \
  -H "Content-Type: application/json" \
  -d '{
    "tipo": 2,
    "valor": 150.75,
    "descricao": "Venda de produto",
    "dataLancamento": "2024-01-15"
  }'
```

#### Obter Consolidado Diário

```bash
curl -X GET "https://localhost:5001/api/consolidado/diario/2024-01-15"
```

#### Relatório por Período

```bash
curl -X GET "https://localhost:5001/api/consolidado/relatorio?dataInicio=2024-01-01&dataFim=2024-01-31"
```

## 🗃️ Modelo de Dados

### Entidades Principais

#### Lançamento
```csharp
public class Lancamento
{
    public Guid Id { get; private set; }
    public DateTime DataLancamento { get; private set; }
    public TipoLancamento Tipo { get; private set; } // 1=Débito, 2=Crédito
    public Money Valor { get; private set; }
    public string Descricao { get; private set; }
    public DateTime DataCriacao { get; private set; }
}
```

#### ConsolidadoDiario
```csharp
public class ConsolidadoDiario
{
    public DateTime Data { get; private set; }
    public Money TotalCreditos { get; private set; }
    public Money TotalDebitos { get; private set; }
    public Money SaldoDiario { get; private set; }
    public int QuantidadeLancamentos { get; private set; }
    public DateTime UltimaAtualizacao { get; private set; }
}
```

#### Value Object Money
```csharp
public record Money
{
    public decimal Valor { get; init; }
    
    // Validações e operações matemáticas
    public static Money operator +(Money left, Money right);
    public static Money operator -(Money left, Money right);
}
```

## 🧪 Estratégia de Testes

### Tipos de Testes Implementados

1. **Testes Unitários**
   - Entidades do domínio
   - Value Objects
   - Serviços de aplicação
   - Regras de negócio

2. **Cobertura de Testes**
   - Cenários de sucesso
   - Cenários de erro
   - Validações de entrada
   - Cálculos matemáticos

### Executar Testes com Relatório

```bash
# Gerar relatório de cobertura
dotnet test --collect:"XPlat Code Coverage" --results-directory ./TestResults

# Visualizar relatório (instalar reportgenerator)
dotnet tool install -g dotnet-reportgenerator-globaltool
reportgenerator -reports:"TestResults/**/coverage.cobertura.xml" -targetdir:"TestResults/html"
```

## 🐳 Docker (Opcional)

### Dockerfile

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:8.0 AS base
WORKDIR /app
EXPOSE 80
EXPOSE 443

FROM mcr.microsoft.com/dotnet/sdk:8.0 AS build
WORKDIR /src
COPY ["src/FluxoCaixa.API/FluxoCaixa.API.csproj", "src/FluxoCaixa.API/"]
COPY ["src/FluxoCaixa.Application/FluxoCaixa.Application.csproj", "src/FluxoCaixa.Application/"]
COPY ["src/FluxoCaixa.Domain/FluxoCaixa.Domain.csproj", "src/FluxoCaixa.Domain/"]
COPY ["src/FluxoCaixa.Infrastructure/FluxoCaixa.Infrastructure.csproj", "src/FluxoCaixa.Infrastructure/"]

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

### docker-compose.yml

```yaml
version: '3.8'
services:
  fluxocaixa-api:
    build: .
    ports:
      - "5000:80"
    environment:
      - ASPNETCORE_ENVIRONMENT=Development
      - ConnectionStrings__DefaultConnection=Server=sqlserver;Database=FluxoCaixaDb;User Id=sa;Password=YourPassword123;TrustServerCertificate=true
    depends_on:
      - sqlserver

  sqlserver:
    image: mcr.microsoft.com/mssql/server:2022-latest
    environment:
      - ACCEPT_EULA=Y
      - SA_PASSWORD=YourPassword123
    ports:
      - "1433:1433"
    volumes:
      - sqlserver_data:/var/opt/mssql

volumes:
  sqlserver_data:
```

## 📊 Monitoramento e Logs

### Configuração de Logs

```json
{
  "Logging": {
    "LogLevel": {
      "Default": "Information",
      "Microsoft.AspNetCore": "Warning",
      "Microsoft.EntityFrameworkCore": "Information"
    }
  }
}
```

### Métricas Importantes

- Tempo de resposta das APIs
- Taxa de erro por endpoint
- Volume de lançamentos por dia
- Performance das consultas no banco

### Padrões de Código

- Seguir convenções do C#
- Documentar métodos públicos
- Manter cobertura de testes > 80%
- Usar nomes descritivos para variáveis e métodos

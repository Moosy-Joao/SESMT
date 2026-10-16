# SESMT — PT Digital

O repositório contém a base .NET 10 em Clean Architecture. A especificação técnica da Entrega 2 está em [docs/ENTREGA-2-ESPECIFICACAO-TECNICA.md](docs/ENTREGA-2-ESPECIFICACAO-TECNICA.md) e cobre arquitetura, modelo de dados/DER, contratos da API, autenticação e segurança, frontend e plano inicial de testes.

## Estado da fundação

O inventário da branch main consultada contém cinco projetos, mas ainda não apresenta funcionalidades de negócio implementadas.

| Projeto | Responsabilidade prevista | Estado verificado |
| --- | --- | --- |
| SESMT.Domain | Entidades, objetos de valor, regras e eventos | Pastas vazias com arquivos .gitkeep |
| SESMT.Application | Casos de uso, contratos, validações e interfaces | Pastas vazias com arquivos .gitkeep |
| SESMT.Infrastructure | Persistência e integrações | Sem DbContext, repositórios ou migrations |
| SESMT.API | Endpoints HTTP, segurança, OpenAPI e composição | Inicialização básica, controlador de exemplo e autorização sem autenticação configurada |
| SESMT.Tests | Testes unitários, arquiteturais e de integração | Um teste vazio de exemplo |

Os projetos apontam para net10.0. EF Core, Npgsql, ASP.NET Core Identity e bibliotecas JWT não estão referenciados no inventário consultado. O cartão F0 no Trello cita entidades e middleware que não aparecem na branch main; a equipe deve confirmar a branch/base correta antes de reaproveitar esses componentes.

## Arquitetura alvo

```text
SESMT.API ──> SESMT.Application ──> SESMT.Domain
     │                  ▲
     └──> SESMT.Infrastructure ────┘
```

- **Domain:** regras independentes de infraestrutura para organização, trabalhadores, documentos e Permissões de Trabalho (PT).
- **Application:** casos de uso, DTOs, validação, autorização de operações e interfaces de persistência/serviços.
- **Infrastructure:** EF Core com PostgreSQL/Npgsql, migrations, repositórios e armazenamento privado de anexos.
- **API:** controllers, autenticação/autorização HTTP, composição de dependências e contratos.
- **Frontend:** aplicação web responsiva que consome a API por HTTPS/JSON; autorização final permanece no servidor.
- **Tests:** regras de domínio, contratos, isolamento de acesso e fluxos críticos com dados fictícios.

A arquitetura e os contratos completos estão na [especificação técnica da Entrega 2](docs/ENTREGA-2-ESPECIFICACAO-TECNICA.md). As decisões operacionais DC-01 a DC-14 estão identificadas como pendências de validação do produto/SESMT, não como funcionalidades aprovadas.

## Build e testes

Com o SDK .NET 10 instalado:

```sh
dotnet restore SESMT.slnx
dotnet build SESMT.slnx --no-restore
dotnet test SESMT.slnx --no-build
```

A aplicação atual não exige configuração de banco para iniciar porque ainda não possui persistência configurada. Executar build/testes no checkout da equipe e registrar os resultados no CI.

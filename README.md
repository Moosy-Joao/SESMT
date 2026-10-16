# SESMT — PT Digital

## Estado da fundação

Este repositório está no início da implementação. A solução contém cinco projetos .NET 10 organizados como uma base de Clean Architecture, mas o inventário atual ainda não contém funcionalidades de negócio implementadas.

| Projeto | Responsabilidade prevista | Estado verificado |
| --- | --- | --- |
| `SESMT.Domain` | Entidades, objetos de valor, regras e eventos de domínio | Pastas vazias com arquivos `.gitkeep` |
| `SESMT.Application` | Casos de uso, contratos, validações e interfaces | Pastas vazias com arquivos `.gitkeep` |
| `SESMT.Infrastructure` | Persistência, integrações e implementações de interfaces | Pastas vazias; sem DbContext, repositórios ou migrations |
| `SESMT.API` | Endpoints HTTP, composição da aplicação e documentação OpenAPI | Inicialização básica, controlador de exemplo e autorização sem autenticação configurada |
| `SESMT.Tests` | Testes unitários, arquiteturais e de integração | Um teste vazio de exemplo; nenhuma cobertura de comportamento |

A solução e os projetos apontam para `net10.0`. No estado atual, os projetos não referenciam EF Core, Npgsql, ASP.NET Core Identity nem bibliotecas JWT. Não há entidades de negócio ou Value Objects no inventário da branch `main`; portanto, não há modelos existentes para classificar como reutilizar, alterar ou retirar. A descrição atual do cartão F0 no Trello menciona entidades e middleware que não aparecem nessa branch. Essa diferença precisa ser confirmada com a equipe antes de qualquer reaproveitamento.

## Arquitetura alvo

Manter as dependências apontando para dentro da solução:

```text
SESMT.API ──> SESMT.Application ──> SESMT.Domain
     │                  ▲
     └──> SESMT.Infrastructure ────┘
```

- **Domain:** regras independentes de infraestrutura para organização, trabalhadores, documentos e Permissões de Trabalho (PT).
- **Application:** casos de uso, DTOs, validação, autorização de operações e interfaces de persistência/serviços.
- **Infrastructure:** implementações de persistência com EF Core e PostgreSQL/Npgsql, armazenamento de anexos e demais integrações.
- **API:** controllers, autenticação e autorização HTTP, composição de dependências e documentação dos contratos.
- **Tests:** testes de domínio e aplicação; testes de integração para API, persistência, segurança e fluxos críticos.

Essa é a organização pretendida, não uma declaração de componentes já entregues. A decisão de usar PostgreSQL com EF Core e a estratégia de autenticação precisam ser materializadas em dependências, configuração, migrations e testes antes de serem consideradas concluídas.

## Próximos passos da Entrega 2

1. Confirmar com a equipe a divergência entre a descrição do cartão F0 e os arquivos presentes na branch.
2. Registrar as decisões bloqueadoras do MVP: serviços, designações, requisitos documentais, validade, ciência dos participantes, versão da PT, liberação e revisão final.
3. Com essas decisões, modelar entidades, relações, chaves, integridade e estados; documentar o DER.
4. Definir contratos da API com método, rota, autorização, entradas, saídas, validações e erros.
5. Implementar persistência PostgreSQL/EF Core e autenticação/autorização conforme as decisões registradas.
6. Substituir o teste vazio por testes de regras e criar testes de integração para os bloqueios de segurança operacional.

Não tratar dados, templates ou documentos ilustrativos como evidência de operação real. A liberação de uma PT deve depender de validação no servidor, com autoria e histórico.

## Build e testes

Com o SDK .NET 10 instalado:

```sh
dotnet restore SESMT.slnx
dotnet build SESMT.slnx --no-restore
dotnet test SESMT.slnx --no-build
```

A aplicação atual não exige configuração de banco para iniciar porque ainda não possui persistência configurada. Os comandos de build e teste acima descrevem a validação local esperada; a execução deve ser registrada no ambiente que tenha o SDK e o repositório clonado.

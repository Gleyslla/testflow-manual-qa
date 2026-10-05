# TestFlow — Manual QA Case Study

![QA](https://img.shields.io/badge/QA-Manual%20Testing-2563EB)
![BDD](https://img.shields.io/badge/BDD-Gherkin-7C3AED)
![Cases](https://img.shields.io/badge/Test%20Cases-90-0EA5E9)
![Scope](https://img.shields.io/badge/Scope-MVP-success)

> **Um produto, diferentes abordagens de qualidade.**  
> Este repositório apresenta o processo de **QA Manual** aplicado ao TestFlow: análise de requisitos, regras de negócio, planejamento, BDD/Gherkin, casos de teste, rastreabilidade, execução e gestão de defeitos.

## Objetivo

O **TestFlow** é um produto fictício de gerenciamento de inscrições utilizado como case de portfólio. Este repositório tem foco exclusivamente no processo manual de QA.

`Requisito → Regra de negócio → Risco → BDD/Gherkin → Caso de teste → Execução → Evidência → Defeito/Resultado`

## Escopo funcional

Cadastro, login, recuperação de senha, sessão, proteção de rotas, Dashboard, criação/listagem/pesquisa/filtro/exclusão de inscrições, perfil e logout.

## Estratégia

| Abordagem | Objetivo |
|---|---|
| Funcional | Validar requisitos e regras |
| Positiva/negativa | Cobrir sucesso e entradas inválidas |
| Validação/limite | Verificar obrigatoriedade, formato e fronteiras |
| Sessão/navegação | Autenticação, persistência, rotas e logout |
| Regressão | Proteger jornadas críticas |
| Exploratória/usabilidade | Investigar riscos além do roteiro e clareza da interface |

Prioridades: **P0 crítica**, **P1 alta**, **P2 média**, **P3 baixa**.

## Requisitos e regras

| ID | Requisito |
|---|---|
| RF-001 | Cadastrar usuário |
| RF-002 | Autenticar usuário |
| RF-003 | Tratar credenciais inválidas |
| RF-004 | Permitir recuperação de senha |
| RF-005 | Manter sessão válida |
| RF-006 | Proteger rotas autenticadas |
| RF-007 | Exibir Dashboard consistente |
| RF-008 | Criar inscrição |
| RF-009 | Validar obrigatórios da inscrição |
| RF-010 | Listar inscrições |
| RF-011 | Pesquisar inscrições |
| RF-012 | Filtrar por status |
| RF-013 | Excluir mediante confirmação |
| RF-014 | Atualizar perfil |
| RF-015 | Encerrar sessão |

| ID | Regra de negócio |
|---|---|
| RN-001 | E-mail identifica unicamente uma conta |
| RN-002 | E-mail já cadastrado não pode gerar nova conta |
| RN-003 | Senha deve respeitar o mínimo definido |
| RN-004 | Apenas credenciais válidas iniciam sessão |
| RN-005 | Rotas protegidas exigem autenticação |
| RN-006 | Sessão válida persiste após reload |
| RN-007 | Obrigatórios devem ser preenchidos antes da inscrição |
| RN-008 | Criação/exclusão deve refletir nos dados apresentados |
| RN-009 | Pesquisa/filtros não alteram dados armazenados |
| RN-010 | Exclusão exige confirmação explícita |
| RN-011 | Cancelar confirmação preserva registro |
| RN-012 | Alterações válidas do perfil persistem |
| RN-013 | Logout invalida sessão |
| RN-014 | Após logout, rotas protegidas exigem nova autenticação |

## BDD / Gherkin

```gherkin
Funcionalidade: Autenticação

Cenário: Autenticar usuário com credenciais válidas
  Dado que possuo uma conta cadastrada
  E estou na página de login
  Quando informo e-mail e senha válidos
  E aciono "Entrar"
  Então devo ser autenticado
  E direcionado ao Dashboard

Cenário: Recusar credenciais inválidas
  Dado que estou na página de login
  Quando informo credenciais inválidas
  Então a autenticação não deve ocorrer
  E uma mensagem adequada deve ser apresentada
```

```gherkin
Funcionalidade: Inscrições

Cenário: Criar inscrição válida
  Dado que estou autenticado
  E acesso Nova Inscrição
  Quando preencho os campos obrigatórios com dados válidos
  E confirmo o cadastro
  Então a inscrição deve ser criada
  E deve aparecer na listagem

Cenário: Impedir inscrição sem obrigatórios
  Dado que estou no formulário de nova inscrição
  Quando tento concluir sem preencher campos obrigatórios
  Então a inscrição não deve ser criada
  E os campos inválidos devem ser sinalizados

Cenário: Excluir inscrição
  Dado que existe uma inscrição cadastrada
  Quando solicito sua exclusão
  E confirmo a operação
  Então a inscrição deve ser removida
```

```gherkin
Funcionalidade: Sessão

Cenário: Manter sessão após F5
  Dado que estou autenticado
  Quando atualizo uma página protegida
  Então devo permanecer autenticado

Cenário: Bloquear rota sem autenticação
  Dado que não possuo sessão válida
  Quando tento acessar uma página protegida
  Então devo ser direcionado ao login

Cenário: Encerrar sessão
  Dado que estou autenticado
  Quando aciono "Sair"
  Então a sessão deve ser invalidada
  E devo retornar ao login
```

# Catálogo — 90 casos de teste manual

Os campos **Resultado obtido, Status e Evidência** devem ser preenchidos somente durante execução real.

## Cadastro — CT-MAN-001 a 012

| ID | Pri. | Cenário | Resultado esperado |
|---|---|---|---|
| CT-MAN-001 | P0 | Cadastro com todos os dados válidos | Conta criada uma única vez e sucesso informado |
| CT-MAN-002 | P0 | Cadastro com e-mail já utilizado | Operação bloqueada e duplicidade informada |
| CT-MAN-003 | P1 | Senha abaixo do mínimo | Cadastro bloqueado e requisito informado |
| CT-MAN-004 | P1 | Nome vazio | Obrigatoriedade sinalizada |
| CT-MAN-005 | P1 | E-mail vazio | Obrigatoriedade sinalizada |
| CT-MAN-006 | P1 | Senha vazia | Obrigatoriedade sinalizada |
| CT-MAN-007 | P1 | E-mail com formato inválido | Formato rejeitado |
| CT-MAN-008 | P2 | Espaços nas extremidades do e-mail | Entrada tratada consistentemente, sem duplicidade indevida |
| CT-MAN-009 | P2 | Nome com acentos e espaços válidos | Valor aceito e preservado |
| CT-MAN-010 | P2 | Formulário totalmente vazio | Nenhuma conta criada; obrigatórios indicados |
| CT-MAN-011 | P2 | Corrigir campo após erro | Estado de erro atualizado/removido |
| CT-MAN-012 | P2 | Navegação após cadastro bem-sucedido | Fluxo prossegue sem estado inconsistente |

## Login/recuperação — CT-MAN-013 a 025

| ID | Pri. | Cenário | Resultado esperado |
|---|---|---|---|
| CT-MAN-013 | P0 | Login válido | Dashboard exibido e sessão criada |
| CT-MAN-014 | P0 | Senha incorreta | Login recusado |
| CT-MAN-015 | P0 | E-mail inexistente | Login recusado com feedback |
| CT-MAN-016 | P1 | Login pela tecla Enter | Mesmo comportamento do botão Entrar |
| CT-MAN-017 | P1 | E-mail vazio | Autenticação bloqueada |
| CT-MAN-018 | P1 | Senha vazia | Autenticação bloqueada |
| CT-MAN-019 | P1 | Ambos vazios | Campos sinalizados; nenhuma sessão |
| CT-MAN-020 | P2 | Formato de e-mail inválido | Entrada tratada sem autenticação |
| CT-MAN-021 | P2 | Corrigir credenciais após erro | Nova tentativa válida autentica |
| CT-MAN-022 | P2 | Abrir Esqueci minha senha | Recuperação acessível sem sessão |
| CT-MAN-023 | P2 | Recuperação com e-mail inválido | Validação apresentada |
| CT-MAN-024 | P2 | Recuperação com e-mail vazio | Obrigatoriedade apresentada |
| CT-MAN-025 | P2 | Voltar ao login pela recuperação | Retorno ocorre sem erro |

## Sessão/rotas/logout — CT-MAN-026 a 037

| ID | Pri. | Cenário | Resultado esperado |
|---|---|---|---|
| CT-MAN-026 | P0 | F5 no Dashboard autenticado | Sessão preservada |
| CT-MAN-027 | P0 | Dashboard sem sessão | Acesso bloqueado/redirecionado |
| CT-MAN-028 | P0 | Nova Inscrição sem sessão | Acesso protegido |
| CT-MAN-029 | P0 | Minhas Inscrições sem sessão | Acesso protegido |
| CT-MAN-030 | P0 | Perfil sem sessão | Acesso protegido |
| CT-MAN-031 | P0 | Logout | Sessão invalidada e login exibido |
| CT-MAN-032 | P0 | URL protegida após logout | Acesso continua bloqueado |
| CT-MAN-033 | P0 | Botão Voltar após logout | Conteúdo protegido não volta utilizável |
| CT-MAN-034 | P1 | Navegar entre páginas autenticadas | Sessão permanece consistente |
| CT-MAN-035 | P1 | Reload em Minhas Inscrições | Autenticação permanece válida |
| CT-MAN-036 | P2 | Abrir rota protegida em nova aba autenticado | Comportamento consistente com a sessão |
| CT-MAN-037 | P1 | Novo login depois de logout | Nova sessão criada normalmente |

## Dashboard — CT-MAN-038 a 045

| ID | Pri. | Cenário | Resultado esperado |
|---|---|---|---|
| CT-MAN-038 | P1 | Abrir Dashboard após login | Componentes essenciais carregam |
| CT-MAN-039 | P1 | Conferir total de inscrições | Total corresponde aos registros |
| CT-MAN-040 | P1 | Criar inscrição e retornar | Totalizador reflete criação |
| CT-MAN-041 | P1 | Excluir inscrição e retornar | Totalizador reflete exclusão |
| CT-MAN-042 | P2 | Dashboard sem inscrições | Estado vazio coerente |
| CT-MAN-043 | P2 | Usar atalhos do Dashboard | Destinos corretos |
| CT-MAN-044 | P2 | Reload após alteração | Dados permanecem consistentes |
| CT-MAN-045 | P3 | Redimensionar viewport | Sem sobreposição crítica; conteúdo utilizável |

## Nova inscrição — CT-MAN-046 a 057

| ID | Pri. | Cenário | Resultado esperado |
|---|---|---|---|
| CT-MAN-046 | P0 | Criar com obrigatórios válidos | Registro criado e listado |
| CT-MAN-047 | P0 | Criar sem obrigatórios | Operação bloqueada |
| CT-MAN-048 | P1 | Omitir um obrigatório por vez | Campo ausente sinalizado |
| CT-MAN-049 | P1 | Corrigir obrigatórios após erro | Cadastro passa a ser permitido |
| CT-MAN-050 | P1 | Reload após criação | Registro persiste sem duplicar |
| CT-MAN-051 | P1 | Duas inscrições em sequência | Dois registros corretos, sem duplicação extra |
| CT-MAN-052 | P2 | Acentos em campos textuais | Conteúdo preservado |
| CT-MAN-053 | P2 | Espaços nas extremidades | Tratamento consistente |
| CT-MAN-054 | P2 | Valores próximos aos limites | Interface e validação permanecem estáveis |
| CT-MAN-055 | P2 | Abandonar formulário sem salvar | Nenhum registro acidental |
| CT-MAN-056 | P2 | Uma submissão | Somente um registro criado |
| CT-MAN-057 | P3 | Feedback pós-criação | Mensagem compreensível e coerente |

## Listagem/pesquisa/filtros — CT-MAN-058 a 068

| ID | Pri. | Cenário | Resultado esperado |
|---|---|---|---|
| CT-MAN-058 | P0 | Abrir listagem com registros | Dados persistidos exibidos |
| CT-MAN-059 | P1 | Pesquisar título exato | Registro correspondente exibido |
| CT-MAN-060 | P1 | Pesquisar parte do título | Resultados compatíveis exibidos |
| CT-MAN-061 | P1 | Pesquisar termo inexistente | Estado sem resultados |
| CT-MAN-062 | P2 | Limpar pesquisa | Listagem restaurada |
| CT-MAN-063 | P1 | Filtrar por status existente | Somente status escolhido |
| CT-MAN-064 | P1 | Status sem registros | Estado vazio coerente |
| CT-MAN-065 | P2 | Remover filtro | Lista completa restaurada |
| CT-MAN-066 | P1 | Combinar texto + status | Ambos os critérios respeitados |
| CT-MAN-067 | P2 | Alternar filtros repetidamente | Dados não são alterados/duplicados |
| CT-MAN-068 | P2 | Reload após filtros | Persistência permanece íntegra |

## Exclusão — CT-MAN-069 a 075

| ID | Pri. | Cenário | Resultado esperado |
|---|---|---|---|
| CT-MAN-069 | P0 | Solicitar exclusão | Confirmação exibida antes da remoção |
| CT-MAN-070 | P0 | Confirmar exclusão | Registro removido |
| CT-MAN-071 | P0 | Cancelar exclusão | Registro preservado |
| CT-MAN-072 | P1 | Fechar modal com Escape | Modal fecha; registro preservado |
| CT-MAN-073 | P1 | Reload após excluir | Registro não reaparece |
| CT-MAN-074 | P1 | Conferir Dashboard após excluir | Totalizadores consistentes |
| CT-MAN-075 | P2 | Cancelar e solicitar novamente | Nova confirmação funciona sem resíduo |

## Perfil — CT-MAN-076 a 083

| ID | Pri. | Cenário | Resultado esperado |
|---|---|---|---|
| CT-MAN-076 | P1 | Abrir Perfil autenticado | Dados atuais apresentados |
| CT-MAN-077 | P1 | Atualizar dados válidos | Alterações salvas |
| CT-MAN-078 | P1 | Reload após salvar | Alterações persistem |
| CT-MAN-079 | P1 | Obrigatório vazio | Salvamento inválido bloqueado |
| CT-MAN-080 | P2 | Formato inválido | Campo rejeitado/sinalizado |
| CT-MAN-081 | P2 | Corrigir valor inválido | Alteração válida aceita |
| CT-MAN-082 | P2 | Navegar e retornar ao Perfil | Dados continuam consistentes |
| CT-MAN-083 | P3 | Conferir labels/campos | Formulário compreensível e utilizável |

## Exploratórios/usabilidade/regressão — CT-MAN-084 a 090

| ID | Pri. | Cenário | Resultado esperado |
|---|---|---|---|
| CT-MAN-084 | P1 | Cadastro → Login → Inscrição → Lista → Logout | Jornada crítica sem bloqueios |
| CT-MAN-085 | P1 | Login → criar → excluir após alteração | Fluxos críticos preservados |
| CT-MAN-086 | P2 | Navegação rápida pelo menu | Estado consistente |
| CT-MAN-087 | P2 | Navegação por Tab em formulários | Ordem de foco compreensível |
| CT-MAN-088 | P2 | Feedback de sucesso/erro | Mensagens coerentes com a ação |
| CT-MAN-089 | P3 | Legibilidade em viewport reduzido | Conteúdo essencial utilizável |
| CT-MAN-090 | P2 | Smoke: login, Dashboard, criar, listar, excluir, logout | Nenhum fluxo crítico bloqueado |

# Modelo de execução detalhada

| Campo | Exemplo |
|---|---|
| ID | CT-MAN-046 |
| Requisito | RF-008 |
| Regra | RN-007 / RN-008 |
| Módulo | Nova inscrição |
| Tipo | Funcional positivo |
| Prioridade | P0 |
| Pré-condição | Usuário autenticado |
| Passos | Acessar Nova Inscrição → preencher obrigatórios → confirmar |
| Resultado esperado | Registro criado e listado |
| Resultado obtido | Preencher durante execução |
| Status | PASSOU / FALHOU / BLOQUEADO / NÃO EXECUTADO |
| Evidência | Referência da evidência da rodada |

# Matriz de rastreabilidade

| Requisito | Regras | Cobertura manual |
|---|---|---|
| RF-001 | RN-001–003 | CT-MAN-001–012 |
| RF-002/003 | RN-004 | CT-MAN-013–021 |
| RF-004 | — | CT-MAN-022–025 |
| RF-005/006 | RN-005/006 | CT-MAN-026–037 |
| RF-007 | RN-008 | CT-MAN-038–045 |
| RF-008/009 | RN-007/008 | CT-MAN-046–057 |
| RF-010/011/012 | RN-009 | CT-MAN-058–068 |
| RF-013 | RN-010/011 | CT-MAN-069–075 |
| RF-014 | RN-012 | CT-MAN-076–083 |
| RF-015 | RN-013/014 | CT-MAN-031–033, 037 |
| Regressão transversal | Aplicáveis | CT-MAN-084–090 |

# Gestão de defeitos

```text
ID: BUG-XXX
Título:
Módulo:
Ambiente:
Build/versão:
Severidade:
Prioridade:
Pré-condição:

Passos para reproduzir:
1.
2.
3.

Resultado obtido:
Resultado esperado:
Evidência:
Status:
```

Severidade: **Crítica** (bloqueio/perda relevante), **Alta** (função principal comprometida), **Média** (impacto limitado/contorno), **Baixa** (baixo impacto/cosmético).

> Bugs não são inventados para aumentar o portfólio; devem ser registrados quando observados em execução real.

# Evidências e relatório

Uma evidência útil identifica o caso, estado validado, ambiente/build e resultado observado. Pode ser screenshot ou vídeo.

Modelo de relatório:

```text
Produto: TestFlow
Versão:
Ambiente:
Navegador:
Data:
Responsável:

Planejados:
Executados:
PASSOU:
FALHOU:
BLOQUEADO:
NÃO EXECUTADO:

Defeitos por severidade:
Riscos residuais:
Conclusão da rodada:
```

A aprovação deve considerar criticidade, cobertura, defeitos abertos e risco residual — não apenas a porcentagem de testes aprovados.

# Critérios de entrada e saída

**Entrada:** escopo definido, build disponível, requisitos compreendidos, ambiente acessível e dados identificados.

**Saída:** casos executados ou justificados, defeitos registrados, bloqueios documentados, evidências organizadas e riscos comunicados.

# Smoke checklist

- [ ] aplicação abre;
- [ ] cadastro/login acessíveis;
- [ ] autenticação funciona;
- [ ] Dashboard carrega;
- [ ] inscrição pode ser criada;
- [ ] registro aparece na listagem;
- [ ] exclusão funciona;
- [ ] logout invalida a sessão.

# Resultado do case

Este material demonstra **test design, análise de requisitos, cobertura baseada em risco, BDD/Gherkin, testes positivos/negativos, regressão, exploratórios, rastreabilidade, bug reporting e comunicação de risco**.

**90 casos de teste manual projetados.**

Os 90 representam **cobertura planejada**. Nenhum resultado é declarado como executado sem execução real.

## Evolução do portfólio TestFlow

`QA Manual → Cypress → Playwright + CI`

A versão Playwright já demonstra automação E2E, rastreabilidade e integração contínua com GitHub Actions:

https://github.com/Gleyslla/testflow-qa

## Autora

**Gleyslla Morais**  
QA Analyst • Quality Assurance • Software Testing

# IAE — Incremental AI Engineering

IAE é uma metodologia de orquestração de IA para desenvolvimento de software:

> Specify → Design → Decompose → Implement → Test → Adapt

O objetivo é reduzir ambiguidades, context rot e implementações grandes demais. Antes de escrever código, o Codex transforma o problema em uma especificação, cria um design incremental e decompõe o trabalho em subfases executáveis.

Este repositório distribui a metodologia como um plugin de skills para o Codex.

## Como funciona

O fluxo principal é:

1. **Specify:** esclarece requisitos, regras de negócio, fluxos, edge cases, critérios de aceite e cenários de teste.
2. **Design:** analisa a especificação e a arquitetura atual para criar fases funcionais e testáveis.
3. **Decompose:** transforma cada fase em pequenas subfases com arquivos, passos e critérios de sucesso explícitos.
4. **Loop:** implementa somente a próxima subfase pendente e executa as validações automatizadas.
5. **Test:** ao terminar uma fase, o Codex apresenta um roteiro e aguarda a validação manual do desenvolvedor.
6. **Adapt:** diagnostica falhas como erro de implementação ou planejamento e evolui o plano sem apagar seu histórico.

## Instalação

Adicione este repositório como marketplace:

```bash
codex plugin marketplace add GuilhermeMendesRosa/iae --ref main
```

Instale o plugin:

```bash
codex plugin add iae@iae
```

Depois da instalação, abra uma nova conversa para que as skills sejam carregadas.

Também é possível localizar as skills pelo seletor `/skills`.

## Uso

As skills possuem invocação explícita. Use `$` para mencionar cada skill:

### 1. Especificar

```text
$iae:specify Quero implementar autenticação por email e senha...
```

O Codex conduz os esclarecimentos necessários e gera:

```text
iae/specification.md
```

Nenhuma decisão de negócio relevante deve ser assumida pela IA.

### 2. Projetar

```text
$iae:design
```

O Codex analisa a especificação, o código e a arquitetura existente para gerar:

```text
iae/design.md
```

O design contém fases incrementais, dependências, validações automatizadas, testes manuais e o mapeamento dos cenários de negócio.

### 3. Decompor

```text
$iae:decompose
```

Cada fase é quebrada em subfases executáveis:

```text
iae/
├── specification.md
├── design.md
└── phases/
    ├── phase_1_A.md
    ├── phase_1_B.md
    ├── phase_1_C.md
    └── phase_2_A.md
```

Cada subfase registra objetivo, contexto, escopo, arquivos envolvidos, passos, critérios de sucesso, validação automatizada, cenários cobertos, riscos e evidências da execução.

### 4. Executar o loop

```text
$iae:loop
```

Cada execução implementa no máximo uma subfase. O Codex:

- localiza a próxima subfase pendente;
- implementa apenas o escopo planejado;
- executa testes e verificações automatizadas;
- registra arquivos alterados e evidências;
- informa qual será a próxima subfase.

Execute `$iae:loop` novamente para avançar.

## Validação manual

Quando a última subfase de uma fase termina, o loop para automaticamente. O Codex apresenta:

- pré-requisitos;
- ações numeradas;
- resultado esperado de cada ação;
- critérios de aceite e cenários de negócio validados.

Depois de testar, continue com:

```text
$iae:loop Testei a fase e passou.
```

O Codex registra a validação e inicia a próxima fase.

Se o teste falhar:

```text
$iae:adapt Ao executar <ação>, aconteceu <resultado> em vez de <esperado>.
```

## Adaptação

A skill `$iae:adapt` classifica o problema antes de agir:

- **Erro de implementação:** o plano estava correto; o código é corrigido e revalidado.
- **Erro de planejamento:** os artefatos recebem uma adaptação versionada e novas subfases são criadas.

Decisões e fases antigas não são apagadas. Instruções substituídas permanecem registradas para preservar a evolução do plano.

## Skills incluídas

| Skill | Responsabilidade |
| --- | --- |
| `$iae:specify` | Definir o comportamento esperado sem tomar decisões de negócio pelo usuário |
| `$iae:design` | Criar uma solução técnica incremental baseada no repositório real |
| `$iae:decompose` | Produzir subfases pequenas, ordenadas e executáveis |
| `$iae:loop` | Implementar uma subfase por vez e controlar os gates manuais |
| `$iae:adapt` | Diagnosticar falhas e evoluir código ou planejamento |

## Regras importantes

- Código não é implementado durante Specify, Design ou Decompose.
- Cada fase entrega algo funcional e testável.
- Cada subfase possui critérios objetivos de sucesso.
- Cenários de negócio nascem na especificação e recebem IDs estáveis.
- Fases que alteram lógica de backend terminam com uma subfase exclusiva de testes automatizados.
- Testes automatizados são responsabilidade da IA.
- O avanço entre fases depende de validação manual.
- A IA não toma decisões de negócio sem validação do desenvolvedor.
- Artefatos IAE são temporários e ficam no diretório `iae/` do projeto.

## Atualização

Para atualizar o snapshot do marketplace:

```bash
codex plugin marketplace upgrade iae
codex plugin add iae@iae
```

Abra uma nova conversa depois da atualização.

## Estrutura do repositório

```text
.
├── .agents/plugins/marketplace.json
└── plugins/iae/
    ├── .codex-plugin/plugin.json
    └── skills/
        ├── specify/
        ├── design/
        ├── decompose/
        ├── loop/
        └── adapt/
```

## Referências

- [Documentação oficial sobre skills do Codex](https://learn.chatgpt.com/docs/build-skills)
- [Documentação oficial sobre plugins do Codex](https://learn.chatgpt.com/docs/build-plugins)

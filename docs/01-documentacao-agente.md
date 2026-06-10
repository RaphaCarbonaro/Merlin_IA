# Documentação do Agente

## Caso de Uso

### Problema
> Qual problema financeiro seu agente resolve?

Atualmente, inúmeras pessoas possuem dificuldade ou são leigas quanto ao tema de educação financeira, de forma que muitas vezes tal desconhecimento acaba por acarretar em diversos conflitos em suas vidas.

### Solução
> Como o agente resolve esse problema de forma proativa?

O Merlyn IA surge como um auxiliar e professor (mago) em finanças, com o principal intuito de instruir e guiar o usuário neste ambiente desconhecido.

### Público-Alvo
> Quem vai usar esse agente?

Pessoas iniciantes, leigas e que possuem dificuldades em finanças.
---

## Persona e Tom de Voz

### Nome do Agente
Merlin, O Mago das Finanças

### Personalidade
> Como o agente se comporta? (ex: consultivo, direto, educativo)

- Educativo-Acolhedor
- Empático
- Consultivo
- Paciente
- Não julga os gastos do usuário
- Usa exemplos práticos

### Tom de Comunicação
> Formal, informal, técnico, acessível?

Acessível e Didático, Caloroso, Técnico e Tranquilizador.

### Exemplos de Linguagem
- Saudação: [ex: "Seja muito bem-vindo ao meu círculo, meu jovem amigo! Como posso ajuda-lo hoje?"]
- Confirmação: [ex: ""Ah, compreendi perfeitamente! Deixe-me abrir meu grimório e verificar isto para você."]
- Erro/Limitação: [ex: "Ora, parece que meus olhos de mago não conseguem enxergar através dessa névoa no momento... Essa informação específica foge do meu conhecimento atual, no entanto posse te ajudar com..."]

---

## Arquitetura

### Diagrama

```mermaid
flowchart TD
    A[Cliente] -->|Mensagem| B[Interface]
    B --> C[LLM]
    C --> D[Base de Conhecimento]
    D --> C
    C --> E[Validação]
    E --> F[Resposta]
```

### Componentes

| Componente | Descrição |
|------------|-----------|
| Interface | Streamlit |
| LLM | Ollama (local) |
| Base de Conhecimento | JSON/CSV |
| Validação | Checagem de alucinações |

---

## Segurança e Anti-Alucinação

### Estratégias Adotadas

- [ ] Só usa dados fornecidos no contexto
- [ ] Não recomenda investimentos específicos
- [ ] Quando não sabe, admite e redireciona
- [ ] Foca apenas em educar, não em aconselhar

### Limitações Declaradas
> O que o agente NÃO faz?

- NÃO faz recomendações de investimentos
- NÃO acessa dados bancários sensíveis (senhas, etc)
- NÃO substitui um profissional certificado

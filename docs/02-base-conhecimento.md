# Base de Conhecimento

## Dados Utilizados

| Arquivo | Formato | Utilização no Agente |
|---------|---------|---------------------|
| `historico_atendimento.csv` | CSV | Contextualizar interações anteriores |
| `perfil_investidor.json` | JSON | Personalizar recomendações |
| `produtos_financeiros.json` | JSON | Sugerir produtos adequados ao perfil |
| `transacoes.csv` | CSV | Analisar padrão de gastos do cliente |

---

## Adaptações nos Dados

> Você modificou ou expandiu os dados mockados? Descreva aqui.

- Expansão de Colunas (Estrutura): Adicionado novas dimensões para enriquecer o contexto analítico da IA:

- ID_Transacao: Para identificação única de cada registro.

- Sentimento_Usuario: Atributo psicológico/comportamental para apoiar uma abordagem empática da persona.

- Hobby_Relacionado e Mecanica_Financeira: Atributos focados no nicho dos gastos, permitindo à IA identificar gastos específicos desse setor.

- Expansão de Linhas (Volume): Adicionamos novas transações fictícias no final do mês focadas no hobby de jogos para testar a capacidade de segmentação e engajamento do consultor financeiro.

- Padronização: Aplicamos o valor "N/A" para as linhas e colunas que não tinham relação com o hobby, garantindo consistência e evitando campos nulos na leitura dos dados.
  
- Adicionado nova meta no arquivo `perfil_investidor.json`.

---

## Estratégia de Integração

### Como os dados são carregados?
> Descreva como seu agente acessa a base de conhecimento.

```python
import pandas as pd
import json

perfil = json.load(open('./data/perfil_investidor.json'))
transacoes = pd.read_csv('./data/transacoes.csv')
historico = pd.read_csv('./data/historico_atendimento.csv')
produtos = json.load(open('./data/produtos_financeiros.json'))
```

### Como os dados são usados no prompt?
> Os dados vão no system prompt? São consultados dinamicamente?

Para fins de simplificação, os dados foram inseridos diretamente (hardcoded) no corpo do prompt, assegurando que o Agente disponha do contexto ideal para a execução.

```text
DADOS DO CLIENTE E PERFIL (data/perfil_investidor.json):
{
  "nome": "João Silva",
  "idade": 32,
  "profissao": "Analista de Sistemas",
  "renda_mensal": 5000.00,
  "perfil_investidor": "moderado",
  "objetivo_principal": "Construir reserva de emergência",
  "patrimonio_total": 15000.00,
  "reserva_emergencia_atual": 10000.00,
  "aceita_risco": false,
  "metas": [
    {
      "meta": "Completar reserva de emergência",
      "valor_necessario": 15000.00,
      "prazo": "2026-06"
    },
    {
      "meta": "Upgrade no computador",
      "valor_necessario": 3000.00,
      "prazo": "2027-05"
    },
    {
      "meta": "Entrada do apartamento",
      "valor_necessario": 50000.00,
      "prazo": "2027-12"
    }
  ]
}

TRANSACOES DO CLIENTE (data/transacoes.csv):
ID_Transacao,Data,Descricao,Categoria,Valor,Tipo,Sentimento_Usuario,Hobby_Relacionado,Mecanica_Financeira
001,2025-10-01,Salário,receita,5000.00,entrada,N/A,N/A,N/A
002,2025-10-02,Aluguel,moradia,1200.00,saida,N/A,N/A,Mensalidade (Conta)
003,2025-10-03,Supermercado,alimentacao,450.00,saida,Necessario,N/A,Necessidade
004,2025-10-05,Netflix,lazer,55.90,saida,Satisfeito,N/A,Mensalidade (Assinatura)
005,2025-10-07,Farmácia,saude,89.00,saida,N/A,N/A,N/A
006,2025-10-10,Restaurante,alimentacao,120.00,saida,N/A,N/A,N/A
007,2025-10-12,Uber,transporte,45.00,saida,Neutro,N/A,N/A
008,2025-10-15,Conta de Luz,moradia,180.00,saida,N/A,N/A,Mensalidade (Conta)
009,2025-10-20,Academia,saude,99.00,saida,Satisfeito,N/A,Mensalidade (Assinatura)
010,2025-10-25,Combustível,transporte,250.00,saida,N/A,N/A,N/A
011,2025-10-26,Steam Purchase - Elden Ring,lazer,250.00,saida,Satisfeito,Video-Games,Jogo Base (Compra Unica)
012,2025-10-27,LoL Riot Points,lazer,80.00,saida,Arrependido,Video-Games,Cosmetico/Skin
013,2025-10-28,Upgrade Memoria RAM 16GB,investimento pessoal,320.00,saida,Satisfeito,Video-Games,Hardware/Setup
014,2025-10-29,Assinatura Xbox Game Pass,lazer,50.00,saida,Neutro,Video-Games,Mensalidade (Assinatura)

HISTORICO DE ATENDIMENTO DO CLIENTE (data/historico_atendimento.csv):
data,canal,tema,resumo,resolvido
2025-09-15,chat,CDB,Cliente perguntou sobre rentabilidade e prazos,sim
2025-09-22,telefone,Problema no app,Erro ao visualizar extrato foi corrigido,sim
2025-10-01,chat,Tesouro Selic,Cliente pediu explicação sobre o funcionamento do Tesouro Direto,sim
2025-10-12,chat,Metas financeiras,Cliente acompanhou o progresso da reserva de emergência,sim
2025-10-25,email,Atualização cadastral,Cliente atualizou e-mail e telefone,sim

PRODUTOS DISPONIVEIS PARA ENSINO (data/produtos_financeiros.json):
[
  {
    "nome": "Tesouro Selic",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "100% da Selic",
    "aporte_minimo": 30.00,
    "indicado_para": "Reserva de emergência e iniciantes"
  },
  {
    "nome": "CDB Liquidez Diária",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "102% do CDI",
    "aporte_minimo": 100.00,
    "indicado_para": "Quem busca segurança com rendimento diário"
  },
  {
    "nome": "LCI/LCA",
    "categoria": "renda_fixa",
    "risco": "baixo",
    "rentabilidade": "95% do CDI",
    "aporte_minimo": 1000.00,
    "indicado_para": "Quem pode esperar 90 dias (isento de IR)"
  },
  {
    "nome": "Fundo Imobiliário (FII)",
    "categoria": "fundo",
    "risco": "medio",
    "rentabilidade": "Dividend Yield (DY) costuma ficar entre 6% a 12% ao ano",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil moderado que busca diversificação e renda recorrente mensal"
  },
  {
    "nome": "Fundo de Ações",
    "categoria": "fundo",
    "risco": "alto",
    "rentabilidade": "Variável",
    "aporte_minimo": 100.00,
    "indicado_para": "Perfil arrojado com foco no longo prazo"
  }
]
```

---

## Exemplo de Contexto Montado

> Mostre um exemplo de como os dados são formatados para o agente.

Contexto criado e montado, através dos dados fornecidos pela base de conhecimento.

```
INFORMAÇÕES CONSOLIDADAS DO CLIENTE

[PERFIL DO USUÁRIO]
- Nome: João Silva (32 anos)
- Profissão: Analista de Sistemas
- Renda Mensal: R$ 5.000,00
- Património Total: R$ 15.000,00
- Reserva atual: R$ 10.000
- Perfil de Risco: Moderado (Não aceita riscos de perda de capital no momento)

[METAS]
- 1. Completar Reserva de Emergência: R$ 15.000,00 | Prazo: 2026-06 (Status atual: R$ 10.000,00)
- 2. Upgrade no Computador: R$ 3.000,00 | Prazo: 2027-05
- 3. Entrada do Apartamento: R$ 50.000,00 | Prazo: 2027-12

[HISTÓRICO RECENTE DE TRANSAÇÕES (Outubro/2025)]
- 01/10: Salário (Receita) -> +R$ 5.000,00 |
- 02/10: Aluguel (Moradia) -> -R$ 1.200,00 | Tipo: Mensalidade (Conta)
- 03/10: Supermercado (Alimentação) -> -R$ 450,00 | Sentimento: Necessário
- 05/10: Netflix (Lazer) -> -R$ 55,90 | Tipo: Mensalidade (Assinatura)
- 26/10: Steam - Elden Ring (Lazer) -> -R$ 250,00 | Hobby: Video-Games (Jogo Base) | Sentimento: Satisfeito
- 27/10: LoL Riot Points (Lazer) -> -R$ 80,00 | Hobby: Video-Games (Cosmético) | Sentimento: Arrependido
- 28/10: Upgrade Memória RAM (Investimento Pessoal) -> -R$ 320,00 | Hobby: Video-Games (Hardware) | Sentimento: Satisfeito
- 29/10: Xbox Game Pass (Lazer) -> -R$ 50,00 | Hobby: Video-Games (Assinatura)

[ÚLTIMOS ATENDIMENTOS E INTERAÇÕES]
- 01/10: Cliente solicitou explicações didáticas via chat sobre o funcionamento do Tesouro Selic.
- 12/10: Acompanhamento de progresso das metas e evolução da reserva de emergência.

[PRODUTOS RECOMENDADOS PARA ESTE PERFIL]
1. Tesouro Selic (Renda Fixa | Risco Baixo | 100% da Selic) - Ideal para a Reserva de Emergência.
2. CDB Liquidez Diária (Renda Fixa | Risco Baixo | 102% do CDI) - Ótimo custo-benefício para segurança.
======================================================================
```

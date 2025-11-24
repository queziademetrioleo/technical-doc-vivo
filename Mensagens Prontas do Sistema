# Documentação Técnica: Mensagens Hardcoded
## Projeto: Vivo Oracle - Agente de Cobrança com IA

**Data:** 2025-11-24
**Versão:** 1.0

---

## 📋 ÍNDICE

1. [Visão Geral](#-visão-geral)
2. [Fluxo Completo de Conversa](#-fluxo-completo-de-conversa)
3. [Estágio 1: Name Validation](#-estágio-1-name-validation)
4. [Estágio 2: PID Validation](#-estágio-2-pid-validation)
5. [Estágio 3: Negotiation](#-estágio-3-negotiation)
6. [Mensagens de Sistema](#-mensagens-de-sistema)
7. [Áudio e Efeitos Sonoros](#-áudio-e-efeitos-sonoros)
8. [Variáveis Dinâmicas](#-variáveis-dinâmicas)
9. [Códigos de Status](#-códigos-de-status)
10. [Resumo Executivo](#-resumo-executivo)

---

## 📊 VISÃO GERAL

### Estatísticas

| Categoria | Quantidade | Arquivo(s) |
|-----------|-----------|-----------|
| **Name Validation** | 10 mensagens | `agents/name_validation/` |
| **PID Validation** | 10 mensagens | `agents/pid_validation/` |
| **Negotiation** | 25+ mensagens | `agents/negotiation/` |
| **Sistema/Integração** | 8 mensagens | `agent.py`, `main.py` |
| **Áudio/Efeitos** | 3 clipes | `agent.py` |
| **Variáveis Dinâmicas** | 15+ variáveis | `states.py`, prompts |
| **Códigos de Status** | 17 códigos | `integration.py`, `states.py` |

**Total:** 60+ mensagens hardcoded identificadas

---

## 🔄 FLUXO COMPLETO DE CONVERSA

### Diagrama de Fluxo

```
┌─────────────────────────────────────────────────────────────┐
│                  INÍCIO DA CHAMADA SIP                       │
└─────────────────────────────────────────────────────────────┘
                          ↓
          [Carregamento de dados do Nectar]
                          ↓
        "Aguarde um momento enquanto carrego suas informações."
                          ↓
┌─────────────────────────────────────────────────────────────┐
│              ESTÁGIO 1: NAME VALIDATION                      │
└─────────────────────────────────────────────────────────────┘
                          ↓
    Agente: "{Saudação}! Estou falando com {Nome}?"
                          ↓
    ┌─────────────────┬─────────────────┬─────────────────┐
    ↓                 ↓                 ↓                 ↓
  [SIM]            [NÃO]         [TITULAR?]      [FALECIDO]
    ↓                 ↓                 ↓                 ↓
"Muito obrigada" "Desculpa o    "Pode passar    "Meus sentimentos"
    ↓            incômodo"      o telefone?"    → Encerra
    ↓            → Encerra          ↓
    ↓                            [AGUARDA]
    ↓
┌─────────────────────────────────────────────────────────────┐
│              ESTÁGIO 2: PID VALIDATION                       │
│                      (Opcional)                              │
└─────────────────────────────────────────────────────────────┘
                          ↓
    "Para confirmar sua identidade, informe os três
     primeiros dígitos do seu CPF?"
                          ↓
    ┌──────────────────┬──────────────────┬────────────────┐
    ↓                  ↓                  ↓                ↓
[CORRETO]         [INCORRETO]        [RECUSA]       [3 ERROS]
    ↓                  ↓                  ↓                ↓
"Um momento..."   "Incorretos"    "É necessário"  "Não posso
    ↓             → Retry (3x)     → Insiste       prosseguir"
    ↓                                              → Encerra
    ↓
┌─────────────────────────────────────────────────────────────┐
│              ESTÁGIO 3: NEGOTIATION                          │
└─────────────────────────────────────────────────────────────┘
                          ↓
    "Você tem um saldo em aberto de R$ X,XX
     referente a [produtos] com vencimento em DD/MM"
                          ↓
    [Apresenta Ofertas: à vista, parcelado]
                          ↓
    ┌─────────────┬──────────────┬──────────────┬────────────┐
    ↓             ↓              ↓              ↓            ↓
[ACEITA]    [DATA RUIM]   [JÁ PAGUEI]    [FRAUDE]   [RECUSA TODAS]
    ↓             ↓              ↓              ↓            ↓
"Fico feliz  "Posso        "No sistema   "Vamos      "Não chegamos
 que         estender      consta        enviar      a um acordo"
 chegamos    até DD/MM"    pendente"     SMS"        → Survey/Encerra
 em acordo"       ↓              ↓              ↓
    ↓        → Reapresenta  → Survey      → Survey
    ↓
[Registra no Nectar]
    ↓
"Você vai receber o boleto por e-mail"
    ↓
┌─────────────────────────────────────────────────────────────┐
│                   ENCERRAMENTO                               │
└─────────────────────────────────────────────────────────────┘
                          ↓
    ┌──────────────────┬──────────────────┬────────────────┐
    ↓                  ↓                  ↓                ↓
[SURVEY]         [ATENDENTE]         [HANGUP]      [CALLBACK]
    ↓                  ↓                  ↓                ↓
"Fica na linha  "Vou te         "Até logo"    "Qual melhor
 e responde     transferir"                    horário?"
 pesquisa"      → Transfer SIP                     ↓
    ↓                                         [Salva horário]
 Transfer SIP                                       ↓
                                               "Ligarei
                                                novamente"
```

---

## 🎭 ESTÁGIO 1: NAME VALIDATION

**Arquivo:** `agents/name_validation/agent.py` + `prompt.md`

**Objetivo:** Confirmar se está falando com o titular da conta

### Mensagens do Sistema (Prompt)

#### 1. Saudação Inicial
```
{greeting}! Estou falando com {customer_name}?
```

**Variáveis:**
- `{greeting}`: "Bom dia" / "Boa tarde" / "Boa noite" (dinâmico)
- `{customer_name}`: Nome completo do cliente

**Exemplo:**
> "Boa tarde! Estou falando com João da Silva?"

---

#### 2. Quando Cliente Pergunta Sobre o Assunto
```
"Você estou chamando para falar sobre informações importantes da sua conta com a Vivo e por segurança, não posso contar mais detalhes antes de confirmar se estou falando com a pessoa certa"
```

**Contexto:** Cliente questiona o motivo da ligação antes de confirmar identidade

---

### Mensagens do Agente (Python)

#### 3. Confirmação de Nome (SIM)
```python
"Muito obrigada por confirmar."
```

**Contexto:** Cliente confirma que é o titular
**Ação:** Transita para PID Validation (se necessário) ou Negotiation

---

#### 4. Falecimento
```python
"Meus sentimentos pela sua perda. Estamos com você neste momento difícil. Quando puder, por favor, fale com a gente no um zero três quinze para informar o falecimento. Assim, podemos cancelar o serviço ou transferir a titularidade. Estamos à disposição para ajudar. A Vivo agradece a compreensão e atenção."
```

**Contexto:** Cliente informa que titular faleceu
**Ação:** Encerra ligação (end_call)

---

#### 5. Rejeição do Nome (NÃO - Pessoa Errada)
```python
"Desculpa o incômodo. A Vivo agradece pelo seu tempo. Até logo!"
```

**Contexto:** Cliente diz que não é o titular e não conhece/não quer passar telefone
**Ação:** Encerra ligação (end_call)

---

#### 6. Pessoa Diferente Responde (Pedir para Usar App)
```python
"Pode pedir pro titular da linha falar com a gente? É rapidinho: basta abrir o app da Vivo. Temos uma questão importante pra resolver. A Vivo agradece pelo seu tempo. Até logo!"
```

**Contexto:** Terceiro atende mas não pode/quer passar telefone
**Ação:** Encerra ligação (end_call)

---

#### 7. Transferência de Telefone (Conhece o Titular)
```python
"Consegue passar o telefone para {customer_first_name}? Temos uma questão importante pra resolver. Tô aguardando na linha, ok?"
```

**Variável:**
- `{customer_first_name}`: Primeiro nome do cliente

**Contexto:** Terceiro atende mas conhece o titular
**Ação:** Aguarda titular

**Exemplo:**
> "Consegue passar o telefone para João? Temos uma questão importante pra resolver. Tô aguardando na linha, ok?"

---

#### 8. Insistência para Passar o Telefone
```python
"Desculpe a insistência, mas você poderia me dizer se {customer_first_name()} está disponível para conversar agora?"
```

**Contexto:** Segunda tentativa de localizar titular
**Ação:** Aguarda resposta

---

#### 9. Terceiro Não Conhece Titular
```python
"Desculpa o incômodo. A Vivo agradece pelo seu tempo. Até logo!"
```

**Contexto:** Terceiro não conhece o titular
**Ação:** Encerra ligação (end_call)

---

#### 10. Transferência para Atendente Humano
```python
"Tudo bem, já vou te transferir para um atendente. Um momento."
```

**Contexto:** Cliente solicita atendente humano
**Ação:** Transfer SIP para fila de atendentes

---

## 🔐 ESTÁGIO 2: PID VALIDATION

**Arquivo:** `agents/pid_validation/agent.py` + `prompt.md`

**Objetivo:** Validar identidade com primeiros 3 dígitos do CPF

### Mensagens do Sistema (Prompt)

#### 11. Solicitação de CPF (Inicial)
```
"Aqui é da Vivo. Para terminar de confirmar sua identidade, você poderia me informar os três primeiros dígitos do seu CPF?"
```

**Contexto:** Primeira solicitação de CPF após name validation

---

#### 12. Processando Validação
```
"Um momento enquanto valido..."
```

**Contexto:** Após cliente informar CPF, enquanto valida

---

### Mensagens do Agente (Python)

#### 13. CPF Incorreto - Primeira Tentativa
```python
"Para continuar com o atendimento, eu preciso confirmar os três primeiros dígitos do seu CPF. Você pode tentar buscar um documento que contenha ele? Eu fico no aguardo"
```

**Contexto:** Primeira tentativa com CPF incorreto (tentativa 1/3)
**Ação:** Aguarda nova tentativa

---

#### 14. CPF Incorreto - Segunda Tentativa
```python
"Você poderia informar os três primeiros dígitos do seu CPF agora?"
```

**Contexto:** Segunda tentativa com CPF incorreto (tentativa 2/3)
**Ação:** Aguarda nova tentativa

---

#### 15. Recusa de Compartilhar CPF - Primeira Vez
```python
"A validação do CPF é necessária para a sua segurança e indispensável para prosseguirmos com a ligação. Você poderia me informar os três primeiros dígitos?"
```

**Contexto:** Cliente se recusa a informar CPF
**Ação:** Insiste uma vez

---

#### 16. Recusa Persistente / Não Consegue Informar
```python
"Poxa, não posso prosseguir sem validar seu CPF, então não consigo te ajudar agora. Quando puder, fala com a gente pelo app da Vivo ou no um zero três quinze, ok? Obrigada!"
```

**Contexto:** Cliente recusa persistentemente ou falha 3 tentativas
**Ação:** Encerra ligação (end_call - invalid_identity)

---

#### 17. CPF Incorreto - Pedir Nova Tentativa
```python
# Variante 1:
"Os dígitos informados estão incorretos. Você poderia tentar novamente?"

# Variante 2:
"Por favor, responda com os três primeiros dígitos do seu CPF. Você poderia tentar novamente?"
```

**Contexto:** Tentativa N com CPF incorreto
**Ação:** Aguarda nova tentativa

---

#### 18. Cliente Pede para Aguardar
```python
"Ok, fico no aguardo"
```

**Contexto:** Cliente diz "aguarde um momento" para buscar documento
**Ação:** Aguarda (timeout de 10s por user_away)

---

#### 19. Transferência para Atendente
```python
"Tudo bem, já vou te transferir para um atendente. Um momento."
```

**Contexto:** Cliente solicita atendente humano
**Ação:** Transfer SIP

---

#### 20. Falecimento (Durante PID)
```python
"Meus sentimentos pela sua perda. Estamos com você neste momento difícil. Quando puder, por favor, fale com a gente no um zero três quinze para informar o falecimento. Assim, podemos cancelar o serviço ou transferir a titularidade. Estamos à disposição para ajudar. A Vivo agradece a compreensão e atenção."
```

**Contexto:** Cliente informa falecimento durante PID validation
**Ação:** Encerra ligação

---

## 💰 ESTÁGIO 3: NEGOTIATION

**Arquivo:** `agents/negotiation/agent.py` + `prompt.md` + `special_offers.md`

**Objetivo:** Negociar pagamento de dívidas

### Mensagens do Sistema (Prompt)

#### 21. Introdução da Dívida
```
"Consta um saldo em aberto de {total_debt} no serviço {products_text} referente a {debt_dates_summary}"
```

**Variáveis:**
- `{total_debt}`: Valor total da dívida (ex: "R$ 150,00")
- `{products_text}`: Produtos/planos (ex: "Linha Móvel e Internet Banda Larga")
- `{debt_dates_summary}`: Datas de vencimento (ex: "15/01/2025 e 15/02/2025")

**Exemplo:**
> "Consta um saldo em aberto de R$ 150,00 no serviço Linha Móvel referente a 15/01/2025"

---

#### 22. Prefixo de Oferta (COM PID)
```
"Obrigado pela confirmação, {customer_first_name()}. Estou aqui para te ajudar a resolver uma pendência da sua conta com uma proposta exclusiva"
```

**Contexto:** Após PID validation bem-sucedida

**Exemplo:**
> "Obrigado pela confirmação, João. Estou aqui para te ajudar a resolver uma pendência da sua conta com uma proposta exclusiva"

---

#### 23. Prefixo de Oferta (SEM PID)
```
"Sou uma assistente inteligente da Vivo e estou aqui para te ajudar com informações importantes sobre sua conta"
```

**Contexto:** Quando PID validation não foi necessária

---

#### 24. Palavras Proibidas
**IMPORTANTE:** O prompt instrui o agente a NUNCA usar:
- ❌ "Débito"
- ❌ "Dívida"
- ❌ "Conta"

**Usar em seu lugar:**
- ✅ "Fatura"
- ✅ "Saldo em aberto"
- ✅ "Pendência"

---

### Mensagens do Agente (Python) - Cenários

#### 25. Reclamação sobre Data de Pagamento
```python
# Quando data é flexível:
"Entendo que você está desconfortável com a data do pagamento, tenho um pouco de flexibilidade para negociar uma data melhor. Posso estender a data do pagamento até {formatted_date}. Que tal pagar {offer_details} nessa nova data?"

# Fallback (data fixa):
"Entendo que você está desconfortável com a data do pagamento, tenho um pouco de flexibilidade para negociar uma data melhor. Vou estender a data do pagamento até {formatted_date}"
```

**Variáveis:**
- `{formatted_date}`: Nova data de pagamento (ex: "20 de março")
- `{offer_details}`: Detalhes da oferta (ex: "R$ 120,00 à vista")

**Contexto:** Cliente reclama que data de pagamento é muito próxima
**Tool:** `date_complaint()`

**Exemplo:**
> "Entendo que você está desconfortável com a data do pagamento, tenho um pouco de flexibilidade para negociar uma data melhor. Posso estender a data do pagamento até 20 de março. Que tal pagar R$ 120,00 à vista nessa nova data?"

---

#### 26. Confirmação de Aceitação
```python
"Entendi que você está aceitando a proposta de acordo que te apresentei, posso confirmar?"
```

**Contexto:** Cliente demonstra aceitação mas não é explícito
**Ação:** Aguarda confirmação clara

---

#### 27. Sucesso - Oferta Parcelada
```python
"Fico feliz que chegamos em um acordo! Você aceitou a oferta: {offer_details}. Você vai receber o boleto da primeira parcela por e-mail com vencimento em {formatted_payment_date}. As outras parcelas seguirão o mesmo vencimento da sua fatura original. A oferta só é válida se o pagamento ocorrer até a data de vencimento. Se puder, fica na linha e responde rapidinho nossa pesquisa de satisfação. Obrigada!"
```

**Variáveis:**
- `{offer_details}`: "3x de R$ 50,00"
- `{formatted_payment_date}`: "15 de março"

**Contexto:** Cliente aceita oferta parcelada
**Tool:** `accept_offer(id=X)`
**Ação:** Registra no Nectar → Transfer para survey

**Exemplo:**
> "Fico feliz que chegamos em um acordo! Você aceitou a oferta: 3x de R$ 50,00. Você vai receber o boleto da primeira parcela por e-mail com vencimento em 15 de março. As outras parcelas seguirão o mesmo vencimento da sua fatura original. A oferta só é válida se o pagamento ocorrer até a data de vencimento. Se puder, fica na linha e responde rapidinho nossa pesquisa de satisfação. Obrigada!"

---

#### 28. Sucesso - Oferta à Vista (Única Parcela)
```python
"Fico feliz que chegamos em um acordo! Você aceitou a oferta: {offer_details}. Você vai receber o boleto por e-mail com vencimento em {formatted_payment_date}. A oferta só é válida se o pagamento ocorrer até a data de vencimento. Se puder, fica na linha e responde rapidinho nossa pesquisa de satisfação. Obrigada!"
```

**Variáveis:**
- `{offer_details}`: "R$ 120,00 à vista"
- `{formatted_payment_date}`: "15 de março"

**Contexto:** Cliente aceita oferta à vista
**Ação:** Registra no Nectar → Transfer para survey

---

#### 29. Oferta Não Encontrada (Fallback)
```python
"Desculpe, não encontrei a opção de negociação no sistema, vamos tentar novamente?"
```

**Contexto:** Erro interno ao buscar oferta
**Ação:** Retry

---

#### 30. Cliente Alega Pagamento - Há Mais de 2 Dias
```python
"No nosso sistema o pagamento ainda consta como pendente. Para melhor entendermos o que aconteceu, vou te enviar um SMS com um link pra você enviar o comprovante do pagamento. Qualquer dúvida, entra em contato com a gente pelo app. Se puder, fica na linha e responde rapidinho nossa pesquisa de satisfação. Obrigada!"
```

**Contexto:** Cliente diz que pagou há mais de 2 dias
**Tool:** `payment_claim(days_since_payment > 2)`
**Ação:** Transfer para survey

---

#### 31. Cliente Alega Pagamento - Há Menos de 2 Dias
```python
"Faz menos de 2 dias que você pagou, então o sistema pode ainda não ter registrado. O prazo máximo é de 48 horas e você pode acompanhar o andamento pelo app da Vivo. Qualquer dúvida, entra em contato com a gente pelo app. Se puder, fica na linha e responde rapidinho nossa pesquisa de satisfação. Obrigada!"
```

**Contexto:** Cliente diz que pagou recentemente
**Tool:** `payment_claim(days_since_payment < 2)`
**Ação:** Transfer para survey

---

#### 32. Cliente Alega Pagamento - Exatamente 2 Dias
```python
"Faz 2 dias que você pagou, então o sistema pode ainda não ter registrado. O prazo máximo é de 48 horas e você pode acompanhar o andamento pelo app da Vivo. Qualquer dúvida, entra em contato com a gente pelo app. Se puder, fica na linha e responde rapidinho nossa pesquisa de satisfação. Obrigada!"
```

**Contexto:** Exatamente 2 dias desde pagamento
**Ação:** Transfer para survey

---

#### 33. Cliente Alega Pagamento - Exatamente 1 Dia
```python
"Faz 1 dia que você pagou, então o sistema pode ainda não ter registrado. O prazo máximo é de 48 horas e você pode acompanhar o andamento pelo app da Vivo. Qualquer dúvida, entra em contato com a gente pelo app. Se puder, fica na linha e responde rapidinho nossa pesquisa de satisfação. Obrigada!"
```

**Contexto:** Pagamento há 1 dia
**Ação:** Transfer para survey

---

#### 34. Cliente Acabou de Pagar
```python
"Você acabou de pagar, então o sistema pode ainda não ter registrado. O prazo máximo é de 48 horas e você pode acompanhar o andamento pelo app da Vivo. Qualquer dúvida, entra em contato com a gente pelo app. Se puder, fica na linha e responde rapidinho nossa pesquisa de satisfação. Obrigada!"
```

**Contexto:** Pagamento muito recente (hoje)
**Ação:** Transfer para survey

---

#### 35. Confirmação de Pagamento (Pergunta)
```python
"Você pode me confirmar se o pagamento foi feito há mais de 2 dias?"
```

**Contexto:** Cliente diz que pagou mas não especifica quando
**Ação:** Aguarda confirmação de dias

---

#### 36. Nenhuma Oferta / Recusa Todas
```python
"Poxa, ainda não chegamos a um acordo. Quando puder, fala com a gente pelo app da Vivo pra resolver suas faturas atrasadas. Se puder, fica na linha e responde rapidinho nossa pesquisa de satisfação. Obrigada!"
```

**Contexto:** Cliente recusou todas as ofertas disponíveis
**Tool:** `end_call(reason="no_more_offers")`
**Ação:** Transfer para survey

---

#### 37. Sem Acordo - Transferir para Atendente
```python
"Poxa, ainda não chegamos a um acordo. Vou te transferir pra um atendente pra resolver isso contigo. Só um instante."
```

**Contexto:** Cliente solicita atendente após recusar ofertas
**Tool:** `end_call(last_action="agent")`
**Ação:** Transfer SIP para fila de atendentes

---

#### 38. Cliente Ocupado - Callback
```python
"Sem problemas! Posso tentar ligar em outro momento. Qual seria o melhor período: manhã, tarde ou noite?"
```

**Contexto:** Cliente não pode falar agora
**Tool:** `save_best_callback_window()`
**Ação:** Aguarda resposta de horário

---

#### 39. Confirmar Melhor Horário
```python
"Certo! Vou anotar isso. Obrigado pelo seu tempo e até logo!"
```

**Contexto:** Após cliente informar melhor horário para callback
**Tool:** `save_best_callback_window(period="manhã/tarde/noite")`
**Ação:** Encerra ligação (end_call)

---

#### 40. Callback Sem Confirmar Horário
```python
"Temos um assunto importante para tratar, então ligarei novamente em outra oportunidade. Obrigado."
```

**Contexto:** Cliente não pode falar mas não especifica horário
**Ação:** Encerra ligação

---

#### 41. Suspeita de Fraude
```python
"Vamos tentar entender o que está acontecendo. Vou te enviar um formulário por SMS. Preenche e devolve pra gente continuar a investigação, em até sete dias daremos uma devolutiva. Qualquer dúvida, estamos por aqui. Se puder, fica na linha e responde rapidinho nossa pesquisa de satisfação. Obrigada!"
```

**Contexto:** Cliente alega fraude ou não reconhece cobrança
**Tool:** `handle_fraud()`
**Ação:** Transfer para survey

---

#### 42. Transferência para Atendente
```python
"Tudo bem, já vou te transferir para um atendente. Um momento."
```

**Contexto:** Cliente solicita atendente humano
**Tool:** `end_call(last_action="agent")`
**Ação:** Transfer SIP para fila de atendentes

---

#### 43. Falecimento (Durante Negociação)
```python
"Meus sentimentos pela sua perda. Estamos com você neste momento difícil. Quando puder, por favor, fale com a gente no um zero três quinze para informar o falecimento. Assim, podemos cancelar o serviço ou transferir a titularidade. Estamos à disposição para ajudar. A Vivo agradece a compreensão e atenção."
```

**Contexto:** Cliente informa falecimento durante negociação
**Ação:** Encerra ligação

---

#### 44. Cliente Não é Vivo / Identidade Inválida
```python
"Desculpa o incômodo. Se topar vir pra Vivo, temos ofertas bem legais esperando por você. Dá uma olhada em vivo ponto com ponto bê érre. Até já!"
```

**Contexto:** Cliente diz que não é Vivo ou número errado
**Ação:** Encerra ligação

---

## 🔧 MENSAGENS DE SISTEMA

**Arquivo:** `agent.py` (VivoCollectionsAgent)

### Integração Nectar

#### 45. Aguardando Carregamento de Dados
```python
"Aguarde um momento enquanto carrego suas informações."
```

**Contexto:** Início da chamada, enquanto busca dados do Nectar via OCI Function
**Tool:** `wait_for_nectar()`
**Áudio:** Toca música de espera (SUMMER_UPBEAT)

---

#### 46. Erro de Integração com Nectar
```python
"Desculpe, ocorreu um problema no nosso sistema. Ligarei novamente em outro momento."
```

**Contexto:** Timeout ou erro ao buscar dados do Nectar
**Ação:** Encerra ligação (end_call)

---

#### 47. Erro ao Registrar Negociação
```python
"Olha... Tive um erro ao registrar nossa negociação. Você pode continuar pelo App Vivo. Ah, para ajudar você também pode pagar a sua fatura com cartão de crédito... Muito obrigada pela compreensão!"
```

**Contexto:** Falha ao salvar acordo no Nectar
**Ação:** Transfer para survey (tenta salvar mesmo com erro)

---

#### 48. Processando Registro
```python
"Só um momento por favor, estou registrando umas informações rapidinho."
```

**Contexto:** Após aceite de oferta, enquanto registra no Nectar
**Áudio:** Toca som de digitação (KEYBOARD_TYPING)

---

### Gerenciamento de Sessão

**Arquivo:** `main.py` e `main.new.py`

#### 49. Usuário Afastado (Away) - Verificação
```python
"Olá, você ainda está por aí?"
```

**Contexto:** Timeout de user_away (10s sem fala)
**Ação:** Aguarda resposta (5s)

---

#### 50. Usuário Não Respondendo - Encerramento
```python
"Parece que você não está aí, então vou desligar. Até logo"
```

**Contexto:** Usuário não responde após verificação de away
**Ação:** Encerra ligação (hangup)

---

#### 51. Modo Dev - Exemplo Não Disponível
```python
"Nenhum exemplo disponível"
```

**Contexto:** Dev mode ativado mas sem exemplos configurados
**Ação:** Exception (apenas em dev)

---

#### 52. Aviso de Produção em Dev Mode
```python
"ATENÇÃO! Você está executando o agente no servidor de produção no modo de testes (clientes aleatórios)"
```

**Contexto:** Dev mode ativado em ambiente de produção
**Ação:** Log warning (terminal)

---

## 🎵 ÁUDIO E EFEITOS SONOROS

**Arquivo:** `agent.py`

### Background Audio

#### 53. Música de Espera
```python
BackgroundAudioPlayer()
audio.set_playing(True)
# Clip: BuiltinAudioClip.SUMMER_UPBEAT
```

**Contexto:**
- Durante `wait_for_nectar()` (carregamento de dados)
- Volume: Padrão

**Funcionalidade:**
- STT é desativado durante reprodução (previne reconhecimento do próprio áudio)

---

#### 54. Som de Digitação (Typing 1)
```python
BackgroundAudioPlayer()
# Clip: BuiltinAudioClip.KEYBOARD_TYPING
# Volume: 0.8
```

**Contexto:** Durante registro de informações no Nectar

---

#### 55. Som de Digitação (Typing 2)
```python
BackgroundAudioPlayer()
# Clip: BuiltinAudioClip.KEYBOARD_TYPING2
# Volume: 0.7
```

**Contexto:** Alternativa para som de digitação

---

#### 56. Som Ambiente de Escritório
```python
BackgroundAudioPlayer()
# Clip: BuiltinAudioClip.OFFICE_AMBIENCE
# Volume: 0.8
```

**Contexto:** Ambiente de fundo durante toda a ligação (se configurado)

---

## 🔄 VARIÁVEIS DINÂMICAS

**Arquivo:** `agents/_common/states.py` e prompts

### Variáveis de Cliente

| Variável | Descrição | Exemplo | Fonte |
|----------|-----------|---------|-------|
| `{customer_name}` | Nome completo | "João da Silva" | Header SIP |
| `{customer_first_name}` | Primeiro nome | "João" | Parseia `customer_name` |
| `{cnpjcpf}` | CPF/CNPJ | "12345678901" | Header SIP |
| `{phone}` | Telefone | "+5511999999999" | SIP phoneNumber |
| `{idcrm}` | ID do CRM | "CRM123456" | Header SIP |
| `{call_id}` | ID da chamada | "call_abc123" | Header SIP |

### Variáveis de Tempo

| Variável | Descrição | Exemplo | Lógica |
|----------|-----------|---------|--------|
| `{greeting}` | Saudação | "Boa tarde" | Ver tabela abaixo |
| `{date}` | Data atual | "24 de novembro de 2025" | `datetime.now()` |
| `{time}` | Hora atual | "14:30" | `datetime.now()` |

#### Lógica de Saudação

```python
def greeting_based_on_time():
    hour = datetime.now().hour
    if 5 <= hour < 12:
        return "Bom dia"
    elif 12 <= hour < 18:
        return "Boa tarde"
    else:  # 18 <= hour or hour < 5
        return "Boa noite"
```

| Horário | Saudação |
|---------|----------|
| 05:00 - 11:59 | "Bom dia" |
| 12:00 - 17:59 | "Boa tarde" |
| 18:00 - 04:59 | "Boa noite" |

### Variáveis de Dívida

| Variável | Descrição | Exemplo | Fonte |
|----------|-----------|---------|-------|
| `{total_debt}` | Valor total | "R$ 150,00" | Nectar API |
| `{debt_dates_summary}` | Datas vencimento | "15/01/2025 e 15/02/2025" | Nectar API |
| `{products_text}` | Produtos | "Linha Móvel" | Nectar API |
| `{payment_date}` | Data de pagamento | "15/03/2025" | Primeira dívida |
| `{formatted_payment_date}` | Data formatada | "15 de março" | `babel.dates.format_date()` |

### Variáveis de Oferta

| Variável | Descrição | Exemplo | Fonte |
|----------|-----------|---------|-------|
| `{offer_id}` | ID da oferta | 1, 2, 3... | State |
| `{offer_details}` | Detalhes | "R$ 120,00 à vista" | Formatado |
| `{offer_details}` | Detalhes parcelado | "3x de R$ 50,00" | Formatado |
| `{offers}` | Lista de ofertas | Array de objetos | Nectar API |

### Variáveis de Data

| Variável | Descrição | Exemplo |
|----------|-----------|---------|
| `{formatted_date}` | Data estendida | "20 de março" |
| `{days_since_payment}` | Dias desde pgto | 3 |

---

## 📋 CÓDIGOS DE STATUS

**Arquivo:** `common/integration.py` e `agents/_common/states.py`

### Códigos de Andamento (Nectar)

Quando o agente encerra, envia um código de status para o Nectar:

| Código | Descrição | Quando Usar |
|--------|-----------|-------------|
| `Robô-CLIENTE DESLIGOU - SEM PID` | Cliente desligou antes de validar CPF | Name validation OK, sem PID, cliente encerra |
| `Robô-CLIENTE DESLIGOU - COM PID` | Cliente desligou após validar CPF | PID validation OK, cliente encerra |
| `Robô-SEM INTERACAO - SEM PID` | Sem interação (timeout) antes de CPF | User away timeout sem PID |
| `Robô-ACORDO A VISTA` | Acordo fechado à vista | Aceite de oferta com 1 parcela |
| `Robô-ACORDO PARCELADO` | Acordo fechado parcelado | Aceite de oferta com 2+ parcelas |
| `Robô-Erro WS - 2 Via Nao Entendeu` | Erro ao registrar negociação | Falha ao salvar acordo no Nectar |
| `Robô-DESCONHECE CLIENTE` | Cliente não é titular/não conhece | Name validation falha |
| `Robô-ALEGA PAGAMENTO MAIOR QUE 2 DIAS` | Cliente diz que pagou (>2 dias) | Tool: payment_claim(days > 2) |
| `Robô-FALECIDO` | Titular faleceu | Tool: end_call(reason="deceased") |
| `Robô-RECADO` | Deixou recado com terceiro | Name validation - terceiro atende |
| `Robô-DESLIGOU RETORNO LIGACAO` | Cliente pede callback | Tool: save_best_callback_window() |
| `Robô-DESCONHECE DIVIDA` | Cliente não reconhece dívida | Tool: handle_fraud() |
| `Robô-ALEGA PAGAMENTO MENOR QUE 2 DIAS` | Cliente diz que pagou (<2 dias) | Tool: payment_claim(days <= 2) |
| `Robô-Quer Falar com Atendente` | Cliente solicita atendente | Tool: end_call(last_action="agent") |
| `Robô-NAO PODE PAGAR` | Cliente recusou todas ofertas | Tool: end_call(reason="no_more_offers") |
| `Robô-ERRO WS` | Erro genérico de integração | Timeout ou erro na API Nectar |

### Códigos de Last Action

| Código | Descrição | Ação no SIP |
|--------|-----------|-------------|
| `survey` | Transferir para pesquisa | Transfer SIP para número de survey |
| `agent` | Transferir para atendente | Transfer SIP para fila de atendentes |
| `end` | Encerrar ligação | Hangup / Delete room |

---

## 🔍 CORREÇÕES AUTOMÁTICAS DE STT

**Arquivo:** `agent.py` - método `on_user_turn_completed()`

### Mapeamento de Erros Comuns

O agente corrige automaticamente erros de transcrição do STT:

| Transcrição Incorreta | Correção | Contexto |
|----------------------|----------|----------|
| "fim", "thing", "fem" | `"Sim."` | Resposta afirmativa |
| "si", "sí", "pin" | `"Sim."` | Resposta afirmativa |
| "sem", "vem", "tiga" | `"Sim."` | Resposta afirmativa |
| "thin", "tem" | `"Sim."` | Resposta afirmativa |
| "semelhantes" | `"Sim, sou eu."` | Confirmação de identidade |
| "mão", "tão" | `"Não."` | Resposta negativa |

### Fallback para Não Entendimento

Quando o agente não consegue interpretar a resposta:

```python
# Em vários contextos
"Desculpe, não entendi"
```

**Ação:** Solicita que cliente repita

---

## 📊 RESUMO EXECUTIVO

### Categorização por Tipo

| Tipo | Quantidade | Exemplos |
|------|-----------|----------|
| **Saudações** | 3 | "Bom dia", "Boa tarde", "Boa noite" |
| **Validações** | 15 | Confirmar nome, CPF, identidade |
| **Negociação** | 25+ | Ofertas, objeções, fechamento |
| **Encerramento** | 10 | Survey, atendente, callback |
| **Erros/Sistema** | 5 | Falhas de integração, timeouts |
| **Áudio/Efeitos** | 4 | Música, digitação, ambiente |
| **Variáveis** | 20+ | Nome, valores, datas |

### Mensagens Mais Usadas (Top 10)

1. **"{Saudação}! Estou falando com {Nome}?"** - Primeira mensagem em toda chamada
2. **"Aguarde um momento enquanto carrego suas informações."** - Início de toda chamada
3. **"Muito obrigada por confirmar."** - Confirmação de nome
4. **"Para confirmar sua identidade, informe os três primeiros dígitos do seu CPF?"** - PID validation
5. **"Consta um saldo em aberto de R$ X..."** - Introdução de dívida
6. **"Fico feliz que chegamos em um acordo!"** - Sucesso de negociação
7. **"Tudo bem, já vou te transferir para um atendente."** - Escalation
8. **"Se puder, fica na linha e responde nossa pesquisa de satisfação."** - Encerramento
9. **"Meus sentimentos pela sua perda..."** - Falecimento
10. **"Desculpa o incômodo. A Vivo agradece pelo seu tempo."** - Encerramento genérico

### Mensagens Críticas para o Negócio

| Mensagem | Importância | Motivo |
|----------|-------------|--------|
| Introdução de dívida | 🔴 CRÍTICA | Primeira impressão da negociação |
| Sucesso de acordo | 🔴 CRÍTICA | Confirmação de conversão |
| Instruções de pagamento | 🔴 CRÍTICA | Cliente precisa saber como pagar |
| Reclamação de data | 🟡 ALTA | Objeção comum, precisa flexibilidade |
| Cliente já pagou | 🟡 ALTA | Evita frustração do cliente |
| Transferência para atendente | 🟡 ALTA | Escalation path importante |

### Oportunidades de Melhoria

1. **Personalização de saudação:** Considerar tom mais personalizado por perfil de cliente
2. **Variações de mensagem:** Adicionar variações para evitar robotização
3. **A/B Testing:** Testar diferentes abordagens de introdução de dívida
4. **Localização:** Adaptar expressões por região (ex: "num" vs "número")
5. **Empatia aumentada:** Reforçar mensagens empáticas em objeções

---

## 📁 REFERÊNCIAS DE ARQUIVOS

### Prompts de Sistema (Markdown)

```
agents/
├── name_validation/
│   └── prompt.md                    # Prompts do Name Validation
├── pid_validation/
│   └── prompt.md                    # Prompts do PID Validation
└── negotiation/
    ├── prompt.md                    # Prompts da Negociação
    └── special_offers.md            # Instruções de ofertas especiais
```

### Código Python (Mensagens)

```
livekit-worker-current-prod/
├── agent.py                         # VivoCollectionsAgent (mensagens de sistema)
├── main.py                          # Session management (timeouts, away)
├── main.new.py                      # Versão alternativa
│
├── agents/
│   ├── _common/
│   │   └── states.py               # Variáveis dinâmicas (saudações, datas)
│   ├── name_validation/
│   │   └── agent.py                # Mensagens de validação de nome
│   ├── pid_validation/
│   │   └── agent.py                # Mensagens de validação de CPF
│   └── negotiation/
│       └── agent.py                # Mensagens de negociação
│
└── common/
    └── integration.py              # Códigos de status Nectar
```

---

## 🔄 FLUXO DE DECISÃO POR MENSAGEM

### Árvore de Decisão Simplificada

```
INÍCIO
  └─ "Aguarde enquanto carrego informações"
       └─ "{Saudação}! Estou falando com {Nome}?"
            ├─ SIM → "Muito obrigada"
            │         └─ PID? → "Informe 3 dígitos CPF"
            │                    ├─ CORRETO → Negotiation
            │                    └─ ERRO (3x) → "Não posso prosseguir"
            │
            ├─ NÃO → "Está com {Nome}?"
            │         ├─ SIM → "Consegue passar telefone?"
            │         └─ NÃO → "Desculpa o incômodo"
            │
            ├─ FALECIDO → "Meus sentimentos"
            │
            └─ ATENDENTE → "Vou te transferir"

NEGOTIATION
  └─ "Consta saldo em aberto de R$ X..."
       └─ Apresenta ofertas
            ├─ ACEITA → "Fico feliz que chegamos em acordo!"
            │            └─ "Você vai receber boleto por e-mail"
            │                 └─ "Fica na linha para pesquisa"
            │
            ├─ DATA RUIM → "Posso estender até DD/MM"
            │               └─ Reapresenta oferta
            │
            ├─ JÁ PAGUEI → "No sistema consta pendente"
            │               └─ "Vou enviar SMS para comprovante"
            │
            ├─ FRAUDE → "Vou enviar formulário por SMS"
            │
            ├─ OCUPADO → "Qual melhor horário? Manhã/tarde/noite?"
            │
            ├─ ATENDENTE → "Vou te transferir"
            │
            └─ RECUSA TUDO → "Não chegamos em acordo"
                              └─ "Fala com a gente pelo app"

ENCERRAMENTO
  └─ Survey / Atendente / Hangup
```

---

## 📝 NOTAS FINAIS

### Boas Práticas Identificadas

1. **Consistência:** Todas as mensagens usam "você" (evita "senhor/senhora")
2. **Naturalidade:** Expressões como "rapidinho", "poxa" tornam agente mais humano
3. **Clareza:** Informações importantes (valores, datas) são explícitas
4. **Empatia:** Mensagens de falecimento, dificuldades financeiras são respeitosas
5. **Call-to-action:** Sempre indica próximo passo ("fica na linha", "fala com a gente pelo app")

### Pontos de Atenção

1. **Números por extenso:** "um zero três quinze" (1015) pode ser confuso
2. **URL por extenso:** "vivo ponto com ponto bê érre" pode ser difícil de entender
3. **Repetição:** "Se puder, fica na linha..." aparece em muitas mensagens
4. **Abreviações:** "Um momento" vs "Um minutinho" - falta consistência

### Métricas Sugeridas

Para análise futura, considerar medir:
- Taxa de confirmação após cada mensagem específica
- Mensagens que mais causam transferência para atendente
- Variações de resposta por mensagem (sucesso vs escalation)
- Tempo médio de fala por mensagem

---

**Documento preparado por:** Claude (AI Assistant)
**Data:** 2025-11-24
**Versão:** 1.0
**Total de mensagens mapeadas:** 60+
**Status:** Completo e pronto para uso

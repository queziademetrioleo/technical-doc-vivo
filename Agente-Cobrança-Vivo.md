## Sistema de Cobrança Inteligente com IA Conversacional

---

## 📋 Índice

1. [Visão Geral](#-visão-geral)
2. [Como Funciona](#-como-funciona)
3. [Arquitetura do Sistema](#-arquitetura-do-sistema)
4. [Os Agentes Inteligentes](#-os-agentes-inteligentes)
5. [Tecnologias Utilizadas](#-tecnologias-utilizadas)
6. [Fluxo de Uma Chamada](#-fluxo-de-uma-chamada)
7. [Funcionalidades Principais](#-funcionalidades-principais)
8. [Inteligência Artificial](#-inteligência-artificial)
9. [Segurança e Compliance](#-segurança-e-compliance)
10. [Benefícios e Resultados](#-benefícios-e-resultados)

---

## 🎯 Visão Geral

O **Papel Agent VIVO** é um sistema de cobrança automatizada que utiliza inteligência artificial para realizar ligações telefônicas e negociar dívidas de forma natural e humanizada. O sistema combina tecnologias avançadas de reconhecimento de voz, processamento de linguagem natural e síntese de voz para criar uma experiência conversacional eficiente e respeitosa.

### O Que o Sistema Faz?

- **Realiza ligações automáticas** para clientes com contas em aberto
- **Valida a identidade** do cliente de forma segura
- **Apresenta ofertas personalizadas** de pagamento baseadas no perfil do cliente
- **Negocia** de forma inteligente, adaptando-se às respostas do cliente
- **Registra automaticamente** todas as interações e resultados
- **Transfere** para atendimento humano quando necessário

### Por Que é Diferente?

Diferente de sistemas tradicionais de URA (Unidade de Resposta Audível), o Papel Agent VIVO:

- ✅ **Entende linguagem natural** - Não usa menus numéricos
- ✅ **Responde contextualizadamente** - Entende intenções, não apenas palavras-chave
- ✅ **Adapta a conversa** - Muda de estratégia baseado nas respostas
- ✅ **Detecta interrupções** - Permite que o cliente fale a qualquer momento
- ✅ **Soa natural** - Usa vozes sintetizadas de alta qualidade

---

## 🔄 Como Funciona

O sistema opera através de **três agentes especializados** que trabalham em sequência, cada um com uma responsabilidade específica:

```
┌─────────────────────────────────────────────────────────────────┐
│                     JORNADA DO CLIENTE                          │
└─────────────────────────────────────────────────────────────────┘

    📞 LIGAÇÃO RECEBIDA
         ↓
    ┌────────────────────────────────────┐
    │  AGENTE 1: Validação de Nome       │
    │  "Estou falando com João Silva?"   │
    └────────────────────────────────────┘
         ↓
    SIM → Próximo Agente
    NÃO → Verifica se conhece o titular
         ↓
    ┌────────────────────────────────────┐
    │  AGENTE 2: Validação de CPF        │
    │  "Quais os 3 primeiros dígitos     │
    │   do seu CPF?"                     │
    └────────────────────────────────────┘
         ↓
    CORRETO → Próximo Agente
    INCORRETO → Retry (máx. 3 tentativas)
         ↓
    ┌────────────────────────────────────┐
    │  AGENTE 3: Negociação              │
    │  "Você tem uma conta em aberto     │
    │   de R$ 150,00..."                 │
    └────────────────────────────────────┘
         ↓
    OFERTAS APRESENTADAS → ACEITA → ✅ ACORDO REGISTRADO
                        → RECUSA → 📊 TRANSFERE PARA PESQUISA
                                → 👤 TRANSFERE PARA ATENDENTE
```

### Processo Passo a Passo
   
<img width="822" height="919" alt="_- visual selection (3)" src="https://github.com/user-attachments/assets/3f7ed795-8b2a-49c2-8c27-4f4503c92940" />

---

## 🏗️ Arquitetura do Sistema

### Visão de Alto Nível

```
┌─────────────────────────────────────────────────────────────────────┐
│                    ** CAMADA DE COMUNICAÇÃO**                       │
│  ┌──────────────┐         ┌──────────────┐         ┌─────────────┐  │
│  │   Cliente    │ ←──────→│  LiveKit     │ ←──────→│  Sistema    │  │
│  │  (Telefone)  │   SIP   │   Session    │  WebRTC │    VIVO     │  │
│  └──────────────┘         └──────────────┘         └─────────────┘  │
└─────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────┐
│                **CAMADA DE PROCESSAMENTO DE VOZ**                   │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐  ┌──────────┐             │
│  │   VAD    │→ │   STT    │→ │   LLM    │→ │   TTS    │             │
│  │ (Silero) │  │(Whisper) │  │ (OCI AI) │  │(ElevenLab│             │
│  └──────────┘  └──────────┘  └──────────┘  └──────────┘             │
│                                                                     │
│  VAD: Detecta quando o cliente está falando                         │
│  STT: Converte voz em texto (Speech-to-Text)                        │
│  LLM: Processa intenção e gera resposta                             │
│  TTS: Converte resposta em voz (Text-to-Speech)                     │
└─────────────────────────────────────────────────────────────────────┘ 
                                    ↓
┌─────────────────────────────────────────────────────────────────────┐
│                   **CAMADA DE INTELIGÊNCIA**                        │
│  ┌───────────────┐  ┌───────────────┐  ┌──────────────┐             │
│  │  Agente 1     │→ │  Agente 2     │→ │  Agente 3    │             │
│  │  Validação    │  │  Validação    │  │  Negociação  │             │
│  │  de Nome      │  │  de CPF       │  │              │             │
│  └───────────────┘  └───────────────┘  └──────────────┘             │
│                                                                     │
│  Cada agente é especializado e trabalha de forma autônoma           │
└─────────────────────────────────────────────────────────────────────┘
                                    ↓
┌─────────────────────────────────────────────────────────────────────┐
│                      **CAMADA DE DADOS**                            │
│  ┌─────────────────┐         ┌─────────────────┐                    │
│  │  Oracle DB      │         │  OCI Functions  │                    │
│  │  (Autônomo)     │         │  (Nectar CRM)   │                    │
│  │                 │         │                 │                    │
│  │  • Estados      │         │  • Importa dados│                    │
│  │  • Logs         │         │  • Salva acordos│                    │
│  │  • Histórico    │         │  • Gera ofertas │                    │
│  └─────────────────┘         └─────────────────┘                    │
└─────────────────────────────────────────────────────────────────────┘
```

<img width="876" height="684" alt="_- visual selection (4)" src="https://github.com/user-attachments/assets/a495ab15-8983-4a49-8e83-8e4d763df56e" />


### Componentes Principais

| Componente | Função | Tecnologia |
|------------|--------|------------|
| **LiveKit Session** | Gerencia a conexão de voz em tempo real | LiveKit Agents Framework |
| **Orquestrador** | Coordena os 3 agentes e o fluxo geral | VivoCollectionsAgent |
| **VAD** | Detecta quando há voz ativa | Silero VAD (ONNX) |
| **STT** | Transcreve voz para texto | FasterWhisper |
| **LLM** | Processa linguagem e decide respostas | OCI Generative AI (Cohere) |
| **TTS** | Converte texto em voz natural | ElevenLabs |
| **Database** | Armazena estados e logs | Oracle Autonomous Database |
| **CRM Integration** | Integra com sistema de cobrança | OCI Functions (Nectar) |

---

## 🤖 Os Agentes Inteligentes

O sistema utiliza uma arquitetura multi-agente, onde cada agente é especializado em uma etapa da conversa.

### 🔵 Agente 1: Validação de Nome

**Objetivo:** Confirmar que está falando com o titular da conta

**Personalidade:** Cordial, educado, direto

**Exemplo de Conversa:**
```
🤖 Agente: "Boa tarde! Aqui é a assistente virtual da VIVO.
            Estou falando com Maria da Silva?"

👤 Cliente: "Sim, sou eu."

🤖 Agente: [Marca confirmação e transfere para próximo agente]
```

**Situações que o Agente Trata:**

- ✅ Cliente confirma ser o titular → Avança
- ❌ Outra pessoa atendeu → Verifica se conhece o titular
- 💀 Titular faleceu → Encerra com mensagens de condolências
- 🤚 Cliente pede atendente humano → Transfere imediatamente

**Estado Gerenciado:**
- Nome do cliente
- Se o nome foi confirmado
- Se está falando com terceiro
- Se terceiro conhece o titular

---

### 🟢 Agente 2: Validação de CPF

**Objetivo:** Validar a identidade através dos primeiros 3 dígitos do CPF

**Personalidade:** Profissional, seguro, objetivo

**Exemplo de Conversa:**
```
🤖 Agente: "Para sua segurança, poderia me informar os três
            primeiros dígitos do seu CPF?"

👤 Cliente: "Um, dois, três."

🤖 Agente: [Valida] "Perfeito! Identidade confirmada."
```

**Regras de Segurança:**

- 🔒 Máximo de **3 tentativas** de validação
- 🔒 Se errar 3 vezes → Encerra por segurança
- 🔒 Se recusar informar → Insiste uma vez, depois encerra
- 🔒 Registra todas as tentativas para auditoria

**Estado Gerenciado:**
- Primeiros 3 dígitos esperados do CPF
- Número de tentativas realizadas
- Se o CPF foi validado com sucesso

---

### 🟡 Agente 3: Negociação

**Objetivo:** Negociar o pagamento da dívida com empatia e eficiência

**Personalidade:** Empático, flexível, solucionador

**Exemplo de Conversa:**
```
🤖 Agente: "Maria, consta um saldo em aberto de R$ 150,00,
            com vencimento em 15 de janeiro. Para facilitar,
            posso oferecer um pagamento à vista de R$ 120,00
            com 20% de desconto. O que acha?"

👤 Cliente: "Não tenho esse valor agora."

🤖 Agente: "Entendo perfeitamente. Que tal parcelar em 3 vezes
            de R$ 50,00? A primeira parcela vence dia 20."

👤 Cliente: "Assim fica melhor, aceito."

🤖 Agente: [Registra acordo] "Ótimo! Vou registrar seu acordo.
            Você receberá um SMS com todos os detalhes."
```

**Estratégias de Negociação:**

1. **Apresenta ofertas gradualmente** (uma por vez)
2. **Oferece descontos** em pagamentos à vista
3. **Oferece parcelamento** para facilitar
4. **Lida com objeções comuns:**
   - "Já paguei" → Verifica se pagou há mais ou menos de 2 dias
   - "Não conheço essa dívida" → Envia formulário de contestação por SMS
   - "Data ruim" → Oferece estender o prazo
   - "Não posso pagar" → Agenda retorno em momento melhor

**Ofertas Personalizadas:**

As ofertas são carregadas do sistema Nectar e podem incluir:
- Pagamento à vista com desconto
- Parcelamento em 2x, 3x, até 6x
- Prazos estendidos
- Ofertas especiais baseadas no perfil do cliente

**Estado Gerenciado:**
- Valor total da dívida
- Detalhes dos contratos
- Lista de ofertas disponíveis
- Ofertas já apresentadas
- Oferta aceita pelo cliente
- Data de pagamento escolhida
- Objeções do cliente

---

### 🎭 Orquestrador Principal

**VivoCollectionsAgent** - O Maestro do Sistema

Além dos 3 agentes especializados, existe um **agente orquestrador** que:

- 🎵 Toca música de espera durante carregamento de dados
- 🔄 Gerencia as transições entre agentes (handover)
- 💾 Salva o estado completo no banco de dados
- 📊 Gera logs estruturados para análise
- 📤 Faz upload de gravações para armazenamento em nuvem
- 📞 Controla transferências SIP (para pesquisa ou atendente)
- ⏸️ Desabilita reconhecimento de voz durante músicas

---

## 💻 Tecnologias Utilizadas

### Stack Principal

<img width="1080" height="1176" alt="_- visual selection (2)" src="https://github.com/user-attachments/assets/33472eca-6bd7-41c4-aeea-bf3c81eed558" />



```
┌─────────────────────────────────────────────────────────────┐
│                    PILHA TECNOLÓGICA                         │
└─────────────────────────────────────────────────────────────┘

🔵 FRAMEWORK CORE
   • LiveKit Agents Framework
     → Gerenciamento de sessões de voz em tempo real
     → WebRTC para comunicação de baixa latência
     → SIP para integração telefônica

🟢 INTELIGÊNCIA ARTIFICIAL
   • OCI Generative AI (Oracle Cloud)
     → Modelo: Cohere Command
     → Temperature: 0.0 (respostas determinísticas)
     → Function calling para ferramentas

🟡 RECONHECIMENTO DE VOZ (STT)
   • FasterWhisper (Atual)
     → Modelo otimizado para português
     → Prompts contextuais por agente
   • OCI Speech (Alternativa homologada)

🟠 SÍNTESE DE VOZ (TTS)
   • ElevenLabs (Atual)
     → Voz natural e expressiva
     → Estabilidade: 1.0, Velocidade: 1.1x
   • OCI Text-to-Speech (Alternativa homologada)

🔴 DETECÇÃO DE VOZ ATIVA (VAD)
   • Silero VAD
     → Detecta quando cliente está falando
     → Threshold: 0.3
     → Mínimo de fala: 150ms

🟣 BANCO DE DADOS
   • Oracle Autonomous Database
     → Armazenamento de estados
     → Logs de todas as interações
     → Histórico completo de chamadas

🟤 CLOUD & INFRAESTRUTURA
   • Oracle Cloud Infrastructure (OCI)
     → OCI Functions (Serverless)
     → Object Storage (Logs e gravações)
     → Generative AI Service

⚫ INTEGRAÇÃO DE SISTEMAS
   • FastAPI
     → API REST para webhooks
   • OCI Functions
     → Integração com CRM Nectar
     → Importação de dados
     → Registro de acordos
```

### Dependências Python

**Core:**
- `livekit` - Framework de comunicação em tempo real
- `livekit-agents` - SDK para agentes de IA
- `oci` - SDK da Oracle Cloud

**AI & ML:**
- `torch`, `torchaudio` - Machine learning
- `deepfilternet`, `pyrnnoise` - Redução de ruído

**Data & Database:**
- `sqlalchemy` - ORM para banco de dados
- `oracledb` - Driver Oracle Database
- `pydantic` - Validação de dados

**Utilities:**
- `fastapi` - API web framework
- `num2words` - Conversão de números para palavras
- `babel` - Internacionalização
- `python-dotenv` - Variáveis de ambiente

### Ferramentas de Processamento

**Redução de Ruído:**
- RNNoise - Algoritmo de redução de ruído neural
- DeepFilterNet - Deep learning para filtragem de áudio

**Detecção de Turnos:**
- Multilingual Turn Detection - Detecta quando cliente terminou de falar
- Endpointing Adaptativo - Evita cortes prematuro

---

## 🧠 Inteligência Artificial

### Como o LLM Toma Decisões

O sistema usa **OCI Generative AI** com o modelo **Cohere Command**, otimizado para:

1. **Compreensão de Intenções**
   - Não depende de palavras-chave exatas
   - Entende contexto e nuances
   - Exemplo: "Tá", "Beleza", "Ok" → Todas interpretadas como aceitação

2. **Function Calling (Ferramentas)**
   - LLM decide quando usar ferramentas específicas
   - Ferramentas disponíveis:
     - `user_first_name_confirmed(bool)` - Confirma nome
     - `validate_cpf(digits)` - Valida CPF
     - `present_offer(id)` - Apresenta oferta
     - `accept_offer(id)` - Aceita oferta
     - `end_call(reason)` - Encerra chamada
     - E outras 10+ ferramentas especializadas

3. **Handover Automático**
   - LLM decide quando passar para próximo agente
   - Baseado no estado e nas respostas do cliente
   - Transições suaves e naturais

### Exemplo de Prompt (Agente de Negociação)

```markdown
# IDENTIDADE
Você é a assistente virtual da VIVO, responsável por negociar
pagamentos de contas em aberto de forma empática e eficiente.

# CONTEXTO ATUAL
Cliente: {customer_name}
Dívida Total: R$ {total_debt}
Ofertas Disponíveis: {offers}

# REGRAS CRÍTICAS
1. NUNCA use as palavras "débito" ou "dívida" → Use "conta em aberto"
2. SEMPRE apresente ofertas uma por vez
3. Se cliente recusar, apresente próxima oferta
4. Se aceitar, chame accept_offer() IMEDIATAMENTE
5. Seja empático mas objetivo
6. Não invente informações que não tem

# FERRAMENTAS DISPONÍVEIS
- present_offer(id) - Marca oferta como apresentada
- accept_offer(id) - Registra acordo
- date_complaint() - Cliente reclama da data
- payment_claim(days) - Cliente alega ter pago
- handle_fraud() - Cliente não reconhece dívida

# EXEMPLO DE CONVERSA
Você: "João, consta uma conta em aberto de R$ 150,00..."
Cliente: "Quanto com desconto?"
Você: "Posso oferecer R$ 120,00 à vista, economizando R$ 30."
```

### Temperature = 0.0

---

## 🔄 Fluxo de Uma Chamada

### Timeline Detalhada

```
⏱️ T+0s - INÍCIO DA CHAMADA
├─ Cliente recebe ligação
├─ LiveKit estabelece sessão SIP
└─ Sistema extrai dados do cabeçalho SIP (nome, CPF, ID do CRM)

⏱️ T+0s - T+3s - CARREGAMENTO ASSÍNCRONO
├─ Agente 1 (Nome) inicia conversa
├─ Em paralelo: Sistema busca dados no Nectar
│  ├─ Contratos com dívidas
│  ├─ Histórico de pagamentos
│  └─ Ofertas personalizadas
└─ Música de espera (se necessário)

⏱️ T+3s - T+30s - VALIDAÇÃO DE IDENTIDADE
├─ Agente 1: Confirma nome
│  └─ "Estou falando com [Nome]?"
├─ Agente 2: Valida CPF
│  └─ "Quais os 3 primeiros dígitos do CPF?"
└─ Transições automáticas entre agentes

⏱️ T+30s - T+180s - NEGOCIAÇÃO
├─ Sistema carrega ofertas do banco de dados
├─ Agente 3 apresenta contexto da dívida
├─ Ofertas apresentadas uma por vez
├─ Tratamento de objeções em tempo real
└─ Registro da decisão do cliente

⏱️ T+180s - T+200s - FINALIZAÇÃO
├─ Se ACORDO:
│  ├─ Registra no Nectar via OCI Function
│  ├─ Envia SMS com detalhes do acordo
│  └─ Transfere para pesquisa de satisfação
├─ Se RECUSA:
│  ├─ Registra código de status
│  └─ Transfere para pesquisa ou atendente
└─ Se ERRO/DESLIGOU:
   └─ Registra motivo e salva estado completo

⏱️ T+200s - FIM
├─ Upload de logs para OCI Object Storage
├─ Gravação da chamada salva
├─ Estado final registrado no Oracle DB
└─ Sessão LiveKit encerrada
```

### Pontos de Decisão Críticos

Durante a chamada, o sistema toma decisões automaticamente:

**1. Validação de Nome:**
- ✅ Confirmado → Continua
- ❌ Negado, mas conhece titular → Agenda callback
- ❌ Negado, faleceu → Encerra com condolências
- 🤚 Pede atendente → Transfere imediatamente

**2. Validação de CPF:**
- ✅ 3 dígitos corretos → Continua
- ❌ Incorreto (tentativa 1-2) → Permite retry
- ❌ Incorreto (tentativa 3) → Encerra por segurança

**3. Negociação:**
- ✅ Aceita oferta → Registra acordo → Pesquisa
- ❌ Recusa todas → Transfere para atendente
- 💭 Objeção → Aplica estratégia específica
- 📅 Quer agendar → Registra melhor horário

---

## ⚙️ Funcionalidades Principais

### 1. Processamento de Áudio em Tempo Real

**Voice Activity Detection (VAD):**
- Detecta quando o cliente está falando
- Evita processar silêncio ou ruído de fundo
- Configuração otimizada:
  - Duração mínima de fala: 150ms
  - Duração mínima de silêncio: 1000ms
  - Threshold de ativação: 0.3

**Turn Detection:**
- Detecta quando o cliente terminou de falar
- Evita interrupções no meio da frase
- Modelo multilíngue otimizado para português

**Interrupção Inteligente:**
- Cliente pode interromper o agente a qualquer momento
- Sistema para de falar e escuta imediatamente
- Duração mínima de interrupção: 800ms

**Redução de Ruído:**
- Remove ruídos de fundo automaticamente
- Algoritmos: RNNoise + DeepFilterNet
- Melhora significativa na precisão do STT

---

### 2. Transcrição Inteligente

**Prompts Contextuais:**
Cada agente usa um prompt específico para melhorar a precisão:

---

### 3. Geração de Respostas (LLM)

**OCI Generative AI:**
- Modelo: Cohere Command (otimizado para português)
- Temperature: 0.0 (máxima consistência)
- Função calling para executar ações

**Exemplo de Processamento:**

```
1. Cliente fala: "Sim, sou eu."
   ↓
2. STT transcreve: "Sim, sou eu."
   ↓
3. LLM recebe contexto:
   - Histórico da conversa
   - Prompt do agente atual
   - Estado atual (esperando confirmação de nome)
   - Ferramentas disponíveis
   ↓
4. LLM decide:
   - Ação: Chamar tool "user_first_name_confirmed(True)"
   - Resposta: "Perfeito! Vou só confirmar sua identidade..."
   ↓
5. Sistema executa ação e fala resposta
```

**Controles de Qualidade:**
- Palavras proibidas: "débito", "dívida" (usa "conta em aberto")
- Tom: Sempre respeitoso e empático
- Validações: Checa se ofertas são válidas antes de apresentar

---

### 4. Síntese de Voz Natural

**ElevenLabs TTS:**
- Voz feminina profissional
- Configurações otimizadas:
  - Estabilidade: 1.0 (voz consistente)
  - Velocidade: 1.1x (10% mais rápido)
  - Similarity boost: 0.75
  - Speaker boost: Ativado

**Efeitos Sonoros:**
- 🎵 Música de espera durante carregamento
- ⌨️ Som de digitação ao registrar informações
- 🏢 Som ambiente de escritório (opcional)

**Personalização:**
- Saudações baseadas no horário (Bom dia/Boa tarde/Boa noite)
- Uso do primeiro nome do cliente
- Tom adaptado à situação (empático, profissional, etc.)

---

### 5. Gestão de Estado

**Estado Persistente:**
Cada chamada tem um estado que é salvo continuamente:

```json
{
  "call_id": "123456",
  "customer_name": "João Silva",
  "cpf_validated": true,
  "current_agent": "NegotiatorAgent",
  "debt_total": 150.00,
  "offers_presented": [1, 2],
  "accepted_offer": 1,
  "payment_date": "2025-01-20",
  "status_code": "Robô-ACORDO A VISTA",
  "conversation_history": [...]
}
```

**Benefícios:**
- ✅ Recovery automático em caso de falha
- ✅ Auditoria completa de cada chamada
- ✅ Análise de performance dos agentes
- ✅ Debugging facilitado

---

### 6. Integração com CRM (Nectar)

**3 Funções Serverless (OCI Functions):**

**1. Import Data (Início da Chamada)**
```
Input: ID do cliente
Output: Contratos, dívidas, ofertas, histórico
Timeout: 120 segundos
```

**2. Save Summary (Durante a Chamada)**
```
Input: Código de status, resumo da conversa
Output: Confirmação de registro
```

**3. Save Negotiation (Acordo Fechado)**
```
Input: ID da oferta, data de pagamento, valor
Output: Confirmação de acordo
```

**Características:**
- Execução assíncrona (não bloqueia a conversa)
- Retry automático em caso de falha
- Logging completo de todas as chamadas

---

### 7. Sistema de Códigos de Status

**17 Códigos Diferentes:**

✅ **Positivos:**
- `Robô-ACORDO A VISTA`
- `Robô-ACORDO PARCELADO`

⚠️ **Neutros:**
- `Robô-ALEGA PAGAMENTO MAIOR QUE 2 DIAS`
- `Robô-ALEGA PAGAMENTO MENOR QUE 2 DIAS`
- `Robô-NAO PODE PAGAR`
- `Robô-DESLIGOU RETORNO LIGACAO`

❌ **Negativos:**
- `Robô-CLIENTE DESLIGOU - SEM PID`
- `Robô-CLIENTE DESLIGOU - COM PID`
- `Robô-FALECIDO`
- `Robô-DESCONHECE DIVIDA`

🔧 **Técnicos:**
- `Robô-SEM INTERACAO - SEM PID`
- `Robô-ERRO WS`
- `Robô-AMBIENTE RUIDOSO`

---

### 8. Logging e Observabilidade

**3 Níveis de Log:**

**1. INFO** - Eventos principais
```
[2025-01-15 14:30:22] Cliente confirmou nome
[2025-01-15 14:30:45] CPF validado com sucesso
[2025-01-15 14:31:30] Oferta #1 apresentada
[2025-01-15 14:32:10] Acordo aceito
```

**2. DEBUG** - Transcrições completas
```
[2025-01-15 14:30:22] STT: "Sim, sou eu."
[2025-01-15 14:30:23] LLM Response: "Perfeito! Para..."
[2025-01-15 14:30:25] TTS: Playing audio (3.2s)
```

**3. ERROR** - Erros e exceções
```
[2025-01-15 14:30:50] ERROR: Nectar timeout após 120s
[2025-01-15 14:30:51] RECOVERY: Usando dados do cache
```

**Armazenamento:**
- Local: `.logs/{call_id}/` durante a chamada
- OCI Object Storage: Upload ao final para persistência

**Arquivos Gerados:**
- `original.wav` - Gravação completa da chamada
- `output.log` - Logs estruturados
- `state.json` - Estado final completo

---

## 🔒 Segurança e Compliance

### Proteção de Dados

**Validação de Identidade em 2 Etapas:**
1. Confirmação de nome
2. Validação de CPF (primeiros 3 dígitos)

**Máximo de 3 Tentativas:**
- Previne tentativas de fraude
- Protege dados do cliente
- Registra todas as tentativas

**Criptografia:**
- Dados em trânsito: TLS/SSL
- Dados em repouso: Oracle Database (criptografado)
- Gravações: OCI Object Storage (privado)

### Auditoria

**Rastreabilidade Completa:**
- Cada decisão registrada
- Timestamp de todas as ações
- Estado completo salvo no banco

**Logs Estruturados:**
- Formato JSON para análise automatizada
- Searchable e indexável
- Permite investigação de incidentes

**Códigos de Status:**
- Tabulação automática de resultados
- Facilita análise de performance
- Identifica padrões de comportamento

---

## 🎬 Conclusão

O **Papel Agent VIVO** representa a evolução do atendimento automatizado, combinando:

✅ **Inteligência Artificial avançada** para conversas naturais
✅ **Arquitetura multi-agente** para especialização
✅ **Stack Oracle completo** para segurança e compliance
✅ **Experiência humanizada** mantendo eficiência operacional
✅ **Observabilidade total** para melhoria contínua

---




**Documentação feita por: Quézia Demetrio**
<img width="620" height="220" alt="Quezia-Demétrio (1)" src="https://github.com/user-attachments/assets/90b14a51-5aad-4786-b8ff-6fea33e2fa7a" />


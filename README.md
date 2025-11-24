# Vivo Oracle - Agente de Cobrança com IA

Sistema inteligente de cobrança automatizada por telefone utilizando IA para negociação de dívidas.

## 📋 Sobre o Projeto

Agente de voz baseado em IA que realiza ligações SIP para clientes com débitos, valida identidade e negocia acordos de pagamento de forma automatizada.

## 🏗️ Arquitetura

- **Framework:** LiveKit Agents
- **LLM:** OCI Generative AI (Cohere)
- **STT:** FasterWhisper / OCI Speech-to-Text
- **TTS:** ElevenLabs
- **Banco de Dados:** Oracle Autonomous Database
- **Cloud:** Oracle Cloud Infrastructure (OCI)

## 📂 Estrutura

```
livekit-worker-current-prod/
├── agent.py              # Agente principal
├── main.py              # Entrypoint
├── agents/              # Agentes multi-estágio
│   ├── name_validation/
│   ├── pid_validation/
│   └── negotiation/
├── oracle/              # Integrações OCI
└── common/              # Banco de dados e utils
```

## 🔄 Fluxo

1. **Name Validation:** Confirma identidade do titular
2. **PID Validation:** Valida CPF (3 primeiros dígitos)
3. **Negotiation:** Apresenta ofertas e negocia pagamento

## 📚 Documentação

- [`ANALISE_TECNICA_ARQUITETURA.md`](ANALISE_TECNICA_ARQUITETURA.md) - Análise técnica completa da arquitetura
- [`hardcoded_messages.md`](hardcoded_messages.md) - Documentação de todas as mensagens do sistema

## 🚀 Tecnologias

Python 3.x | LiveKit | OCI | Oracle DB | ElevenLabs | FastAPI

# Vivo Oracle - Documentação Técnica

Documentação técnica sobre a arquitetura do agente de cobrança com IA da Vivo.

## 📋 Sobre

Este repositório contém a documentação técnica detalhada do **sistema de cobrança automatizada por telefone** que utiliza IA para negociar dívidas. O agente realiza ligações SIP, valida identidade e negocia acordos de pagamento de forma automatizada.

## 🏗️ Arquitetura

## 🏗️ Arquitetura

- **Framework:** LiveKit Agents
- **LLM:** OCI Generative AI (Cohere)
- **STT:** FasterWhisper / OCI Speech-to-Text
- **TTS:** ElevenLabs
- **Banco de Dados:** Oracle Autonomous Database
- **Cloud:** Oracle Cloud Infrastructure (OCI)

## 📚 Documentação

- [`ANALISE_TECNICA_ARQUITETURA.md`](ANALISE_TECNICA_ARQUITETURA.md) - Análise técnica completa da arquitetura
- [`hardcoded_messages.md`](hardcoded_messages.md) - Documentação de todas as mensagens do sistema

## 🚀 Tecnologias

Python 3.x | LiveKit | OCI | Oracle DB | ElevenLabs | FastAPI

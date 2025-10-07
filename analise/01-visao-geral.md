# Visão Geral - Projeto Timelinize

## O que é o Timelinize?

O **Timelinize** é uma aplicação em Go criada por Matthew Holt (criador do Caddy Server) que permite aos usuários **organizarem toda sua vida digital em uma timeline unificada**. O projeto existe desde 2013 e evoluiu para se tornar uma solução completa para libertar e organizar dados pessoais.

### Propósito Principal

> *"Organize your photos & videos, chats & messages, location history, social media content, contacts, and more into a single cohesive timeline on your own computer where you can keep them alive forever."*

O projeto resolve dois problemas fundamentais:
1. **Conexão familiar**: Criar registros detalhados da vida para futuras gerações
2. **Soberania de dados**: Libertar dados pessoais de corporações e manter controle local

## Funcionalidades Principais

### 📥 Importação Universal
- Suporte a **22+ fontes de dados** diferentes
- Importação de arquivos nativos sem necessidade de extração manual
- Reconhecimento automático de formatos de dados
- Processamento de archives (ZIP, TAR, etc.)

### 🗃️ Armazenamento Inteligente
- Database SQLite como base de dados principal
- Arquivos organizados por data no sistema de arquivos
- Sistema de thumbnails separado
- Detecção de duplicatas por hash

### 🔍 Visualização Avançada
- Interface web moderna com múltiplas projeções dos dados
- Mapas 3D mostrando localização e tempo
- Agregação de conversas entre diferentes plataformas
- Galeria unificada de fotos/vídeos de todas as fontes
- Sistema de entidades para pessoas, lugares e organizações

### 🤖 Recursos de IA
- Servidor Python integrado para recursos semânticos
- Embeddings vetoriais com SQLite-vec
- Processamento de linguagem natural
- Análise de conteúdo e relacionamentos

## Filosofia do Projeto

### Privacidade por Design
- **100% offline e local** - todos os dados ficam no computador do usuário
- **Sem cloud obrigatório** - funciona completamente offline
- **Licença AGPL** - garante que permanecerá open source
- **Sem telemetria** - zero coleta de dados

### Extensibilidade
- Arquitetura modular de datasources
- Sistema plugin-friendly
- API HTTP simétrica com CLI
- Estrutura preparada para machine learning

### Sustentabilidade
- Formatos abertos e não-proprietários
- Dados sempre acessíveis mesmo sem a aplicação
- Portabilidade completa dos repositórios
- Backup e migração simples

## Casos de Uso

### Para Indivíduos
- **Arquivo pessoal completo**: Todas as memórias digitais em um só lugar
- **Análise de vida**: Padrões, insights e retrospectivas
- **Backup consolidado**: Proteção contra perda de dados em serviços
- **Pesquisa unificada**: Encontrar qualquer coisa através de toda a vida digital

### Para Famílias
- **História familiar digital**: Registro detalhado para gerações futuras
- **Consolidação de memórias**: Fotos, mensagens e momentos de toda a família
- **Preservação de legado**: Manter vivas as memórias de parentes falecidos

### Para Pesquisadores
- **Análise de dados pessoais**: Estudos sobre comportamento digital
- **Desenvolvimento de ferramentas**: Base para novas funcionalidades de IA
- **Prova de conceito**: Demonstração de soberania de dados

---

**Próximo**: [Arquitetura de DataSources →](02-arquitetura-datasources.md)

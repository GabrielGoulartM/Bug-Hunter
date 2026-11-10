# 👾 Bug Hunter

Kanban web para dar visibilidade aos problemas reportados por clientes e já validados pelo time de Suporte, organizados por assunto e acompanhados até a resolução.

> Projeto pessoal, desenvolvido por **Gabriel da Maia** (time de Suporte).

---

## 📌 Sobre

O **Bug Hunter** nasceu para responder a uma pergunta simples: *"esse problema já é conhecido?"*

Os tickets que passam pela triagem do Suporte e são validados como falhas reais entram no board, agrupados por tema (Financeiro, Pedidos, Impressão, Integrações, etc.). Assim, qualquer pessoa autorizada consegue consultar o que está em análise sem precisar abrir um novo chamado.

## ✨ Funcionalidades

- **Board por assunto**: colunas temáticas com cor própria; o tema é detectado automaticamente a partir do título do ticket.
- **Busca rápida**: filtro por título ou ID.
- **Detalhes do ticket**: modal com informações do card.
- **Arrastar e soltar**: mover cards entre colunas (perfil com permissão de edição).
- **Importação via JSON**: com revisão antes de confirmar, evitando duplicidades.
  - ticket já no board → ignorado
  - ticket já arquivado que voltou → aviso para decisão manual
  - título idêntico a um problema antigo → aviso de possível recorrência
- **Tickets finalizados**: visão separada (fundo verde animado) com os problemas já resolvidos, mantendo busca e filtros por assunto; quem tem permissão de edição pode restaurar um ticket.
- **Última atualização**: rodapé com data e horário da última vez que o board foi alimentado.
- **Modal explicativo**: botão ao lado do título que apresenta o projeto e como usá-lo.
- **Controle de acesso**: login exclusivo com conta Google corporativa; permissões diferentes para quem consulta e para quem edita.

## 🧱 Stack

| Camada | Tecnologia |
|---|---|
| Front-end | HTML, CSS e JavaScript puro (sem build); biblioteca `supabase-js` com versão fixa, embutida no próprio `index.html` (sem CDN) |
| Backend / banco | [Supabase](https://supabase.com) (PostgreSQL + Auth + Realtime) |
| Hospedagem | GitHub Pages |

## 🏗️ Arquitetura

```
GitHub Pages (index.html) ──HTTPS──> Supabase (PostgreSQL + Auth + Realtime)
```

## 🚀 Como rodar o seu próprio

1. Crie um projeto gratuito no Supabase.
2. Crie as tabelas `tickets` e `history` (campos: `id`, `conv_id`, `title`, `theme`, `state`, `added_at`; em `history`, adicione também `removed_at`) e a tabela `meta` (`key`, `value`) usada para a data de última atualização.
3. Configure as políticas de acesso (RLS) e a autenticação de acordo com a sua necessidade.
4. No `index.html`, ajuste as constantes de configuração:
   ```js
   const SUPABASE_URL = 'https://SEU_PROJETO.supabase.co';
   const SUPABASE_KEY = 'SUA_CHAVE_ANON_PUBLIC';
   const ALLOWED_DOMAIN = 'suaempresa.com'; // vazio = qualquer conta autenticada
   ```
5. Publique no GitHub Pages: **Settings → Pages → Branch: main → Save**.

## 📥 Formato do JSON de importação

```json
{
  "tickets": [
    {
      "ticket_id": "100001",
      "conv_id": "abc123",
      "title": "[Pedidos] Descrição do problema",
      "state": "open"
    }
  ]
}
```

O tema é identificado pelo prefixo entre colchetes no título (ex.: `[Impressão]`). Por privacidade, apenas os campos acima são lidos; qualquer outro campo do JSON (como responsável ou resumo da conversa) é ignorado.

## 🗺️ Ideias futuras

- Atualização automática, sem importação manual
- Filtro por tema direto na barra superior
- Métricas de tempo médio de resolução

## 👤 Autor

**Gabriel da Maia**, Suporte.
Sugestões e feedbacks são bem-vindos! Abra uma *issue* ou entre em contato.

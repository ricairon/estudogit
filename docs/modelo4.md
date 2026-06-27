## 🧩 RF-210 - Dashboard Administrativo

**Prioridade:** Média-Alta | **Status:** 🟡 Em Revisão

### Estrutura do Requisito
1. **Gráficos:** Exibir 3 widgets principais (Vendas, Acessos, Ticket Médio).
2. **Filtros:** Permitir filtragem por data (hoje, semana, mês).
3. **Exportação:** Botão de exportar dados brutos em CSV.

---

#### 📋 Sub-requisitos Técnicos
- **RF-210.1:** Os gráficos devem ser renderizados via Chart.js.
- **RF-210.2:** A atualização dos dados deve ser via WebSocket (tempo real).

#### ❗ Condições de Contorno (Edge Cases)
- Se não houver dados no período, exibir uma mensagem amigável e um gráfico vazio.
- O dashboard deve ser responsivo (se adaptar a telas de 1024px).
## 📌 RF-027: Exportação de Relatório Financeiro

| Propriedade | Valor |
| :--- | :--- |
| **Prioridade** | <kbd>🔴 Alta</kbd> |
| **Complexidade** | ⭐⭐⭐ (Média) |
| **Autor** | @joao.silva |
| **Versão** | v2.1 |
| **Status** | ✅ Aprovado |

---

**📝 Descrição**  
O sistema deve gerar um relatório financeiro consolidado em formato `.xlsx`, contendo as colunas: Data, Cliente, Valor Bruto, Impostos e Valor Líquido.

**🎯 Critérios de Aceitação**  
- [ ] O arquivo deve ser baixado em menos de 3 segundos.  
- [ ] Os totais devem ser calculados com 2 casas decimais.  
- [ ] Células vazias devem ser preenchidas com "N/A".  

**🔗 Dependências**  
- RF-015 (Cadastro de Clientes)  
- RNF-009 (Biblioteca Apache POI)  
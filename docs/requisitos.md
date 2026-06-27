# Requisitos do Sistema – [Nome do Sistema]

## 1. Introdução
### 1.1 Propósito
Este documento especifica os requisitos do sistema [nome], descrevendo as funcionalidades e restrições.

### 1.2 Escopo
O sistema será utilizado por [usuários] para [objetivo principal].

### 1.3 Referências
- Documento de Visão (link)
- Padrões internos da empresa

---

## 2. Requisitos Funcionais (RF)
Os requisitos são identificados por **RF-XXX**.

| ID     | Descrição                                                                 | Prioridade |
|--------|---------------------------------------------------------------------------|------------|
| RF-001 | O sistema deve permitir que o usuário se autentique com e-mail e senha.   | Alta       |
| RF-002 | O sistema deve emitir um relatório de vendas em formato PDF.              | Média      |
| RF-003 | ...                                                                       | ...        |

---

## 3. Requisitos Não Funcionais (RNF)

| ID      | Descrição                                                                 | Categoria      |
|---------|---------------------------------------------------------------------------|----------------|
| RNF-001 | O tempo de resposta para consultas deve ser inferior a 2 segundos.        | Desempenho     |
| RNF-002 | O sistema deve estar disponível 99,9% do tempo em horário comercial.      | Disponibilidade|
| RNF-003 | Deve ser desenvolvido em Java 17 com Spring Boot.                         | Tecnologia     |
| RNF-004 | Deve seguir as diretrizes de acessibilidade WCAG 2.1 nível AA.            | Usabilidade    |

---

## 4. Restrições e Suposições
- **Restrições**: O sistema deve ser compatível com navegadores Chrome e Firefox.
- **Suposições**: Os usuários possuem treinamento básico em sistemas similares.

---

## 5. Anexos
- Diagrama de casos de uso (em `casos-de-uso.md`)
- Glossário (em `glossario.md`)
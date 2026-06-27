# Requisito Funcional — Integração para Envio de Procedimento por JSON

## Objetivo

Disponibilizar um contrato REST para recepção de procedimentos por JSON, permitindo o envio estruturado de:

- Boletim de Ocorrência (BO)
- Procedimentos derivados
- Atendimentos operacionais do Corpo de Bombeiros

---

## Endpoint

### Receber procedimento

`POST /api/v1/procedimentos`

### Content-Type

```http
application/json
```

---

## Estrutura Geral

```text
cabecalho
procedimento
 ├── dadosRegistro
 ├── pessoas[]
 ├── objetos[]
 ├── naturezas[]
 ├── vinculosEntrePessoas[]
 ├── vinculosPessoaObjeto[]
 ├── orgaoApoio
 └── atendimentosOperacionais[]
```

---

## Identificação do envio

Campos mínimos:

- tipoProcedimento
- instituicaoEnvio
- unidadeEnvio
- codigoMunicipioEnvio
- ufEnvio

Exemplo:

```json
{
  "identificacaoEnvio": {
    "tipoProcedimento": "BO",
    "instituicaoEnvio": "PMCE-1BRM",
    "codigoMunicipioEnvio": "2304103",
    "ufEnvio": "CE"
  }
}
```

---

## Registro

Deve permitir:

- número local;
- histórico;
- origem do procedimento;
- responsáveis;
- controle de sigilo e situação.

Regras:

- `adendo` deve ser boolean.
- `sigiloso` deve ser boolean.
- `dataHoraRegistro` deve representar data e hora completas.

Exemplo:

```json
{
  "controleRegistro": {
    "adendo": false,
    "sigiloso": false,
    "situacao": "ATIVO"
  }
}
```

---

## Pessoas

Cada pessoa deve possuir identificador único.

Exemplo:

```json
{
  "idPessoa": "PESSOA1",
  "codigoTipoPessoa": "FIS"
}
```

Podem existir:

- documentos;
- endereços;
- contatos;
- profissões;
- filhos;
- características físicas.

---

## Objetos

Objetos devem possuir classificação e dados específicos.

Exemplo:

```json
{
  "idObjeto": "OBJ2",

  "classificacao": {
    "codigoTipoObjeto": "3",
    "codigoGrupoObjeto": "2",
    "codigoSubgrupoObjeto": "7"
  },

  "dadosEspecificos": {}
}
```

---

## Naturezas

Permitir múltiplas naturezas por procedimento.

Cada natureza poderá possuir:

- meios empregados;
- envolvimentos;
- evento relacionado.

---

## Relacionamentos

### Pessoa → Pessoa

Exemplo:

```json
{
  "idPessoaOrigem": "PESSOA1",
  "idPessoaDestino": "PESSOA2",
  "codigoTipoRelacao": "TIO"
}
```

### Pessoa → Objeto

Relacionamento desacoplado da pessoa.

---

## Atendimentos Operacionais (CBM)

Permitir múltiplos atendimentos.

Exemplo:

```json
{
  "codigoTipoAtendimentoCBM": "IVE",

  "dados": {}
}
```

Tipos suportados:

- Incêndio Vegetação
- Incêndio Edificação
- Incêndio Meio de Transporte
- Busca e Salvamento
- Atendimento Pré-Hospitalar
- Produto Perigoso
- Atendimento Comunitário
- Vistoria
- Desastre

---

## Regras Técnicas

1. JSON UTF-8.
2. Arrays devem representar coleções.
3. Valores monetários devem ser numéricos.
4. Evitar `null` quando campo opcional puder ser omitido.
5. Identificadores internos devem ser reutilizados para vínculos.

---

## Critérios de Aceitação

- Receber JSON válido.
- Validar relacionamentos.
- Persistir histórico do procedimento.
- Permitir evolução sem quebra de contrato.

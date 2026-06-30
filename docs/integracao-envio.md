## 📌 HU - Enviar Boletim de Ocorrência - BO Único : Integração com Sistema de Procedimentos

### Controle de Versões

| Número Demanda | Nome Demanda | Versão | Data | Tipo de Alteração | Descrição da Modificação |Autor |
| :---:| :---:| :---: | :---: | :---: | :--- | :--- |
| 49494 | Sinesp Integração | 1.0 | 29/06/2026 | Criação | Criação inicial do requisito |  Ricardo Lopes de Lima |

---

**Narrativa**\
**Como** Usuário do Sistema SINESP - INTEGRAÇÃO\
**Eu quero** enviar um Boletim de Ocorrência para a base de dados do Sistema de Integração Procedimentos.
**Para** que os dados e informações sejam armazenados, tratados e integrados para o acompanhamento, execução e implementação de políticas relacionadas com segurança pública.

**✅ Critérios de Aceitação:**
O usuário do Sistema SINESP - INTEGRAÇÃO deve obter a credencial de acesso fornecida por meio do serviço customizável de acesso no Barramento:
[936044: HU - Serviço Customizável para Envio de Procedimento](https://alm.serpro/rm/resources/TX_RXqlIGfBEfCirqqtwbaCiw) **HU - Serviço Customizável para Envio de Procedimento**

[RF-002](./RF-002.md)

**Narrativa**
**Como** Gestor do Sistema SINESP - INTEGRAÇÃO
**Eu quero** informar parâmetros para o recebimento de procedimentos através de um serviço customizável no Sinesp Barramento.
**Para** fazer o controle de quais usuários podem enviar os procedimentos de forma personalizada.

**✅ Critérios de Aceitação:**
O serviço customizável de recebimento de procedimentos, sendo utilizado para permitir a gestão da permissão de envio pelo cliente através do Gerenciador de APIs.
Os parâmetros que serão customizáveis para o serviço de receber procedimentos estão definidos na regra:
[936045: RN - Campos de Personalização de Acesso Envio Procedimento](https://alm.serpro/rm/resources/TX_vjWgoGfBEfCirqqtwbaCiw)

| **Campo** | **Descrição** | **Tipo** |
|---|---|---|
| ID Unidade de Registro | Identificador da Unidade de Registro | Texto |
| ID Esfera | Esfera do Registro | Lista |
| ID UF de Registro | UF de Registro do Procedimento. | Lista |
| ID Município | Município de Registro | Lista |
| ID Instituição | Instituição de Registro | Lista |
| Tipos de Procedimento | BO, AIAI, IP, TCO, BOC... | Lista |

Após selecionado os parâmetros, o gestor deve acionar o botão de salvar credencial.
Tal credencial só dará direito ao envio da instituição enviar procedimentos dos quais essa foi selecionada para enviar, limitando por instituição, procedimento e município.
Se o campo "ID Unidade de Registro" ficar em branco, deve-se retornar a seguinte mensagem ao usuário: [936050: MSG - ID Unidade não pode ficar em branco](https://alm.serpro/rm/resources/TX_bKEL8GfEEfCirqqtwbaCiw) O campo "ID Unidade de Registro" não pode ficar vazio.
Se alguma lista deixar de ser preenchida, deve-se retornar a seguinte mensagem ao usuário: [936052: MSG - Lista não preenchida](https://alm.serpro/rm/resources/TX_gfPEYGfFEfCirqqtwbaCiw) O campo <nome_do_campo> não foi preenchido. Preencha para continuar.
Na customização de parâmetros de envio será possível definir valores para os filtros UF de Registro, ID Esfera, ID Instituição e ID Município para permitir o controle de quais procedimentos cada consumidor pode enviar para o Sinesp Integração.
Caso o consumidor informe valores diferentes dos valores que foram pré-definidos, o sistema deve recusar o envio com a seguinte mensagem: 
[936046: MSG - Valores Diferentes](https://alm.serpro/rm/resources/TX_KLxwwGfCEfCirqqtwbaCiw) Não foi possível definir a concessão de acesso. Favor revisar os campos ID Unidade de Registro, ID Esfera, ID UF de Registro, ID Instituição, ID Município e Tipo de Procedimento conforme o Guia de Integração seção 1.1 Gerenciamento de Acessos.
Após obter a autenticação necessária, o usuário deve enviar um Boletim de Ocorrência (B.O) por meio da seguinte estrutura:

## Endpoint

### Receber procedimento

<span style="color:yellow">POST</span> `/api/v1/procedimentos`

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

**Regras Gerais**
1) Se um campo for enviado com a tipagem inválida, o procedimento deve ser recusado com o seguinte motivo de recusa:
[938521: MR01 - Tipagem Inválida](https://alm.serpro/rm/resources/TX_0VNTgHLtEfCXc_rffzpa6A) Se o tipo de dado não for a tipagem esperada:

-   Recusar o B.O
-   Adicionar ao motivo da recusa a seguinte mensagem: [936033: MSG - Tipagem Inválida](https://alm.serpro/rm/resources/TX_CrqrwGe2EfCirqqtwbaCiw) `<nome_campo>`: Tipagem Inválida

2) Se um campo for enviado com tamanho máximo excedido, o procedimento deve ser recusado com o seguinte motivo de recusa:
938524: MR02 - Tamanho Máximo Excedido Se o tipo tamanho máximo do campo exceder:

-   Recusar o B.O
-   Adicionar ao motivo da recusa a seguinte mensagem: 936037: MSG - Tamanho Máximo Excedido `<nome_campo>`: Tamanho máximo do campo excedido.

3) Se um campo obrigatório não for enviado, o procedimento deve ser recusado com o seguinte motivo de recusa:
938529: MR03 - Campo Obrigatório Se o campo obrigatório não for enviado:

-   Recusar o B.O
-   Adicionar ao motivo da recusa a seguinte mensagem: 936218: MSG - Campo obrigatório não informado `<nome_campo>`: Campo obrigatório não informado.

4) Se um campo for enviado com um dado de domínio diferente do esperado, o procedimento deve ser recusado com o seguinte motivo de recusa:
939753: MR04 - Dado de Domínio Inválido Se o tipo de dado de domínio não estiver na faixa prevista e esse dado não estiver ativo:

-   Recusar o B.O
-   Adicionar ao motivo da recusa a seguinte mensagem: 936502: MSG - Valor de Dado Corporativo Inválido `<nome_campo>`: Valor de Dado Corporativo Inválido

5) Se um campo for enviado com uma formatação inválida, o procedimento deve ser recusado com o seguinte motivo de recusa:
941042: MR05 - Formatação Inválida Se o tipo de dado não tiver a formatação esperada:

-   Recusar o B.O
-   Adicionar ao motivo da recusa a seguinte mensagem: 941041: MSG - Formatação Inválida `<nome_campo>`: Formatação Inválida.

6) São permitidos, no máximo, o recebimento de 999 adendos para cada B.O. Caso seja enviado mais de 999 adendos para um procedimento:
948359: MR06 - Adendos Atingiu Limite Se o procedimento ultrapassar o limite de 999 adendos:

-   Recusar o B.O
-   Adicionar ao motivo da recusa a seguinte mensagem: 948360: MSG - Número de adendos ultrapassou o limite (999) Número de adendos ultrapassou o limite (999).

**Campos Mínimos para recebimento do Boletim de Ocorrência**
Para que um Boletim de Ocorrência seja enviado, ele deve possuir no mínimo os seguintes campos de envio:

934259: RN - Campos Mínimos do B.O - Recebimento **Regras para os campos mínimos**
Se pelo menos um campo da tabela abaixo dos dados mínimos do B.O não for enviado:

-   Será recusado o procedimento.
-   Será retornada seguinte mensagem de recusa: 936218: MSG - Campo obrigatório não informado `<nome_campo>`: Campo obrigatório não informado.

**Tabela de Regras de Campos Mínimos**

| **Nome do Campo** | **Descrição** | **Tipo** | **Obrigatório** | **Tamanho** | **Regras de Recebimento** |
|:---|:---|:---:|:---:|:---|:---|
| Unidade de Registro | Identificador da Unidade de Registro do B.O. | Texto | Sim | 100 | - A Unidade de Registro do procedimento deve pertencer a Instituição que está enviando.<br>- A Unidade de Registro deve estar cadastrada na SENASP.<br>- A Unidade de Registro deve estar ativa na data/hora do registro do procedimento. |
| Código Esfera | Identificador da Esfera da Unidade de Registro | Texto | Sim | 6 | - Quando a esfera da unidade de registro for igual a "Federal", o campo Código Instituição da Esfera deve corresponder a uma instituição "Federal".<br>- Quando a esfera da unidade de registro for igual a "Estadual", o campo Código Instituição da Esfera deve corresponder a uma instituição "Estadual".<br>- Quando a esfera da unidade de registro for igual a "Municipal", o campo Código Instituição da Esfera deve corresponder a uma instituição "Municipal".<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **geral-v3**: "Listar Esferas". |
| Código Instituição | Identificador da Instituição | Texto | Sim | 6 | - Quando a esfera da unidade de registro for igual a "Federal", a instituição informada deve pertencer a uma esfera federal.<br>- Quando a esfera da unidade de registro for igual a "Estadual", a instituição informada deve pertencer a uma esfera estadual.<br>- Quando a Esfera da Unidade de Registro for "Municipal", a Instituição informada deve pertencer a uma esfera municipal.<br>- MR06 - Instituição Inválida<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-procedimento-v3**: "Listar Instituições". |
| Código Município Registro | Identificador do Registro do Município | Texto | Sim | 11 | - O Código do Município deve estar dentro dos municípios de autorização para envio do procedimento.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios". |
| Código UF de Registro | Identificador da UF | Texto | Sim | 2 | - A UF de registro deve estar dentro das autorizações concedidas para envio.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar UFs". |
| **Campos do Procedimento** | | | | | |
| Código do Tipo de Procedimento | Código do Tipo de Procedimento Enviado | Texto | Sim | 6 | - Código do Procedimento tem que ser válido com a estrutura enviada.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-procedimento-v3**: "Listar Tipos de Procedimento Policial". |
| Número de Registro Local do B.O | Número de Registro Local do Boletim de Ocorrência | Texto | Sim | 40 | |
| Data Hora Registro | Data e Hora do Registro do Boletim de Ocorrência | Texto (Data e Hora) | Sim | 20 | - Formato esperado: dd/MM/aaaa HH:mm:ss<br>- RN - A data e hora não pode ser posterior a data de envio ao sistema.<br>Caso o campo "Data Hora Registro" seja posterior a data de envio ao sistema: Recusar o B.O. Adicionar ao motivo da recusa a seguinte mensagem: MSG - A data e hora não pode ser posterior a data de envio ao sistema. A data e hora não pode ser posterior a data de envio ao sistema.<br>- Será considerado o Horário Local do Município de Registro do B.O. |
| Relato Histórico | Relato histórico da ocorrência | Texto | Sim | - | |
| **Dados da Ocorrência** | | | | | |
| Data Início da Ocorrência | Data de Início da Ocorrência | Lista Circunstância do Fato - Data | Sim | 10 | - Formato esperado: dd/MM/aaaa<br>- Data Início da Ocorrência não pode ser posterior à data atual ou à data de registro do B.O. |
| Hora de Início Ocorrência | Hora de  | Lista    | Sim      | 8        | -        |
|    | Início   | Circu    |          |          |  Formato |
|        | da       | nstância |          |          |     e    |
|        | Oc       | do Fato  |          |          | sperado: |
|  | orrência | - Hora   |          |          |          |
|          |          |          |          |          |    HH:MM |
+----------+----------+----------+----------+----------+----------+
| Código   | País da  | Lista    | Sim      | 6        | -        |
| País de  | oc       | Circu    |          |          |  Valores |
| Oc       | orrência | nstância |          |          |     fo   |
| orrência |          | do Fato  |          |          | rnecidos |
| do B.O   |          | -        |          |          |          |
|          |          | Numérico |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     P    |
|          |          |          |          |          | aíses\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Unidade  | Lista    | \*(1)    | 2        | -        |
| UF de    | Fe       | Circu    | Sim      |          |  Valores |
| Oc       | derativa | nstância |          |          |     fo   |
| orrência | de onde  | do Fato  |          |          | rnecidos |
| do B.O   | ocorreu. | - Texto  |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   UFs\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |    \*(1) |
|          |          |          |          |          |          |
|          |          |          |          |          |    Campo |
|          |          |          |          |          |          |
|          |          |          |          |          |    deixa |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     ser  |
|          |          |          |          |          |     obr  |
|          |          |          |          |          | igatório |
|          |          |          |          |          |     se   |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Código |
|          |          |          |          |          |     País |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Oc   |
|          |          |          |          |          | orrência |
|          |          |          |          |          |     do   |
|          |          |          |          |          |     B    |
|          |          |          |          |          | .O\" não |
|          |          |          |          |          |     for  |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |  Brasil. |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Lista    | \*(2)    | 11       | -        |
| M        | do       | Circu    | Sim      |          |  Valores |
| unicípio | m        | nstância |          |          |     fo   |
| Oc       | unicípio | do Fato  |          |          | rnecidos |
| orrência | de       | -Texto   |          |          |          |
|          | oc       |          |          |          |    pelos |
|          | orrência |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     - ** |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Munic |
|          |          |          |          |          | ípios\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |    \*(2) |
|          |          |          |          |          |          |
|          |          |          |          |          |    Campo |
|          |          |          |          |          |          |
|          |          |          |          |          |    deixa |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     ser  |
|          |          |          |          |          |     obr  |
|          |          |          |          |          | igatório |
|          |          |          |          |          |     se   |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Código |
|          |          |          |          |          |     País |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Oc   |
|          |          |          |          |          | orrência |
|          |          |          |          |          |     do   |
|          |          |          |          |          |     B    |
|          |          |          |          |          | .O\" não |
|          |          |          |          |          |     for  |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |  Brasil. |
+----------+----------+----------+----------+----------+----------+
| Lo       | Lo       | Lista    | Sim      | 200      |          |
| gradouro | gradouro | Circu    |          |          |          |
| da       | da       | nstância |          |          |          |
| Oc       | oc       | do Fato  |          |          |          |
| orrência | orrência | -Texto   |          |          |          |
+----------+----------+----------+----------+----------+----------+
| P        | P        | Lista    | \*(3)    | 200      | -        |
| rovíncia | rovíncia | Circu    | Não      |          |    \*(3) |
| da       | da       | nstância |          |          |     Se o |
| Oc       | Oc       | do Fato  |          |          |     país |
| orrência | orrência | - Texto  |          |          |     da   |
|          |          |          |          |          |     oc   |
|          |          |          |          |          | orrência |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     for  |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |  Brasil, |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    campo |
|          |          |          |          |          |     P    |
|          |          |          |          |          | rovíncia |
|          |          |          |          |          |     M    |
|          |          |          |          |          | unicípio |
|          |          |          |          |          |     da   |
|          |          |          |          |          |     Oc   |
|          |          |          |          |          | orrência |
|          |          |          |          |          |          |
|          |          |          |          |          |   tornar |
|          |          |          |          |          | -se obri |
|          |          |          |          |          | gatório. |
|          |          |          |          |          |          |
|          |          |          |          |          | -   Caso |
|          |          |          |          |          |     o    |
|          |          |          |          |          |     país |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     oc   |
|          |          |          |          |          | orrência |
|          |          |          |          |          |     seja |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |  Brasil, |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    campo |
|          |          |          |          |          |     P    |
|          |          |          |          |          | rovíncia |
|          |          |          |          |          |     da   |
|          |          |          |          |          |     Oc   |
|          |          |          |          |          | orrência |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     deve |
|          |          |          |          |          |     ser  |
|          |          |          |          |          |     pr   |
|          |          |          |          |          | eenchido |
+----------+----------+----------+----------+----------+----------+
| Natureza | Natureza | Lista    | Sim      | \-       | -   É    |
| da       | da       | N        |          |          |     obr  |
| Oc       | oc       | aturezas |          |          | igatório |
| orrência | orrência | da       |          |          |          |
|          |          | Oc       |          |          | informar |
|          |          | orrência |          |          |     pelo |
|          |          |          |          |          |          |
|          |          |          |          |          |    menos |
|          |          |          |          |          |     uma  |
|          |          |          |          |          |          |
|          |          |          |          |          | natureza |
|          |          |          |          |          |     da   |
|          |          |          |          |          |     oc   |
|          |          |          |          |          | orrência |
|          |          |          |          |          |          |
|          |          |          |          |          | -   Não  |
|          |          |          |          |          |     é    |
|          |          |          |          |          |     p    |
|          |          |          |          |          | ermitido |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          | cadastro |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     N    |
|          |          |          |          |          | aturezas |
|          |          |          |          |          |     da   |
|          |          |          |          |          |     Oc   |
|          |          |          |          |          | orrência |
|          |          |          |          |          |     dup  |
|          |          |          |          |          | licadas. |
|          |          |          |          |          |          |
|          |          |          |          |          |    Serão |
|          |          |          |          |          |          |
|          |          |          |          |          |  descons |
|          |          |          |          |          | ideradas |
|          |          |          |          |          |     as   |
|          |          |          |          |          |     n    |
|          |          |          |          |          | aturezas |
|          |          |          |          |          |     que  |
|          |          |          |          |          |     p    |
|          |          |          |          |          | ossuírem |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    mesmo |
|          |          |          |          |          |          |
|          |          |          |          |          |   código |
|          |          |          |          |          |     e    |
|          |          |          |          |          |          |
|          |          |          |          |          |    mesmo |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     para |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    campo |
|          |          |          |          |          |          |
|          |          |          |          |          |   \"tent |
|          |          |          |          |          | ativa\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Lista    | Sim      | 10       | [[MR3 -  |
| Tipo de  | Cor      | Circu    |          |          | Campo    |
| Local    | porativo | nstância |          |          | Obr      |
|          | do tipo  | do Fato  |          |          | igatório |
|          | de local | -        |          |          | Não      |
|          |          | Numérico |          |          | In       |
|          |          |          |          |          | formado] |
|          |          |          |          |          | {.underl |
|          |          |          |          |          | ine}](ht |
|          |          |          |          |          | tps://al |
|          |          |          |          |          | m.serpro |
|          |          |          |          |          | /rm/web# |
|          |          |          |          |          | action=c |
|          |          |          |          |          | om.ibm.r |
|          |          |          |          |          | dm.web.p |
|          |          |          |          |          | ages.sho |
|          |          |          |          |          | wArtifac |
|          |          |          |          |          | tPage&ar |
|          |          |          |          |          | tifactUR |
|          |          |          |          |          | I=https% |
|          |          |          |          |          | 3A%2F%2F |
|          |          |          |          |          | alm.serp |
|          |          |          |          |          | ro%2Frm% |
|          |          |          |          |          | 2Fresour |
|          |          |          |          |          | ces%2FTX |
|          |          |          |          |          | _5eZI8HL |
|          |          |          |          |          | vEfCXc_r |
|          |          |          |          |          | ffzpa6A& |
|          |          |          |          |          | vvc.conf |
|          |          |          |          |          | iguratio |
|          |          |          |          |          | n=https% |
|          |          |          |          |          | 3A%2F%2F |
|          |          |          |          |          | alm.serp |
|          |          |          |          |          | ro%2Frm% |
|          |          |          |          |          | 2Fcm%2Fs |
|          |          |          |          |          | tream%2F |
|          |          |          |          |          | _kimOgAO |
|          |          |          |          |          | JEeiiCL9 |
|          |          |          |          |          | T4NHyOQ) |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |  Valores |
|          |          |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |          |
|          |          |          |          |          |    - **l |
|          |          |          |          |          | ocalidad |
|          |          |          |          |          | e-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Local\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Lista    | \*(2)    | 10       | [[MR3 -  |
| Subgrupo | do       | Circu    | Sim      |          | Campo    |
| Local    | subgrupo | nstância |          |          | Obr      |
|          | do local | do Fato  |          |          | igatório |
|          | de Dados | - Texto  |          |          | Não      |
|          | Corp     |          |          |          | Inf      |
|          | orativos |          |          |          | ormado]{ |
|          |          |          |          |          | .underli |
|          |          |          |          |          | ne}](htt |
|          |          |          |          |          | ps://alm |
|          |          |          |          |          | .serpro/ |
|          |          |          |          |          | rm/web#a |
|          |          |          |          |          | ction=co |
|          |          |          |          |          | m.ibm.rd |
|          |          |          |          |          | m.web.pa |
|          |          |          |          |          | ges.show |
|          |          |          |          |          | Artifact |
|          |          |          |          |          | Page&art |
|          |          |          |          |          | ifactURI |
|          |          |          |          |          | =https%3 |
|          |          |          |          |          | A%2F%2Fa |
|          |          |          |          |          | lm.serpr |
|          |          |          |          |          | o%2Frm%2 |
|          |          |          |          |          | Fresourc |
|          |          |          |          |          | es%2FTX_ |
|          |          |          |          |          | 5eZI8HLv |
|          |          |          |          |          | EfCXc_rf |
|          |          |          |          |          | fzpa6A&v |
|          |          |          |          |          | vc.confi |
|          |          |          |          |          | guration |
|          |          |          |          |          | =https%3 |
|          |          |          |          |          | A%2F%2Fa |
|          |          |          |          |          | lm.serpr |
|          |          |          |          |          | o%2Frm%2 |
|          |          |          |          |          | Fcm%2Fst |
|          |          |          |          |          | ream%2F_ |
|          |          |          |          |          | kimOgAOJ |
|          |          |          |          |          | EeiiCL9T |
|          |          |          |          |          | 4NHyOQ)\ |
|          |          |          |          |          | [[MR6 -  |
|          |          |          |          |          | Subgrupo |
|          |          |          |          |          | Local    |
|          |          |          |          |          | não      |
|          |          |          |          |          | condiz   |
|          |          |          |          |          | com Tipo |
|          |          |          |          |          | de       |
|          |          |          |          |          | Local]   |
|          |          |          |          |          | {.underl |
|          |          |          |          |          | ine}](ht |
|          |          |          |          |          | tps://al |
|          |          |          |          |          | m.serpro |
|          |          |          |          |          | /rm/web# |
|          |          |          |          |          | action=c |
|          |          |          |          |          | om.ibm.r |
|          |          |          |          |          | dm.web.p |
|          |          |          |          |          | ages.sho |
|          |          |          |          |          | wArtifac |
|          |          |          |          |          | tPage&ar |
|          |          |          |          |          | tifactUR |
|          |          |          |          |          | I=https% |
|          |          |          |          |          | 3A%2F%2F |
|          |          |          |          |          | alm.serp |
|          |          |          |          |          | ro%2Frm% |
|          |          |          |          |          | 2Fresour |
|          |          |          |          |          | ces%2FTX |
|          |          |          |          |          | _8wBU0HR |
|          |          |          |          |          | 7EfCXc_r |
|          |          |          |          |          | ffzpa6A& |
|          |          |          |          |          | vvc.conf |
|          |          |          |          |          | iguratio |
|          |          |          |          |          | n=https% |
|          |          |          |          |          | 3A%2F%2F |
|          |          |          |          |          | alm.serp |
|          |          |          |          |          | ro%2Frm% |
|          |          |          |          |          | 2Fcm%2Fs |
|          |          |          |          |          | tream%2F |
|          |          |          |          |          | _kimOgAO |
|          |          |          |          |          | JEeiiCL9 |
|          |          |          |          |          | T4NHyOQ) |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |    \*(2) |
|          |          |          |          |          |          |
|          |          |          |          |          |   Quando |
|          |          |          |          |          |     for  |
|          |          |          |          |          |     i    |
|          |          |          |          |          | nformado |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Código |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |          |
|          |          |          |          |          | Local\": |
|          |          |          |          |          |     \"   |
|          |          |          |          |          | AMBIENTE |
|          |          |          |          |          |          |
|          |          |          |          |          |  VIRTUAL |
|          |          |          |          |          |     (INT |
|          |          |          |          |          | ERNET)\" |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     será |
|          |          |          |          |          |     obr  |
|          |          |          |          |          | igatório |
|          |          |          |          |          |          |
|          |          |          |          |          | informar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |     s    |
|          |          |          |          |          | ubgrupo. |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |  Valores |
|          |          |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |          |
|          |          |          |          |          |    - **l |
|          |          |          |          |          | ocalidad |
|          |          |          |          |          | e-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     S    |
|          |          |          |          |          | ubgrupos |
|          |          |          |          |          |          |
|          |          |          |          |          | Local\". |
+----------+----------+----------+----------+----------+----------+
| *        |          |          |          |          |          |
| *Envolvi |          |          |          |          |          |
| mentos** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Envo     | Lista    | Vínculo  | Sim      | \-       |          |
| lvimento | com o    | Pessoa-  |          |          |          |
| Pessoa e | envo     | Natureza |          |          |          |
| Natureza | lvimento |          |          |          |          |
|          | de       |          |          |          |          |
|          | Pessoa e |          |          |          |          |
|          | Natureza |          |          |          |          |
+----------+----------+----------+----------+----------+----------+

**Dados do Registro:**\

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Código Tipo de Procedimento | Código do Tipo de Procedimento Enviado | Texto | Sim | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-procedimento-v3**: "Listar Tipos de Procedimento Policial".<br>- Para o Boletim de Ocorrência, deve-se ser enviado o valor: 'BO'. |
| Número do Registro Local | Número de Registro Local do B.O | Texto | Sim | 40 | |
| Data Hora Registro | Data e Hora do Registro Local do B.O | Texto (Data e Hora) | Sim | 20 | - Formato esperado: dd/MM/aaaa HH:mm:ss<br>- [949002: RN - A data e hora não pode ser posterior a data de envio ao sistema.](#)<br>- Será considerado o Horário Local do Município de Registro do B.O. |
| Código Município de Registro | Código do Município de Registro do B.O | Texto | Não | 11 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios". |
| Unidade de Registro | Unidade de Registro do B.O | Texto | Sim | 100 | - A Unidade de Registro do procedimento deve pertencer a Instituição que está enviando.<br>- A Unidade de Registro deve estar cadastrada na SENASP.<br>- A Unidade de Registro deve estar ativa na data/hora do registro do procedimento. |
| Unidade de Afeto | Unidade de Afeto | Texto | Não | 100 | - Se o campo Unidade de Afeto for informado em branco, o sistema copiará o valor informado do campo Unidade de Registro.<br>- A Unidade de Apuração não pode pertencer à uma UF diferente da unidade de registro.<br>- A Unidade de Apuração deve estar cadastrada junto a SENASP.<br>- A Unidade de Apuração deve estar ativa durante a Data e Hora do registro do B.O enviado. |
| Aditamento (Addendum) | Indicativo se o B.O enviado é um aditamento. | Booleano | Não | 5 | - Se o campo não for enviado ou enviado como null, considerar o valor padrão NÃO.<br>- Os adendos serão ordenados de maneira cronológica, baseado na "Data Hora Registro". Se possuírem a mesma Data de Registro, serão ordenados de acordo com a ordem de Recebimento do B.O. pelo sistema. |
| Relato Histórico | Relato histórico do acontecimento | Texto | Sim | - | - Campo não pode ser enviado em branco. |
| Código Motivação | Motivação | Texto | Não | 12 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-procedimento-v3**: "Listar Motivações". |
| Descrição Motivação | Descrição da Motivação da Natureza | Texto | Não | 70 | |
| Código Situação do B.O | Situação do Boletim de Ocorrência (Ativo ou Cancelado) | Texto | Não | 12 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-procedimento-v3**: "Listar Situações do Procedimento".<br>- Caso não seja enviado, considerar como valor padrão ATIVO. |
| Sigiloso | Indicativo se o procedimento é sigiloso ou não. | Booleano | Não | 5 | - Se o campo não for enviado ou enviado como null, considerar o valor padrão NÃO. |
| Código Órgão de Origem do B.O | Órgão que deu origem ao B.O | Texto | Não | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **geral-v3**: "Listar Orgãos". |
| Código Tipo de Documento de Origem | Tipo de Documento de origem do B.O | Texto | Não | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-procedimento-v3**: "Listar Tipos de Documento de Origem". |
| Número Documento de Origem | Número do Documento de Origem do Procedimento | Texto | Não | 50 | |
| Data de Origem | Data de Origem do Documento | Data | Não | 10 | - Formato esperado: dd/MM/aaaa<br>- Data de Origem não pode ser inferior à 'Data Hora Registro'. |
| CPF Escrivão | Identificação do Escrivão | Texto | Não | 11 | [MR08 - CPF Inválido](#) |
| Nome do Escrivão | Nome do Escrivão | Texto | Não | 100 | |
| CPF Delegado | Identificação do Delegado | Texto | Não | 11 | [MR08 - CPF Inválido](#) |
| Nome do Delegado | Nome do Delegado | Texto | Não | 100 | |
| CPF Usuário Alterador | CPF do usuário alterador | Texto | Não | 11 | [MR08 - CPF Inválido](#) |
| Nome Usuário Alterador | Nome do usuário alterador | Texto | Não | 100 | |
| Data Hora Alteração | Data e Hora da alteração | Texto (Data e Hora) | Não | 20 | Formato esperado: dd/MM/aaaaTHH:mm:ss |
| ID Unidade Apuração | ID da Unidade de Apuração | Texto | Não | 32 | |

 \
**Data e Local da Ocorrência**\
Lista com as datas e locais da ocorrência\

### <span style="color:Orchid">Tempo do Fato</span>

| Nome | Descrição | Tipo | Obrigatório | Tamanho (chars) | Formato Esperado | Regras de Recebimento |
| :--- | :--- | :---: | :---: | :---: | :---: | :--- |
| **Data Início** | Data de Início da Ocorrência | Texto |  <span style="color:green;">**Sim**</span> | 10 | `dd/mm/aaaa` | |
| **Indicativo Data Início Aproximada** | Indicativo se a data de início é aproximada | Booleano |  <span style="color:green;">**Sim**</span> | 5 | `true` \| `false` \| `null` |- Caso não seja enviado ou enviado como **null**, considerar o valor padrão **NÃO**. |
| **Hora Início** | Hora do Início da Ocorrência | Texto |  <span style="color:green;">**Sim**</span> | 5 | `HH:mm` | |
| **Indicativo Hora Início Aproximada** | Indica se a hora informada é aproximada | Booleano | *(1) Não | 5 | `true` \| `false` \| `null` | - Ao sinalizar a 'Hora Inicío Aproximada' como SIM, o campo 'Código Período Início Ocorrência' torna-se obrigatório. <br> - Caso não seja enviado ou enviado como null, considerar o valor padrão **NÃO**|
| **Código Período Início Ocorrência** | Código corporativo do período do início da ocorrência | Texto | Não | 12 |  |Valores fornecidos pelo serviço disponível em Sinesp Dados Corporativos - **qualificação-procedimento-v3: <span style="color:blue">Listar Períodos do dia.</span>** <br> - Ao sinalizar a 'Hora Início Aproximada' como aproximada SIM, o campo Código Período Início Ocorrência' torna-se obrigatório. |
| **Data Fim** | Data do fim da ocorrência | Texto | Não | 10 | `dd/mm/aaaa` | |
| **Indicativo Data Fim Aproximada** | Indicativo se a Data Fim é aproximada | Booleano | Não | 5 |  `true` \| `false` \| `null` | |
| **Hora Fim** | Hora do Fim da Ocorrência | Texto | *(3) Não | 5 | `HH:mm` | |
| **Indicativo Hora Fim Aproximada** | Indicativo se a Hora Fim é aproximada | Booleano  | Não | 5 |  `true` \| `false` \| `null` | |
| **Código Período Fim Ocorrência** | Código corporativo do período do fim da ocorrência | Texto | Não | 12 |  |Valores fornecidos pelo serviço disponível em Sinesp Dados Corporativos - **qualificação-procedimento-v3: <span style="color:blue">Listar Períodos do dia.</span>** <br> - Ao sinalizar a 'Hora Fim Aproximada' como aproximada SIM, o campo Código Período Fim Ocorrência' torna-se obrigatório. |



### Local da Ocorrência

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| CEP | CEP do Endereço da Ocorrência | Texto | Não | 8 | - Formato esperado: Somente dígitos númericos |
| Latitude da Ocorrência | Latitude da Ocorrência | Numérico Decimal | Não | (20,17) | |
| Longitude da Ocorrência | Longitude da Ocorrência | Numérico Decimal | Não | (20,17) | |
| Endereço Ocorrência | Endereço da Ocorrência | Texto | Sim | 200 | |
| Número Endereço Ocorrência | Número do endereço da Ocorrência | Texto | Não | 20 | |
| Complemento Endereço Ocorrência | Complemento do endereço da ocorrência | Texto | Não | 60 | |
| Ponto de Referência Ocorrência | Descrição do ponto de referência da ocorrência | Texto | Não | 100 | |
| Descrição do Local Ocorrência | Descrição do local da ocorrência | Texto | Não | 200 | |
| Código País | Código do país | Texto | Não | 6 | - [MR3 - Campo Obrigatório Não Informado](#)<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **localidade-v3**: "Listar Países". |
| Código UF | Sigla da UF | Texto | (5) Não | 2 | - [MR3 - Campo Obrigatório Não Informado](#)<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **localidade-v3**: "Listar Unidades Federativas".<br>- (5) Campo obrigatório somente se país for Brasil. |
| Código Município | Código do município | Texto | (5) Sim | 11 | - [MR3 - Campo Obrigatório Não Informado](#)<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **localidade-v3**: "Listar Municípios".<br>- (5) Campo obrigatório somente se país for Brasil. |
| Nome Província/Município | Nome do município/província | Texto | *(6) Não | 200 | - (6) Nome Província torna-se obrigatório se "Código País" não for o Brasil. |
| Código Bairro Ocorrência | Código do bairro em Dados Corporativos | Texto | Não | 12 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **localidade-v3**: "Listar Bairros". |
| Nome Bairro Ocorrência | Nome do Bairro | Texto | Não | 100 | - Se o valor de 'Código Bairro' for informado, o valor enviado em 'Nome Bairro' será desprezado. |
| Código Tipo de Local Ocorrência | Código do tipo de local | Texto | Sim | 10 | - [MR3 - Campo Obrigatório Não Informado](#)<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **localidade-v3**: "Listar Tipos de Local". |
| Código Subgrupo Local | Código do subgrupo do local de Dados Corporativos | Texto | *(7) Sim | 10 | - [MR3 - Campo Obrigatório Não Informado](#)<br>- *(7) Quando for informado o "Código Tipo Local": "AMBIENTE VIRTUAL (INTERNET)" não será obrigatório informar o subgrupo.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **localidade-v3**: "Listar Subgrupos Local".<br>- Subgrupo Local deve condizer com o 'Código Tipo de Local Ocorrência' informado. |

**Natureza(s) da Ocorrência**\
Lista com a(s) natureza(s) da ocorrência\
[[949008: RN - Não é permitido o cadastro de Naturezas da Ocorrência
duplicadas no B.O. Serão consideradas duplicadas as naturezas que
possuírem o mesmo código e o mesmo valor para o campo
"tentativa".]{.underline}](https://alm.serpro/rm/resources/TX_ZscnUH3REfCTPtVxzDe9CA)Caso
seja enviado Naturezas da Ocorrência duplicadas no B.O:

-   Recusar o B.O

-   Adicionar a seguinte mensagem de erro no recebimento: [[949011:
    MSG - Não é permitido o cadastro de Naturezas da Ocorrência
    duplicadas no
    B.O.]{.underline}](https://alm.serpro/rm/resources/TX_8asvEH3REfCTPtVxzDe9CA)Não
    é permitido o cadastro de Naturezas da Ocorrência duplicadas no B.O.

Serão consideradas duplicadas as naturezas que possuírem o [mesmo
código]{.underline} e o [mesmo valor para o campo
"tentativa"]{.underline}.\
\


| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Identificador da Natureza | Identificador sequencial da Natureza quando se declara a natureza | Numérico | 6 | Sim | - Se houver Identificadores de Natureza iguais: [MR07 - Elemento Duplicado](#) |
| Código da Natureza | Código da Natureza | Texto | 10 | Sim | - Se houver "Código da Natureza" e "Tentativa" iguais: [MR07 - Elemento Duplicado](#)<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **natureza-ocorrência-v3**: "Listar Natureza Ocorrência". |
| Indicativo Violência Doméstica Contra a Mulher | Indica se o crime está relacionado com a lei Maria da Penha | Booleano | 5 | Não | - Caso não seja enviado ou enviado como null, assumir valor padrão NÃO. |
| Indicativo Tráfico de Pessoa | Indicativo se a ocorrência envolve tráfico de pessoa | Booleano | 5 | Não | - Caso não seja enviado ou enviado como null, assumir valor padrão NÃO. |
| Indicativo Tentativa | Indica se o crime foi tentado, ou apenas consumado. | Booleano | 5 | Não | - Caso não seja enviado ou enviado como null, assumir valor padrão NÃO. |
| Indicativo Fato Ocorrido em Evento | Indica se o fato ocorrido no evento. | Booleano | 5 | Não | - Caso não seja enviado ou enviado como null, assumir valor padrão NÃO. |
| Nome do Evento | Nome do Evento | Texto | 120 | Não | |
| Lista de Códigos de Meio(s) Empregado(s) | Meio(s) Empregado(s), disponível em Dados Corporativos | Lista | 10 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **natureza-ocorrência-v3**: "Listar Meios Empregados". |
| Modus Operandi | Modus Operandi | Texto | 10 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-procedimento-v3**: "Listar Modus Operandi". |
| Lista de Códigos Envolvimento Natureza | Lista de Envolvimentos Pessoa Natureza | Lista | 6 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **natureza-ocorrência-v3**: "Listar Envolvimentos Natureza". |


**Vínculos e Envolvimentos**\
**Lista de Vínculos entre Pessoa, Natureza e Envolvimento**\
Vínculo entre Pessoa, Natureza e Envolvimento:
\
| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Identificador da Pessoa | Identificador da Pessoa | Numérico | Não | 4 | |
| Identificador da Natureza | Identificador da Natureza. Preencher com o campo identificador de "Natureza da Ocorrência" - Identificador da Natureza | Numérico | Não | 4 | |
| Código Tipo de Envolvimento | Tipo de Envolvimento da Pessoa | Numérico | Não | 12 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Envolvimentos Natureza". |

Se a pessoa é Física ou Jurídica, deve-se ter os tipos de envolvimento
obrigatórios mínimos:\
[[949271: RN - Envolvimento Mínima Natureza Típica
Obrigatório]{.underline}](https://alm.serpro/rm/resources/TX_T59ucH6ZEfCTPtVxzDe9CA)Se
a natureza for TÍPICA e não tiverem os envolvimentos como vítima e
suposto\_autor\_infrator:

-   Anexar ao motivo da recusa a seguinte mensagem:[[949264: MSG -
    Envolvimento obrigatório para natureza
    Típica]{.underline}](https://alm.serpro/rm/resources/TX_AiuCYH6YEfCTPtVxzDe9CA)Envolvimento
    obrigatório para natureza do tipo Típica não foi informado.

[[949273: RN - Envolvimento Mínima Natureza Atípica
Obrigatório]{.underline}](https://alm.serpro/rm/resources/TX_cwwwAH6ZEfCTPtVxzDe9CA)Se
a natureza for ATÍPICA e não tiver o envolvimento como comunicante:

-   Anexar ao motivo da recusa a seguinte mensagem:[[949266: MSG -
    Envolvimento obrigatório para natureza
    Atípica]{.underline}](https://alm.serpro/rm/resources/TX_P8BusH6YEfCTPtVxzDe9CA)Envolvimento
    obrigatório para natureza do tipo Atípica não foi informado.

**Lista de Vínculos entre Pessoa e Objeto**\
 \
| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Identificador do Objeto | Identificador do Objeto | Numérico | Não | 4 | - Identificação do Objeto tem que estar de acordo com a objetos declarados em Objetos<br>- Caso não seja identificado vínculo de Pessoa com Objeto: Recusar o B.O |
| Identificador da Pessoa | Identificador da Pessoa | Numérico | Não | 4 | - Identificação da Pessoa tem que estar de acordo com a pessoa declarada em Pessoas |
| Código Tipo de Vínculo Objeto Pessoa | Lista com Tipos de Vínculo da Pessoa com Objeto | Lista Numérica | Não | 12 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipos de Vínculo Objeto Pessoa". |

 \
**Vínculo entre Pessoas**\
| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Identificador Pessoa 1 | Identificador da Pessoa 1 | Texto | Sim | 20 | |
| Identificador da Pessoa 2 | Identificador da Pessoa 2 | Texto | Sim | 20 | |
| Código Tipo de Relacionamento Pessoa | Tipo de Relacionamento entre Pessoas | Código | Sim | 12 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Relacionamento Pessoa". |
| Ano(s) Tempo de Relação | Anos de Relação | Numérico | Não | 4 | |
| Mês(es) Tempo de Relação | Meses de Relação | Numérico | Não | 2 | |
| Dia(s) Tempo de Relação | Dias de Relação | Numérico | Não | 2 | |


 \
**Envolvimento Pessoa em Acidente(BOAT)**\
\
| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Identificador Pessoa | Identificador da Pessoa | Numérico | Sim | 4 | |
| Identificador Veículo | Identificador do Veículo envolvido do acidente. | Numérico | Não | 4 | |
| Relação Pessoa com o Veículo | Relação da Pessoa com o Veículo | Numérico | Sim | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Relação Pessoa Veículo" |
| Tipo de Envolvimento Pessoa Acidente | Tipo de Relacionamento entre a Pessoa e o Acidente (Motorista, Passageiro, Terceiro Atingido, Atropelado). | Numérico | Sim | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Envolvimento Pessoa em Acidente". |
| Utilizava capacete | Indicativo se pessoa utilizava capacete | Booleano | Não | 1 | - Se não enviado, considerar valor padrão NULL. |
| Utilizava cinto de segurança | Indicativo se pessoa utilizava cinto de segurança | Booleano | Não | 1 | - Se não enviado, considerar valor padrão NULL. |
| Utilizava cadeirinha de segurança | Indicativo se pessoa utilizava cadeirinha de segurança | Booleano | Não | 1 | - Se não enviado, considerar valor padrão NULL. |
| Outro Equipamento de segurança | Descrição de outros equipamentos de segurança utilizados pela pessoa. | Texto | Não | 50 | |
| Pessoa foi vítima fatal | Indicativo se a pessoa foi vítima fatal | Booleano | Não | 1 | - Se não enviado ou enviado como null, considerar valor padrão NÃO. |
| Análise visual externa dos ferimentos | Situação visual externa dos ferimentos da pessoa. (Grave, fatais, leves) | Numérico | Não | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Análise Externa Ferimento". |
| Condutor Habilitado | Indicativo se condutor é habilitado | Booleano | Não | 1 | - Valor somente disponível para "Tipo de Envolvimento Pessoa Acidente" como "Motorista". |
| Houve Notificação | Indicativo se pessoa foi notificada. | Booleano | Não | 1 | - Valor somente disponível para "Tipo de Envolvimento Pessoa Acidente" como "Motorista". |
| Artigo da Notificação | Artigo que a pessoa foi notificada | Texto | *(1) Não | 50 | - Valor somente disponível para "Tipo de Envolvimento Pessoa Acidente" como "Motorista".<br>- (1) Valor torna-se obrigatório quando "Houve Notificação" for igual a SIM. |
| Ação do Condutor | Ação tomada pelo condutor no acidente (Evadiu-se, Permaneceu no local, Sem informação, Socorrido). | Numérico | Não | 6 | - Valor somente disponível para "Tipo de Envolvimento Pessoa Acidente" como "Motorista".<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Ação do Condutor". |
| Suspeita de uso de álcool | Indicativo se há suspeita de uso de alcool. (Sim, Não, Não aplicável, Desconhecido) | Numérico | Não | 6 | - Somente disponível para "Tipo de Envolvimento Pessoa Acidente" como "Motorista".<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Suspeita de Álcool" |
| Exame Alcóolico Realizado | Indicativo se foi realizado exame alcóolico | Booleano | Não | 1 | - Somente disponível para "Tipo de Envolvimento Pessoa Acidente" como "Motorista".<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Exame Alcóolico Realizado" |
| Teor Alcóolico | Valor do teor alcoólico em mg/L | Texto | Não | 15 | - Somente disponível quando "Exame Alcóolico realizado" for SIM. |

**Pessoas**\
Lista de Pessoas \
**Pessoa Jurídica**\
[[934846: RN - Pessoa Jurídica -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_HkheIGGBEfCirqqtwbaCiw)**Regras
Gerais - Pessoa Jurídica**

+----------------------+----------------------+----------------------+
| **Nome do(s)         | **Descrição**        | **Regras de          |
| Campo(s)**           |                      | Recebimento**        |
+----------------------+----------------------+----------------------+
| Razão Social, Ramo   | Se a pessoa jurídica |  Se a pessoa         |
| de Atuação           | for não desconhecida | jurídica e seu       |
|                      | e tiver envolvimento | envolvimento         |
|                      | como                 | condizerem e         |
|                      |                      | os campos que se     |
|                      | -   comunicante      | t                    |
|                      |                      | ornaram obrigatórios |
|                      | -   supo             | não forem            |
|                      | sto\_autor\_infrator | preenchidos:         |
|                      |                      |                      |
|                      | ,os seguintes campos | -   Anexar ao motivo |
|                      | tor                  |     da recusa a      |
|                      | nam-se obrigatórios: |     seguinte         |
|                      |                      |                      |
|                      | -   Razão Social     |   mensagem:[[936218: |
|                      |                      |     MSG - Campo      |
|                      | -   Ramo de Atuação  |     obrigatório não  |
|                      |                      |                      |
|                      | **OBS:** Se for      |   informado]{.underl |
|                      | empresa do estado os | ine}](https://alm.se |
|                      | campos acima não são | rpro/rm/resources/TX |
|                      | obrigatórios.        | _V-bn8GidEfCirqqtwba |
|                      |                      | Ciw)\<nome\_campo\>: |
|                      |                      |     Campo            |
|                      |                      |     obrigatório não  |
|                      |                      |     informado.       |
|                      |                      |                      |
|                      |                      |                      |
+----------------------+----------------------+----------------------+

 \
| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Identificação da Pessoa Jurídica | Identificação única que irá identificar a Pessoa no envio | Numérico | 4 | Sim | |
| Empresa do Governo (Estado) | Indicativo se Empresa pertence ao Governo (Estado) | Booleano | 1 | Não | - Se o valor não for informado, considerar NÃO por padrão. |
| CNPJ | CNPJ | Texto | 14 | Não | |
| Razão Social | Nome Razão Social da Empresa | Texto | 200 | (1) Não | (1) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento. |
| Nome Fantasia | Nome Fantasia da Empresa | Texto | 200 | Não | |
| Nome Representante | Nome do Representante da Empresa | Texto | 100 | Não | |
| Código Ramo Atuação | Ramo de Atuação da Empresa | Texto | 4 | (1) Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **qualificação-pessoa-v3**: "Listar Ramo de Atuação".<br>(1) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento. |

 \
**  Pessoa Jurídica - Endereço(s)**\
  Lista com Endereço(s) da Pessoa jurídica\
| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Endereço Principal | Indicativo se é o endereço Principal | Booleano | 5 | Não | - Se o campo não for preenchido: Considerar o valor padrão como NÃO. |
| Código Tipo de Endereço | Código Tipo de Endereço | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Tipos Endereço". |
| CEP | CEP | Texto | 9 | Não | - Formato Esperado: somente dígitos numéricos |
| Latitude | Latitude | Numérico decimal | (20,17) | Não | |
| Longitude | Longitude | Numérico decimal | (20,17) | Não | |
| Logradouro | Endereço da pessoa | Texto | 200 | Não | |
| Número | Número do endereço | Texto | 20 | Não | |
| Complemento | Complemento do endereço da pessoa | Texto | 100 | Não | |
| Código do País | Código corporativo do país de endereço | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Países". |
| Código UF do Estado | Sigla da UF do endereço | Texto | 2 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar UFs". |
| Código do Município | Código corporativo para o município | Texto | 11 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios". |
| Código do Bairro | Código corporativo para o bairro | Texto | 11 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Bairros". |
| Nome do Bairro | Nome do Bairro | Texto | 100 | Não | |
| Nome da Província | Nome da Província | Texto | 200 | *Não | - Caso o 'Código do País' não for o Brasil, o campo 'Nome da Província' é obrigatório: [MR3 - Campo Obrigatório Não Informado](#) |

 \
**  Pessoa Jurídica - Documento(s)**\
  Lista com Documento(s) da pessoa jurídica\
## Documento Pessoa 

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Tipo do Documento | Código corporativo do Tipo de Documento | Texto | 10 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Tipo de Documento". |
| Nome Proprietário | Nome do Proprietário declarado | Texto | 100 | Não | |
| Código Tipo Propriedade | Tipo de Propriedade do Documento (Público/Particular) | Texto | 6 | Não | |
| Número do Documento | Número do Documento | Texto | (*1) 16 | Não | - (*1) Se o "Tipo de Documento" for correspondente ao documento CPF: o tamanho máximo do campo passa a ser 11; CPF deve ser válido.<br>- Não pode ter "Número de Documento" duplicado para um documento da pessoa. |
| Número de Matrícula/Inscrição | Número de Matrícula ou Inscrição do Documento | Texto | 15 | Não | |
| Número Livro Fls. | Número do Livro de Registro em cartório | Texto | 10 | Não | |
| Número Averbação | Número de Averbação | Texto | 15 | Não | |
| Número de Série | Número de Série do Documento | Texto | 6 | Não | |
| Número da Seção | Número de seção do documento | Texto | 6 | Não | |
| Número da Zona | Número da zona do documento | Texto | 4 | Não | |
| Órgão Expedidor | Nome do Órgão Expedidor | Texto | 50 | Não | |
| Número Agência | Número da agência do documento | Texto | 15 | Não | |
| Número Conta Banco | Número da conta bancária | Texto | 15 | Não | |
| Código País Documento | Código do País do Documento | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Países". |
| Código UF Documento | Sigla Estado do Documento | Texto | 2 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Unidades Federativas". |
| Código Município do Documento | Código do Município do Documento | Texto | 11 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios". |
| Data de Admissão | Data de Admissão do Documento | Texto | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Data Averbação | Data de Averbação | Texto | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Data de Expedição | Data de expedição do documento | Texto | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Data de Validade | Data de validade do documento | Texto | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Data da Primeira Habilitação | Data que foi obtida a primeira habilitação | Texto | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Categoria da Habilitação | Categoria da Habilitação | Texto | 5 | Não | - Campo deve ser usado quando Tipo do Documento for correspondente a CNH.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **veículos-v3**: "Listar Categorias de Habilitação de Veículo". |
| Número RENAVAM | Número Renavam do documento | Texto | 11 | Não | |
| Código Tipo Passaporte | Código Corporativo do Tipo de Passaporte da pessoa | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **qualificação-pessoa-v3**: "Listar Tipo de Passaporte". |
| Fundamentação do documento | Descrição da fundamentação/adulteração do documento | Texto | 200 | Não | |
| Descrição Documento | Descrição do Documento | Texto | 100 | Não | |
| Indicativo Adulteração | Indicativo se o documento apresenta sinais de adulteração | Booleano | 5 | Não | - Caso o campo não seja enviado, o sistema considerará o campo como valor null. |
| Número PIS/PASEP | Número do PIS (Programa de integração Social) | Numérico | 12 | Não | |

 \
**Pessoa Jurídica - Rede(s) Social(is)**\

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Rede Social | Código do Tipo de Rede Social | Texto | 6 | (1) Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - outros-v3: "Listar Redes Sociais".<br>- (1) Ou 'Código Rede Social' ou 'Outra Rede Social' devem ser informados. |
| Outra Rede Social | Caso não exista a rede social em código de rede social | Texto | 50 | (1) Não | - (1) Ou 'Código Rede Social' ou 'Outra Rede Social' devem ser informados.<br>- Caso 'Código Rede Social' seja informado, desprezar o valor de 'Outra Rede Social'. |
| Nome de Usuário na Plataforma | Nome do Usuário na Rede Social | Texto | 50 | Sim | |

## Pessoa Jurídica - Telefone(s)

  | Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| DDI do Telefone | DDI (Discagem Direta Internacional) | Texto | 3 | Não | Formato esperado: somente números |
| Código do País | Código do País | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Países" |
| DDD do Telefone | DDD (Discagem Direta a Distância) | Texto | 3 | Não | Formato esperado: somente números |
| Número do Telefone | Número do Telefone da Pessoa | Texto | 20 | Sim | Formato esperado: somente números |
| Indicativo WhatsApp | Indicativo se o telefone informado é de um contato com WhatsApp | Booleano | 5 | Não | |
| Código Tipo de Telefone | Código Corporativo do Tipo de Telefone (Comercial, celular..) | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - qualificação-pessoa-v3: "Listar Tipo de Telefone". |
| Nome do Contato de Recado | Texto para descrever contato que atende pelo telefone | Texto | 50 | Não | |

**Pessoa Jurídica - Email(s)**\
[[935367: RN - Email -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_AuHzkGPdEfCirqqtwbaCiw)

  ------------------- --------------- ---------- ------------- ----------------- ---------------------------
  **Nome do Campo**   **Descrição**   **Tipo**   **Tamanho**   **Obrigatório**   **Regras de Recebimento**
  E-mail              E-mail          Texto      256           Não                
  ------------------- --------------- ---------- ------------- ----------------- ---------------------------

**Pessoa Física**\
[[934897: RN - Pessoa Física -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_Q8rsEGGcEfCirqqtwbaCiw)**Pessoa
Física - Regras de Pessoa**\
[[935407: RN - Dados Exclusivos Pessoa Física -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_TueLIGP1EfCirqqtwbaCiw)**Regras
Específicas**\
[[949028: RN - Campos Obrigatórios de Pessoa Física a partir do
Envolvimento -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_te-_4H3gEfCTPtVxzDe9CA)Regras
de mudança de obrigatoriedade de campos de acordo com a natureza
informada.

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Regras |          |          |          |
| do(s)    | crição** | de       |          |          |          |
| Ca       |          | Process  |          |          |          |
| mpo(s)** |          | amento** |          |          |          |
+----------+----------+----------+----------+----------+----------+
| \(1\)    | Se a     |  Se a    |          |          |          |
| Nome     | natureza | natureza |          |          |          |
| C        | da       | da       |          |          |          |
| ompleto, | oc       | oc       |          |          |          |
| Sexo,    | orrência | orrência |          |          |          |
| R        | for      | for do   |          |          |          |
| aça/Cor, | TÍPICA e | tipo     |          |          |          |
| Estado   | se       | TIPI     |          |          |          |
| Civil,   | ho       | CA, a(s) |          |          |          |
| Data de  | uver uma | p        |          |          |          |
| Nas      | pessoa   | essoa(s) |          |          |          |
| cimento, | física,  | informad |          |          |          |
| P        | tem que  | a(s) tem |          |          |          |
| rofissão | existir  | que ter  |          |          |          |
|          | pelo     | envo     |          |          |          |
|          | menos    | lvimento |          |          |          |
|          | dois     | como     |          |          |          |
|          | e        | vítima e |          |          |          |
|          | nvolvime | s        |          |          |          |
|          | ntos com | uporto\_ |          |          |          |
|          | o vítima | autor\_i |          |          |          |
|          | e su     | nfrator. |          |          |          |
|          | posto\_a | E os     |          |          |          |
|          | utor\_in | campos   |          |          |          |
|          | frator.\ | \"Nome   |          |          |          |
|          | e os     | Com      |          |          |          |
|          | s        | pleto\", |          |          |          |
|          | eguintes | \        |          |          |          |
|          | campos   | "Sexo\", |          |          |          |
|          | de       | \"Raç    |          |          |          |
|          | pessoa   | a/Cor\", |          |          |          |
|          | física   | \"Estado |          |          |          |
|          | tornam-  | Civil\", |          |          |          |
|          | se obrig | \"Data   |          |          |          |
|          | atórios: | de       |          |          |          |
|          |          | Nasci    |          |          |          |
|          | -   Nome | mento\", |          |          |          |
|          |          | \"Pro    |          |          |          |
|          | Completo | fissão\" |          |          |          |
|          |          | t        |          |          |          |
|          | -   Sexo | ornam-se |          |          |          |
|          |          | obrig    |          |          |          |
|          | -        | atórios. |          |          |          |
|          | Raça/Cor | Caso o   |          |          |          |
|          |          | campo    |          |          |          |
|          | -        | não for  |          |          |          |
|          |   Estado | enviado: |          |          |          |
|          |          |          |          |          |          |
|          |    Civil | -        |          |          |          |
|          |          |   Anexar |          |          |          |
|          | -   Data |     ao   |          |          |          |
|          |     de   |          |          |          |          |
|          |     Na   |   motivo |          |          |          |
|          | scimento |     da   |          |          |          |
|          |          |          |          |          |          |
|          | -   P    |   recusa |          |          |          |
|          | rofissão |     a    |          |          |          |
|          |          |          |          |          |          |
|          |          | seguinte |          |          |          |
|          |          |     me   |          |          |          |
|          |          | nsagem:[ |          |          |          |
|          |          | [936218: |          |          |          |
|          |          |          |          |          |          |
|          |          |    MSG - |          |          |          |
|          |          |          |          |          |          |
|          |          |    Campo |          |          |          |
|          |          |     obr  |          |          |          |
|          |          | igatório |          |          |          |
|          |          |     não  |          |          |          |
|          |          |     in   |          |          |          |
|          |          | formado] |          |          |          |
|          |          | {.underl |          |          |          |
|          |          | ine}](ht |          |          |          |
|          |          | tps://al |          |          |          |
|          |          | m.serpro |          |          |          |
|          |          | /rm/reso |          |          |          |
|          |          | urces/TX |          |          |          |
|          |          | _V-bn8Gi |          |          |          |
|          |          | dEfCirqq |          |          |          |
|          |          | twbaCiw) |          |          |          |
|          |          | \<nome\_ |          |          |          |
|          |          | campo\>: |          |          |          |
|          |          |          |          |          |          |
|          |          |    Campo |          |          |          |
|          |          |     obr  |          |          |          |
|          |          | igatório |          |          |          |
|          |          |     não  |          |          |          |
|          |          |     in   |          |          |          |
|          |          | formado. |          |          |          |
+----------+----------+----------+----------+----------+----------+
| \(2\)    | Se a     |  Se a    |          |          |          |
| Nome     | natureza | natureza |          |          |          |
| C        | da       | da       |          |          |          |
| ompleto, | oc       | oc       |          |          |          |
| Nome da  | orrência | orrência |          |          |          |
| Mãe,     | for      | for do   |          |          |          |
| Nacion   | ATÍPICA  | tipo     |          |          |          |
| alidade, | e essa   | AT       |          |          |          |
| P        | tiver    | ÍPICA, a |          |          |          |
| rofissão | uma      | pess     |          |          |          |
|          | pessoa   | oa infor |          |          |          |
|          | física   | mada tem |          |          |          |
|          | com      | que ter  |          |          |          |
|          | um envo  | envo     |          |          |          |
|          | lvimento | lvimento |          |          |          |
|          | como     | como     |          |          |          |
|          |          | comu     |          |          |          |
|          | -   com  | nicante. |          |          |          |
|          | unicante | E os     |          |          |          |
|          |          | campos   |          |          |          |
|          | ,os      | \"Nome   |          |          |          |
|          | s        | Com      |          |          |          |
|          | eguintes | pleto\", |          |          |          |
|          | campos   | \"Nome   |          |          |          |
|          | t        | da       |          |          |          |
|          | ornam-se | Mãe\",   |          |          |          |
|          | obrig    | \        |          |          |          |
|          | atórios: | "Naciona |          |          |          |
|          |          | lidade\" |          |          |          |
|          | -   Nome | e \"Pro  |          |          |          |
|          |          | fissão\" |          |          |          |
|          | Completo | t        |          |          |          |
|          |          | ornam-se |          |          |          |
|          | -   Nome | obrig    |          |          |          |
|          |     da   | atórios. |          |          |          |
|          |     Mãe  | Caso o   |          |          |          |
|          |          | campo    |          |          |          |
|          | -        | não for  |          |          |          |
|          |    Nacio | enviado: |          |          |          |
|          | nalidade |          |          |          |          |
|          |          | -        |          |          |          |
|          | -   P    |   Anexar |          |          |          |
|          | rofissão |     ao   |          |          |          |
|          |          |          |          |          |          |
|          |          |   motivo |          |          |          |
|          |          |     da   |          |          |          |
|          |          |          |          |          |          |
|          |          |   recusa |          |          |          |
|          |          |     a    |          |          |          |
|          |          |          |          |          |          |
|          |          | seguinte |          |          |          |
|          |          |     me   |          |          |          |
|          |          | nsagem:[ |          |          |          |
|          |          | [936218: |          |          |          |
|          |          |          |          |          |          |
|          |          |    MSG - |          |          |          |
|          |          |          |          |          |          |
|          |          |    Campo |          |          |          |
|          |          |     obr  |          |          |          |
|          |          | igatório |          |          |          |
|          |          |     não  |          |          |          |
|          |          |     in   |          |          |          |
|          |          | formado] |          |          |          |
|          |          | {.underl |          |          |          |
|          |          | ine}](ht |          |          |          |
|          |          | tps://al |          |          |          |
|          |          | m.serpro |          |          |          |
|          |          | /rm/reso |          |          |          |
|          |          | urces/TX |          |          |          |
|          |          | _V-bn8Gi |          |          |          |
|          |          | dEfCirqq |          |          |          |
|          |          | twbaCiw) |          |          |          |
|          |          | \<nome\_ |          |          |          |
|          |          | campo\>: |          |          |          |
|          |          |          |          |          |          |
|          |          |    Campo |          |          |          |
|          |          |     obr  |          |          |          |
|          |          | igatório |          |          |          |
|          |          |     não  |          |          |          |
|          |          |     in   |          |          |          |
|          |          | formado. |          |          |          |
+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Ident    | Ident    | Texto    | 10       | Sim      | [[MR07 - |
| ificação | ificação |          |          |          | Elemento |
| da       | que irá  |          |          |          | Dup      |
| Pessoa   | ide      |          |          |          | licado]{ |
| Física   | ntificar |          |          |          | .underli |
|          | a Pessoa |          |          |          | ne}](htt |
|          | no envio |          |          |          | ps://alm |
|          |          |          |          |          | .serpro/ |
|          |          |          |          |          | rm/resou |
|          |          |          |          |          | rces/TX_ |
|          |          |          |          |          | oFepUHSE |
|          |          |          |          |          | EfCXc_rf |
|          |          |          |          |          | fzpa6A)  |
+----------+----------+----------+----------+----------+----------+
| Código   | Dado     | Texto    | 6        | Sim      | -   Para |
| Tipo     | cor      |          |          |          |          |
| Pessoa   | porativo |          |          |          |   Pessoa |
|          | do tipo  |          |          |          |          |
|          | de       |          |          |          |  Física, |
|          | pessoa   |          |          |          |     obr  |
|          |          |          |          |          | igatório |
|          |          |          |          |          |          |
|          |          |          |          |          |  receber |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |          |
|          |          |          |          |          |    como: |
|          |          |          |          |          |          |
|          |          |          |          |          |   FISICA |
+----------+----------+----------+----------+----------+----------+
| Desc     | In       | Booleano | 5        | Não      | -   Se   |
| onhecida | dicativo |          |          |          |     for  |
|          | se a     |          |          |          |          |
|          | Pessoa é |          |          |          |  marcado |
|          | Desco    |          |          |          |     como |
|          | nhecida. |          |          |          |          |
|          |          |          |          |          |   Descon |
|          |          |          |          |          | hecida - |
|          |          |          |          |          |          |
|          |          |          |          |          | \'SIM\', |
|          |          |          |          |          |     os   |
|          |          |          |          |          |          |
|          |          |          |          |          |   demais |
|          |          |          |          |          |          |
|          |          |          |          |          |   campos |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |   pessoa |
|          |          |          |          |          |          |
|          |          |          |          |          |   física |
|          |          |          |          |          |     t    |
|          |          |          |          |          | ornam-se |
|          |          |          |          |          |     Não  |
|          |          |          |          |          |          |
|          |          |          |          |          |    Obrig |
|          |          |          |          |          | atórios. |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |   Pessoa |
|          |          |          |          |          |          |
|          |          |          |          |          |   física |
|          |          |          |          |          |     desc |
|          |          |          |          |          | onhecida |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     pode |
|          |          |          |          |          |     ter  |
|          |          |          |          |          |     os   |
|          |          |          |          |          |     s    |
|          |          |          |          |          | eguintes |
|          |          |          |          |          |          |
|          |          |          |          |          |  envolvi |
|          |          |          |          |          | mentos:  |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |  -   COM |
|          |          |          |          |          | UNICANTE |
|          |          |          |          |          |          |
|          |          |          |          |          |     -    |
|          |          |          |          |          | CONDUTOR |
|          |          |          |          |          |          |
|          |          |          |          |          |     -    |
|          |          |          |          |          | ADVOGADO |
|          |          |          |          |          |          |
|          |          |          |          |          |     -    |
|          |          |          |          |          | DEFENSOR |
|          |          |          |          |          |          |
|          |          |          |          |          |  PÚBLICO |
|          |          |          |          |          |          |
|          |          |          |          |          |     -    |
|          |          |          |          |          | PROMOTOR |
|          |          |          |          |          |          |
|          |          |          |          |          |       DE |
|          |          |          |          |          |          |
|          |          |          |          |          |  JUSTIÇA |
|          |          |          |          |          |          |
|          |          |          |          |          |     -    |
|          |          |          |          |          | TRADUTOR |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |   -   IN |
|          |          |          |          |          | TÉRPRETE |
|          |          |          |          |          |          |
|          |          |          |          |          |     -    |
|          |          |          |          |          | CONSELHO |
|          |          |          |          |          |          |
|          |          |          |          |          |     -    |
|          |          |          |          |          |    REPRE |
|          |          |          |          |          | SENTANTE |
|          |          |          |          |          |          |
|          |          |          |          |          |    LEGAL |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |   -   PR |
|          |          |          |          |          | OCURADOR |
+----------+----------+----------+----------+----------+----------+
| **Inf    |          |          |          |          |          |
| ormações |          |          |          |          |          |
| da       |          |          |          |          |          |
| Pessoa   |          |          |          |          |          |
| No       |          |          |          |          |          |
| Fato**   |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Estado   | Estado   | Numérico | 3        | Não      | Valores  |
| Aparente | Aparente |          |          |          | fo       |
| da       | em que a |          |          |          | rnecidos |
| Pessoa   | pessoa   |          |          |          | pelos    |
|          | se       |          |          |          | serviços |
|          | en       |          |          |          | dis      |
|          | contrava |          |          |          | poníveis |
|          |          |          |          |          | no       |
|          |          |          |          |          | Sinesp   |
|          |          |          |          |          | Dados    |
|          |          |          |          |          | Corpo    |
|          |          |          |          |          | rativos: |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          | Estado   |
|          |          |          |          |          | Aparente |
|          |          |          |          |          | P        |
|          |          |          |          |          | essoa\". |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Booleano | 5        | Não      |          |
| dicativo | dicativo |          |          |          |          |
| Torn     | se a     |          |          |          |          |
| ozeleira | Pessoa   |          |          |          |          |
| El       | utiliza  |          |          |          |          |
| etrônica | Torn     |          |          |          |          |
|          | ozeleira |          |          |          |          |
|          | El       |          |          |          |          |
|          | etrônica |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Ident    | Ident    | Texto    | 25       | Não      |          |
| ificação | ificação |          |          |          |          |
| da       | da       |          |          |          |          |
| Torn     | torn     |          |          |          |          |
| ozeleira | ozeleira |          |          |          |          |
| El       | el       |          |          |          |          |
| etrônica | etrônica |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Booleano | 5        | Não      |          |
| dicativo | dicativo |          |          |          |          |
| In       | se a     |          |          |          |          |
| tegrante | Pessoa   |          |          |          |          |
| Facção   | se       |          |          |          |          |
| C        | id       |          |          |          |          |
| riminosa | entifica |          |          |          |          |
|          | com      |          |          |          |          |
|          | alguma   |          |          |          |          |
|          | facção   |          |          |          |          |
|          | c        |          |          |          |          |
|          | riminosa |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Ident    | Ident    | Texto    | 6        | Não      | -        |
| ificação | ificação |          |          |          |  Valores |
| da       | da       |          |          |          |     fo   |
| Facção   | Facção   |          |          |          | rnecidos |
| C        | C        |          |          |          |          |
| riminosa | riminosa |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |          |
|          |          |          |          |          |    **out |
|          |          |          |          |          | ros-v3** |
|          |          |          |          |          |     :    |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Facçõ |
|          |          |          |          |          | es Crimi |
|          |          |          |          |          | nosas\". |
+----------+----------+----------+----------+----------+----------+
| Nome da  | Nome da  | Texto    | 50       | \*(3)Não | -   Caso |
| Facção   | Facção   |          |          |          |     o    |
| C        | Cr       |          |          |          |          |
| riminosa | iminosa, |          |          |          |    campo |
|          | caso não |          |          |          |          |
|          | tenha em |          |          |          |  \"Ident |
|          | Dados    |          |          |          | ificação |
|          | Corpo    |          |          |          |     da   |
|          | rativos. |          |          |          |          |
|          |          |          |          |          |   Facção |
|          |          |          |          |          |     cri  |
|          |          |          |          |          | minosa\" |
|          |          |          |          |          |     seja |
|          |          |          |          |          |     pr   |
|          |          |          |          |          | eenchido |
|          |          |          |          |          |     e    |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |  válido, |
|          |          |          |          |          |     este |
|          |          |          |          |          |          |
|          |          |          |          |          |    campo |
|          |          |          |          |          |     se   |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |  enviado |
|          |          |          |          |          |     deve |
|          |          |          |          |          |     ser  |
|          |          |          |          |          |     i    |
|          |          |          |          |          | gnorado. |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |    \*(3) |
|          |          |          |          |          |     Se o |
|          |          |          |          |          |          |
|          |          |          |          |          |    campo |
|          |          |          |          |          |     \"In |
|          |          |          |          |          | tegrante |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |   facção |
|          |          |          |          |          |     cri  |
|          |          |          |          |          | minosa\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |  enviado |
|          |          |          |          |          |     como |
|          |          |          |          |          |     SIM, |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    campo |
|          |          |          |          |          |          |
|          |          |          |          |          |  \"Ident |
|          |          |          |          |          | ificação |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Facção |
|          |          |          |          |          |     Cri  |
|          |          |          |          |          | minosa\" |
|          |          |          |          |          |          |
|          |          |          |          |          | torna-se |
|          |          |          |          |          |     obri |
|          |          |          |          |          | gatório. |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Booleano | 5        | Não      | -   Se o |
| dicativo | dicativo |          |          |          |          |
| Turista  | se a     |          |          |          |    valor |
|          | Pessoa é |          |          |          |     não  |
|          | Turista  |          |          |          |     for  |
|          |          |          |          |          |     in   |
|          |          |          |          |          | formado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     como |
|          |          |          |          |          |     NÃO. |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Booleano | 5        | Não      | -   Se o |
| dicativo | dicativo |          |          |          |          |
| Situação | se a     |          |          |          |    valor |
| de Rua   | Pessoa   |          |          |          |     não  |
|          | vive em  |          |          |          |     for  |
|          | situação |          |          |          |     in   |
|          | de rua   |          |          |          | formado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     como |
|          |          |          |          |          |     NÃO. |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Booleano | 5        | Não      | -   Se o |
| dicativo | dicativo |          |          |          |          |
| Fato     | se a     |          |          |          |    valor |
| Ocorrido | Pessoa   |          |          |          |     não  |
| em       | no ato   |          |          |          |     for  |
| Serviço  | estava   |          |          |          |     in   |
|          | em       |          |          |          | formado, |
|          | horário  |          |          |          |     co   |
|          | de       |          |          |          | nsiderar |
|          | serviço  |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     como |
|          |          |          |          |          |     NÃO. |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Booleano | 5        | Não      | -   Se o |
| dicativo | dicativo |          |          |          |          |
| V        | se houve |          |          |          |    valor |
| iolência | v        |          |          |          |     não  |
| D        | iolência |          |          |          |     for  |
| oméstica | d        |          |          |          |     in   |
|          | oméstica |          |          |          | formado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     como |
|          |          |          |          |          |     NÃO. |
+----------+----------+----------+----------+----------+----------+
| **Inf    |          |          |          |          |          |
| ormações |          |          |          |          |          |
| Cadas    |          |          |          |          |          |
| trais da |          |          |          |          |          |
| Pessoa** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| CPF      | CPF da   | Texto    | 11       | Não      | [[MR08 - |
|          | Pessoa   |          |          |          | CPF      |
|          |          |          |          |          | In       |
|          |          |          |          |          | válido]{ |
|          |          |          |          |          | .underli |
|          |          |          |          |          | ne}](htt |
|          |          |          |          |          | ps://alm |
|          |          |          |          |          | .serpro/ |
|          |          |          |          |          | rm/resou |
|          |          |          |          |          | rces/TX_ |
|          |          |          |          |          | ADiBEHSJ |
|          |          |          |          |          | EfCXc_rf |
|          |          |          |          |          | fzpa6A)\ |
|          |          |          |          |          | Se os    |
|          |          |          |          |          | campos   |
|          |          |          |          |          | \'CPF\', |
|          |          |          |          |          | \'Nome   |
|          |          |          |          |          | Co       |
|          |          |          |          |          | mpleto\' |
|          |          |          |          |          | e        |
|          |          |          |          |          | \'Pro    |
|          |          |          |          |          | fissão\' |
|          |          |          |          |          | forem    |
|          |          |          |          |          | iguais:  |
|          |          |          |          |          | [[MR07 - |
|          |          |          |          |          | Elemento |
|          |          |          |          |          | Dup      |
|          |          |          |          |          | licado]{ |
|          |          |          |          |          | .underli |
|          |          |          |          |          | ne}](htt |
|          |          |          |          |          | ps://alm |
|          |          |          |          |          | .serpro/ |
|          |          |          |          |          | rm/resou |
|          |          |          |          |          | rces/TX_ |
|          |          |          |          |          | oFepUHSE |
|          |          |          |          |          | EfCXc_rf |
|          |          |          |          |          | fzpa6A)\ |
|          |          |          |          |          | Formato  |
|          |          |          |          |          | e        |
|          |          |          |          |          | sperado: |
|          |          |          |          |          | Somente  |
|          |          |          |          |          | dígitos  |
|          |          |          |          |          | nu       |
|          |          |          |          |          | méricos. |
+----------+----------+----------+----------+----------+----------+
| Nome     | Nome     | Texto    | 100      | (1)(2)   | \*(1)    |
| Completo | completo |          |          | \*Sim    | Vide     |
|          | da       |          |          |          | tabela   |
|          | pessoa   |          |          |          | acima de |
|          |          |          |          |          | campo    |
|          |          |          |          |          | obr      |
|          |          |          |          |          | igatório |
|          |          |          |          |          | para     |
|          |          |          |          |          | pessoa   |
|          |          |          |          |          | de       |
|          |          |          |          |          | acordo   |
|          |          |          |          |          | com tipo |
|          |          |          |          |          | envolv   |
|          |          |          |          |          | imento.\ |
|          |          |          |          |          | \*(2)    |
|          |          |          |          |          | Vide     |
|          |          |          |          |          | tabela   |
|          |          |          |          |          | acima de |
|          |          |          |          |          | campo    |
|          |          |          |          |          | obr      |
|          |          |          |          |          | igatório |
|          |          |          |          |          | para     |
|          |          |          |          |          | pessoa   |
|          |          |          |          |          | de       |
|          |          |          |          |          | acordo   |
|          |          |          |          |          | com tipo |
|          |          |          |          |          | envol    |
|          |          |          |          |          | vimento. |
+----------+----------+----------+----------+----------+----------+
| Nome     | Nome     | Texto    | 100      | Não      |          |
| Social   | social   |          |          |          |          |
|          | da       |          |          |          |          |
|          | pessoa   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| Opção    | da Opção |          |          |          |  Valores |
| Nome     | nome     |          |          |          |     fo   |
| Social   | social   |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **q  |
|          |          |          |          |          | ualifica |
|          |          |          |          |          | cao-pess |
|          |          |          |          |          | oa-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Opções |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Nome |
|          |          |          |          |          |     S    |
|          |          |          |          |          | ocial\". |
+----------+----------+----------+----------+----------+----------+
| Alcunha  | Apelido  | Texto    | 100      | Não      |          |
|          | da       |          |          |          |          |
|          | pessoa   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Data de  | Data de  | Data     | 10       | \(1\)    | Formato  |
| Na       | Na       |          |          | Não      | e        |
| scimento | scimento |          |          |          | sperado: |
|          | da       |          |          |          | dd/      |
|          | pessoa   |          |          |          | MM/aaaa\ |
|          |          |          |          |          | \*(1)    |
|          |          |          |          |          | Vide     |
|          |          |          |          |          | tabela   |
|          |          |          |          |          | acima de |
|          |          |          |          |          | campo    |
|          |          |          |          |          | obr      |
|          |          |          |          |          | igatório |
|          |          |          |          |          | para     |
|          |          |          |          |          | pessoa   |
|          |          |          |          |          | de       |
|          |          |          |          |          | acordo   |
|          |          |          |          |          | com tipo |
|          |          |          |          |          | envol    |
|          |          |          |          |          | vimento. |
+----------+----------+----------+----------+----------+----------+
| Idade    | Idade    | Inteiro  | 3        | Não      | -        |
| I        | I        |          |          |          |    Idade |
| nformada | nformada |          |          |          |     não  |
|          | quando   |          |          |          |     pode |
|          | não      |          |          |          |     ser  |
|          | pr       |          |          |          |          |
|          | eenchido |          |          |          | negativa |
|          | data de  |          |          |          |          |
|          | na       |          |          |          | -        |
|          | scimento |          |          |          |    Idade |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     pode |
|          |          |          |          |          |     ser  |
|          |          |          |          |          |          |
|          |          |          |          |          | superior |
|          |          |          |          |          |     a    |
|          |          |          |          |          |     150  |
|          |          |          |          |          |          |
|          |          |          |          |          |    anos. |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Booleano | 1        | Não      | -   Se o |
| dicativo | dicativo |          |          |          |          |
| Idade    | de Idade |          |          |          |    campo |
| Ap       | Ap       |          |          |          |     não  |
| roximada | roximada |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          | enviado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |          |
|          |          |          |          |          |   padrão |
|          |          |          |          |          |     como |
|          |          |          |          |          |     NÃO. |
+----------+----------+----------+----------+----------+----------+
| Nome Mãe | Nome da  | Texto    | 100      | \(2\)    | \*(2)    |
|          | Mãe      |          |          | Não      | Vide     |
|          |          |          |          |          | tabela   |
|          |          |          |          |          | acima de |
|          |          |          |          |          | campo    |
|          |          |          |          |          | obr      |
|          |          |          |          |          | igatório |
|          |          |          |          |          | para     |
|          |          |          |          |          | pessoa   |
|          |          |          |          |          | de       |
|          |          |          |          |          | acordo   |
|          |          |          |          |          | com tipo |
|          |          |          |          |          | envol    |
|          |          |          |          |          | vimento. |
+----------+----------+----------+----------+----------+----------+
| Nome Pai | Nome do  | Texto    | 100      | Não      |          |
|          | Pai      |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| Sexo     | cor      |          |          |          |  Valores |
|          | porativo |          |          |          |     fo   |
|          | do sexo  |          |          |          | rnecidos |
|          | da       |          |          |          |          |
|          | Pessoa   |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |          |
|          |          |          |          |          |    - **q |
|          |          |          |          |          | ualifica |
|          |          |          |          |          | cao-pess |
|          |          |          |          |          | oa-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |          |
|          |          |          |          |          |  Sexo\". |
|          |          |          |          |          |          |
|          |          |          |          |          | \*(1)    |
|          |          |          |          |          | Vide     |
|          |          |          |          |          | tabela   |
|          |          |          |          |          | acima de |
|          |          |          |          |          | campo    |
|          |          |          |          |          | obr      |
|          |          |          |          |          | igatório |
|          |          |          |          |          | para     |
|          |          |          |          |          | pessoa   |
|          |          |          |          |          | de       |
|          |          |          |          |          | acordo   |
|          |          |          |          |          | com tipo |
|          |          |          |          |          | envol    |
|          |          |          |          |          | vimento. |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| Or       | da       |          |          |          |  Valores |
| ientação | Or       |          |          |          |     fo   |
| Sexual   | ientação |          |          |          | rnecidos |
|          | Sexual   |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **q  |
|          |          |          |          |          | ualifica |
|          |          |          |          |          | ção-pess |
|          |          |          |          |          | oa-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Ori  |
|          |          |          |          |          | entações |
|          |          |          |          |          |     Se   |
|          |          |          |          |          | xuais\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| Autode   | cor      |          |          |          |  Valores |
| claração | porativo |          |          |          |     fo   |
| de       | de       |          |          |          | rnecidos |
| Gênero   | Autode   |          |          |          |          |
|          | claração |          |          |          |    pelos |
|          | de       |          |          |          |          |
|          | or       |          |          |          | serviços |
|          | ientação |          |          |          |     dis  |
|          | de       |          |          |          | poníveis |
|          | gênero   |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **q  |
|          |          |          |          |          | ualifica |
|          |          |          |          |          | ção-pess |
|          |          |          |          |          | oa-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Auto |
|          |          |          |          |          |     De   |
|          |          |          |          |          | claração |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     G    |
|          |          |          |          |          | ênero\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Id       | Texto    | 6        | Não      | -        |
| Id       | entidade |          |          |          |  Valores |
| entidade | de       |          |          |          |     fo   |
| de       | gênero   |          |          |          | rnecidos |
| Gênero   |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **q  |
|          |          |          |          |          | ualifica |
|          |          |          |          |          | ção-pess |
|          |          |          |          |          | oa-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Id   |
|          |          |          |          |          | entidade |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     G    |
|          |          |          |          |          | ênero\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Raça/Cor | Texto    | 6        | \(1\)    | -        |
| Raça/Cor | da       |          |          | Não      |  Valores |
|          | pessoa   |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **q  |
|          |          |          |          |          | ualifica |
|          |          |          |          |          | ção-pess |
|          |          |          |          |          | oa-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Raças |
|          |          |          |          |          |     e    |
|          |          |          |          |          |          |
|          |          |          |          |          |   Cor\". |
|          |          |          |          |          |          |
|          |          |          |          |          | \*(1)    |
|          |          |          |          |          | Vide     |
|          |          |          |          |          | tabela   |
|          |          |          |          |          | acima de |
|          |          |          |          |          | campo    |
|          |          |          |          |          | obr      |
|          |          |          |          |          | igatório |
|          |          |          |          |          | para     |
|          |          |          |          |          | pessoa   |
|          |          |          |          |          | de       |
|          |          |          |          |          | acordo   |
|          |          |          |          |          | com tipo |
|          |          |          |          |          | envol    |
|          |          |          |          |          | vimento. |
+----------+----------+----------+----------+----------+----------+
| Código   | Estado   | Texto    | 6        | \(1\)    | -        |
| Estado   | civil da |          |          | Não      |  Valores |
| Civil    | pessoa   |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **q  |
|          |          |          |          |          | ualifica |
|          |          |          |          |          | ção-pess |
|          |          |          |          |          | oa-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Estado  |
|          |          |          |          |          | Civil\". |
|          |          |          |          |          |          |
|          |          |          |          |          | \*(1)    |
|          |          |          |          |          | Vide     |
|          |          |          |          |          | tabela   |
|          |          |          |          |          | acima de |
|          |          |          |          |          | campo    |
|          |          |          |          |          | obr      |
|          |          |          |          |          | igatório |
|          |          |          |          |          | para     |
|          |          |          |          |          | pessoa   |
|          |          |          |          |          | de       |
|          |          |          |          |          | acordo   |
|          |          |          |          |          | com tipo |
|          |          |          |          |          | envol    |
|          |          |          |          |          | vimento. |
+----------+----------+----------+----------+----------+----------+
| Código   | Nacio    | Texto    | 6        | \(2\)    | -        |
| Nacio    | nalidade |          |          | Não      |  Valores |
| nalidade | da       |          |          |          |     fo   |
|          | pessoa   |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     P    |
|          |          |          |          |          | aíses\". |
|          |          |          |          |          |          |
|          |          |          |          |          | \*(2)    |
|          |          |          |          |          | Vide     |
|          |          |          |          |          | tabela   |
|          |          |          |          |          | acima de |
|          |          |          |          |          | campo    |
|          |          |          |          |          | obr      |
|          |          |          |          |          | igatório |
|          |          |          |          |          | para     |
|          |          |          |          |          | pessoa   |
|          |          |          |          |          | de       |
|          |          |          |          |          | acordo   |
|          |          |          |          |          | com tipo |
|          |          |          |          |          | envol    |
|          |          |          |          |          | vimento. |
+----------+----------+----------+----------+----------+----------+
| Código   | Estado   | Texto    | 2        | Não      | -        |
| Natu     | de       |          |          |          |  Valores |
| ralidade | natu     |          |          |          |     fo   |
| - UF     | ralidade |          |          |          | rnecidos |
|          | da       |          |          |          |          |
|          | pessoa   |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   UFs\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 11       | Não      | -        |
| Natu     | do       |          |          |          |  Valores |
| ralidade | M        |          |          |          |     fo   |
| -        | unicípio |          |          |          | rnecidos |
| M        |          |          |          |          |          |
| unicípio |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Munic |
|          |          |          |          |          | ípios\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |   Código |
|          |          |          |          |          |     Natu |
|          |          |          |          |          | ralidade |
|          |          |          |          |          |     UF   |
|          |          |          |          |          |     deve |
|          |          |          |          |          |          |
|          |          |          |          |          | condizer |
|          |          |          |          |          |     com  |
|          |          |          |          |          |     a UF |
|          |          |          |          |          |     do   |
|          |          |          |          |          |     Mu   |
|          |          |          |          |          | nicípio. |
+----------+----------+----------+----------+----------+----------+
| Nome do  | Nome do  | Texto    | 200      | Não      |          |
| Mun      | mun      |          |          |          |          |
| icípio/P | icípio/p |          |          |          |          |
| rovíncia | rovíncia |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| Esco     | da       |          |          |          |  Valores |
| laridade | Esco     |          |          |          |     fo   |
|          | laridade |          |          |          | rnecidos |
|          | da       |          |          |          |          |
|          | Pessoa   |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **q  |
|          |          |          |          |          | ualifica |
|          |          |          |          |          | ção-pess |
|          |          |          |          |          | oa-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Escolar |
|          |          |          |          |          | idade\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| Tipo     | do Tipo  |          |          |          |  Valores |
| Físico   | de       |          |          |          |     fo   |
|          | Físico   |          |          |          | rnecidos |
|          | da       |          |          |          |          |
|          | Pessoa   |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **q  |
|          |          |          |          |          | ualifica |
|          |          |          |          |          | ção-pess |
|          |          |          |          |          | oa-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Tipo F |
|          |          |          |          |          | ísico\". |
+----------+----------+----------+----------+----------+----------+
| Altura   | Altura   | Numérico | 3        | Não      |          |
|          | da       |          |          |          |          |
|          | Pessoa   |          |          |          |          |
|          | em       |          |          |          |          |
|          | cen      |          |          |          |          |
|          | tímetros |          |          |          |          |
+----------+----------+----------+----------+----------+----------+

 \
**Pessoa Física - Profissão**\
##  Lista de Profissões\

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Tipo de Trabalho | Tipo de Trabalho | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Tipo de Trabalho". |
| Código Profissão | Código corporativo da Profissão da Pessoa | Texto | 6 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Profissões". |
| Renda Mensal | Renda mensal por profissão em reais | Numérico decimal | (11,2) | Não | |

 \
**Pessoa Física - Lista de Parte(s) do Corpo**\
** Lista de Partes do Corpo\
| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Parte do Corpo | Código da Parte do Corpo | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Parte do Corpo". |
| Código Identificação Visual | Código da Identificação Visual | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Tipo de Identificação Visual". |
| Código de Cor da Identificação Visual | Cor da Identificação Visual | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Cor Identificação Visual". |
| Descrição da Parte do Corpo | Descrição da Parte do Corpo | Texto | 200 | Não | |

**Pessoa Física - Lista de Deficiência(s)**\
** **    Lista de Deficiências Físicas\
| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Tipo de Deficiência | Tipo da Deficiência da Pessoa | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Tipo de Deficiência". |
| Nome da Deficiência | Nome da Deficiência Física | Texto | 100 | Não | |

 \
**Pessoa Física - Documento(s)**\
**   **  Lista de Documentos\
     RN Recebimento: Permitido enviar no máximo 10 documentos para cada
pessoa do B.O \
 \
[[935370: RN - Documento Pessoa -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_WRvGAGPdEfCirqqtwbaCiw)

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 10       | Sim      | -        |
| Tipo do  | cor      |          |          |          |  Valores |
| D        | porativo |          |          |          |     fo   |
| ocumento | do Tipo  |          |          |          | rnecidos |
|          | de       |          |          |          |          |
|          | D        |          |          |          |    pelos |
|          | ocumento |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **q  |
|          |          |          |          |          | ualifica |
|          |          |          |          |          | ção-pess |
|          |          |          |          |          | oa-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Tipo de |
|          |          |          |          |          |     Docu |
|          |          |          |          |          | mento\". |
+----------+----------+----------+----------+----------+----------+
| Nome     | Nome do  | Texto    | 100      | Não      |          |
| Prop     | Prop     |          |          |          |          |
| rietário | rietário |          |          |          |          |
|          | d        |          |          |          |          |
|          | eclarado |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Tipo de  | Texto    | 6        | Não      |          |
| Tipo     | Pro      |          |          |          |          |
| Pro      | priedade |          |          |          |          |
| priedade | do       |          |          |          |          |
|          | Docum    |          |          |          |          |
|          | ento(Púb |          |          |          |          |
|          | lico/Par |          |          |          |          |
|          | ticular) |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | (\*1) 16 | Não      | -        |
| do       | do       |          |          |          |    (\*1) |
| D        | D        |          |          |          |     Se o |
| ocumento | ocumento |          |          |          |          |
|          |          |          |          |          |   \"Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Doc  |
|          |          |          |          |          | umento\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     ao   |
|          |          |          |          |          |     d    |
|          |          |          |          |          | ocumento |
|          |          |          |          |          |     CPF: |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |    -   o |
|          |          |          |          |          |          |
|          |          |          |          |          |  tamanho |
|          |          |          |          |          |          |
|          |          |          |          |          |   máximo |
|          |          |          |          |          |          |
|          |          |          |          |          |       do |
|          |          |          |          |          |          |
|          |          |          |          |          |    campo |
|          |          |          |          |          |          |
|          |          |          |          |          |    passa |
|          |          |          |          |          |          |
|          |          |          |          |          |        a |
|          |          |          |          |          |          |
|          |          |          |          |          |      ser |
|          |          |          |          |          |          |
|          |          |          |          |          |       11 |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |  -   CPF |
|          |          |          |          |          |          |
|          |          |          |          |          |     deve |
|          |          |          |          |          |          |
|          |          |          |          |          |      ser |
|          |          |          |          |          |          |
|          |          |          |          |          |   válido |
|          |          |          |          |          |          |
|          |          |          |          |          | -   Não  |
|          |          |          |          |          |     pode |
|          |          |          |          |          |     ter  |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Número |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Doc  |
|          |          |          |          |          | umento\" |
|          |          |          |          |          |     d    |
|          |          |          |          |          | uplicado |
|          |          |          |          |          |     para |
|          |          |          |          |          |     um   |
|          |          |          |          |          |     d    |
|          |          |          |          |          | ocumento |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |  pessoa. |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 15       | Não      |          |
| de       | de       |          |          |          |          |
| Mat      | M        |          |          |          |          |
| rícula/I | atrícula |          |          |          |          |
| nscrição | ou       |          |          |          |          |
|          | I        |          |          |          |          |
|          | nscrição |          |          |          |          |
|          | do       |          |          |          |          |
|          | D        |          |          |          |          |
|          | ocumento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 10       | Não      |          |
| Livro    | do Livro |          |          |          |          |
| Fls.     | de       |          |          |          |          |
|          | Registro |          |          |          |          |
|          | em       |          |          |          |          |
|          | cartório |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 15       | Não      |          |
| A        | de       |          |          |          |          |
| verbação | A        |          |          |          |          |
|          | verbação |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 6        | Não      |          |
| de Série | de Série |          |          |          |          |
|          | do       |          |          |          |          |
|          | D        |          |          |          |          |
|          | ocumento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 6        | Não      |          |
| da Seção | de seção |          |          |          |          |
|          | do       |          |          |          |          |
|          | d        |          |          |          |          |
|          | ocumento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 4        | Não      |          |
| da Zona  | da zona  |          |          |          |          |
|          | do       |          |          |          |          |
|          | d        |          |          |          |          |
|          | ocumento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Órgão    | Nome do  | Texto    | 50       | Não      |          |
| E        | Órgão    |          |          |          |          |
| xpedidor | E        |          |          |          |          |
|          | xpedidor |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 15       | Não      |          |
| Agência  | da       |          |          |          |          |
|          | agência  |          |          |          |          |
|          | do       |          |          |          |          |
|          | d        |          |          |          |          |
|          | ocumento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 15       | Não      |          |
| Conta    | da conta |          |          |          |          |
| Banco    | bancária |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| País     | do País  |          |          |          |  Valores |
| D        | do       |          |          |          |     fo   |
| ocumento | D        |          |          |          | rnecidos |
|          | ocumento |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     P    |
|          |          |          |          |          | aíses\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Sigla    | Texto    | 2        | Não      | -        |
| UF       | E        |          |          |          |  Valores |
| D        | stado do |          |          |          |     fo   |
| ocumento | D        |          |          |          | rnecidos |
|          | ocumento |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Unidades |
|          |          |          |          |          |          |
|          |          |          |          |          |   Federa |
|          |          |          |          |          | tivas\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 11       | Não      | -        |
| M        | do       |          |          |          |  Valores |
| unicípio | M        |          |          |          |     fo   |
| do       | unicípio |          |          |          | rnecidos |
| D        | do       |          |          |          |          |
| ocumento | D        |          |          |          |    pelos |
|          | ocumento |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Munic |
|          |          |          |          |          | ípios\". |
+----------+----------+----------+----------+----------+----------+
| Data de  | Data de  | Texto    | 10       | Não      | -        |
| Admissão | Admissão |          |          |          |  Formato |
|          | do       |          |          |          |     e    |
|          | D        |          |          |          | sperado: |
|          | ocumento |          |          |          |     dd   |
|          |          |          |          |          | /MM/aaaa |
+----------+----------+----------+----------+----------+----------+
| Data     | Data de  | Texto    | 10       | Não      | -        |
| A        | A        |          |          |          |  Formato |
| verbação | verbação |          |          |          |     e    |
|          |          |          |          |          | sperado: |
|          |          |          |          |          |     dd   |
|          |          |          |          |          | /MM/aaaa |
+----------+----------+----------+----------+----------+----------+
| Data de  | Data de  | Texto    | 10       | Não      | -        |
| E        | e        |          |          |          |  Formato |
| xpedição | xpedição |          |          |          |     e    |
|          | do       |          |          |          | sperado: |
|          | d        |          |          |          |     dd   |
|          | ocumento |          |          |          | /MM/aaaa |
+----------+----------+----------+----------+----------+----------+
| Data de  | Data de  | Texto    | 10       | Não      | -        |
| Validade | validade |          |          |          |  Formato |
|          | do       |          |          |          |     e    |
|          | d        |          |          |          | sperado: |
|          | ocumento |          |          |          |     dd   |
|          |          |          |          |          | /MM/aaaa |
+----------+----------+----------+----------+----------+----------+
| Data da  | Data que | Texto    | 10       | Não      | -        |
| Primeira | foi      |          |          |          |  Formato |
| Hab      | obtida a |          |          |          |     e    |
| ilitação | primeira |          |          |          | sperado: |
|          | hab      |          |          |          |     dd   |
|          | ilitação |          |          |          | /MM/aaaa |
+----------+----------+----------+----------+----------+----------+
| C        | C        | Texto    | 5        | Não      | -        |
| ategoria | ategoria |          |          |          |    Campo |
| da       | da       |          |          |          |     deve |
| Hab      | Hab      |          |          |          |     ser  |
| ilitação | ilitação |          |          |          |          |
|          |          |          |          |          |    usado |
|          |          |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     do   |
|          |          |          |          |          |     D    |
|          |          |          |          |          | ocumento |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     a    |
|          |          |          |          |          |     CNH. |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |  Valores |
|          |          |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     *    |
|          |          |          |          |          | *veículo |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Ca   |
|          |          |          |          |          | tegorias |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Hab  |
|          |          |          |          |          | ilitação |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Ve   |
|          |          |          |          |          | ículo\". |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 11       | Não      |          |
| RENAVAM  | Renavam  |          |          |          |          |
|          | do       |          |          |          |          |
|          | d        |          |          |          |          |
|          | ocumento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| Tipo     | Cor      |          |          |          |  Valores |
| Pa       | porativo |          |          |          |     fo   |
| ssaporte | do Tipo  |          |          |          | rnecidos |
|          | de       |          |          |          |          |
|          | Pa       |          |          |          |    pelos |
|          | ssaporte |          |          |          |          |
|          | da       |          |          |          | serviços |
|          | pessoa   |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **q  |
|          |          |          |          |          | ualifica |
|          |          |          |          |          | ção-pess |
|          |          |          |          |          | oa-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Tipo de |
|          |          |          |          |          |          |
|          |          |          |          |          |    Passa |
|          |          |          |          |          | porte\". |
+----------+----------+----------+----------+----------+----------+
| Funda    | D        | Texto    | 200      | Não      |          |
| mentação | escrição |          |          |          |          |
| do       | da       |          |          |          |          |
| d        | f        |          |          |          |          |
| ocumento | undament |          |          |          |          |
|          | ação/adu |          |          |          |          |
|          | lteração |          |          |          |          |
|          | do       |          |          |          |          |
|          | d        |          |          |          |          |
|          | ocumento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| D        | D        | Texto    | 100      | Não      |          |
| escrição | escrição |          |          |          |          |
| D        | do       |          |          |          |          |
| ocumento | D        |          |          |          |          |
|          | ocumento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Indica   | In       | Booleano | 5        | Não      | -   Caso |
| tivo Adu | dicativo |          |          |          |     o    |
| lteração | se o     |          |          |          |          |
|          | d        |          |          |          |    campo |
|          | ocumento |          |          |          |     não  |
|          | a        |          |          |          |     seja |
|          | presenta |          |          |          |          |
|          | sinais   |          |          |          | enviado, |
|          | de       |          |          |          |     o    |
|          | adu      |          |          |          |          |
|          | lteração |          |          |          |  sistema |
|          |          |          |          |          |     con  |
|          |          |          |          |          | siderará |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    campo |
|          |          |          |          |          |     como |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |          |
|          |          |          |          |          |    null. |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Numérico | 12       | Não      |          |
| P        | do       |          |          |          |          |
| IS/PASEP | PIS(     |          |          |          |          |
|          | Programa |          |          |          |          |
|          | de       |          |          |          |          |
|          | in       |          |          |          |          |
|          | tegração |          |          |          |          |
|          | Social)  |          |          |          |          |
+----------+----------+----------+----------+----------+----------+

**Pessoa Física - Endereço(s)**\
Lista de Endereços\
[[935319: RN - Endereço -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX__iQsIGPWEfCirqqtwbaCiw)**Endereço**

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Endereço | In       | Booleano | 5        | Não      | -   Se o |
| P        | dicativo |          |          |          |          |
| rincipal | se é o   |          |          |          |    campo |
|          | endereço |          |          |          |     não  |
|          | P        |          |          |          |     for  |
|          | rincipal |          |          |          |     pre  |
|          |          |          |          |          | enchido: |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |   -   Co |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |          |
|          |          |          |          |          |        o |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |          |
|          |          |          |          |          |   padrão |
|          |          |          |          |          |          |
|          |          |          |          |          |     como |
|          |          |          |          |          |          |
|          |          |          |          |          |    NÃO.  |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| Tipo de  | Tipo de  |          |          |          |  Valores |
| Endereço | Endereço |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **l  |
|          |          |          |          |          | ocalidad |
|          |          |          |          |          | e-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     End  |
|          |          |          |          |          | ereço\". |
+----------+----------+----------+----------+----------+----------+
| CEP      | CEP      | Texto    | 9        | Não      | -        |
|          |          |          |          |          |  Formato |
|          |          |          |          |          |     E    |
|          |          |          |          |          | sperado: |
|          |          |          |          |          |          |
|          |          |          |          |          |  somente |
|          |          |          |          |          |          |
|          |          |          |          |          |  dígitos |
|          |          |          |          |          |     n    |
|          |          |          |          |          | uméricos |
+----------+----------+----------+----------+----------+----------+
| Latitude | Latitude | Numérico | (20,17)  | Não      |          |
|          |          | decimal  |          |          |          |
+----------+----------+----------+----------+----------+----------+
| L        | L        | Numérico | (20,17)  | Não      |          |
| ongitude | ongitude | decimal  |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Lo       | Endereço | Texto    | 200      | Não      |          |
| gradouro | da       |          |          |          |          |
|          | pessoa   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 20       | Não      |          |
|          | do       |          |          |          |          |
|          | endereço |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Com      | Com      | Texto    | 100      | Não      |          |
| plemento | plemento |          |          |          |          |
|          | do       |          |          |          |          |
|          | endereço |          |          |          |          |
|          | da       |          |          |          |          |
|          | pessoa   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| do País  | cor      |          |          |          |  Valores |
|          | porativo |          |          |          |     fo   |
|          | do país  |          |          |          | rnecidos |
|          | de       |          |          |          |          |
|          | endereço |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     P    |
|          |          |          |          |          | aíses\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Sigla da | Texto    | 2        | Não      | -        |
| UF do    | UF do    |          |          |          |  Valores |
| Estado   | endereço |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   UFs\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 11       | Não      | -        |
| do       | cor      |          |          |          |  Valores |
| M        | porativo |          |          |          |     fo   |
| unicípio | para o   |          |          |          | rnecidos |
|          | m        |          |          |          |          |
|          | unicípio |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | localida |
|          |          |          |          |          | de-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Munic |
|          |          |          |          |          | ípios\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 11       | Não      | -        |
| do       | cor      |          |          |          |  Valores |
| Bairro   | porativo |          |          |          |     fo   |
|          | para o   |          |          |          | rnecidos |
|          | bairro   |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **l  |
|          |          |          |          |          | ocalidad |
|          |          |          |          |          | e-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Ba   |
|          |          |          |          |          | irros\". |
+----------+----------+----------+----------+----------+----------+
| Nome do  | Nome do  | Texto    | 100      | Não      |          |
| Bairro   | Bairro   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Nome da  | Nome da  | Texto    | 200      | \*Não    | -   Caso |
| P        | P        |          |          |          |     o    |
| rovíncia | rovíncia |          |          |          |          |
|          |          |          |          |          | \'Código |
|          |          |          |          |          |     do   |
|          |          |          |          |          |          |
|          |          |          |          |          |   País\' |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     for  |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |  Brasil, |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    campo |
|          |          |          |          |          |          |
|          |          |          |          |          |   \'Nome |
|          |          |          |          |          |     da   |
|          |          |          |          |          |     Pro  |
|          |          |          |          |          | víncia\' |
|          |          |          |          |          |     é    |
|          |          |          |          |          |     obri |
|          |          |          |          |          | gatório: |
|          |          |          |          |          |  [[MR3 - |
|          |          |          |          |          |          |
|          |          |          |          |          |    Campo |
|          |          |          |          |          |     Obr  |
|          |          |          |          |          | igatório |
|          |          |          |          |          |     Não  |
|          |          |          |          |          |     In   |
|          |          |          |          |          | formado] |
|          |          |          |          |          | {.underl |
|          |          |          |          |          | ine}](ht |
|          |          |          |          |          | tps://al |
|          |          |          |          |          | m.serpro |
|          |          |          |          |          | /rm/reso |
|          |          |          |          |          | urces/TX |
|          |          |          |          |          | _5eZI8HL |
|          |          |          |          |          | vEfCXc_r |
|          |          |          |          |          | ffzpa6A) |
+----------+----------+----------+----------+----------+----------+

**Pessoa Física - Rede(s) Social(is)**\
    Lista de Redes Sociais\
[[935328: RN - Rede Social -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_QKU_sGPZEfCirqqtwbaCiw)**Rede
Social**

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | \(1\)    | -        |
| Rede     | do Tipo  |          |          | Não      |  Valores |
| Social   | de Rede  |          |          |          |     fo   |
|          | Social   |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     ou   |
|          |          |          |          |          | tros-v3: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Redes |
|          |          |          |          |          |     So   |
|          |          |          |          |          | ciais\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |    \(1\) |
|          |          |          |          |          |     Ou   |
|          |          |          |          |          |          |
|          |          |          |          |          | \'Código |
|          |          |          |          |          |     Rede |
|          |          |          |          |          |          |
|          |          |          |          |          | Social\' |
|          |          |          |          |          |     ou   |
|          |          |          |          |          |          |
|          |          |          |          |          |  \'Outra |
|          |          |          |          |          |     Rede |
|          |          |          |          |          |          |
|          |          |          |          |          | Social\' |
|          |          |          |          |          |          |
|          |          |          |          |          |    devem |
|          |          |          |          |          |     ser  |
|          |          |          |          |          |     inf  |
|          |          |          |          |          | ormados. |
+----------+----------+----------+----------+----------+----------+
| Outra    | Caso não | Texto    | 50       | \(1\)    | Opção    |
| Rede     | exista a |          |          | Não      | para     |
| Social   | rede     |          |          |          | quando o |
|          | social   |          |          |          | código   |
|          | em       |          |          |          | ident    |
|          | código   |          |          |          | ificador |
|          | de rede  |          |          |          | da rede  |
|          | social   |          |          |          | social   |
|          |          |          |          |          | for      |
|          |          |          |          |          | igual a  |
|          |          |          |          |          | \        |
|          |          |          |          |          | "Outro\" |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |    \(1\) |
|          |          |          |          |          |     Ou   |
|          |          |          |          |          |          |
|          |          |          |          |          | \'Código |
|          |          |          |          |          |     Rede |
|          |          |          |          |          |          |
|          |          |          |          |          | Social\' |
|          |          |          |          |          |     ou   |
|          |          |          |          |          |          |
|          |          |          |          |          |  \'Outra |
|          |          |          |          |          |     Rede |
|          |          |          |          |          |          |
|          |          |          |          |          | Social\' |
|          |          |          |          |          |          |
|          |          |          |          |          |    devem |
|          |          |          |          |          |     ser  |
|          |          |          |          |          |     in   |
|          |          |          |          |          | formados |
|          |          |          |          |          |          |
|          |          |          |          |          | -   Caso |
|          |          |          |          |          |          |
|          |          |          |          |          | \'Código |
|          |          |          |          |          |     Rede |
|          |          |          |          |          |          |
|          |          |          |          |          | Social\' |
|          |          |          |          |          |     seja |
|          |          |          |          |          |     in   |
|          |          |          |          |          | formado, |
|          |          |          |          |          |     d    |
|          |          |          |          |          | esprezar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |  \'Outra |
|          |          |          |          |          |     Rede |
|          |          |          |          |          |     S    |
|          |          |          |          |          | ocial\'. |
+----------+----------+----------+----------+----------+----------+
| Nome de  | Nome do  | Texto    | 50       | Sim      |          |
| Usuário  | Usuário  |          |          |          |          |
| na       | na Rede  |          |          |          |          |
| Pl       | Social   |          |          |          |          |
| ataforma |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+

**Pessoa Física - Telefone(s)**\
** **   Lista de Telefones\
[[935359: RN - Telefone -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_ooWYUGPaEfCirqqtwbaCiw)**Telefone**

  --------------------------- ----------------------------------------------------------------- ---------- ------------- ----------------- -----------------------------------------------------------------------------------------------------------------------------------
  **Nome do Campo**           **Descrição**                                                     **Tipo**   **Tamanho**   **Obrigatório**   **Regras de Recebimento**
  DDI do Telefone             DDI(Discagem Direta Internacional)                                Texto      3             Não               Formato esperado: somente números
  Código do País              Código do País                                                    Texto      6             Não               Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: \"Listar Países\"
  DDD do Telefone             DDD(Discagem Direta a Distância)                                  Texto      3             Não               Formato esperado: somente números
  Número do Telefone          Número do Telefone da Pessoa                                      Texto      20            Sim               Formato esperado: somente números
  Indicativo WhatsApp         Indicativo se o telefone informado é de um contato com WhatsApp   Booleano   5             Não                
  Código Tipo de Telefone     Código Corporativo do Tipo de Telefone(Comercial, celular..)      Texto      6             Não               Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - qualificação-pessoa-v3: \"Listar Tipo de Telefone\".
  Nome do Contato de Recado   Texto para descrever contato que atende pelo telefone             Texto      50            Não                
  --------------------------- ----------------------------------------------------------------- ---------- ------------- ----------------- -----------------------------------------------------------------------------------------------------------------------------------

**Pessoa Física - Email(s)**\
   Lista de E-mails\
[[935367: RN - Email -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_AuHzkGPdEfCirqqtwbaCiw)

  ------------------- --------------- ---------- ------------- ----------------- ---------------------------
  **Nome do Campo**   **Descrição**   **Tipo**   **Tamanho**   **Obrigatório**   **Regras de Recebimento**
  E-mail              E-mail          Texto      256           Não                
  ------------------- --------------- ---------- ------------- ----------------- ---------------------------

**Pessoa Física - Filho(s)**\
  Lista de Filhos\
[[1009493: RN - Filhos -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_o0hOYHFgEfG2V-dwy-uU8g)

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Nome     | Nome do  | Texto    | 200      | Não      |          |
| Filho    | Filho    |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      |          |
| Sexo     | cor      |          |          |          |          |
|          | porativo |          |          |          |          |
|          | do sexo  |          |          |          |          |
|          | do filho |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Idade    | Idade do | Numérico | 3        | Não      | -        |
|          | filho    |          |          |          |    Idade |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     pode |
|          |          |          |          |          |     ser  |
|          |          |          |          |          |          |
|          |          |          |          |          | negativa |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Booleano | 5        | Não      |          |
| dicativo | dicativo |          |          |          |          |
| Idade    | se a     |          |          |          |          |
| Ap       | idade    |          |          |          |          |
| roximada | i        |          |          |          |          |
|          | nformada |          |          |          |          |
|          | é        |          |          |          |          |
|          | ap       |          |          |          |          |
|          | roximada |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Data de  | Data de  | Texto    | 10       | Não      | -        |
| Na       | na       |          |          |          |  Formato |
| scimento | scimento |          |          |          |     e    |
|          | do Filho |          |          |          | sperado: |
|          |          |          |          |          |          |
|          |          |          |          |          |   \'dd/m |
|          |          |          |          |          | m/aaaa\' |
+----------+----------+----------+----------+----------+----------+

 \
**Objetos**\
Para cada objeto declarado, deve-se respeitar os campos obrigatórios
quando for informado.\
**Cada Objeto terá o seguinte cabeçalho:**\
[[935431: RN - Tipo Objeto -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_6tDFUGQFEfCirqqtwbaCiw)**Tipo
Objeto**

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Ident    | Número   | Numérico | 4        | Sim      | -   Se   |
| ificação | que      | Inteiro  |          |          |          |
| do       | id       |          |          |          |  existir |
| Objeto   | entifica |          |          |          |          |
|          | o objeto |          |          |          |    Ident |
|          |          |          |          |          | ificador |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Objeto |
|          |          |          |          |          |     Du   |
|          |          |          |          |          | plicado, |
|          |          |          |          |          |          |
|          |          |          |          |          | deve-se: |
|          |          |          |          |          |          |
|          |          |          |          |          |     -    |
|          |          |          |          |          |  Recusar |
|          |          |          |          |          |          |
|          |          |          |          |          |        o |
|          |          |          |          |          |          |
|          |          |          |          |          |      B.O |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |    -   A |
|          |          |          |          |          | dicionar |
|          |          |          |          |          |          |
|          |          |          |          |          |        o |
|          |          |          |          |          |          |
|          |          |          |          |          |   motivo |
|          |          |          |          |          |          |
|          |          |          |          |          |       da |
|          |          |          |          |          |          |
|          |          |          |          |          |   recusa |
|          |          |          |          |          |          |
|          |          |          |          |          |        a |
|          |          |          |          |          |          |
|          |          |          |          |          | seguinte |
|          |          |          |          |          |          |
|          |          |          |          |          |       me |
|          |          |          |          |          | nsagem:[ |
|          |          |          |          |          | [936967: |
|          |          |          |          |          |          |
|          |          |          |          |          |      MSG |
|          |          |          |          |          |          |
|          |          |          |          |          |     ERRO |
|          |          |          |          |          |          |
|          |          |          |          |          |   Objeto |
|          |          |          |          |          |          |
|          |          |          |          |          |      dup |
|          |          |          |          |          | licado]{ |
|          |          |          |          |          | .underli |
|          |          |          |          |          | ne}](htt |
|          |          |          |          |          | ps://alm |
|          |          |          |          |          | .serpro/ |
|          |          |          |          |          | rm/resou |
|          |          |          |          |          | rces/TX_ |
|          |          |          |          |          | FYmgMGyl |
|          |          |          |          |          | EfCXc_rf |
|          |          |          |          |          | fzpa6A)E |
|          |          |          |          |          | 019:\<no |
|          |          |          |          |          | me\_do\_ |
|          |          |          |          |          | campo\>: |
|          |          |          |          |          |          |
|          |          |          |          |          |        O |
|          |          |          |          |          | bjeto du |
|          |          |          |          |          | plicado. |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 10       | Sim      | -        |
| Subgrupo | cor      |          |          |          |  Valores |
| Objeto   | porativo |          |          |          |     fo   |
|          | do       |          |          |          | rnecidos |
|          | subgrupo |          |          |          |          |
|          | objeto   |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |          |
|          |          |          |          |          |   **obje |
|          |          |          |          |          | to-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     S    |
|          |          |          |          |          | ubgrupos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     O    |
|          |          |          |          |          | bjeto\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Có       | Texto    | 6        | Sim      | -        |
| Grupo    | digo Cor |          |          |          |  Valores |
| Unidade  | porativo |          |          |          |     fo   |
| de       | do Grupo |          |          |          | rnecidos |
| Medida   | de       |          |          |          |          |
|          | Unidade  |          |          |          |    pelos |
|          | de       |          |          |          |          |
|          | medida   |          |          |          | serviços |
|          | do       |          |          |          |     dis  |
|          | objeto   |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |          |
|          |          |          |          |          |   **obje |
|          |          |          |          |          | to-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Grupos |
|          |          |          |          |          |          |
|          |          |          |          |          | Unidades |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     M    |
|          |          |          |          |          | edida\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Sim      | -        |
| Unidade  | cor      |          |          |          |  Valores |
| de       | porativo |          |          |          |     fo   |
| Medida   | da       |          |          |          | rnecidos |
|          | Unidade  |          |          |          |          |
|          | de       |          |          |          |    pelos |
|          | Medida   |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |          |
|          |          |          |          |          |   **obje |
|          |          |          |          |          | to-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Unidades |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     M    |
|          |          |          |          |          | edida\". |
+----------+----------+----------+----------+----------+----------+
| Qu       | Qu       | Numérico | (12,3)   | Sim      |          |
| antidade | antidade | decimal  |          |          |          |
| do       | do       |          |          |          |          |
| Objeto   | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Lista    | Lista de | Lista de | \-       | Não      |          |
| Qual     | Qual     | Qual     |          |          |          |
| ificação | ificação | ificação |          |          |          |
| Objeto   | do       |          |          |          |          |
|          | Objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+

**Segue estrutura para identificação da qualificação de um objeto:**\
| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Situação do Objeto | Situação do objeto (Apreendido, Danificado, Destruído...) | Texto | 6 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **objeto-v3**: "Listar Tipos de Qualificação de Objeto". |
| Valor Estimado do Objeto | Valor Estimado do(s) objeto(s) apreendido(s) em reais | Numérico | (11,2) | Não | |
| Data de Apreensão | Data de Apreensão ou Recuperação do objeto | Texto | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Endereço | Endereço onde se encontra o objeto | Texto | 200 | Não | |
| Número | Número do endereço onde se encontra o objeto | Texto | 20 | Não | |
| Código Bairro | Código Corporativo do Bairro onde se encontra o objeto | Texto | 12 | Não | - Se código bairro for informado, o "Nome Bairro de Apreensão" deve ser desprezado.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Bairros". |
| Nome Bairro | Nome Bairro em que o objeto está | Texto | 100 | Não | |
| Complemento do Endereço | Complemento do Endereço | Texto | 100 | Não | |
| CEP de Apreensão | CEP | Texto | 9 | Não | - Formato esperado: Somente dígitos numéricos |
| Código do Município | Código do Município de Apreensão | Texto | 11 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios". |
| Código UF | UF | Texto | 2 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar UFs". |
| Código País | Código do país | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Países". |
| Nome Delegacia | Nome da Delegacia onde objeto está | Texto | 150 | Não | |

**Todo objeto será enviado em uma das seguintes estruturas:**\
[[1009727: RN - Objeto Geral -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_9VEYwHO6EfG2V-dwy-uU8g)**Objetos**

+----------+----------+----------+----------+----------+----------+
| **A      |          |          |          |          |          |
| tributos |          |          |          |          |          |
| Gerais   |          |          |          |          |          |
| do       |          |          |          |          |          |
| Objeto** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| D        | D        | Texto    | 100      | Não      |          |
| escrição | escrição |          |          |          |          |
| do       | da       |          |          |          |          |
| Objeto   | ident    |          |          |          |          |
|          | ificação |          |          |          |          |
|          | do       |          |          |          |          |
|          | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Bem      | D        | Texto    | 100      | Não      |          |
| Objeto   | escrição |          |          |          |          |
|          | do       |          |          |          |          |
|          | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 100      | Não      |          |
| de Série | de série |          |          |          |          |
|          | do       |          |          |          |          |
|          | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Marca do | Marca do | Texto    | 100      | Não      |          |
| Objeto   | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Modelo   | Modelo   | Texto    | 100      | Não      |          |
| do       | do       |          |          |          |          |
| Objeto   | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Cor do   | Cor do   | Texto    | 100      | Não      |          |
| Objeto   | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| D        | D        | Texto    | 100      | Não      |          |
| escrição | escrição |          |          |          |          |
| do       | do       |          |          |          |          |
| Ac       | ac       |          |          |          |          |
| abamento | abamento |          |          |          |          |
| do       | do       |          |          |          |          |
| Objeto   | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Estado   | Estado   | Texto    | 50       | Não      |          |
| de Uso   | de uso   |          |          |          |          |
|          | do       |          |          |          |          |
|          | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Numérico | 6        | Não      | -        |
| De       | da       |          |          |          |  Valores |
| stinação | De       |          |          |          |     fo   |
| Uso      | stinação |          |          |          | rnecidos |
|          | do       |          |          |          |          |
|          | Objeto.  |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Des  |
|          |          |          |          |          | tinações |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Uso\". |
+----------+----------+----------+----------+----------+----------+
| Valor    | Valor    | Numérico | (11,2)   | Não      |          |
| Estimado | estimado | Decimal  |          |          |          |
| do       | do       |          |          |          |          |
| Objeto   | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Numérico | 6        | Não      | -        |
| Tipo de  | do Tipo  |          |          |          |  Valores |
| Fa       | Fa       |          |          |          |     fo   |
| bricação | bricação |          |          |          | rnecidos |
|          | do       |          |          |          |          |
|          | Objeto.  |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Fabri |
|          |          |          |          |          | cação\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Numérico | 6        | Não      | -        |
| do País  | do País  |          |          |          |  Valores |
|          | de       |          |          |          |     fo   |
|          | fa       |          |          |          | rnecidos |
|          | bricação |          |          |          |          |
|          | do       |          |          |          |    pelos |
|          | objeto.  |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     P    |
|          |          |          |          |          | aíses\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 2        | Não      | -        |
| da UF    | da       |          |          |          |  Valores |
|          | Unidade  |          |          |          |     fo   |
|          | Fed      |          |          |          | rnecidos |
|          | erativa. |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   UFs\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Numérico | 11       | Não      | -        |
| do       | do       |          |          |          |  Valores |
| M        | M        |          |          |          |     fo   |
| unicípio | unicípio |          |          |          | rnecidos |
|          | de       |          |          |          |          |
|          | fa       |          |          |          |    pelos |
|          | bricação |          |          |          |          |
|          | do       |          |          |          | serviços |
|          | Objeto.  |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Munic |
|          |          |          |          |          | ípios\". |
+----------+----------+----------+----------+----------+----------+
| Nome     | Nome da  | Texto    | 200      | \* Não   | \* Nome  |
| P        | p        |          |          |          | P        |
| rovíncia | rovíncia |          |          |          | rovíncia |
| do       | de       |          |          |          | é        |
| Objeto   | fa       |          |          |          | obr      |
|          | bricação |          |          |          | igatório |
|          | do       |          |          |          | quando o |
|          | Objeto,  |          |          |          | país não |
|          | caso o   |          |          |          | é o      |
|          | país não |          |          |          | B        |
|          | seja o   |          |          |          | rasil​​​ |
|          | Brasil   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| CNPJ     | CNPJ da  | Texto    | 14       | Não      | -   CNPJ |
| Nota     | nota     |          |          |          |     deve |
| Fiscal   | fiscal   |          |          |          |     ser  |
|          | de       |          |          |          |          |
|          | compra   |          |          |          |   válido |
|          | do       |          |          |          |          |
|          | objeto.  |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| CPF Nota | CPF da   | Texto    | 11       | Não      | -   CPF  |
| Fiscal   | nota     |          |          |          |     deve |
|          | fiscal   |          |          |          |     ser  |
|          | de       |          |          |          |          |
|          | compra   |          |          |          |   válido |
|          | do       |          |          |          |          |
|          | objeto.  |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Nome     | Nome do  | Texto    | 100      | Não      |          |
| Prop     | prop     |          |          |          |          |
| rietário | rietário |          |          |          |          |
| Nota     | descrito |          |          |          |          |
| Fiscal   | na nota  |          |          |          |          |
|          | fiscal   |          |          |          |          |
|          | de       |          |          |          |          |
|          | compra   |          |          |          |          |
|          | do       |          |          |          |          |
|          | objeto.  |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 20       | Não      |          |
| D        | do       |          |          |          |          |
| ocumento | d        |          |          |          |          |
| Pro      | ocumento |          |          |          |          |
| priedade | da       |          |          |          |          |
|          | prop     |          |          |          |          |
|          | riedade. |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| In       | I        | Booleano | 1        | Não      | -   Se o |
| dicativo | ndicador |          |          |          |          |
| de       | se há    |          |          |          |    valor |
| Adu      | adu      |          |          |          |     não  |
| lteração | lteração |          |          |          |     for  |
|          | no       |          |          |          |     in   |
|          | objeto.  |          |          |          | formado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     como |
|          |          |          |          |          |     NÃO. |
+----------+----------+----------+----------+----------+----------+
| D        | D        | Texto    | 200      | Não      |          |
| escrição | escrição |          |          |          |          |
| Adu      | do tipo  |          |          |          |          |
| lteração | de       |          |          |          |          |
|          | adu      |          |          |          |          |
|          | lteração |          |          |          |          |
|          | do       |          |          |          |          |
|          | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| In       | I        | Booleano | 1        | Não      | -   Se o |
| dicativo | ndicador |          |          |          |          |
| de       | se a     |          |          |          |    valor |
| Adu      | marca    |          |          |          |     não  |
| lteração | foi      |          |          |          |     for  |
| de Marca | adu      |          |          |          |     in   |
|          | lterada. |          |          |          | formado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     como |
|          |          |          |          |          |     NÃO. |
+----------+----------+----------+----------+----------+----------+
| I        | I        | Booleano | 1        | Não      | -   Se o |
| ndicador | ndicador |          |          |          |          |
| de       | se o     |          |          |          |    valor |
| Adu      | modelo   |          |          |          |     não  |
| lteração | foi      |          |          |          |     for  |
| de       | ad       |          |          |          |     in   |
| Modelo   | ulterado |          |          |          | formado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     como |
|          |          |          |          |          |     NÃO. |
+----------+----------+----------+----------+----------+----------+
| I        | I        | Booleano | 1        | Não      | -   Se o |
| ndicador | ndicador |          |          |          |          |
| de       | se a cor |          |          |          |    valor |
| Adu      | foi      |          |          |          |     não  |
| lteração | ad       |          |          |          |     for  |
| de Cor   | ulterada |          |          |          |     in   |
|          |          |          |          |          | formado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     como |
|          |          |          |          |          |     NÃO. |
+----------+----------+----------+----------+----------+----------+
| Qu       | Qu       | Numérico | (11,3)   | Sim      |          |
| antidade | antidade | Decimal  |          |          |          |
| do       | do       |          |          |          |          |
| Objeto   | objeto   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Numérico | 6        | Não      | Valores  |
| Tipo     | do Tipo  |          |          |          | fo       |
| Loc      | da       |          |          |          | rnecidos |
| alização | Loc      |          |          |          | pelos    |
| do       | alização |          |          |          | serviços |
| Objeto   | onde o   |          |          |          | dis      |
|          | objeto   |          |          |          | poníveis |
|          | se       |          |          |          | no       |
|          | en       |          |          |          | Sinesp   |
|          | contra.  |          |          |          | Dados    |
|          |          |          |          |          | Corpo    |
|          |          |          |          |          | rativos: |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          | Loca     |
|          |          |          |          |          | lizações |
|          |          |          |          |          | O        |
|          |          |          |          |          | bjeto\". |
+----------+----------+----------+----------+----------+----------+
| D        | D        | Texto    | 100      | Não      |          |
| escrição | escrição |          |          |          |          |
| da       | da       |          |          |          |          |
| Loc      | loc      |          |          |          |          |
| alização | alização |          |          |          |          |
|          | onde o   |          |          |          |          |
|          | objeto   |          |          |          |          |
|          | se       |          |          |          |          |
|          | e        |          |          |          |          |
|          | ncontra. |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Numérico | 6        | Não      | Valores  |
| Unidade  | do       |          |          |          | fo       |
| Medida   | Unidade  |          |          |          | rnecidos |
|          | Medida   |          |          |          | pelos    |
|          |          |          |          |          | serviços |
|          |          |          |          |          | dis      |
|          |          |          |          |          | poníveis |
|          |          |          |          |          | no       |
|          |          |          |          |          | Sinesp   |
|          |          |          |          |          | Dados    |
|          |          |          |          |          | Corpo    |
|          |          |          |          |          | rativos: |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          | Unidades |
|          |          |          |          |          | de       |
|          |          |          |          |          | M        |
|          |          |          |          |          | edida\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Loc      | Numérico | 6        | Não      | -        |
| Loc      | alização |          |          |          |  Valores |
| alização | do       |          |          |          |     fo   |
| do       | objeto   |          |          |          | rnecidos |
| Objeto   | de       |          |          |          |          |
|          | acordo   |          |          |          |    pelos |
|          | com dado |          |          |          |          |
|          | cor      |          |          |          | serviços |
|          | porativo |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Loca |
|          |          |          |          |          | lizações |
|          |          |          |          |          |     O    |
|          |          |          |          |          | bjeto\". |
+----------+----------+----------+----------+----------+----------+

[[1009627: RN - Objeto Arma -
| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Arma | Código da Arma de Dados Corporativos. | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Armas".<br>- Caso "Código Arma" seja informado e seja válido, os campos de "Dados Descritivos do Tipo de Arma" que conflitarem serão desprezados. |
| Código Marca | Código da marca da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Marcas Arma". |
| Descrição Marca | Nome da Marca da Mala, caso não especificado em Código Marca. | Texto | 100 | Não | |
| Código Calibre | Código Corporativo com o calibre da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Calibre Arma". |
| Descrição Calibre | Descrição do calibre da arma | Texto | 30 | Não | |
| Código Acabamento | Código do acabamento da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Armas Acabamento". |
| Descrição Acabamento | Descrição do acabamento da arma | Texto | 100 | Não | |
| Código Modelo | Código do modelo da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Armas Modelo". |
| Descrição Modelo | Nome do modelo da arma | Texto | 30 | Não | |
| Código Espécie | Código da espécie da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Armas Espécies". |
| Descrição Espécie | Descrição da espécie da arma | Texto | 30 | Não | |
| Código Alma | Código corporativo da alma da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Armas Alma". |
| Descrição Alma | Descrição da Alma da arma | Texto | 30 | Não | |
| Código Funcionamento Arma | Código para o tipo de funcionamento da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **arma-v3**: "Listar Armas Funcionamento". |
| Código Sentido Arma | Código Corporativo do sentido da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Armas Sentido". |
| Código Carregamento | Código do carregamento da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Sistemas de Carregamento de Arma". |
| Descrição Carregamento | Descrição do carregamento da arma | Texto | 100 | Não | |
| Quantidade de Canos | Quantidade de canos | Numérico | 6 | Não | - Formato esperado: somente números<br>- Número não pode ser negativo |
| Quantidade de Tiros | Quantidade de tiros da arma | Numérico | 6 | Não | - Formato esperado: somente números<br>- Número não pode ser negativo |
| Quantidade de Raias | Quantidade de raias da arma | Numérico | 6 | Não | - Formato esperado: somente números<br>- Número não pode ser negativo |
| Descrição do Comprimento do Cano | Descrição do comprimento do cano | Texto | 50 | Não | |
| Código do Tipo de Uso da Arma | Código do tipo de uso da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Tipo Uso Arma Munição". |
| Indicativo Arma Artesanal | Indicativo se a arma é de fabricação artesanal | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Número da arma | Número da arma | Texto | 50 | Não | |
| Número SINARM | Número do Sinarm da arma | Texto | 50 | Não | |
| Descrição da Adulteração da Arma | Descrição da modificação feita na arma | Texto | 200 | Não | |
| Indicativo Adulterado Calibre | Indicativo de adulteração do calibre | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Adulterado Fabricação | Indicativo de adulteração de fabricação | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Adulterado Número da Arma | Indicativo de adulteração no número da arma | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Adulterado SINARM | Indicativo de adulteração do número SINARM | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Adulterado Quantidade de Tiros | Indicativo de adulteração da quantidade de tiros | Booleano | 5 | Não | |
| Indicativo Adulterado Quantidade de Canos | Indicativo de adulteração da quantidade de canos | Booleano | 5 | Não | |
| Indicativo Adulterado Carregamento | Indicativo de adulteração do carregamento | Booleano | 5 | Não | |
| Indicativo Adulterado Acabamento | Indicativo de adulteração do acabamento | Booleano | 5 | Não | |
| Indicativo Adulterado Comprimento do Cano | Indicativo de comprimento do cano | Booleano | 5 | Não | |

## Objeto Documento

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Tipo de Propriedade Documento | Código do Tipo de Documento. | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **objeto-v3**: "Listar Tipos de Propriedade". |
| Indicativo de Adulteração | Indicador se há adulteração no objeto. | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Descrição Adulteração | Descrição do tipo de adulteração do objeto | Texto | 200 | Não | |
| Descrição Fundamentação | Descrição da fundamentação | Texto | 200 | Não | |
| Nome do Proprietário do Documento | Nome do Proprietário do documento | Texto | 100 | Não | |
| Número do Documento | Número do documento | Texto | 16 | Não | |
| Número de Matrícula | Número de Matrícula ou Inscrição do Documento | Texto | 15 | Não | |
| Número Livro Fls. | Livro de Registro em cartório | Texto | 10 | Não | |
| Número Série | Número de série | Texto | 6 | Não | |
| Número Seção | Número de seção do documento | Texto | 6 | Não | - Formato esperado: somente números |
| Número da Zona | Número da zona do documento | Texto | 4 | Não | - Formato esperado: somente números |
| Órgão Expedidor | Nome do Órgão Expedidor | Texto | 50 | Não | |
| Número Agência | Agência do documento | Texto | 15 | Não | |
| Número Conta Banco | Número da conta do banco. | Texto | 15 | Não | |
| Número RENAVAM | Número RENAVAM | Texto | 11 | Não | |
| Data de Expedição | Data de expedição do documento | Texto | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Data de Validade | Data de validade do documento | Texto | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Data da Primeira Habilitação/Admissão | Data que foi obtida a primeira habilitação ou admissão | Texto | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Código Categoria da Habilitação | Categorias da Habilitação | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **veiculo-v3**: "Listar Categorias de Habilitação de Veículo". |
| Número Averbação | Número de Averbação do divórcio | Texto | 15 | Não | |
| Data Averbação | Data de Averbação do divórcio | Data | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Número PIS/PASEP | Número do PIS (Programa de integração Social) | Texto | 12 | Não | - Formato esperado: somente números |
| Código Tipo de Passaporte | Código do Tipo de Passaporte da pessoa | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Tipo de Passaporte". |
| Código do País | Código do País de fabricação do objeto. | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Países". |
| Código da UF | Código da Unidade Federativa. | Texto | 2 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar UFs". |
| Código do Município | Código do Município de fabricação do Objeto. | Texto | 11 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios". |

## Objeto Munição

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Situação de Disparo do Projétil | Código situação de disparo do projétil | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Situação de Disparo Munição". |
| Indicativo de Adulteração | Indicador se há adulteração no objeto. | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Descrição Adulteração | Descrição do tipo de adulteração do objeto | Texto | 200 | Não | |
| Código Tipo de Uso da Arma | Código do tipo de uso da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Tipo Uso Arma Munição". |
| Código Calibre da Arma | Código Corporativo com o calibre da munição da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Calibre Arma". |

## Objeto Veículo
| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Destinação de Uso | Código corporativo da Destinação de Uso do Veiculo. | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **objeto-v3**: "Listar Destinações de Uso". |
| Indicativo de Adulteração | Indicador se há adulteração no objeto. | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Carroceria Adulterada | Indicação se carroceria foi adulterada | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Adulterado Lacre | Indicação se Lacre foi adulterado | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Adulterado Motor | Indicação se motor foi adulterado | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Adulterado Chassi | Indicação se chassi foi adulterado | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Adulterado Placa | Indicação se placa foi adulterada | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Código do Município | Código do Município de fabricação do Objeto. | Texto | 11 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios". |
| Código da UF | Código da Unidade Federativa. | Texto | 2 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar UFs". |
| Ano de Fabricação | Ano de Fabricação do Veículo | Numérico | 4 | Não | Formato esperado: somente números |
| Ano do Modelo | Ano do Modelo do Veículo | Numérico | 4 | Não | Formato esperado: somente números |
| Chassi do Veículo | Chassi do Veículo | Texto | 100 | Não | |
| Cor do Veículo | Cor do veículo | Texto | 30 | Não | |
| Descrição Adulteração | Descrição do tipo de adulteração do objeto | Texto | 200 | Não | |
| Nome do Proprietário do Veículo | Nome do Proprietário do veículo | Texto | 100 | Não | |
| Marca do Veículo | Marca do veículo | Texto | 100 | Não | |
| Modelo do Veículo | Modelo do veículo | Texto | 100 | Não | |
| Número da Carroceria | Número da carroceria | Texto | 100 | Não | |
| Número do Lacre | Número do lacre | Texto | 100 | Não | |
| Número do Motor | Número do motor | Texto | 100 | Não | |
| Placa do Veículo | Placa do veículo | Texto | 8 | Não | |
| Número RENAVAM | Número RENAVAM | Texto | 11 | Não | |

## Objeto Droga

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Descrição Aparência | Descrição da Aparência da droga | Texto | 30 | Não | |
| Descrição Embalagem | Descrição da Embalagem da droga | Texto | 30 | Não | |
| Data Validade da Droga | Data de Validade da Droga | Data | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Tipo de Embalagem da Droga | Código do Tipo Embalagem da Droga. | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **objeto-v3**: "Listar Tipos de Embalagem". |
| Lote da Droga | Descrição do Lote da Droga | Texto | 50 | Não | |

## Objeto Celular

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Número Celular | Número da linha do celular | Texto | 20 | Não | Formato esperado: somente números |
| Nome Operadora | Nome da Operadora da linha | Texto | 100 | Não | |
| IMEI | Número IMEI principal do telefone | Texto | 20 | Não | Formato esperado: somente números |
| IMEI 2 | Número IMEI 2 | Texto | 20 | Não | Formato esperado: somente números |
| IMEI 3 | Número IMEI 3 | Texto | 20 | Não | Formato esperado: somente números |
| IMEI 4 | Número IMEI 4 | Texto | 20 | Não | Formato esperado: somente números |
| macAdress | Número MAC do celular | Texto | 17 | Não | Formato esperado: XX:XX:XX:XX:XX:XX |

## Objeto Moeda

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Qualificação da Moeda | Código corporativo para qualificação da moeda (circulante, falsificada, inexistente, não circulante). | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **objeto-v3**: "Listar Tipos de Qualificação de Moeda". |
| Descrição da Qualificação | Descrição da qualificação | Texto | 21 | Não | |
| Valor da Face | Valor da face da moeda | Numérico decimal | (11,2) | Não | |
| Valor Total | Valor total da moeda | Numérico decimal | (11,2) | Não | |
| Quantidade | Quantidade de moeda | Numérico | 9 | Não | - Formato esperado: somente números |
| Número de Série Inicial | Número de Série Inicial | Texto | 50 | Não | |
| Número de Série Final | Número de Série Final | Texto | 50 | Não | |

​​​​​​## Objeto Embarcação

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Porto de Registro | Nome do porto registro | Texto | 100 | Não | |
| Número de Registro | Número de registro da embarcação | Texto | 10 | Não | |
| Classificação | Classificação da embarcação | Texto | 10 | Não | |
| Código do Município | Código do Município de fabricação do Objeto. | Texto | 11 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios". |
| Código da UF | Código da Unidade Federativa. | Texto | 2 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar UFs". |
| Nome Província do Objeto | Nome da província de fabricação do Objeto, caso o país não seja o Brasil | Texto | 200 | *(1) Não | *(1) Nome Província é obrigatório quando o país não é o Brasil |

**Dados Utilizados pelos Bombeiros**\
**Informar Órgão(s) de Apoio na Operação**\
\
[[934048: RN - Órgãos de Apoio(Bombeiros) -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_XsOT8Fy_EfCirqqtwbaCiw)**Informar
Órgão de Apoio**

  --------------------- ------------------------------------------------------------------------------------------- ---------- ------------- ----------------- ---------------------------
  **Campo**             **Descrição**                                                                               **Tipo**   **Tamanho**   **Obrigatório**   **Regras de Recebimento**
  Tipo de Jurisdição    Campo texto para descrição dos órgão, tipo e jurisdição que fizeram parte do atendimento.   Texto      250           Não               
  Nome do Responsável   Campo texto para inserção dos nomes dos responsáveis pelo atendimento.                      Texto      250           Não                
  Total de pessoas      Total de Pessoas do órgão de apoio.                                                         Numérico   4             Não                
  Total de veículos     Total de Veículos do órgão de apoio.                                                        Numérico   4             Não                
  Ação do órgão         Campo texto para descrição das ações dos órgãos de apoio que auxiliaram no atendimento.     Texto      500           Não                
  --------------------- ------------------------------------------------------------------------------------------- ---------- ------------- ----------------- ---------------------------

 \
**Campos de Descrição do Evento do B.O**\
[[934615: RN - Incêndio/Explosão -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_D_4xQGCmEfCirqqtwbaCiw)**Incêndio/Explosão**

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Tipo de  | Texto    | 5        | Sim      | -        |
| I        | Incêndio |          |          |          |  Valores |
| ncêndio/ | ou       |          |          |          |     fo   |
| Explosão | explosão |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **   |
|          |          |          |          |          | incêndio |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Inc  |
|          |          |          |          |          | êndio\". |
+----------+----------+----------+----------+----------+----------+
| Subtipo  | Subtipo  | Texto    | 5        | Não      | -        |
| de       | de       |          |          |          |  Valores |
| Incêndio | Incêndio |          |          |          |     fo   |
| ou       | ou       |          |          |          | rnecidos |
| Explosão | explosão |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     - *  |
|          |          |          |          |          | *incêndi |
|          |          |          |          |          | os-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |  Subtipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Incê |
|          |          |          |          |          | ndio/Exp |
|          |          |          |          |          | losão\". |
+----------+----------+----------+----------+----------+----------+
| **Tempo  |          |          |          |          |          |
| de       |          |          |          |          |          |
| Op       |          |          |          |          |          |
| eração** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Data     | Data do  | Te       | 10       | Sim      | -        |
| Início   | início   | xto-Data |          |          |  Formato |
| Incêndio | do       |          |          |          |     e    |
|          | incêndio |          |          |          | sperado: |
|          |          |          |          |          |     dd   |
|          |          |          |          |          | /MM/aaaa |
+----------+----------+----------+----------+----------+----------+
| Hora     | Hora do  | Te       | 7        | Não      | -        |
| Início   | início   | xto-Hora |          |          |  Formato |
| Incêndio | do       |          |          |          |     e    |
|          | incêndio |          |          |          | sperado: |
|          |          |          |          |          |          |
|          |          |          |          |          | HH:MM:ss |
+----------+----------+----------+----------+----------+----------+
| Data     | Data da  | Te       | 10       | Sim      | -        |
| Extinção | Extinção | xto-Data |          |          |  Formato |
| do       | do       |          |          |          |     e    |
| Incêndio | Incêndio |          |          |          | sperado: |
|          |          |          |          |          |     dd   |
|          |          |          |          |          | /MM/aaaa |
+----------+----------+----------+----------+----------+----------+
| Hora     | Hora da  | Te       | 7        | Não      | -        |
| Extinção | Extinção | xto-Hora |          |          |  Formato |
| do       | do       |          |          |          |     e    |
| Incêndio | Incêndio |          |          |          | sperado: |
|          |          |          |          |          |          |
|          |          |          |          |          | HH:MM:ss |
+----------+----------+----------+----------+----------+----------+
| Data do  | Data do  | Te       | 10       | Sim      | -        |
| Rescaldo | rescaldo | xto-Data |          |          |  Formato |
| do       | do       |          |          |          |     e    |
| Incêndio | incêndio |          |          |          | sperado: |
|          |          |          |          |          |     dd   |
|          |          |          |          |          | /MM/aaaa |
+----------+----------+----------+----------+----------+----------+
| Hora do  | Hora do  | Te       | 7        | Não      | -        |
| Rescaldo | rescaldo | xto-Hora |          |          |  Formato |
| do       | do       |          |          |          |     e    |
| Incêndio | incêndio |          |          |          | sperado: |
|          |          |          |          |          |          |
|          |          |          |          |          | HH:MM:ss |
+----------+----------+----------+----------+----------+----------+
| *        |          |          |          |          |          |
| *Consumo |          |          |          |          |          |
| de       |          |          |          |          |          |
| Agente   |          |          |          |          |          |
| Ex       |          |          |          |          |          |
| tintor** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Qu       | Qu       | Numérico | 6        | Não      |          |
| antidade | antidade |          |          |          |          |
| de Água  | de água  |          |          |          |          |
|          | u        |          |          |          |          |
|          | tilizada |          |          |          |          |
|          | em       |          |          |          |          |
|          | litros   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Qu       | Qu       | Numérico | 6        | Não      |          |
| antidade | antidade |          |          |          |          |
| de       | de       |          |          |          |          |
| LGE      | espuma   |          |          |          |          |
| (Líquido | em       |          |          |          |          |
| Gerador  | litros   |          |          |          |          |
| de       |          |          |          |          |          |
| Esp      |          |          |          |          |          |
| uma)/EFE |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| **Outras |          |          |          |          |          |
| Infor    |          |          |          |          |          |
| mações** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Causa    | Causa    | Texto    | 5        | Não      | -        |
| Provável | provável |          |          |          |  Valores |
| de       | de       |          |          |          |     fo   |
| Ignição  | ignição  |          |          |          | rnecidos |
|          | de       |          |          |          |          |
|          | i        |          |          |          |    pelos |
|          | ncêndio. |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     - ** |
|          |          |          |          |          | incêndio |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Causas |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |  Ignição |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Inc  |
|          |          |          |          |          | êndio\". |
+----------+----------+----------+----------+----------+----------+
| Tipo     | Local    | Texto    | 5        | Não      | -        |
| Local de | p        |          |          |          |  Valores |
| Origem   | resumido |          |          |          |     fo   |
| do       | de       |          |          |          | rnecidos |
| Incêndio | origem   |          |          |          |          |
|          | do       |          |          |          |    pelos |
|          | incêndio |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     - ** |
|          |          |          |          |          | incêndio |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Locais |
|          |          |          |          |          |          |
|          |          |          |          |          |   Origem |
|          |          |          |          |          |     Inc  |
|          |          |          |          |          | êndio\". |
+----------+----------+----------+----------+----------+----------+
| Subtipo  | I        | Texto    | 5        | Não      | -        |
| Local de | ndicação |          |          |          |  Valores |
| Origem   | do       |          |          |          |     fo   |
| do       | Subtipo  |          |          |          | rnecidos |
| Incêndio | do local |          |          |          |          |
|          | de       |          |          |          |    pelos |
|          | origem   |          |          |          |          |
|          | do       |          |          |          | serviços |
|          | incêndio |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     - ** |
|          |          |          |          |          | incêndio |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Subtipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Locai |
|          |          |          |          |          | s Origem |
|          |          |          |          |          |     Inc  |
|          |          |          |          |          | êndio\". |
+----------+----------+----------+----------+----------+----------+
| Tipo     | Tipo de  | Texto    | 3        | Não      | -        |
| Fonte    | fonte    |          |          |          |  Valores |
| Provável | provável |          |          |          |     fo   |
| de       | de       |          |          |          | rnecidos |
| Ignição  | ignição  |          |          |          |          |
|          | de       |          |          |          |    pelos |
|          | i        |          |          |          |          |
|          | ncêndio. |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     - ** |
|          |          |          |          |          | incêndio |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Fontes |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |  Ignição |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Inc  |
|          |          |          |          |          | êndio\". |
+----------+----------+----------+----------+----------+----------+
| Subtipo  | Subtipo  | Texto    | 5        | Não      | -        |
| de Fonte | de fonte |          |          |          |  Valores |
| de       | provável |          |          |          |     fo   |
| Ignição  | de       |          |          |          | rnecidos |
| de       | ignição  |          |          |          |          |
| Incêndio | de       |          |          |          |    pelos |
|          | incêndio |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     - ** |
|          |          |          |          |          | incêndio |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Subtipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Fontes |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |  Ignição |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Inc  |
|          |          |          |          |          | êndio\". |
+----------+----------+----------+----------+----------+----------+

Caso o \"Tipo de Incêndio/Explosão\" for de \"EDIFICAÇÃO\", deve-se
enviar pelo menos uma estrutura de edificação:\
[[947958: RN - Incêndio/Explosão - Edificação -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_9UU8cHhEEfCXc_rffzpa6A)**Incêndio/Explosão
- Edificação**\
Quando o código Tipo de Formulário de Incêndio/Explosão indicado tiver
como valor \"EDIFICAÇÃO\".

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Sim      | -        |
| Tipo de  | cor      |          |          |          |  Valores |
| Ed       | porativo |          |          |          |     fo   |
| ificação | do Tipo  |          |          |          | rnecidos |
|          | de       |          |          |          |          |
|          | Ed       |          |          |          |    pelos |
|          | ificação |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos - |
|          |          |          |          |          |     **e  |
|          |          |          |          |          | dificaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Edifi |
|          |          |          |          |          | cação\". |
+----------+----------+----------+----------+----------+----------+
| Outro    | Campo    | Texto    | 50       | \(1\)    | -        |
| tipo de  | para     |          |          | Não      |    \(1\) |
| Ed       | d        |          |          |          |          |
| ificação | escrição |          |          |          |    Campo |
|          | de outro |          |          |          |          |
|          | tipo de  |          |          |          | torna-se |
|          | ed       |          |          |          |     obr  |
|          | ificação |          |          |          | igatório |
|          | quando o |          |          |          |     e    |
|          | \"Tipo   |          |          |          |     não  |
|          | de       |          |          |          |     pode |
|          | Edif     |          |          |          |          |
|          | icação\" |          |          |          |    ficar |
|          | for      |          |          |          |     em   |
|          | \"       |          |          |          |          |
|          | OUTRO\". |          |          |          |   branco |
|          |          |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |          |
|          |          |          |          |          | \"**Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Edific |
|          |          |          |          |          | ação**\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     a    |
|          |          |          |          |          |     \'   |
|          |          |          |          |          | Outro\'. |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 5        | Não      | -        |
| Tipo de  | Cor      |          |          |          |  Valores |
| Uso      | porativo |          |          |          |     fo   |
| Ocupação | Tipo de  |          |          |          | rnecidos |
|          | Ocupação |          |          |          |          |
|          | e uso do |          |          |          |    pelos |
|          | Imóvel   |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **e  |
|          |          |          |          |          | dificaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Uso/ |
|          |          |          |          |          | Ocupação |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Edifi |
|          |          |          |          |          | cação\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Subtipo  | Texto    | 5        | Não      | -        |
| Subtipo  | de Uso   |          |          |          |  Valores |
| Uso      | Ocupação |          |          |          |     fo   |
| Ocupação | da       |          |          |          | rnecidos |
|          | Ed       |          |          |          |          |
|          | ificação |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **e  |
|          |          |          |          |          | dificaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Subtipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Uso/ |
|          |          |          |          |          | Ocupação |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Edifi |
|          |          |          |          |          | cação\". |
+----------+----------+----------+----------+----------+----------+
| Código   | A        | Texto    | 6        | Não      | -        |
| Situação | situação |          |          |          |  Valores |
| de       | de       |          |          |          |     fo   |
| Ocup     | ocupação |          |          |          | rnecidos |
| ação/Uso | da       |          |          |          |          |
|          | ed       |          |          |          |    pelos |
|          | ificação |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **e  |
|          |          |          |          |          | dificaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Situ |
|          |          |          |          |          | ações de |
|          |          |          |          |          |          |
|          |          |          |          |          | Ocupação |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Edifi |
|          |          |          |          |          | cação\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Tipo de  | Texto    | 6        | Não      | -        |
| Tipo de  | e        |          |          |          |  Valores |
| E        | strutura |          |          |          |     fo   |
| strutura | pred     |          |          |          | rnecidos |
| da       | ominante |          |          |          |          |
| Ed       | da       |          |          |          |    pelos |
| ificação | ed       |          |          |          |          |
|          | ificação |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **e  |
|          |          |          |          |          | dificaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     E    |
|          |          |          |          |          | strutura |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Edifi |
|          |          |          |          |          | cação\". |
+----------+----------+----------+----------+----------+----------+
| Outros   | Campo    | Texto    | 50       | \(2\)    | -        |
| Tipo de  | para     |          |          | Não      |    \(2\) |
| E        | d        |          |          |          |          |
| strutura | escrição |          |          |          |    Campo |
|          | do tipo  |          |          |          |          |
|          | de       |          |          |          | torna-se |
|          | e        |          |          |          |     obr  |
|          | strutura |          |          |          | igatório |
|          | quando o |          |          |          |     e    |
|          | valor de |          |          |          |     não  |
|          | Tipo de  |          |          |          |     pode |
|          | E        |          |          |          |          |
|          | strutura |          |          |          |    ficar |
|          | Prem     |          |          |          |     em   |
|          | oninante |          |          |          |          |
|          | for      |          |          |          |   branco |
|          | \"O      |          |          |          |          |
|          | UTRAS\". |          |          |          |   quando |
|          |          |          |          |          |     \    |
|          |          |          |          |          | "**Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     E    |
|          |          |          |          |          | strutura |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Edific |
|          |          |          |          |          | ação**\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     a    |
|          |          |          |          |          |     \'O  |
|          |          |          |          |          | utras\'. |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| Classe   | Cor      |          |          |          |  Valores |
| Pred     | porativo |          |          |          |     fo   |
| ominante | da       |          |          |          | rnecidos |
|          | Classe   |          |          |          |          |
|          | pred     |          |          |          |    pelos |
|          | ominante |          |          |          |          |
|          | da       |          |          |          | serviços |
|          | ed       |          |          |          |     dis  |
|          | ificação |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **e  |
|          |          |          |          |          | dificaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Classe |
|          |          |          |          |          |     Pred |
|          |          |          |          |          | ominante |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Edifi |
|          |          |          |          |          | cação\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Qual     | Texto    | 6        | Não      | -        |
| Caract   | caract   |          |          |          |  Valores |
| erística | erística |          |          |          |     fo   |
| Con      | con      |          |          |          | rnecidos |
| strutiva | strutiva |          |          |          |          |
|          | da       |          |          |          |    pelos |
|          | ed       |          |          |          |          |
|          | ificação |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **e  |
|          |          |          |          |          | dificaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Caracte |
|          |          |          |          |          | rísticas |
|          |          |          |          |          |     Cons |
|          |          |          |          |          | trutivas |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Edifi |
|          |          |          |          |          | cação\". |
+----------+----------+----------+----------+----------+----------+
| Outra    | Campo    | Texto    | 50       | \(3\)    | -        |
| Caract   | para     |          |          | Não      |    \(3\) |
| erística | d        |          |          |          |          |
| Con      | escrição |          |          |          |    Campo |
| strutiva | de outra |          |          |          |          |
|          | caract   |          |          |          | torna-se |
|          | erística |          |          |          |     obr  |
|          | con      |          |          |          | igatório |
|          | strutiva |          |          |          |     e    |
|          | quando a |          |          |          |     não  |
|          | \"Caract |          |          |          |     pode |
|          | erística |          |          |          |          |
|          | Const    |          |          |          |    ficar |
|          | rutiva\" |          |          |          |     em   |
|          | for      |          |          |          |          |
|          | \        |          |          |          |   branco |
|          | "OUTRO\" |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |     \"   |
|          |          |          |          |          | **Código |
|          |          |          |          |          |          |
|          |          |          |          |          |   Caract |
|          |          |          |          |          | erística |
|          |          |          |          |          |          |
|          |          |          |          |          |  Constru |
|          |          |          |          |          | tiva**\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     a    |
|          |          |          |          |          |     \'O  |
|          |          |          |          |          | utros\'. |
+----------+----------+----------+----------+----------+----------+
| Total de | Total de | Numérico | 3        | Não      |          |
| Pa       | Pa       |          |          |          |          |
| vimentos | vimentos |          |          |          |          |
| da       | da       |          |          |          |          |
| Ed       | Ed       |          |          |          |          |
| ificação | ificação |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Número   | Qu       | Numérico | 3        | Não      | -        |
| de       | antidade |          |          |          |   Número |
| Pa       | de       |          |          |          |     de   |
| vimentos | pa       |          |          |          |     Pa   |
| A        | vimentos |          |          |          | vimentos |
| tingidos | a        |          |          |          |     A    |
|          | tingidos |          |          |          | tingidos |
|          | da       |          |          |          |     não  |
|          | ed       |          |          |          |     pode |
|          | ificação |          |          |          |     ser  |
|          |          |          |          |          |          |
|          |          |          |          |          |    menor |
|          |          |          |          |          |     do   |
|          |          |          |          |          |     que  |
|          |          |          |          |          |          |
|          |          |          |          |          |  \'Total |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Pa   |
|          |          |          |          |          | vimentos |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Edifi |
|          |          |          |          |          | cação\'. |
+----------+----------+----------+----------+----------+----------+
| D        | D        | Texto    | 250      | Não      |          |
| escrição | escrição |          |          |          |          |
| Pa       | de quais |          |          |          |          |
| vimentos | dos      |          |          |          |          |
| A        | pa       |          |          |          |          |
| tingidos | vimentos |          |          |          |          |
|          | foram    |          |          |          |          |
|          | a        |          |          |          |          |
|          | tingidos |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Área     | Área     | Numérico | (8,2)    | Não      |          |
| Total de | total de | decimal  |          |          |          |
| Ed       | edific   |          |          |          |          |
| ificação | ação(m²) |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Área     | Área     | Numérico | (8,2)    | Não      |          |
| Atingida | Atingida | decimal  |          |          |          |
|          | (m²)     |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Texto    | 6        | Não      | -        |
| dicativo | dicativo |          |          |          |  Valores |
| Houve    | se houve |          |          |          |     fo   |
| explosão | explosão |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |          |
|          |          |          |          |          |  **outro |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Opções |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Res  |
|          |          |          |          |          | posta\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -   Caso |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     seja |
|          |          |          |          |          |          |
|          |          |          |          |          | enviado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |          |
|          |          |          |          |          |   padrão |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     a    |
|          |          |          |          |          |          |
|          |          |          |          |          |  \"**Não |
|          |          |          |          |          |          |
|          |          |          |          |          |   Inform |
|          |          |          |          |          | ado**\". |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Texto    | 6        | Não      | -        |
| dicativo | dicativo |          |          |          |  Valores |
| Houve    | se houve |          |          |          |     fo   |
| Danos    | danos    |          |          |          | rnecidos |
| A        | a        |          |          |          |          |
| parentes | parentes |          |          |          |    pelos |
| à        | na       |          |          |          |          |
| E        | e        |          |          |          | serviços |
| strutura | strutura |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |          |
|          |          |          |          |          |  **outro |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Opções |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Res  |
|          |          |          |          |          | posta\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -   Caso |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     seja |
|          |          |          |          |          |          |
|          |          |          |          |          | enviado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |          |
|          |          |          |          |          |   padrão |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     a    |
|          |          |          |          |          |          |
|          |          |          |          |          |  \"**Não |
|          |          |          |          |          |          |
|          |          |          |          |          |   Inform |
|          |          |          |          |          | ado**\". |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Texto    | 6        | Não      | -        |
| dicativo | dicativo |          |          |          |  Valores |
| Houve    | se houve |          |          |          |     fo   |
| des      | des      |          |          |          | rnecidos |
| abamento | abamento |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |          |
|          |          |          |          |          |  **outro |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Opções |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Res  |
|          |          |          |          |          | posta\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -   Caso |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     seja |
|          |          |          |          |          |          |
|          |          |          |          |          | enviado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |          |
|          |          |          |          |          |   padrão |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     a    |
|          |          |          |          |          |          |
|          |          |          |          |          |  \"**Não |
|          |          |          |          |          |          |
|          |          |          |          |          |   Inform |
|          |          |          |          |          | ado**\". |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Texto    | 6        | Não      | -        |
| dicativo | dicativo |          |          |          |  Valores |
| Houve    | se houve |          |          |          |     fo   |
| Nec      | entrada  |          |          |          | rnecidos |
| essidade | forçada  |          |          |          |          |
| de       |          |          |          |          |    pelos |
| Entrada  |          |          |          |          |          |
| Forçada  |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |          |
|          |          |          |          |          |  **outro |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Opções |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Res  |
|          |          |          |          |          | posta\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -   Caso |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     seja |
|          |          |          |          |          |          |
|          |          |          |          |          | enviado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |          |
|          |          |          |          |          |   padrão |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     a    |
|          |          |          |          |          |          |
|          |          |          |          |          |  \"**Não |
|          |          |          |          |          |          |
|          |          |          |          |          |   Inform |
|          |          |          |          |          | ado**\". |
+----------+----------+----------+----------+----------+----------+
| Código   | Lista de | Lista    | 6        | Não      | -        |
| Nec      | Código   | Código   |          |          |  Valores |
| essidade | de       | Texto    |          |          |     fo   |
| para     | Nece     |          |          |          | rnecidos |
| Entrada  | ssidades |          |          |          |          |
| Forçada  | de       |          |          |          |    pelos |
|          | Entrada  |          |          |          |          |
|          | Forçada  |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **e  |
|          |          |          |          |          | dificaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Nec  |
|          |          |          |          |          | essidade |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |  Entrada |
|          |          |          |          |          |          |
|          |          |          |          |          |  Forçada |
|          |          |          |          |          |          |
|          |          |          |          |          | da Edifi |
|          |          |          |          |          | cação\". |
+----------+----------+----------+----------+----------+----------+
| D        | D        | Texto    | 50       | Não      |          |
| escrição | escrição |          |          |          |          |
| Outra    | de Outra |          |          |          |          |
| Nec      | nec      |          |          |          |          |
| essidade | essidade |          |          |          |          |
| para     | de       |          |          |          |          |
| Entrada  | entrada  |          |          |          |          |
| Forçada  | Forçada  |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Texto    | 6        | Não      | -        |
| dicativo | dicativo |          |          |          |  Valores |
| Possui   | se a     |          |          |          |     fo   |
| Alvará   | ed       |          |          |          | rnecidos |
| do Corpo | ificação |          |          |          |          |
| de       | possui   |          |          |          |    pelos |
| B        | alvará   |          |          |          |          |
| ombeiros | do corpo |          |          |          | serviços |
|          | de       |          |          |          |     dis  |
|          | b        |          |          |          | poníveis |
|          | ombeiros |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |          |
|          |          |          |          |          |  **outro |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Opções |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Res  |
|          |          |          |          |          | posta\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -   Caso |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     seja |
|          |          |          |          |          |          |
|          |          |          |          |          | enviado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |          |
|          |          |          |          |          |   padrão |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     a    |
|          |          |          |          |          |          |
|          |          |          |          |          |  \"**Não |
|          |          |          |          |          |          |
|          |          |          |          |          |   Inform |
|          |          |          |          |          | ado**\". |
+----------+----------+----------+----------+----------+----------+
| Sistemas | Lista    | Lista    | 6        | Não      | -        |
| Pre      | com os   | Código   |          |          |  Valores |
| ventivos | sistemas | Texto    |          |          |     fo   |
| Ut       | pre      |          |          |          | rnecidos |
| ilizados | ventivos |          |          |          |          |
|          | ut       |          |          |          |    pelos |
|          | ilizados |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **e  |
|          |          |          |          |          | dificaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Sistemas |
|          |          |          |          |          |     Pre  |
|          |          |          |          |          | ventivos |
|          |          |          |          |          |          |
|          |          |          |          |          | da Edifi |
|          |          |          |          |          | cação\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |  Valores |
|          |          |          |          |          |     du   |
|          |          |          |          |          | plicados |
|          |          |          |          |          |          |
|          |          |          |          |          |    devem |
|          |          |          |          |          |     ser  |
|          |          |          |          |          |     desp |
|          |          |          |          |          | rezados. |
+----------+----------+----------+----------+----------+----------+
| D        | Campo    | Texto    | 50       | Não      |          |
| escrição | para     |          |          |          |          |
| Outros   | d        |          |          |          |          |
| Sistemas | escrição |          |          |          |          |
| Pre      | de       |          |          |          |          |
| ventivos | outros   |          |          |          |          |
| Ut       | sistemas |          |          |          |          |
| ilizados | pre      |          |          |          |          |
|          | ventivos |          |          |          |          |
|          | ut       |          |          |          |          |
|          | ilizados |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| D        | Campo    | Texto    | 400      | Não      |          |
| escrição | para     |          |          |          |          |
| Bens     | d        |          |          |          |          |
| Móveis e | escrição |          |          |          |          |
| Imóveis  | de bens  |          |          |          |          |
| A        |          |          |          |          |          |
| tingidos |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Ações    | Lista    | Lista    | 5        | Sim      | -        |
| Re       | para     | Código   |          |          |  Valores |
| alizadas | indicar  | Texto    |          |          |     fo   |
|          | as ações |          |          |          | rnecidos |
|          | re       |          |          |          |          |
|          | alizadas |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **   |
|          |          |          |          |          | incêndio |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Ações |
|          |          |          |          |          |     Re   |
|          |          |          |          |          | alizadas |
|          |          |          |          |          |          |
|          |          |          |          |          |   CBM\". |
+----------+----------+----------+----------+----------+----------+

Caso o \"Tipo de Incêndio/Explosão\" for de \"VEGETAÇÃO\", deve-se
enviar pelo menos uma estrutura de vegetação:\
[[947977: RN - Incêndio/Explosão - Vegetação -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_9wH44HhKEfCXc_rffzpa6A)**Incêndio/Explosão
- Vegetação**\
Quando o tipo de Incêndio/Explosão indicado tiver como valor
\"VEGETAÇÃO\".

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Código   | Tipo de  | Texto    | 6        | Não      | -        |
| Tipo de  | unidade  |          |          |          |  Valores |
| Unidade  | de       |          |          |          |     fo   |
| de       | pre      |          |          |          | rnecidos |
| Conserv  | servação |          |          |          |          |
| ação/Pre | (cons    |          |          |          |    pelos |
| servação | ervação, |          |          |          |          |
|          | pres     |          |          |          | serviços |
|          | ervação) |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **   |
|          |          |          |          |          | vegetaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Conserva |
|          |          |          |          |          | ção/Vege |
|          |          |          |          |          | tação\". |
+----------+----------+----------+----------+----------+----------+
| Total    | Total    | Numérico | (8,2)    | Sim      |          |
| Ap       | ap       | decimal  |          |          |          |
| roximado | roximado |          |          |          |          |
| de Área  | da área  |          |          |          |          |
| Q        | da       |          |          |          |          |
| ueimada( | v        |          |          |          |          |
| hectare) | egetação |          |          |          |          |
|          | q        |          |          |          |          |
|          | ueimada( |          |          |          |          |
|          | hectare) |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Texto    | 6        | Não      | -        |
| dicativo | dicativo |          |          |          |  Valores |
| Houve    | se houve |          |          |          |     fo   |
| Apoio    | apoio de |          |          |          | rnecidos |
|          | Br       |          |          |          |          |
|          | igada/Vi |          |          |          |    pelos |
|          | gilante/ |          |          |          |          |
|          | Bombeiro |          |          |          | serviços |
|          | Civil    |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Cor  |
|          |          |          |          |          | porativo |
|          |          |          |          |          | s**outro |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Opções |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Res  |
|          |          |          |          |          | posta\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -   Caso |
|          |          |          |          |          |     não  |
|          |          |          |          |          |     seja |
|          |          |          |          |          |          |
|          |          |          |          |          | enviado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |          |
|          |          |          |          |          |   padrão |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     a    |
|          |          |          |          |          |          |
|          |          |          |          |          |  \"**Não |
|          |          |          |          |          |          |
|          |          |          |          |          |   Inform |
|          |          |          |          |          | ado**\". |
+----------+----------+----------+----------+----------+----------+
| Qu       | Qu       | Numérico | 6        | Não      | -        |
| antidade | antidade |          |          |          |  Somente |
| de       | de       |          |          |          |          |
| Pessoas  | Pessoas  |          |          |          |  receber |
| de Apoio | de apoio |          |          |          |     se   |
|          |          |          |          |          |     \"In |
|          |          |          |          |          | dicativo |
|          |          |          |          |          |          |
|          |          |          |          |          |    Houve |
|          |          |          |          |          |          |
|          |          |          |          |          |  Apoio\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |  enviado |
|          |          |          |          |          |     como |
|          |          |          |          |          |     SIM. |
+----------+----------+----------+----------+----------+----------+
| Códigos  | Lista de | Lista    | 6        | Sim      | -        |
| Tipos de | Tipos de | Texto    |          |          |  Valores |
| Incêndio | Incêndio |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **   |
|          |          |          |          |          | vegetaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Incêndio |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Vege |
|          |          |          |          |          | tação\". |
+----------+----------+----------+----------+----------+----------+
| Códigos  | Lista de | Lista    | 6        | Sim      | -        |
| Tipos de | Tipos de | Texto    |          |          |  Valores |
| V        | V        |          |          |          |     fo   |
| egetação | egetação |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     **   |
|          |          |          |          |          | vegetaçã |
|          |          |          |          |          | o-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Vege |
|          |          |          |          |          | tação\". |
+----------+----------+----------+----------+----------+----------+
| Outro    | D        | Texto    | 50       | Não      | -        |
| Tipo de  | escrição |          |          |          |    Campo |
| V        | de outro |          |          |          |     só   |
| egetação | tipo de  |          |          |          |     pode |
|          | v        |          |          |          |     ser  |
|          | egetação |          |          |          |          |
|          |          |          |          |          |  enviado |
|          |          |          |          |          |     se   |
|          |          |          |          |          |          |
|          |          |          |          |          |  \'Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Veg  |
|          |          |          |          |          | etação\' |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |   corres |
|          |          |          |          |          | pondente |
|          |          |          |          |          |     a    |
|          |          |          |          |          |     \'O  |
|          |          |          |          |          | utros\'. |
+----------+----------+----------+----------+----------+----------+
| D        | D        | Texto    | 200      | Não      |          |
| escrição | escrição |          |          |          |          |
| Tipo de  | do tipo  |          |          |          |          |
| Solo     | de solo  |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Código   | Código   | Texto    | 6        | Não      | -        |
| Tipo de  | Cor      |          |          |          |  Valores |
| Relevo   | porativo |          |          |          |     fo   |
|          | do Tipo  |          |          |          | rnecidos |
|          | de       |          |          |          |          |
|          | Relevo   |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     *    |
|          |          |          |          |          | *vegetaç |
|          |          |          |          |          | ão-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     R    |
|          |          |          |          |          | elevo\". |
+----------+----------+----------+----------+----------+----------+
| **C      |          |          |          |          |          |
| ondições |          |          |          |          |          |
| Meteorol |          |          |          |          |          |
| ógicas** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Tem      | Tem      | Texto    | 50       | Não      |          |
| peratura | peratura |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Umidade  | Umidade  | Texto    | 50       | Não      |          |
| Relativa | do ar    |          |          |          |          |
| do Ar    |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Prec     | Prec     | Texto    | 50       | Não      |          |
| ipitação | ipitação |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Ve       | Ve       | Texto    | 50       | Não      |          |
| locidade | locidade |          |          |          |          |
| do vento | do vento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Códigos  | Listagem | Lista    | 6        | Não      | -        |
| Recursos | com      | Texto    |          |          |  Valores |
| Hídricos | recursos |          |          |          |     fo   |
| Dis      | hídricos |          |          |          | rnecidos |
| poníveis | dis      |          |          |          |          |
|          | poníveis |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |     Corp |
|          |          |          |          |          | orativos |
|          |          |          |          |          |     *    |
|          |          |          |          |          | *vegetaç |
|          |          |          |          |          | ão-v3**: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Tipos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |  Recurso |
|          |          |          |          |          |          |
|          |          |          |          |          |  Hídrico |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dispo |
|          |          |          |          |          | nível\". |
+----------+----------+----------+----------+----------+----------+
| Outros   | D        | Texto    | 50       | Não      |          |
| Recursos | escrição |          |          |          |          |
| Hídrivos | de       |          |          |          |          |
|          | outros   |          |          |          |          |
|          | recursos |          |          |          |          |
|          | hídricos |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Ações    | Lista de | Lista    | 6        | Sim      | -        |
| Re       | Ações    | Texto    |          |          |  Valores |
| alizadas | Re       |          |          |          |     fo   |
|          | alizadas |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos** |
|          |          |          |          |          | incêndio |
|          |          |          |          |          | s-v3**:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Ações |
|          |          |          |          |          |     Re   |
|          |          |          |          |          | alizadas |
|          |          |          |          |          |          |
|          |          |          |          |          |   CBM\". |
+----------+----------+----------+----------+----------+----------+

Caso o \"Tipo de Incêndio/Explosão\" for de \"MEIO DE TRANSPORTE\",
deve-se enviar pelo menos uma estrutura de meio de transporte:\
[[948015: RN - Incêndio/Explosão - Meio de Transporte -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_26oRoHhTEfCXc_rffzpa6A)**Incêndio/Explosão
- Meio de Transporte**\
Quando o tipo de Incêndio/Explosão indicado tiver como valor \"MEIO DE
TRANSPORTE\".

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Subgrupo | Numérico | 6        | Não      | -        |
| Meio de  | do meio  |          |          |          |  Valores |
| Tr       | de       |          |          |          |     fo   |
| ansporte | tr       |          |          |          | rnecidos |
|          | ansporte |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Subgrupo |
|          |          |          |          |          |     O    |
|          |          |          |          |          | bjeto\". |
|          |          |          |          |          |          |
|          |          |          |          |          | -   Os   |
|          |          |          |          |          |          |
|          |          |          |          |          |  valores |
|          |          |          |          |          |     r    |
|          |          |          |          |          | ecebidos |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Subgrupo |
|          |          |          |          |          |          |
|          |          |          |          |          |   Objeto |
|          |          |          |          |          |     tem  |
|          |          |          |          |          |     que  |
|          |          |          |          |          |     p    |
|          |          |          |          |          | ertencer |
|          |          |          |          |          |     a    |
|          |          |          |          |          |     c    |
|          |          |          |          |          | ategoria |
|          |          |          |          |          |          |
|          |          |          |          |          |   \"Meio |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Trans |
|          |          |          |          |          | porte\". |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Tipo de  | Numérico | 6        | Não      | -        |
| Com      | com      |          |          |          |  Valores |
| bustível | bustível |          |          |          |     fo   |
|          | do       |          |          |          | rnecidos |
|          | veículo  |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Comb |
|          |          |          |          |          | ustíveis |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Ve   |
|          |          |          |          |          | ículo\". |
+----------+----------+----------+----------+----------+----------+
| Danos    | D        | Texto    | 400      | Não      |          |
| M        | escrição |          |          |          |          |
| ateriais | dos      |          |          |          |          |
| A        | danos    |          |          |          |          |
| parentes | m        |          |          |          |          |
|          | ateriais |          |          |          |          |
|          | a        |          |          |          |          |
|          | parentes |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Houve    | In       | Booleano | 1        | Não      | -   Caso |
| Explosão | dicativo |          |          |          |     não  |
|          | se houve |          |          |          |     seja |
|          | explosão |          |          |          |          |
|          | do meio  |          |          |          | enviado, |
|          | de       |          |          |          |     co   |
|          | tr       |          |          |          | nsiderar |
|          | ansporte |          |          |          |     por  |
|          |          |          |          |          |          |
|          |          |          |          |          |   padrão |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |          |
|          |          |          |          |          |    NULL  |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |  -   Sim |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |  -   Não |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |  -   Não |
|          |          |          |          |          |          |
|          |          |          |          |          |    Apura |
|          |          |          |          |          | do(null) |
+----------+----------+----------+----------+----------+----------+
| *        |          |          |          |          |          |
| *Carga** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Possui   | In       | Booleano | 1        | Sim      | -   Caso |
| Carga    | dicativo |          |          |          |     não  |
|          | se o     |          |          |          |     seja |
|          | veículo  |          |          |          |          |
|          | possuia  |          |          |          | enviado, |
|          | carga    |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     por  |
|          |          |          |          |          |          |
|          |          |          |          |          |   padrão |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     NÃO  |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |  -   Sim |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |  -   Não |
+----------+----------+----------+----------+----------+----------+
| Tipo     | Tipo de  | Texto    | 3        | \*(1)    | -        |
| Carga    | carga    |          |          | Não      |    \*(1) |
| Tran     | tran     |          |          |          |          |
| sportada | sportada |          |          |          |    Campo |
|          |          |          |          |          |          |
|          |          |          |          |          | torna-se |
|          |          |          |          |          |     obr  |
|          |          |          |          |          | igatorio |
|          |          |          |          |          |     se   |
|          |          |          |          |          |          |
|          |          |          |          |          |   possui |
|          |          |          |          |          |          |
|          |          |          |          |          |    carga |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |  enviado |
|          |          |          |          |          |     com  |
|          |          |          |          |          |     SIM. |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |  Valores |
|          |          |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Tipo de |
|          |          |          |          |          |          |
|          |          |          |          |          |    Carga |
|          |          |          |          |          |          |
|          |          |          |          |          |  Transpo |
|          |          |          |          |          | rtada\". |
+----------+----------+----------+----------+----------+----------+
| Outro    | D        | Texto    | 50       | \*(2)    | -        |
| Tipo de  | escrição |          |          | Não      |    \*(2) |
| Carga    | de outro |          |          |          |          |
| Tran     | tipo de  |          |          |          | Torna-se |
| sportada | carga    |          |          |          |     obr  |
|          | tran     |          |          |          | igatorio |
|          | sportada |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |   \"Tipo |
|          |          |          |          |          |          |
|          |          |          |          |          |    Carga |
|          |          |          |          |          |          |
|          |          |          |          |          |   Transp |
|          |          |          |          |          | ortada\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |     \"   |
|          |          |          |          |          | OUTRO\". |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Booleano | 1        | Não      |          |
| dicativo | dicativo |          |          |          |          |
| Carga    | se a     |          |          |          |          |
| Atingida | carga    |          |          |          |          |
|          | foi      |          |          |          |          |
|          | atingida |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Booleano | 1        | Não      |          |
| dicativo | dicativo |          |          |          |          |
| Carga    | se a     |          |          |          |          |
| com      | carga    |          |          |          |          |
| Seguro   | tinha    |          |          |          |          |
|          | seguro   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Ações    | Lista    | Lista    | 3        | Sim      | -        |
| Re       | com      | Código   |          |          |  Valores |
| alizadas | ações    | Texto    |          |          |     fo   |
|          | re       |          |          |          | rnecidos |
|          | alizadas |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Ações |
|          |          |          |          |          |          |
|          |          |          |          |          |    Reali |
|          |          |          |          |          | zadas\". |
+----------+----------+----------+----------+----------+----------+

Caso o \"Tipo de Incêndio/Explosão\" for \"PNEU\", \"LIXO\" ou
\"OUTRO\", deve-se enviar pelo menos uma estrutura:\
[[948030: RN - Incêndio/Explosão - Outros -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_lR5YAHhhEfCXc_rffzpa6A)**Incêndio/Explosão
- Edificação**\
Quando o tipo de Incêndio/Explosão indicado tiver como valor
\"PNEU\", \"LIXO\" ou \"OUTRO\".

  ------------------- ---------------------------------------- ---------------- ------------- ----------------- -------------------------------------------------------------------------------------------------------------
  **Nome do Campo**   **Descrição**                            **Tipo**         **Tamanho**   **Obrigatório**   **Regras de Recebimento**
  Ações Realizadas    Lista para indicar as ações realizadas   Lista Numérica   6             Sim               Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: \"Listar de Ações Realizadas\".
  ------------------- ---------------------------------------- ---------------- ------------- ----------------- -------------------------------------------------------------------------------------------------------------

[[934653: RN - Busca e Salvamento -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_inxYMGC0EfCirqqtwbaCiw)**Busca
e Salvamento**

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **Obrig  | **T      | **Regras |
| do       | crição** |          | atório** | amanho** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Tipo de  | Lista    | Sim      | 3        | -        |
| Busca e  | busca e  | Código   |          |          |  Valores |
| Sa       | sa       | Texto    |          |          |     fo   |
| lvamento | lvamento |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Busca |
|          |          |          |          |          |     e    |
|          |          |          |          |          |          |
|          |          |          |          |          |    Salva |
|          |          |          |          |          | mento\". |
+----------+----------+----------+----------+----------+----------+
| Subtipo  | Subtipo  | Lista    | Sim      | 3        | -        |
| de Busca | de busca | Código   |          |          |  Valores |
| e        | e        | Texto    |          |          |     fo   |
| Sa       | sa       |          |          |          | rnecidos |
| lvamento | lvamento |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  SubTipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Busca |
|          |          |          |          |          |     e    |
|          |          |          |          |          |          |
|          |          |          |          |          |    Salva |
|          |          |          |          |          | mento\". |
+----------+----------+----------+----------+----------+----------+
| Tipo     | Lista    | Lista    | Sim      | 3        | -        |
| Envo     | com os   | Código   |          |          |  Valores |
| lvimento | objetos  | Texto    |          |          |     fo   |
| Busca e  | en       |          |          |          | rnecidos |
| Sa       | volvidos |          |          |          |          |
| lvamento |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     En   |
|          |          |          |          |          | volvimen |
|          |          |          |          |          | to Busca |
|          |          |          |          |          |     e    |
|          |          |          |          |          |          |
|          |          |          |          |          |    Salva |
|          |          |          |          |          | mento\". |
+----------+----------+----------+----------+----------+----------+
| A        | Lista    | Lista    | Sim      | 3        | -        |
| ção(ões) | com as   | Código   |          |          |  Valores |
| Real     | ações    | Texto    |          |          |     fo   |
| izada(s) | re       |          |          |          | rnecidos |
|          | alizadas |          |          |          |          |
|          | pelo     |          |          |          |    pelos |
|          | órgão    |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Ações |
|          |          |          |          |          |          |
|          |          |          |          |          |    Reali |
|          |          |          |          |          | zadas\". |
+----------+----------+----------+----------+----------+----------+
| Outras   | D        | Texto    | Não      | 50       | -        |
| Ações    | escrição |          |          |          |   Quando |
|          | de       |          |          |          |     a    |
|          | outras   |          |          |          |          |
|          | ações    |          |          |          |    opção |
|          | não      |          |          |          |     de   |
|          | listadas |          |          |          |          |
|          | em Ações |          |          |          |   \"Ação |
|          | Rea      |          |          |          |     Rea  |
|          | lizadas. |          |          |          | lizada\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |     \"   |
|          |          |          |          |          | OUTRO\". |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Lista de | Lista    | Não      | 3        | -        |
| Animal   | animais  | Código   |          |          |  Valores |
|          |          | Texto    |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Tipo de |
|          |          |          |          |          |     A    |
|          |          |          |          |          | nimal\". |
+----------+----------+----------+----------+----------+----------+

[[934660: RN - Atendimento Pré-Hospitalar -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_UrYD4GC2EfCirqqtwbaCiw)**Atendimento
Pré-Hospitalar**

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **Obrig  | **T      | **Regras |
| do       | crição** |          | atório** | amanho** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Vítima   | I        | Numérico | Sim      | 4        | -        |
|          | ndicação |          |          |          |  Vínculo |
|          | da       |          |          |          |     com  |
|          | Pessoa   |          |          |          |          |
|          | que foi  |          |          |          |  Pessoas |
|          | vítima   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| **Dados  |          |          |          |          |          |
| da       |          |          |          |          |          |
| Lesão**  |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Booleano | Não      | 1        |          |
| dicativo | dicativo |          |          |          |          |
| Lesão    | se não   |          |          |          |          |
|          | teve     |          |          |          |          |
|          | lesão    |          |          |          |          |
|          | aparente |          |          |          |          |
|          | ou foi   |          |          |          |          |
|          | politrau |          |          |          |          |
|          | matizado |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Lista de | Lista    | Não      | 3        | -        |
| Lesão    | lesões   | Código   |          |          |  Valores |
|          |          | Texto    |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Lesão\". |
+----------+----------+----------+----------+----------+----------+
| Ate      | Ate      | Lista    | Não      | 3        | -        |
| ndimento | ndimento | Código   |          |          |  Valores |
| de       | de       | Texto    |          |          |     fo   |
| corrente | corrente |          |          |          | rnecidos |
|          | de       |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Ate  |
|          |          |          |          |          | ndimento |
|          |          |          |          |          |          |
|          |          |          |          |          |    Decor |
|          |          |          |          |          | rente\". |
+----------+----------+----------+----------+----------+----------+
| Subtipo  | Subtipo  | Lista    | Não      | 3        | -        |
| Ate      | de       | Código   |          |          |  Valores |
| ndimento | Ate      | Texto    |          |          |     fo   |
| De       | ndimento |          |          |          | rnecidos |
| corrente | De       |          |          |          |          |
|          | corrente |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Su   |
|          |          |          |          |          | btipo de |
|          |          |          |          |          |     Ate  |
|          |          |          |          |          | ndimento |
|          |          |          |          |          |          |
|          |          |          |          |          |    Decor |
|          |          |          |          |          | rente\". |
+----------+----------+----------+----------+----------+----------+
| **Qu     |          |          |          |          |          |
| eimadura |          |          |          |          |          |
| -        |          |          |          |          |          |
| Somente  |          |          |          |          |          |
| para     |          |          |          |          |          |
| \"Ate    |          |          |          |          |          |
| ndimento |          |          |          |          |          |
| Deco     |          |          |          |          |          |
| rrente\" |          |          |          |          |          |
| de       |          |          |          |          |          |
| valor:   |          |          |          |          |          |
| \'Queima |          |          |          |          |          |
| dura\'** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Faixa    | Faixa    | Texto    | Não      | 3        | -        |
| etária   | etária   |          |          |          |  Valores |
| da       | da       |          |          |          |     fo   |
| vítima   | vítima   |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Faix |
|          |          |          |          |          | a Etária |
|          |          |          |          |          |     da   |
|          |          |          |          |          |     V    |
|          |          |          |          |          | ítima\". |
+----------+----------+----------+----------+----------+----------+
| Parte do | Lista de | Lista    | Não      | 6        | -        |
| corpo    | parte(s) | Numérica |          |          |  Valores |
| queimada | do corpo |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Partes |
|          |          |          |          |          |     do   |
|          |          |          |          |          |          |
|          |          |          |          |          | Corpo\". |
+----------+----------+----------+----------+----------+----------+
| Produto  | Produto  | Lista    | Não      | 6        | -        |
| causador | causador | Numérica |          |          |  Valores |
| Qu       | da       |          |          |          |     fo   |
| eimadura | qu       |          |          |          | rnecidos |
|          | eimadura |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Produto |
|          |          |          |          |          |          |
|          |          |          |          |          | Causador |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Queim |
|          |          |          |          |          | adura\". |
+----------+----------+----------+----------+----------+----------+
| Outro    | D        | Texto    | Não      | 50       | -        |
| produto  | escrição |          |          |          |   Quando |
|          | do outro |          |          |          |     o    |
|          | produto  |          |          |          |     \    |
|          | causador |          |          |          | "Produto |
|          |          |          |          |          |     Ca   |
|          |          |          |          |          | usador\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |     \    |
|          |          |          |          |          | 'OUTRO\' |
+----------+----------+----------+----------+----------+----------+
| Várias   | In       | Booleano | Não      | 1        |          |
| áreas    | dicativo |          |          |          |          |
| a        | de       |          |          |          |          |
| tingidas | várias   |          |          |          |          |
|          | áreas    |          |          |          |          |
|          | a        |          |          |          |          |
|          | tingidas |          |          |          |          |
|          | pela     |          |          |          |          |
|          | qu       |          |          |          |          |
|          | eimadura |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Su       | Por      | Numérico | Não      | 3        | -        |
| perfície | centagem |          |          |          |    Valor |
| atingida | da       |          |          |          |     deve |
|          | su       |          |          |          |          |
|          | perfície |          |          |          |    estar |
|          | atingida |          |          |          |     com  |
|          |          |          |          |          |          |
|          |          |          |          |          | mínimo 0 |
|          |          |          |          |          |     e    |
|          |          |          |          |          |     má   |
|          |          |          |          |          | ximo 100 |
+----------+----------+----------+----------+----------+----------+
| **Escala |          |          |          |          |          |
| de       |          |          |          |          |          |
| G        |          |          |          |          |          |
| lasgow** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Abertura | valor    | Numérico | Não      | 6        | -        |
| Ocular   | corres   |          |          |          |  Valores |
|          | pondente |          |          |          |     fo   |
|          | a        |          |          |          | rnecidos |
|          | abertura |          |          |          |          |
|          | ocular   |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Abertura |
|          |          |          |          |          |     O    |
|          |          |          |          |          | cular\". |
+----------+----------+----------+----------+----------+----------+
| Melhor   | valor    | Numérico | Não      | 4        | -        |
| Resposta | corres   |          |          |          |  Valores |
| Verbal   | pondente |          |          |          |     fo   |
|          | a        |          |          |          | rnecidos |
|          | resposta |          |          |          |          |
|          | verbal   |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Resposta |
|          |          |          |          |          |     V    |
|          |          |          |          |          | erbal\". |
+----------+----------+----------+----------+----------+----------+
| Melhor   | valor    | Numérico | Não      | 4        | -        |
| Resposta | corres   |          |          |          |  Valores |
| Motora   | pondente |          |          |          |     fo   |
|          | a        |          |          |          | rnecidos |
|          | resposta |          |          |          |          |
|          | motora   |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Resposta |
|          |          |          |          |          |     M    |
|          |          |          |          |          | otora\". |
+----------+----------+----------+----------+----------+----------+
| Pu       | medição  | Numérico | Não      | 3        |          |
| lso(bpm) | do pulso |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| SaO2(S   | medição  | Numérico | Não      | (3,2)    |          |
| aturação | da       | decimal  |          |          |          |
| O        | s        |          |          |          |          |
| xigênio) | aturação |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| **Trauma |          |          |          |          |          |
| Score**  |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Faixa de | Índice   | Texto    | Não      | 6        | -        |
| Respir   | de       |          |          |          |  Valores |
| ação/Min | re       |          |          |          |     fo   |
|          | spitação |          |          |          | rnecidos |
|          | por      |          |          |          |          |
|          | minuto   |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Faixa |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     R    |
|          |          |          |          |          | espiraçã |
|          |          |          |          |          | o/Min\". |
+----------+----------+----------+----------+----------+----------+
| P.A      | Pressão  | Texto    | Não      | 6        | -        |
| Máxima   | arterial |          |          |          |  Valores |
| (mmHg)   | máxima   |          |          |          |     fo   |
|          | medida   |          |          |          | rnecidos |
|          | (mmHg)   |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Faixa |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     P.A  |
|          |          |          |          |          |     M    |
|          |          |          |          |          | áxima\". |
+----------+----------+----------+----------+----------+----------+
| **Proce  |          |          |          |          |          |
| dimentos |          |          |          |          |          |
| Pr       |          |          |          |          |          |
| é-Hospit |          |          |          |          |          |
| alares** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Vítima   | In       | Booleano | Sim      | 1        |          |
| Recusou  | dicativo |          |          |          |          |
| Ate      | se       |          |          |          |          |
| ndimento | recusou  |          |          |          |          |
|          | ate      |          |          |          |          |
|          | ndimento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Houve    | In       | Booleano | Sim      | 1        |          |
| Suporte  | dicativo |          |          |          |          |
| Avançado | de       |          |          |          |          |
|          | suporte  |          |          |          |          |
|          | avançado |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Nome     | Nome do  | Texto    | Sim      | 100      |          |
| Suporte  | suporte  |          |          |          |          |
| Avançado | avançado |          |          |          |          |
| que      | que      |          |          |          |          |
| Intercep | int      |          |          |          |          |
| tou(USA) | erceptou |          |          |          |          |
|          | o        |          |          |          |          |
|          | ate      |          |          |          |          |
|          | ndimento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| **Aç     |          |          |          |          |          |
| ão(ções) |          |          |          |          |          |
| Realiz   |          |          |          |          |          |
| ada(s)** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Nome     | Lista de | Lista    | Não      | 6        | -        |
| da(s)    | ações    | Numérica |          |          |  Valores |
| Aç       | re       |          |          |          |     fo   |
| ão(ções) | alizadas |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Ações |
|          |          |          |          |          |          |
|          |          |          |          |          |    Reali |
|          |          |          |          |          | zadas\". |
+----------+----------+----------+----------+----------+----------+
| *        |          |          |          |          |          |
| *Destino |          |          |          |          |          |
| Dado a   |          |          |          |          |          |
| Vítima** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Vítima   | Nome do  | Numérica | Não      | 6        | -        |
| R        | órgão    |          |          |          |  Valores |
| epassada | que foi  |          |          |          |     fo   |
| Para     | entregue |          |          |          | rnecidos |
|          | a vítima |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Orgão de |
|          |          |          |          |          |          |
|          |          |          |          |          |  Repasse |
|          |          |          |          |          |     da   |
|          |          |          |          |          |     V    |
|          |          |          |          |          | ítima\". |
+----------+----------+----------+----------+----------+----------+
| Nome do  | nome do  | Texto    | \*Não    | 150      | -        |
| Órgão de | órgão    |          |          |          |    Campo |
| Entrega  |          |          |          |          |     obr  |
|          |          |          |          |          | igatório |
|          |          |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Vítima |
|          |          |          |          |          |     R    |
|          |          |          |          |          | epassada |
|          |          |          |          |          |          |
|          |          |          |          |          |   Para\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |     \"   |
|          |          |          |          |          | Entregue |
|          |          |          |          |          |     a    |
|          |          |          |          |          |          |
|          |          |          |          |          |    outro |
|          |          |          |          |          |          |
|          |          |          |          |          | órgão\". |
+----------+----------+----------+----------+----------+----------+
| Data de  | data de  | Data     | Não      | 10       | -        |
| Entrega  | entrega  |          |          |          |  Formato |
| no Órgão | ao órgão |          |          |          |     e    |
|          |          |          |          |          | sperado: |
|          |          |          |          |          |     dd   |
|          |          |          |          |          | /MM/aaaa |
+----------+----------+----------+----------+----------+----------+
| Hora de  | hora de  | Hora     | Não      | 10       | -        |
| Entrega  | entrega  |          |          |          |  Formato |
| no Órgão | ao órgão |          |          |          |     e    |
|          |          |          |          |          | sperado: |
|          |          |          |          |          |          |
|          |          |          |          |          |    HH:MM |
+----------+----------+----------+----------+----------+----------+
| Houve    | In       | Booleano | Não      | 1        | -   Caso |
| Óbito no | dicativo |          |          |          |     não  |
| Local    | se houve |          |          |          |     seja |
|          | óbito no |          |          |          |          |
|          | local    |          |          |          | enviado, |
|          |          |          |          |          |     co   |
|          |          |          |          |          | nsiderar |
|          |          |          |          |          |     por  |
|          |          |          |          |          |          |
|          |          |          |          |          |   padrão |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |    valor |
|          |          |          |          |          |     NÃO  |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |  -   Sim |
|          |          |          |          |          |          |
|          |          |          |          |          |          |
|          |          |          |          |          |  -   Não |
+----------+----------+----------+----------+----------+----------+
| Tipo     | Tipo de  | Texto    | \*Sim    | 6        | -        |
| Con      | con      |          |          |          |    Campo |
| statação | statação |          |          |          |     obr  |
| do Óbito | de óbito |          |          |          | igatório |
|          |          |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |     há   |
|          |          |          |          |          |          |
|          |          |          |          |          |    óbito |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   local. |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |  Valores |
|          |          |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Con  |
|          |          |          |          |          | statação |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Óbito\". |
+----------+----------+----------+----------+----------+----------+
| Data do  | Data do  | Data     | \*Sim    | 10       | -        |
| Óbito    | óbito    |          |          |          |    Campo |
|          |          |          |          |          |     obr  |
|          |          |          |          |          | igatório |
|          |          |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |     há   |
|          |          |          |          |          |          |
|          |          |          |          |          |    óbito |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   local. |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |  Formato |
|          |          |          |          |          |     e    |
|          |          |          |          |          | sperado: |
|          |          |          |          |          |     dd   |
|          |          |          |          |          | /MM/aaaa |
+----------+----------+----------+----------+----------+----------+
| Hora do  | Hora do  | Hora     | \*Sim    | 10       | -        |
| Óbito    | óbito    |          |          |          |    Campo |
|          |          |          |          |          |     obr  |
|          |          |          |          |          | igatório |
|          |          |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |     há   |
|          |          |          |          |          |          |
|          |          |          |          |          |    óbito |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   local. |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |  Formato |
|          |          |          |          |          |     e    |
|          |          |          |          |          | sperado: |
|          |          |          |          |          |          |
|          |          |          |          |          |    HH:mm |
+----------+----------+----------+----------+----------+----------+
| Nome     | Nome do  | Texto    | \*Sim    | 100      | -        |
| Médico   | médico   |          |          |          |    Campo |
| que      | que      |          |          |          |     obr  |
| atestou  | atestou  |          |          |          | igatório |
| o óbito  | o óbito  |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |   \"Tipo |
|          |          |          |          |          |     do   |
|          |          |          |          |          |          |
|          |          |          |          |          |  Óbito\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |    \"Por |
|          |          |          |          |          |          |
|          |          |          |          |          | médico\" |
+----------+----------+----------+----------+----------+----------+
| CRM do   | CRM do   | Texto    | \*Sim    | 30       | -        |
| Médico   | médico   |          |          |          |    Campo |
| que      | que      |          |          |          |     obr  |
| atestou  | atestou  |          |          |          | igatório |
| o óbito  | o óbito  |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |     o    |
|          |          |          |          |          |          |
|          |          |          |          |          |   \"Tipo |
|          |          |          |          |          |     do   |
|          |          |          |          |          |          |
|          |          |          |          |          |  Óbito\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |    \"Por |
|          |          |          |          |          |          |
|          |          |          |          |          | médico\" |
+----------+----------+----------+----------+----------+----------+
| *        |          |          |          |          |          |
| *Unidade |          |          |          |          |          |
| de Saúde |          |          |          |          |          |
| que      |          |          |          |          |          |
| recebeu  |          |          |          |          |          |
| o        |          |          |          |          |          |
| pa       |          |          |          |          |          |
| ciente** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Nome     | nome da  | Texto    | Não      | 200      |          |
|          | unidade  |          |          |          |          |
|          | de saúde |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Ficha    | ficha    | Texto    | Não      | 200      |          |
| ho       | ho       |          |          |          |          |
| spitalar | spitalar |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Nome do  | Nome do  | Texto    | Não      | 200      |          |
| médico   | médico   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+

[[934682: RN - Produto Perigoso -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_srnXgGC7EfCirqqtwbaCiw)**Produto
Perigoso**

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **Obrig  | **T      | **Regras |
| do       | crição** |          | atório** | amanho** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| **Dados  |          |          |          |          |          |
| do       |          |          |          |          |          |
| P        |          |          |          |          |          |
| roduto** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Produto  | Nome do  | Lista    | Sim      | 10       | -        |
| Perigoso | pr       | Código   |          |          |  Valores |
|          | oduto/Nº | Texto    |          |          |     fo   |
|          | da       |          |          |          | rnecidos |
|          | ON       |          |          |          |          |
|          | U/Número |          |          |          |    pelos |
|          | de risco |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Produto |
|          |          |          |          |          |     Per  |
|          |          |          |          |          | igoso\". |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Tipo de  | Texto    | Sim      | 3        | -        |
| Produto  | produto  |          |          |          |  Valores |
| Perigoso | per      |          |          |          |     fo   |
|          | igoso(bi |          |          |          | rnecidos |
|          | ológico, |          |          |          |          |
|          | radioat  |          |          |          |    pelos |
|          | ivo\...) |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |  Produto |
|          |          |          |          |          |     Per  |
|          |          |          |          |          | igoso\". |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Tipo de  | Texto    | Sim      | 3        | -        |
| Re       | re       |          |          |          |  Valores |
| cipiente | cipiente |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Recip |
|          |          |          |          |          | iente\". |
+----------+----------+----------+----------+----------+----------+
| Subtipo  | Subtipo  | Texto    | Não      | 3        | -        |
| de       | de       |          |          |          |  Valores |
| Re       | Re       |          |          |          |     fo   |
| cipiente | cipiente |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Subtipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Recip |
|          |          |          |          |          | iente\". |
+----------+----------+----------+----------+----------+----------+
| Volume   | Volume   | Numérico | Não      | 6        |          |
| Estimado | estimado |          |          |          |          |
| de       | de       |          |          |          |          |
| V        | v        |          |          |          |          |
| azamento | azamento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Unidade  | Unidade  | Numérico | Não      | 4        | Valores  |
| de       | de       |          |          |          | fo       |
| Medida   | medida   |          |          |          | rnecidos |
| do       | do       |          |          |          | pelos    |
| Volume   | v        |          |          |          | serviços |
| Estimado | azamento |          |          |          | dis      |
|          |          |          |          |          | poníveis |
|          |          |          |          |          | no       |
|          |          |          |          |          | Sinesp   |
|          |          |          |          |          | Dados    |
|          |          |          |          |          | Corpo    |
|          |          |          |          |          | rativos: |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          | Unidade  |
|          |          |          |          |          | de       |
|          |          |          |          |          | M        |
|          |          |          |          |          | edida\". |
+----------+----------+----------+----------+----------+----------+
| Volume   | Volume   | Numérico | Não      | 8        |          |
| Estimado | estimado |          |          |          |          |
| do       | do       |          |          |          |          |
| Re       | re       |          |          |          |          |
| cipiente | cipiente |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Unidade  | Unidade  | Numérico | Não      | 4        | Valores  |
| de       | de       |          |          |          | fo       |
| medida   | medida   |          |          |          | rnecidos |
| do       | do       |          |          |          | pelos    |
| Re       | re       |          |          |          | serviços |
| cipiente | cipiente |          |          |          | dis      |
|          |          |          |          |          | poníveis |
|          |          |          |          |          | no       |
|          |          |          |          |          | Sinesp   |
|          |          |          |          |          | Dados    |
|          |          |          |          |          | Corpo    |
|          |          |          |          |          | rativos: |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          | Unidade  |
|          |          |          |          |          | de       |
|          |          |          |          |          | M        |
|          |          |          |          |          | edida\". |
+----------+----------+----------+----------+----------+----------+
| Volume   | Volume   | Numérico | Não      | 8        |          |
| Estimado | estimado |          |          |          |          |
| do       | do       |          |          |          |          |
| Produto  | produto  |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Unidade  | unidade  | Numérico | Não      | 4        |          |
| de       | de       |          |          |          |          |
| Medida   | medida   |          |          |          |          |
| do       | do       |          |          |          |          |
| Volume   | produto  |          |          |          |          |
| Estimado |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Estado   | Estado   | Texto    | Não      | 3        | -        |
| Físico   | físico   |          |          |          |  Valores |
| do       | do       |          |          |          |     fo   |
| Produto  | produto  |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Estado |
|          |          |          |          |          |          |
|          |          |          |          |          |   Físico |
|          |          |          |          |          |     do   |
|          |          |          |          |          |     Pr   |
|          |          |          |          |          | oduto\". |
+----------+----------+----------+----------+----------+----------+
| De       | De       | Lista    | Não      | 3        | -        |
| stinação | stinação | Código   |          |          |  Valores |
| do       | do       | Texto    |          |          |     fo   |
| Produto  | Produto  |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     De   |
|          |          |          |          |          | stinação |
|          |          |          |          |          |     do   |
|          |          |          |          |          |     Pr   |
|          |          |          |          |          | oduto\". |
+----------+----------+----------+----------+----------+----------+
| Outra    | D        | Lista    | Não      | 50       |          |
| De       | escrição | Numérica |          |          |          |
| stinação | da       |          |          |          |          |
| do       | de       |          |          |          |          |
| Produto  | stinação |          |          |          |          |
|          | do       |          |          |          |          |
|          | produto  |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| **Área   |          |          |          |          |          |
| At       |          |          |          |          |          |
| ingida** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Área     | Tamanho  | Numérico | Sim      | 6        |          |
| Isolada  | da área  |          |          |          |          |
|          | isolada  |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Unidade  | Unidade  | Numérico | Sim      | 6        | -        |
| da Área  | de       |          |          |          |  Valores |
| Isolada  | medida   |          |          |          |     fo   |
|          | da área  |          |          |          | rnecidos |
|          | iso      |          |          |          |          |
|          | lada(m2, |          |          |          |    pelos |
|          | km2,     |          |          |          |          |
|          | qu       |          |          |          | serviços |
|          | adras..) |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Unidade |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     M    |
|          |          |          |          |          | edida\". |
+----------+----------+----------+----------+----------+----------+
| Área     | Tamanho  | Numérico | Sim      | 6        |          |
| Con      | da área  |          |          |          |          |
| taminada | con      |          |          |          |          |
|          | taminada |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Unidade  | Unidade  | Numérico | Sim      | 6        | -        |
| da área  | de       |          |          |          |  Valores |
| con      | medida   |          |          |          |     fo   |
| taminada | da área  |          |          |          | rnecidos |
|          | con      |          |          |          |          |
|          | taminada |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Unidade |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     M    |
|          |          |          |          |          | edida\". |
+----------+----------+----------+----------+----------+----------+
| Área de  | Tamanho  | Numérico | Sim      | 6        |          |
| Abandono | da área  |          |          |          |          |
|          | de       |          |          |          |          |
|          | abandono |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Unidade  | Unidade  | Numérico | \*Sim    | 6        | -        |
| da Área  | de       |          |          |          |    Campo |
| Abandono | medidade |          |          |          |     obr  |
|          | da área  |          |          |          | igatório |
|          | de       |          |          |          |     se   |
|          | abandono |          |          |          |     i    |
|          |          |          |          |          | nformado |
|          |          |          |          |          |     área |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          | abandono |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |  Valores |
|          |          |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
+----------+----------+----------+----------+----------+----------+
| Ambiente | D        | Texto    | Não      | 400      |          |
| Afetado  | escrição |          |          |          |          |
|          | do       |          |          |          |          |
|          | ambiente |          |          |          |          |
|          | afetado  |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Causa V  | Lista de | Lista    | Sim      | 3        | -        |
| azamento | causas   | Código   |          |          |  Valores |
|          | p        | Texto    |          |          |     fo   |
|          | rováveis |          |          |          | rnecidos |
|          | do       |          |          |          |          |
|          | v        |          |          |          |    pelos |
|          | azamento |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Causa de |
|          |          |          |          |          |     Vaz  |
|          |          |          |          |          | amento\" |
+----------+----------+----------+----------+----------+----------+
| **Aç     |          |          |          |          |          |
| ão(ções) |          |          |          |          |          |
| Realiz   |          |          |          |          |          |
| ada(s)** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Nome da  | Lista de | Lista    | Não      | 3        | -        |
| Ação     | ações    | Código   |          |          |  Valores |
|          | re       | Texto    |          |          |     fo   |
|          | alizadas |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Ações |
|          |          |          |          |          |     Real |
|          |          |          |          |          | izadas\" |
+----------+----------+----------+----------+----------+----------+

[[934695: RN - Atendimento Comunitário -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_DaCI0GC_EfCirqqtwbaCiw)**Atendimento
Comunitário**

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **Obrig  | **T      | **Regras |
| do       | crição** |          | atório** | amanho** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Es       | Es       | Numérico | Não      | 8        |          |
| timativa | timativa |          |          |          |          |
| de       | de       |          |          |          |          |
| Público  | público  |          |          |          |          |
| Atingida | no       |          |          |          |          |
|          | ate      |          |          |          |          |
|          | ndimento |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Duração  | Tempo de | Numérico | Não      | 8        |          |
| do       | duração  |          |          |          |          |
| Evento   | do       |          |          |          |          |
|          | evento   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Unidade  | Unidade  | Numérico | Não      | 8        | -        |
| da       | de       |          |          |          |  Valores |
| Duração  | Temp     |          |          |          |     fo   |
| do       | o(horas, |          |          |          | rnecidos |
| Evento   | dias)    |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |  Unidade |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     M    |
|          |          |          |          |          | edida\". |
+----------+----------+----------+----------+----------+----------+
| **Aç     |          |          |          |          |          |
| ão(ções) |          |          |          |          |          |
| Realiz   |          |          |          |          |          |
| ada(s)** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Ação     | A        | Lista    | Sim      | 8        | -        |
| R        | ção(ões) | Numérica |          |          |  Valores |
| ealizada | real     |          |          |          |     fo   |
|          | izada(s) |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Ações |
|          |          |          |          |          |          |
|          |          |          |          |          |    Reali |
|          |          |          |          |          | zadas\". |
+----------+----------+----------+----------+----------+----------+

[[934702: RN - Desastre -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_3Q1bAGDBEfCirqqtwbaCiw)**Desastre**

+----------+----------+----------+----------+----------+----------+---+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |   |
| do       | crição** |          | amanho** | atório** | de       |   |
| Campo**  |          |          |          |          | Receb    |   |
|          |          |          |          |          | imento** |   |
+----------+----------+----------+----------+----------+----------+---+
| Áreas    | d        | Texto    | 500      | Não      |          |   |
| Afetadas | escrição |          |          |          |          |   |
|          | das      |          |          |          |          |   |
|          | áreas    |          |          |          |          |   |
|          | afetadas |          |          |          |          |   |
|          | pelo     |          |          |          |          |   |
|          | desastre |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Causas   | d        | Texto    | 500      | Não      |          |   |
|          | escrição |          |          |          |          |   |
|          | das      |          |          |          |          |   |
|          | causas   |          |          |          |          |   |
|          | do       |          |          |          |          |   |
|          | desastre |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Porte do | porte do | Texto    | 3        | Não      | -        |   |
| Evento   | evento   |          |          |          |  Valores |   |
|          | (grande, |          |          |          |     fo   |   |
|          | pequeno, |          |          |          | rnecidos |   |
|          | médio..) |          |          |          |          |   |
|          |          |          |          |          |    pelos |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          | serviços |   |
|          |          |          |          |          |     dis  |   |
|          |          |          |          |          | poníveis |   |
|          |          |          |          |          |     no   |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |   Sinesp |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |    Dados |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |    Corpo |   |
|          |          |          |          |          | rativos: |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          | \"Listar |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |    Porte |   |
|          |          |          |          |          |     do   |   |
|          |          |          |          |          |     E    |   |
|          |          |          |          |          | vento\". |   |
+----------+----------+----------+----------+----------+----------+---+
| **Danos  |          |          |          |          |          |   |
| H        |          |          |          |          |          |   |
| umanos** |          |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Pessoas  | Qu       | Numérico | 6        | Não      |          |   |
| Desa     | antidade |          |          |          |          |   |
| brigadas | de       |          |          |          |          |   |
|          | Pessoas  |          |          |          |          |   |
|          | Desa     |          |          |          |          |   |
|          | brigadas |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Pessoas  | Qu       | Numérico | 6        | Não      |          |   |
| Des      | antidade |          |          |          |          |   |
| alojadas | de       |          |          |          |          |   |
|          | Pessoas  |          |          |          |          |   |
|          | Des      |          |          |          |          |   |
|          | alojadas |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Pessoas  | Qu       | Numérico | 6        | Não      |          |   |
| Desap    | antidade |          |          |          |          |   |
| arecidas | de       |          |          |          |          |   |
|          | Pessoas  |          |          |          |          |   |
|          | Desap    |          |          |          |          |   |
|          | arecidas |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Pessoas  | Qu       | Numérico | 6        | Não      |          |   |
| Mortas   | antidade |          |          |          |          |   |
|          | de       |          |          |          |          |   |
|          | Pessoas  |          |          |          |          |   |
|          | Mortas   |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Pessoas  | Qu       | Numérico | 6        | Não      |          |   |
| Enfermas | antidade |          |          |          |          |   |
|          | de       |          |          |          |          |   |
|          | Pessoas  |          |          |          |          |   |
|          | Enfermas |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Pessoas  | Qu       | Numérico | 6        | Não      |          |   |
| com      | antidade |          |          |          |          |   |
| Feridas  | de       |          |          |          |          |   |
| Leves    | Pessoas  |          |          |          |          |   |
|          | com      |          |          |          |          |   |
|          | Feridas  |          |          |          |          |   |
|          | Leves    |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Pessoas  | Qu       | Numérico | 6        | Não      |          |   |
| com      | antidade |          |          |          |          |   |
| Feridas  | de       |          |          |          |          |   |
| Graves   | Pessoas  |          |          |          |          |   |
|          | com      |          |          |          |          |   |
|          | Feridas  |          |          |          |          |   |
|          | Graves   |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Pessoas  | Qu       | Numérico | 6        | Não      |          |   |
| Afetadas | antidade |          |          |          |          |   |
| com      | de       |          |          |          |          |   |
| Outras   | Pessoas  |          |          |          |          |   |
| Opções   | Efetadas |          |          |          |          |   |
|          | com      |          |          |          |          |   |
|          | Outras   |          |          |          |          |   |
|          | Opções   |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| **Danos  |          |          |          |          |          |   |
| Mat      |          |          |          |          |          |   |
| eriais** |          |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Qu       | qu       | Numérico | 6        | Não      |          |   |
| antidade | antidade |          |          |          |          |   |
| de       | de       |          |          |          |          |   |
| Res      | res      |          |          |          |          |   |
| idências | idências |          |          |          |          |   |
|          | afetadas |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Qu       | qu       | Numérico | 6        | Não      |          |   |
| antidade | antidade |          |          |          |          |   |
| de       | de       |          |          |          |          |   |
| C        | c        |          |          |          |          |   |
| omércios | omércios |          |          |          |          |   |
|          | afetados |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Qu       | qu       | Numérico | 6        | Não      |          |   |
| antidade | antidade |          |          |          |          |   |
| de       | de       |          |          |          |          |   |
| In       | in       |          |          |          |          |   |
| dústrias | dústrias |          |          |          |          |   |
|          | afetadas |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Qu       | qu       | Numérico | 6        | Não      |          |   |
| antidade | antidade |          |          |          |          |   |
| de       | de       |          |          |          |          |   |
| Prédios  | prédios  |          |          |          |          |   |
| Públicos | públicos |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Qu       | qu       | Numérico | 6        | Não      |          |   |
| antidade | antidade |          |          |          |          |   |
| de       | de       |          |          |          |          |   |
| Inst     | inst     |          |          |          |          |   |
| ituições | ituições |          |          |          |          |   |
| de       | de       |          |          |          |          |   |
| Ensino   | ensino   |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Qu       | Qu       | Numérico | 6        | Não      |          |   |
| antidade | antidade |          |          |          |          |   |
| de       | de       |          |          |          |          |   |
| Estabele | estabele |          |          |          |          |   |
| cimentos | cimentos |          |          |          |          |   |
| de Saúde | de saúde |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Qu       | Qu       | Numérico | 6        | Não      |          |   |
| antidade | antidade |          |          |          |          |   |
| de Obras | de obras |          |          |          |          |   |
| de Arte  | de arte  |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Qu       | Qu       | Numérico | 6        | Não      |          |   |
| antidade | antidade |          |          |          |          |   |
| de       | de       |          |          |          |          |   |
| Estradas | estradas |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Qu       | Qu       | Numérico | 6        | Não      |          |   |
| antidade | antidade |          |          |          |          |   |
| de Danos | de danos |          |          |          |          |   |
| Am       | am       |          |          |          |          |   |
| bientais | bientais |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| **Danos  |          |          |          |          |          |   |
| Ambi     |          |          |          |          |          |   |
| entais** |          |          |          |          |          |   |
+----------+----------+----------+----------+----------+----------+---+
| Danos    | Lista de | Lista    | 3        | Não      | -        |   |
| Am       | danos    | Código   |          |          |  Valores |   |
| bientais | am       | Texto    |          |          |     fo   |   |
|          | bientais |          |          |          | rnecidos |   |
|          | do       |          |          |          |          |   |
|          | desastre |          |          |          |    pelos |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          | serviços |   |
|          |          |          |          |          |     dis  |   |
|          |          |          |          |          | poníveis |   |
|          |          |          |          |          |     no   |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |   Sinesp |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |    Dados |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |   Corpor |   |
|          |          |          |          |          | ativos:  |   |
|          |          |          |          |          | \"Listar |   |
|          |          |          |          |          |     Tipo |   |
|          |          |          |          |          |     de   |   |
|          |          |          |          |          |     Dano |   |
|          |          |          |          |          |     Ambi |   |
|          |          |          |          |          | ental\". |   |
+----------+----------+----------+----------+----------+----------+---+
| Serviços | Lista de | Lista    | 3        | Não      | -        |   |
| Afetados | serviços | Código   |          |          |  Valores |   |
|          | afetados | Texto    |          |          |     fo   |   |
|          |          |          |          |          | rnecidos |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |    pelos |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          | serviços |   |
|          |          |          |          |          |     dis  |   |
|          |          |          |          |          | poníveis |   |
|          |          |          |          |          |     no   |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |   Sinesp |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |    Dados |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |   Corpor |   |
|          |          |          |          |          | ativos:  |   |
|          |          |          |          |          | \"Listar |   |
|          |          |          |          |          |     Tipo |   |
|          |          |          |          |          |     de   |   |
|          |          |          |          |          |          |   |
|          |          |          |          |          |  Serviço |   |
|          |          |          |          |          |     Af   |   |
|          |          |          |          |          | etado\". |   |
+----------+----------+----------+----------+----------+----------+---+

[[934714: RN - Vistoria Técnica -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX__2SskGDPEfCirqqtwbaCiw)**Vistoria
Técnica**

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Pessoa   | Ident    | Numérico | 4        | Não      |          |
| Jurídica | ificador |          |          |          |          |
|          | da       |          |          |          |          |
|          | Pessoa   |          |          |          |          |
|          | Jurídica |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Tipo de  | Texto    | 3        | Sim      | -        |
| Vistoria | Vistor   |          |          |          |  Valores |
|          | ia(Fisca |          |          |          |     fo   |
|          | lização, |          |          |          | rnecidos |
|          | regula   |          |          |          |          |
|          | rização, |          |          |          |    pelos |
|          | oper     |          |          |          |          |
|          | acional) |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Vis  |
|          |          |          |          |          | toria\". |
+----------+----------+----------+----------+----------+----------+
| Motivo   | Motivo   | Texto    | 3        | Sim      | -        |
| da       | da       |          |          |          |  Valores |
| Vistoria | vistori  |          |          |          |     fo   |
|          | a(regula |          |          |          | rnecidos |
|          | rização, |          |          |          |          |
|          | den      |          |          |          |    pelos |
|          | úncia..) |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |   Motivo |
|          |          |          |          |          |     da   |
|          |          |          |          |          |     Vis  |
|          |          |          |          |          | toria\". |
+----------+----------+----------+----------+----------+----------+
| Simpl    | In       | Booleano | 1        | Sim      |          |
| ificação | dicativo |          |          |          |          |
| /isenção | de       |          |          |          |          |
| do       | simpl    |          |          |          |          |
| processo | ificação |          |          |          |          |
| e        | do       |          |          |          |          |
| s        | processo |          |          |          |          |
| egurança |          |          |          |          |          |
| contra   |          |          |          |          |          |
| incêndio |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Opção de | Opção de | Texto    | 3        | \*Não    | -   Obr  |
| Isenção  | isenção  |          |          |          | igatório |
|          |          |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |  \"Simpl |
|          |          |          |          |          | ificação |
|          |          |          |          |          | /isenção |
|          |          |          |          |          |     do   |
|          |          |          |          |          |     pr   |
|          |          |          |          |          | ocesso\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |     SIM. |
|          |          |          |          |          |          |
|          |          |          |          |          | -        |
|          |          |          |          |          |  Valores |
|          |          |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Opção |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |  Isenção |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Vis  |
|          |          |          |          |          | toria\". |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Texto    | 20       | Não      |          |
| do       | do       |          |          |          |          |
| Processo | processo |          |          |          |          |
| de       | de       |          |          |          |          |
| P        | p        |          |          |          |          |
| revenção | revenção |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Data de  | Data de  | Data     | 10       | Não      | -        |
| C        | c        |          |          |          |  Formato |
| oncessão | oncessão |          |          |          |     E    |
| do       | do       |          |          |          | sperado: |
| Alvará   | alvará   |          |          |          |     dd   |
|          |          |          |          |          | /mm/aaaa |
+----------+----------+----------+----------+----------+----------+
| Ocupação | Tipo de  | Texto    | 3        | Não      | -        |
|          | ocupação |          |          |          |  Valores |
|          | do       |          |          |          |     fo   |
|          | imóvel   |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Uso/ |
|          |          |          |          |          | Ocupação |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Edifi |
|          |          |          |          |          | cação\". |
+----------+----------+----------+----------+----------+----------+
| Número   | Número   | Numérico | 3        | Não      |          |
| do       | de       |          |          |          |          |
| Pa       | pa       |          |          |          |          |
| vimentos | vimentos |          |          |          |          |
|          | vis      |          |          |          |          |
|          | toriados |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Área(m²) | área     | Numérico | 6        | Não      |          |
|          | a        |          |          |          |          |
|          | brangida |          |          |          |          |
|          | pela     |          |          |          |          |
|          | vistoria |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Obs      | obs      | Texto    | 250      | Não      |          |
| ervações | ervações |          |          |          |          |
|          | da       |          |          |          |          |
|          | vistoria |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| R        | Lista de | Lista    | 3        | Não      | -        |
| esultado | re       | Código   |          |          |  Valores |
| da Ação  | sultados | Texto    |          |          |     fo   |
| de       | da       |          |          |          | rnecidos |
| Vistoria | vistoria |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     R    |
|          |          |          |          |          | esultado |
|          |          |          |          |          |     da   |
|          |          |          |          |          |     Ação |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Vis  |
|          |          |          |          |          | toria\". |
+----------+----------+----------+----------+----------+----------+
| Outro    | Outro    | Texto    | 50       | Não      |          |
| R        | r        |          |          |          |          |
| esultado | esultado |          |          |          |          |
| da Ação  | da ação  |          |          |          |          |
| de       | de       |          |          |          |          |
| Vistoria | vistoria |          |          |          |          |
|          | quando a |          |          |          |          |
|          | opção de |          |          |          |          |
|          | \"R      |          |          |          |          |
|          | esultado |          |          |          |          |
|          | da ação  |          |          |          |          |
|          | da       |          |          |          |          |
|          | vi       |          |          |          |          |
|          | storia\" |          |          |          |          |
|          | for      |          |          |          |          |
|          | OUTRO.   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| **Irr    |          |          |          |          |          |
| egularid |          |          |          |          |          |
| ade(s)** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Lista de | Lista    | 3        | Não      | -        |
| Irregu   | medidas  | Código   |          |          |  Valores |
| laridade | de       | Texto    |          |          |     fo   |
|          | s        |          |          |          | rnecidos |
|          | egurança |          |          |          |          |
|          | para     |          |          |          |    pelos |
|          | descriçã |          |          |          |          |
|          | das      |          |          |          | serviços |
|          | irregul  |          |          |          |     dis  |
|          | aridades |          |          |          | poníveis |
|          | adotadas |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |   Corpor |
|          |          |          |          |          | ativos:  |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Irregu |
|          |          |          |          |          | laridade |
|          |          |          |          |          |     da   |
|          |          |          |          |          |     Vis  |
|          |          |          |          |          | toria\". |
+----------+----------+----------+----------+----------+----------+
| D        | Campo    | Texto    | 200      | Não      |          |
| escrição | livre    |          |          |          |          |
| das      | para     |          |          |          |          |
| Irregul  | d        |          |          |          |          |
| aridades | escrever |          |          |          |          |
|          | as       |          |          |          |          |
|          | irregul  |          |          |          |          |
|          | aridades |          |          |          |          |
|          | enc      |          |          |          |          |
|          | ontradas |          |          |          |          |
+----------+----------+----------+----------+----------+----------+

 \
**Arquivos/Formulário em Anexo**\
Para indicar que o Boletim de Ocorrência tem arquivos em anexo, poderá
ser colocado uma lista de arquivos com a seguinte descrição:\
\
[[936220: RN - Documentos Anexos ao Procedimento -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_gCZx0GieEfCirqqtwbaCiw)

  ------------------------------- ------------------------------------------------------------------------------ ---------- ----------------- ------------- ---------------------------------------
  **Nome do Campo**               **Descrição**                                                                  **Tipo**   **Obrigatório**   **Tamanho**   **Regras de Recebimento**

  Descrição do Arquivo em Anexo   Descrição do Documento em Anexo                                                Texto      Sim               100            

  Tipo de Arquivo em Anexo        Tipo de Arquivo em Anexo ao B.O(Imagem, Documento, Formulário, Áudio, Vídeo)   Numérico   Sim               6              

  Arquivo                         Arquivo em anexo                                                               Arquivo    Sim                             \- Tamanho máximo: \<\<a definir\>\>\
                                                                                                                                                            - Formatos: \<\<a definir\>\>
  ------------------------------- ------------------------------------------------------------------------------ ---------- ----------------- ------------- ---------------------------------------

 \
**Dados Utilizados pelo BOAT (BOLETIM DE ACIDENTE DE TRÂNSITO)**\
 \
[[936177: RN - BOAT - Informações do BOAT -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_15oYEGiDEfCirqqtwbaCiw)

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Natureza | Natureza | Numérico | 6        | Sim      | -        |
| do       | do       |          |          |          |  Valores |
| Acidente | acidente |          |          |          |     fo   |
|          | de       |          |          |          | rnecidos |
|          | trânsito |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     N    |
|          |          |          |          |          | aturezas |
|          |          |          |          |          |          |
|          |          |          |          |          |    Ocorr |
|          |          |          |          |          | ência\". |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Texto    | 3        | Não      | -        |
| dicativo | dicativo |          |          |          |  Valores |
| de       | de       |          |          |          |     fo   |
| Suspeita | suspeita |          |          |          | rnecidos |
| de       | de       |          |          |          |          |
| Álcool   | álcool   |          |          |          |    pelos |
|          | quando   |          |          |          |          |
|          | no       |          |          |          | serviços |
|          | momento  |          |          |          |     dis  |
|          | do       |          |          |          | poníveis |
|          | acidente |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Suspeita |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Á    |
|          |          |          |          |          | lcool\". |
+----------+----------+----------+----------+----------+----------+
| In       | In       | Texto    | 3        | Não      | -        |
| dicativo | dicativo |          |          |          |  Valores |
| Exame    | se exame |          |          |          |     fo   |
| A        | a        |          |          |          | rnecidos |
| lcoolico | lcóolico |          |          |          |          |
| r        | foi      |          |          |          |    pelos |
| ealizado | r        |          |          |          |          |
|          | ealizado |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Exame |
|          |          |          |          |          |     A    |
|          |          |          |          |          | lcoólico |
|          |          |          |          |          |     Real |
|          |          |          |          |          | izado\". |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Tipo de  | Lista    | 3        | Sim      | -        |
| Acidente | acidente | Código   |          |          |  Valores |
|          | registra | Texto    |          |          |     fo   |
|          | do(Atrop |          |          |          | rnecidos |
|          | elamento |          |          |          |          |
|          | com      |          |          |          |    pelos |
|          | animal,  |          |          |          |          |
|          | Capo     |          |          |          | serviços |
|          | tamento, |          |          |          |     dis  |
|          | coli     |          |          |          | poníveis |
|          | são\...) |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Acidente |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Trâ  |
|          |          |          |          |          | nsito\". |
+----------+----------+----------+----------+----------+----------+
| D        | D        | Texto    | 100      | Não      |          |
| escrição | escrição |          |          |          |          |
| do Tipo  | do tipo  |          |          |          |          |
| de       | de       |          |          |          |          |
| Acidente | acidente |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| C        | Lista    | Lista    | 3        | Sim      | -        |
| ondições | com as   | Código   |          |          |  Valores |
| Metere   | c        | Texto    |          |          |     fo   |
| ológicas | ondições |          |          |          | rnecidos |
|          | me       |          |          |          |          |
|          | tereológ |          |          |          |    pelos |
|          | icas(Céu |          |          |          |          |
|          | claro,   |          |          |          | serviços |
|          | chuva,   |          |          |          |     dis  |
|          | ga       |          |          |          | poníveis |
|          | roa\...) |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     C    |
|          |          |          |          |          | ondição  |
|          |          |          |          |          | Metereol |
|          |          |          |          |          | ógica\". |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Tipo de  | Lista de | 6        | Não      |          |
| P        | acidente | (Pa      |          |          |          |
| avimento | re       | vimento) |          |          |          |
|          | gistrado |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| C        | Lista    | Lista    | 3        | Sim      | -        |
| ondições | com as   | Código   |          |          |  Valores |
| da Via   | c        | Texto    |          |          |     fo   |
|          | ondições |          |          |          | rnecidos |
|          | da       |          |          |          |          |
|          | Via.(    |          |          |          |    pelos |
|          | Alagada, |          |          |          |          |
|          | com      |          |          |          | serviços |
|          | areia,   |          |          |          |     dis  |
|          | com      |          |          |          | poníveis |
|          | bur      |          |          |          |     no   |
|          | aco\...) |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Con  |
|          |          |          |          |          | dição da |
|          |          |          |          |          |          |
|          |          |          |          |          |   Via\". |
+----------+----------+----------+----------+----------+----------+
| Traçado  | Lista de | Lista    | 3        | Sim      | -        |
| da Via   | traçados | Código   |          |          |  Valores |
|          | da       | Texto    |          |          |     fo   |
|          | via.(Bif |          |          |          | rnecidos |
|          | urcação, |          |          |          |          |
|          | cru      |          |          |          |    pelos |
|          | zamento, |          |          |          |          |
|          | cu       |          |          |          | serviços |
|          | rva\...) |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |  Traçado |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Via\". |
+----------+----------+----------+----------+----------+----------+
| Outro    | D        | Texto    | 50       | Não      |          |
| Traçado  | escrição |          |          |          |          |
| da Via   | de       |          |          |          |          |
|          | Traçado  |          |          |          |          |
|          | da via   |          |          |          |          |
|          | quando   |          |          |          |          |
|          | \        |          |          |          |          |
|          | "Traçado |          |          |          |          |
|          | da Via\" |          |          |          |          |
|          | for      |          |          |          |          |
|          | \"       |          |          |          |          |
|          | OUTRO\". |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Tipo de  | Texto    | 3        | Sim      | -        |
| Lumi     | Lu       |          |          |          |  Valores |
| nosidade | minosida |          |          |          |     fo   |
|          | de.(Aman |          |          |          | rnecidos |
|          | hecendo, |          |          |          |          |
|          | anoi     |          |          |          |    pelos |
|          | tecendo, |          |          |          |          |
|          | il       |          |          |          | serviços |
|          | uminação |          |          |          |     dis  |
|          | i        |          |          |          | poníveis |
|          | nsuficie |          |          |          |     no   |
|          | nte\...) |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |  Luminos |
|          |          |          |          |          | idade\". |
+----------+----------+----------+----------+----------+----------+
| **Sinal  |          |          |          |          |          |
| ização** |          |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Faixa de | In       | Booleano | 1        | Não      |          |
| Pedestre | dicativo |          |          |          |          |
|          | se tem   |          |          |          |          |
|          | faixa de |          |          |          |          |
|          | pedestre |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Faixas   | In       | Booleano | 1        | Não      |          |
| ou       | dicativo |          |          |          |          |
| Pistas   | se há    |          |          |          |          |
| Ex       | faixas   |          |          |          |          |
| clusivas | ex       |          |          |          |          |
| de       | clusivas |          |          |          |          |
| Ônibus   | de       |          |          |          |          |
|          | ônibus   |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Do       | In       | Booleano | 1        | Não      |          |
| Agente   | dicativo |          |          |          |          |
| de       | se a     |          |          |          |          |
| Trânsito | BOAT     |          |          |          |          |
|          | veio de  |          |          |          |          |
|          | um       |          |          |          |          |
|          | agente   |          |          |          |          |
|          | de       |          |          |          |          |
|          | trânsito |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Lombada  | In       | Booleano | 1        | Não      |          |
| El       | dicativo |          |          |          |          |
| etrônica | de       |          |          |          |          |
|          | lombada  |          |          |          |          |
|          | el       |          |          |          |          |
|          | etrônica |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Condição | Condição | Texto    | 3        | Não      | -        |
| do       | de       |          |          |          |  Valores |
| semáforo | operação |          |          |          |     fo   |
|          | do       |          |          |          | rnecidos |
|          | semáfor  |          |          |          |          |
|          | o.(Pleno |          |          |          |    pelos |
|          | funcio   |          |          |          |          |
|          | namento, |          |          |          | serviços |
|          | Def      |          |          |          |     dis  |
|          | eituoso, |          |          |          | poníveis |
|          | Não      |          |          |          |     no   |
|          | ex       |          |          |          |          |
|          | istente) |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Condição |
|          |          |          |          |          |     do   |
|          |          |          |          |          |     Sem  |
|          |          |          |          |          | áforo\". |
+----------+----------+----------+----------+----------+----------+
| Ve       | Qual a   | Texto    | 3        | Não      | -        |
| locidade | ve       |          |          |          |  Valores |
| Máxima   | locidade |          |          |          |     fo   |
| P        | máxima   |          |          |          | rnecidos |
| ermitida | da       |          |          |          |          |
|          | via.(20, |          |          |          |    pelos |
|          | 30,      |          |          |          |          |
|          | 40\...)  |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Ve   |
|          |          |          |          |          | locidade |
|          |          |          |          |          |          |
|          |          |          |          |          |   Máxima |
|          |          |          |          |          |     da   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Via\". |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Qual o   | Texto    | 3        | Não      | -        |
| Opção de | tipo de  |          |          |          |  Valores |
| Pista    | opção de |          |          |          |     fo   |
|          | pista.   |          |          |          | rnecidos |
|          | (        |          |          |          |          |
|          | Simples, |          |          |          |    pelos |
|          | dupla,   |          |          |          |          |
|          | m        |          |          |          | serviços |
|          | últipla, |          |          |          |     dis  |
|          | auxil    |          |          |          | poníveis |
|          | iar\...) |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Opção |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Pista\". |
+----------+----------+----------+----------+----------+----------+
| Outro    | Envio    | Texto    | 50       | Não      |          |
| Tipo de  | quando o |          |          |          |          |
| Pista    | \"Tipo   |          |          |          |          |
|          | de       |          |          |          |          |
|          | Pista\"  |          |          |          |          |
|          | for      |          |          |          |          |
|          | igual a  |          |          |          |          |
|          | \        |          |          |          |          |
|          | "OUTRO\" |          |          |          |          |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Tipo de  | Texto    | 3        | Não      | -        |
| Linha    | linha da |          |          |          |  Valores |
| Pista    | p        |          |          |          |     fo   |
|          | ista.(Tr |          |          |          | rnecidos |
|          | acejada, |          |          |          |          |
|          | C        |          |          |          |    pelos |
|          | ontínua, |          |          |          |          |
|          | Não      |          |          |          | serviços |
|          | Exi      |          |          |          |     dis  |
|          | stente). |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Linha |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          | Pista\". |
+----------+----------+----------+----------+----------+----------+
| Faixa de | Qual a   | Texto    | 3        | Não      | -        |
| Trânsito | faixa    |          |          |          |  Valores |
|          | onde     |          |          |          |     fo   |
|          | estava o |          |          |          | rnecidos |
|          | veículo  |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Faixa |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Trâ  |
|          |          |          |          |          | nsito\". |
+----------+----------+----------+----------+----------+----------+
| Sin      | D        | Texto    | 400      | Não      |          |
| alização | escrição |          |          |          |          |
| Vertical | da       |          |          |          |          |
|          | sin      |          |          |          |          |
|          | alização |          |          |          |          |
|          | vertical |          |          |          |          |
+----------+----------+----------+----------+----------+----------+

**Lista de Tipo(s) de Pavimento(s)**\
[[936181: RN - BOAT - Pavimento -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_a9N_gGiEEfCirqqtwbaCiw)

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **T      | **Obrig  | **Regras |
| do       | crição** |          | amanho** | atório** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Tipo de  | Tipo de  | Texto    | 3        | Não      | -        |
| P        | acidente |          |          |          |  Valores |
| avimento | r        |          |          |          |     fo   |
|          | egistrad |          |          |          | rnecidos |
|          | o(Areia, |          |          |          |          |
|          | Asfalto, |          |          |          |    pelos |
|          | C        |          |          |          |          |
|          | ascalho, |          |          |          | serviços |
|          | Concr    |          |          |          |     dis  |
|          | eto\...) |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Pavi |
|          |          |          |          |          | mento\". |
+----------+----------+----------+----------+----------+----------+
| Situação | Situação | Texto    | 3        | Não      | -        |
| do       | do       |          |          |          |  Valores |
| P        | pavimen  |          |          |          |     fo   |
| avimento | to(Ruim, |          |          |          | rnecidos |
|          | Regular  |          |          |          |          |
|          | e Bom)   |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          | Situação |
|          |          |          |          |          |     do   |
|          |          |          |          |          |     Pavi |
|          |          |          |          |          | mento\". |
+----------+----------+----------+----------+----------+----------+

 \
Para a identificação do veículo e seus danos no BOAT, deve-se usar da
seguinte estrutura para classificar os danos do veículo:\
[[948718: RN - Identificação Veículo Dano - BOAT -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_lIjLcHxWEfCTPtVxzDe9CA)

  ----------------------- ------------------------------- ----------------------------------------------------------------- ----------------- ------------- ---------------------------
  **Nome do Campo**       **Descrição**                   **Tipo**                                                          **Obrigatório**   **Tamanho**   **Regras de Recebimento**
  Identificador Veículo   Identificação do veículo        Numérico                                                          Sim               4              
  Lista Danos             Listagem com Danos do Veículo   Listagem [Peça ou Componente do Veículo Danificado]{.underline}   Sim               6             
  ----------------------- ------------------------------- ----------------------------------------------------------------- ----------------- ------------- ---------------------------

Para a estrutura de Listagem de [Peça ou Componente do Veículo
Danificado]{.underline}, terá a seguinte estrutura:\
[[948714: RN - Peça ou Componente do Veículo Danificado - BOAT -
Recebimento]{.underline}](https://alm.serpro/rm/resources/TX_CnercHxTEfCTPtVxzDe9CA)

+----------+----------+----------+----------+----------+----------+
| **Nome   | **Des    | **Tipo** | **Obrig  | **T      | **Regras |
| do       | crição** |          | atório** | amanho** | de       |
| Campo**  |          |          |          |          | Receb    |
|          |          |          |          |          | imento** |
+----------+----------+----------+----------+----------+----------+
| Grupo    | Grupo    | Texto    | Não      | 3        | -        |
| Peça     | Peça do  |          |          |          |  Valores |
|          | Veículo  |          |          |          |     fo   |
|          |          |          |          |          | rnecidos |
|          |          |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |          |
|          |          |          |          |          |    Grupo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |          |
|          |          |          |          |          |    Peças |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Co   |
|          |          |          |          |          | mponente |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Ve   |
|          |          |          |          |          | ículo\". |
+----------+----------+----------+----------+----------+----------+
| Tipo     | Tipo     | Texto    | Não      | 3        | -        |
| Peça     | Peça/Co  |          |          |          |  Valores |
|          | mponente |          |          |          |     fo   |
|          | do       |          |          |          | rnecidos |
|          | Veículo  |          |          |          |          |
|          |          |          |          |          |    pelos |
|          |          |          |          |          |          |
|          |          |          |          |          | serviços |
|          |          |          |          |          |     dis  |
|          |          |          |          |          | poníveis |
|          |          |          |          |          |     no   |
|          |          |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Tipo |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Peça |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Co   |
|          |          |          |          |          | mponente |
|          |          |          |          |          |     de   |
|          |          |          |          |          |     Ve   |
|          |          |          |          |          | ículo\". |
+----------+----------+----------+----------+----------+----------+
| Dano do  | Class    | Numérico | Sim      | 6        | -        |
| Co       | ificação |          |          |          |  Valores |
| mponente | do dano  |          |          |          |     fo   |
|          | da peça  |          |          |          | rnecidos |
|          | do       |          |          |          |          |
|          | ve       |          |          |          |    pelos |
|          | ículo(NA |          |          |          |          |
|          | - Não    |          |          |          | serviços |
|          | A        |          |          |          |     dis  |
|          | valiado, |          |          |          | poníveis |
|          | Não,     |          |          |          |     no   |
|          | Sim)     |          |          |          |          |
|          |          |          |          |          |   Sinesp |
|          |          |          |          |          |          |
|          |          |          |          |          |    Dados |
|          |          |          |          |          |          |
|          |          |          |          |          |    Corpo |
|          |          |          |          |          | rativos: |
|          |          |          |          |          |          |
|          |          |          |          |          | \"Listar |
|          |          |          |          |          |     Dano |
|          |          |          |          |          |     Ve   |
|          |          |          |          |          | ículo\". |
+----------+----------+----------+----------+----------+----------+
| Justi    | Justi    | Texto    | Não      | 50       | -        |
| ficativa | ficativa |          |          |          |    Valor |
|          | para não |          |          |          |     di   |
|          | ter sido |          |          |          | sponível |
|          | a        |          |          |          |          |
|          | valiado. |          |          |          |   apenas |
|          |          |          |          |          |          |
|          |          |          |          |          |   quando |
|          |          |          |          |          |          |
|          |          |          |          |          |   \"Dano |
|          |          |          |          |          |     do   |
|          |          |          |          |          |     Comp |
|          |          |          |          |          | onente\" |
|          |          |          |          |          |     for  |
|          |          |          |          |          |          |
|          |          |          |          |          |   \"NA - |
|          |          |          |          |          |     Não  |
|          |          |          |          |          |     Ava  |
|          |          |          |          |          | liado\". |
+----------+----------+----------+----------+----------+----------+

**Regras Gerais de Processamento do B.O:**

-   Se o B.O envio for idêntico a todos os campos do último B.O, deve-se
    desprezar o B.O enviado e gerar no log de processamento a seguinte
    mensagem:

    -   [[936600: MSG - B.O não
        registrado.]{.underline}](https://alm.serpro/rm/resources/TX_ynNfoGulEfCXc_rffzpa6A)Procedimento
        não recepcionado devido a não identificação de diferença(s)
        entre o último procedimento enviado.

 \
**Resposta do Envio**\
**Como resposta ao envio do B.O, será retornado a seguinte mensagem:**\
[[934489: RN - Resposta ao Enviar B.O
Integração]{.underline}](https://alm.serpro/rm/resources/TX_ZDzwAF6CEfCirqqtwbaCiw)

| **Nome do Campo** | **Descrição** | **Tipo** | **Obrigatório** | **Regras**|
| :--- | :--- | :---: | :---: | :--- |
| Protocolo de Recebimento | Protocolo Único Recebimento Procedimento | Texto | Sim | Toda mensagem recepcionada pelo sistema gerará um número de protocolo de recebimento |
| Número do Procedimento | Número do Procedimento do B.O | Texto | Sim | Retorno do número que foi passado. <br> Caso não seja repassado número do procedimento, deverá ser retornado mensagem de |
| Data e Hora do Recebimento | Data e Hora do Recebimento do Procedimento | Data | Sim | Será retornado se passar pelas regras de recebimento. |
| Resposta de Recebimento | Resposta da solicitação de envio do B.O | Texto | Sim | - Valores:<br> - B.O em fila de processamento <br> - B.O Recusado |
| Motivo(s) da Recusa de Recebimento | Motivo da recusa no recebimento do B.O | Texto | Não | - Caso o B.O seja recusado já na fase de recebimento, enviar o(s) motivo(s) da recusa. <br> O registro de recusa também estará disponível na consulta de procedimentos. |

 \
**As situações do B.O são:**

-   **Recusado** (Não passou pela validação dos campos obrigatórios)

    -   Caso a estrutura enviada não esteja de acordo com o esperado.

    -   Caso a tipagem para o campo não esteja de acordo com o esperado.

    -   Caso os tamanhos mínimos ou máximos do campo não estejam no
        tamanho esperado.

    -   Caso os dados de domínio não estejam dentro do esperado.

-   **Protocolado** (Possui os campos obrigatórios preenchidos e os
    dados informados são válidos)​​​​​

-   **Processado** (Passou pela verificação das regras de negócio)

    -   B.O recepcionado.

Caso o B.O seja recusado, todos os motivos da recusa serão enviados na
mensagem de resposta do envio do B.O.\
Todo o processamento deverá gerar um registro(log), que poderá ser
consultado pela história:\
[[HU - Consultar Processamento do
Envio]{.underline}](https://alm.serpro/rm/resources/TX_gZ5Y4GirEfCirqqtwbaCiw)
ou [[HU - Consultar
Processamento WEB]{.underline}](https://alm.serpro/rm/resources/TX_QpkloGipEfCirqqtwbaCiw).\
 

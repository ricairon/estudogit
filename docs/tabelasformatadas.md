# HU - Enviar Boletim de Ocorrência - BO Único : Integração com Sistema de Procedimentos

## Visão Geral

# HU - Enviar Boletim de Ocorrência - BO Único : Integração com Sistema de Procedimentos

## Tabela 1 – Parâmetros de Personalização de Acesso – Envio Procedimento

| Campo | Descrição | Tipo |
| :--- | :--- | :---: |
| ID Unidade de Registro | Identificador da Unidade de Registro | Texto |
| ID Esfera | Esfera do Registro | Lista |
| ID UF de Registro | UF de Registro do Procedimento. | Lista |
| ID Município | Município de Registro | Lista |
| ID Instituição | Instituição de Registro | Lista |
| Tipos de Procedimento | BO, AIAI, IP, TCO, BOC... | Lista |

---

## Tabela 2 – Campos Mínimos para Recebimento do Boletim de Ocorrência

### Identificação do órgão que envia o B.O

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|:---|:---|:---:|:---:|:---:|------------------------|
| Unidade de Registro | Identificador da Unidade de Registro do B.O. | Texto | Sim | 100 | - A Unidade de Registro do procedimento deve pertencer a Instituição que está enviando.<br>- A Unidade de Registro deve estar cadastrada na SENASP.<br>- A Unidade de Registro deve estar ativa na data/hora do registro do procedimento. |
| Código Esfera | Identificador da Esfera da Unidade de Registro | Texto | Sim | 6 | - Quando a esfera da unidade de registro for igual a "Federal", o campo Código Instituição da Esfera deve corresponder a uma instituição "Federal".<br>- Quando a esfera da unidade de registro for igual a "Estadual", o campo Código Instituição da Esfera deve corresponder a uma instituição "Estadual".<br>- Quando a esfera da unidade de registro for igual a "Municipal", o campo Código Instituição da Esfera deve corresponder a uma instituição "Municipal".<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **geral-v3**: "Listar Esferas". |
| Código Instituição | Identificador da Instituição | Texto | Sim | 6 | - Quando a esfera da unidade de registro for igual a "Federal", a instituição informada deve pertencer a uma esfera federal.<br>- Quando a esfera da unidade de registro for igual a "Estadual", a instituição informada deve pertencer a uma esfera estadual.<br>- Quando a Esfera da Unidade de Registro for "Municipal", a Instituição informada deve pertencer a uma esfera municipal.<br>- [MR06 - Instituição Inválida](#)<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-procedimento-v3**: "Listar Instituições". |
| Código Município Registro | Identificador do Registro do Município | Texto | Sim | 11 | - O Código Município deve estar dentro dos municípios de autorização para envio do procedimento.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios". |
| Código UF de Registro | Identificador da UF | Texto | Sim | 2 | - A UF de registro deve estar dentro das autorizações concedidas para envio.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar UFs". |

### Campos do Procedimento

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Código do Tipo de Procedimento | Código do Tipo de Procedimento Enviado | Texto | Sim | 6 | - Código do Procedimento tem que ser válido com a estrutura enviada.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-procedimento-v3**: "Listar Tipos de Procedimento Policial". |
| Número de Registro Local do B.O | Número de Registro Local do Boletim de Ocorrência | Texto | Sim | 40 | |
| Data Hora Registro | Data e Hora do Registro do Boletim de Ocorrência | Texto (Data e Hora) | Sim | 20 | - Formato esperado: dd/MM/aaaa HH:mm:ss<br>- [949002: RN - A data e hora não pode ser posterior a data de envio ao sistema.](#)<br>- Será considerado o Horário Local do Município de Registro do B.O. |
| Relato Histórico | Relato histórico da ocorrência | Texto | Sim | - | |

### Dados da Ocorrência

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Data Início da Ocorrência | Data de Início da Ocorrência | Lista Circunstância do Fato - Data | Sim | 10 | - Formato esperado: dd/MM/aaaa<br>- Data Início da Ocorrência não pode ser posterior à data atual ou à data de registro do B.O. |
| Hora Início da Ocorrência | Hora de Início da Ocorrência | Lista Circunstância do Fato - Hora | Sim | 8 | - Formato esperado: HH:MM |
| Código País de Ocorrência do B.O | País da ocorrência | Lista Circunstância do Fato - Numérico | Sim | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Países". |
| Código UF de Ocorrência do B.O | Unidade Federativa de onde ocorreu. | Lista Circunstância do Fato - Texto | *(1) Sim | 2 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar UFs".<br>- *(1) Campo deixa de ser obrigatório se "Código País de Ocorrência do B.O" não for o Brasil. |
| Código Município Ocorrência | Código do município de ocorrência | Lista Circunstância do Fato - Texto | *(2) Sim | 11 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios".<br>- *(2) Campo deixa de ser obrigatório se "Código País de Ocorrência do B.O" não for o Brasil. |
| Logradouro da Ocorrência | Logradouro da ocorrência | Lista Circunstância do Fato - Texto | Sim | 200 | |
| Província da Ocorrência | Província da Ocorrência | Lista Circunstância do Fato - Texto | *(3) Não | 200 | - *(3) Se o país da ocorrência não for o Brasil, o campo Província Município da Ocorrência tornar-se obrigatório.<br>- Caso o país de ocorrência seja o Brasil, o campo Província da Ocorrência não deve ser preenchido. |
| Natureza da Ocorrência | Natureza da ocorrência | Lista Naturezas da Ocorrência | Sim | - | - É obrigatório informar pelo menos uma natureza da ocorrência.<br>- Não é permitido o cadastro de Naturezas da Ocorrência duplicadas. Serão desconsideradas as naturezas que possuírem o mesmo código e mesmo valor para o campo "tentativa". |
| Código Tipo de Local | Código Corporativo do tipo de local | Lista Circunstância do Fato - Numérico | Sim | 10 | - [MR3 - Campo Obrigatório Não Informado](#)<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Tipos de Local". |
| Código Subgrupo Local | Código do subgrupo do local de Dados Corporativos | Lista Circunstância do Fato - Texto | *(2) Sim | 10 | - [MR3 - Campo Obrigatório Não Informado](#)<br>- [MR6 - Subgrupo Local não condiz com Tipo de Local](#)<br>- *(2) Quando for informado o "Código Tipo Local": "AMBIENTE VIRTUAL (INTERNET)" não será obrigatório informar o subgrupo.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Subgrupos Local". |

### Envolvimentos

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Envolvimento Pessoa e Natureza | Lista com o envolvimento de Pessoa e Natureza | Vínculo Pessoa-Natureza | Sim | - | |

---

## Tabela 3 – Dados do Registro

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

---

## Tabela 4 – Data e Local da Ocorrência

### Tempo do Fato

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Data do Início da Ocorrência | Data do Início da Ocorrência | Texto | Sim | 10 | - [MR3 - Campo Obrigatório Não Informado](#)<br>- Formato esperado: dd/MM/aaaa<br>- Data do Início do Fato não pode ser posterior a data atual.<br>- Data do Início do Fato não pode ser posterior ao Fim do Fato.<br>- Para a validação da data futura, será utilizado o fuso horário do município de ocorrência. |
| Indicativo Data Início Aproximada | Indicativo se a data início é aproximada | Booleano | Não | 5 | - Caso não seja enviado ou enviado como null, considerar valor padrão NÃO. |
| Hora do Início da Ocorrência | Hora do Início da Ocorrência | Texto | Sim | 5 | - [MR3 - Campo Obrigatório Não Informado](#)<br>- Formato esperado: HH:mm |
| Indicativo Hora Início Aproximada | Informa se a hora é aproximada | Booleano | *(1) Não | 5 | - (1) Ao sinalizar a "Hora Início Aproximada" como SIM, o campo "Código Período Início Ocorrência" torna-se obrigatório.<br>- Caso não seja enviado ou enviado como null, considerar valor padrão NÃO. |
| Código Período Ocorrência | Código corporativo do Período da Ocorrência | Texto | *(2) Não | 12 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **qualificação-procedimento-v3**: "Listar Períodos do Dia".<br>- (2) Ao sinalizar a "Hora Início Aproximada" como Aproximada SIM, o campo "Código Período Início Ocorrência" torna-se obrigatório. |
| Data do Fim da Ocorrência | Data do Fim da Ocorrência | Texto | Não | 10 | - Formato esperado: dd/MM/aaaa |
| Indicativo Data Fim Aproximada | Indicativo se a data Fim é aproximada | Booleano | Não | 5 | - Caso "Data do Fim do Fato" não seja enviado, considerar valor de "Data Fim Aproximada" como null. |
| Hora Fim Ocorrência | Hora do fim da ocorrência | Texto | Não | 5 | - Formato esperado: HH:mm |
| Indicativo Hora Fim Aproximada | Indicativo se hora fim é aproximada | Booleano | *(3) Não | 5 | - (3) Ao sinalizar a "Indicativo Hora Fim Aproximada" como SIM, o campo "Código Período Fim Ocorrência" torna-se obrigatório. |
| Código Período Fim Ocorrência | Código corporativo do Período Fim da Ocorrência | Texto | *(4) Não | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **qualificação-procedimento-v3**: "Listar Períodos do Dia".<br>- (4) Ao sinalizar a "Hora Fim Aproximada" como SIM, o campo "Código Período Fim Ocorrência" torna-se obrigatório. |

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

---

## Tabela 5 – Natureza(s) da Ocorrência

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

---

## Tabela 6 – Vínculo Pessoa e Natureza

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Identificador da Pessoa | Identificador da Pessoa | Numérico | Não | 4 | |
| Identificador da Natureza | Identificador da Natureza. Preencher com o campo identificador de "Natureza da Ocorrência" - Identificador da Natureza | Numérico | Não | 4 | |
| Código Tipo de Envolvimento | Tipo de Envolvimento da Pessoa | Numérico | Não | 12 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Envolvimentos Natureza". |

---

## Tabela 7 – Vínculo Pessoa e Objeto

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Identificador do Objeto | Identificador do Objeto | Numérico | Não | 4 | - Identificação do Objeto tem que estar de acordo com a objetos declarados em Objetos<br>- Caso não seja identificado vínculo de Pessoa com Objeto: Recusar o B.O |
| Identificador da Pessoa | Identificador da Pessoa | Numérico | Não | 4 | - Identificação da Pessoa tem que estar de acordo com a pessoa declarada em Pessoas |
| Código Tipo de Vínculo Objeto Pessoa | Lista com Tipos de Vínculo da Pessoa com Objeto | Lista Numérica | Não | 12 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipos de Vínculo Objeto Pessoa". |

---

## Tabela 8 – Vínculo entre Pessoas

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Identificador Pessoa 1 | Identificador da Pessoa 1 | Texto | Sim | 20 | |
| Identificador da Pessoa 2 | Identificador da Pessoa 2 | Texto | Sim | 20 | |
| Código Tipo de Relacionamento Pessoa | Tipo de Relacionamento entre Pessoas | Código | Sim | 12 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Relacionamento Pessoa". |
| Ano(s) Tempo de Relação | Anos de Relação | Numérico | Não | 4 | |
| Mês(es) Tempo de Relação | Meses de Relação | Numérico | Não | 2 | |
| Dia(s) Tempo de Relação | Dias de Relação | Numérico | Não | 2 | |

---

## Tabela 9 – Envolvimento Pessoa em Acidente (BOAT)

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

---

## Tabela 10 – Dados Exclusivos Pessoa Jurídica

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Identificação da Pessoa Jurídica | Identificação única que irá identificar a Pessoa no envio | Numérico | 4 | Sim | |
| Empresa do Governo (Estado) | Indicativo se Empresa pertence ao Governo (Estado) | Booleano | 1 | Não | - Se o valor não for informado, considerar NÃO por padrão. |
| CNPJ | CNPJ | Texto | 14 | Não | |
| Razão Social | Nome Razão Social da Empresa | Texto | 200 | (1) Não | (1) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento. |
| Nome Fantasia | Nome Fantasia da Empresa | Texto | 200 | Não | |
| Nome Representante | Nome do Representante da Empresa | Texto | 100 | Não | |
| Código Ramo Atuação | Ramo de Atuação da Empresa | Texto | 4 | (1) Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **qualificação-pessoa-v3**: "Listar Ramo de Atuação".<br>(1) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento. |

---

## Tabela 11 – Endereço (Pessoa Jurídica e Física)

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

---

## Tabela 12 – Documento Pessoa (Jurídica e Física)

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

---

## Tabela 13 – Rede Social

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Rede Social | Código do Tipo de Rede Social | Texto | 6 | (1) Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - outros-v3: "Listar Redes Sociais".<br>- (1) Ou 'Código Rede Social' ou 'Outra Rede Social' devem ser informados. |
| Outra Rede Social | Caso não exista a rede social em código de rede social | Texto | 50 | (1) Não | - (1) Ou 'Código Rede Social' ou 'Outra Rede Social' devem ser informados.<br>- Caso 'Código Rede Social' seja informado, desprezar o valor de 'Outra Rede Social'. |
| Nome de Usuário na Plataforma | Nome do Usuário na Rede Social | Texto | 50 | Sim | |

---

## Tabela 14 – Telefone

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| DDI do Telefone | DDI (Discagem Direta Internacional) | Texto | 3 | Não | Formato esperado: somente números |
| Código do País | Código do País | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Países" |
| DDD do Telefone | DDD (Discagem Direta a Distância) | Texto | 3 | Não | Formato esperado: somente números |
| Número do Telefone | Número do Telefone da Pessoa | Texto | 20 | Sim | Formato esperado: somente números |
| Indicativo WhatsApp | Indicativo se o telefone informado é de um contato com WhatsApp | Booleano | 5 | Não | |
| Código Tipo de Telefone | Código Corporativo do Tipo de Telefone (Comercial, celular..) | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - qualificação-pessoa-v3: "Listar Tipo de Telefone". |
| Nome do Contato de Recado | Texto para descrever contato que atende pelo telefone | Texto | 50 | Não | |

---

## Tabela 15 – E-mail

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| E-mail | E-mail | Texto | 256 | Não | |

---

## Tabela 16 – Dados Exclusivos Pessoa Física

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Identificação da Pessoa Física | Identificação que irá identificar a Pessoa no envio | Texto | 10 | Sim | [MR07 - Elemento Duplicado](#) |
| Código Tipo Pessoa | Dado corporativo do tipo de pessoa | Texto | 6 | Sim | - Para Pessoa Física, obrigatório receber o valor como: FISICA |
| Desconhecida | Indicativo se a Pessoa é Desconhecida. | Booleano | 5 | Não | - Se for marcado como Desconhecida - 'SIM', os demais campos da pessoa física tornam-se Não Obrigatórios.<br>- Pessoa física desconhecida não pode ter os seguintes envolvimentos: COMUNICANTE, CONDUTOR, ADVOGADO, DEFENSOR PÚBLICO, PROMOTOR DE JUSTIÇA, TRADUTOR, INTÉRPRETE, CONSELHO, REPRESENTANTE LEGAL, PROCURADOR. |
| Estado Aparente da Pessoa | Estado Aparente em que a pessoa se encontrava | Numérico | 3 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Estado Aparente Pessoa". |
| Indicativo Tornozeleira Eletrônica | Indicativo se a Pessoa utiliza Tornozeleira Eletrônica | Booleano | 5 | Não | |
| Identificação da Tornozeleira Eletrônica | Identificação da tornozeleira eletrônica | Texto | 25 | Não | |
| Indicativo Integrante Facção Criminosa | Indicativo se a Pessoa se identifica com alguma facção criminosa | Booleano | 5 | Não | |
| Identificação da Facção Criminosa | Identificação da Facção Criminosa | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **outros-v3** : "Listar Facções Criminosas". |
| Nome da Facção Criminosa | Nome da Facção Criminosa, caso não tenha em Dados Corporativos. | Texto | 50 | *(3) Não | - Caso o campo "Identificação da Facção criminosa" seja preenchido e for válido, este campo se for enviado deve ser ignorado.<br>- *(3) Se o campo "Integrante de facção criminosa" for enviado como SIM, o campo "Identificação da Facção Criminosa" torna-se obrigatório. |
| Indicativo Turista | Indicativo se a Pessoa é Turista | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Situação de Rua | Indicativo se a Pessoa vive em situação de rua | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Fato Ocorrido em Serviço | Indicativo se a Pessoa no ato estava em horário de serviço | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicativo Violência Doméstica | Indicativo se houve violência doméstica | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| CPF | CPF da Pessoa | Texto | 11 | Não | [MR08 - CPF Inválido](#)<br>- Se os campos 'CPF', 'Nome Completo' e 'Profissão' forem iguais: [MR07 - Elemento Duplicado](#)<br>- Formato esperado: Somente dígitos numéricos. |
| Nome Completo | Nome completo da pessoa | Texto | 100 | (1)(2) *Sim | *(1) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento.<br>*(2) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento. |
| Nome Social | Nome social da pessoa | Texto | 100 | Não | |
| Código Opção Nome Social | Código da Opção nome social | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificacao-pessoa-v3**: "Listar Opções de Nome Social". |
| Alcunha | Apelido da pessoa | Texto | 100 | Não | |
| Data de Nascimento | Data de Nascimento da pessoa | Data | 10 | (1) Não | - Formato esperado: dd/MM/aaaa<br>- *(1) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento. |
| Idade Informada | Idade Informada quando não preenchido data de nascimento | Inteiro | 3 | Não | - Idade não pode ser negativa<br>- Idade não pode ser superior a 150 anos. |
| Indicativo Idade Aproximada | Indicativo de Idade Aproximada | Booleano | 1 | Não | - Se o campo não for enviado, considerar o valor padrão como NÃO. |
| Nome Mãe | Nome da Mãe | Texto | 100 | (2) Não | *(2) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento. |
| Nome Pai | Nome do Pai | Texto | 100 | Não | |
| Código Sexo | Código corporativo do sexo da Pessoa | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificacao-pessoa-v3**: "Listar Tipo Sexo".<br>*(1) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento. |
| Código Orientação Sexual | Código da Orientação Sexual | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Orientações Sexuais". |
| Código Autodeclaração de Gênero | Código corporativo de Autodeclaração de orientação de gênero | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Auto Declaração de Gênero". |
| Código Identidade de Gênero | Identidade de gênero | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Identidade de Gênero". |
| Código Raça/Cor | Raça/Cor da pessoa | Texto | 6 | (1) Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Raças e Cor".<br>*(1) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento. |
| Código Estado Civil | Estado civil da pessoa | Texto | 6 | (1) Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Estado Civil".<br>*(1) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento. |
| Código Nacionalidade | Nacionalidade da pessoa | Texto | 6 | (2) Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Países".<br>*(2) Vide tabela acima de campo obrigatório para pessoa de acordo com tipo envolvimento. |
| Código Naturalidade - UF | Estado de naturalidade da pessoa | Texto | 2 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar UFs". |
| Código Naturalidade - Município | Código do Município | Texto | 11 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios".<br>- Código Naturalidade UF deve condizer com a UF do Município. |
| Nome do Município/Província | Nome do município/província | Texto | 200 | Não | |
| Código Escolaridade | Código da Escolaridade da Pessoa | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Escolaridade". |
| Código Tipo Físico | Código do Tipo de Físico da Pessoa | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Tipo Físico". |
| Altura | Altura da Pessoa em centímetros | Numérico | 3 | Não | |

---

## Tabela 17 – Dados Profissionais (Pessoa Física)

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Tipo de Trabalho | Tipo de Trabalho | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Tipo de Trabalho". |
| Código Profissão | Código corporativo da Profissão da Pessoa | Texto | 6 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Profissões". |
| Renda Mensal | Renda mensal por profissão em reais | Numérico decimal | (11,2) | Não | |

---

## Tabela 18 – Parte do Corpo

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Parte do Corpo | Código da Parte do Corpo | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Parte do Corpo". |
| Código Identificação Visual | Código da Identificação Visual | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Tipo de Identificação Visual". |
| Código de Cor da Identificação Visual | Cor da Identificação Visual | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Cor Identificação Visual". |
| Descrição da Parte do Corpo | Descrição da Parte do Corpo | Texto | 200 | Não | |

---

## Tabela 19 – Deficiência Física

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Tipo de Deficiência | Tipo da Deficiência da Pessoa | Texto | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **qualificação-pessoa-v3**: "Listar Tipo de Deficiência". |
| Nome da Deficiência | Nome da Deficiência Física | Texto | 100 | Não | |

---

## Tabela 20 – Tipo Objeto

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Identificação do Objeto | Número que identifica o objeto | Numérico Inteiro | 4 | Sim | - Se existir Identificador de Objeto Duplicado, deve-se: Recusar o B.O |
| Código Subgrupo Objeto | Código corporativo do subgrupo objeto | Texto | 10 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **objeto-v3**: "Listar Subgrupos de Objeto". |
| Código Grupo Unidade de Medida | Código Corporativo do Grupo de Unidade de medida do objeto | Texto | 6 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **objeto-v3**: "Listar Grupos Unidades de Medida". |
| Código Unidade de Medida | Código corporativo da Unidade de Medida | Texto | 6 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **objeto-v3**: "Listar Unidades de Medida". |
| Quantidade do Objeto | Quantidade do objeto | Numérico decimal | (12,3) | Sim | |
| Lista Qualificação Objeto | Lista de Qualificação do Objeto | Lista de Qualificação | - | Não | |

---

## Tabela 21 – Qualificação do Objeto

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

---

## Tabela 22 – Atributos Gerais do Objeto

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Descrição do Objeto | Descrição da identificação do objeto | Texto | 100 | Não | |
| Bem Objeto | Descrição do objeto | Texto | 100 | Não | |
| Número de Série | Número de série do objeto | Texto | 100 | Não | |
| Marca do Objeto | Marca do objeto | Texto | 100 | Não | |
| Modelo do Objeto | Modelo do objeto | Texto | 100 | Não | |
| Cor do Objeto | Cor do objeto | Texto | 100 | Não | |
| Descrição do Acabamento do Objeto | Descrição do acabamento do objeto | Texto | 100 | Não | |
| Estado de Uso | Estado de uso do objeto | Texto | 50 | Não | |
| Código Destinação Uso | Código da Destinação do Objeto. | Numérico | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Destinações de Uso". |
| Valor Estimado do Objeto | Valor estimado do objeto | Numérico Decimal | (11,2) | Não | |
| Código Tipo de Fabricação | Código do Tipo Fabricação do Objeto. | Numérico | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipos de Fabricação". |
| Código do País | Código do País de fabricação do objeto. | Numérico | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Países". |
| Código da UF | Código da Unidade Federativa. | Texto | 2 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar UFs". |
| Código do Município | Código do Município de fabricação do Objeto. | Numérico | 11 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Municípios". |
| Nome Província do Objeto | Nome da província de fabricação do Objeto, caso o país não seja o Brasil | Texto | 200 | * Não | * Nome Província é obrigatório quando o país não é o Brasil |
| CNPJ Nota Fiscal | CNPJ da nota fiscal de compra do objeto. | Texto | 14 | Não | - CNPJ deve ser válido |
| CPF Nota Fiscal | CPF da nota fiscal de compra do objeto. | Texto | 11 | Não | - CPF deve ser válido |
| Nome Proprietário Nota Fiscal | Nome do proprietário descrito na nota fiscal de compra do objeto. | Texto | 100 | Não | |
| Número Documento Propriedade | Número do documento da propriedade. | Texto | 20 | Não | |
| Indicativo de Adulteração | Indicador se há adulteração no objeto. | Booleano | 1 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Descrição Adulteração | Descrição do tipo de adulteração do objeto | Texto | 200 | Não | |
| Indicativo de Adulteração de Marca | Indicador se a marca foi adulterada. | Booleano | 1 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicador de Adulteração de Modelo | Indicador se o modelo foi adulterado | Booleano | 1 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Indicador de Adulteração de Cor | Indicador se a cor foi adulterada | Booleano | 1 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Quantidade do Objeto | Quantidade do objeto | Numérico Decimal | (11,3) | Sim | |
| Código Tipo Localização do Objeto | Código do Tipo da Localização onde o objeto se encontra. | Numérico | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Localizações Objeto". |
| Descrição da Localização | Descrição da localização onde o objeto se encontra. | Texto | 100 | Não | |
| Código Unidade Medida | Código do Unidade Medida | Numérico | 6 | Não | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Unidades de Medida". |
| Código Localização do Objeto | Localização do objeto de acordo com dado corporativo | Numérico | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Localizações Objeto". |

---

## Tabela 23 – Objeto Arma

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

---

## Tabela 24 – Objeto Documento

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

---

## Tabela 25 – Objeto Munição

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Situação de Disparo do Projétil | Código situação de disparo do projétil | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Situação de Disparo Munição". |
| Indicativo de Adulteração | Indicador se há adulteração no objeto. | Booleano | 5 | Não | - Se o valor não for informado, considerar o valor como NÃO. |
| Descrição Adulteração | Descrição do tipo de adulteração do objeto | Texto | 200 | Não | |
| Código Tipo de Uso da Arma | Código do tipo de uso da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Tipo Uso Arma Munição". |
| Código Calibre da Arma | Código Corporativo com o calibre da munição da arma | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **arma-v3**: "Listar Calibre Arma". |

---

## Tabela 26 – Objeto Veículo

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

---

## Tabela 27 – Objeto Droga

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Descrição Aparência | Descrição da Aparência da droga | Texto | 30 | Não | |
| Descrição Embalagem | Descrição da Embalagem da droga | Texto | 30 | Não | |
| Data Validade da Droga | Data de Validade da Droga | Data | 10 | Não | - Formato esperado: dd/MM/aaaa |
| Tipo de Embalagem da Droga | Código do Tipo Embalagem da Droga. | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **objeto-v3**: "Listar Tipos de Embalagem". |
| Lote da Droga | Descrição do Lote da Droga | Texto | 50 | Não | |

---

## Tabela 28 – Objeto Celular

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Número Celular | Número da linha do celular | Texto | 20 | Não | Formato esperado: somente números |
| Nome Operadora | Nome da Operadora da linha | Texto | 100 | Não | |
| IMEI | Número IMEI principal do telefone | Texto | 20 | Não | Formato esperado: somente números |
| IMEI 2 | Número IMEI 2 | Texto | 20 | Não | Formato esperado: somente números |
| IMEI 3 | Número IMEI 3 | Texto | 20 | Não | Formato esperado: somente números |
| IMEI 4 | Número IMEI 4 | Texto | 20 | Não | Formato esperado: somente números |
| macAdress | Número MAC do celular | Texto | 17 | Não | Formato esperado: XX:XX:XX:XX:XX:XX |

---

## Tabela 29 – Objeto Moeda

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Qualificação da Moeda | Código corporativo para qualificação da moeda (circulante, falsificada, inexistente, não circulante). | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **objeto-v3**: "Listar Tipos de Qualificação de Moeda". |
| Descrição da Qualificação | Descrição da qualificação | Texto | 21 | Não | |
| Valor da Face | Valor da face da moeda | Numérico decimal | (11,2) | Não | |
| Valor Total | Valor total da moeda | Numérico decimal | (11,2) | Não | |
| Quantidade | Quantidade de moeda | Numérico | 9 | Não | - Formato esperado: somente números |
| Número de Série Inicial | Número de Série Inicial | Texto | 50 | Não | |
| Número de Série Final | Número de Série Final | Texto | 50 | Não | |

---

## Tabela 30 – Objeto Embarcação

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Porto de Registro | Nome do porto registro | Texto | 100 | Não | |
| Número de Registro | Número de registro da embarcação | Texto | 10 | Não | |
| Classificação | Classificação da embarcação | Texto | 10 | Não | |
| Código do Município | Código do Município de fabricação do Objeto. | Texto | 11 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar Municípios". |
| Código da UF | Código da Unidade Federativa. | Texto | 2 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **localidade-v3**: "Listar UFs". |
| Nome Província do Objeto | Nome da província de fabricação do Objeto, caso o país não seja o Brasil | Texto | 200 | *(1) Não | *(1) Nome Província é obrigatório quando o país não é o Brasil |

---

## Tabela 31 – Informar Órgão de Apoio

| Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|-------|-----------|------|---------|-------------|------------------------|
| Tipo de Jurisdição | Campo texto para descrição dos órgão, tipo e jurisdição que fizeram parte do atendimento. | Texto | 250 | Não | |
| Nome do Responsável | Campo texto para inserção dos nomes dos responsáveis pelo atendimento. | Texto | 250 | Não | |
| Total de pessoas | Total de Pessoas do órgão de apoio. | Numérico | 4 | Não | |
| Total de veículos | Total de Veículos do órgão de apoio. | Numérico | 4 | Não | |
| Ação do órgão | Campo texto para descrição das ações dos órgãos de apoio que auxiliaram no atendimento. | Texto | 500 | Não | |

---

## Tabela 32 – Incêndio/Explosão

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Tipo de Incêndio/Explosão | Tipo de Incêndio ou explosão | Texto | 5 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **incêndios-v3**: "Listar Tipos de Incêndio". |
| Subtipo de Incêndio ou Explosão | Subtipo de Incêndio ou explosão | Texto | 5 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **incêndios-v3**: "Listar Subtipo de Incêndio/Explosão". |
| Data Início Incêndio | Data do início do incêndio | Texto-Data | 10 | Sim | - Formato esperado: dd/MM/aaaa |
| Hora Início Incêndio | Hora do início do incêndio | Texto-Hora | 7 | Não | - Formato esperado: HH:MM:ss |
| Data Extinção do Incêndio | Data da Extinção do Incêndio | Texto-Data | 10 | Sim | - Formato esperado: dd/MM/aaaa |
| Hora Extinção do Incêndio | Hora da Extinção do Incêndio | Texto-Hora | 7 | Não | - Formato esperado: HH:MM:ss |
| Data do Rescaldo do Incêndio | Data do rescaldo do incêndio | Texto-Data | 10 | Sim | - Formato esperado: dd/MM/aaaa |
| Hora do Rescaldo do Incêndio | Hora do rescaldo do incêndio | Texto-Hora | 7 | Não | - Formato esperado: HH:MM:ss |
| Quantidade de Água | Quantidade de água utilizada em litros | Numérico | 6 | Não | |
| Quantidade de LGE (Líquido Gerador de Espuma)/EFE | Quantidade de espuma em litros | Numérico | 6 | Não | |
| Causa Provável de Ignição | Causa provável de ignição de incêndio. | Texto | 5 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **incêndios-v3**: "Listar Causas de Ignição de Incêndio". |
| Tipo Local de Origem do Incêndio | Local presumido de origem do incêndio | Texto | 5 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **incêndios-v3**: "Listar Tipos de Locais Origem Incêndio". |
| Subtipo Local de Origem do Incêndio | Indicação do Subtipo do local de origem do incêndio | Texto | 5 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **incêndios-v3**: "Listar Subtipos de Locais Origem Incêndio". |
| Tipo Fonte Provável de Ignição | Tipo de fonte provável de ignição de incêndio. | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **incêndios-v3**: "Listar Tipos de Fontes de Ignição de Incêndio". |
| Subtipo de Fonte de Ignição de Incêndio | Subtipo de fonte provável de ignição de incêndio | Texto | 5 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **incêndios-v3**: "Listar Subtipos de Fontes de Ignição de Incêndio". |

---

## Tabela 33 – Incêndio/Explosão - Edificação

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Tipo de Edificação | Código corporativo do Tipo de Edificação | Texto | 6 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos - **edificação-v3**: "Listar Tipos da Edificação". |
| Outro tipo de Edificação | Campo para descrição de outro tipo de edificação quando o "Tipo de Edificação" for "OUTRO". | Texto | 50 | (1) Não | - (1) Campo torna-se obrigatório e não pode ficar em branco quando "Tipo de Edificação" for correspondente a 'Outro'. |
| Código Tipo de Uso Ocupação | Código Corporativo Tipo de Ocupação e uso do Imóvel | Texto | 5 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **edificação-v3**: "Listar Tipos de Uso/Ocupação da Edificação". |
| Código Subtipo Uso Ocupação | Subtipo de Uso Ocupação da Edificação | Texto | 5 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **edificação-v3**: "Listar Subtipos de Uso/Ocupação da Edificação". |
| Código Situação de Ocupação/Uso | A situação de ocupação da edificação | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **edificação-v3**: "Listar Situações de Ocupação da Edificação". |
| Código Tipo de Estrutura da Edificação | Tipo de estrutura predominante da edificação | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **edificação-v3**: "Listar Tipos de Estrutura da Edificação". |
| Outros Tipo de Estrutura | Campo para descrição do tipo de estrutura quando o valor de Tipo de Estrutura Premoninante for "OUTRAS". | Texto | 50 | (2) Não | - (2) Campo torna-se obrigatório e não pode ficar em branco quando "Tipos de Estrutura da Edificação" for correspondente a 'Outras'. |
| Código Classe Predominante | Código Corporativo da Classe predominante da edificação | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **edificação-v3**: "Listar Classe Predominante da Edificação". |
| Código Característica Construtiva | Qual característica construtiva da edificação | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **edificação-v3**: "Listar Características Construtivas da Edificação". |
| Outra Característica Construtiva | Campo para descrição de outra característica construtiva quando a "Característica Construtiva" for "OUTRO" | Texto | 50 | (3) Não | - (3) Campo torna-se obrigatório e não pode ficar em branco quando "Código Característica Construtiva" for correspondente a 'Outros'. |
| Total de Pavimentos da Edificação | Total de Pavimentos da Edificação | Numérico | 3 | Não | |
| Número de Pavimentos Atingidos | Quantidade de pavimentos atingidos da edificação | Numérico | 3 | Não | - Número de Pavimentos Atingidos não pode ser menor do que 'Total de Pavimentos da Edificação'. |
| Descrição Pavimentos Atingidos | Descrição de quais dos pavimentos foram atingidos | Texto | 250 | Não | |
| Área Total de Edificação | Área total de edificação (m²) | Numérico decimal | (8,2) | Não | |
| Área Atingida | Área Atingida (m²) | Numérico decimal | (8,2) | Não | |
| Indicativo Houve explosão | Indicativo se houve explosão | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **outros-v3**: "Listar Opções de Resposta".<br>- Caso não seja enviado, considerar o valor padrão correspondente a "Não Informado". |
| Indicativo Houve Danos Aparentes à Estrutura | Indicativo se houve danos aparentes na estrutura | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **outros-v3**: "Listar Opções de Resposta".<br>- Caso não seja enviado, considerar o valor padrão correspondente a "Não Informado". |
| Indicativo Houve desabamento | Indicativo se houve desabamento | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **outros-v3**: "Listar Opções de Resposta".<br>- Caso não seja enviado, considerar o valor padrão correspondente a "Não Informado". |
| Indicativo Houve Necessidade de Entrada Forçada | Indicativo se houve entrada forçada | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **outros-v3**: "Listar Opções de Resposta".<br>- Caso não seja enviado, considerar o valor padrão correspondente a "Não Informado". |
| Código Necessidade para Entrada Forçada | Lista de Código de Necessidades de Entrada Forçada | Lista Código Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **edificação-v3**: "Listar Necessidade de Entrada Forçada da Edificação". |
| Descrição Outra Necessidade para Entrada Forçada | Descrição de Outra necessidade de entrada Forçada | Texto | 50 | Não | |
| Indicativo Possui Alvará do Corpo de Bombeiros | Indicativo se a edificação possui alvará do corpo de bombeiros | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **outros-v3**: "Listar Opções de Resposta".<br>- Caso não seja enviado, considerar o valor padrão correspondente a "Não Informado". |
| Sistemas Preventivos Utilizados | Lista com os sistemas preventivos utilizados | Lista Código Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **edificação-v3**: "Listar Sistemas Preventivos da Edificação".<br>- Valores duplicados devem ser desprezados. |
| Descrição Outros Sistemas Preventivos Utilizados | Campo para descrição de outros sistemas preventivos utilizados | Texto | 50 | Não | |
| Descrição Bens Móveis e Imóveis Atingidos | Campo para descrição de bens | Texto | 400 | Não | |
| Ações Realizadas | Lista para indicar as ações realizadas | Lista Código Texto | 5 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **incêndios-v3**: "Listar de Ações Realizadas CBM". |

---

## Tabela 34 – Incêndio/Explosão - Vegetação

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Código Tipo de Unidade de Conservação/Preservação | Tipo de unidade de preservação (conservação, preservação) | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **vegetação-v3**: "Listar Tipos de Conservação/Vegetação". |
| Total Aproximado de Área Queimada (hectare) | Total aproximado da área da vegetação queimada (hectare) | Numérico decimal | (8,2) | Sim | |
| Indicativo Houve Apoio | Indicativo se houve apoio de Brigada/Vigilante/Bombeiro Civil | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **outros-v3**: "Listar Opções de Resposta".<br>- Caso não seja enviado, considerar o valor padrão correspondente a "Não Informado". |
| Quantidade de Pessoas de Apoio | Quantidade de Pessoas de apoio | Numérico | 6 | Não | - Somente receber se "Indicativo Houve Apoio" for enviado como SIM. |
| Códigos Tipos de Incêndio | Lista de Tipos de Incêndio | Lista Texto | 6 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **vegetação-v3**: "Listar Tipos de Incêndio de Vegetação". |
| Códigos Tipos de Vegetação | Lista de Tipos de Vegetação | Lista Texto | 6 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **vegetação-v3**: "Listar Tipos de Vegetação". |
| Outro Tipo de Vegetação | Descrição de outro tipo de vegetação | Texto | 50 | Não | - Campo só pode ser enviado se 'Tipos de Vegetação' for correspondente a 'Outros'. |
| Descrição Tipo de Solo | Descrição do tipo de solo | Texto | 200 | Não | |
| Código Tipo de Relevo | Código Corporativo do Tipo de Relevo | Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **vegetação-v3**: "Listar Tipos de Relevo". |
| Temperatura | Temperatura | Texto | 50 | Não | |
| Umidade Relativa do Ar | Umidade do ar | Texto | 50 | Não | |
| Precipitação | Precipitação | Texto | 50 | Não | |
| Velocidade do vento | Velocidade do vento | Texto | 50 | Não | |
| Códigos Recursos Hídricos Disponíveis | Listagem com recursos hídricos disponíveis | Lista Texto | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **vegetação-v3**: "Listar Tipos de Recurso Hídrico Disponível". |
| Outros Recursos Hídricos | Descrição de outros recursos hídricos | Texto | 50 | Não | |
| Ações Realizadas | Lista de Ações Realizadas | Lista Texto | 6 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos **incêndios-v3**: "Listar de Ações Realizadas CBM". |

---

## Tabela 35 – Incêndio/Explosão - Meio de Transporte

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Tipo de Meio de Transporte | Subgrupo do meio de transporte | Numérico | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Subgrupo Objeto".<br>- Os valores recebidos de Subgrupo Objeto tem que pertencer a categoria "Meio de Transporte". |
| Tipo de Combustível | Tipo de combustível do veículo | Numérico | 6 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Combustíveis de Veículo". |
| Danos Materiais Aparentes | Descrição dos danos materiais aparentes | Texto | 400 | Não | |
| Houve Explosão | Indicativo se houve explosão do meio de transporte | Booleano | 1 | Não | - Caso não seja enviado, considerar por padrão o valor NULL (Sim, Não, Não Apurado) |
| Possui Carga | Indicativo se o veículo possuia carga | Booleano | 1 | Sim | - Caso não seja enviado, considerar por padrão o valor NÃO |
| Tipo Carga Transportada | Tipo de carga transportada | Texto | 3 | *(1) Não | - *(1) Campo torna-se obrigatorio se possui carga for enviado com SIM.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Carga Transportada". |
| Outro Tipo de Carga Transportada | Descrição de outro tipo de carga transportada | Texto | 50 | *(2) Não | - *(2) Torna-se obrigatorio quando o "Tipo Carga Transportada" for "OUTRO". |
| Indicativo Carga Atingida | Indicativo se a carga foi atingida | Booleano | 1 | Não | |
| Indicativo Carga com Seguro | Indicativo se a carga tinha seguro | Booleano | 1 | Não | |
| Ações Realizadas | Lista com ações realizadas | Lista Código Texto | 3 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Ações Realizadas". |

---

## Tabela 36 – Incêndio/Explosão - Outros (PNEU, LIXO, OUTRO)

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Ações Realizadas | Lista para indicar as ações realizadas | Lista Numérica | 6 | Sim | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar de Ações Realizadas". |

---

## Tabela 37 – Busca e Salvamento

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Tipo de Busca e Salvamento | Tipo de busca e salvamento | Lista Código Texto | Sim | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Busca e Salvamento". |
| Subtipo de Busca e Salvamento | Subtipo de busca e salvamento | Lista Código Texto | Sim | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar SubTipo de Busca e Salvamento". |
| Tipo Envolvimento Busca e Salvamento | Lista com os objetos envolvidos | Lista Código Texto | Sim | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Envolvimento Busca e Salvamento". |
| Ação(ões) Realizada(s) | Lista com as ações realizadas pelo órgão | Lista Código Texto | Sim | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Ações Realizadas". |
| Outras Ações | Descrição de outras ações não listadas em Ações Realizadas. | Texto | Não | 50 | - Quando a opção de "Ação Realizada" for "OUTRO". |
| Tipo de Animal | Lista de animais | Lista Código Texto | Não | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Animal". |

---

## Tabela 38 – Atendimento Pré-Hospitalar

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Vítima | Indicação da Pessoa que foi vítima | Numérico | Sim | 4 | - Vínculo com Pessoas |
| Indicativo Lesão | Indicativo se não teve lesão aparente ou foi politraumatizado | Booleano | Não | 1 | |
| Tipo de Lesão | Lista de lesões | Lista Código Texto | Não | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Lesão". |
| Atendimento decorrente | Atendimento decorrente de | Lista Código Texto | Não | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Atendimento Decorrente". |
| Subtipo Atendimento Decorrente | Subtipo de Atendimento Decorrente | Lista Código Texto | Não | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Subtipo de Atendimento Decorrente". |
| Faixa etária da vítima | Faixa etária da vítima | Texto | Não | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Faixa Etária da Vítima". |
| Parte do corpo queimada | Lista de parte(s) do corpo | Lista Numérica | Não | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Partes do Corpo". |
| Produto causador Queimadura | Produto causador da queimadura | Lista Numérica | Não | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Produto Causador de Queimadura". |
| Outro produto | Descrição do outro produto causador | Texto | Não | 50 | - Quando o "Produto Causador" for 'OUTRO' |
| Várias áreas atingidas | Indicativo de várias áreas atingidas pela queimadura | Booleano | Não | 1 | |
| Superfície atingida | Porcentagem da superfície atingida | Numérico | Não | 3 | - Valor deve estar com mínimo 0 e máximo 100 |
| Abertura Ocular | valor correspondente a abertura ocular | Numérico | Não | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Abertura Ocular". |
| Melhor Resposta Verbal | valor correspondente a resposta verbal | Numérico | Não | 4 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Resposta Verbal". |
| Melhor Resposta Motora | valor correspondente a resposta motora | Numérico | Não | 4 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Resposta Motora". |
| Pulso (bpm) | medição do pulso | Numérico | Não | 3 | |
| SaO2 (Saturação Oxigênio) | medição da saturação | Numérico decimal | Não | (3,2) | |
| Faixa de Respiração/Min | Índice de respitação por minuto | Texto | Não | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Faixa de Respiração/Min". |
| P.A Máxima (mmHg) | Pressão arterial máxima medida (mmHg) | Texto | Não | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Faixa de P.A Máxima". |
| Vítima Recusou Atendimento | Indicativo se recusou atendimento | Booleano | Sim | 1 | |
| Houve Suporte Avançado | Indicativo de suporte avançado | Booleano | Sim | 1 | |
| Nome Suporte Avançado que Interceptou (USA) | Nome do suporte avançado que interceptou o atendimento | Texto | Sim | 100 | |
| Nome da(s) Ação(ções) | Lista de ações realizadas | Lista Numérica | Não | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Ações Realizadas". |
| Vítima Repassada Para | Nome do órgão que foi entregue a vítima | Numérica | Não | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Orgão de Repasse da Vítima". |
| Nome do Órgão de Entrega | nome do órgão | Texto | *Não | 150 | - Campo obrigatório quando o valor de "Vítima Repassada Para" for "Entregue a outro órgão". |
| Data de Entrega no Órgão | data de entrega ao órgão | Data | Não | 10 | - Formato esperado: dd/MM/aaaa |
| Hora de Entrega no Órgão | hora de entrega ao órgão | Hora | Não | 10 | - Formato esperado: HH:MM |
| Houve Óbito no Local | Indicativo se houve óbito no local | Booleano | Não | 1 | - Caso não seja enviado, considerar por padrão o valor NÃO |
| Tipo Constatação do Óbito | Tipo de constatação de óbito | Texto | *Sim | 6 | - Campo obrigatório quando há óbito no local.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Constatação de Óbito". |
| Data do Óbito | Data do óbito | Data | *Sim | 10 | - Campo obrigatório quando há óbito no local.<br>- Formato esperado: dd/MM/aaaa |
| Hora do Óbito | Hora do óbito | Hora | *Sim | 10 | - Campo obrigatório quando há óbito no local.<br>- Formato esperado: HH:mm |
| Nome Médico que atestou o óbito | Nome do médico que atestou o óbito | Texto | *Sim | 100 | - Campo obrigatório quando o "Tipo do Óbito" for "Por médico" |
| CRM do Médico que atestou o óbito | CRM do médico que atestou o óbito | Texto | *Sim | 30 | - Campo obrigatório quando o "Tipo do Óbito" for "Por médico" |
| Nome | nome da unidade de saúde | Texto | Não | 200 | |
| Ficha hospitalar | ficha hospitalar | Texto | Não | 200 | |
| Nome do médico | Nome do médico | Texto | Não | 200 | |

---

## Tabela 39 – Produto Perigoso

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Produto Perigoso | Nome do produto/Nº da ONU/Número de risco | Lista Código Texto | Sim | 10 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Produto Perigoso". |
| Tipo de Produto Perigoso | Tipo de produto perigoso (biológico, radioativo...) | Texto | Sim | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Produto Perigoso". |
| Tipo de Recipiente | Tipo de recipiente | Texto | Sim | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Recipiente". |
| Subtipo de Recipiente | Subtipo de Recipiente | Texto | Não | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Subtipo de Recipiente". |
| Volume Estimado de Vazamento | Volume estimado de vazamento | Numérico | Não | 6 | |
| Unidade de Medida do Volume Estimado | Unidade de medida do vazamento | Numérico | Não | 4 | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Unidade de Medida". |
| Volume Estimado do Recipiente | Volume estimado do recipiente | Numérico | Não | 8 | |
| Unidade de medida do Recipiente | Unidade de medida do recipiente | Numérico | Não | 4 | Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Unidade de Medida". |
| Volume Estimado do Produto | Volume estimado do produto | Numérico | Não | 8 | |
| Unidade de Medida do Volume Estimado | unidade de medida do produto | Numérico | Não | 4 | |
| Estado Físico do Produto | Estado físico do produto | Texto | Não | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Estado Físico do Produto". |
| Destinação do Produto | Destinação do Produto | Lista Código Texto | Não | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Destinação do Produto". |
| Outra Destinação do Produto | Descrição da destinação do produto | Lista Numérica | Não | 50 | |
| Área Isolada | Tamanho da área isolada | Numérico | Sim | 6 | |
| Unidade da Área Isolada | Unidade de medida da área isolada (m2, km2, quadras..) | Numérico | Sim | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Unidade de Medida". |
| Área Contaminada | Tamanho da área contaminada | Numérico | Sim | 6 | |
| Unidade da área contaminada | Unidade de medida da área contaminada | Numérico | Sim | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Unidade de Medida". |
| Área de Abandono | Tamanho da área de abandono | Numérico | Sim | 6 | |
| Unidade da Área Abandono | Unidade de medidade da área de abandono | Numérico | *Sim | 6 | - Campo obrigatório se informado área for abandono<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos |
| Ambiente Afetado | Descrição do ambiente afetado | Texto | Não | 400 | |
| Causa Vazamento | Lista de causas prováveis do vazamento | Lista Código Texto | Sim | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Causa de Vazamento" |
| Nome da Ação | Lista de ações realizadas | Lista Código Texto | Não | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Ações Realizadas" |

---

## Tabela 40 – Atendimento Comunitário

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Estimativa de Público Atingida | Estimativa de público no atendimento | Numérico | Não | 8 | |
| Duração do Evento | Tempo de duração do evento | Numérico | Não | 8 | |
| Unidade da Duração do Evento | Unidade de Tempo (horas, dias) | Numérico | Não | 8 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Unidade de Medida". |
| Ação Realizada | Ação(ões) realizada(s) | Lista Numérica | Sim | 8 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Ações Realizadas". |

---

## Tabela 41 – Desastre

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Áreas Afetadas | descrição das áreas afetadas pelo desastre | Texto | 500 | Não | |
| Causas | descrição das causas do desastre | Texto | 500 | Não | |
| Porte do Evento | porte do evento (grande, pequeno, médio..) | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Porte do Evento". |
| Pessoas Desabrigadas | Quantidade de Pessoas Desabrigadas | Numérico | 6 | Não | |
| Pessoas Desalojadas | Quantidade de Pessoas Desalojadas | Numérico | 6 | Não | |
| Pessoas Desaparecidas | Quantidade de Pessoas Desaparecidas | Numérico | 6 | Não | |
| Pessoas Mortas | Quantidade de Pessoas Mortas | Numérico | 6 | Não | |
| Pessoas Enfermas | Quantidade de Pessoas Enfermas | Numérico | 6 | Não | |
| Pessoas com Feridas Leves | Quantidade de Pessoas com Feridas Leves | Numérico | 6 | Não | |
| Pessoas com Feridas Graves | Quantidade de Pessoas com Feridas Graves | Numérico | 6 | Não | |
| Pessoas Afetadas com Outras Opções | Quantidade de Pessoas Efetadas com Outras Opções | Numérico | 6 | Não | |
| Quantidade de Residências | quantidade de residências afetadas | Numérico | 6 | Não | |
| Quantidade de Comércios | quantidade de comércios afetados | Numérico | 6 | Não | |
| Quantidade de Indústrias | quantidade de indústrias afetadas | Numérico | 6 | Não | |
| Quantidade de Prédios Públicos | quantidade de prédios públicos | Numérico | 6 | Não | |
| Quantidade de Instituições de Ensino | quantidade de instituições de ensino | Numérico | 6 | Não | |
| Quantidade de Estabelecimentos de Saúde | Quantidade de estabelecimentos de saúde | Numérico | 6 | Não | |
| Quantidade de Obras de Arte | Quantidade de obras de arte | Numérico | 6 | Não | |
| Quantidade de Estradas | Quantidade de estradas | Numérico | 6 | Não | |
| Quantidade de Danos Ambientais | Quantidade de danos ambientais | Numérico | 6 | Não | |
| Danos Ambientais | Lista de danos ambientais do desastre | Lista Código Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Dano Ambiental". |
| Serviços Afetados | Lista de serviços afetados | Lista Código Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Serviço Afetado". |

---

## Tabela 42 – Vistoria Técnica

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Pessoa Jurídica | Identificador da Pessoa Jurídica | Numérico | 4 | Não | |
| Tipo de Vistoria | Tipo de Vistoria (Fiscalização, regularização, operacional) | Texto | 3 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Vistoria". |
| Motivo da Vistoria | Motivo da vistoria (regularização, denúncia..) | Texto | 3 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Motivo da Vistoria". |
| Simplificação/isenção do processo e segurança contra incêndio | Indicativo de simplificação do processo | Booleano | 1 | Sim | |
| Opção de Isenção | Opção de isenção | Texto | 3 | *Não | - Obrigatório quando no "Simplificação/isenção do processo" for SIM.<br>- Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Opção de Isenção de Vistoria". |
| Número do Processo de Prevenção | Número do processo de prevenção | Texto | 20 | Não | |
| Data de Concessão do Alvará | Data de concessão do alvará | Data | 10 | Não | - Formato Esperado: dd/mm/aaaa |
| Ocupação | Tipo de ocupação do imóvel | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Uso/Ocupação da Edificação". |
| Número do Pavimentos | Número de pavimentos vistoriados | Numérico | 3 | Não | |
| Área (m²) | área abrangida pela vistoria | Numérico | 6 | Não | |
| Observações | observações da vistoria | Texto | 250 | Não | |
| Resultado da Ação de Vistoria | Lista de resultados da vistoria | Lista Código Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Resultado da Ação de Vistoria". |
| Outro Resultado da Ação de Vistoria | Outro resultado da ação de vistoria quando a opção de "Resultado da ação da vistoria" for OUTRO. | Texto | 50 | Não | |
| Tipo de Irregularidade | Lista de medidas de segurança para descriçã das irregularidades adotadas | Lista Código Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Irregularidade da Vistoria". |
| Descrição das Irregularidades | Campo livre para descrever as irregularidades encontradas | Texto | 200 | Não | |

---

## Tabela 43 – Documentos Anexos ao Procedimento

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Descrição do Arquivo em Anexo | Descrição do Documento em Anexo | Texto | Sim | 100 | |
| Tipo de Arquivo em Anexo | Tipo de Arquivo em Anexo ao B.O (Imagem, Documento, Formulário, Áudio, Vídeo) | Numérico | Sim | 6 | |
| Arquivo | Arquivo em anexo | Arquivo | Sim | - | - Tamanho máximo: <<a definir>><br>- Formatos: <<a definir>> |

---

## Tabela 44 – BOAT - Informações do BOAT

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Natureza do Acidente | Natureza do acidente de trânsito | Numérico | 6 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Naturezas Ocorrência". |
| Indicativo de Suspeita de Álcool | Indicativo de suspeita de álcool quando no momento do acidente | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Suspeita de Álcool". |
| Indicativo Exame Alcoolico realizado | Indicativo se exame alcóolico foi realizado | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Exame Alcoólico Realizado". |
| Tipo de Acidente | Tipo de acidente registrado (Atropelamento com animal, Capotamento, colisão...) | Lista Código Texto | 3 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Acidente de Trânsito". |
| Descrição do Tipo de Acidente | Descrição do tipo de acidente | Texto | 100 | Não | |
| Condições Metereológicas | Lista com as condições metereológicas (Céu claro, chuva, garoa...) | Lista Código Texto | 3 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Condição Metereológica". |
| Tipo de Pavimento | Tipo de acidente registrado | Lista de (Pavimento) | 6 | Não | |
| Condições da Via | Lista com as condições da Via. (Alagada, com areia, com buraco...) | Lista Código Texto | 3 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Condição da Via". |
| Traçado da Via | Lista de traçados da via. (Bifurcação, cruzamento, curva...) | Lista Código Texto | 3 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo Traçado da Via". |
| Outro Traçado da Via | Descrição de Traçado da via quando "Traçado da Via" for "OUTRO". | Texto | 50 | Não | |
| Tipo de Luminosidade | Tipo de Luminosidade. (Amanhecendo, anoitecendo, iluminação insuficiente...) | Texto | 3 | Sim | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Luminosidade". |
| Faixa de Pedestre | Indicativo se tem faixa de pedestre | Booleano | 1 | Não | |
| Faixas ou Pistas Exclusivas de Ônibus | Indicativo se há faixas exclusivas de ônibus | Booleano | 1 | Não | |
| Do Agente de Trânsito | Indicativo se a BOAT veio de um agente de trânsito | Booleano | 1 | Não | |
| Lombada Eletrônica | Indicativo de lombada eletrônica | Booleano | 1 | Não | |
| Condição do semáforo | Condição de operação do semáforo. (Pleno funcionamento, Defeituoso, Não existente) | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Condição do Semáforo". |
| Velocidade Máxima Permitida | Qual a velocidade máxima da via. (20, 30, 40...) | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Velocidade Máxima da Via". |
| Tipo de Opção de Pista | Qual o tipo de opção de pista. (Simples, dupla, múltipla, auxiliar...) | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Opção de Pista". |
| Outro Tipo de Pista | Envio quando o "Tipo de Pista" for igual a "OUTRO" | Texto | 50 | Não | |
| Tipo de Linha Pista | Tipo de linha da pista. (Tracejada, Contínua, Não Existente). | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Linha de Pista". |
| Faixa de Trânsito | Qual a faixa onde estava o veículo | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Faixa de Trânsito". |
| Sinalização Vertical | Descrição da sinalização vertical | Texto | 400 | Não | |

---

## Tabela 45 – BOAT - Pavimento

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Tipo de Pavimento | Tipo de acidente registrado (Areia, Asfalto, Cascalho, Concreto...) | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Pavimento". |
| Situação do Pavimento | Situação do pavimento (Ruim, Regular e Bom) | Texto | 3 | Não | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Situação do Pavimento". |

---

## Tabela 46 – Identificação Veículo Dano - BOAT

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Identificador Veículo | Identificação do veículo | Numérico | Sim | 4 | |
| Lista Danos | Listagem com Danos do Veículo | Listagem Peça ou Componente do Veículo Danificado | Sim | 6 | |

---

## Tabela 47 – Peça ou Componente do Veículo Danificado - BOAT

| Nome do Campo | Descrição | Tipo | Obrigatório | Tamanho | Regras de Recebimento |
|---------------|-----------|------|-------------|---------|------------------------|
| Grupo Peça | Grupo Peça do Veículo | Texto | Não | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Grupo de Peças de Componente de Veículo". |
| Tipo Peça | Tipo Peça/Componente do Veículo | Texto | Não | 3 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Tipo de Peça de Componente de Veículo". |
| Dano do Componente | Classificação do dano da peça do veículo (NA - Não Avaliado, Não, Sim) | Numérico | Sim | 6 | - Valores fornecidos pelos serviços disponíveis no Sinesp Dados Corporativos: "Listar Dano Veículo". |
| Justificativa | Justificativa para não ter sido avaliado. | Texto | Não | 50 | - Valor disponível apenas quando "Dano do Componente" for "NA - Não Avaliado". |

---

## Tabela 48 – Resposta ao Enviar B.O Integração

| Nome do Campo | Descrição | Tipo | Obrigatório | Regras |
|---------------|-----------|------|-------------|--------|
| Protocolo de Recebimento | Protocolo único para o recebimento do procedimento | Texto | Sim | - Toda mensagem recepcionada pelo sistema gerará um número de protocolo de recebimento. |
| Número do Procedimento | Número do procedimento do B.O | Texto | Sim | - (RETORNO do número que foi passado)<br>- Caso não seja passado número do procedimento, deverá ser retornado mensagem: [935250: MSG - B.O Recusado.](#) E000 - ERRO. Os campos mínimos do procedimento não foram identificados para o correto recebimento. |
| Data e Hora do Recebimento | Data e Hora do Recebimento do Procedimento | Data | Sim | - Será retornado se passar pelas regras de recebimento. |
| Resposta de Recebimento | Resposta da solicitação de envio do B.O. | Texto | Sim | - Valores: B.O em fila de processamento / B.O Recusado |
| Motivo(s) da Recusa de Recebimento | Motivo da Recusa no Recebimento do B.O | Texto | Não | - Caso o B.O seja recusado já na fase de recebimento, enviar o(s) motivo(s) da recusa.<br>- Esse registro de recusa também estará disponível na consulta de procedimentos. |

---

## Tabela 49 – Filhos

| Nome do Campo | Descrição | Tipo | Tamanho | Obrigatório | Regras de Recebimento |
|---------------|-----------|------|---------|-------------|------------------------|
| Nome Filho | Nome do Filho | Texto | 200 | Não | |
| Código Sexo | Código corporativo do sexo do filho | Texto | 6 | Não | |
| Idade | Idade do filho | Numérico | 3 | Não | - Idade não pode ser negativa |
| Indicativo Idade Aproximada | Indicativo se a idade informada é aproximada | Booleano | 5 | Não | |
| Data de Nascimento | Data de nascimento do Filho | Texto | 10 | Não | - Formato esperado: 'dd/mm/aaaa' |

---

## Tabela 50 – Regras Gerais de Processamento

| Situação | Descrição |
|----------|-----------|
| B.O idêntico ao último enviado | Se o B.O envio for idêntico a todos os campos do último B.O, deve-se desprezar o B.O enviado e gerar no log de processamento a mensagem: [936600: MSG - B.O não registrado.](#) Procedimento não recepcionado devido a não identificação de diferença(s) entre o último procedimento enviado. |

---

## Tabela 51 – Situações do B.O

| Situação | Descrição |
|----------|-----------|
| Recusado | Não passou pela validação dos campos obrigatórios, tipagem inválida, tamanhos mínimos/máximos ou dados de domínio fora do esperado. |
| Protocolado | Possui os campos obrigatórios preenchidos e os dados informados são válidos. |
| Processado | Passou pela verificação das regras de negócio. B.O recepcionado. |
## 📌 HU - Enviar Boletim de Ocorrência Unificado

| :--- | :--- |
| **Prioridade** | <kbd>🔴 Alta</kbd> |
| **Complexidade** | ⭐⭐⭐⭐⭐ (Alta) |
| **Autor** | Ricardo Lopes de Lima |
| **Versão** | v1.0 |
| **Status** | ✅ Aprovado |

---

# Controle de Versão# Documentação do JSON - Estrutura "Pessoas"

Este documento descreve todos os campos do array `pessoas` presente no JSON de entrada, organizados por nível hierárquico.

---

## 1. Nível Raiz – Objeto `Pessoa`

| Campo | Tipo | Obrigatório? | Observações / Exemplo |
| :--- | :--- | :--- | :--- |
| `idPessoa` | `string` | Sim | Identificador único. Ex: `"PESSOA1"` |
| `codigoTipoPessoa` | `string` | Sim | Tipo de pessoa. Ex: `"FIS"` (Física) ou `"JUR"` (Jurídica) |
| `indicativoPessoaDesconhecida` | `boolean` | Não | Indica se a pessoa não foi identificada. Ex: `false` |
| `informacoesNoFato` | `objeto` | Não | Circunstâncias da ocorrência (detalhado na Tabela 2) |
| `dadosPessoa` | `objeto` | Não | Dados cadastrais e biográficos (detalhado na Tabela 3) |
| `partesCorpo` | `array` | Não | Lista de características físicas/marcas (detalhado na Tabela 5) |
| `deficiencias` | `array` | Não | Lista de deficiências (detalhado na Tabela 6) |
| `objetoRelacionamento` | `array` | Não | **⚠️ Inconsistente:** Objeto com campo vazio e faltando chave. Validar estrutura. |
| `documentos` | `array` | Não | Lista de documentos (detalhado na Tabela 7) |
| `enderecos` | `array` | Não | Lista de endereços (detalhado na Tabela 8) |
| `emails` | `array` | Não | Lista de e-mails (detalhado na Tabela 9) |
| `telefones` | `array` | Não | Lista de telefones (detalhado na Tabela 10) |
| `redesSociais` | `array` | Não | Lista de redes sociais (detalhado na Tabela 11) |
| `listaFilhos` | `array` | Não | **⚠️ Array vazio** no exemplo. Estrutura dos filhos não definida. |
| `profissoes` | `array` | Não | Lista de profissões/ocupações (detalhado na Tabela 12) |
| `naturezas` | `array` | Não | Lista de naturezas/envolvimentos criminais (detalhado na Tabela 13) |

---

## 2. Objeto `informacoesNoFato`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `codigoEstadoAparente` | `string` | Estado aparente. Ex: `"AGR"` (Agressivo) |
| `indicativoTornozeleiraEletronica` | `boolean` | Usa tornozeleira? Ex: `true` |
| `numeroTornozeleira` | `string` | Número do equipamento. Ex: `"3232323"` |
| `utilizaTornozeleiraEletronica` | `boolean` | (Redundante com o campo acima). Ex: `true` |
| `indicativoIntegranteFaccaoCriminosa` | `boolean` | É de facção? Ex: `true` |
| `códigoFaccaoCriminosa` | `string` | **⚠️ Acento:** `código` com acento. Ex: `"191"` |
| `outraFaccaoCriminosa` | `string` | Nome da facção. Ex: `"Bonde do Tigrão"` |
| `indicativoTurista` | `boolean` | É turista? Ex: `false` |
| `situacaoRua` | `boolean` | Está em situação de rua? Ex: `false` |
| `fatoOcorridoEmServico` | `boolean` | Ocorreu durante serviço/trabalho? Ex: `false` |

---

## 3. Objeto `dadosPessoa` (Dados Cadastrais)

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `cpf` | `string` | CPF (apenas números). Ex: `"11872665071"` |
| `nomeCompleto` | `string` | Nome registral. Ex: `"Marcelo Batista Soares"` |
| `nomeSocial` | `string` | Nome social. Ex: `"Marcelina"` |
| `codigoOpcaoNomeSocial` | `string` | Código da opção. Ex: `"NQI"` (Não quis informar) |
| `alcunha` | `string` | Apelido/alcunha. Ex: `"Marcelina do Bonde"` |
| `dataNascimento` | `string` | Data de nascimento (formato DD/MM/AAAA). Ex: `"24/09/1985"` |
| `idadeInformada` | `integer` | Idade numérica. Ex: `39` |
| `indicativoIdadeAproximada` | `boolean` | Idade é aproximada? Ex: `false` |
| `codigoRacaCor` | `string` | Código da raça/cor. Ex: `"BRA"` (Branca) |
| `codigoEstadoCivil` | `string` | Código do estado civil. Ex: `"CAS"` (Casado) |
| `codigoNacionalidade` | `string` | Código do país. Ex: `"31"` (Brasil) |
| `codigoUfNaturalidade` | `string` | UF de nascimento. Ex: `"CE"` |
| `codigoMunipioNaturalidade` | `string` | **⚠️ Erro de digitação:** `Municipio` sem 'c'. Ex: `"2300309"` |
| `nomeMunicipioProvincia` | `string` | Nome do município (se não usar código). Ex: `null` |
| `codigoEscolaridade` | `string` | Código de escolaridade. Ex: `"SCO"` (Superior Completo) |
| `codigoTipoFisico` | `string` | Tipo físico. Ex: `"NOR"` (Normal) |
| `altura` | `integer` | Altura em centímetros. Ex: `154` |
| `filiacao` | `objeto` | **Subobjeto** (detalhado abaixo). Contém `nomePai` e `nomeMae`. |
| `codigoTipoSexo` | `string` | Sexo biológico. Ex: `"MAS"` (Masculino) |
| `codigoIdentidadeGenero` | `string` | Identidade de gênero. Ex: `"HOM"` (Homem) |
| `codigoOrientacaoSexual` | `string` | **⚠️ Repetido 2x** no JSON. Ex: `"BIS"` (Bissexual) |
| `codigoAutoDeclaracaoGenero` | `string` | Autodeclaração de gênero. Ex: `"TRS"` (Travesti) |

### Subobjeto `filiacao` (dentro de `dadosPessoa`)

| Campo | Tipo | Exemplo |
| :--- | :--- | :--- |
| `nomePai` | `string` | `"Francisco Ademir da Silva"` |
| `nomeMae` | `string` | `"Gildeclina de Souza Batista Soares"` |

---

## 4. Array `partesCorpo`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `codigoParteCorpo` | `string` | Código da parte do corpo. Ex: `"33"` (Braço Direito) |
| `codigoIdentificacaoVisual` | `string` | Código da marca (tatuagem, cicatriz, etc.). Ex: `"81"` (Tatuagem) |
| `codigoCorIdentificacaoVisual` | `string` | Cor da marca. Ex: `null` |
| `descricaoParteCorpo` | `string` | Descrição textual. Ex: `"Desenho de uma águia."` |

---

## 5. Array `deficiencias`

| Campo | Tipo | Exemplo |
| :--- | :--- | :--- |
| `codigoTipoDeficiencia` | `string` | Código da deficiência. Ex: `"MEN"` (Mental) / `"AUD"` (Auditiva) |
| `nomeDeficiencia` | `string` | Nome descritivo. Ex: `"Transtorno Bipolar"` |

---

## 6. Array `documentos`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `codTipoDocumento` | `string` | Código do tipo. Ex: `"11"` (CNH) |
| `nomeProprietario` | `string` | Nome do dono do documento. Ex: `"Marcelo Batista Soares"` |
| `codTipoPropriedade` | `string` | Ex: `"PAR"` (Particular) |
| `numero` | `string` | Número do documento. Ex: `"81145417948"` |
| `numeroMatricula` | `string` | Ex: `null` |
| `numeroLivroFls` | `string` | Ex: `null` |
| `numeroAverbacao` | `string` | Ex: `null` |
| `numeroSerie` | `string` | Ex: `null` |
| `numeroSecao` | `string` | Ex: `null` |
| `numeroZona` | `string` | Ex: `null` |
| `orgaoExpedidor` | `string` | Órgão emissor. Ex: `"DETRAN-CE"` |
| `numeroAgencia` | `string` | Ex: `null` |
| `codigoPais` | `string` | Código do país. Ex: `"31"` |
| `codigoUf` | `string` | UF. Ex: `"CE"` |
| `codigoMunicipio` | `string` | Código do município. Ex: `"2304103"` |
| `dataAdmissao` | `string` | Ex: `null` |
| `dataAverbacao` | `string` | Ex: `null` |
| `dataExpedicao` | `string` | Data de emissão. Ex: `"12/01/2023"` |
| `dataValidade` | `string` | Data de validade. Ex: `"12/01/2028"` |
| `dataPrimeiraHabilitacao` | `string` | 1ª habilitação (CNH). Ex: `"18/12/2002"` |
| `numeroRenavam` | `string` | Ex: `null` |
| `codigoTipoPassaporte` | `string` | Ex: `null` |
| `fundamentacao` | `string` | Ex: `null` |
| `descricao` | `string` | Ex: `null` |
| `indicativoAdulterado` | `boolean` | Foi adulterado? Ex: `false` |
| `numeroPis` | `string` | Número PIS. Ex: `null` |

---

## 7. Array `enderecos`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `indicativoEndereçoPrincipal` | `boolean` | **⚠️ Acento:** `Endereço` com 'ç'. Ex: `true` |
| `codigoTipoEndereco` | `string` | Ex: `"2"` (Residencial) |
| `cep` | `string` | CEP. Ex: `"60014088"` |
| `logradouro` | `string` | Nome da rua/avenida. Ex: `"Avenida José Bastos"` |
| `numeroLogradouro` | `string` | Número. Ex: `"654"` |
| `complementoLogradouro` | `string` | Complemento. Ex: `""` |
| `codigoPais` | `string` | Código do país. Ex: `"31"` |
| `codigoUF` | `string` | UF. Ex: `"CE"` |
| `codigoMunicipio` | `string` | Código IBGE do município. Ex: `"2300309"` |
| `nomeProvincia` | `string` | Ex: `null` |
| `codigoBairro` | `string` | Código do bairro. Ex: `"2300309001"` |
| `nomeBairro` | `string` | Nome do bairro. Ex: `"Centro - Catalão"` |
| `enderecoLatitude` | `float` | Coordenada. Ex: `-293.32` |
| `enderecoLongitude` | `float` | Coordenada. Ex: `32.32` |

---

## 8. Array `emails`

| Campo | Tipo | Exemplo |
| :--- | :--- | :--- |
| `email` | `string` | `"marcosandrefilho@gmail.com"` |

---

## 9. Array `telefones`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `ddi` | `string` | Código internacional. Ex: `null` |
| `codigoPais` | `string` | Código do país. Ex: `"31"` |
| `ddd` | `string` | DDD. Ex: `"085"` |
| `numero` | `string` | Número do telefone. Ex: `"9762526325"` |
| `indicativoWhatsApp` | `boolean` | É WhatsApp? Ex: `true` |
| `codigoTipoTelefone` | `string` | Tipo. Ex: `"CEL"` (Celular) |
| `nomeContatoRecado` | `string` | Nome para recado. Ex: `""` |

---

## 10. Array `redesSociais`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `codigoRedeSocial` | `string` | Código padrão. Ex: `"YOU"` (YouTube), `"INS"` (Instagram) |
| `nomeRedeSocial` | `string` | Nome alternativo (quando não há código). Ex: `"Grindle"` |
| `nomeUsuario` | `string` | @ ou nome de perfil. Ex: `"gildogol"` |

---

## 11. Array `profissoes`

| Campo | Tipo | Exemplo |
| :--- | :--- | :--- |
| `codigoTipoTrabalho` | `string` | Ex: `"FOR"` (Formal) / `"INF"` (Informal) |
| `codigoProfissao` | `string` | Código CBO ou similar. Ex: `"235"` (Analista) |
| `rendaMensalRS` | `float` | Renda em Reais. Ex: `6392.50` |

---

## 12. Array `naturezas` (Envolvimentos/Infrações)

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `codigoNatureza` | `string` | Código da natureza/artigo. Ex: `"24152"` |
| `indicativoViolenciaMulher` | `boolean` | Violência contra mulher? Ex: `false` |
| `indicativoTraficoPessoa` | `boolean` | Tráfico de pessoas? Ex: `false` |
| `indicativoTentativa` | `boolean` | Foi tentativa? Ex: `null` |
| `indicativoFatoOcorridoEvento` | `boolean` | Ocorreu em evento? Ex: `true` |
| `nomeEvento` | `string` | Nome do evento. Ex: `"Encontro das Paquitas"` |
| `meiosEmpregados` | `array` | Lista de objetos com `codigoMeioEmpregado` (ex: `"2"` = Arma de Fogo) |
| `modusOperandi` | `string` | Modo de operação. Ex: `"3"` |
| `envolvimentos` | `array` | Lista de objetos com `codigoEnvolvimento` (ex: `"SAI"` = Suposto Autor) |

---

## ⚠️ Lista de Inconsistências Encontradas no JSON

1. **`objetoRelacionamento`**: Objeto mal formatado com uma chave vazia (`""`). Verificar a estrutura esperada.
2. **Campos com Acento**: `códigoFaccaoCriminosa` e `indicativoEndereçoPrincipal` – mantenha igual ao original ou padronize sem acentos para evitar bugs.
3. **`codigoMunipioNaturalidade`**: Digitação errada (faltou o 'c' na palavra Município).
4. **`codigoOrientacaoSexual`**: Repetido duas vezes dentro de `dadosPessoa`. Remover a duplicata.
5. **`listaFilhos`**: Array vazio. Se houver estrutura, será necessário mapear depois.

| Número Demanda | Nome Demanda | Modificações | Data Modificação | Usuário Alterador |
| :---: | :---: | :---: | :--- | :--- |
| **472728** | Sinesp Integração - Integração Procedimentos | Requisito Inicial | 27/06/2026 | Ricardo Lopes de Lima |

---

**📝 Descrição**  
O sistema deve gerar um relatório financeiro consolidado em formato `.xlsx`, contendo as colunas: Data, Cliente, Valor Bruto, Impostos e Valor Líquido.

**✅ Critérios de Aceitação:**
1. O código deve expirar em 60 segundos.
2. O usuário pode solicitar reenvio de SMS até 3 vezes.
3. Em caso de falha 5 vezes consecutivas, a conta é bloqueada por 15 minutos.


---


# Documentação do JSON - Estrutura "Pessoas"

Este documento descreve todos os campos do array `pessoas` presente no JSON de entrada, organizados por nível hierárquico.

---

## 1. Nível Raiz – Objeto `Pessoa`

| Campo | Tipo | Obrigatório? | Observações / Exemplo |
| :--- | :--- | :--- | :--- |
| `idPessoa` | `string` | Sim | Identificador único. Ex: `"PESSOA1"` |
| `codigoTipoPessoa` | `string` | Sim | Tipo de pessoa. Ex: `"FIS"` (Física) ou `"JUR"` (Jurídica) |
| `indicativoPessoaDesconhecida` | `boolean` | Não | Indica se a pessoa não foi identificada. Ex: `false` |
| `informacoesNoFato` | `objeto` | Não | Circunstâncias da ocorrência (detalhado na Tabela 2) |
| `dadosPessoa` | `objeto` | Não | Dados cadastrais e biográficos (detalhado na Tabela 3) |
| `partesCorpo` | `array` | Não | Lista de características físicas/marcas (detalhado na Tabela 5) |
| `deficiencias` | `array` | Não | Lista de deficiências (detalhado na Tabela 6) |
| `objetoRelacionamento` | `array` | Não | **⚠️ Inconsistente:** Objeto com campo vazio e faltando chave. Validar estrutura. |
| `documentos` | `array` | Não | Lista de documentos (detalhado na Tabela 7) |
| `enderecos` | `array` | Não | Lista de endereços (detalhado na Tabela 8) |
| `emails` | `array` | Não | Lista de e-mails (detalhado na Tabela 9) |
| `telefones` | `array` | Não | Lista de telefones (detalhado na Tabela 10) |
| `redesSociais` | `array` | Não | Lista de redes sociais (detalhado na Tabela 11) |
| `listaFilhos` | `array` | Não | **⚠️ Array vazio** no exemplo. Estrutura dos filhos não definida. |
| `profissoes` | `array` | Não | Lista de profissões/ocupações (detalhado na Tabela 12) |
| `naturezas` | `array` | Não | Lista de naturezas/envolvimentos criminais (detalhado na Tabela 13) |

---

## 2. Objeto `informacoesNoFato`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `codigoEstadoAparente` | `string` | Estado aparente. Ex: `"AGR"` (Agressivo) |
| `indicativoTornozeleiraEletronica` | `boolean` | Usa tornozeleira? Ex: `true` |
| `numeroTornozeleira` | `string` | Número do equipamento. Ex: `"3232323"` |
| `utilizaTornozeleiraEletronica` | `boolean` | (Redundante com o campo acima). Ex: `true` |
| `indicativoIntegranteFaccaoCriminosa` | `boolean` | É de facção? Ex: `true` |
| `códigoFaccaoCriminosa` | `string` | **⚠️ Acento:** `código` com acento. Ex: `"191"` |
| `outraFaccaoCriminosa` | `string` | Nome da facção. Ex: `"Bonde do Tigrão"` |
| `indicativoTurista` | `boolean` | É turista? Ex: `false` |
| `situacaoRua` | `boolean` | Está em situação de rua? Ex: `false` |
| `fatoOcorridoEmServico` | `boolean` | Ocorreu durante serviço/trabalho? Ex: `false` |

---

## 3. Objeto `dadosPessoa` (Dados Cadastrais)

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `cpf` | `string` | CPF (apenas números). Ex: `"11872665071"` |
| `nomeCompleto` | `string` | Nome registral. Ex: `"Marcelo Batista Soares"` |
| `nomeSocial` | `string` | Nome social. Ex: `"Marcelina"` |
| `codigoOpcaoNomeSocial` | `string` | Código da opção. Ex: `"NQI"` (Não quis informar) |
| `alcunha` | `string` | Apelido/alcunha. Ex: `"Marcelina do Bonde"` |
| `dataNascimento` | `string` | Data de nascimento (formato DD/MM/AAAA). Ex: `"24/09/1985"` |
| `idadeInformada` | `integer` | Idade numérica. Ex: `39` |
| `indicativoIdadeAproximada` | `boolean` | Idade é aproximada? Ex: `false` |
| `codigoRacaCor` | `string` | Código da raça/cor. Ex: `"BRA"` (Branca) |
| `codigoEstadoCivil` | `string` | Código do estado civil. Ex: `"CAS"` (Casado) |
| `codigoNacionalidade` | `string` | Código do país. Ex: `"31"` (Brasil) |
| `codigoUfNaturalidade` | `string` | UF de nascimento. Ex: `"CE"` |
| `codigoMunipioNaturalidade` | `string` | **⚠️ Erro de digitação:** `Municipio` sem 'c'. Ex: `"2300309"` |
| `nomeMunicipioProvincia` | `string` | Nome do município (se não usar código). Ex: `null` |
| `codigoEscolaridade` | `string` | Código de escolaridade. Ex: `"SCO"` (Superior Completo) |
| `codigoTipoFisico` | `string` | Tipo físico. Ex: `"NOR"` (Normal) |
| `altura` | `integer` | Altura em centímetros. Ex: `154` |
| `filiacao` | `objeto` | **Subobjeto** (detalhado abaixo). Contém `nomePai` e `nomeMae`. |
| `codigoTipoSexo` | `string` | Sexo biológico. Ex: `"MAS"` (Masculino) |
| `codigoIdentidadeGenero` | `string` | Identidade de gênero. Ex: `"HOM"` (Homem) |
| `codigoOrientacaoSexual` | `string` | **⚠️ Repetido 2x** no JSON. Ex: `"BIS"` (Bissexual) |
| `codigoAutoDeclaracaoGenero` | `string` | Autodeclaração de gênero. Ex: `"TRS"` (Travesti) |

### Subobjeto `filiacao` (dentro de `dadosPessoa`)

| Campo | Tipo | Exemplo |
| :--- | :--- | :--- |
| `nomePai` | `string` | `"Francisco Ademir da Silva"` |
| `nomeMae` | `string` | `"Gildeclina de Souza Batista Soares"` |

---

## 4. Array `partesCorpo`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `codigoParteCorpo` | `string` | Código da parte do corpo. Ex: `"33"` (Braço Direito) |
| `codigoIdentificacaoVisual` | `string` | Código da marca (tatuagem, cicatriz, etc.). Ex: `"81"` (Tatuagem) |
| `codigoCorIdentificacaoVisual` | `string` | Cor da marca. Ex: `null` |
| `descricaoParteCorpo` | `string` | Descrição textual. Ex: `"Desenho de uma águia."` |

---

## 5. Array `deficiencias`

| Campo | Tipo | Exemplo |
| :--- | :--- | :--- |
| `codigoTipoDeficiencia` | `string` | Código da deficiência. Ex: `"MEN"` (Mental) / `"AUD"` (Auditiva) |
| `nomeDeficiencia` | `string` | Nome descritivo. Ex: `"Transtorno Bipolar"` |

---

## 6. Array `documentos`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `codTipoDocumento` | `string` | Código do tipo. Ex: `"11"` (CNH) |
| `nomeProprietario` | `string` | Nome do dono do documento. Ex: `"Marcelo Batista Soares"` |
| `codTipoPropriedade` | `string` | Ex: `"PAR"` (Particular) |
| `numero` | `string` | Número do documento. Ex: `"81145417948"` |
| `numeroMatricula` | `string` | Ex: `null` |
| `numeroLivroFls` | `string` | Ex: `null` |
| `numeroAverbacao` | `string` | Ex: `null` |
| `numeroSerie` | `string` | Ex: `null` |
| `numeroSecao` | `string` | Ex: `null` |
| `numeroZona` | `string` | Ex: `null` |
| `orgaoExpedidor` | `string` | Órgão emissor. Ex: `"DETRAN-CE"` |
| `numeroAgencia` | `string` | Ex: `null` |
| `codigoPais` | `string` | Código do país. Ex: `"31"` |
| `codigoUf` | `string` | UF. Ex: `"CE"` |
| `codigoMunicipio` | `string` | Código do município. Ex: `"2304103"` |
| `dataAdmissao` | `string` | Ex: `null` |
| `dataAverbacao` | `string` | Ex: `null` |
| `dataExpedicao` | `string` | Data de emissão. Ex: `"12/01/2023"` |
| `dataValidade` | `string` | Data de validade. Ex: `"12/01/2028"` |
| `dataPrimeiraHabilitacao` | `string` | 1ª habilitação (CNH). Ex: `"18/12/2002"` |
| `numeroRenavam` | `string` | Ex: `null` |
| `codigoTipoPassaporte` | `string` | Ex: `null` |
| `fundamentacao` | `string` | Ex: `null` |
| `descricao` | `string` | Ex: `null` |
| `indicativoAdulterado` | `boolean` | Foi adulterado? Ex: `false` |
| `numeroPis` | `string` | Número PIS. Ex: `null` |

---

## 7. Array `enderecos`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `indicativoEndereçoPrincipal` | `boolean` | **⚠️ Acento:** `Endereço` com 'ç'. Ex: `true` |
| `codigoTipoEndereco` | `string` | Ex: `"2"` (Residencial) |
| `cep` | `string` | CEP. Ex: `"60014088"` |
| `logradouro` | `string` | Nome da rua/avenida. Ex: `"Avenida José Bastos"` |
| `numeroLogradouro` | `string` | Número. Ex: `"654"` |
| `complementoLogradouro` | `string` | Complemento. Ex: `""` |
| `codigoPais` | `string` | Código do país. Ex: `"31"` |
| `codigoUF` | `string` | UF. Ex: `"CE"` |
| `codigoMunicipio` | `string` | Código IBGE do município. Ex: `"2300309"` |
| `nomeProvincia` | `string` | Ex: `null` |
| `codigoBairro` | `string` | Código do bairro. Ex: `"2300309001"` |
| `nomeBairro` | `string` | Nome do bairro. Ex: `"Centro - Catalão"` |
| `enderecoLatitude` | `float` | Coordenada. Ex: `-293.32` |
| `enderecoLongitude` | `float` | Coordenada. Ex: `32.32` |

---

## 8. Array `emails`

| Campo | Tipo | Exemplo |
| :--- | :--- | :--- |
| `email` | `string` | `"marcosandrefilho@gmail.com"` |

---

## 9. Array `telefones`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `ddi` | `string` | Código internacional. Ex: `null` |
| `codigoPais` | `string` | Código do país. Ex: `"31"` |
| `ddd` | `string` | DDD. Ex: `"085"` |
| `numero` | `string` | Número do telefone. Ex: `"9762526325"` |
| `indicativoWhatsApp` | `boolean` | É WhatsApp? Ex: `true` |
| `codigoTipoTelefone` | `string` | Tipo. Ex: `"CEL"` (Celular) |
| `nomeContatoRecado` | `string` | Nome para recado. Ex: `""` |

---

## 10. Array `redesSociais`

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `codigoRedeSocial` | `string` | Código padrão. Ex: `"YOU"` (YouTube), `"INS"` (Instagram) |
| `nomeRedeSocial` | `string` | Nome alternativo (quando não há código). Ex: `"Grindle"` |
| `nomeUsuario` | `string` | @ ou nome de perfil. Ex: `"gildogol"` |

---

## 11. Array `profissoes`

| Campo | Tipo | Exemplo |
| :--- | :--- | :--- |
| `codigoTipoTrabalho` | `string` | Ex: `"FOR"` (Formal) / `"INF"` (Informal) |
| `codigoProfissao` | `string` | Código CBO ou similar. Ex: `"235"` (Analista) |
| `rendaMensalRS` | `float` | Renda em Reais. Ex: `6392.50` |

---

## 12. Array `naturezas` (Envolvimentos/Infrações)

| Campo | Tipo | Observações / Exemplo |
| :--- | :--- | :--- |
| `codigoNatureza` | `string` | Código da natureza/artigo. Ex: `"24152"` |
| `indicativoViolenciaMulher` | `boolean` | Violência contra mulher? Ex: `false` |
| `indicativoTraficoPessoa` | `boolean` | Tráfico de pessoas? Ex: `false` |
| `indicativoTentativa` | `boolean` | Foi tentativa? Ex: `null` |
| `indicativoFatoOcorridoEvento` | `boolean` | Ocorreu em evento? Ex: `true` |
| `nomeEvento` | `string` | Nome do evento. Ex: `"Encontro das Paquitas"` |
| `meiosEmpregados` | `array` | Lista de objetos com `codigoMeioEmpregado` (ex: `"2"` = Arma de Fogo) |
| `modusOperandi` | `string` | Modo de operação. Ex: `"3"` |
| `envolvimentos` | `array` | Lista de objetos com `codigoEnvolvimento` (ex: `"SAI"` = Suposto Autor) |

---

## ⚠️ Lista de Inconsistências Encontradas no JSON

1. **`objetoRelacionamento`**: Objeto mal formatado com uma chave vazia (`""`). Verificar a estrutura esperada.
2. **Campos com Acento**: `códigoFaccaoCriminosa` e `indicativoEndereçoPrincipal` – mantenha igual ao original ou padronize sem acentos para evitar bugs.
3. **`codigoMunipioNaturalidade`**: Digitação errada (faltou o 'c' na palavra Município).
4. **`codigoOrientacaoSexual`**: Repetido duas vezes dentro de `dadosPessoa`. Remover a duplicata.
5. **`listaFilhos`**: Array vazio. Se houver estrutura, será necessário mapear depois.

| Nome | Descrição | Tipo | Tamanho | Regras |
| :---: | :---: | :---: | :--- | :--- |
| 

**🔗 Dependências**  
- RF-015 (Cadastro de Clientes)  
- RNF-009 (Biblioteca Apache POI)  
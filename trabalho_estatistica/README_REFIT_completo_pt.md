# REFIT: Medições de consumo elétrico residencial

## 1. Sobre o conjunto de dados

O REFIT (*Electrical Load Measurements*) reúne medições de potência elétrica de residências da região de Loughborough, no Reino Unido, coletadas entre 2013 e 2014. O estudo inclui 20 residências.

A coleta foi apoiada pelo Engineering and Physical Sciences Research Council (EPSRC), por meio do projeto *Personalised Retrofit Decision Support Tools for UK Homes using Smart Home Technology*, desenvolvido em colaboração entre as universidades de Strathclyde, Loughborough e East Anglia.

Cada residência conta com um sensor para medir a potência agregada da casa e até nove sensores individuais, chamados *Individual Appliance Monitors* (IAMs). As medições correspondem à potência ativa, em watts (W), com intervalo nominal de 8 segundos.

Página do dataset: https://zenodo.org/records/5063428

## 2. Formato dos arquivos

Os dados são disponibilizados em arquivos CSV. A estrutura descrita na documentação original é:

`DATETIME, UNIX TIMESTAMP (UCT), Aggregate, Appliance1, Appliance2, Appliance3, ..., Appliance9, Issues`

- `DATETIME`: data e hora da medição.
- `UNIX TIMESTAMP`: timestamp Unix.
- `Aggregate`: potência agregada da residência, em watts.
- `Appliance1` a `Appliance9`: potência medida nos canais individuais de aparelhos, em watts.
- `Issues`: indicador de possíveis inconsistências entre a soma dos canais individuais e a potência agregada.

Confira os nomes exatos das colunas no arquivo utilizado, pois podem variar entre versões.

Os sensores não são perfeitamente sincronizados: o processo de coleta consultava os sensores a cada 6 a 8 segundos, e cada leitura pode refletir uma atualização ocorrida em algum momento desse intervalo. Os dados foram registrados principalmente quando havia alteração na carga; valores intermediários foram preenchidos quando necessário.

## 3. Limpeza dos dados

A versão limpa do dataset passou pelos seguintes tratamentos, conforme a documentação original:

- Correção dos horários relacionados ao horário de verão do Reino Unido.
- Realinhamento de trechos das colunas dos sensores individuais para corresponder ao aparelho monitorado após reinicializações ou mudanças feitas pelos moradores.
- Preenchimento para frente (*forward fill*) de valores ausentes em algumas colunas.
- Remoção de picos superiores a 4.000 W nos canais individuais e substituição desses valores por zero.
- Inclusão da coluna `Issues`, definida como `1` quando a soma das medições individuais (IAMs) é maior que a potência agregada da residência.

A documentação recomenda descartar ou sinalizar os registros em que `Issues = 1`, ou, no mínimo, discutir a discrepância na análise. Também existem períodos com dados ausentes, especialmente em fevereiro de 2014, devido a falhas de internet, hardware e rede.

Atenção: como parte dos valores ausentes foi preenchida para frente e alguns picos foram substituídos por zero, esses valores não devem ser interpretados automaticamente como leituras originais dos sensores.

### Versões dos arquivos

Segundo a documentação:
- O sufixo `_Part1` refere-se à primeira versão da base, na qual sensores indisponíveis foram registrados como `0`.
- O sufixo `_Part2` refere-se à segunda versão, na qual sensores indisponíveis foram registrados como `NaN`, permitindo distingui-los de leituras iguais a zero.

## 4. Informações gerais das residências

A tabela abaixo reproduz as informações de residência apresentadas no README original. A House 14 não aparece na lista.

| Residência | Ocupantes | Ano de construção | Aparelhos próprios (quantidade) | Tipo de residência | Tamanho |
|---|---:|---|---:|---|---|
| House 1 | 2 | 1975–1980 | 35 | Independente (*detached*) | 4 quartos |
| House 2 | 4 | Não informado | 15 | Geminada por uma lateral (*semi-detached*) | 3 quartos |
| House 3 | 2 | 1988 | 27 | Independente (*detached*) | 3 quartos |
| House 4 | 2 | 1850–1899 | 33 | Independente (*detached*) | 4 quartos |
| House 5 | 4 | 1878 | 44 | Casa em fileira, unidade intermediária (*mid-terrace*) | 4 quartos |
| House 6 | 2 | 2005 | 49 | Independente (*detached*) | 4 quartos |
| House 7 | 4 | 1965–1974 | 25 | Independente (*detached*) | 3 quartos |
| House 8 | 2 | 1966 | 35 | Independente (*detached*) | 2 quartos |
| House 9 | 2 | 1919–1944 | 24 | Independente (*detached*) | 3 quartos |
| House 10 | 4 | 1919–1944 | 31 | Independente (*detached*) | 3 quartos |
| House 11 | 1 | 1945–1964 | 25 | Independente (*detached*) | 3 quartos |
| House 12 | 3 | 1991–1995 | 26 | Independente (*detached*) | 3 quartos |
| House 13 | 4 | Após 2002 | 28 | Independente (*detached*) | 4 quartos |
| House 15 | 1 | 1965–1974 | 19 | Geminada por uma lateral (*semi-detached*) | 3 quartos |
| House 16 | 6 | 1981–1990 | 48 | Independente (*detached*) | 5 quartos |
| House 17 | 3 | Meados dos anos 1960 | 22 | Independente (*detached*) | 3 quartos |
| House 18 | 2 | 1965–1974 | 34 | Independente (*detached*) | 3 quartos |
| House 19 | 4 | 1945–1964 | 26 | Geminada por uma lateral (*semi-detached*) | 3 quartos |
| House 20 | 2 | 1965–1974 | 39 | Independente (*detached*) | 3 quartos |
| House 21 | 4 | 1981–1990 | 23 | Independente (*detached*) | 3 quartos |

**Importante:** “aparelhos próprios” corresponde à quantidade indicada na tabela original e não ao número de canais monitorados. Cada residência tem um canal agregado e até nove canais IAM.

## 5. Mapeamento dos canais por residência

A lista a seguir apresenta os aparelhos que, segundo a documentação, eram conhecidos como monitorados no início do estudo. O mapeamento pode mudar ao longo do tempo. Alguns canais, como *Television Site* e *Computer Site*, podem representar vários equipamentos conectados em conjunto.

Os nomes e modelos foram registrados pela equipe de instalação com base nas imagens coletadas. Quando a documentação informa `Unknown`, o modelo ou a identificação não era conhecido. Os canais identificados como `???` permanecem sem identificação na fonte original.

### House 1
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira (Hotpoint, RLA50P).
- `Appliance2`: freezer (1) (Beko, CF393APW).
- `Appliance3`: freezer (2) (marca e modelo desconhecidos).
- `Appliance4`: lava e seca (Creda, T522VW).
- `Appliance5`: máquina de lavar roupa (Beko, WMC6140).
- `Appliance6`: lava-louças (Bosch, modelo desconhecido).
- `Appliance7`: computador (Lenovo, H520s).
- `Appliance8`: conjunto de equipamentos de televisão (Toshiba, 32BL502b).
- `Appliance9`: aquecedor elétrico (GLEN, 2172).

### House 2
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira com freezer (marca e modelo desconhecidos).
- `Appliance2`: máquina de lavar roupa (LG, F1289TD).
- `Appliance3`: lava-louças (marca e modelo desconhecidos).
- `Appliance4`: conjunto de equipamentos de televisão.
- `Appliance5`: micro-ondas (marca e modelo desconhecidos).
- `Appliance6`: torradeira (marca e modelo desconhecidos).
- `Appliance7`: aparelho de som Hi-Fi (marca e modelo desconhecidos).
- `Appliance8`: chaleira elétrica (marca e modelo desconhecidos).
- `Appliance9`: ventilador de teto.

### House 3
- `Aggregate`: potência agregada da residência.
- `Appliance1`: torradeira (Dualit, DPP2).
- `Appliance2`: geladeira com freezer (Whirlpool, ARC7612).
- `Appliance3`: freezer (Frigidaire, Freezer Elite).
- `Appliance4`: secadora de roupas (modelo desconhecido).
- `Appliance5`: lava-louças (Bosch, Exxcel Auto Option).
- `Appliance6`: máquina de lavar roupa (modelo desconhecido).
- `Appliance7`: conjunto de equipamentos de televisão (Samsung, LE46A656A1FXXU).
- `Appliance8`: micro-ondas (Panasonic, NN-CT565MBPQ; grafia normalizada).
- `Appliance9`: chaleira elétrica (Dualit, JKt3; substituída em 16 de abril de 2014 por Vektra Vacuum Kettle, conforme a documentação).

### House 4
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira (Neff, K1514X0GB/31).
- `Appliance2`: freezer (Ocean, UF 1025).
- `Appliance3`: geladeira com freezer (Ariston, DF230; troca registrada em 17 de agosto de 2014).
- `Appliance4`: máquina de lavar roupa (1) (Servis, 6065).
- `Appliance5`: máquina de lavar roupa (2) (Zanussi, Z917).
- `Appliance6`: computador desktop (marca e modelo desconhecidos).
- `Appliance7`: conjunto de equipamentos de televisão (Sony, KDL-32W706B; troca registrada em 19 de dezembro de 2014).
- `Appliance8`: micro-ondas (Matsui, 170TC).
- `Appliance9`: chaleira elétrica (Swan, modelo desconhecido).

### House 5
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira com freezer (Fisher & Paykel, modelo desconhecido).
- `Appliance2`: secadora de roupas (modelo desconhecido).
- `Appliance3`: máquina de lavar roupa (AEG, L99695HWD).
- `Appliance4`: lava-louças (marca e modelo desconhecidos).
- `Appliance5`: computador desktop (marca e modelo desconhecidos; troca registrada em 17 de março de 2015).
- `Appliance6`: conjunto de equipamentos de televisão (marca e modelo desconhecidos).
- `Appliance7`: micro-ondas (marca e modelo desconhecidos).
- `Appliance8`: chaleira elétrica (Logik, L17SKC14).
- `Appliance9`: torradeira (Breville, TT33).

### House 6
- `Aggregate`: potência agregada da residência.
- `Appliance1`: freezer (Whirlpool, CV128W).
- `Appliance2`: máquina de lavar roupa (Bosch, Classixx 1200 Express).
- `Appliance3`: lava-louças (Neff, modelo desconhecido).
- `Appliance4`: conjunto de equipamentos de computador (MJY Computer; modelo desconhecido).
- `Appliance5`: televisão/satélite (Samsung, UE55F6500SB).
- `Appliance6`: micro-ondas (Neff, H5642N0GB/02).
- `Appliance7`: chaleira elétrica (ASDA, GPK101W).
- `Appliance8`: torradeira (Breville, PT15).
- `Appliance9`: conjunto de equipamentos de computador (PGM Computer; modelo desconhecido).

### House 7
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira (Bosch, KSR30422GB).
- `Appliance2`: freezer (1) (Whirlpool, AFG 392/H).
- `Appliance3`: freezer (2) (marca e modelo desconhecidos; troca registrada em 23 de novembro de 2013).
- `Appliance4`: secadora de roupas (White Knight, modelo desconhecido).
- `Appliance5`: máquina de lavar roupa (Bosch, modelo desconhecido).
- `Appliance6`: lava-louças (marca e modelo desconhecidos; troca registrada em 20 de maio de 2014).
- `Appliance7`: conjunto de equipamentos de televisão.
- `Appliance8`: torradeira (marca e modelo desconhecidos).
- `Appliance9`: chaleira elétrica (Sainsburys, 121988254).

### House 8
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira (Liebherr, KP2620).
- `Appliance2`: freezer (marca e modelo desconhecidos).
- `Appliance3`: lava e seca (Zanussi, modelo desconhecido).
- `Appliance4`: máquina de lavar roupa (marca e modelo desconhecidos).
- `Appliance5`: torradeira (Bosch, TAT6101GB/02).
- `Appliance6`: computador (marca e modelo desconhecidos).
- `Appliance7`: conjunto de equipamentos de televisão (Sony, KDL-32V2000).
- `Appliance8`: micro-ondas (Panasonic, NN-CT565MBPQ; grafia normalizada).
- `Appliance9`: chaleira elétrica (Morphy Richards, 43615).

### House 9
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira com freezer (Bosch, KGH34X05GB/05).
- `Appliance2`: lava e seca (Hotpoint, TCM580, conforme grafia da fonte).
- `Appliance3`: máquina de lavar roupa (Bosch, Classixx 6 1200 Express).
- `Appliance4`: lava-louças (Bosch, Classixx).
- `Appliance5`: conjunto de equipamentos de televisão (LG, 32LH3000).
- `Appliance6`: micro-ondas (Argos, MM717CFA).
- `Appliance7`: chaleira elétrica (Russell Hobbs, modelo desconhecido).
- `Appliance8`: aparelho de som Hi-Fi (marca e modelo desconhecidos).
- `Appliance9`: aquecedor elétrico.

### House 10
- `Aggregate`: potência agregada da residência.
- `Appliance1`: liquidificador Magimix (modelo desconhecido; troca registrada em 17 de junho de 2014).
- `Appliance2`: torradeira (marca e modelo desconhecidos; troca registrada em 17 de outubro de 2014).
- `Appliance3`: freezer horizontal (*chest freezer*) (marca e modelo desconhecidos).
- `Appliance4`: geladeira com freezer (marca e modelo desconhecidos).
- `Appliance5`: máquina de lavar roupa (Beko, WI1382).
- `Appliance6`: lava-louças (AEG, modelo desconhecido).
- `Appliance7`: conjunto de equipamentos de televisão (Samsung, UE40ES5500K).
- `Appliance8`: micro-ondas (marca e modelo desconhecidos).
- `Appliance9`: batedeira K Mix (marca e modelo desconhecidos).

### House 11
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira (Gorenje, HPI 1566; monitoramento iniciado em 7 de outubro de 2014).
- `Appliance2`: geladeira com freezer (marca e modelo desconhecidos).
- `Appliance3`: máquina de lavar roupa (marca e modelo desconhecidos).
- `Appliance4`: lava-louças (marca e modelo desconhecidos; troca registrada em 4 de outubro de 2014).
- `Appliance5`: conjunto de equipamentos de computador (*Computer Site*; marca e modelo desconhecidos).
- `Appliance6`: micro-ondas (marca e modelo desconhecidos).
- `Appliance7`: chaleira elétrica (marca e modelo desconhecidos).
- `Appliance8`: roteador (marca e modelo desconhecidos).
- `Appliance9`: aparelho de som Hi-Fi (marca e modelo desconhecidos).

### House 12
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira com freezer (Gorenje, HZS 3266).
- `Appliance2`: aparelho não identificado (`???`).
- `Appliance3`: aparelho não identificado (`???`).
- `Appliance4`: conjunto de equipamentos de computador (*Computer Site*).
- `Appliance5`: micro-ondas.
- `Appliance6`: chaleira elétrica.
- `Appliance7`: torradeira (documentada como “Nothing Recorded”).
- `Appliance8`: televisão (documentada como “Nothing Recorded”).
- `Appliance9`: aparelho não identificado (`???`; documentado como “Nothing Recorded”).

### House 13
- `Aggregate`: potência agregada da residência.
- `Appliance1`: conjunto de equipamentos de televisão (Samsung, UE55H6400AK).
- `Appliance2`: freezer (marca e modelo desconhecidos; monitoramento encerrado em 18 de agosto de 2014).
- `Appliance3`: máquina de lavar roupa (marca e modelo desconhecidos; mudança registrada em 25 de março de 2015).
- `Appliance4`: lava-louças (marca e modelo desconhecidos).
- `Appliance5`: aparelho não identificado (`???`).
- `Appliance6`: conjunto de equipamentos de rede (*Network Site*).
- `Appliance7`: micro-ondas.
- `Appliance8`: micro-ondas.
- `Appliance9`: chaleira elétrica.

### House 15
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira com freezer (marca e modelo desconhecidos).
- `Appliance2`: secadora de roupas (marca e modelo desconhecidos).
- `Appliance3`: máquina de lavar roupa (Beko, WMB91242LB).
- `Appliance4`: lava-louças (marca e modelo desconhecidos).
- `Appliance5`: conjunto de equipamentos de computador (*Computer Site*).
- `Appliance6`: conjunto de equipamentos de televisão (LG, 22LS4D).
- `Appliance7`: micro-ondas (marca e modelo desconhecidos).
- `Appliance8`: aparelho de som Hi-Fi (marca e modelo desconhecidos).
- `Appliance9`: torradeira (marca e modelo desconhecidos).

### House 16
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira com freezer (1) (Bosch, KGN30VW20G/01; troca registrada em 7 de abril de 2015).
- `Appliance2`: geladeira com freezer (2) (marca e modelo desconhecidos).
- `Appliance3`: aquecedor elétrico (1).
- `Appliance4`: aquecedor elétrico (2).
- `Appliance5`: máquina de lavar roupa (Bosch, WAB24262GB/01).
- `Appliance6`: lava-louças (marca e modelo desconhecidos).
- `Appliance7`: conjunto de equipamentos de computador (*Computer Site*).
- `Appliance8`: conjunto de equipamentos de televisão (Samsung, UE55HU8500T).
- `Appliance9`: desumidificador.

### House 17
- `Aggregate`: potência agregada da residência.
- `Appliance1`: freezer.
- `Appliance2`: geladeira com freezer (Whirlpool, ARC 2990).
- `Appliance3`: secadora de roupas.
- `Appliance4`: máquina de lavar roupa (Bosch, Exxcel 8 Vario Perfect).
- `Appliance5`: conjunto de equipamentos de computador (*Computer Site*).
- `Appliance6`: conjunto de equipamentos de televisão (*Television Site*).
- `Appliance7`: micro-ondas (Matsui, M195T).
- `Appliance8`: chaleira elétrica (Russell Hobbs, 17869).
- `Appliance9`: conjunto de equipamentos de televisão do quarto (*TV Site (Bedroom)*).

### House 18
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira da garagem (LEC, R.403W).
- `Appliance2`: freezer da garagem.
- `Appliance3`: geladeira com freezer.
- `Appliance4`: lava e seca da garagem.
- `Appliance5`: máquina de lavar roupa.
- `Appliance6`: lava-louças.
- `Appliance7`: computador desktop.
- `Appliance8`: conjunto de equipamentos de televisão.
- `Appliance9`: micro-ondas.

### House 19
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira com freezer (Bosch, KGS-3272-GB/01).
- `Appliance2`: máquina de lavar roupa (Bosch, WAE24060GB/03).
- `Appliance3`: conjunto de equipamentos de televisão (Sony, KDL32EX703).
- `Appliance4`: micro-ondas (Kenwood, K20MSS10).
- `Appliance5`: chaleira elétrica (Breville, VKJ336).
- `Appliance6`: torradeira (Bellini, BET240).
- `Appliance7`: máquina de fazer pão (*bread-maker*).
- `Appliance8`: console de videogame.
- `Appliance9`: aparelho de som Hi-Fi.

### House 20
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira.
- `Appliance2`: freezer.
- `Appliance3`: secadora de roupas.
- `Appliance4`: máquina de lavar roupa.
- `Appliance5`: lava-louças.
- `Appliance6`: conjunto de equipamentos de computador (*Computer Site*).
- `Appliance7`: conjunto de equipamentos de televisão (*Television Site*).
- `Appliance8`: micro-ondas.
- `Appliance9`: chaleira elétrica.

### House 21
- `Aggregate`: potência agregada da residência.
- `Appliance1`: geladeira com freezer (Samsung, SR-L3216B).
- `Appliance2`: secadora de roupas.
- `Appliance3`: máquina de lavar roupa (Beko, WMB81241LW).
- `Appliance4`: lava-louças (AEG, FAVORIT).
- `Appliance5`: batedeira/misturador de alimentos (*food mixer*).
- `Appliance6`: televisão.
- `Appliance7`: chaleira elétrica (mudança registrada em 16 de agosto de 2014).
- `Appliance8`: vivário (*vivarium*).
- `Appliance9`: bomba de lago ornamental (*pond pump*).

## 6. Cuidados ao interpretar o mapeamento

- O mapeamento acima descreve os aparelhos conhecidos no início da coleta; alguns canais tiveram trocas, encerramento de monitoramento ou alterações documentadas.
- `Computer Site`, `Television Site`, `Network Site` e termos semelhantes podem incluir mais de um equipamento conectado ao mesmo ponto de medição.
- Quando a fonte registra `Unknown`, `???` ou `Nothing Recorded`, não há informação suficiente para atribuir um modelo ou, em alguns casos, um aparelho específico.
- Não presuma que todos os canais têm dados válidos durante todo o período.
- Ao apresentar resultados, informe claramente quais canais foram analisados e quais limitações foram consideradas.

## 7. Recomendações para análise dos alunos

1. Carregue o CSV e confirme os nomes das colunas, tipos de dados e intervalo temporal.
2. Verifique valores ausentes, duplicatas, intervalos temporais irregulares e a coluna `Issues`.
3. Calcule estatísticas descritivas para `Aggregate` e para os canais individuais relevantes: média, mediana, desvio-padrão, mínimo, máximo e quartis.
4. Produza visualizações adequadas, como série temporal, histograma, boxplot e perfil por hora do dia ou dia da semana.
5. Interprete os padrões com base nos resultados, evitando atribuir causas que os dados não comprovam.
6. Diferencie potência (W) de energia (kWh). A média de potência não equivale diretamente ao consumo de energia.
7. Se estimar energia, explique o método de integração temporal e utilize os timestamps.
8. Documente todo tratamento de dados e declare as limitações da análise.

## 8. Licença e citação

O dataset está licenciado sob Creative Commons Attribution 4.0 International (CC BY 4.0):
https://creativecommons.org/licenses/by/4.0/legalcode

A documentação original solicita a citação do seguinte trabalho:

Murray, D.; Liao, J.; Stankovic, L.; Stankovic, V.; Hauxwell-Baldwin, R.; Wilson, C.; Coleman, M.; Kane, T.; Firth, S. (2015). *A data management platform for personalised real-time energy feedback*. Proceedings of the 8th International Conference on Energy Efficiency in Domestic Appliances and Lighting.

Artigo relacionado à descrição do conjunto:

Murray, D.; Stankovic, L.; Stankovic, V. (2017). *An electrical load measurements dataset of United Kingdom households from a two-year longitudinal study*. Scientific Data. https://doi.org/10.1038/sdata.2016.122

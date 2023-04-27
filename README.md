#B3 Dispatcher - B2B SFTP Stream

O **B3 Dispatcher** é uma **ferramenta** B2B concebida para o envio **automatizado** de arquivos via **SFTP** para a [B3](https://www.b3.com.br/pt_br/).

A tool, basicamente, recebe **arquivos em um diretório específico**, realiza **validações**, **organiza a ordem** de envio dos arquivos, pois há uma ordem específica, e **os envia para a B3** caso passem nas validações. Caso não passem, os arquivos são mantidos em um diretório apartado.

Todo arquivo enviado para a B3, quando processado pela empresa, recebe um arquivo de retorno que aqui chamamos de report. A ferramenta **coleta o report da B3**, o **baixa**, o **analisa** e o **aloca** em um diretório específico; caso hajam apontamentos de falha da B3 nesse report, o arquivo é alocado em um diretório para arquivos reportados com falha, ao passo que, caso não haja apontamentos no report, a alocação do arquivo é feita em um diretório para arquivos sem apontamentos.

Existem, basicamente, 3 comandos; Dois deles são interativos, o outro, não. São eles:

| **Comando** | **Descrição** |
| ------------------ | ----------------- |
|`b3cron`| Não interativo. |
|```b3cli```| Interativo, gráfico. |
|```b3dispatcher```| Interativo, não gráfico. |

A seguir, veremos o que cada um deles faz.

# Comandos b3cli e b3dispatcher

| **B3 CLI** | **B3 Dispatcher** |
| ------------------ | ----------------- |
| $ `b3cli` | $ `b3dispatcher [options]` |
| O **b3cli** é um prompt gráfico interativo e amigável para operação do b3dispatcher. A partir do teclado pode-se selecionar opções dispostas no menu de entrada. Essas opções invocam o **b3dispatcher** que realiza as ações necessárias. | Esse é o b3dispatcher. É um orquestrator de arquivos, configurações, funções, validadores e snippets, que a partir de determinados padrões toma decisões quanto a validade de arquivos para envio para a B3. |
| ![b3cli](/docs/b3cli.PNG) | ![b3dispatcher](/docs/b3dispatcher.png) |

### Observação:

> Tanto o **b3cli** quanto o **b3dispatcher** possuem interação em Português. Nomes de variáveis, funções e configurações são predominantemente em inglês, mas algumas chamadas de recursos podem ser em português para facilitar o mantenimento distribuído do código.

# Utilitário b3cron

O **b3cron** é um utilitário **não interativo** concebido para rodar rotinas a partir agendamentos de **jobs** no **Crontab** dos servidores que alocam a ferramenta, de forma que nenhuma ação de usuário seja necessária para o funcionamento do **b3dispatcher**. 

Uma vez que os arquivos a serem enviados estejam alocados no diretório apripriado, a ferramenta realiza as operações de validação e envio para a B3.

# Formas de uso:

O modo mais prático de usar o **b3dispatcher** é pelo **b3cli**. Caso queira mais possibilidade do que **b3cli** provê, é preciso usar o **b3dispatcher** mesmo.

As opções de uso do **b3dispatcher** são essas:

| Comando | Descrição |
| ------------------ | ----------------- |
|```b3dispatcher --list```| Lista os arquivos na fila de envio para a B3|
|```b3dispatcher --validate```|Valida arquivos no diretório de entrada|
|```b3dispatcher --report```|Reporta todos arquivos que não passaram na validação da tool e da B3|
|```b3dispatcher --sendfiles```|Valida e envia arquivos para a B3|
|```b3dispatcher --taillog```|Exibe logs em tempo real do B3 dispatcher|
|```b3dispatcher --printvars```|Exibe todas configurações da tool|

# Componentes do B3 Dispatcher

A seguir veremos como o B3 Dispatcher está disposto. A seguir está o Fluxograma do **b3dispatcher**:

![b3cli](/docs/b3dispatcher_fluxograma.png)

# Arquivo de configurações

A tool possui um arquivo de configurações por onde são feitos ajustes de parâmetros que determinam o funcionamento da tool.

Essas são as configurações:

| Configuração | Descrição |
| ------------------ | ----------------- |
|modal| Define o ambiente. O valor pode ser "uat" ou "prd"|
|tool_name| Nome da Ferramenta |
|cli_name| Nome do Cli |
|tool_home| Path dos Scripts da Tool |
|tool_cron_name | Nome do script b3cron
|tool| Path absoluto do b3dispatcher |
|tool_cron| Path absoluto do b3cron
|cli| Path absoluto do b3cli |
|assets_home| Path absoluto dos diretórios config, properties, rsakeys e snippets |
|config_home| Path absoluto do diretório config |
|snippets| Path absoluto do diretório snippet |
|functions_home| Path absoluto do diretório properties |
|templates| Path absoluto do Storage NFS (/b3/fileshare001) |
|input_dir| Path absoluto do diretório de entrada de arquivos no Storage NFS |
|validation_dir| Path absoluto do diretório de arquivos validados no Storage NFS |
|failed_dir| Path absoluto do diretório falhas em arquivos no Storage NFS |
|largefiles_dir| Path absoluto do diretório de arquivos vazios ou maiores do que o permitido no Storage NFS |
|failure_dir| Path absoluto do diretório de arquivos recusados pela B3 no Storage NFS |
|sent_files_waiting_b3_report|Diretório para arquivos enviados para a B3 e que aguardam recebimento de report|
|sent_files_failure_report_dir|Arquivos enviados para a b3, mas reportados com falha|
|sent_files_success_report_dir|Arquivos enviados para a b3 e sem report de falha|
|lockdir| Diretório de controle de execução |
|lockfile| Arquivo de controle de execução + PID |
|systemuser| Usuário executor da Tool |
|sent_files_failure_report_dir| Lista 1 de varíaveis de nomes de arquivos em ordem de envio|
|sent_files_success_report_dir| Lista 2 de varíaveis de nomes de arquivos em ordem de envio|
|system_dir | Diretório raiz da tool |
| empresas | Lista de empresas "01" |
|file_incl_ap| Regex para validação de arquivo|
|file_incl_en| Regex para validação de arquivo|
|file_incl_mo| Regex para validação de arquivo|
|file_incl_si| Regex para validação de arquivo|
|file_altr_si| Regex para validação de arquivo|
|file_incl_co|Regex para validação de arquivo de Resseguro|
|set1_files_order|Lista 1 de variáveis de nomes de arquivos para envio em ordem pre definida|
|set2_files_order|Lista 1 de variáveis de nomes de arquivos para envio em ordem pre definida|
|phrase_error|Mensagem de erro em arquivos oriundos do Rector usada para validação pre-envio|
|phrase_error_report|Mensagem de erro padrão da b3 para determinar análise de reports|
|invalid_file|Termo dado para arquivo não aprovado em reprocessamento|
|maxfilesize|Tamanho máximo de arquivo para envio para a B3
|waiting_collect_interval|Tempo em minutos para coleta de reports após envio de arquivos|
|mode_send_files|Modo de envio "Parallel" ou "Sequential"|
|sendinterval|Intervalo de envio de arquivos. Obsoleto.|
|target_path|Diretório de entrega de arquivos na B3|
|reports_path|Diretório de coleta de reports na B3|
|target_port|Porta de conexão no SFTP da B3
|rsakeys_path|Path do diretório onde ficam as chaves RSA para conexão na B3
|ylw| Cor amarela para impressão de tela |
|red| Cor vermelha para impressão de tela |
|grn| Cor verde para impressão de tela |
|pur| Cor rosa para impressão de tela |
|cyn| Cor azul claro para impressão de tela |
|nc| Remoção do tratamento de cores |
|date| Data no formato AAAAMMDD |
|datefull| Data no formato AAAA/MM/DD |
|year| Ano no formato AAAAA |
|month| Mês no formato MM |
|day| Dia no formato DD |
|time| Determina a hora no formato HH |
|b3_error_msg1| Mensagem de erro n1 da Whitelist|
|b3_error_msg2| Mensagem de erro n1 da Whitelist|
|b3_error_msg3| Mensagem de erro n1 da Whitelist|
|b3_error_msg4| Mensagem de erro n1 da Whitelist|
|b3_error_msg5| Mensagem de erro n1 da Whitelist|
|b3_error_whitelist| Lista de mensagens incluídas na Whitelist|

Dentro do arquivo de configurações também estão declaradas as variáveis com credenciais de acesso.

São elas:

```
# UAT: Empresa 01:
declare uat_target_empresa_01=b3_URL
declare uat_userid_empresa_01=user1
declare uat_rsakey_empresa_01="${rsakeys_path}/id_rsa_user1_uat"

# UAT: Empresa 05:
declare uat_target_empresa_05=b3_URL
declare uat_userid_empresa_05=user1
declare uat_rsakey_empresa_05="${rsakeys_path}/id_rsa_user1_uat"

# PRD: Empresa 01:
declare prd_target_empresa_01=b3_URL
declare prd_userid_empresa_01=user1Prd
declare prd_rsakey_empresa_01="${rsakeys_path}/id_rsa_user1Prd"

# PRD: Empresa 05:
declare prd_target_empresa_05=b3_URL
declare prd_userid_empresa_05=user2Prd
declare prd_rsakey_empresa_05="${rsakeys_path}/id_rsa_user2Prd"
```
Com base no **Modal** e na **Empresa**, a tool determina as credenciais que utilizará criando variáveis em **tempo de execução**. São elas:

| Variável | Descrição |
| ------------------ | ----------------- |
|host|usa modal + empresa para determinar qual host utilizará. Ex: prd_target_empresa_05|
|user|usa modal + empresa para terminar qual user utilizará. Ex: prd_userid_empresa_05|
|pass|usa modal + empresa para terminar qual chave rsa utilizará. Ex: prd_rsakey_empresa_05|

> Você pode consultar o arquivo de configurações [aqui](https://Azure_Devops_URL/b3dispatcher/_git/b3dispatcher?path=/etc/b3dispatcher/assets/config/b3dispatcher.conf) mesmo no AzureDevOps.

# Script Header

O **Script Header** é um mecanismo do b3dispatcher que valida a existência dos arquivos essenciais, paths, permissões, entre outros, para garantir o funcionamento do b3dispatcher.

A questão da permissão é importante, pois não se pode executar **snippets** da tool sem usar a própria tool.

Você pode consultar o arquivo **Script Header** [aqui](https://Azure_Devops_URL/b3dispatcher/_git/b3dispatcher?path=/etc/b3dispatcher/assets/properties/scriptheader) mesmo no AzureDevOps.

# Snippets

A tool é estruturada de forma **modular**. Ou seja, há vários scripts que a compõe. Cada script modularizado é chamado de **snippet**, ou seja, um pequeno trecho de código.

Cada **snippet** é responsável por ações específicas e podem interagir uns com os outros para realizar determinadas ações.

# Functions

As **funções** da tool estão contidas em um arquivo específico chamado **functions**.

Cada função possui uma determinada finalidade e são chamadas repetidamente ao longo do código.

Existem funções para tudo, inclusive para formatação de saída de tela.

São as funções as responsáveis por interagir com os **snippets**, executando os mesmos sempre que necessário.

# Conectividade SFTP com a B3

Temos conectividade com a B3 via protocolo **SFTP** (FTP over SSH). Ou seja, a transmissão de arquivos é **criptografada de ponta a ponta**.

O acesso é realizado por usuários criados pela B3 que são autenticados por chave **SSH RSA de 2048 bits** gera por nós.

Atualmente temos 2 usuários por ambiente, sendo dois os ambientes: UAT e PRD.

> O host de acesso muda de acordo com o ambiente.

| Ambiente | Usuário | Host | Porta |
| ------------------ | ----------------- | ------------------ | ----------------- |
|UAT|user1|b3_URL | 9039 | 
|UAT|user1|b3_URL | 9039 | 
|PRD|user1Prd|b3_URL| 9039 | 
|PRD|user2Prd|b3_URL| 9039 | 

Temos dois usuários por ambiente por que atualmente temos **duas empresas**.

Caso surjam novas empresas, haverão novos usuários. Sendo uma chave SSH RSA por usuário, novas chaves pracisarão ser geradas.

# O que é uma "Empresa"?

Esse conceito atribui um número à cada empresa do grupo. Se houver mais de uma, é preciso adicionar outra empresa.

Cada **empresa** possui seus próprios arquivos e seu próprio ambiente SFTP na B3.

A tool trata arquivos por **empresa** para enviar tais arquivos para o destino correto na B3.

Para adaptar a tool à mais de uma empresa, é preciso incluí-la no **b3dispatcher.conf**.

Por exemplo:

### b3dispatcher.conf

```
declare uat_target_empresa_02=b3_URL
declare uat_userid_empresa_02=usuario
declare uat_rsakey_empresa_02="${rsakeys_path}/chave_privada
```

No exemplo acima, estamos adicionando uma empresa "02". Em **usuario** é preciso informar o usuário que a B3 fornecerá. Em **chave privada** é preciso informar o path da chave privada.

A B3 também informará um número. Esse número precisa ser declarado na variável **empresas**.

Exemplo. Valor atual:

```empresas=01```

Novo valor:

```empresas=01,02```

> Lembrando que "02" é um exemplo.

# Validações da Tool

Atualmente existem alguns templates que são enviados para a B3.

A Tool b3dispatcher valida:

* 01 - A empresa deve ser reconhecida (Exemplo: Empresa 01);
* 02 - O nome do arquivo deve obeder o formato de nome definido em arquivo de configuração;
* 03 - O arquivo não pode estar vazio;
* 04 - O arquivo não deve possuir mais do que 70MB;
* 05 - O arquivo não deve possuir a mensagem 'Não há ocorrências para este Registro' em seu conteúdo.

# Whitelist

Conforme informado, a Tool possui uma Whitelist que contém algumas mensagens de erro aceitas como "normal" e que não são impeditivos para a continuidade da execução da tool.

Exemplos:

* "Apólice já cadastrada"
* "Já existe um endosso registrado com o número informado"
* "Identificador Movimento já cadastrado"
* "Sinistro [[0-9]*\] já cadastrado"
* "Identificador do Movimento de Sinistro já cadastrado"

Caso precisemos adicionar novas mensagens na Whitelist, é preciso criar uma varíavel "b3_error_msg6" e adicioná-la na variável b3_error_whitelist.

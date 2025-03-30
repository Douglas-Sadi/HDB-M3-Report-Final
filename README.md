# Hackers Do Bem - Módulo 3 - Atividade 5
⚠️ **O documento em questão destina-se exclusivamente a fins educacionais.** ⚠️

⚠️**Atividades executadas em um ambiente controlado fornecido pela própria instituição de ensino.** ⚠️

ℹ️ ***Informação:*** *Devido a um problema de processamento na ferramenta `peek`, alguns GIFs tiveram parte da execução "cortada".*

**Observação:** Para uma melhor experiência ao visualizar este documento, incluindo a exibição das ações em formato **.gif**, acesse-o em: https://github.com/Douglas-Sadi/HDB-M3-Report-Final

💡 **Dica:** Clique no ▶️ no canto superior direito de cada GIF para iniciar a reprodução. 🎥

## Relatório de Pré-Engajamento - Teste de Penetração

### 1. Objetivo
O objetivo deste teste de penetração é avaliar a segurança dos seguintes ambientes: **intra.net, importante.com e vulneravel.com**, identificando vulnerabilidades nos sistemas e serviços presentes. O escopo do teste inclui:

- Avaliação da infraestrutura;
- Identificação de vulnerabilidades críticas;
- Planejamento de estratégias de mitigação;

### 2. Contexto
O ambiente em questão contém aplicações web e serviços. As principais características incluem:

- Servidores Linux;
- Aplicações Web;
- Serviços como **FTP** ,**SSH** e **HTTP**.

### 3. Tipo de Pentest
Com base na complexidade dos sistemas, o tipo de pentest mais adequado é **caixa cinza**, permitindo testar vulnerabilidades com informações limitadas. As abordagens incluem:

- Testes de intrusão na rede e protocolos;
- Análise de aplicações web;
- Exploração de falhas conhecidas (CVE);
- Testes de configuração em servidores.

### 4. Escopo
#### Elementos incluídos:
- Aplicações web internas;
- Serviços expostos **(HTTP, SSH e FTP).**

#### Elementos excluídos:
- Sistemas de produção críticos;
- Testes de DoS (negação de serviço);

### 5. Comunicação com os Envolvidos
Durante o pentest, serão adotadas as seguintes diretrizes de comunicação:

- Relatórios periódicos enviados à equipe de segurança;
- Reuniões de alinhamento com responsáveis do laboratório;
- Compartilhamento de documentação relevante.

### 6. Plano de Teste de Penetração
#### Metodologia:
1. **Coleta de informações**: levantamento de informações com técnicas de **OSINT**;
2. **Mapeamento da infraestrutura**: identificação de portas abertas e serviços (**Nmap**);
3. **Análise de vulnerabilidades**;
4. **Execução de ataques controlados**: ataques de força bruta, utilizando ferramentas, como: **FFUF**, **Hydra**, **Patator** e **John The Ripper;**
5. **Documentação dos resultados e mitigação**.

### 7. Acordo de Não Divulgação (NDA)
Um **Acordo de Não Divulgação (NDA)** foi assinado entre as partes para garantir a proteção das informações sensíveis obtidas durante o teste de penetração. O documento cobre:

- Confidencialidade das descobertas;
- Restrições de compartilhamento de informações;
- Penalidades em caso de violação do acordo.

### 8. Autorização Formal
Foi obtida uma **autorização formal** do cliente para a realização do teste de penetração. O documento inclui:

- Assinatura dos responsáveis;

- Escopo e limitações do pentest;

- Prazos e entregáveis.


## Entendendo a Necessidade do Cliente

### 1. **Infraestrutura e Sistemas Presentes no Ambiente Alvo**

A varredura realizada com **NMAP** foi executada no ambiente alvo, com base no relatório analisado. Foram identificados os seguintes elementos principais na infraestrutura:

- 🖥️ **Sistemas:** intra.net, importante.com e vulneravel.com  

  🔓 **Portas Abertas e Serviços:** O **NMAP** detectou diversos serviços e funcionalidades em operação nos servidores analisados, como servidores web rodando em portas tradicionais (**por exemplo: 21, 22, 80**, entre outras):


![](gifs/0-Reconhecimento-Geral.gif)

A versão do Apache também foi analisada e, até o momento da elaboração deste relatório, não foram encontrados registros de vulnerabilidades no CVE:

![](gifs/1-reconhecimento-nmap-intra-net.gif)


### 2. **Resultados da Verificação de Segurança**

📊 **Obtivemos os seguintes resultados:**

Através do uso da ferramenta `ffuf`, foi possível realizar a enumeração de usuários presentes no sistema alvo.

A análise dos domínios `intra.net` e `vulneravel.com` resultou em achados semelhantes:

![](gifs/2-ffuf-intra-net.gif)

**Esta etapa permitiu a identificação do usuário** **Paulo** no sistema.

**Ao analisar o diretório** `http://intra.net/~Paulo`, foi possível obter informações sobre suas preferências em filmes e séries, no qual possibilitou a criação de listas de palavras personalizadas para ataques de força bruta, com o intuito de descobrir sua senha:

![](gifs/3-cewl-e-hydra.gif)

Após a execução da atividade acima, foi possível obter a **senha do usuário Paulo.**

Em seguida, foi estabelecida uma conexão FTP com esse usuário, permitindo a obtenção de uma chave SSH privilegiada esquecida no diretório. Utilizando a técnica de `password spraying` combinada com `wordlists `personalizadas e a ferramenta `patator`, **foi possível acessar o sistema com um usuário de privilégios administrativos (*sysadmin*):**

![](gifs/4-wordlist-e-patator.gif)

⚠️ **Falha Grave:** ⚠️

Devido ao esquecimento ou armazenamento inadequado de um arquivo importante, como a chave privada, foi possível realizar a conexão como **sysadmin**. Caso esta falha seja explorada de forma mal-intencionada, pode resultar em **danos irreversíveis** aos sistemas em questão.

_________________________________________________________________________________________________

🎯 **Alvo:** `importante.com`  

🔐 **Execução:** Ataque de força bruta.  

🛠️ **Ferramenta:** `patator`

Este site utiliza o protocolo HTTP em vez de HTTPS, o que permite o tráfego de dados sem criptografia. Isso significa que, caso um atacante **use ferramentas de análise de tráfego, como o `Wireshark` ,`OWASP-ZAP`, `BurpSuite` **,  é possível **interceptar informações sensíveis** do sistema. No entanto, para o alvo em questão, utilizamos uma abordagem diferente:

Recorremos à técnica de **`shoulder surfing`** (espreitar sobre o ombro) para identificar que o nome do usuário é `fred` e observar padrões de digitação no campo de senha.

Com base nos padrões observados, criamos uma lista personalizada utilizando expressões regulares e a combinamos com a ferramenta **`patator`** para realizar um ataque de força bruta, obtendo a senha do mesmo:

![](gifs/5-fred-e-patator.gif)

✅ **Logado com sucesso:**

![](gifs/6-logando-fred.gif)

_________________________________________________________________________________________________

🎯 **Alvo:** `vulneravel.com`  

🔓 **Execução:** Quebra de hashes - senhas fracas.  

🛠️ **Ferramenta:** `John The Ripper`

Devido ao uso de senhas fracas e mecanismos de proteção inadequados, foi possível quebrar facilmente os hashes de senhas armazenadas em texto claro no sistema em questão:

![](gifs/7-jonh-quebrando-hashes.gif)


### 3. **Análise das Principais Áreas de Risco**

Com base no relatório, as áreas mais críticas são:

- **Injeções SQL**: Classificadas como **críticas**, essas vulnerabilidades podem comprometer totalmente a integridade e confidencialidade dos dados.  
- **Falta de Proteções Anti-CSRF**: Embora de risco médio, essa falha pode levar a ações não autorizadas em nome dos usuários, comprometendo a integridade e controle dos sistemas.  
- **Falta de Cabeçalhos de Segurança (CSP e X-Frame-Options)**: A falta dessas proteções compromete tanto a confidencialidade quanto a integridade dos dados e pode ser explorada por atacantes para injetar código malicioso ou realizar ataques de clickjacking.  

### 4. **Impacto Potencial de Cada Vulnerabilidade**

- **Confidencialidade**: A injeção SQL pode expor dados sensíveis do banco de dados, afetando a privacidade e segurança de informações.  
- **Integridade**: Vulnerabilidades como injeções SQL e falta de tokens CSRF podem permitir que atacantes alterem dados sem autorização.  
- **Disponibilidade**: Embora o relatório não mencione diretamente riscos à disponibilidade, injeções e falhas em XSS podem ser usadas para denegar serviços através de manipulação de dados ou sobrecarga de recursos.  

### 5. **Definição do Tipo de Pentest Mais Adequado**

- **Pentest de Aplicações Web**: Devido ao foco nas vulnerabilidades de segurança web (injeção SQL, falhas de CSP, CSRF e clickjacking), recomenda-se um pentest focado em **aplicações web**.  
- **Teste de Intrusão de Rede**: Adicionalmente, considerando a possibilidade de serviços web rodando em múltiplos servidores, um teste de intrusão de rede pode avaliar a segurança do ambiente de infraestrutura.  
- **Recursos e Tempo**: Se os recursos e tempo forem limitados, o foco deve estar nas vulnerabilidades críticas (injeções SQL e falta de proteção CSRF). Para uma análise mais completa, deve-se incluir testes mais amplos de rede e configuração de servidores.  

### 6. **Escopo e Considerações Adicionais**

- **Infraestrutura**: Como o Laboratório de Segurança inclui serviços web, APIs e sistemas interativos, o escopo deve abranger todas as interfaces expostas.  
- **Referências e Melhores Práticas**: O uso de padrões como o **OWASP Testing Guide** e frameworks como o **MITRE ATT&CK** ajudaria a garantir a cobertura de potenciais vetores de ataque.  

## Atividade 2: Definição de Escopo

### Documento de Escopo para o Teste de Penetração no Laboratório de Segurança

#### 1. **Introdução**

Este documento define o escopo do teste de penetração a ser realizado no Laboratório de Segurança, com base nos resultados da varredura de vulnerabilidades conduzida utilizando o OWASP ZAP. O objetivo é garantir que o teste cubra as áreas mais críticas e relevantes, concentrando-se nas vulnerabilidades de alto risco identificadas anteriormente.

#### 2. **Sistemas, Aplicativos e Redes no Escopo**

A seguir, estão os elementos que farão parte do escopo do teste de penetração, considerando sua criticidade e relevância para o objetivo de segurança:

#### 2.1. **Sistemas Web**

##### Sistema 1: Site Web Principal (testphp.vulnweb.com)

| Atributo                        | Detalhes                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| **Versão de Software**          | PHP 5.6.40-38+ubuntu20.04.1                                  |
| **Serviços em Execução**        | Apache 2 com suporte a PHP                                   |
| **Principais Vulnerabilidades** | - Injeção SQL (alta criticidade)   - Falta de cabeçalho Content Security Policy (CSP)   - Falta de proteção Anti-CSRF   - Exposição de informações através do cabeçalho "X-Powered-By" |

##### Sistema 2: API Pública (API AJAX)

| Atributo                        | Detalhes                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| **Versão de Software**          | PHP 5.6.40-38+ubuntu20.04.1                                  |
| **Serviços em Execução**        | API servindo funcionalidades de recuperação de dados (`infoartist.php`, `infocateg.php`) |
| **Principais Vulnerabilidades** | - Injeção SQL (alta criticidade)   - Exposição de informações de versão via cabeçalho "X-Powered-By" |

##### Sistema 3: Serviço de Autenticação (Serviço de Login)

| Atributo                        | Detalhes                                                     |
| ------------------------------- | ------------------------------------------------------------ |
| **Serviços em Execução**        | Interface de login para autenticação de usuários (`newuser.php`, `login.php`) |
| **Principais Vulnerabilidades** | - Injeção SQL no formulário de login   - Falta de tokens Anti-CSRF em formulários |

#### 3. **Exclusões do Escopo**

Os seguintes sistemas e áreas estão **fora do escopo** do teste de penetração, seja por razões de infraestrutura crítica ou por não serem considerados prioritários para este teste:

- **Servidores de Banco de Dados Internos**: O teste de penetração não incluirá ataques diretos aos servidores de banco de dados, apenas será verificada a proteção contra injeções SQL via aplicativos web.  
- **Sistemas de Back-End**: Componentes de back-end que não possuem interfaces web diretamente acessíveis estão fora do escopo.  
- **Servidores de Arquivos ou Storage**: Não será realizada a análise ou exploração de servidores de arquivos no escopo atual.  
- **Dispositivos de Segurança Física (Câmeras, Controles de Acesso)**: Elementos relacionados à segurança física não serão alvo de pentest.  

#### 4. **Áreas Críticas**

As áreas mais críticas identificadas incluem:

- **Injeção SQL** nos sistemas web e APIs, pois podem comprometer a integridade e confidencialidade de dados.  
- **Ausência de tokens Anti-CSRF** em formulários de login e submissão, o que pode expor o sistema a falsificação de solicitações.  
- **Falta de políticas de segurança como CSP e X-Frame-Options**, o que aumenta o risco de ataques XSS e clickjacking.  

#### 5. **Metodologia e Abordagem**

O teste de penetração será conduzido utilizando metodologias baseadas no **OWASP Testing Guide** e no **MITRE ATT&CK**, com foco em ataques comuns a aplicações web e APIs, como:

- Injeções SQL  
- Cross-Site Scripting (XSS)  
- Testes de autenticação e controle de acesso  
- Testes de exposição de informações sensíveis

## Atividade 3: Documentação

### Plano de Teste de Penetração para o Laboratório de Segurança

#### 1. **Objetivos do Pentest**

O teste de penetração visa identificar vulnerabilidades de segurança, explorar falhas presentes nos sistemas e aplicativos do Laboratório de Segurança, e avaliar a resistência do ambiente em relação a possíveis ataques. Os objetivos específicos incluem:

- **Identificar vulnerabilidades** em sistemas web e APIs, com foco em falhas de alto impacto como injeções SQL e falta de proteção CSRF.  
- **Avaliar a configuração de segurança** de servidores e sistemas, garantindo que boas práticas estejam sendo seguidas (ex.: políticas CSP, X-Frame-Options).  
- **Explorar falhas de segurança** para verificar o potencial de comprometimento dos dados e serviços.  
- **Fornecer recomendações** para mitigar as vulnerabilidades encontradas, visando melhorar a segurança dos sistemas.  

#### 2. **Etapas e Metodologias**

O plano de teste de penetração será conduzido utilizando a metodologia **OWASP Testing Guide**, com suporte no framework **MITRE ATT&CK**. 

As etapas incluem:

##### 2.1. **Reconhecimento**

- Identificação de informações sobre o ambiente, como endereços IP, serviços em execução e portas abertas, utilizando ferramentas como Nmap e OWASP ZAP.  

##### 2.2. **Varredura de Vulnerabilidades**

- Utilização do OWASP ZAP para varreduras automáticas e análise de vulnerabilidades em sistemas web e APIs.  
- Verificação manual de vulnerabilidades como injeções SQL, Cross-Site Scripting (XSS), falta de tokens Anti-CSRF, entre outras. 	

##### 2.3. **Exploração**

- Exploração das vulnerabilidades encontradas para verificar a profundidade das falhas, como tentativa de injeção de comandos SQL, manipulação de sessões, e ataques CSRF.  
- Ferramentas: OWASP ZAP, Burp Suite, SQLMap, entre outras.  

##### 2.4. **Relatório de Vulnerabilidades**

- Documentação das vulnerabilidades encontradas, incluindo a descrição detalhada, impacto, métodos de exploração e recomendações de mitigação.  

#### 3. **Modelo de Documentos**

##### 3.1. **Relatório de Teste**

- **Título**: Relatório de Teste de Penetração no Laboratório de Segurança  
- **Data**: (Início e término do teste)  
- **Resumo Executivo**: Breve descrição do teste, objetivos e principais resultados.  
- **Vulnerabilidades Encontradas**: Listagem das vulnerabilidades identificadas, classificadas por risco (crítico, alto, médio, baixo), com recomendações de mitigação.  
- **Impacto Potencial**: Explicação dos possíveis danos que a exploração das vulnerabilidades pode causar, como perda de dados, controle de sistemas ou indisponibilidade de serviços.  
- **Ferramentas Utilizadas**: Listagem das ferramentas (OWASP ZAP, Burp Suite, Nmap) e técnicas aplicadas.  
- **Conclusões e Recomendações**: Sumário das ações necessárias para melhorar a segurança do ambiente.  

##### 3.2. **Registro de Atividades**

- **Data e Hora**: Registro detalhado de todas as ações realizadas, desde o reconhecimento até a exploração.  
- **Ação Executada**: Descrição da etapa realizada (ex.: varredura com OWASP ZAP, exploração de injeção SQL).  
- **Ferramentas Utilizadas**: Indicação da ferramenta ou técnica aplicada em cada atividade.  
- **Resultado Obtido**: Resultado da ação (ex.: vulnerabilidade encontrada, falha explorada, falha corrigida).  

##### 3.3. **Evidências de Vulnerabilidades**

- Capturas de tela dos resultados de testes e explorações.  
- Logs e relatórios gerados por ferramentas, incluindo provas de conceito de vulnerabilidades exploradas.  

#### 4. **Documentação de Vulnerabilidades**

Todas as vulnerabilidades identificadas devem ser documentadas de forma clara e objetiva. Cada vulnerabilidade incluirá:

- **Descrição**: O que é a vulnerabilidade e como foi identificada.  
- **Impacto Potencial**: O que pode ocorrer se a vulnerabilidade for explorada.  
- **Recomendações**: Medidas sugeridas para correção (ex.: uso de prepared statements, implementação de tokens CSRF).  
- **Referências**: Links para guias de melhores práticas, como o OWASP Cheat Sheets.  

#### 5. **Integração dos Resultados da Varredura de Vulnerabilidades**

Os resultados da varredura de vulnerabilidades realizada com o OWASP ZAP, detalhados anteriormente, serão integrados ao plano de teste, com destaque para:

- Injeções SQL  
- Falta de políticas de segurança CSP  
- Ausência de tokens Anti-CSRF  
- Exposição de informações sensíveis em cabeçalhos HTTP  

Cada uma dessas vulnerabilidades será detalhada no relatório, e seu impacto será discutido no contexto dos sistemas do Laboratório de Segurança.

#### 6. **Desafios Enfrentados**

Qualquer obstáculo ou dificuldade encontrada durante o teste será documentado, como:

- Restrições de acesso a determinados sistemas ou servidores.  
- Limitações de tempo ou recursos disponíveis.  
- Detecção de mecanismos de defesa ou mitigação já implementados.  

#### 7. **Contrato Formal**

O contrato formal de pentest incluirá as seguintes cláusulas:

- **Confidencialidade**: Garantir que todas as informações obtidas durante o pentest sejam tratadas de forma confidencial.  
- **Propriedade Intelectual**: Especificar que os resultados e relatórios gerados pertencem ao Laboratório de Segurança.  
- **Limitações de Responsabilidade**: Definir que o teste será realizado de forma controlada e que eventuais falhas exploradas serão informadas ao cliente, sem causar danos irreparáveis aos sistemas.  

## Atividade 4: Linhas de Comunicação com os Envolvidos

### Plano de Comunicação e Colaboração – Teste de Penetração no Laboratório de Segurança

#### 1. **Relatório Resumido de Vulnerabilidades**

O relatório resumido destina-se à equipe técnica e aos responsáveis pelo Laboratório de Segurança e destaca as principais vulnerabilidades identificadas durante a varredura de segurança, incluindo:

### 1.1. Vulnerabilidades Críticas

| **Vulnerabilidade**                              | **Descrição**                                                                                         | **Impacto**                                                                                         | **Recomendação**                                                                           |
|--------------------------------------------------|-------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Injeção SQL (Alta Prioridade)**                | Foi identificada a possibilidade de injeção SQL em vários pontos, permitindo manipulação do banco de dados e exposição de dados sensíveis. | Compromete a integridade, confidencialidade e controle total sobre o banco de dados.              | Implementação imediata de prepared statements e sanitização de entradas de usuários.        |
| **Ausência de Tokens Anti-CSRF (Prioridade Média)** | Formulários sem tokens de verificação CSRF foram detectados, expondo o sistema a ataques que realizam ações em nome do usuário. | Pode permitir a execução de ações não autorizadas.                                                  | Implementar tokens Anti-CSRF e validar nas solicitações.                                  |

### 1.2. Configurações de Segurança Ausentes

| **Vulnerabilidade**                           | **Descrição**                                                                                         | **Impacto**                                                                                         | **Recomendação**                                                                           |
|-----------------------------------------------|-------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------|
| **Falta de Content Security Policy (CSP)**    | Não há política de segurança para controlar scripts e conteúdos externos.                             | Facilita ataques de Cross-Site Scripting (XSS) e a injeção de scripts maliciosos.                   | Configurar uma política CSP restritiva no servidor.                                        |
| **Falta de X-Frame-Options**                  | A ausência deste cabeçalho permite ataques de clickjacking.                                           | Pode permitir que o conteúdo do site seja embutido em outros sites, resultando em ataques de clickjacking. | Incluir cabeçalho X-Frame-Options com a diretiva `SAMEORIGIN` ou `DENY`.                   |

### 1.3. Exposição de Informações

| **Vulnerabilidade**                            | **Descrição**                                                                                         | **Recomendação**                                                                                   |
|------------------------------------------------|-------------------------------------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------|
| **Cabeçalho "X-Powered-By"**                   | O servidor revela informações sobre a tecnologia utilizada (PHP), o que pode ser explorado por invasores. | Remover ou mascarar o cabeçalho "X-Powered-By" para minimizar a exposição de tecnologia.          |


#### 2. **Reunião de Apresentação dos Resultados**

Para apresentar e discutir os resultados da varredura de vulnerabilidades, será agendada uma reunião com a equipe do Laboratório de Segurança e os responsáveis pelo projeto. O objetivo é garantir que todos os envolvidos entendam as vulnerabilidades encontradas, as etapas subsequentes e as recomendações.

##### **Agenda da Reunião**:

- **Introdução**: Resumo do objetivo do pentest e da varredura de vulnerabilidades.  
- **Apresentação dos Resultados**: Discussão dos pontos críticos do relatório, incluindo injeção SQL, falta de tokens Anti-CSRF e cabeçalhos de segurança.  
- **Próximas Etapas**: Definição das fases subsequentes do pentest e prazos.  
- **Discussão Aberta**: Esclarecimento de dúvidas e alinhamento de expectativas.  

#### 3. **Linhas de Comunicação com os Envolvidos**

##### 3.1. **Comunicação Frequente**

Será mantido contato frequente com o (a) **gerente de projetos ou gerente de segurança** responsável, para compartilhar atualizações sobre o progresso do pentest, esclarecer dúvidas e alinhar expectativas sobre prazos, escopo e prioridades.

##### 3.2. **Canal de Comunicação**

Serão estabelecidos canais de comunicação eficientes, como:

- **Reuniões semanais**: Para compartilhar o andamento das atividades e discutir os próximos passos.  
- **Atualizações por e-mail**: Para envio de resumos periódicos com os resultados obtidos e ajustes no escopo.  
- **Plataforma de Gestão de Projetos**: Utilização de uma ferramenta colaborativa para rastreamento das atividades e vulnerabilidades encontradas.  

#### 4. **Contrato Formal e NDA (Non-Disclosure Agreement)**

##### 4.1. **Contrato Formal**

Será elaborada a contribuição para o contrato formal, em conjunto com o departamento legal e o (a) gerente de projetos. O contrato deverá abordar:

- **Escopo do Pentest**: Definir as áreas e sistemas que serão testados, conforme o escopo detalhado na atividade anterior.  
- **Responsabilidades**: Estabelecer as responsabilidades da equipe de pentest e dos envolvidos no Laboratório de Segurança.  
- **Prazos e Entregáveis**: Definir o cronograma das atividades e os relatórios a serem entregues.  
- **Limitações de Responsabilidade**: Especificar as limitações e responsabilidades em caso de falhas exploradas ou incidentes durante o teste.  
- **Propriedade Intelectual**: Garantir que todos os relatórios e resultados gerados durante o pentest sejam de propriedade do cliente.  

##### 4.2. **Acordo de Confidencialidade (NDA)**

A equipe colaborará com a **elaboração do NDA** para garantir a proteção adequada das informações confidenciais. O NDA deverá incluir:

- **Informações Sensíveis e Críticas**: Identificar as informações que devem ser protegidas, como detalhes sobre a infraestrutura do Laboratório de Segurança, relatórios de vulnerabilidades e resultados do pentest. 	
- **Obrigações de Confidencialidade**: Definir que todas as informações obtidas durante o pentest são confidenciais e não podem ser compartilhadas ou divulgadas sem autorização.  
- **Penalidades por Violação**: Incluir cláusulas claras sobre as consequências e penalidades em caso de quebra de confidencialidade.  

#### 5. **Revisão do Contrato e NDA**

Após a elaboração do contrato formal e do NDA, será feita uma revisão minuciosa para garantir que todas as necessidades e requisitos específicos do pentest estejam contemplados, e que as obrigações de confidencialidade sejam claras e compreensíveis.

## Atividade 5: Autorização

### Procedimentos para Obtenção de Autorização Formal para o Teste de Penetração

#### 1. Submissão da Documentação Completa

A documentação completa e detalhada sobre o teste de penetração no Laboratório de Segurança será submetida ao cliente para obter a autorização formal para prosseguir com as atividades. Os documentos enviados incluirão:

- **Plano de Teste de Penetração**: O escopo, objetivos, metodologia e etapas detalhadas do pentest.
- **Relatório Resumido de Vulnerabilidades**: Destacando as principais vulnerabilidades encontradas durante a varredura inicial e as ações recomendadas.
- **Contrato Formal**: Definindo obrigações, responsabilidades, prazos, limitações de responsabilidade e propriedade intelectual.
- **Acordo de Não Divulgação (NDA)**: Garantindo a proteção das informações sensíveis e confidenciais do cliente.

#### 2. Assinatura do NDA (Non-Disclosure Agreement)

Certifique-se de que todas as partes envolvidas assinem o Acordo de Não Divulgação (NDA), garantindo que os dados sensíveis do Laboratório de Segurança sejam protegidos e que não ocorra divulgação não autorizada de informações. A assinatura do NDA é um pré-requisito para o início do pentest.

- **Ações**:
    - Enviar o NDA para assinatura de todas as partes envolvidas (equipe de pentest, cliente, gerente de segurança e departamento legal).
    - Confirmar a assinatura de todas as partes antes de prosseguir com o teste.

#### 3. Requisitos Legais e Regulamentares

Antes de iniciar o pentest, é essencial verificar e garantir que todas as normas legais e regulatórias específicas aplicáveis ao setor e ao ambiente do Laboratório de Segurança sejam seguidas.

- **Passos**:
    - **Análise de conformidade regulatória**: Verificar se há requisitos específicos, como normas de proteção de dados (ex.: LGPD, GDPR) ou padrões de segurança (ex.: PCI-DSS, ISO/IEC 27001).
    - **Adequação do escopo e métodos**: Garantir que o teste de penetração esteja em conformidade com essas regulações e que qualquer atividade potencialmente sensível seja realizada dentro dos limites legais.

#### 4. Revisão e Atualização Regular da Documentação

A documentação deverá ser revisada e atualizada conforme necessário, especialmente em caso de mudanças no escopo, metodologia ou cronograma do pentest. Assegure-se de que todas as alterações sejam refletidas na documentação de forma precisa e estejam alinhadas com os requisitos do cliente.

- **Frequência de Revisão**: A documentação será revisada em cada fase crítica do projeto, e quaisquer mudanças deverão ser formalizadas e comunicadas às partes envolvidas.

#### 5. Obtenção da Autorização Formal

Após a submissão de toda a documentação e assinatura do NDA, o cliente deverá fornecer uma autorização formal para iniciar o pentest.

- **Ação**:
    - Confirmar o recebimento da autorização por escrito (e-mail, assinatura digital ou documento físico).
    - Garantir que todas as condições estabelecidas no contrato e NDA estejam atendidas antes de prosseguir com o teste.

Com a documentação enviada, o NDA assinado e a verificação dos requisitos legais, o próximo passo será obter a autorização formal do cliente. Após a confirmação, o teste de penetração poderá ser conduzido de acordo com o plano previamente definido.




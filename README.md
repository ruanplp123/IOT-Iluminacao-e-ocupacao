# Luz Ambiente Inteligente: Som & Presença

## Entrega formal — Aula 02

**Disciplina:** Internet das Coisas  
**Turma:** 6º semestre — Turma B  
**Professor:** Edson Vaz Lopes  
**Família temática:** Família 3 — Iluminação e ocupação  
**Repositório:** [IOT-Iluminacao-e-ocupacao](https://github.com/ruanplp123/IOT-Iluminacao-e-ocupacao)

## Integrantes

| Integrante | Participação inicial |
|---|---|
| Rodrigo Bonifácio Conceição | Organização do circuito e testes de ligação |
| Ruan Pablo de Lima Pereira | Organização do repositório e documentação |
| Guilherme Pietro Ruiz Costa | Pesquisa e teste do sensor de som |
| Vinícius Clemente Negherbon | Pesquisa e teste do sensor de presença |
| Bianca Barp | Pesquisa dos componentes e desenho da arquitetura |

> As responsabilidades podem ser ajustadas pelo grupo durante o desenvolvimento. A divisão inicial serve para garantir que todas as tarefas do backlog tenham um responsável.

## 1. Ideia do projeto

O projeto propõe uma **iluminação ambiente inteligente que reage ao som e à ocupação do espaço**. Um microfone ou sensor de som identifica a intensidade sonora do ambiente, enquanto um sensor de presença verifica se há alguém no local. Com base nessas informações, o ESP32 controla uma iluminação RGB, alterando o brilho e/ou as cores dos LEDs.

Quando o ambiente estiver ocupado e houver som acima de determinado nível, a iluminação poderá ficar mais intensa ou apresentar uma animação de cores. Quando houver pouca movimentação sonora, a iluminação poderá permanecer suave. Se não houver presença no ambiente, as luzes deverão ser desligadas ou colocadas em modo de baixo consumo.

O projeto será inicialmente desenvolvido para uso pessoal, como uma iluminação de quarto, escritório ou área de estudo. A proposta poderá ser ampliada futuramente para ambientes de lazer, salas de reunião ou automação residencial.

## 2. Problema

Em ambientes de uso pessoal, a iluminação normalmente precisa ser ajustada manualmente, mesmo quando o espaço está vazio ou quando a atividade realizada no local muda. Isso pode causar desperdício de energia e tornar o ambiente menos confortável.

Além disso, uma iluminação fixa não reage ao contexto do ambiente. O projeto pretende investigar como sensores simples podem ser utilizados para criar uma iluminação mais interativa, automática e adequada à presença de pessoas e à intensidade do som.

## 3. Usuário e contexto de uso

O usuário inicial será uma pessoa utilizando um quarto, escritório ou espaço de estudo em sua residência. O sistema deverá detectar a presença do usuário e modificar a iluminação de acordo com o nível de som do ambiente.

O protótipo será utilizado em laboratório e, posteriormente, poderá ser adaptado para uma aplicação pessoal. Nesta primeira etapa, o objetivo é validar o funcionamento do circuito e da lógica de controle, não construir um produto final pronto para instalação residencial.

## 4. Objetivo da N1

Desenvolver e documentar um protótipo de iluminação inteligente utilizando ESP32, sensor de som, sensor de presença e LEDs RGB. O protótipo deverá ser capaz de:

1. Detectar se existe uma pessoa no ambiente por meio de um sensor de presença.
2. Medir ou classificar a intensidade do som no ambiente.
3. Controlar a iluminação RGB de acordo com a presença e o nível sonoro.
4. Desligar ou reduzir a iluminação quando não houver ocupação.
5. Demonstrar, em laboratório, a integração entre sensores, processamento no ESP32 e atuadores luminosos.

## 5. Funcionamento previsto

A lógica inicial do sistema será a seguinte:

| Presença detectada | Intensidade do som | Resposta da iluminação |
|---|---|---|
| Não | Qualquer nível | LEDs desligados ou em modo econômico |
| Sim | Baixa | Luz fraca e estável |
| Sim | Média | Brilho intermediário ou mudança lenta de cor |
| Sim | Alta | Brilho maior ou animação de cores responsiva ao som |

Os limites de som e os efeitos de iluminação serão definidos após os primeiros testes com o sensor. A primeira versão poderá utilizar faixas simples — baixa, média e alta — em vez de uma análise musical complexa.

## 6. Componentes previstos

A lista abaixo é uma previsão inicial e deverá ser confirmada conforme a disponibilidade do laboratório:

| Componente | Função |
|---|---|
| ESP32 | Processar as leituras e controlar a iluminação |
| Sensor de som com saída analógica, preferencialmente MAX9814 ou equivalente | Medir a intensidade aproximada do som |
| Sensor de presença PIR HC-SR501 ou equivalente | Identificar ocupação do ambiente |
| Fita ou anel de LEDs RGB WS2812B | Servir como atuador de iluminação controlável |
| Protoboard | Montar o circuito sem solda |
| Cabos jumper | Realizar as conexões |
| Fonte USB ou alimentação adequada para o ESP32 | Alimentar a placa de controle |
| Fonte externa de 5 V, se necessária | Alimentar a fita ou o anel de LEDs |
| Resistor de aproximadamente 330 Ω | Proteger a linha de dados dos LEDs, caso seja utilizado WS2812B |
| Capacitor eletrolítico de aproximadamente 1000 µF | Ajudar a estabilizar a alimentação dos LEDs, caso necessário |

> A quantidade de LEDs deve ser pequena na primeira montagem, para reduzir o consumo e facilitar os testes. O grupo deverá confirmar a tensão, a corrente e a disponibilidade de cada componente antes da montagem definitiva.

## 7. Arquitetura inicial

```text
[Sons do ambiente]
        |
        v
[Sensor de som] ---- sinal analógico ----\
                                          \\
                                           v
                                      [ESP32]
                                           ^
                                           |
[Presença no ambiente] -> [Sensor PIR] ---/
                                           |
                                           v
                                  [Fita/anel LED RGB]
                                           |
                                           v
                                [Iluminação responsiva]
```

O sensor de som enviará uma leitura ao ESP32, que classificará o nível sonoro. O sensor PIR informará se há presença no ambiente. O ESP32 combinará as duas informações e enviará os comandos para os LEDs RGB.

Na primeira versão, o processamento será local no ESP32. Não será obrigatório utilizar internet, aplicativo ou banco de dados para validar o protótipo da N1. Uma etapa futura poderá incluir monitoramento pela rede Wi-Fi ou uma interface de controle.

## 8. Primeiro risco técnico

O principal risco técnico é a **integração entre o ESP32, o sensor de som e a iluminação RGB**, especialmente a leitura correta do sinal analógico do sensor e a compatibilidade elétrica entre a placa e os LEDs. O grupo ainda possui pouca experiência com ESP32 e pode enfrentar dificuldades para configurar as entradas, interpretar os valores do sensor, alimentar os LEDs e evitar instabilidade no circuito.

Esse risco poderá ser investigado por meio de testes pequenos e verificáveis: primeiro acender um LED, depois ler o sensor de som, em seguida testar o sensor PIR e, por fim, integrar os três elementos. Também será necessário verificar a disponibilidade dos componentes e confirmar se a alimentação escolhida é suficiente para a quantidade de LEDs utilizada.

## 9. Backlog inicial

| Tarefa | Responsável | Status |
|---|---|---|
| Criar e verificar a organização do repositório | Ruan Pablo de Lima Pereira | A fazer |
| Preencher e revisar o README inicial | Ruan Pablo de Lima Pereira | A fazer |
| Testar o ESP32 com um LED simples | Rodrigo Bonifácio Conceição | A fazer |
| Testar o Arduino/ESP32 com o exemplo de semáforo solicitado na atividade | Rodrigo Bonifácio Conceição | A fazer |
| Pesquisar e identificar o sensor de som disponível | Guilherme Pietro Ruiz Costa | A fazer |
| Realizar uma leitura básica do sensor de som no ESP32 | Guilherme Pietro Ruiz Costa | A fazer |
| Pesquisar e testar o sensor de presença PIR | Vinícius Clemente Negherbon | A fazer |
| Listar os componentes disponíveis e os componentes faltantes | Bianca Barp | A fazer |
| Desenhar a arquitetura inicial do sistema | Bianca Barp | A fazer |
| Definir os pinos do ESP32 que serão utilizados | Rodrigo Bonifácio Conceição | A fazer |
| Testar o controle da fita ou anel de LEDs RGB | Ruan Pablo de Lima Pereira | A fazer |
| Integrar sensor de presença, sensor de som e LEDs | Todos os integrantes | A fazer |
| Registrar o resultado do primeiro teste integrado | Todos os integrantes | A fazer |
| Registrar o primeiro risco técnico no repositório | Ruan Pablo de Lima Pereira | A fazer |

## 10. Critérios de validação da primeira versão

A primeira versão será considerada funcional quando o grupo conseguir demonstrar que o ESP32 liga os LEDs, identifica a presença por meio do sensor PIR e altera o comportamento da iluminação conforme a intensidade sonora. Mesmo que a resposta ao som seja inicialmente feita por níveis simples, o circuito deverá apresentar uma reação observável e repetível.

## 11. Dúvidas para o professor

1. A proposta pode utilizar simultaneamente sensor de som e sensor PIR para atender à família “Iluminação e ocupação”?
2. A iluminação RGB pode ser feita com um LED RGB comum ou é preferível utilizar uma fita/anel WS2812B?
3. O projeto precisa obrigatoriamente utilizar comunicação Wi-Fi na primeira entrega da N1?
4. O sensor de som precisa medir o áudio de forma contínua ou uma classificação em níveis baixo, médio e alto é suficiente?
5. Quais componentes estarão disponíveis no laboratório para o grupo?
6. A atividade exige um diagrama elétrico detalhado já nesta primeira entrega ou a arquitetura inicial é suficiente?
7. O teste com semáforo deve ser realizado e documentado no repositório ou apenas acompanhado em laboratório?

## 12. Próximos passos

O grupo deve confirmar a disponibilidade do ESP32, do sensor de som, do sensor PIR e dos LEDs RGB. Depois, deve executar os testes individualmente, registrar os resultados no repositório e atualizar o status das tarefas do backlog. Por fim, o grupo deve revisar este README, adicionar eventuais alterações na proposta e postar o arquivo ou o repositório organizado no Teams.

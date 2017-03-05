# Poka-Yoke Genérico
Programa desenvolvido para controlar sensores/postos em linhas de montagem usando CLP Altus Duo351

##Definição
O objetivo deste programa é tornar, o processo de aplicação e alteração da sequência de montagem de um produto, mais rápida e facilitada pela definição desta sequência através da IHM integrada do CLP, fazendo com que não seja necessária a reprogramação do CLP a cada alteração da sequência ou aplicação de um novo posto. É possível configurar o poka-yoke para uma quantidade variável de sensores, com ou sem parafusadeira e com o número de modelos ajustável conforme a necessidade de cada posto de montagem, utilizando-se do mesmo programa para todas aplicações.

##Informações Técnicas
- Número máximo de modelos: 15 
- Número máximo de sensores: 14 
- Número máximo de parafusadeira: 1 
- Número máximo de progbits: 4 (corresponde ao número de saídas disponíveis para a seleção de programa da parafusadeira, sendo possíveis 16 diferentes seleções). 
- Número máximo de ativos: 3 (corresponde ao número de sensores que podem ser ligados simultaneamente – incluindo com parafusadeira). 
- Número máximo de etapas: 12 (corresponde as etapas da sequência de montagem). 

##Configuração de Entradas e Saídas
A ordem de ligação das Entradas e Saídas (CLP Altus Duo351), segue o seguinte padrão utilizado no programa: 

- Entradas:  
   I0.0 -> TOMADA KAP (feedback)  
   I0.1 -> LED SENSOR 1  
   I0.2 -> LED SENSOR 2  
   I0.3 -> LED SENSOR 3  
   I0.4 -> LED SENSOR 4  
   I0.5 -> LED SENSOR 5  
   I0.6 -> LED SENSOR 6  
   I0.7 -> LED SENSOR 7  
   I1.0 -> LED SENSOR 8  
   I1.1 -> LED SENSOR 9  
   I1.2 -> LED SENSOR 10  
   I1.3 -> LED SENSOR 11  
   I1.4 -> LED SENSOR 12  
   I1.5 -> LED SENSOR 13  
   I1.6 -> LED SENSOR 14  
   I1.7 -> *(nada)*  
   I2.0 -> TORQUE OK PARAFUSADEIRA  
   I2.1 -> TORQUE NOK PARAFUSADEIRA  

- Saídas:
   Q0.0 -> BATENTE *(liberação)*
   Q0.1 -> LED SENSOR 1
   Q0.2 -> LED SENSOR 2
   Q0.3 -> LED SENSOR 3
   Q0.4 -> LED SENSOR 4
   Q0.5 -> LED SENSOR 5
   Q0.6 -> LED SENSOR 6
   Q0.7 -> LED SENSOR 7
   Q1.0 -> LED SENSOR 8
   Q1.1 -> LED SENSOR 9
   Q1.2 -> PROGBIT 4 PARAFUSADEIRA 	|   LED SENSOR 10
   Q1.3 -> PROGBIT 3 PARAFUSADEIRA 	|   LED SENSOR 11
   Q1.4 -> PROGBIT 2 PARAFUSADEIRA 	|   LED SENSOR 12
   Q1.5 -> PROGBIT 1 PARAFUSADEIRA 	|   LED SENSOR 13
   Q1.6 -> RESET PARAFUSADEIRA 		|   LED SENSOR 14
   Q1.7 -> ENABLE PARAFUSADEIRA

##Primeira Implementação
Com o programa descarregado no CLP, a primeira tela apresentada será a tela de Configuração dos Parâmetros, onde é determinado o número de sensores, parafusadeira, progbits e modelos.
![Screen ParametersConfig](../assets/img6.jpg)
Para alterar os valores, alterne entre as teclas SETA\_ESQUERDA e SETA\_DIREITA, então pressione ENTER, apegue o valor anterior com a SETA_ESQUERDA e digite o novo valor no teclado numérico. Para salvar e sair pressione ENTER novamente, caso não queira salvar, pressione ESC.  
Com os parâmetros configurados, pressione tecla MAIN ou F7 para sair.  
A próxima tela é a tela principal, a sequência de montagem estará vazia, por isso é exibida a mensagem “CADASTRAR MODELO”. Para isso, veja o tópico de Alteração da Sequência de Montagem.

Vide tópico de Parametrização para maiores detalhes desta tela e alterações futuras.

##Tela Principal *(operação)*
A tela de operação do montador apresenta os modelos, botões de controle (zerar, reset, setup) e os sensores ativos no momento.
![Screen Main](../assets/img3.jpg)
A sessão de modelos se ajustará conforme o número de modelos configurado na tela de parâmetros, caso esse número seja maior que 4, aparecerá um botão na tecla F7 para acessar os outros modelos.  
Para trocar o modelo basta pressionar as teclas [F4-F7] referentes ao modelo apresentado na tela. Ao selecioná-lo, este ficará em destaque e o ciclo de montagem inicia do zero, conforme a sequência determinada na tela SETUP.  
Os sensores ativos são mostrados na tela como S + o número do sensor, e quando há parafusadeira, é indicado como P + o programa de seleção. Conforme o montador vai pegando as peças o display vai descontando os sensores e/ou parafusadeira até completar o ciclo. Ao completar, o CLP libera o batente e reinicia o ciclo.  
O ZERAR (tecla F1) serve para iniciar o ciclo do zero novamente. Pode ser usado no caso de peça errada ou em qualquer etapa da montagem.  
O RESET (tecla F2) serve para no caso de o montador pegar uma peça errada, então o display exibe a mensagem “PEÇA ERRADA” e aguarda o sinal de Reset para continuar o ciclo de onde houve a interrupção.  

##Alteração de Parâmetros e Sequência de Montagem
Para entrar na tela de configurações, selecione o modelo a ser configurado e pressione a tecla F3 (indica entrada para SETUP) a partir da tela principal.  
Se o sistema de segurança estiver ativo aparecerá a tela SENHA, logo, para prosseguir é necessário colocar a senha no campo password. Essa é determinada conforme a Chave gerada aleatoriamente e apresentada na tela.  
Para descobrir a senha, basta multiplicar o número pela letra, convertendo-a do sistema HEXA para DECIMAL.  
Exemplo: Chave: 3A – senha: 3 X 10 = 30. Pois A = 10.  
![Screen Password](../assets/img4.jpg)
Para escrever a senha, entre no campo password alternando entre as teclas SETA\_ESQUERDA e SETA_DIREITA, digite o valor no teclado numérico e pressione ENTER. Se a senha estiver correta, irá para a tela SETUP que queremos.

##Sequência de Montagem
A estrutura da sequência de montagem está dividida em etapas, na qual corresponde às etapas de montagem do produto. Cada etapa pode monitorar até 3 ativos, ou seja, é possível ligar até 3 sensores, ou 2 sensores e 1 parafusadeira por etapa.  
Os ativos são indicados pelos campos I, II e III. O campo P destina-se a seleção do programa da parafusadeira e é liberado quando é colocada uma parafusadeira na etapa. O número limite de programas da parafusadeira é determinado conforme o número de progbits configurado na tela de Parâmetros.  
O código dos sensores é o seu próprio número, já para a parafusadeira o código é 15.	Por exemplo, para ligar o sensor 2 e a parafusadeira com programa 5 na etapa 1, a sequência fica a seguinte:  
![Screen Setup](..assets/img5.jpg)
Para alterar os valores, alterne entre as teclas SETA\_ESQUERDA e SETA_DIREITA, digite o número do sensor no teclado numérico e pressione ENTER. Não é possível colocar sensores repetidos na mesma etapa, nem um sensor maior do que o número de sensores configurado.  
Para passar para a próxima etapa, basta pressionar a tecla SETA\_PARA\_BAIXO e para voltar a etapa, pressionar a tecla SETA\_PARA_CIMA. Limite: 12 etapas.

Configurado a sequência, pressione a tecla MAIN ou F7 para sair. O programa salvará as alterações e já iniciará o ciclo com a nova sequência.

##Parametrização
Para entrar na tela de parâmetros, é necessário que se esteja na tela SETUP, conforme início do capítulo. Estando nela, pressione a tecla [- +/.].  
Aqui é possível configurar o número de sensores, parafusadeira, progbits e modelos. Para alterá-los, alterne entre as teclas SETA\_ESQUERDA e SETA_DIREITA, digite o novo valor no teclado numérico e pressione ENTER.
![Screen ParametersConfig2](../assets/img1.jpg)
Após feito as alterações, pressione a tecla MAIN ou F7 para sair.  
O programa irá salvar e validar a sequência de montagem com os novos parâmetros. Caso seja alterado o número de sensores para um valor menor, a sequência de montagem deve ser revisada. Igualmente para no caso de ser tirado a parafusadeira. O programa pedirá para “RECONFIGURAR MATRIZ” em ambos os casos.

OBS: O programa altera o limite de sensores e parafusadeira conforme a disponibilidade das saídas, logo para colocar 14 sensores é necessário que a parafusadeira esteja em zero. Para colocar uma parafusadeira, é possível com no máximo 12 sensores, logo, terá só um progbit disponível. Para aumentar o progbits, diminua o número de sensores. Nove sensores ou menos liberam 4 progbits.  

##Edição do Nome dos Modelos
O programa original nomeia os modelos como 1000 + número do modelo. *[1001-1015]*. Para alterar o nome para outro número, pressione a tecla ZERO [0] na tela principal. O programa entrará em Modo Edição, assim basta alternar entre as teclas SETA\_ESQUERDA e SETA_DIREITA para selecionar o modelo a ser alterado, digitar o novo nome no teclado numérico e pressionar ENTER.
![Screen ModelsEdition](../assets/img7.jpg)
Feito a alteração, pressione tecla MAIN ou ESC para sair. O programa salvará e voltará a rodar o ciclo.  
É necessário sair e entrar no Modo Edição para cada sessão de modelos.  
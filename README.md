# CP1 - O Caso da Vinheria Agnello: Luminosidade

# Descrição do projeto
fomos contratados pela Vinheria Agnello com a finalidade de ajudá-los a gerenciar a luminosidade no local onde armazenam os vinhos. A empresa nos falou  que a iluminação na adega é vital para manter a qualidade do vinho. Isso acontece, pois quanto mais tempo a bebida é exposta a luz, mais a integridade do vinho é comprometida, especialmente, os vinhos brancos e espumantes que são mais vulneráveis a luz.
Por essas razões, propomos esse projeto, no qual um sensor captura os índices de luminosidade da adega , além de indicar se o ambiente está bom, em níveis de alerta ou com algum problema. Além disso, se a luminosidade do ambiente estiver em um nível crítico, soará um alarme por 3 segundos. Se a situação persistir, o alarme tocará novamente.
# Componentes necessários
Arduino Uno R3 - 1; 220 Ω Resistor - 3, 10 kΩ Resistor - 1; LED verde	- 1;
LED amarelo	- 1; LED vermelho	- 1; Buzzer (Piezo) -	1; LDR -	1; Cabo	- 14
# Montagem
![image](https://github.com/Guig3003/chekpoint-edge/assets/92872071/ca9e9692-f134-48c5-9938-892be690a0d7)

# Video e Link do Projeto
https://youtu.be/R9jP0alYNWY?si=B2fCeV9S4Yj8fWcN
https://www.tinkercad.com/things/jD2H0pZm24E-chekpoint-edge
# código do projeto
int buzzerPin = 10;//Alarme ligado ao pino 10
int gPin = 7;//Led verde no pino 7
int yPin = 4;//Led amarela no pino 4
int rPin = 2;//Led vermelha no pino 2
int LDR = A0; //LDR ligado ao pino A0

int mediaMovel[10]; // Variável para a média móvel
void setup(){
  
//Define os pinos como saída  
  pinMode(buzzerPin,OUTPUT);
  pinMode(gPin, OUTPUT);
  pinMode(yPin, OUTPUT);
  pinMode(rPin, OUTPUT);
//Define os pinos de saída
  pinMode(LDR, INPUT);
//Inicia o Serial do Arduino
  Serial.begin(9600);
  
}
void loop(){
  
  int LDR_in = analogRead(LDR);  // Lê o valor do LDR

  // Atualiza a média móvel
  for (int i = 9; i > 0; i--) {
    mediaMovel[i] = mediaMovel[i - 1];
  }
  //Utilizando a função map() para uma maior precisão
  mediaMovel[0] = map(LDR_in, 38, 1010, 0, 100);

  //Calculando a média de luminosidade
  int media = 0;
  for (int i = 0; i < 10; i++) {
    media += mediaMovel[i];
  }
  media /= 10;

  Serial.println(media); // Mostra esse valor no Monitor Serial
  delay(300);
//Se a média do LDR for maior ou igual que 40
  if(media >=75){
    digitalWrite(rPin, HIGH);//Led vermelho liga
    digitalWrite(yPin, LOW);//Led amarelo desliga
    digitalWrite(gPin, LOW);//Led verde desliga
    tone(buzzerPin, 1500);//Define o tom do alarme
    delay(3000);//Tempo para o alarme tocar de 3 segundos
    noTone(buzzerPin);
    
//Se a média do LDR for maior ou igual que 40 e menor que 75
  }else if(media >= 40 && media < 75){
    digitalWrite(yPin, HIGH);//Led amarela liga
    digitalWrite(rPin, LOW);//Led vermelho desliga
    digitalWrite(gPin, LOW);//Led verde desliga
    
//Se a média do LDR for menor que 40
  }else{
    digitalWrite(gPin, HIGH);//Led verde liga
    digitalWrite(yPin, LOW);//Led amarelo desliga
    digitalWrite(rPin, LOW);//Led vermelho delisga
  }
  # 
}



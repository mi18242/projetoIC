//Car8
int x = 0;
#define df 5 //motor esquerdo-frente
#define ef 6 //motor direito-frente
#define dt 4 //motor esquerdo-tras
#define et 7 //motor direito-tras
#define vd 3 //velocidade-esquerdo
#define ve 9 //velocidade-direito
#define ultraout 8 //ultrassonico echo (out)
#define ultrain 10 //ultrassonico trig (in)
#define infra 2 //sensor infravermelho

void parar(){
  digitalWrite(ve, LOW);
  digitalWrite(vd, LOW);
  digitalWrite(ef, LOW);
  digitalWrite(df, LOW);
  digitalWrite(et, LOW);
  digitalWrite(dt, LOW);
}

void andarfrente(){
  digitalWrite(ve, HIGH);
  digitalWrite(vd, HIGH);
  digitalWrite(ef, HIGH);
  digitalWrite(df, HIGH);
}

void andarfrentelento(){
  analogWrite(ve, 100);
  analogWrite(vd, 100);
  digitalWrite(ef, HIGH);
  digitalWrite(df, HIGH);
}

void andartras(){
  digitalWrite(ve, HIGH);
  digitalWrite(vd, HIGH);
  digitalWrite(et, HIGH);
  digitalWrite(dt, HIGH);
}

void girodireita(){
  digitalWrite(ve, HIGH);
  digitalWrite(ef, HIGH);
}

void giroesquerda(){
  digitalWrite(vd, HIGH);
  digitalWrite(df, HIGH);
}

void giroeixohorario(){
  analogWrite(ve, 70);
  analogWrite(vd, 70);
  digitalWrite(ef, HIGH);
  digitalWrite(dt, HIGH);
}

void giroeixoantihorario(){
  analogWrite(ve, 70);
  analogWrite(vd, 70);
  digitalWrite(et, HIGH);
  digitalWrite(df, HIGH);
}

void curvalevedireita(){
  analogWrite(ve, 100);
  analogWrite(vd, 80);
  digitalWrite(ef, HIGH);
  digitalWrite(df, HIGH);
}

void curvaleveesquerda(){
  analogWrite(vd, 100);
  analogWrite(ve, 80);
  digitalWrite(df, HIGH);
  digitalWrite(ef, HIGH);
}

void andarpreto(){
  if(digitalRead(infra) == 0){
    digitalWrite(ve, HIGH);
    digitalWrite(vd, HIGH);
    digitalWrite(ef, HIGH);
    digitalWrite(df, HIGH);
    }
  else{
    parar();
  }
  Serial.println(digitalRead(infra));
}

void andarbranco(){
  if(digitalRead(infra) == 1){
    digitalWrite(ve, HIGH);
    digitalWrite(vd, HIGH);
    digitalWrite(ef, HIGH);
    digitalWrite(df, HIGH);
    }
  else{
    parar();
  }
  Serial.println(digitalRead(infra));
}

void andardist(){
  long duracao;
  float distancia;
  digitalWrite(ultraout, LOW);
  delayMicroseconds(2);
  digitalWrite(ultraout, HIGH);
  delayMicroseconds(10);
  digitalWrite(ultraout, LOW);

 duracao = pulseIn(ultrain, HIGH, 30000); 

  if (duracao > 0){
      distancia = duracao*0.034/2;
      }else{
          distancia = 999;
      }

  Serial.println(distancia); //printar pra teste 

  if (distancia < 20){
    parar();
  }else{
      andarfrente();
  }
  delay(100);
}

void andargrandedist(){
  long duracao;
  float distancia;
  digitalWrite(ultraout, LOW);
  delayMicroseconds(2);
  digitalWrite(ultraout, HIGH);
  delayMicroseconds(10);
  digitalWrite(ultraout, LOW);

 duracao = pulseIn(ultrain, HIGH, 30000); 

  if (duracao > 0){
      distancia = duracao*0.034/2;
      }else{
          distancia = 999;
      }

  Serial.println(distancia); //printar pra teste 

  if (distancia > 20){
    parar();
  }else{
      andarfrente();
  }
  delay(100);
}

void seguirlinha() {
  float tempo = 20;
  static int contador_linha = 0
  bool linha_reencontrada = false
  static bool ultimo_movimento_direita = true; // Descobrir para qual lado da linha preta o sensor saiu

  // Verificar se o sensor está na linha
  if (digitalRead(infra) == 0) {
    // Está na linha preta
    contador_linha++;
    Serial.println(contador_linha);
    andarfrentelento();
  } else {
    // Saiu da linha, precisa descobrir para qual lado ir
    parar();

    // Tentar encontrar a linha girando na direção oposta 
    if (ultimo_movimento_direita) {
      giroeixoantihorario();
    } else {
      giroeixohorario();
    }

    for (int i = 0; i < 20; i++) {
      delay(tempo);
      if (digitalRead(infra) == 0) {
        linha_reencontrada = true;
        break;
      }
    }
    parar();

    if (!linha_reencontrada) {  //aqui, ele vai tentar girar para outro lado, se nao encontrou a linha 
      // Tentar encontrar a linha girando na outra direção
      if (ultimo_movimento_direita) {
        giroeixohorario();
      } else {
        giroeixoantihorario();
      }

      for (int i = 0; i < 40; i++) {
        delay(tempo);
        if (digitalRead(infra) == 0) {
          linha_reencontrada = true;
          ultimo_movimento_direita = !ultimo_movimento_direita; // Se ele encontrou a linha nessa parte, em que ele está girando para o outro lado, então o lado do último movimento estava errado
          break;
        }
      }
      parar();
    }

    if (!linha_reencontrada) { //Caso não tenha encontrado em nenhum dos lados que procurou, ele vai fazer uma sequência de movimentos "horário, anti-horário e andar em frente" até achar a linha
      while (digitalRead(infra) != 0) {
        giroeixoantihorario();
        delay(tempo);
        parar();
        if(digitalRead(infra) != 0) {
          giroeixohorario();
          delay(tempo);
          parar();
          if(digitalRead(infra) != 0) {
            andarfrentelento();
            delay(tempo);
            parar();}}
      }
    }

    // Atualizar o último movimento baseado em qual direção a linha foi reencontrada
    ultimo_movimento_direita = !ultimo_movimento_direita;
  }
  Serial.println(digitalRead(infra));
}

void setup() {
  pinMode(ve, OUTPUT);
  pinMode(vd, OUTPUT);
  pinMode(ef, OUTPUT);
  pinMode(df, OUTPUT);
  pinMode(et, OUTPUT);
  pinMode(dt, OUTPUT);
  pinMode(infra, INPUT);
  pinMode(ultraout, OUTPUT);
  pinMode(ultrain, INPUT);
  Serial.begin(9600);
}

void loop() {
  seguirlinha();
}

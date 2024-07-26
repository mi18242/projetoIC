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
int estado_roda_a = 20;
int estado_roda_b = 80;

void andarfrentelentocerto(){
  analogWrite(ve, estado_roda_a);
  analogWrite(vd, estado_roda_b);
  digitalWrite(ef, HIGH);
  digitalWrite(df, HIGH);
}

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
  } else {
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
  } else {
    parar();
  }
  Serial.println(digitalRead(infra));
}

void desviarObstaculo() {
  parar();
  delay(500);

  // Recuar
  andartras();
  delay(500);
  parar();
  delay(500);

  // Virar à direita
  girodireita();
  delay(500);
  parar();
  delay(500);

  // Ir para frente
  andarfrente();
  delay(1000);
  parar();
  delay(500);

  // Virar à esquerda para voltar à linha
  giroesquerda();
  delay(500);
  parar();
  delay(500);
}

void andardist() {
  long duracao;
  float distancia;
  digitalWrite(ultraout, LOW);
  delayMicroseconds(2);
  digitalWrite(ultraout, HIGH);
  delayMicroseconds(10);
  digitalWrite(ultraout, LOW);

  duracao = pulseIn(ultrain, HIGH, 30000); //talvez seja necessario modificar esse tempo para funcionar

  if (duracao > 0){
    distancia = duracao * 0.034 / 2;
  } else {
    distancia = 999;
  }

  Serial.println(distancia);

  if (distancia < 20){ //talvez seja necessário diminuir esse valor ou aumentar o delay
    desviarObstaculo();
  } else {
    andarfrente();
  }
  delay(100);
}

void seguirlinha() {
  int temp = estado_roda_a;
  estado_roda_a = estado_roda_b;
  estado_roda_b = temp;
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
  attachInterrupt(digitalPinToInterrupt(infra), seguirlinha, RISING);
}

void loop() {
  andarfrentelentocerto();
  andardist(); // Ver a distância do obstáculo
}

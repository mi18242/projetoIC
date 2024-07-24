void verificarLinha() {
  static int contador_linha = 0;
  static bool ultimo_movimento_direita = true;

  if (digitalRead(infra) == LOW) {
    contador_linha++;
    Serial.println(contador_linha);
    andarFrenteLento();
  } else {
    parar();
    bool linha_reencontrada = false;

    if (ultimo_movimento_direita) {
      giroEixoAntiHorario();
    } else {
      giroEixoHorario();
    }

    delay(500);

    if (digitalRead(infra) == LOW) {
      linha_reencontrada = true;
    } else {
      if (ultimo_movimento_direita) {
        giroEixoHorario();
      } else {
        giroEixoAntiHorario();
      }

      delay(1000);

      if (digitalRead(infra) == LOW) {
        linha_reencontrada = true;
      }
    }

    if (linha_reencontrada) {
      andarFrenteLento();
    } else {
      parar();
      while (digitalRead(infra) != LOW) {
        giroEixoAntiHorario();
        delay(20);
        parar();
      }
      andarFrenteLento();
    }

    ultimo_movimento_direita = !ultimo_movimento_direita;
  }
}

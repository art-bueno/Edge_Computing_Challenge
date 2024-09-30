### README do Projeto: Monitoramento de Temperatura e Umidade com ESP32 e DHT22

---

# Projeto ESP32 com Sensor DHT22 no Wokwi

Este repositório contém um projeto de IoT utilizando a placa ESP32 e o sensor DHT22 para monitoramento de temperatura e umidade. A simulação foi realizada na plataforma Wokwi, que permite testar projetos de eletrônica e IoT sem a necessidade de hardware físico.

## 🛠 Tecnologias Utilizadas

- **ESP32**: Placa de microcontrolador com conectividade Wi-Fi.
- **Sensor DHT22**: Sensor de temperatura e umidade.
- **Wokwi**: Plataforma de simulação de eletrônica e IoT.
- **Linguagem C++**: Código implementado utilizando a IDE do Arduino.

## 📋 Funcionalidades

1. **Leitura de Temperatura e Umidade**: O sensor DHT22 coleta dados do ambiente e envia essas informações para a ESP32.
2. **Servidor Web**: A ESP32 atua como um servidor web, onde os dados de temperatura e umidade podem ser acessados em qualquer navegador conectado à mesma rede Wi-Fi.
3. **Interface HTML Dinâmica**: A página HTML atualiza automaticamente as leituras a cada 4 segundos.

## 🚀 Como rodar o projeto

### Pré-requisitos
Você precisará do Arduino IDE e das bibliotecas abaixo:
- Biblioteca **DHT** para comunicação com o sensor.
- Biblioteca **WiFi** para conectar a ESP32 à rede.
- Biblioteca **ESPmDNS** para utilizar um nome amigável de rede (opcional).


## 📄 Código Completo

Aqui está o código principal utilizado no projeto:

#include <WiFi.h>
#include <WiFiClient.h>
#include <WebServer.h>
#include <ESPmDNS.h>
#include <DHT.h>

// Definição das constantes para conectar a internet.
const char *ssid = "My-Network";
const char *password = "chaker123456";

// Conecção como o web server http e com o sensor dht
WebServer server(80);
DHT dht(26, DHT11);


// Declaração da quantidade de bytes e construção da página a ser exibida no HTML.
void handleRoot() {
  char msg[1500];

  snprintf(msg, 1500,
"<html>\
  <head>\
    <meta http-equiv='refresh' content='4'/>\
    <meta name='viewport' content='width=device-width, initial-scale=1'>\
    <link rel='stylesheet' href='https://use.fontawesome.com/releases/v5.7.2/css/all.css' integrity='sha384-fnmOCqbTlWIlj8LyTjo7mOUStjsKC4pOpQbqyi7RrhN7udi9RwhKkMHpvLbHG9Sr' crossorigin='anonymous'>\
    <title>Medida de temperatura e umidade do motor</title>\
    <style>\
    html { font-family: Arial; display: inline-block; margin: 0px auto; text-align: center;}\
    h2 { font-size: 3.0rem; }\
    p { font-size: 3.0rem; }\
    .units { font-size: 1.2rem; }\
    .dht-labels{ font-size: 1.5rem; vertical-align:middle; padding-bottom: 15px;}\
    </style>\
  </head>\
  <body>\
      <h2>Temperatura e umidade do motor</h2>\
      <p>\
        <i class='fas fa-thermometer-half' style='color:#ca3517;'></i>\
        <span class='dht-labels'>Temperatura</span>\
        <span>%.2f</span>\
        <sup class='units'>&deg;C</sup>\
      </p>\
      <p>\
        <i class='fas fa-tint' style='color:#00add6;'></i>\
        <span class='dht-labels'>Umidade</span>\
        <span>%.2f</span>\
        <sup class='units'>&percnt;</sup>\
      </p>\
  </body>\
</html>",
           readDHTTemperature(), readDHTHumidity()
          );
  server.send(200, "text/html", msg);
}

//Prints no monitor Serial, mostrando a conecção e solicitando o endereço do IP
void setup(void) {

  Serial.begin(115200);
  dht.begin();
  
  WiFi.mode(WIFI_STA);
  WiFi.begin(ssid, password);
  Serial.println("");

  //
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }

  Serial.println("");
  Serial.print("Conectado a(o) ");
  Serial.println(ssid);
  Serial.print("Endereço do IP: ");
  Serial.println(WiFi.localIP());

// Se não aparecer o endereço do IP, podemos colocar "esp32.local" na barra de pesquisa da web que o resultado aparecerá
  if (MDNS.begin("esp32")) {
    Serial.println("MDNS responder started");
  }
  server.on("/", handleRoot);

  server.begin();
  Serial.println("HTTP server started");
}

void loop(void) {
  server.handleClient();
  delay(2);
}


float readDHTTemperature() {
  // Fazendo com que o sensor leia a temperatura em °C
  float t = dht.readTemperature();
  if (isnan(t)) {    
    Serial.println("Falha ao ler o sensor DHT");
    return -1;
  }
  else {
    Serial.println(t);
    return t;
  }
}

float readDHTHumidity() {
  float h = dht.readHumidity();
  if (isnan(h)) {
    Serial.println("Falha ao ler o sensor DHT");
    return -1;
  }
  else {
    Serial.println(h);
    return h;
  }
}

## 🎥 Explicação Detalhada do Projeto

Neste projeto, abordamos o conceito de **Internet das Coisas (IoT)**, que permite a conexão de dispositivos físicos à internet, proporcionando controle remoto e coleta de dados. O **ESP32**, com seu Wi-Fi integrado, se conecta à rede e atua como servidor web, onde podemos visualizar os dados lidos pelo **sensor DHT22**. O DHT22 é responsável pela medição de **temperatura e umidade**, dados esses exibidos em uma página HTML gerada automaticamente.

Além disso, o código inclui uma função para atualizar a página web a cada 4 segundos, garantindo que as leituras estejam sempre atualizadas. O projeto foi completamente simulado na plataforma **Wokwi**, facilitando a visualização e testes antes de construir o hardware real.

---

### Recursos Necessários para Implementar a Solução

Para implementar o projeto de monitoramento de temperatura e umidade utilizando IoT com ESP32 e DHT22, os seguintes recursos são necessários:

#### 1. **Hardware**
- **ESP32**: Placa de microcontrolador com conectividade Wi-Fi integrada, essencial para conectar o projeto à internet e fornecer controle remoto dos sensores.
- **DHT22**: Sensor para medir temperatura e umidade ambiente, fácil de integrar com a ESP32.
- **Protoboard** e **fios jumper**: Para facilitar a montagem do circuito.
- **Cabo USB**: Para conectar a ESP32 ao computador e realizar o upload do código.

#### 2. **Software**
- **Arduino IDE**: Ambiente de desenvolvimento para programar e fazer o upload do código na ESP32.
- **Bibliotecas Necessárias**:
  - **DHT22**: Para comunicação com o sensor de temperatura e umidade.
  - **WiFi**: Para conectar a ESP32 à rede Wi-Fi.
  - **ESPmDNS**: Para fornecer um nome de domínio local amigável (opcional).
- **Wokwi**: Plataforma de simulação online que permite desenvolver e testar o projeto sem precisar do hardware físico.

**LINKS DE ACESSO**
**Youtube** - https://youtu.be/4qLmnWOzF-g
**Wokwi** - https://wokwi.com/projects/410117007368318977


**Integrantes do Projeto**:
- **Arthur Bueno de Oliveira** - RM558396
- **Diego Eleuterio Fadul da Costa** - RM557218
- **João Vitor Carotta Ribeiro** - RM555187
- **Victor Magdaleno Marcos** - RM556729

---

## 📃 Licença

Este projeto está sob a licença MIT. Para mais detalhes, consulte o arquivo `LICENSE`.

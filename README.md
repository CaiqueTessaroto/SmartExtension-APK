# SmartExtension 📱🔌

> **Controle Inteligente de Extensões IoT via MQTT e HTTP**

O **SmartExtension** é um aplicativo Android nativo desenvolvido para transformar uma extensão elétrica comum em um dispositivo inteligente (IoT). O aplicativo permite o controle remoto de até 4 tomadas (relés) utilizando o protocolo **MQTT** para comunicação via internet, além de possuir um modo offline (local) via **HTTP/WebView** e autenticação segura com **Firebase**.

## ✨ Funcionalidades

* **Controle Remoto (MQTT):** Acione suas tomadas de qualquer lugar do mundo com baixa latência utilizando o protocolo MQTT.
* **Modo Offline (Fallback Local):** Se a internet cair, o app detecta e muda automaticamente para controle local via WebView (conectando diretamente ao IP do microcontrolador, ex: `192.168.4.1`).
* **Controle de 4 Canais:** Interface dedicada para controlar 4 dispositivos independentes.
* **Feedback de Status:** O ícone e o texto indicam se o dispositivo está ligado, desligado ou sem resposta.
* **Personalização:**
* **Renomear Botões:** Dê nomes personalizados para cada tomada (ex: "Abajur", "TV", "Ventilador").
* **Temas:** Altere a aparência do app (Azul, Vermelho, Verde, Amarelo ou Imagem da Galeria).


* **Autenticação:** Sistema de login integrado com Firebase.
* **Persistência de Dados:** Salva configurações de MQTT (Broker, Tópicos, Senhas) e preferências do usuário localmente.

## 🛠️ Tecnologias Utilizadas

* **Linguagem:** Java (Android Nativo).
* **Protocolo de Mensageria:** [Eclipse Paho MQTT Client](https://www.eclipse.org/paho/index.php?page=clients/java/index.php).
* **Backend / Autenticação:** Firebase Auth.
* **Armazenamento Local:** SharedPreferences.
* **Interface Web (Local):** Android WebView.

## 🔌 Hardware Compatível

Este aplicativo foi projetado para funcionar com microcontroladores baseados em ESP (Espressif), como:

* **ESP8266 (NodeMCU, Wemos D1)** ou **ESP32**.
* Módulo de Relé de 4 canais.

### Funcionamento do Firmware (Hardware)

Para que o aplicativo funcione corretamente, o código no seu ESP deve:

1. Se inscrever nos tópicos MQTT configurados no app.
2. Escutar comandos "ligado" e "desligado".
3. Criar um Access Point (SoftAP) com IP `192.168.4.1` para o modo offline e responder a requisições GET (ex: `/get?inputX=...`).

## 🚀 Como Configurar

### 1. Pré-requisitos

* Android Studio instalado.
* Conta no Firebase (para o arquivo `google-services.json`).
* Um Broker MQTT (pode ser público como `broker.hivemq.com` ou privado).

### 2. Instalação

1. Clone este repositório:
```bash
git clone https://github.com/seu-usuario/SmartExtension.git

```


2. Abra o projeto no Android Studio.
3. Adicione o arquivo `google-services.json` (do seu console Firebase) na pasta `app/`.
4. Sincronize o Gradle e execute o app no seu dispositivo.

### 3. Configuração no App

Ao abrir o aplicativo, acesse a tela de configurações (ícone de engrenagem) e preencha:

* **Host MQTT:** Endereço do seu broker (ex: `tcp://broker.hivemq.com:1883`).
* **Tópicos:** Defina os tópicos para cada relé (ex: `casa/quarto/tomada1`).
* **Autenticação (Opcional):** Usuário e senha do broker MQTT, se for privado.

## 🧩 Estrutura do Código (Snippet)

O trecho principal de controle MQTT utiliza a biblioteca Paho:

```java
// Exemplo de publicação de mensagem MQTT
String payload1 = "ligado";
byte[] encodedPayload1 = payload1.getBytes("UTF-8");
MqttMessage message1 = new MqttMessage(encodedPayload1);
client.publish(topico1, message1);

```

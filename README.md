# Sistema de Monitoramento com Raspberry Pi e MQTT

Projeto de monitoramento ambiental utilizando **Raspberry Pi** e múltiplos sensores para coleta de dados em tempo real.  
Os dados são publicados via **MQTT**, permitindo integração com dashboards, automações e sistemas de alerta.

## 📌 Descrição

Este sistema realiza a leitura de variáveis ambientais e de qualidade do ar/ambiente, combinando os seguintes sensores:

- **MQ-135**: detecção de gases e estimativa de qualidade do ar
- **DHT11**: temperatura e umidade
- **BMP180**: pressão atmosférica (e temperatura auxiliar)
- **LDR**: intensidade de luz ambiente
- **MAX4466**: nível de ruído (sinal analógico de áudio)

Após a leitura, os valores são processados pelo Raspberry Pi e enviados para um broker MQTT em tópicos específicos.

---

## 🧠 Como o sistema funciona

1. O Raspberry Pi inicializa os sensores e interfaces (GPIO/I2C/ADC).
2. O script principal executa leituras periódicas.
3. Os dados são organizados em payload (JSON, por exemplo).
4. O Raspberry Pi publica os dados nos tópicos MQTT.
5. Um cliente (Node-RED, Home Assistant, app ou backend) consome os dados e exibe/armazenha.

---

## 🔧 Sensores utilizados

### 1) MQ-135 (Gás / Qualidade do ar)
Sensor usado para detectar presença de gases (como amônia, benzeno, fumaça e outros compostos), servindo como indicador de qualidade do ar.

> Observação: sensores da linha MQ exigem **calibração** e tempo de aquecimento para melhor precisão.

### 2) DHT11 (Temperatura e Umidade)
Sensor digital simples e de baixo custo para monitoramento básico de:
- Temperatura (°C)
- Umidade relativa (%)

### 3) BMP180 (Pressão atmosférica)
Sensor barométrico via I2C para leitura de:
- Pressão atmosférica (hPa)
- Temperatura (°C, auxiliar)

Pode ser usado para análises climáticas e estimativa de altitude.

### 4) LDR (Luminosidade)
Resistor dependente de luz:
- Quanto maior a luz, menor a resistência.
- Usado para indicar claro/escuro ou nível relativo de iluminação.

### 5) MAX4466 (Ruído ambiente)
Módulo de microfone com ganho ajustável, utilizado para captar nível de ruído ambiente por variação de sinal analógico.

---

## 📡 Comunicação MQTT

O MQTT é um protocolo leve e eficiente para IoT.  
Neste projeto, o Raspberry Pi atua como **publisher**, enviando dados para um broker.

### Exemplo de tópicos

- `monitoramento/ar/mq135`
- `monitoramento/temperatura`
- `monitoramento/umidade`
- `monitoramento/pressao`
- `monitoramento/luminosidade`
- `monitoramento/ruido`

### Exemplo de payload JSON

```json
{
  "timestamp": "2026-08-25T12:00:00Z",
  "mq135": 320,
  "temperatura": 27.4,
  "umidade": 61,
  "pressao": 1012.8,
  "luminosidade": 740,
  "ruido": 415
}
```

---

## 🖥️ Hardware necessário

- Raspberry Pi (com Raspberry Pi OS)
- Sensor MQ-135
- Sensor DHT11
- Sensor BMP180 (I2C)
- LDR + resistor para divisor de tensão
- Sensor de ruído MAX4466
- Conversor ADC (ex.: MCP3008), quando necessário para sensores analógicos
- Protoboard e jumpers
- Fonte adequada para o Raspberry Pi

---

## 🧩 Software e bibliotecas (exemplo)

- Python 3
- `paho-mqtt` (publicação MQTT)
- Bibliotecas para DHT11/BMP180
- Leitura GPIO/I2C/SPI conforme os módulos usados

Instalação base (exemplo):

```bash
sudo apt update
sudo apt install -y python3-pip i2c-tools
pip3 install paho-mqtt
```

---

## ▶️ Execução (exemplo)

1. Configure o broker MQTT (IP, porta, usuário/senha se houver).
2. Ajuste os pinos e parâmetros dos sensores no código.
3. Execute o script principal:

```bash
python3 main.py
```

---

## 📈 Possíveis aplicações

- Monitoramento de salas e laboratórios
- Automação residencial
- Alertas de qualidade do ar
- Histórico ambiental em banco de dados/dashboard
- Projetos educacionais de IoT

---

## ⚠️ Boas práticas

- Calibrar sensores analógicos (MQ-135, LDR, MAX4466).
- Filtrar ruído nas leituras (média móvel, mediana etc.).
- Definir intervalo de amostragem adequado.
- Implementar reconexão automática no MQTT.
- Adicionar logs e tratamento de exceções.

---

## 👨‍💻 Autor

Projeto desenvolvido por **davidpezzin** para monitoramento ambiental com Raspberry Pi e MQTT.

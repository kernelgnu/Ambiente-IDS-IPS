# Sistema de Detecção e Prevenção de Intrusões (IDS/IPS) com Snort e Kibana

## **Descrição**
Este projeto implementa um sistema de detecção e prevenção de intrusões (IDS/IPS) utilizando o **Snort** para análise de tráfego de rede e o **Elastic Stack** (Elasticsearch, Logstash e Kibana) para visualização e monitoramento de alertas. O objetivo é criar um ambiente seguro capaz de detectar ameaças em tempo real e apresentar os dados de forma clara e centralizada.

---

## **Pré-requisitos**
- **Sistema operacional:** Ubuntu 22.04.
- **Softwares necessários:**
  - Snort
  - Elasticsearch
  - Logstash
  - Kibana
- **Ferramentas auxiliares:** 
  - Nmap (para testes).
- **Conhecimentos básicos:**
  - Redes de computadores.
  - Linha de comando no Linux.

---

## **Estrutura do Ambiente**
- **Sistema Operacional:** Ubuntu 22.04.5 LTS
- **Hardware:**
  - CPU: Intel i5, 4 núcleos.
  - Memória RAM: 8GB.
  - Armazenamento: 50GB SSD.
- **Rede:**
  - Interface de rede configurada como `eth0`.
  - IP local: `192.168.1.100/24`.

---

## **Instalação e Configuração**

### **1. Instalação do Snort**
1. Atualize os pacotes:
   ```bash
   sudo apt update && sudo apt upgrade -y
2. Instale o Snort:
   ```bash
   sudo apt install snort -y
3. Configure o arquivo /etc/snort/snort.conf:
   - Defina a rede interna:
     ```bash
     var HOME_NET 192.168.1.100/24
   - Adicione o formato de saída JSON:
     ```bash
     output alert_json: filename snort_alerts.json

---

### **2. Configuração do Elastic Stack**
2.1 Elasticsearch
  1. Instale o Elasticsearch:
     ```bash
     sudo apt install elasticsearch -y
  2. Inicie o serviço:
     ```bash
     sudo systemctl start elasticsearch
     sudo systemctl enable elasticsearch
2.2 Logstash
  1. Instale o Logstash:
     ```bash
     sudo apt install logstash -y
  2. Crie o pipeline em /etc/logstash/conf.d/snort.conf:
     ```bash
     input {
        file {
            path => "/var/log/snort/snort_alerts.json"
            start_position => "beginning"
            codec => "json"
        }
        }
        filter {
            if [alert] {
                mutate {
                    add_field => { "alert_message" => "%{[alert][signature]}" }
                    rename => { "[alert][category]" => "category" }
                }
            }
        }
        output {
            elasticsearch {
                hosts => ["http://localhost:9200"]
                index => "snort-alerts-%{+YYYY.MM.dd}"
            }
            stdout { codec => rubydebug }
        }
  3. Inicie o serviço:
     ```bash
     sudo systemctl start logstash
2.3 Kibana
  1. Instale o Kibana:
     ```bash
     sudo apt install kibana -y
  2. Inicie o serviço:
     ```bash
     sudo systemctl start kibana
     sudo systemctl enable kibana
  3. Acesse o Kibana no navegador: http://localhost:5601.

### **3. Integração e visualização**
1. Adicionar o índice no Kibana:
   - Vá em Management > Index Patterns.
   - Crie um padrão de índice: snort-alerts-*
2. Criar dashboards:
   - Contagem de alertas por categoria.
   - Origem e destino dos ataques.

### **Testes**
1. Scan de portas com Nmap:
   ```bash
   nmap -sS -p 22,80,443 192.168.1.1
2. Ping ICMP:
   ```bash
   ping -c 4 8.8.8.8
3. Confira os alertas no Kibana no painel de visualização ou no arquivo de log:
   ```bash
   cat /var/log/snort/snort_alerts.json

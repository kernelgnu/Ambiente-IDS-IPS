# Ambiente SIEM com ELK Stack

## Descrição
Este projeto implementa um ambiente de SIEM (Security Information and Event Management) utilizando a ELK Stack (Elasticsearch, Logstash e Kibana). O objetivo é coletar, armazenar, analisar e visualizar eventos de segurança em tempo real, proporcionando maior visibilidade e controle sobre a infraestrutura de TI.

---

## Funcionalidades
- Coleta de logs de diferentes fontes (servidores, firewalls, IDS/IPS).
- Armazenamento centralizado de eventos.
- Dashboards personalizáveis para visualização de métricas e alertas.
- Regras de alerta para atividades suspeitas ou maliciosas.
- Integração com ferramentas como Suricata e Syslog.

---

## Tecnologias Usadas
- **Elasticsearch**: Armazenamento e busca de logs.
- **Logstash**: Coleta e processamento de dados.
- **Kibana**: Visualização e criação de dashboards.
- **Beats**: Filebeat e Metricbeat para coleta de logs e métricas.

---

## Como Usar

### Pré-requisitos
Certifique-se de ter:
- Sistema operacional Linux (testado no Ubuntu).
- Recursos de hardware adequados (mínimo 4 GB de RAM e 2 CPUs).
- Acesso root para instalar pacotes.

### Instalação

1. Atualize o sistema:
   ```bash
   sudo apt update && sudo apt upgrade -y
   ```

2. Instale o Elasticsearch:
   ```bash
   wget -qO - https://artifacts.elastic.co/GPG-KEY-elasticsearch | sudo apt-key add -
   sudo apt install apt-transport-https
   echo "deb https://artifacts.elastic.co/packages/7.x/apt stable main" | sudo tee -a /etc/apt/sources.list.d/elastic-7.x.list
   sudo apt update && sudo apt install elasticsearch
   ```

3. Configure e inicie o Elasticsearch:
   ```bash
   sudo nano /etc/elasticsearch/elasticsearch.yml
   # Ajuste as configurações necessárias, como network.host
   sudo systemctl start elasticsearch
   sudo systemctl enable elasticsearch
   ```

4. Instale o Logstash:
   ```bash
   sudo apt install logstash
   ```

5. Configure o Logstash com um pipeline:
   ```bash
   sudo nano /etc/logstash/conf.d/logstash.conf
   # Adicione as configurações para entrada (input), processamento (filter) e saída (output).
   sudo systemctl start logstash
   sudo systemctl enable logstash
   ```

6. Instale o Kibana:
   ```bash
   sudo apt install kibana
   sudo systemctl start kibana
   sudo systemctl enable kibana
   ```

7. Acesse o Kibana no navegador:
   ```plaintext
   http://<SEU_IP>:5601
   ```

8. Configure Beats (Filebeat/Metricbeat) nos dispositivos ou servidores que irão enviar logs para o SIEM.

---

## Exemplo de Uso
- Um ataque de força bruta identificado por Suricata gera logs que são enviados ao Logstash.
- Logstash processa os dados e os envia para o Elasticsearch.
- Kibana exibe um alerta em um painel de controle mostrando o número de tentativas de login falhas.

---

## Estrutura do Projeto
```plaintext
siem-environment/
├── elasticsearch/      # Configurações do Elasticsearch
├── logstash/           # Configurações do Logstash
├── kibana/             # Configurações do Kibana
├── beats/              # Configurações do Filebeat e Metricbeat
├── docs/               # Documentação e imagens
└── README.md           # Este arquivo
```

---

## Aprendizados
- Configuração de um ambiente SIEM completo com ELK Stack.
- Criação de pipelines para processamento de logs.
- Configuração de dashboards e alertas personalizados no Kibana.

---

## Próximos Passos
- Implementar regras de detecção específicas para ameaças comuns.
- Automatizar a instalação com scripts Shell ou Ansible.
- Adicionar integrações com outras ferramentas de segurança, como Wazuh.

---

## Contribuições
Contribuições são bem-vindas! Para sugerir melhorias ou relatar problemas, abra uma issue ou envie um pull request.

---

## Licença
Este projeto está licenciado sob a [MIT License](LICENSE).

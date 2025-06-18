# Módulo 2: Wazuh - SIEM e HIDS

## Aula 2.1: Instalação e Configuração Inicial

### Teoria: Arquitetura do Wazuh

#### Componentes Principais
1. **Wazuh Manager**: Centro de controle e correlação
2. **Wazuh Agent**: Coletores de dados nos endpoints
3. **Wazuh Indexer**: Baseado no OpenSearch (Elasticsearch)
4. **Wazuh Dashboard**: Interface web para visualização

#### Arquitetura de Funcionamento
```
[Agents] → [Manager] → [Indexer] → [Dashboard]
    ↓         ↓          ↓           ↓
 Collect   Analyze   Store      Visualize
```

#### Capacidades do Wazuh
- **Log Analysis**: Parsing e correlação de logs
- **File Integrity Monitoring**: Detecção de alterações
- **Rootkit Detection**: Identificação de rootkits
- **Active Response**: Ações automatizadas
- **Vulnerability Assessment**: Scanner integrado
- **Compliance**: PCI DSS, GDPR, HIPAA, NIST 800-53

### Instalação Passo a Passo

#### Pré-requisitos do Sistema
- Ubuntu 20.04+ / CentOS 8+ / RHEL 8+
- Mínimo 4GB RAM, 8GB recomendado
- 50GB+ de espaço em disco
- Conectividade de rede

#### Comandos de Instalação (Ubuntu)
```bash
# Atualizar sistema
sudo apt update && sudo apt upgrade -y

# Baixar instalador Wazuh
curl -sO https://packages.wazuh.com/4.7/wazuh-install.sh

# Executar instalação all-in-one
sudo bash wazuh-install.sh -a
```

### Links para Análise Técnica

#### Documentação Oficial
- **Wazuh Documentation**: https://documentation.wazuh.com/
- **Wazuh GitHub**: https://github.com/wazuh/wazuh
- **Installation Guide**: https://documentation.wazuh.com/current/installation-guide/

#### Regras e Decoders
- **Wazuh Rules**: https://github.com/wazuh/wazuh/tree/master/ruleset/rules
- **Custom Rules Repository**: https://github.com/wazuh/wazuh-ruleset
- **Community Contributions**: https://wazuh.com/community/

### Exercício Prático
1. Instalar Wazuh all-in-one
2. Acessar dashboard web
3. Explorar interface inicial
4. Verificar status dos serviços

---

## Aula 2.2: Agentes e Coleta de Logs (1h)

### Teoria: Agentes Wazuh

#### Tipos de Monitoramento
1. **Agent-based**: Agente instalado no endpoint
2. **Agentless**: Conexão via SSH/WMI
3. **Syslog**: Recepção de logs via syslog

#### Comunicação Manager-Agent
- **Protocolo**: UDP 1514 (padrão)
- **Autenticação**: Chaves pré-compartilhadas
- **Criptografia**: AES encryption
- **Compressão**: Opcional para reduzir bandwidth

#### Configuração do Agente

##### Linux Agent (ossec.conf)
```xml
<ossec_config>
  <client>
    <server>192.168.1.10</server>
    <port>1514</port>
    <protocol>udp</protocol>
  </client>
  
  <localfile>
    <log_format>syslog</log_format>
    <location>/var/log/auth.log</location>
  </localfile>
  
  <localfile>
    <log_format>apache</log_format>
    <location>/var/log/apache2/access.log</location>
  </localfile>
</ossec_config>
```

##### Windows Agent (ossec.conf)
```xml
<ossec_config>
  <client>
    <server>192.168.1.10</server>
    <port>1514</port>
  </client>
  
  <localfile>
    <location>Security</location>
    <log_format>eventchannel</log_format>
  </localfile>
  
  <localfile>
    <location>System</location>
    <log_format>eventchannel</log_format>
  </localfile>
</ossec_config>
```

### File Integrity Monitoring (FIM)

#### Configuração FIM
```xml
<syscheck>
  <!-- Frequência de verificação -->
  <frequency>21600</frequency>
  
  <!-- Diretórios Linux -->
  <directories check_all="yes">/etc,/usr/bin,/usr/sbin</directories>
  <directories check_all="yes">/bin,/sbin</directories>
  
  <!-- Diretórios Windows -->
  <directories check_all="yes">C:\Windows\System32</directories>
  <directories check_all="yes">C:\Program Files</directories>
  
  <!-- Ignorar arquivos -->
  <ignore>/etc/mtab</ignore>
  <ignore>/etc/hosts.deny</ignore>
</syscheck>
```

### Links para Análise Técnica

#### Samples de Configuração
- **Agent Configuration**: https://documentation.wazuh.com/current/user-manual/reference/ossec-conf/
- **FIM Configuration**: https://documentation.wazuh.com/current/user-manual/capabilities/file-integrity/
- **Log Analysis**: https://documentation.wazuh.com/current/user-manual/capabilities/log-data-analysis/

#### Logs de Exemplo para Análise
- **Sample Logs Repository**: https://github.com/logpai/loghub
- **Security Onion Samples**: https://github.com/Security-Onion-Solutions/securityonion-samples
- **EVTX Samples**: https://github.com/sbousseaden/EVTX-ATTACK-SAMPLES

### Exercício Prático
1. Instalar agentes Linux e Windows
2. Configurar coleta de logs específicos
3. Configurar FIM em diretórios críticos
4. Gerar eventos de teste
5. Verificar recepção no manager

---

## Aula 2.3: Detecção de Ameaças (1h)

### Teoria: Regras e Detecção

#### Estrutura das Regras Wazuh
```xml
<rule id="100001" level="10">
  <if_sid>5712</if_sid>
  <srcip>!192.168.1.0/24</srcip>
  <description>SSH login from external network</description>
  <group>authentication_success,ssh,</group>
</rule>
```

#### Elementos das Regras
- **id**: Identificador único
- **level**: Nível de criticidade (0-15)
- **if_sid**: Regra pai
- **match/regex**: Padrão de busca
- **description**: Descrição do alerta
- **group**: Categorias da regra

#### Níveis de Criticidade
- **0-3**: Informativo
- **4-6**: Baixo
- **7-9**: Médio
- **10-12**: Alto
- **13-15**: Crítico

### Regras Customizadas

#### Detecção de Brute Force SSH
```xml
<rule id="100010" level="10" frequency="8" timeframe="120">
  <if_matched_sid>5716</if_matched_sid>
  <same_source_ip />
  <description>SSH brute force attack</description>
  <group>authentication_failures,ssh,</group>
</rule>
```

#### Detecção de PowerShell Suspeito
```xml
<rule id="100020" level="12">
  <if_sid>60106</if_sid>
  <field name="win.eventdata.commandLine">\.Invoke|IEX|DownloadString</field>
  <description>Suspicious PowerShell execution detected</description>
  <group>attack,powershell,</group>
</rule>
```

### Active Response

#### Configuração de Resposta Automática
```xml
<active-response>
  <command>firewall-drop</command>
  <location>local</location>
  <rules_id>100010</rules_id>
  <timeout>600</timeout>
</active-response>
```

### Links para Análise Técnica

#### Repositórios de Regras
- **Atomic Red Team**: https://github.com/redcanaryco/atomic-red-team
- **Sigma Rules**: https://github.com/SigmaHQ/sigma
- **Wazuh Custom Rules**: https://github.com/wazuh/wazuh-ruleset

#### Databases de TTPs
- **MITRE ATT&CK Navigator**: https://mitre-attack.github.io/attack-navigator/
- **LOLBAS**: https://lolbas-project.github.io/
- **GTFOBins**: https://gtfobins.github.io/

#### Threat Hunting Resources
- **ThreatHunter-Playbook**: https://github.com/OTRF/ThreatHunter-Playbook
- **Hunting Queries**: https://github.com/microsoft/WindowsDefenderATP-Hunting-Queries
- **Splunk Security Content**: https://github.com/splunk/security_content

### Exercício Prático
1. Criar regras customizadas para detecção específica
2. Configurar active response
3. Simular ataques (brute force, malware execution)
4. Analisar alertas gerados
5. Ajustar falsos positivos

---

## Aula 2.4: Análise de Incidentes (1h)

### Teoria: Investigação de Alertas

#### Metodologia de Análise
1. **Triage**: Classificação inicial do alerta
2. **Analysis**: Investigação detalhada
3. **Containment**: Contenção da ameaça
4. **Eradication**: Eliminação da causa raiz
5. **Recovery**: Restauração dos serviços
6. **Lessons Learned**: Documentação e melhoria

#### Correlação de Eventos
- **Temporal**: Eventos relacionados por tempo
- **Spatial**: Eventos relacionados por localização/host
- **Causal**: Eventos com relação causa-efeito
- **Behavioral**: Eventos relacionados por padrão

### Dashboard e Relatórios

#### KPIs Essenciais
- **MTTD (Mean Time To Detection)**: Tempo médio para detecção
- **MTTR (Mean Time To Response)**: Tempo médio para resposta
- **Alert Volume**: Volume de alertas por período
- **False Positive Rate**: Taxa de falsos positivos

#### Dashboards Customizados
```json
{
  "visualization": {
    "title": "Top Attack Sources",
    "type": "pie",
    "query": {
      "match": {
        "rule.level": {"gte": 10}
      }
    },
    "aggs": {
      "source_ips": {
        "terms": {
          "field": "data.srcip"
        }
      }
    }
  }
}
```

### Análise Forense com Wazuh

#### Coleta de Evidências
```bash
# Coleta de logs específicos
/var/ossec/bin/ossec-logcollector -f

# Verificação de integridade
/var/ossec/bin/syscheck_control -u agent_id

# Análise de rootkits
/var/ossec/bin/rootcheck_control -u agent_id
```

### Links para Análise Técnica

#### Datasets para Análise
- **DARPA Intrusion Detection**: https://www.ll.mit.edu/r-d/datasets/1999-darpa-intrusion-detection-evaluation-dataset
- **KDD Cup 99**: http://kdd.ics.uci.edu/databases/kddcup99/kddcup99.html
- **NSL-KDD**: https://www.unb.ca/cic/datasets/nsl.html
- **CICIDS2017**: https://www.unb.ca/cic/datasets/ids-2017.html

#### Ferramentas de Análise
- **Elastic Stack**: https://www.elastic.co/elastic-stack/
- **Grafana**: https://grafana.com/
- **Jupyter Notebooks**: https://jupyter.org/
- **HELK**: https://github.com/Cyb3rWard0g/HELK

#### OSINT para Investigação
- **AbuseIPDB**: https://www.abuseipdb.com/
- **IPInfo**: https://ipinfo.io/
- **Shodan**: https://www.shodan.io/
- **Censys**: https://censys.io/

### Casos de Uso Práticos

#### Investigação de Malware
1. Analisar alerta inicial
2. Correlacionar eventos temporais
3. Identificar processo pai
4. Verificar comunicação de rede
5. Analisar persistência
6. Documentar IOCs

#### Investigação de Insider Threat
1. Analisar padrões de acesso
2. Verificar horários anômalos
3. Correlacionar com RH events
4. Analisar transferência de dados
5. Verificar escalação de privilégios

### Exercício Prático: Análise Completa
1. Simular infecção por malware
2. Analisar alertas gerados
3. Correlacionar eventos relacionados
4. Identificar timeline do ataque
5. Extrair IOCs
6. Criar relatório de incidente

---

## Laboratórios Especializados

### Lab 2.1: Detecção de Ransomware
- Simular execução de ransomware
- Configurar regras específicas
- Analisar padrões de criptografia
- Implementar response automático

### Lab 2.2: Análise de Lateral Movement
- Simular movimento lateral na rede
- Detectar Pass-the-Hash
- Analisar autenticações anômalas
- Correlacionar eventos multi-host

### Lab 2.3: Threat Hunting com Wazuh
- Buscar por TTPs específicos
- Utilizar consultas personalizadas
- Analisar logs históricos
- Identificar campaigns persistentes

## Recursos Adicionais

### Treinamentos Especializados
- **Wazuh Training**: https://wazuh.com/training/
- **SANS FOR508**: https://www.sans.org/cyber-security-courses/digital-forensics-incident-response/

### Comunidade
- **Wazuh Community**: https://wazuh.com/community/
- **Wazuh Google Groups**: https://groups.google.com/forum/#!forum/wazuh
- **Discord/Slack Communities**: Links atualizados na documentação

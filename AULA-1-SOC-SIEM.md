# Módulo 1: Fundamentos de Monitoramento de Segurança

## Aula 1.1: Conceitos Base

### Teoria: Definições Fundamentais

#### Riscos vs Ameaças vs Vulnerabilidades
- **Vulnerabilidade**: Fraqueza em um sistema que pode ser explorada
- **Ameaça**: Agente ou evento que pode explorar uma vulnerabilidade
- **Risco**: Probabilidade de uma ameaça explorar uma vulnerabilidade e causar impacto

**Fórmula básica**: `Risco = Ameaça × Vulnerabilidade × Impacto`

#### Tipos de Ataques Mais Comuns
1. **Malware**: Vírus, trojans, ransomware, rootkits
2. **Ataques de Rede**: DDoS, Man-in-the-Middle, packet sniffing
3. **Ataques de Aplicação**: SQL injection, XSS, buffer overflow
4. **Engenharia Social**: Phishing, pretexting, baiting
5. **Ataques Internos**: Privilege escalation, data exfiltration

#### Frameworks de Segurança
- **NIST Cybersecurity Framework**: Identify, Protect, Detect, Respond, Recover
- **ISO 27001**: Gestão de segurança da informação
- **MITRE ATT&CK**: Tactics, Techniques, and Procedures (TTPs)

### Links para Análise Técnica

#### Bases de Dados de Vulnerabilidades
- **CVE (Common Vulnerabilities and Exposures)**: https://cve.mitre.org/
- **NVD (National Vulnerability Database)**: https://nvd.nist.gov/
- **Exploit Database**: https://www.exploit-db.com/
- **VulnDB**: https://vulndb.cyberriskanalytics.com/

#### Threat Intelligence
- **MITRE ATT&CK**: https://attack.mitre.org/
- **AlienVault OTX**: https://otx.alienvault.com/
- **IBM X-Force**: https://exchange.xforce.ibmcloud.com/
- **VirusTotal**: https://www.virustotal.com/

### Exercício Prático
Analisar um CVE específico (ex: CVE-2021-44228 - Log4Shell):
1. Identificar a vulnerabilidade
2. Entender o vetor de ataque
3. Avaliar o impacto
4. Verificar exploits disponíveis

---

## Aula 1.2: Arquitetura de Monitoramento

### Teoria: Componentes de um SOC

#### Estrutura Básica de um SOC
1. **Tier 1 - Analistas de Monitoramento**
   - Triagem de alertas
   - Classificação inicial
   - Escalação de incidentes

2. **Tier 2 - Analistas de Incidentes**
   - Investigação aprofundada
   - Análise forense básica
   - Contenção inicial

3. **Tier 3 - Especialistas/Hunters**
   - Threat hunting
   - Análise forense avançada
   - Desenvolvimento de regras

#### Tecnologias Essenciais

##### SIEM (Security Information and Event Management)
- **Função**: Coleta, correlação e análise de logs
- **Exemplos**: Splunk, QRadar, Wazuh, ELK Stack
- **Capacidades**: Real-time monitoring, alerting, reporting

##### SOAR (Security Orchestration, Automation and Response)
- **Função**: Automação de resposta a incidentes
- **Exemplos**: Phantom, Demisto, TheHive
- **Capacidades**: Playbooks, case management, integration

##### XDR (Extended Detection and Response)
- **Função**: Detecção e resposta integrada
- **Exemplos**: CrowdStrike, SentinelOne, Microsoft Defender
- **Capacidades**: Multi-vector detection, automated response

#### Indicadores de Compromisso (IoCs)
- **Hash de arquivos**: MD5, SHA1, SHA256
- **Endereços IP**: IPs maliciosos, C&C servers
- **Domínios**: Domínios suspeitos, DGA domains
- **URLs**: Links maliciosos, phishing sites
- **Registry Keys**: Chaves de registro suspeitas
- **Mutexes**: Identificadores de malware

### Links para Análise Técnica

#### Plataformas de Threat Intelligence
- **Hybrid Analysis**: https://www.hybrid-analysis.com/
- **Joe Sandbox**: https://www.joesandbox.com/
- **Any.run**: https://any.run/
- **URLVoid**: https://www.urlvoid.com/

#### IOC Feeds
- **Abuse.ch**: https://abuse.ch/
- **EmergingThreats**: https://rules.emergingthreats.net/
- **Malware Domain List**: https://www.malwaredomainlist.com/
- **PhishTank**: https://www.phishtank.com/

### Exercício Prático
Análise de um sample malware no Hybrid Analysis:
1. Submeter hash conhecido
2. Analisar comportamento
3. Extrair IOCs
4. Identificar TTPs do MITRE ATT&CK

---

## Aula 1.3: Preparação do Ambiente Lab

### Teoria: Arquitetura do Lab

#### Topologia de Rede Recomendada
```
[Internet] ← Firewall → [DMZ] → [Internal Network]
                         ↓
                    [Management Network]
```

#### Sistemas Necessários
1. **Wazuh Manager** (Ubuntu 20.04)
   - CPU: 2 cores
   - RAM: 4GB
   - Storage: 50GB

2. **Windows Server** (2019/2022)
   - CPU: 2 cores
   - RAM: 4GB
   - Storage: 60GB

3. **Windows Client** (10/11)
   - CPU: 2 cores
   - RAM: 4GB
   - Storage: 40GB

4. **Zabbix Server** (Ubuntu 20.04)
   - CPU: 2 cores
   - RAM: 4GB
   - Storage: 50GB

5. **Attacker Machine** (Kali Linux)
   - CPU: 2 cores
   - RAM: 4GB
   - Storage: 40GB

#### Configurações de Rede
- **Management**: 192.168.1.0/24
- **Internal**: 192.168.10.0/24
- **DMZ**: 192.168.100.0/24


---

## Material de Apoio

### Certificações Relacionadas
- **CompTIA Security+**
- **GCIH (GIAC Certified Incident Handler)**
- **GCFA (GIAC Certified Forensic Analyst)**
- **CySA+ (CompTIA Cybersecurity Analyst)**

<h1 align="center"> Monitoramento e Observabilidade </h1>

<h2>OBJETIVO</h2>

- O objetivo é realizar o monitoramento do cluster e das aplicações utilizando o prometheus e o grafana loki, demonstrar no grafana e encaminhar os alertas para o alert manager + slack.

- Criação dos recursos acima utilizando os objetos do kubernetes.

- Para utilização do loki/promtail, utilzamos o helm.

A estrutura acima deverá expor 2 URLs:
    • 1 para o AlertManager
    • 1 para o Grafana


- Ao final, inserir o código no GitLab e contruir uma pipeline de entrega da aplicação (GitLab CI) no cluster GKE.

    - Passos da pipeline:
        - Checkout do código e Lint
        - Construir a imagem da aplicação
        - Enviar sua imagem para o Docker Hub
        - Instalar sua aplicação no Kubernetes
        - Instalar componentes de monitoramento e alerta


- Para realização do desafio, utilizamos o Terraform e como provedor, o GCP (Google Cloud) e para versionamento e automatização, o GitLab.

<h2>FERRAMENTAS UTILIZADAS</h2>

- <b>Python</b> como linguagem de programação para montagem das APIs referentes ao desafio 1.
- <b>Google Cloud - GCP</b> como provedor na nuvem.
- <b>Terraform</b> como ferramenta para construção, manutenção e versionamento de infraestrutura.
- <b>Docker</b> como ferramenta para conteinerização usada para empacotar, entregas e executar aplicações em containers Linux.
- <b>Kubernetes</b> como ferramenta para orquestração dos contêineres.
- <b>Prometheus</b> para receber todas a métricas coletadas pelo Kubernetes e aplicações.
- <b>Alert Manager</b> para gerar alertas sobre o kubernetes e aplicações.
- <b>Loki/Promtail</b> para coleta e exibição dos logs das aplicações e cluster GKE.
- <b>Grafana</b> para visualização das métricas expostas pelo seu cluster Kubernetes e aplicações.
- <b>Slack</b> canal para receber os alertas gerados pelo AlertManager.
- <b>GitLab</b> como gerenciador de repositório baseado em git e gerenciamento de tarefas e CI/CD.

<h2>RECURSOS UTILIZADOS NO GCP</h2>

- [X] VPC Network + Subnetwork 
- [X] Cloud router
- [X] NAT Gateway
- [X] Route Tables
- [X] Firewall Rules
- [X] Bucket
- [X] GKE
- [X] Nood Pools

<h1>PARTE 1</h1>

Para o monitoramento, criei um namespace específico "monitoring", aonde todos os recursos necessários foram direcionados.

Criação dos arquivos .yaml para o <b>Prometheus</b>, possibilitanto o recebimento das métricas coletadas no kubernetes e nas aplicações.

- [X] Prometheus-deployment 
- [X] Prometheus-configmap (arquivo para definição das rules do prometheus para o alert manager e as métricas a serem recebidas por ele)
- [X] Prometheus-clusterrole
- [X] Prometheus-ingress
- [X] Prometheus-service (tipo LB)

Criação dos arquivos .yaml para o <b>Alert Manager</b>, para visualização dos alertas ativos e conexão com o slack.

- [X] Alert-manager-deployment
- [X] Alert-manager-configmap (conexão com slack e definição das rotas)
- [X] Alert-manager-service (tipo LB)
- [X] Alert-manager-template

Criação dos arquivos .yaml para o <b>Grafana</b>, para visualização das métricas através de dashboards.

- [X] Grafana-deployment
- [X] Grafana-configmap (conexão com slack e definição das rotas)
- [X] Grafana-service (tipo LB)

Criação dos arquivos .yaml para o <b>Node Exporter</b>, responsável pelo monitoramento do kubernetes, atua em conjunto com o Prometheus.

- [X] Node-exporter-daemonset-deployment
- [X] Node-exporter-service (tipo clusterIP)

Após a criação de todos os arquivos, é possível acessar com IP externo as URLs do: Grafana, Prometheus, Alert Manager e da aplicação.

No Grafana, também é possível monitorar os logs gerados pelo Grafana Loki.


<h1>PARTE 2</h1>

Com tudo funcionando, realizei os testes de carga/performance, de 2 formas: pelo k6 e de forma manual, acessando o pod e rodando o comando de stress.

Os testes realizados tiveram o objetivo de averiguar a performance da aplicação e estressar a CPU e memória.

Criamos outros alertas que entendemos importantes:

- [X] Pod não está saudável 
- [X] Container e Node acima de 80% CPU 
- [X] Container e node com pouca memória - mais de 80% em uso
- [X] Node não está pronto
- [X] SLO abaixo de 99%

Vejamos:

K6 (Teste do k6 por meio de script .js já integrado com o Grafana, através do plugin)

De forma manual, utilizando o stress-ng (dentro do pod).



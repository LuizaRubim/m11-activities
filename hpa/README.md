# Cluster Kubernetes com HPA - Teste de Carga

Este projeto implementa um cluster Kubernetes com escalabilidade automática usando Horizontal Pod Autoscaler (HPA). O sistema consiste em uma aplicação PHP que simula carga de CPU e um sistema de teste de carga usando k6.

## Estrutura do Projeto

```
.
├── README.md
└── app/
    ├── index.php              # Aplicação PHP com simulação de carga
    ├── deployment.yaml        # Deployment e Service do PHP-Apache
    ├── hpa.yaml    # Configuração do HPA
    └── teste-k6.yaml          # Configuração do teste de carga
```

## Requisitos

- Kubernetes cluster (kind, minikube ou k3s)
- kubectl
- PHP 8.2
- Metrics Server instalado no cluster
- k6 para testes de carga

## Configuração

1. Crie o cluster Kubernetes (se ainda não existir):
   ```bash
   kind create cluster
   ```

2. Instale o Metrics Server (necessário para o HPA):
   ```bash
   kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
   ```

3. Aplique as configurações:
   ```bash
   kubectl apply -f app/deployment.yaml
   kubectl apply -f app/hpa.yaml
   ```

4. Execute os testes de carga:
   ```bash
   kubectl apply -f app/teste-k6.yaml
   ```

## Monitoramento

Para monitorar o HPA em ação:
```bash
kubectl get hpa php-apache-hpa --watch
```

Para visualizar os pods em tempo real:
```bash
kubectl get pods -w
```

## Análise de Resultados

O teste de carga é configurado para:
1. Iniciar com 20 usuários virtuais
2. Manter carga constante por 1 minuto
3. Escalar para 50 usuários
4. Manter carga alta por 1 minuto
5. Reduzir gradualmente para 0

O HPA está configurado para:
- Manter o uso de CPU em torno de 75%
- Escalar de 2 a 10 pods
- Responder a aumentos de carga em aproximadamente 15 segundos

## Inovações Implementadas

1. Controle fino de carga CPU
2. Métricas detalhadas por requisição
3. Testes de carga graduais para análise precisa
4. Configuração otimizada do HPA

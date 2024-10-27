
## Parte 2: Respostas e Soluções

### 1. Configuração do Deployment e HPA

- **Deployment `deployment.yaml`**:
  ```yaml
  kubectl apply -f deployment.yaml
  ```

- **Service `service.yaml`**:
  ```yaml
  kubectl apply -f service.yaml
  ```

- **Horizontal Pod Autoscaler (HPA) `ascale-deploy`**:
  ```yaml
  kubectl apply -f auto-scale.yaml
  ```

### 2. Instalação e Configuração do Metrics Server

- Instalamos o Metrics Server usando o comando:
  ```bash
  kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
  ```

- Ajustamos o Deployment do Metrics Server para ignorar a verificação de TLS e preferir o IP interno dos nós:
  ```yaml
  containers:
  - name: metrics-server
    image: registry.k8s.io/metrics-server/metrics-server:v0.7.2
    args:
    - --cert-dir=/tmp
    - --secure-port=10250
    - --kubelet-insecure-tls
    - --kubelet-preferred-address-types=InternalIP
  ```

### 3. Teste de Carga para Escala para Cima

- Executamos um pod temporário para gerar carga de CPU no Deployment `ascale-deploy`:
  ```bash
  kubectl run -i --tty load-generator --rm --image=busybox:1.28 --restart=Never -- /bin/sh -c "while sleep 0.01; do wget -q -O- http://nginx-service; done"
  ```

- Observamos o HPA aumentar as réplicas em resposta à carga com o comando:
  ```bash
  kubectl get hpa nginx-autoscaler --watch
  ```

### 4. Ajuste da Estabilização de Escala para Baixo

- Quando a carga foi removida, observamos que o HPA levou mais tempo para reduzir as réplicas. Para resolver isso, ajustamos a configuração de estabilização do HPA:
  ```yaml
  spec:
    behavior:
      scaleDown:
        stabilizationWindowSeconds: 60
        policies:
        - type: Percent
          value: 100
          periodSeconds: 60
  ```

- Após esse ajuste, o HPA respondeu mais rapidamente à queda na carga, reduzindo o número de réplicas conforme esperado.

### 5. Resultados Observados

- **Escala para Cima**: O HPA aumentou o número de réplicas quando a carga excedeu 50% de utilização de CPU.
- **Escala para Baixo**: Após o ajuste de estabilização, o HPA diminuiu rapidamente o número de réplicas quando a carga foi removida.

---

Esse documento detalha o processo completo, desde a configuração inicial até a execução de testes e ajustes no HPA. Esse exercício é um excelente exemplo de como configurar e otimizar o HPA no Kubernetes usando o Metrics Server.
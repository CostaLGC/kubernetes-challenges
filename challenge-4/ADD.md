Para configurar os parâmetros do Metrics Server (como ``--kubelet-insecure-tls e --kubelet-preferred-address-types``), você precisa editar o Deployment do Metrics Server diretamente no Kubernetes.

Aqui estão os passos:

Passo 1: Editar o Deployment do Metrics Server
Use o comando abaixo para abrir o Deployment do Metrics Server para edição:

```
kubectl edit deployment metrics-server -n kube-system
```
Localize a seção ``spec.template.spec.containers``, onde o contêiner metrics-server está configurado. Dentro da chave args, adicione as opções necessárias para desativar a verificação TLS e definir o tipo de endereço do Kubelet.

Adicione os seguintes argumentos na seção args do contêiner metrics-server:

````
      containers:
      - name: metrics-server
        image: registry.k8s.io/metrics-server/metrics-server:v0.7.2
        args:
        - --cert-dir=/tmp
        - --secure-port=10250
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
        - --kubelet-insecure-tls                   # Adiciona este argumento para ignorar verificação TLS
        - --kubelet-preferred-address-types=InternalIP
`````

Passo 2: Salve e Feche o Editor
Ao fechar o editor, o Kubernetes aplicará automaticamente as alterações e reiniciará o Metrics Server com os novos argumentos.

Passo 3: Verifique o Status do Metrics Server
Aguarde alguns segundos para o Metrics Server reiniciar.
Verifique o status dele com:

```
kubectl get pods -n kube-system | grep metrics-server
```
Teste as Métricas
Quando o Metrics Server estiver em Running, use o comando kubectl top pod para verificar se as métricas estão disponíveis.

### Adicionando helm repo do apache-airflow e instalando Airflow
- Acessar ArtifactHub [ArtifactHUB](https://artifacthub.io/packages/search?ts_query_web=airflow&sort=relevance&page=1)

```helm repo add apache-airflow https://airflow.apache.org/```

``` helm upgrade --install airflow apache-airflow/airflow --namespace airflow --create-namespace ```

helm install airflow apache-airflow/airflow --version 1.12.0 --namespace airflow --create-namespace


### Verificando Deploy do Airflow:

```kubectl get pods -n airflow```


### Acesso a Interface Web do Airflow:
```kubectl port-forward svc/airflow-webserver 8080:8080 --namespace airflow```

Erros:
- forwarding port 8080... Connection refused...portforward.go:234] lost connection to pod
    - Correção:
        - Para fazer isso, edite o serviço (kubectl edit svc -n kubernetes-dashboard kubernetes-dashboard) e altere "ClusterIP" para "NodePort".
        - Em seguida, verifique seus serviços novamente para obter a porta do nó (kubectl get svc -n kubernetes-dashboard) e tente acessá-la em um navegador (https://<node IP>:<nodePort>).

Acessar http://localhost:8080:

    User: admin
    Password: admin

### Atualizando configuração do Airflow:
Depois de instalado o Airflow pelo Helm padrão, é possível personalizar a configuração.
A configuração padrão fornece um ponto de partida.

#### Extrair a configuração do Airflow em execução:
Este comando salvará os valores de configuração atuais em um arquivo values.yaml no local onde o comando foi executado

```helm show values apache-airflow/airflow > values.yaml```

#### Atualizar configuração Airflow:

###### Extrair configuração para edição:
```helm show values apache-airflow/airflow > airflow-values.yaml```

###### Novo deploy configuração:
```helm upgrade --install airflow apache-airflow/airflow -n airflow -f airflow-values.yaml```

###### Verificar status do deploy:
```helm ls -n airflow```
```kubectl get pods -n airflow```




### Criando cluster:

```kind create cluster --config cluster.yaml```

### Adicionando helm repo do spark

```helm repo add spark-operator https://googlecloudplatform.github.io/spark-on-k8s-operator```



### Criar namespace exclusivo para execução dos jobs
```kubectl create namespace spark-jobs```

### Instalando spark operator
```helm install spark-operator -f values.yaml spark-operator/spark-operator --namespace spark-operator --create-namespace```

### Executando o job de teste
```kubectl apply -f hello-world.yaml```
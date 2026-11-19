# P2---Simulado-Pratico-Integrado-de-Computacao-em-Nuvem

Instituição: Anhanguera
Disciplina: Computação em Nuvem
Professora: Simone Tatiane do Canto
Data: 18/11/2026
Aluno: Felipe Wodewotsky

# 1. Fundamentos da Computação em Nuvem
Questão 1 - Qual é a diferença entre Iaas e SaaS? Cite um exemplo de utilização de cada modelo.
Resposta:
Iaas é infrasctructure as a Service, significa oferecer infraestrutura para interessados, como servidores, data centers. Um exemplo disso seria a Amazon, que oferece servidores para empresas e individuos.
Saas é Software as a Service, significa oferecer um software como serviço em nuvem, conectando individuos através da internet. Um exemplo seria o microsoft 365 e google drive.

Questão 2 - Uma empresa utiliza servidores próprios e também recursos de um provedor de nuvem pública. Qual modelo de implantação em nuvem essa empresa utiliza?
Resposta: Seria possível utilizar uma Nuvem Hibrida, pois permite usar servidores proprios enquanto utiliza recursos de provedores em núvem publica, usando os dois ambientes de forma integrada.

Questão 3 - Qual a diferença entre uma máquina virtual e um contêiner?
Resposta:
Máquina virtual conta com um sistema operacional próprio, não sendo necessário seguir com o sistema da máquina que a está hospedando. Contêiner precisa utilizar o sistema operacional da maquina em que está hospedada e é normalmente mais leve e otimizada do que uma maquina virtual.

# 2. Execução da aplicação com Docker
Executar um servidor Nginx utilizando Docker, verificar seu funcionamento e personalizar a página web.
Evidência 1 - Contêiner em execução

docker run -d --name comunicado -p 8090:80 nginx:alpine
Unable to find image 'nginx:alpine' locally
alpine: Pulling from library/nginx
881fc932939e: Pull complete 
77a3bfea78f5: Pull complete 
04c3905a997c: Pull complete 
7cb374baf968: Pull complete 
4c8c172c6586: Pull complete 
1dc588c89da1: Pull complete 
61d8f4c85a74: Pull complete 
c4f3d97811f6: Pull complete 
Digest: sha256:0563c36f94a6d3ec3584161b77859f740f7e20b4853f52c796a864ce6908e8ea
Status: Downloaded newer image for nginx:alpine
326597ed122c5cc3992f813f487ecffb28da6d0fa6cc5b7acddd6aef0f4f97ec
root@controlplane:~$ docker ps
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                                     NAMES
326597ed122c   nginx:alpine   "/docker-entrypoint.…"   13 seconds ago   Up 12 seconds   0.0.0.0:8090->80/tcp, [::]:8090->80/tcp   comunicado

Evidência 2 - Página personalizada

 curl http://localhost:8090
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional 
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
root@controlplane:~$ docker exec comunicado sh -c 'echo "<h1>Comunicado interno disponivel</h1><p>Aluno: Felipe Wodewotsky</p>" > /usr/share/nginx/html/index.html'
root@controlplane:~$ curl http://localhost:8090
<h1>Comunicado interno disponivel</h1><p>Aluno: Felipe Wodewotsky</p>
root@controlplane:~$ 

# 3. Execução da Aplicação no Kubernetes
Criar um deployment e verificar os Pods gerenciados pelo Kubernetes.
Evidência 3 - Deployment e Pod

root@controlplane:~$ kubectl get nodes
NAME           STATUS   ROLES           AGE   VERSION
controlplane   Ready    control-plane   13d   v1.37.1
node01         Ready    <none>          13d   v1.37.1
root@controlplane:~$ kubectl create deployment comunicado-web --image=nginx:alpine
deployment.apps/comunicado-web created
root@controlplane:~$ kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
comunicado-web-57c76f7bc4-x74bl   1/1     Running   0          13s
root@controlplane:~$ 

# 4. Escalabilidade e Autorrecuperação
Aumentar a quantidade de réplicas e observar a recuperação automática após a exclusão de um Pod.
Evidência 4 - Escalabilidade

root@controlplane:~$ kubectl scale deployment comunicado-web --replicas=3
deployment.apps/comunicado-web scaled
root@controlplane:~$ kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
comunicado-web-57c76f7bc4-kft9k   1/1     Running   0          8s
comunicado-web-57c76f7bc4-vlkpq   1/1     Running   0          8s
comunicado-web-57c76f7bc4-x74bl   1/1     Running   0          73s
root@controlplane:~$ 

Evidência 5 - Autorrecuperação

root@controlplane:~$ kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
comunicado-web-57c76f7bc4-kft9k   1/1     Running   0          8s
comunicado-web-57c76f7bc4-vlkpq   1/1     Running   0          8s
comunicado-web-57c76f7bc4-x74bl   1/1     Running   0          73s
root@controlplane:~$ ^C
root@controlplane:~$ ^C
root@controlplane:~$ kubectl delete pod comunicado-web-57c76f7bc4-kft9k
pod "comunicado-web-57c76f7bc4-kft9k" deleted from default namespace
root@controlplane:~$ kubectl get pods
NAME                              READY   STATUS    RESTARTS   AGE
comunicado-web-57c76f7bc4-ffc9j   1/1     Running   0          10s
comunicado-web-57c76f7bc4-vlkpq   1/1     Running   0          89s
comunicado-web-57c76f7bc4-x74bl   1/1     Running   0          2m34s
root@controlplane:~$ ^C

O pod comunicado-web-57c76f7bc4-kft9k  foi deletado e subtituido

# 5. Disponibilização da Aplicação
Criar um Service e verificar a comunicação HTTP dentro do cluster Kubernetes.
Evidência 6 - Service e acesso HTTP

root@controlplane:~$ kubectl get pods 
NAME                              READY   STATUS    RESTARTS   AGE
comunicado-web-57c76f7bc4-ffc9j   1/1     Running   0          2m52s
comunicado-web-57c76f7bc4-vlkpq   1/1     Running   0          4m11s
comunicado-web-57c76f7bc4-x74bl   1/1     Running   0          5m16s
root@controlplane:~$ kubectl expose deployment comunicado-web --port=80 --target-port=80 --name=comunicado-service
service/comunicado-service exposed
root@controlplane:~$ kubectl get services
NAME                 TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)   AGE
comunicado-service   ClusterIP   10.108.142.126   <none>        80/TCP    9s
kubernetes           ClusterIP   10.96.0.1        <none>        443/TCP   13d
root@controlplane:~$ 

root@controlplane:~$ kubectl run teste --image=curlimages/curl --rm -it --restart=Never -- curl http://comunicado-service
<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
html { color-scheme: light dark; }
body { width: 35em; margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif; }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<p>If you see this page, nginx is successfully installed and working.
Further configuration is required for the web server, reverse proxy, 
API gateway, load balancer, content cache, or other features.</p>

<p>For online documentation and support please refer to
<a href="https://nginx.org/">nginx.org</a>.<br/>
To engage with the community please visit
<a href="https://community.nginx.org/">community.nginx.org</a>.<br/>
For enterprise grade support, professional services, additional 
security features and capabilities please refer to
<a href="https://f5.com/nginx">f5.com/nginx</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html>
All commands and output from this session will be recorded in container logs, including credentials and sensitive information passed through the command prompt.
If you don't see a command prompt, try pressing enter.
Session ended, resume using 'kubectl attach teste -c teste -n default -i -t' command
pod "teste" deleted from default namespace
root@controlplane:~$ 

Inserir resultado do teste HTTP e retorno da página nginx

# 6. Interpretação dos resultados
Questão 1 - Qual a diferença entre uma imagem Docker e um contêiner em execução?
Resposta: Uma imagem Docker é um modelo que contém os arquivos e configurações necessários para executar uma aplicação. Já o contêiner é uma instância dessa imagem em funcionamento, executando a aplicação de acordo com as configurações definidas.

Questão 2 - Qual a função de um Deployment no Kubernetes?
Resposta: O Deployment é responsável por gerenciar os Pods de uma aplicação, permitindo criar, atualizar e controlar a quantidade de réplicas em execução, garantindo que a aplicação continue funcionando conforme o esperado.

Questão 3 - O que aconteceu quando você excluiu um Pod? Por que isso aconteceu?
Resposta: Quando o Pod foi excluído, o Kubernetes identificou que a quantidade de réplicas estava abaixo do valor definido e criou automaticamente um novo Pod para substituí-lo. Isso acontece devido ao mecanismo de autorrecuperação do Kubernetes.

Questão 4 - Qual é a finalidade de um Service no Kubernetes?
Resposta: O Service permite a comunicação e o acesso aos Pods dentro do cluster Kubernetes, fornecendo um endereço estável para conectar-se à aplicação, mesmo quando os Pods são substituídos ou recriados.

# 7. Conclusão
Descreva brevemente o que você aprendeu durante a atividade prática, destacando a utilização do Docker e do Kubernetes.

Durante a atividade, aprendi a utilizar o Docker para executar contêineres e disponibilizar uma aplicação web utilizando o servidor Nginx. Também conheci o funcionamento do Kubernetes na criação e gerenciamento de aplicações. Foi possível testar a escalabilidade por meio da criação de réplicas e observar a autorrecuperação após a exclusão de um Pod. Por fim, compreendi como os Services permitem a comunicação entre os componentes dentro de um cluster.

# 8. Identificação de Entrega
Aluno: Felipe Wodewotsky
Repositório GitHub: https://github.com/Wodewotsky/P2---Simulado-Pratico-Integrado-de-Computa-o-em-Nuvem
Data da realização: 18/11/2026



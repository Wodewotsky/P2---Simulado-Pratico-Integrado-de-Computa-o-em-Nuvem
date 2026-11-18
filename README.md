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
Resposta: 

Questão 3 - Qual a diferença entre uma máquina virtual e um contêiner?
Resposta:
Máquina virtual conta com um sistema operacional próprio, não sendo necessário seguir com o sistema da máquina que a está hospedando. Contêiner precisa utilizar o sistema operacional da maquina em que está hospedada e é normalmente mais leve e otimizada do que uma maquina virtual.

# 2. Execução da aplicação com Docker
Executar um servidor Nginx utilizando Docker, verificar seu funcionamento e personalizar a página web.
Evidência 1 - Contêiner em execução


docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

Evidência 2 - Página personalizada

curl http://localhost:8090

Inserir mensagem com meu nome

# 3. Execução da Aplicação no Kubernetes
Criar um deployment e verificar os Pods gerenciados pelo Kubernetes.
Evidência 3 - Deployment e Pod

Kubectl get pods

inserir o pod criado

# 4. Escalabilidade e Autorrecuperação
Aumentar a quantidade de réplicas e observar a recuperação automática após a exclusão de um Pod.
Evidência 4 - Escalabilidade

kubectl scale deployment comunicado-web --replicas=3
kubectl get pods

Inserir tres pods da aplicação

Evidência 5 - Autorrecuperação

kubectl delete pod NOME_DO_POD
kubectl get pods

mostre o novo pod que foi criado após a exclusão

# 5. Disponibilização da Aplicação
Criar um Service e verificar a comunicação HTTP dentro do cluster Kubernetes.
Evidência 6 - Service e acesso HTTP

kubectl get services
kubectl run teste --image=curlimages/curl --rm -it --restart=Never --curl http://comunicado-service

Inserir resultado do teste HTTP e retorno da página nginx

# 6. Interpretação dos resultados
Questão 1 - Qual a diferença entre uma imagem Docker e um contêiner em execução?
Resposta:

Questão 2 - Qual a função de um Deployment no Kubernetes?
Resposta:

Questão 3 - O que aconteceu quando você excluiu um Pod? Por que isso aconteceu?
Resposta:

Questão 4 - Qual é a finalidade de um Service no Kubernetes?
Resposta:

# 7. Conclusão
Descreva brevemente o que você aprendeu durante a atividade prática, destacando a utilização do Docker e do Kubernetes.

escrever 3 a 5 linhas

# 8. Identificação de Entrega
Aluno: Felipe Wodewotsky
Repositório GitHub: https://github.com/Wodewotsky/P2---Simulado-Pratico-Integrado-de-Computa-o-em-Nuvem
Data da realização: 18/11/2026



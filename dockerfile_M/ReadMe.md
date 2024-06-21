# create new project with your username 
https://hpcharbor.mdanderson.edu/account/sign-in?redirect_url=%2Fharbor%2Fprojects

# jupyter/hub
http://hpcexhaproxy.mdanderson.edu/jupyter/hub/login?next=%2Fjupyter%2Fhub%2F

# kubectl handling
which kubectl
vim ~/bashrc
export PATH="/rsrch1/ip/msalehjahromi/.kube:$PATH"
(:wq in vim, source ~/.bashrc afterward)

# Build the docker image
docker build -t dinov2mori:msalehjahromi .

# Push the docker image to hpcharbor.mdanderson.edu
docker tag dinov2mori:msalehjahromi hpcharbor.mdanderson.edu/mori/dinov2mori:msalehjahromi
docker login hpcharbor.mdanderson.edu # login to the Harbor
docker push hpcharbor.mdanderson.edu/mori/dinov2mori:msalehjahromi

# Viewing your available kubectl contexts
kubectl config get-contexts

# Configure the K8S context
kubectl config use-context msalehjahromi_yn-gpu-workload@research-prd
kubectl config use-context msalehjahromi_yn-cpu-workload@research-prd

# Execute the K8S job    #I need to modify the code in K8s_.yaml
job-runner.sh /rsrch1/ip/msalehjahromi/codes/dinov2-main/dockerfile_M/K8s_.yaml

# Observing your GPU job(s)
kubectl get jobs -n yn-gpu-workload -l k8s-user=msalehjahromi
kubectl get pods -o wide -n yn-gpu-workload

# Deleting your GPU job(s)
kubectl delete job -n yn-gpu-workload msalehjahromi-gpu-job

is it in ? yes
metadata:
  name: msalehjahromi-gpu-job

# check pod detail: CHATGPT
kubectl describe pod msalehjahromi-gpu-job-gnpmq -n yn-gpu-workload




## DinoV2 problems

# cuml-cu11
By the way I could not install this : pip install cuml-cu11 --extra-index-url https://pypi.nvidia.com

Shall I try it on our GPUs?


# ####################################
Hi Yang,
I am currently working on a project that requires the NVIDIA CUDA Development Toolkit. While I can run GPU-accelerated tasks and nvidia-smi works, the nvcc compiler is not available on my JupyterHub

You have to build your image and use the custom to run your container.Please make sure you have the following code in your dockerfile so Jupyter Hub can run the image. Thank you.

USER root
RUN apt-key del 7fa2af80 && \
    apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub && \
    apt-get update -y && \
    DEBIAN_FRONTEND=noninteractive \
    apt-get install -y --no-install-recommends \
    python3 \
    python3-pip \
    python3-venv
# ####################################
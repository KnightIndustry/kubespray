#########
感谢开源
感谢kubernetes
感谢kubespray
感谢ansible
本文以kubespray master v2.13.2 为基础，做了offline 调试
ansible 变量有多重优先级，文中多次调试使用最低优先级变量
#########
docker run -d -v /opt/registry:/var/lib/registry -p 5000:5000 --restart=always --name registry registry:latest

declare -a IPS=(172.16.51.11 172.16.51.12)
CONFIG_FILE=inventory/sample/hosts.yaml python3 contrib/inventory_builder/inventory.py ${IPS[@]}


不重置hostname 
   roles/bootstrap-os/defaults/main.yml


####
----------
inventory/sample/group_vars/all/all.yml
----------

yum_repo: "http://mirrors.aliyun.com/"
docker_rh_repo_base_url: '{{ yum_repo }}/docker-ce/linux/centos/7/$basearch/stable'
docker_rh_repo_gpgkey: "{{ yum_repo }}/docker-ce/linux/centos/gpg"
extras_rh_repo_base_url: "{{  yum_repo}}/centos/$releasever/extras/$basearch"
extras_rh_repo_gpgkey: "{{ yum_repo }}/containerd/gpg"
files_repo: "http://172.16.51.2"
kubeadm_download_url: "{{ files_repo }}/kubernetes/{{ kube_version }}/{{ image_arch }}/kubeadm"
kubectl_download_url: "{{ files_repo }}/kubernetes/{{ kube_version }}/{{ image_arch }}/kubectl"
kubelet_download_url: "{{ files_repo }}/kubernetes/{{ kube_version }}/{{ image_arch }}/kubelet"
etcd_download_url: "{{ files_repo }}/kubernetes/etcd/etcd-{{ etcd_version }}-linux-amd64.tar.gz"
cni_download_url: "{{ files_repo }}/kubernetes/cni/cni-plugins-linux-{{ image_arch }}-{{ cni_version }}.tgz"
crictl_download_url: "{{ files_repo }}/kubernetes/cri-tools/crictl-{{ crictl_version }}-{{ ansible_system | lower }}-{{ image_arch }}.tar.gz"
# If using Calico
calicoctl_download_url: "{{ files_repo }}/kubernetes/calico/{{ calico_ctl_version }}/calicoctl-linux-{{ image_arch }}"

# gcr and kubernetes image repo define
registry_host: "172.16.51.2:5000/kubespray"
docker_image_repo: "{{ registry_host }}"
quay_image_repo: "{{ registry_host }}"


----------
inventory/sample/group_vars/all/docker.yml
----------
docker_insecure_registries:
#   - mirror.registry.io
  - 172.16.51.2:5000

docker_registry_mirrors:
  - https://registry.docker-cn.com
  - https://mirror.aliyuncs.com




----------
inventory/sample/group_vars/k8s-cluster/k8s-cluster.yml
----------
kube_image_repo: "172.16.51.2:5000/kubespray/google_containers"


##########
http://172.16.51.2/kubernetes/v1.18.6/amd64/kubeadm
http://172.16.51.2/kubernetes/v1.18.6/amd64/kubectl
http://172.16.51.2/kubernetes/v1.18.6/amd64/kubelet
http://172.16.51.2/kubernetes/v1.18.6/amd64/kube-apiserver
http://172.16.51.2/kubernetes/v1.18.6/amd64/kube-controller-manager
http://172.16.51.2/kubernetes/v1.18.6/amd64/kube-proxy
http://172.16.51.2/kubernetes/v1.18.6/amd64/kube-scheduler

http://172.16.51.2/kubernetes/v1.18.6/calicoctl
http://172.16.51.2/kubernetes/v1.18.6/calicoctl-linux-amd64

http://172.16.51.2/kubernetes/etcd/etcd-v3.4.3-linux-amd64.tar.gz

http://172.16.51.2/kubernetes/calico/v3.15.1/calicoctl-linux-amd64

http://172.16.51.2/kubernetes/cni/cni-plugins-linux-amd64-v0.8.6.tgz

----------
172.16.51.2:5000/kubespray/google_containers/kube-proxy:v1.18.6
172.16.51.2:5000/kubespray/google_containers/kube-apiserver:v1.18.6
172.16.51.2:5000/kubespray/google_containers/kube-controller-manager:v1.18.6
172.16.51.2:5000/kubespray/google_containers/kube-scheduler:v1.18.6
172.16.51.2:5000/kubespray/library/nginx:1.19
172.16.51.2:5000/kubespray/calico/node:v3.15.1
172.16.51.2:5000/kubespray/calico/cni:v3.15.1
172.16.51.2:5000/kubespray/calico/kube-controllers:v3.15.1
172.16.51.2:5000/kubespray/kubernetesui/dashboard-amd64:v2.0.2
172.16.51.2:5000/kubespray/google_containers/cluster-proportional-autoscaler-amd64:1.8.1
172.16.51.2:5000/kubespray/kubernetesui/metrics-scraper:v1.0.5
172.16.51.2:5000/kubespray/google_containers/k8s-dns-node-cache:1.15.13
172.16.51.2:5000/kubespray/kubernetes-ingress-controller/nginx-ingress-controller:0.32.0
172.16.51.2:5000/kubespray/google_containers/pause:3.2
172.16.51.2:5000/kubespray/coredns/coredns:1.6.7

----------


####

默认镜像
docker.io/calico/kube-controllers:v3.15.1
k8s.gcr.io/pause:3.2
docker.io/library/nginx:1.19
docker.io/coredns/coredns:1.6.7
k8s.gcr.io/k8s-dns-node-cache:1.15.13
k8s.gcr.io/cluster-proportional-autoscaler-amd64:1.8.1
docker.io/kubernetesui/dashboard-amd64:v2.0.2
docker.io/kubernetesui/metrics-scraper:v1.0.5
k8s.gcr.io/kube-apiserver:v1.18.6
k8s.gcr.io/kube-controller-manager:v1.18.6
k8s.gcr.io/kube-scheduler:v1.18.6
k8s.gcr.io/kube-proxy:v1.18.6



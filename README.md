# awx_on_k8s
- preq
    Install tools
    sudo apt install git build-essential curl jq  -y

- clone awx operator
    git clone https://github.com/ansible/awx-operator.git

- create ns
    export NAMESPACE=awx
    kubectl create ns ${NAMESPACE}

- set release tag
    RELEASE_TAG=`curl -s https://api.github.com/repos/ansible/awx-operator/releases/latest | grep tag_name | cut -d '"' -f 4`
    echo $RELEASE_TAG
    git checkout $RELEASE_TAG

- deploy
    export NAMESPACE=awx
    make deploy

- create pvc
    kubectl apply -f pvc.yaml

- Create cluster deployment 
    kubectl apply -f deployment.yaml

- Get admin password    
    kubectl get secret awx-admin-password -o go-template='{{range $k,$v := .data}}{{printf "%s: " $k}}{{if not $v}}{{$v}}{{else}}{{$v | base64decode}}{{end}}{{"\n"}}{{end}}'

- Enjoy
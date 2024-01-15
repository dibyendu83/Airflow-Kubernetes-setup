# Set up Airflow locally in Kubernetes ( Windows OS)
This section describes how to set up airflow in Kubernetes locally

## Quickstart Guide
Clone the repository from  git hub ``` git clone https://github.com/dibyendu83/Airflow-Kubernetes-setup.git ```

### Step 1 - Prepare your Environment
* For set up Airflow in Kubernetes, need to first install docker. 
    * Install docker from [docker-desktop](https://www.docker.com/products/docker-desktop/).
    * Enable Kubernetes, from Kubernetes tab after installing docker desktop.
    * Install [Helm](https://www.cloudsigma.com/installing-software-on-kubernetes-with-helm-3-package-manager-on-windows/).

### Step 2 - Add the Helm Repository
* Add helm repository ```helm repo add airflow-stable https://airflow-helm.github.io/charts```
* Update helm repo cache ```helm repo update```
* Check recently added repository ```helm repo ls```

### Step 3 - Create Persistent Volume (PV) for DAG and log
* Modify the dag folder location in ```setup/pv-dag.yml```. Currently```c/personal/learning/airflow/dags```, it should point your dag folder.
* Modify same for log file generation ```setup/pv-log.yml```.
* Once modified apply those changes ```kubectl apply -f setup/pv-dag.yml``` ```kubectl apply -f setup/pv-log.yml```
* Check the two pv(s) ```kubectl get pv```

### Step 4 - Create Persistent Volume Claim (PVC) for DAG and log
* Create the airflow namespace in Kubernetes ```kubectl create ns airflow```.
* Create pvc ```kubectl apply -f setup/pvc-dag.yaml``` ```kubectl apply -f setup/pvc-log.yaml```
* Check the two pvc(s) created in airflow namespace ```kubectl get pvs -n airflow```

### Step 5 - Create your Custom Values File
* Modify ```setup/values-KubernetesExecutor.yaml``` if you want to change any properties
* Already added dag and log pvc there.

### Step 6 - Install the Airflow Chart
* Following command may take a while so wait until the command returns and resources become ready 
    * ```helm install airflow airflow-stable/airflow --namespace airflow --values setup/values-KubernetesExecutor.yaml```
    * checked airflow installed or not ? ```kubectl get all -n airflow```. All pods and services should be running.

### Step 7 - Access the Airflow UI
* open command prompt and type this command ```kubectl port-forward service/airflow-web 8080:8080 -n airflow```
* open your browser to: http://localhost:8080
* default login: `admin`/`admin`

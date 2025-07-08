ETCD (엣시디) 서버는 쿠버네티스 클러스터의 상태 정보를 저장합니다. 클러스터에 변화가 생길 때마다, 관련 정보가 변경 및 저장 됩니다. 클러스터 상태 변화 작업이 성공적으로 마무리 됐을 때, 정보가 업데이트 됩니다.

etcd 서버를 배포하는 방법은 2가지가 있습니다. 

1) Manual(수동)으로 배포하는 방법
2) 클러스터 생성으로, 자동으로 배포 시키는 방법 (kubeadm)

'매뉴얼 배포 방법'은 etcd 깃허브 저장소에서 binary를 직접 다운 받아 배포합니다.

```bash
wget -q --https-only \
"https://github.com/coreos/etcd/releases/download/v3.3.9/etcd-v3.3.9-linux-amd64.tar.gz"
```

- etcd 서버 배포 시 다양한 파마미터가 지정돼야 합니다. 그 중 특히 중요한 것은 etcd가 Listen하는 주소와 포트 정보입니다. 
- `advertise-client-urls https:${INTERNAL_IP}:2379`
- 해당 url은 kube-apiserver에도 설정이 돼야 합니다.

kubeadm을 통해 클러스터 생성 시, etcd는 자동으로 배포됩니다. 이 경우, etcd는 pod 형태로 `kube-system` name-space에 배포됩니다. etcd 파드 정보를 확인하는 명령은 다음과 같습니다.

```bash
kubectl get pods -n kube-system
```

etcd 관련 자세한 정보는 다음과 같이 확인할 수 있습니다.

```bash
kubectl exec etcd-master -n kubec-system etcdctl get / --prefix -keys-only
```

- etcd는 `Registry`라는 Root 디렉터리를 가지면, 그 하위에 다양한 디렉토리가 존재합니다.
	- `Registry`
		- `minions`
		- `pods`
		- `replicasets`
		- `deployments`
		- `roles`
		- `secrets`
		- ...

고가용성(High Availability) 쿠버네티스 클러스터일 경우, 마스터 노드의 개수는 여러 개(1개, 3개, 5개, ...) 입니다. etcd 서버 개수도 마스터 노드와 동일하게 구성됩니다. etcd 서버 파라미터 설정 시, 각 etcd가 다른 etcd들을 인식할 수 있도록 설정해야 합니다.

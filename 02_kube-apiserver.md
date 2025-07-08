kube-apiserver는 쿠버네티스 내 모든 구성 요소와 직접 상호작용하는 유일한 구성요소입니다. 사용자가 쿠버네티스에 작업(예를 들어, Pod 생성) 수행 시, 1) 사용자는 kube-apiserver로 API 요청을 보냅니다. 2) kube-apiserver는 사용자를 인증하고, 3) 수신한 API 요청에 대해 유효성 검사를 수행합니다. 4) etcd에서 요청 관련 정보를 검색합니다. 5) etcd 내 정보를 업데이트 합니다. 6) 해당 작업을 kube-schedualr에게 전달하여, 작업을 스케쥴링합니다. 7) kubelet에게 지시하여, 해당 작업을 실제로 수행하게 합니다.

kube-apiserver를 배포하는 방법은 2가지 입니다.

1) Manual(수동) 배포
2) 클러스터 생성으로 자동 배포(kubeadm)

매뉴얼 배포 시, kube-apiserver를 깃허브 저장소에서 관련 패키지를 다운로드 받아 직접 배포합니다. 예시는 다음과 같습니다.

```bash
wget https://storage.goolgepis.com/kubernetes-release/v1.13.0/bin/linux/amd64/kube-apiserver
```

패키지 다운로드 후 배포 시 여러 파라미터를 지정합니다. 그 중 특히 중요한 것은 etcd 서버의 IP주소와 포트 정보입니다. `--etcd-servers=https://127.0.0.1:2379`


`kubeadm`으로 클러스터 생성 시, 자동으로 kube-apiserver가 `kube-system` 네임스페이스에 파드로 배포됩니다. 다음의 명령으로 조회가 가능합니다.

- `kubectl get pods -n kube-system`

kube-apiserver의 관련 정보는 다음과 같이 확인할 수 있습니다.

- manifest : `cat /etc/kubernetes/manifests/kube-apiserver.yaml`
- service : `cat /etc/systemd/system/kube-apiserver.service`
- process : `ps -aux | grep kube-apiserver`

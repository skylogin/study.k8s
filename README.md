# study

## create pod
kubectl apply -f simple-pod.yaml

## pod list
kubectl get pod

## inside container (kubectl exec -it [pod name] sh -c [container name])
kubectl exec -it simple-echo sh -c nginx

## view log
kubectl logs -f simple-echo -c echo

## delete pod
kubectl delete pod simple-echo
## or
kubectl delete -f simple-pod.yaml

---

## create replica
kubectl apply -f simple-replicaset.yaml

## delete replica
kubectl delete -f simple-replicaset.yaml


---

## create deployment with record
kubectl apply -f simple-deployment.yaml --record

## check
kubectl rollout history deployment echo

## list (pod, replicaset, deployment)
kubectl get pod,replicaset,deployment --selector app=echo

## view revision 
kubectl rollout history deployment echo --revision=1

## revision rollback
kubectl rollout undo deployment echo

## delete deployment
kubectl delete -f simple-deployment.yaml

---

deployment - replicaset - pod,pod,pod...

---

## create replicaset w/ label
kubectl apply -f simple-replicaset-with-label.yaml

## list (pod)
kubectl get pod -l app=echo -l release=spring
kubectl get pod -l app=echo -l release=summer

## create service (summer)
kubectl apply -f simple-service.yaml

## list (service)
kubectl get svc echo

## temporary debug session 
kubectl run -i --rm --tty debug --image=gihyodocker/fundamental:0.1.0 --restart=Never -- bash -il
## then execute 10 times to [curl http://echo/] and then [exit]
## and check a log (summer는 로그가 남고 spring은 남지않음. pod명은 kubectl get pod -l app=echo -l release=summer 등으로 확인)
kubectl logs -f echo-summer-9wbkw -c echo
kubectl logs -f echo-summer-pmnzh -c echo
kubectl logs -f echo-spring-4rvrp -c echo

---

## create nodePort service
kubectl apply -f simple-nodeport.yaml

## list (여기에 노출된 포트로 외부에서 접속이 가능함) 
kubectl get svc echo

## check service
curl http://127.0.0.1:32082

---

## ingress (nginx_ingress_controller)
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.16.2/deploy/mandatory.yaml
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/nginx-0.16.2/deploy/provider/cloud-generic.yaml

## check
kubectl -n ingress-nginx get service,pod

## service reload
kubectl apply -f simple-service.yaml

## start ingress
kubectl apply -f simple-ingress.yaml

## test
curl http://localhost -H 'Host: ch05.jung.local'

curl http://localhost -H 'Host: ch05.jung.local' \
  -H 'User-Agent: Mozilla/5.0 (iPhone; CPU iPhone OS 11_0 like Mac OS X)a AppleWebkit/604.1.38 (KHTML, like Gecko) Version/11.0 Mobile/15A372 Safari/604.1'

---

## create job pod
kubectl apply -f simple-job.yaml

## view log
kubectl logs -l app=pingpong

## view pod list
kubectl get pod -l app=pingpong


---

## create cron pod
kubectl apply -f simple-cronjob.yaml

## check
kubectl get job -l app=pingpong

## view log
kubectl logs -l app=pingpong
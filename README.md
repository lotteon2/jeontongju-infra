# K8S Command

## Namespace 적용 (default)

```
kubectl ns default
```

## EFS 마운트 확인

```
df -hT --type nfs4
```

## Metric server 설치

```
kubectl apply -f https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
```

## kubectl apply 환경 변수 적용

```
REPOSITORY_URI=<your repository uri>
REPOSITORY_URI=$REPOSITORY_URI envsubst < deployment.yaml | kubectl apply -f -
```

## AWS Load Balancer Controller 설치

```
helm repo add eks https://aws.github.io/eks-charts

helm repo update

helm install aws-load-balancer-controller eks/aws-load-balancer-controller -n kube-system --set clusterName=$CLUSTER_NAME \
  --set serviceAccount.create=false \
  --set serviceAccount.name=aws-load-balancer-controller
```

## ExternalDNS 설치

```
MyDomain=<your domain>

// HostedZone 여러 개일 경우 맞춰서 선택
MyDnsHostedZoneId=$(aws route53 list-hosted-zones-by-name --dns-name "${MyDomain}." --query "HostedZones[0].Id" --output text)

echo $MyDomain, $MyDnsHostedZoneId

curl -s -O https://raw.githubusercontent.com/cloudneta/cnaeblab/master/_data/externaldns.yaml

MyDomain=$MyDomain MyDnsHostedZoneId=$MyDnsHostedZoneId envsubst < externaldns.yaml | kubectl apply -f -
```

## AWS Cert Manager 인증서 확인

```
// ACM 인증서 확인
aws acm list-certificates

// ACM 인증서 변수 선언
// 여러 개일 경우 맞춰서 선택
CERT_ARN=`aws acm list-certificates --query 'CertificateSummaryList[].CertificateArn[]' --output text`; echo $CERT_ARN
```

### s3 버킷생성 ###
milvus 용 데이터 저장 버킷을 생성한다.
```
export CLUSTER_NAME=eks-agentic-ai
export VECTORDB_BUCKET_NAME=${CLUSTER_NAME}-vectordb-milvus

aws s3 mb s3://${VECTORDB_BUCKET_NAME} --region ap-northeast-2
```

### milvus 설치 ###
eks 클러스터에 milvus 를 설치한다.
```
helm repo add milvus https://zilliz.github.io/milvus-helm/
helm repo update

helm install milvus milvus/milvus \
  --set cluster.enabled=false \
  --set externalS3.enabled=true \
  --set externalS3.host=s3.amazonaws.com \
  --set externalS3.bucketName=${VECTORDB_BUCKET_NAME} \
  --set externalS3.useIAM=true \
  --set minio.enabled=false \
  --set pulsar.enabled=false \
  --set milvus.standalone.messageQueue=rocksmq \
  -n milvus --create-namespace
```

### IRSA ##
```
EKS에서 useIAM=true 쓰려면
IRSA 설정이 되어 있어야 합니다:

OIDC Provider가 EKS 클러스터에 연결되어 있어야 하고
S3 접근 권한이 있는 IAM Role을 만들고
해당 Role을 Milvus ServiceAccount에 annotate

kubectl annotate serviceaccount milvus \
  -n milvus \
  eks.amazonaws.com/role-arn=arn:aws:iam::<ACCOUNT_ID>:role/<ROLE_NAME>
```


# 07 — AWS Cost Control

## Objective

Keep the lab cost-controlled when it is not being used.

EKS environments can create costs through:

- worker nodes
- load balancers
- NAT gateways
- EBS volumes
- Elastic IPs
- other provisioned AWS resources

## Before Stopping the Lab

Check:

```bash
kubectl get pods
kubectl get svc
kubectl get ingress
```

Record the resources you created.

## Remove Application Resources

For a disposable lab:

```bash
kubectl delete -f k8s/
```

Or remove resources individually.

## Verify Load Balancers

```bash
kubectl get ingress
kubectl get svc
```

Then check AWS for remaining load balancers.

## Delete the EKS Cluster

If the cluster was created using `eksctl`, use the corresponding delete operation for the exact cluster:

```bash
eksctl delete cluster --name <cluster-name> --region <region>
```

Do not run a delete command until the cluster name and region have been verified.

## Check for Leftover Resources

After deletion, verify:

- EC2 instances
- EKS clusters/node groups
- Load Balancers
- NAT Gateways
- Elastic IPs
- EBS volumes
- CloudFormation stacks
- CloudWatch resources

## Restarting Tomorrow

The safest workflow is:

```text
Start AWS environment
      ↓
Verify EKS
      ↓
Verify kubectl context
      ↓
Verify pods
      ↓
Verify service
      ↓
Verify ingress/ALB
      ↓
Test application
      ↓
Test Splunk ingestion
```

Avoid leaving the environment running overnight when it is not required.

## Important

Stopping an EC2 instance does not necessarily eliminate every AWS charge associated with the environment. Always verify the actual AWS resources that remain provisioned.

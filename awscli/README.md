# aws cli

## security groups

exclude tags from the output
```shell
aws ec2 describe-security-groups --region eu-west-2 \
  --query 'SecurityGroups[*].{Id:GroupId,Name:GroupName,VpcId:VpcId,Ingress:IpPermissions,Egress:IpPermissionsEgress}'
```

security group network interfaces
```shell
aws ec2 describe-network-interfaces --region eu-west-2 \
  --filters "Name=group-id,Values=sg-123456" "Name=status,Values=in-use" \
  --query 'NetworkInterfaces[*].{Description:Description,Id:NetworkInterfaceId,VpcId:VpcId,RequesterManaged:RequesterManaged,RequesterId:RequesterId,Type:InterfaceType,Attachment:Attachment,OwnerId:OwnerId,SubnetId:SubnetId}'
```

## network interfaces

list instance IDs that have ENI with the specified security group attached (`[?not_null]` client filter)
```shell
aws ec2 describe-network-interfaces --region eu-west-2 \
  --filters "Name=group-id,Values=sg-123456" "Name=status,Values=in-use" \
  --query 'NetworkInterfaces[?not_null(Attachment.InstanceId)].Attachment.InstanceId' | jq -r '.[]' | uniq
```

list ENIs with the specified security that are not attached to EC2 instances (`[?!not_null]` client filter)
```shell
aws ec2 describe-network-interfaces --region eu-west-2 \
  --filters "Name=group-id,Values=sg-123456" "Name=status,Values=in-use" \
  --query 'NetworkInterfaces[?!not_null(Attachment.InstanceId)].{Description:Description,Id:NetworkInterfaceId,VpcId:VpcId,RequesterManaged:RequesterManaged,RequesterId:RequesterId,Type:InterfaceType,Attachment:Attachment,OwnerId:OwnerId,SubnetId:SubnetId}'
```

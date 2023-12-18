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

## vpc flow logs

VPC flow logs are grouped by ENI. If the flow direction is ingress, destination address and destination port belong
to the ENI that produced the logs. If the flow direction is egress, they would be source address and source port.
```
+---- eni xyz ----+
|                 |
| +--------------------------------------+
| | +- ingress -+          +-----------+ |
| | | dst Addr  |<---------| src Addr  | |
| | | dst Port  |          | src Port  | |
| | +-----------+          +-----------+ |
| +--------------------------------------+
|                 |
| +--------------------------------------+
| | +- egress --+          +-----------+ |
| | | src Addr  |--------->| dst Addr  | |
| | | src Port  |          | dst Port  | |
| | +-----------+          +-----------+ |
| +--------------------------------------+
+-----------------|
```

### Examples

Following example queries can be run in logs insights.

- VPC egress traffic
```
fields @timestamp, interfaceId, dstAddr, dstPort, flowDirection, tcpFlags, action
| filter (logStatus != "NODATA")
| filter (flowDirection == "egress")
| sort @timestamp desc
| limit 50
```

- VPC ingress traffic
```
fields @timestamp, interfaceId, srcAddr, srcPort, flowDirection, tcpFlags, action
| filter (logStatus != "NODATA")
| filter (flowDirection == "ingress")
| sort @timestamp desc
| limit 50
```

- ENI ingress ports (replace `interfaceId`)
```
fields @timestamp, interfaceId, srcAddr, srcPort, flowDirection, tcpFlags, action
| filter (logStatus != "NODATA")
| filter (flowDirection == "ingress")
| filter (interfaceId == "eni-0aaffaaffaaffaaff")
| dedup srcPort
| limit 50
```

- [more sample queries](https://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_QuerySyntax-examples.html)

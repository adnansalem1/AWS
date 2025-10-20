# CloudFormation EC2 Beispiel

Dieses Repository enthält ein einfaches AWS CloudFormation-Template (`template.yaml`) zum Starten einer EC2-Instanz.

Kurzüberblick
- AMI: `ami-03026aa8227f2f9c3`
- Security Group: `sg-0be14659a6379fe7a` (muss in der Region existieren)
- Instance Type: `t2.micro`
- Ziel-Region: `eu-west-1` (Irland)

Wichtiger Hinweis
- CloudFormation-Stacks sind regionspezifisch. Stelle sicher, dass die angegebene Security Group in `eu-west-1` vorhanden ist.
- Die AMI-ID ist regionspezifisch. Die hier genutzte AMI muss in `eu-west-1` verfügbar sein.

Schnelle Befehle

1) Template validieren (lokal mit AWS CLI):

```bash
aws cloudformation validate-template --template-body file://template.yaml --region eu-west-1
```

2) Stack erstellen / deployen:

```bash
aws cloudformation deploy \
  --template-file template.yaml \
  --stack-name my-ec2-stack \
  --region eu-west-1 \
  --capabilities CAPABILITY_IAM \
  --parameter-overrides SubnetId=""
```

Wenn du eine spezifische SubnetId angeben möchtest, ersetze `SubnetId=""` durch z.B. `SubnetId="subnet-0123456789abcdef0"`.

3) Stack-Outputs ansehen (über AWS CLI):

```bash
aws cloudformation describe-stacks --stack-name my-ec2-stack --region eu-west-1 --query "Stacks[0].Outputs" --output table
```

4) Stack löschen:

```bash
aws cloudformation delete-stack --stack-name my-ec2-stack --region eu-west-1
```

Fehlerbehebung / Hinweise
- Falls der Stack beim Erstellen wegen fehlender Ressourcen fehlschlägt, überprüfe die Availability Zone / SubnetId und ob die Security Group in der selben VPC existiert.
- Für SSH-Zugriff musst du ggf. ein KeyPair angeben (das Template nutzt standardmäßig kein KeyPair). Du kannst das Template erweitern, um ein KeyName-Parameter hinzuzufügen.

Lizenz / Haftungsausschluss
Dieses Template ist ein einfaches Beispiel. Prüfe alle IDs (AMI, Security Group, Subnet) bevor du es in einer Produktionsumgebung benutzt.

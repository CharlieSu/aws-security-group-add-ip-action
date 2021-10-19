# AWS Security Group Add IP Action

This action will add your public ip address to your given aws security group(s) with a description.
If any ip address already exists with the description then it will update the address instead of adding.
And it will remove the added ip address once the main job is completed.

## Inputs

### `aws-region`

**Required** AWS Region.

### `aws-security-group-id`

**Required** AWS Security Group (comma separated if multiple).

### `protocol`

The protocol which you want to allow. Default `"tcp"`.

### `port`

The port which you want to allow. Default `"22"`.

### `to-port`

The to port which you want to allow. Default `""`, leave it empty to allow only one port.

### `description`

The descriptipn of your IP permission. Default `"GitHub Action"`.

## AWS IAM Policy

Make sure the IAM user that you have configured has the following policy
```
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Sid": "UpdateIngress",
            "Effect": "Allow",
            "Action": [
                "ec2:RevokeSecurityGroupIngress",
                "ec2:AuthorizeSecurityGroupIngress"
            ],
            "Resource": "arn:aws:ec2:your-region:your-account-id:security-group/your-security-group-id"
        },
        {
            "Sid": "DescribeGroups",
            "Effect": "Allow",
            "Action": "ec2:DescribeSecurityGroups",
            "Resource": "*"
        }
    ]
}
```
Replace `your-region`, `your-account-id` and `your-security-group-id` with appropiate values.

## Example usage
```yaml
- name: Configure AWS Credentials
  uses: aws-actions/configure-aws-credentials@master
  
  # Plugin assumes credentials were configured in the previous step
- name: Add public IP to AWS security group
  uses: CharlieSu/aws-security-group-add-ip-action@main
  with:
    aws-region: 'us-east-1'
    aws-security-group-id: ${{ secrets.AWS_SECURITY_GROUP_ID }}
    port: '22'
    to-port: '30'
    protocol: 'tcp'
    description: 'GitHub Action'
```

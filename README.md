## Project Overview: AWS CloudFormation CI/CD

### Overview:
This CloudFormation template sets up a CI/CD infrastructure on AWS using various AWS services like EC2, S3, Jenkins, Nexus, SonarQube, and RDS. It automates the deployment process for software projects, ensuring efficiency and reliability in the development lifecycle.

### Parameters:
- **KeyPair:**
  - *Description:* Amazon EC2 Key Pair
  - *Type:* "AWS::EC2::KeyPair::KeyName"
  
- **MyIP:**
  - *Description:* Assigning IP
  - *Type:* String
  - *Default:* 183.83.39.195/32

### Resources:
- **S3RoleforCiCd:**
  - *Type:* AWS::CloudFormation::Stack
  - *Properties:*
    - *TemplateURL:* [cicds3role.yaml](https://s3.amazonaws.com/vpro-cicd-templates/stack-template/cicds3role.yaml)

- **JenkinsInst:**
  - *Type:* AWS::CloudFormation::Stack
  - *DependsOn:* S3RoleforCiCd
  - *Properties:*
    - *TemplateURL:* [jenk.yaml](https://s3.amazonaws.com/vpro-cicd-templates/stack-template/jenk.yaml)
    - *Parameters:*
      - *KeyName:* !Ref KeyPair
      - *MyIP:* !Ref MyIP

- **App01qa:**
  - *Type:* AWS::CloudFormation::Stack
  - *DependsOn:* JenkinsInst
  - *Properties:*
    - *TemplateURL:* [app01qa.yaml](https://s3.amazonaws.com/vpro-cicd-templates/stack-template/app01qa.yaml)
    - *Parameters:*
      - *KeyName:* !Ref KeyPair
      - *MyIP:* !Ref MyIP

- **NexusServer:**
  - *Type:* AWS::CloudFormation::Stack
  - *DependsOn:* JenkinsInst
  - *Properties:*
    - *TemplateURL:* [nexus.yaml](https://s3.amazonaws.com/vpro-cicd-templates/stack-template/nexus.yaml)
    - *Parameters:*
      - *KeyName:* !Ref KeyPair
      - *MyIP:* !Ref MyIP

- **SonarServer:**
  - *Type:* AWS::CloudFormation::Stack
  - *DependsOn:* JenkinsInst
  - *Properties:*
    - *TemplateURL:* [sonar.yaml](https://s3.amazonaws.com/vpro-cicd-templates/stack-template/sonar.yaml)
    - *Parameters:*
      - *KeyName:* !Ref KeyPair
      - *MyIP:* !Ref MyIP

- **db01qa:**
  - *Type:* AWS::CloudFormation::Stack
  - *DependsOn:* App01qa
  - *Properties:*
    - *TemplateURL:* [db01qa.yaml](https://s3.amazonaws.com/vpro-cicd-templates/stack-template/db01qa.yaml)
    - *Parameters:*
      - *KeyName:* !Ref KeyPair
      - *MyIP:* !Ref MyIP

- **WintestServer:**
  - *Type:* AWS::CloudFormation::Stack
  - *DependsOn:* JenkinsInst
  - *Properties:*
    - *TemplateURL:* [wintest.yaml](https://s3.amazonaws.com/vpro-cicd-templates/stack-template/wintest.yaml)
    - *Parameters:*
      - *KeyName:* !Ref KeyPair
      - *MyIP:* !Ref MyIP

### Instructions:
1. Ensure you have an existing Amazon EC2 Key Pair.
2. Define the MyIP parameter with the appropriate IP address or range.
3. Deploy this CloudFormation template in your AWS account.
4. Once deployed, you can access the various components of the CI/CD infrastructure such as Jenkins, Nexus, SonarQube, and database instances.
5. Customize the setup further based on your project requirements.

### Notes:
- This template assumes basic knowledge of AWS services and CloudFormation.
- Make sure to review the resources and parameters before deployment to ensure they meet your project needs.
- Additional customization can be done by modifying the CloudFormation templates linked in the resources section.

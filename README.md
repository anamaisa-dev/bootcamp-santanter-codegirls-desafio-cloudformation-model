
# Implementando Infraestrutura Automatizada com AWS CloudFormation

> Documentação prática do laboratório AWS CloudFormation - Formação AWS Cloud Foundations

---

## Índice

- [Sobre o Projeto](#sobre-o-projeto)
- [O que é AWS CloudFormation](#o-que-é-aws-cloudformation)
- [Estrutura de Templates](#estrutura-de-templates)
- [Exemplos Práticos](#exemplos-práticos)
- [Aprendizados e Insights](#aprendizados-e-insights)
- [Como Usar](#como-usar)
- [Conclusão](#conclusão)

---

## Sobre o Projeto

Este repositório documenta minha jornada de aprendizado sobre **AWS CloudFormation**, serviço de infraestrutura como código (IaC) da AWS. O objetivo é consolidar conhecimentos práticos sobre automação de infraestrutura na nuvem.

**Competências Desenvolvidas:**
- Criação de templates CloudFormation em YAML/JSON
- Provisionamento automatizado de recursos AWS
- Gerenciamento de stacks (criação, atualização, exclusão)
- Documentação técnica estruturada
- Versionamento de infraestrutura

---

## O que é AWS CloudFormation

O AWS CloudFormation permite modelar e provisionar recursos AWS de forma automatizada através de templates declarativos. 

**Principais Benefícios:**
- **Infraestrutura como Código:** Versionamento e reusabilidade
- **Automação completa:** Criação, atualização e remoção de recursos em lote
- **Consistência:** Ambientes idênticos entre dev, teste e produção
- **Rollback automático:** Reversão de mudanças em caso de falhas
- **Auditoria:** Rastreamento de todas as alterações

---

## Estrutura de Templates

### Seções Principais

Um template CloudFormation é composto por:

1. **AWSTemplateFormatVersion** (opcional)
2. **Description** (opcional)
3. **Parameters** (opcional) - Valores customizáveis
4. **Resources** (obrigatório) - Recursos a serem criados
5. **Outputs** (opcional) - Informações exportadas

### Exemplo Básico

```
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Template exemplo - Security Group e EC2'

Parameters:
  InstanceType:
    Type: String
    Default: t2.micro
    Description: Tipo da instancia EC2

Resources:
  MySecurityGroup:
    Type: AWS::EC2::SecurityGroup
    Properties:
      GroupDescription: Permitir HTTP
      SecurityGroupIngress:
        - IpProtocol: tcp
          FromPort: 80
          ToPort: 80
          CidrIp: 0.0.0.0/0

  MyEC2Instance:
    Type: AWS::EC2::Instance
    Properties:
      InstanceType: !Ref InstanceType
      ImageId: ami-0c55b159cbfafe1f0
      SecurityGroups:
        - !Ref MySecurityGroup

Outputs:
  InstanceID:
    Description: ID da instancia criada
    Value: !Ref MyEC2Instance
```

---

## Exemplos Práticos

### 1. Stack de Security Group

**Arquivo:** `templates/exemplo-ec2-sg.yaml`

Cria um Security Group permitindo tráfego HTTP e SSH.

### 2. Stack de Bucket S3

**Arquivo:** `templates/exemplo-s3-bucket.yaml`

Provisiona um bucket S3 com versionamento habilitado.

---

## Aprendizados e Insights

### O que Aprendi

1. **Infraestrutura Declarativa**
   - Templates definem o estado desejado, não os passos
   - CloudFormation gerencia automaticamente a ordem de criação

2. **Gerenciamento de Stacks**
   - Atualização incremental evita recriar toda infraestrutura
   - Rollback automático garante integridade em falhas

3. **Boas Práticas**
   - Usar Parameters para valores que variam entre ambientes
   - Adicionar Outputs para facilitar integração entre stacks
   - Implementar Tags para organização e rastreamento de custos

### Desafios Encontrados

- **Dependências entre recursos:** Uso de `!Ref` e `DependsOn`
- **Validação de templates:** Comando `aws cloudformation validate-template`
- **Escolha de AMIs por região:** Uso de Mappings ou Parameters

---

## Como Usar

### Pré-requisitos

- Conta AWS ativa
- AWS CLI configurada
- Permissões IAM adequadas

### Criando uma Stack via CLI

```
aws cloudformation create-stack \
  --stack-name minha-stack \
  --template-body file://templates/exemplo-ec2-sg.yaml \
  --parameters ParameterKey=InstanceType,ParameterValue=t2.micro
```

### Validando Template

```
aws cloudformation validate-template \
  --template-body file://templates/exemplo-ec2-sg.yaml
```

### Atualizando Stack

```
aws cloudformation update-stack \
  --stack-name minha-stack \
  --template-body file://templates/exemplo-ec2-sg.yaml
```

### Deletando Stack

```
aws cloudformation delete-stack \
  --stack-name minha-stack
```

---

## Conclusão

O AWS CloudFormation é essencial para automação, padronização e governança de infraestrutura na nuvem. Durante este projeto, compreendi como:

- Definir infraestrutura como código usando templates YAML  
- Criar e gerenciar stacks automaticamente  
- Aplicar boas práticas de governança e versionamento  
- Integrar CloudFormation em fluxos DevOps  
- IaC elimina inconsistências entre ambientes
- Automação reduz tempo e erros manuais
- Templates versionados facilitam auditoria e rollback

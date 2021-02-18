# Workflow Template Serverless

Esse projeto tem como objetivo servir de base para a construção dos workflow

Esse projeto também é utilizado no projeto [project-template-serverless](https://github.com/RichardUlissesGabriel/project-template-serverless). 

## Configuração

Realizar a alteração do arquivo de configuração serverless.yml

```yaml
stepFunctions:
  stateMachines:
    <***NOME DO WORKFLOW***>:
      name: <***NOME DO WORKFLOW***>-${opt:stage}
      definition:
        Comment: "Workflow ..."
        StartAt: FirstStep
        States:
          FirstStep:
            Type: Task
            Resource: arn:aws:lambda:#{AWS::Region}:#{AWS::AccountId}:function:api-name-moduloName-hello:${self:provider.stage}
            # fail action
            Retry:
              - ErrorEquals:
                  - States.ALL
                IntervalSeconds: 60
                MaxAttempts: 3
                # BackoffRate: 2
            Next: Choice? # next step of workflow
          Choice?:
            Type: Choice
            Choices:
              # more info in: https://docs.aws.amazon.com/step-functions/latest/dg/amazon-states-language-choice-state.html
              - Variable: "$.statusCode"
                NumericEquals: 200
                Next: Success
              - Not: 
                  Variable: "$.statusCode"
                  NumericEquals: 200
                Next: Failed
            Default: Success

          Failed:
            Type: Fail
            Cause: "Ocorreu Algum erro"
          
          Success:
            Type: Pass
            Result: "Finalizado Com Sucesso"
            End: true    
```

Para maiores informações acessar o github dos devs do plugin ***serverless-step-functions*** 
https://github.com/serverless-operations/serverless-step-functions

## Exemplo 

Segue uma imagem com o resultado do exemplo realizado acima

![](stepfunctions_example.png?raw=true "Exemplo")

## Autores
* **Richard Ulisses Gabriel** - *Trabalho inicial*.

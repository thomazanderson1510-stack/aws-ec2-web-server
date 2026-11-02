# Laboratório AWS - Análise de conectividade de instâncias EC2

## Sobre o projeto

Este laboratório foi desenvolvido durante meus estudos de Cloud Computing com AWS.

O objetivo é investigar problemas de conectividade entre instâncias EC2, compreender as diferenças entre endereços IPv4 públicos e privados e identificar possíveis falhas na comunicação com a internet.

## Cenário

Um cliente possui uma VPC na AWS com o bloco CIDR `10.0.0.0/16` e duas instâncias EC2 na mesma sub-rede pública.

- Instância A: não consegue acessar a internet.
- Instância B: consegue acessar a internet.

O objetivo é investigar as configurações de rede e identificar a causa do problema.

## Serviços AWS utilizados

- Amazon EC2
- Amazon VPC
- Internet Gateway (IGW)
- Security Groups
- Tabelas de rotas

## Etapas do laboratório

1. Análise das instâncias EC2.
2. Identificação dos endereços IPv4 públicos e privados.
3. Comparação das configurações de rede.
4. Investigação da conectividade com a internet.
5. Identificação da possível causa da falha.
6. Documentação da solução recomendada.

## Conceitos aprendidos

- Endereçamento IPv4 público e privado.
- Blocos CIDR.
- Funcionamento de uma VPC.
- Internet Gateway.
- Conectividade de instâncias EC2.
- Troubleshooting de redes na AWS.

## Resultado esperado

Compreender como o endereçamento IP e as configurações de rede influenciam a conectividade de instâncias EC2 com a internet.

## Status

Laboratório educacional desenvolvido durante meus estudos de Cloud Computing.

## Autor

Estudante de Cloud Computing | Analista de Redes N2

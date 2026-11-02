# Laboratório AWS - Análise de Conectividade de Instâncias EC2

## 1. Sobre o projeto

Este laboratório foi desenvolvido durante meus estudos de Cloud Computing com Amazon Web Services (AWS).

O objetivo é investigar problemas de conectividade entre instâncias EC2, compreender as diferenças entre endereços IPv4 públicos e privados e identificar possíveis falhas na comunicação com a internet.

A atividade simula um cenário de atendimento técnico, no qual um cliente solicita suporte para solucionar problemas de conectividade em sua infraestrutura na nuvem.

## 2. Cenário apresentado pelo cliente

Um cliente possui uma infraestrutura na AWS composta por uma VPC com o bloco CIDR `10.0.0.0/16`, duas instâncias EC2 e um Internet Gateway (IGW).

As duas instâncias estão na mesma sub-rede pública e possuem configurações semelhantes, porém apresentam comportamentos diferentes:

- **Instância A:** não consegue acessar a internet.
- **Instância B:** consegue acessar a internet normalmente.

O objetivo é investigar as configurações de rede, comparar os endereços IP e identificar a possível causa da falha.

## 3. Arquitetura do ambiente

A infraestrutura analisada contém:

- Uma VPC com CIDR `10.0.0.0/16`.
- Uma sub-rede pública.
- Duas instâncias Amazon EC2.
- Um Internet Gateway (IGW).
- Tabela de rotas.

### Evidência 01 - Arquitetura da VPC

<img width="1443" height="434" alt="image" src="https://github.com/user-attachments/assets/5a5a31f5-bfdb-4a2a-8adb-830f5fadfdc3" />
<img width="1669" height="425" alt="image" src="https://github.com/user-attachments/assets/feacb240-74cf-43d1-941a-07304ab5c475" />



## 4. Serviços AWS utilizados

- **Amazon EC2:** serviço de computação para criação e gerenciamento de instâncias virtuais.
- **Amazon VPC:** permite criar redes virtuais isoladas na AWS.
- **Internet Gateway (IGW):** permite a comunicação entre a VPC e a internet.
- **Security Groups:** controlam o tráfego de entrada e saída das instâncias.
- **Tabelas de rotas:** definem os caminhos utilizados pelo tráfego de rede.

## 5. Etapas da investigação

### 5.1 Análise das instâncias EC2

Foi realizada a análise das duas instâncias no console de gerenciamento da AWS, verificando suas configurações de rede.

Os principais dados observados foram:

- Endereço IPv4 público.
- Endereço IPv4 privado.
- VPC associada.
- Sub-rede utilizada.

### Evidência 02 - Configuração de rede da instância A

<img width="1650" height="674" alt="image" src="https://github.com/user-attachments/assets/20f410f8-8842-4fac-a79d-017666daa3f0" />


### Evidência 03 - Configuração de rede da instância B

<img width="1663" height="724" alt="image" src="https://github.com/user-attachments/assets/75594af4-ab10-488b-a9c3-3fc6b0a4c032" />


### 5.2 Comparação dos endereços IP

Durante a investigação, é necessário comparar os endereços IPv4 públicos e privados das duas instâncias.

Os endereços privados permitem a comunicação dentro da rede privada, enquanto os endereços públicos permitem a comunicação direta com a internet, desde que as configurações de rede e segurança estejam corretas.

### 5.3 Análise de conectividade

Para identificar a causa do problema, foram considerados os seguintes pontos:

1. Verificação da existência de endereço IPv4 público.
2. Análise da tabela de rotas.
3. Verificação da rota `0.0.0.0/0` direcionada ao Internet Gateway.
4. Análise das regras dos Security Groups.
5. Verificação das Network ACLs (NACLs).
6. Investigação das configurações de rede do sistema operacional.

## 6. Resultado da análise e diagnóstico técnico

### Problema identificado

As duas instâncias EC2 estão na mesma VPC e sub-rede pública, mas apresentam comportamentos diferentes em relação à conectividade com a internet.

### Possível causa raiz

Uma das principais hipóteses é que a instância A não possui um endereço IPv4 público associado, enquanto a instância B possui.

Estar em uma sub-rede pública não garante automaticamente que uma instância EC2 tenha acesso direto à internet.

Para utilizar o Internet Gateway em comunicação IPv4 direta, a instância precisa ter um endereço IPv4 público ou Elastic IP, além de uma rota adequada e das regras de segurança necessárias.

**Observação:** essa hipótese precisa ser confirmada pela comparação dos endereços IP observados no console AWS. O cenário fornecido não apresenta os valores reais dos endereços das instâncias.

## 7. Solução recomendada

Caso seja confirmada a ausência de endereço IPv4 público na instância A, recomenda-se:

1. Associar um endereço Elastic IP à instância A.
2. Verificar se a tabela de rotas possui a rota `0.0.0.0/0` direcionada ao Internet Gateway.
3. Validar as regras de entrada e saída do Security Group.
4. Verificar as regras da Network ACL.
5. Realizar novos testes de conectividade.

### Testes de conectividade

No sistema operacional Linux, podem ser utilizados os seguintes comandos:

**Teste de conectividade IP:**

```bash
ping -c 4 8.8.8.8
```

**Teste de resolução DNS:**

```bash
nslookup amazon.com
```

**Teste de acesso HTTP:**

```bash
curl -I https://amazon.com
```

### Evidência 04 - Resultado dos testes

<img width="1504" height="718" alt="image" src="https://github.com/user-attachments/assets/79edd60d-768c-40b2-9946-5875fec3b72b" />


## 8. Análise sobre utilização de IP público na VPC

O cliente também questionou a possibilidade de utilizar o bloco CIDR `12.0.0.0/16` na criação de uma nova VPC.

Esse intervalo pertence ao espaço de endereçamento IPv4 público e não faz parte dos intervalos privados definidos pela RFC 1918.

A utilização de endereços públicos que não pertencem à organização pode provocar conflitos de roteamento ao tentar acessar recursos legítimos na internet.

### Intervalos privados definidos pela RFC 1918

- `10.0.0.0/8`
- `172.16.0.0/12`
- `192.168.0.0/16`

**Recomendação:** utilizar intervalos privados para o planejamento de endereçamento das redes VPC, evitando conflitos com endereços públicos da internet.

## 9. Principais conceitos aprendidos

Durante este laboratório, foram estudados os seguintes conceitos:

- Diferença entre endereços IPv4 públicos e privados.
- Planejamento de endereçamento IP utilizando CIDR.
- Funcionamento de uma Virtual Private Cloud (VPC).
- Comunicação entre instâncias EC2.
- Funcionamento do Internet Gateway.
- Tabelas de rotas e conectividade.
- Security Groups e Network ACLs.
- Metodologia de troubleshooting de redes.

## 10. Conclusão

O laboratório permitiu compreender a importância do endereçamento IP e das configurações de rede na comunicação de instâncias EC2 com a internet.

A investigação demonstrou que duas instâncias localizadas na mesma sub-rede podem apresentar comportamentos diferentes de conectividade devido às suas configurações individuais.

Também foi possível compreender a importância da utilização de intervalos privados de endereçamento IP no planejamento de redes em ambientes de Cloud Computing.

A atividade contribuiu para desenvolver conhecimentos práticos de análise de conectividade, identificação de falhas e troubleshooting na infraestrutura AWS.

## 11. Status do projeto

Laboratório educacional desenvolvido durante meus estudos de Cloud Computing com AWS.

**Área de conhecimento:** Cloud Computing, Redes e Infraestrutura.

**Serviços estudados:** Amazon EC2, Amazon VPC e Internet Gateway.

## 12. Autor

Estudante de Cloud Computing | Analista de Redes N2

Projeto desenvolvido para fins de aprendizado e construção de portfólio profissional.

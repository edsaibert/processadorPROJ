<a name="br1"></a> 

Processador Uniciclo

Eduarda Saibert

Professor Daniel Oliveira

Novembro de 2023

1 Circuito Geral

O trabalho a seguir diz respeito à um processador uniciclo, que respeita as seguintes

instruções RISC-V (tabela [1](#br1)):

Tabela 1 – Tipos de Instrução

Função

ADD

SUB

XOR

OR

CodOp Tipo Semântica

0110011

0110011

0110011

0110011

0110011

0110011

~~0110011~~

0010011

0010011

0010011

0010011

0010011

~~0010011~~

~~1100011~~

~~1100011~~

~~1100011~~

~~1100011~~

~~0000011~~

~~0100011~~

~~1101111~~

~~1100111~~

R

R

R

R

R

R

~~R~~

I

rd = rs1 + rs2

rd = rs1 - rs2

rd = rs1 ˆrs2

rd = rs1 | rs2

AND

SLL

rd = rs1 & rs2

rd = rs1 « rs2

~~SLT~~

~~rd = (rs1 < rs2) ? 1:0~~

rd = rs1 + imm

ADDI

XORI

ORI

I

rd = rs1 îmm

I

rd = rs1 | imm

ANDI

SLLI

~~SLTI~~

~~BEQ~~

~~BNE~~

~~BLT~~

I

rd = rs1 & imm

I

rd = rs1 « imm[0:4]

~~rd = (rs1 < imm) ? 1:0~~

~~if (rs1 == rs2) PC += imm~~

~~if (rs1 != rs2) PC += imm~~

~~if (rs1 < rs2) PC += imm~~

~~if (rs1 >= rs2) PC += imm~~

~~rd = M[rs1+imm][0:31]~~

~~M[rs1+imm][0:31] = rs2[0:31]~~

~~rd = PC+4 PC += imm~~

~~rd = PC+4 PC = rs1 + imm~~

PC = PC + 0

~~I~~

~~B~~

~~B~~

~~B~~

~~B~~

~~I~~

~~BGE~~

~~LW~~

~~SW~~

~~S~~

~~J~~

~~I~~

~~JAL~~

~~JALR~~

EBREAK 1110011

I

Dito isso, adaptou-se o RISC-V para operações de 24 bits, visto que o simulador

de circuitos digitais Digital não opera ROMs ou RAMs com 32 bits.

A seguir, encontra-se o circuito geral do processador, ilustrado pela ﬁgura [1](#br2)

1



<a name="br2"></a> 

Figura 1 – Circuito

Arquivo: pc+mi

2 Unidade de Controle

A Unidade de Controle foi construída de forma intuitiva a partir das etapas: i)

decodiﬁcar o tipo de instrução; ii) gerar saídas de controle refentes à instrução; iii) gerar

tipo de operação da ULA. Para simpliﬁcar o processo, foi criada a tabela [2](#br3).

Em primeiro momento, por meio do código de operação da instrução, a unidade de

controle i) decodiﬁca o tipo de instrução (ﬁgura [2](#br2)[).](#br2)

Figura 2 – Tipo de Função

Arquivo: qualehfuncao

2



<a name="br3"></a> 

Tabela 2 – UC

~~Função~~

~~ADD~~

~~SUB~~

~~XOR~~

~~OR~~

~~Func3 Func7 RegWrite ImmSrc ULAOp~~

~~MemWrite ResultSrc ULASrc PCSrc~~

~~0x0~~

~~0x0~~

~~0x4~~

~~0x6~~

~~0x7~~

~~0x1~~

~~0x2~~

~~-~~

~~0x00~~

~~0x20~~

~~0x00~~

~~0x00~~

~~0x00~~

~~0x00~~

~~0x00~~

~~-~~

~~1~~

~~1~~

~~1~~

~~1~~

~~1~~

~~1~~

~~1~~

~~-~~

~~000~~

~~000~~

~~000~~

~~000~~

~~000~~

~~000~~

~~000~~

~~-~~

~~ADD~~

~~SUB~~

~~XOR~~

~~OR~~

~~000~~

~~001~~

~~010~~

~~011~~

~~100~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~-~~

~~00~~

~~00~~

~~00~~

~~00~~

~~00~~

~~00~~

~~00~~

~~-~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~-~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~-~~

~~AND~~

~~SLL~~

~~AND~~

~~SHIFT 101~~

~~SUB~~

~~-~~

~~SLT~~

~~-~~

~~001~~

~~-~~

~~ADDI~~

~~XORI~~

~~ORI~~

~~ANDI~~

~~SLLI~~

~~SLTI~~

~~-~~

~~0x0~~

~~0x4~~

~~0x6~~

~~0x7~~

~~0x1~~

~~0x2~~

~~-~~

~~0x0~~

~~0x1~~

~~0x4~~

~~0x5~~

~~-~~

~~0x2~~

~~0x2~~

~~-~~

~~0x0~~

~~0x0~~

~~0x00~~

~~0x00~~

~~0x00~~

~~0x00~~

~~0x00~~

~~0x00~~

~~-~~

~~0x00~~

~~0x00~~

~~0x00~~

~~0x00~~

~~-~~

~~0x00~~

~~0x00~~

~~-~~

~~0x00~~

~~0x00~~

~~1~~

~~1~~

~~1~~

~~1~~

~~1~~

~~1~~

~~-~~

~~0~~

~~0~~

~~0~~

~~0~~

~~-~~

~~1~~

~~0~~

~~-~~

~~0~~

~~1~~

~~001~~

~~001~~

~~001~~

~~001~~

~~001~~

~~001~~

~~-~~

~~010~~

~~010~~

~~010~~

~~010~~

~~-~~

~~001~~

~~011~~

~~-~~

~~100~~

~~001~~

~~ADD~~

~~XOR~~

~~OR~~

~~000~~

~~010~~

~~011~~

~~100~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~-~~

~~0~~

~~0~~

~~0~~

~~0~~

~~-~~

~~0~~

~~1~~

~~-~~

~~0~~

~~0~~

~~00~~

~~00~~

~~00~~

~~00~~

~~00~~

~~00~~

~~-~~

~~00~~

~~00~~

~~00~~

~~00~~

~~-~~

~~01~~

~~01~~

~~10~~

~~11~~

~~01~~

~~1~~

~~1~~

~~1~~

~~1~~

~~1~~

~~1~~

~~-~~

~~0~~

~~0~~

~~0~~

~~0~~

~~-~~

~~1~~

~~1~~

~~-~~

~~1~~

~~1~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~0~~

~~-~~

~~1~~

~~1~~

~~1~~

~~1~~

~~-~~

~~0~~

~~0~~

~~-~~

~~1~~

~~1~~

~~AND~~

~~SHIFT 101~~

~~SUB~~

~~-~~

~~SUB~~

~~SUB~~

~~SUB~~

~~SUB~~

~~-~~

~~ADD~~

~~ADD~~

~~-~~

~~X~~

~~ADD~~

~~001~~

~~-~~

~~001~~

~~001~~

~~001~~

~~001~~

~~-~~

~~000~~

~~000~~

~~-~~

~~BEQ~~

~~BNE~~

~~BLT~~

~~BGE~~

~~-~~

~~LW~~

~~SW~~

~~-~~

~~JAL~~

~~JALR~~

EBREAK

~~000~~

Logo após, ii) gera a maior parte das saídas de controle (RegWrite, ImmSrc,

MemWrite, ResultSrc, ULASrc, PCSrc) (ﬁgura [3](#br3)). Em especíﬁco, a saída PCSrc necessita

da conﬁrmação do tipo jump ou callback da ULA para efetuar as instruções de branch.

Figura 3 – Saídas

Arquivo: uc

3



<a name="br4"></a> 

Por ﬁm, a instrução é encaminhada para a segunda parte da unidade de controle,

que fornece a saída referente a ULA (ULAOp). Nesse componente, as partes funct3 e

funct7 são utilizadas para diferenciar instruções com mesmo código de operação (ﬁgura [4](#br4)).

A saída Bt codiﬁca o tipo de branch, para facilitar a veriﬁcação de mudanças no ﬂuxo de

execução.

Figura 4 – Ula UC

Arquivo: uc\_ula

3 ULA

As operações da ULA foram escolhidas conforme tabela [2](#br3). Além disso, a ULA é

responsável pelas saídas ZERO, GT (greater or equal than) e LT (less than), úteis para a

operação de subtração no caso de branches. O circuito pode ser visualizado na ﬁgura [5](#br4).

Figura 5 – ULA

Arquivo: ula

4



<a name="br5"></a> 

4 Imediatos

Os imediatos em RISC-V seguem a implementação disponível na tabela [3](#br5). Dito

isso, no que se refere ao trabalho, o componente de extensão para cada tipo de imediato

(deﬁnidos na tabela [2](#br3)) está presente na ﬁgura [6](#br5)

Tabela 3 – Imediatos RISC-V

~~31-25~~

~~24-20 19-15 14-12~~

~~11-7~~

~~6-0~~

~~Tipo~~

~~funct7~~

~~rs2~~

~~rs1~~

~~rs1~~

~~rs1~~

~~rs1~~

~~funct3 rd~~

~~opcode R-type~~

~~opcode I-type~~

~~opcode S-type~~

~~imm[11:0]~~

~~imm[11:5]~~

~~imm[12|10:5]~~

~~imm[31:12]~~

~~imm[20|10:1|11|19:12]~~

~~funct3 rd~~

~~rs2~~

~~rs2~~

~~funct3 imm[4:0]~~

~~funct3 imm[4:1|11] opcode B-type~~

~~rd~~

~~rd~~

~~opcode U-type~~

~~opcode J-type~~

Figura 6 – Imediatos Implementação

Arquivo: IMMSRC

5 Assembly

O processador deve operar o seguinte código ﬁbonacci, transcrito de C para

Assembly:

void fib(int \* vet, int tam, int elem1, int elem2){

int i;

vet[0] = elem1;

vet[1] = elem2;

for(i=2; i<tam; ++i){

vet[i] = vet[i-1] + vet[i-2];

}

}

int main(){

int vet[20];

fib(vet, 20, 1, 1);

}

5



<a name="br6"></a> 

Visto isso, o código ﬁnal (introduzido à ROM do processador):

addi s1, zero, 20

\# s1 tem o valor de tam

addi a0, zero, 0

addi a1, s1, 0

\# a0 tem o endereco do vetor (0)

\# a1 tem o valor de tam

addi a2, zero, 1

addi a3, zero, 1

\# a2 tem o primeiro elem da sequencia

\# a3 tem o segundo elem da sequencia

jal fib

ebreak

\# pula para a funcao

\# encerra o programa

fib:

sw a2, 0(a0)

sw a3, 4(a0)

\# guarda 1 no primeiro espaco do vetor

\# guarda 1 no segundo espaco do vetor

addi t1, zero, 2

\# int i = 2

for:

bge t1, a1, fora\_for # sai do for se i >= tam

slli t2, t1, 2

add t2, t2, a0

\# t2 = i\*4

lw t3, -4(t2)

lw t4, -8(t2)

\# carrega o valor de v[i-1]

\# carrega o valor de v[i-2]

add t5, t3, t4

sw t5, 0(t2)

addi t1, t1, 1

jal x0, for

fora\_for:

\# realiza a soma de v[i-1] e v[i-2]

\# guarda o valor de v[i]

\# incrementa 1 em i

jalr ra, 4

\# volta para main

6



## Linguagem para Descrição de LPSs

Repositório do projeto da disciplina Paradigmas de Linguagens de Programação [in1007](https://www.cin.ufpe.br/~in1007/) da pós-graduação em Ciência da Computação da UFPE, ministrada pelo Prof.Dr. Augusto Sampaio em 2021.1.

## Equipe
- Thayonara Pontes (tpa@cin.ufpe.br) 
- Reydne Bruno (rbs8@cin.ufpe.br)

## Introdução

Linha de Produtos de Software (LPS) consiste em um conjunto de sistemas de software que compartilham de funcionalidades em comum, mas que diferem uma das outras. Para isso, se é pensado em componentes já existentes no portfólio ou que ainda serão produzidos, mas que possam fazer parte do conjunto de componentes comuns reutilizáveis por todos os sistemas que são membros da mesma família. A partir do momento em que se deseja criar um outro produto, usam-se os componentes comuns à família e insere, se necessário, outras funcionalidades a fim de atender as necessidades de um determinado segmento de mercado. Assim, o novo produto é construído com menos esforço uma vez que ele faz parte de uma LPS.

Para capturar as intenções dos stakeholders de uma linha de produtos, além de conceitos de design e de implementação usados para estruturar, reutilizar e mudar artefatos de software, existem muitas definições para feature [Apel et al., 2013]. Uma delas é que se trata de um elemento que amplia e modifica a estrutura de um determinado programa para satisfazer os requisitos de um ou mais stakeholders [Apel et al., 2013].

Features são usadas para distinguir produtos em uma LPS e exercem um papel importante quanto ao gerenciamento de variabilidade na abordagem Feature-Oriented Domain Analysis
(FODA) [Kang et al., 1990]. Combinações diferentes de features resultam em produtos diferentes em uma LPS. Essas combinações são bem capturadas por feature models (FMs), geralmente, representados através de árvores, onde é possível expressar os relacionamentos das features.

Apesar dos benefícios das LPSs como maior qualidade, menor custo e tempo de produção, elas trazem desafios tanto para a sua manutenção quanto para a testagem de seus produtos. Com relação a esse último, o problema reside no fato de que o  número de configurações possíveis, induzidas por um determinado FM, cresce exponencialmente com o número de features, o que leva rapidamente a milhões de configurações possíveis para teste. Por essa razão, engenheiros de teste buscam soluções para diminuir o número de configurações testadas, mas com uma maior confiabilidade. 

Trabalhos anteriores [Cohen et al, 1997; Kuhn et al, 2004] identificaram o teste de interação combinatória (CIT) como uma abordagem relevante para reduzir o número de produtos para teste. Essa técnica seleciona o conjunto de todas as combinações de forma que todos os pares possíveis de valores de variáveis ​​sejam incluídos no conjunto de dados de teste. Portanto, uma opção possível é testar SPLs, gerando configurações de teste que cobrem todas as possíveis interações de features t (t-wise), reduzindo drasticamente o número de produtos de teste, garantindo uma cobertura razoável de LPS.

## Objetivo

Esse projeto visa fornecer uma estratégia leve para a descrição de LPSs, cujas semelhanças e variabilidades entre os produtos de uma família são modeladas em termos de features. Uma vez que temos essa representação, podemos usar modelos de features como um artefato relevante para gerar suítes de configuração de teste para LPSs, por meio de técnicas de CIT. É importante destacar que, nesse ponto, nos concentramos no problema de gerar casos de teste abstratos e não em executá-los.

## Possível representação de uma SPL

```
PL nome_PL{
	FeatureName nome_fn [extends fn_pai] as tipo_feature;
	Formula nome_fm (fn1 operador fn2);
	Product nome_produto (fn1,fn2…,fn);
}
```

- Exemplo (incompleto) LPS Mobile Media:
```
PL MobileMedia{
	FeatureName MobileMedia as ROOT;
	FeatureName Media extends MobileMedia as MANDATORY;
						       [...]
	Formula f1  = sendPhoto implies Photo;
	Product p1 = {MobileMedia, Media, SendPhoto, Photo} ;
} 
```

## BNF

Programa ::= [DeclaracaoPL](https://github.com/Thayonara/plp2021_project/blob/master/src/implementations/PLDeclaration.java)

DeclaracaoPL ::= “PL” [Id](https://github.com/Thayonara/plp2021_project/blob/master/src/implementations/Id.java) CorpoPL

CorpoPL ::= “{“ “}”
	      | “{“ ( [DeclaracaoFN](https://github.com/Thayonara/plp2021_project/blob/master/src/implementations/FeatureNameDeclaration.java) | [DeclaracaoFormula](https://github.com/Thayonara/plp2021_project/blob/master/src/implementations/Formula.java) | [DeclaracaoProduto](https://github.com/Thayonara/plp2021_project/blob/master/src/implementations/ProductDeclaration.java)) + “}”
	      
DeclaracaoFN ::= “FeatureName” Id [”extends” Id] “as” 

(ROOT| MANDATORY| OPTIONAL| ALTERNATIVE | OR) “;”


```
DeclaracaoFormula ::= “Formula” Id “=” Formula
Formula ::= FormulaUnaria | FormulaBinaria | Id
FormulaUnaria ::= “not” Formula
FormulaBinaria ::= Formula “and” Formula
		      | Formula “implies” Formula
		      | Formula “or” Formula
          
          
DeclaracaoProduto ::= “Product” Id “=” “{“ (Id) [”,” Id]* “};”
```

## Referências

[Apel et al., 2013] Apel, S., Batory, D., Kastner, C., Saake, G. Feature-Oriented Software Product Lines: Concepts and Implementation. 1. ed. [S.l.]: Springer, 2013. 320 p.

[Cohen et al., 1997] Cohen DM, Dalal SR, Fredman ML, Patton GC (1997) The AETG System: an approach to testing based on combinatorial design. IEEE Trans Softw Eng 23(7):437–444.

[Kang et al., 1990] Kang, K., Cohen, S., Hess, J., Novak, W., and Peterson, S. (1990). Feature-oriented domain analysis (FODA) feasibility study. Technical Report CMU/SEI-90-TR-21, Software Engineering Institute, Carnegie Mellon University.

[Kuhn et al., 2004] Kuhn DR, Wallace DR, Gallo AM (2004) Software fault interactions and implications for software testing. IEEE Trans Softw Eng 30(6):418–421.


## Introdução: The Educated are Harder to Advertise To - *Chitika Insights*
Um estudo realizado pela Chitika Insights sobre a taxa de cliques em anúncios (ou CTR), sugeriu que estados com maior percentagem de graduados eram menos propensos a visualizar anúncios.
Endereço da publicação: (https://research.chitika.com/the-educated-are-harder-to-advertise-to-2/) e os dados realizado para a análise: (https://www.r-statistics.com/wp-content/uploads/2010/02/State_CTR_Date.txt).

O objetivo aqui é investigar se, de fato, a correlação entre nível de educação e taxa de cliques em anúncios sugerida pelos dados implica em causalidade. Para isso, faremos uma análise de regressão e (outros métodos estatísticos, como CI,..)

Dados em forma de texto:
```R
dados_brutos <- "Alabama	0.0119	0.1900  82
Alaska	0.0109	0.2470  51
Arizona	0.0110	0.2350  61
Arkansas	0.0121	0.1670  78
California	0.0096	0.2660  57
Colorado	0.0103	0.3270  58
Connecticut	0.0105	0.3140  55
Delaware	0.0105	0.2500  61
District of Columbia	0.0099	0.3910  61
Florida	0.0122	0.2230  65
Georgia	0.0108	0.2430  76
Hawaii	0.0100	0.2620  57
Idaho	0.0112	0.2170  61
Illinois	0.0097	0.2610  64
Indiana	0.0108	0.1940  68
Iowa	0.0114	0.2120  64
Kansas	0.0106	0.2580  70
Kentucky	0.0117	0.1710  74
Louisiana	0.0112	0.1870  78
Maine	0.0115	0.2290  48
Maryland	0.0099	0.3140  65
Massachusetts	0.0091	0.3320  64
Michigan	0.0107	0.2180  85
Minnesota	0.0097	0.2740  68
Mississippi	0.0127	0.1690  56
Missouri	0.0112	0.2160  67
Montana	0.0127	0.2440  54
Nebraska	0.0101	0.2370  46
Nevada	0.0109	0.1820  60
New Hampshire	0.0110	0.2870  65
New Jersey	0.0099	0.2980  61
New Mexico	0.0128	0.2350  76
New York	0.0100	0.2740  75
North Carolina	0.0109	0.2250
North Dakota	0.0110	0.2200
Ohio	0.0109	0.2110
Oklahoma	0.0106	0.2030
Oregon	0.0098	0.2510
Pennsylvania	0.0102	0.2240
Rhode Island	0.0106	0.2560
South Carolina	0.0117	0.2040
South Dakota	0.0099	0.2150
Tennessee	0.0110	0.1960
Texas	0.0097	0.2320
Utah	0.0103	0.2610
Vermont	0.0100	0.2940
Virginia	0.0101	0.2950
Washington	0.0091	0.2770
West Virginia	0.0139	0.1480
Wisconsin	0.0103	0.2240
Wyoming	0.0132	0.2190"
```

Lendo os dados no R:
```R
dados <- read.table(text = dados_brutos, header = FALSE, sep = "\t", col.names = c("State", "CTR", "College_Grad"), fill = TRUE)
```

Observando as primeiras linhas do dataset:
```R
head(dados)
       State  CTR College_Grad
1    Alabama 1.19         19.0
2     Alaska 1.09         24.7
3    Arizona 1.10         23.5
4   Arkansas 1.21         16.7
5 California 0.96         26.6
6   Colorado 1.03         32.7
```

Observando a correlação entre o CTR e College_Grad:
```R
cor(dados[,2], dados[,3])
[1] -0.6275642
```

Plotando o gráfico de dispersão;
```R
dados[,2:3] <- dados[,2:3] * 100

plot(dados[,2] ~ dados[,3], sub = paste("Correlação: ", round(cor(dados[,2], dados[,3]), 2)),
     main = "Scatter plot of %CTR VS %College_Grad per State",
     xlab = "%College_Grad per State",
     ylab = "%CTR per State")
```

![Rplot03](https://github.com/user-attachments/assets/4a672e24-cb0b-4d00-b39a-677897f8f238)


E traçando a linha de regressão que melhor descreve os dados:
```R
abline(lm(dados[,2] ~ dados[,3]), col = "blue")
```

![Rplot04](https://github.com/user-attachments/assets/e5f04a45-f309-491d-9d83-e4638691e71f)


Outro modo de implementar a função lm():
```R
model = lm(formula = CTR ~ College_Grad, data = dados)
coef(model)
 (Intercept) College_Grad 
  1.41233478  -0.01373001
abline(model, col = "red")
```

```R
summary(model)
Call:
lm(formula = CTR ~ College_Grad, data = dados)

Residuals:
      Min        1Q    Median        3Q       Max 
-0.127140 -0.059291 -0.008673  0.032357  0.208352 

Coefficients:
              Estimate Std. Error t value Pr(>|t|)
(Intercept)   1.412335   0.059694  23.659  < 2e-16
College_Grad -0.013730   0.002433  -5.642 8.28e-07
                
(Intercept)  ***
College_Grad ***
---
Signif. codes:  
0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1

Residual standard error: 0.08173 on 49 degrees of freedom
Multiple R-squared:  0.3938,	Adjusted R-squared:  0.3815 
F-statistic: 31.84 on 1 and 49 DF,  p-value: 8.282e-07
```








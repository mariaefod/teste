Algumas anotações sobre o make

- A estrutura é basicamente:

```
alvo : dependências
    ações
```

- Quando solicitado a compilar um alvo, o Make verifica se alguma dependência foi atualizada desde a última modificação do alvo. As ações são executadas para atualizar o alvo.

- `.PHONY`: marca alvos que não correspondem a arquivos.

- `$@`: se refere ao alvo da regra atual.

- `$^`: se refere às dependências da regra atual.

- `$<`: se refere à primeira dependência da regra atual.

- `make -n dats` mostra os comandos a serem executados, mas sem executá-los

- `%`: marcador de posição em destinos e dependências.

- `$*`: se refere a conjuntos de arquivos correspondentes em ações.

- **Variáveis**: ​​atribuem valores a nomes; devem ser usadas dentro de $(...).

- `wildcard`: obtém listas de arquivos que correspondam a um padrão.

- `patsubst`: reescreve os nomes dos arquivos.

- `help`: no Makefile, adicione comentários formatados sobre o alvo para ser informado no help.

```
help : Makefile
	@sed -n 's/^##//p' $<
```
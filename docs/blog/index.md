# Anotações dos comandos apresentados na aula

## Gestor de bibliotecas
$ pipx install poetry

$ poetry init

$ poetry add mkdocs

$ poetry run mkdocs new .

$ poetry run mkdocs serve

ctrl c para desligar o servidor

## Comandos git

$ git status

$ git add `o que foi modified`

$ git add . (adiciona todos)

$ git commit -m "escrever o que foi modificado"

$ git push OU $ git push origin main

$ git pull origin main (trazer modificações do git)


$ git --version (mostra a versão do git)

$ git init (cria a pasta .git)


$ git log (mostra o histórico de commits) e git log --oneline (apenas uma linha)

$ git checkout `número hash` (volta na versão anterior para consulta)
ex.: git switch main (muda para o branch "main")

sempre começar com git status e git pull origin main

## Comandos básicos

$ pwd (mostra o diretório)

$ ls (lista documentos) e ls -la (vertical e tudo)

$ cd `caminho do diretório` (ex.: cd docs/blog/posts; apertar tab para mostrar caminhos; isso altera o pwd)

$ cd .. (volta uma pasta) e cd ../.. (volta duas pastas em comparação com a atual do pwd)

$ touch `nome do arquivo a ser criado` (ex.: touch 20260220_primeiro_post; cria um novo arquivo dentro do diretório)

$ mkdir `nome da pasta a ser criada` (ex.: mkdir pasta1; cria uma nova pasta dentro do diretório)

rm -rf `caminho do diretório/` (ex.: rm -rf pasta1/; exclui a pasta)
ex.: rm post1.md

## Site online

$ poetry run mkdocs build

$ poetry run mkdocs gh-deploy

## Branch

$ git checkout -b nome da branch

$ git fetch

$ git branch --list

$ git switch nome
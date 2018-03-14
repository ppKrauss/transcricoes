## Procedimentos

Conversão do HTML oficial disponivel para o formato `HTML5-lex2`, especializado de  [HTML5-onlyContent](https://github.com/okfn-brasil/HTML5-onlyContent).


0. [`curl`](https://en.wikipedia.org/wiki/CURL) para *download* do HTML origonal oferecido pelo site do Planalto.
1. conversão para UTF-8 (não foi respeitado o padrão ePING);
2. conversão, via [`pandoc`](https://en.wikipedia.org/wiki/Pandoc), do HTML para [formato Markdown GFM](https://en.wikipedia.org/wiki/Markdown#GFM);
3. Correção dos ordinais masculunos (convertendo-os de HTML para UTF-8) e dos intralinks, do que se criou uma versão de referncia `~p03-ref`. A partir dela pode-se comparar se houveram atualizações no Planalto.
4. Cópia inicial do  `p03-ref` para se completar a transcrição com assistência humana.  Todas as alterações (software ou humanas)  serão daí em diante mantidas no git, sob o arquivo `~p04-format`. 

Cada um desses passos ficam registrados nos respectivos arquivos, ou seja, de final `~p01-utf8.htm`, `~p02-pandoc0`, `~p03-ref.md`, etc.

## 2002-01-10;10406

[Lei nº 10.406, de 10 de Janeiro de 2002](http://lexml.gov.br/urn/urn:lex:br:federal:lei:2002-01-10;10406) (**Código Civil**). Nome uniforme: <code>urn:lex:br:federal:lei:2002-01-10;10406</code>.

### Preparo inicial

Download dos orgiginais no `http://planalto.gov.br` requer uso do *User-Agent*, 

```sh
curl -H "User-Agent: Mozilla/5.0 (Linux)" \
   http://www.planalto.gov.br/CCivil_03/leis/2002/L10406compilada.htm \
   | iconv -c  -t UTF-8  -f ISO-8859-1 > 2002-01-10-10406-compilada~p01-utf8.htm
```

Conversões iniciais:

```sh
pandoc -f html-native_divs-native_spans -t gfm \
   2002-01-10-10406-compilada~p01-utf8.htm > 2002-01-10-10406-compilada~p02-pandoc0.md
```

Em seguida o `pandoc0` tem seus elementos básicos corrigidos (poderiam já ser convertidos em pipe), para se tornar o markdown de referência inicial para os diffs e versionamento no *git*.

Do ponto de vista editorial os ordinais masculinos podem ser expressos por meio de UTF-8, conforme testes de fonte em  [testFonts.htm](../testFonts.htm). A conversão foi por substituição direta de  `<sup><span class="underline">o</span></sup>` por `º`.  Os intralinks antes marcados com o nome do arquivo, foram convertidos para a referência relativa &mdash; por exemplo `L10406.htm#capituloii` em `#capituloii`. De qualquer forma poderão ser eliminados.

### Preparo no p04-format 

Primeira etapa, já realizada, ver [commit/084035f](https://github.com/ppKrauss/transcri-lex/commit/084035f4c9dd07d1e986ef9d2fc3e84f5852dfcb#diff-dc048e1267b5e68a0e009a6a1c0d3dee).

Como ainda não conhecemos o código do planalto, nem acompanhamos a confiabilidade das ferramentas, foram realizadas manualmente as conversões por regular expression no Atom:

De (busca) | Para (substituição) | parâmetros 
-----------|--------------------|----------
`\nLIVRO\s+([IVDX]+)\s*\n` | `\n##LIVRO $1 - ` | regex match-case
`\nT[IÍ]TULO\s+([IVDX]+)\s*\n` | `\n###TÍTULO $1 - ` | regex match-case
`\nCAP[IÍ]TULO\s+([IVDX]+)\s*\n` | `\n####CAPÍTULO $1 - ` | regex match-case
`\nSe[cç][aã]o\s+([IVDX]+)\s*\n` | `\n#####Seção $1 - ` | regex match-case

sempre olhando um a um, evitando *replace-all* justamente para manter a verificação humana neste primeiro experimento.

Segunda etapa...



## Outros documentos

Lotes de transcrição são conjuntos de documentos... Neste caso, o nosso primeiro teste, optamos por trabalhar com um só documento.

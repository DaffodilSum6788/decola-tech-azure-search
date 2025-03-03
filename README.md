## Análise de sentimentos de frases com Azure Language Studio
Este projeto foi realizado no Bootcamp Decola Tech 2025, no curso de IA generativa redigido por [Valéria Baptista](https://www.linkedin.com/in/valeriabaptista/), utilizando os serviços disponíveis no portal da Azure para criar um sistema de buscas para resenhas de uma cafeteria.

### Processo de implementação
1. **Criação dos recursos necessários**
Para implementar esse serviço, foi necessária a criação de 2 recursos: Um recurso de pesquisa, o **Azure AI Search**, e um recurso de serviço, o **Azure AI Services**. Também foi necessário criar uma **conta de armazenamento**, para depositarmos os documentos e integrá-los ao serviço de pesquisa.

2. **Upload das resenhas para a conta de armazenamento**
Após a criação dos três, foram baixados os documentos das resenhas no arquivo ZIP disponibilizados no [guia do laboratório de AI Search](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html) e enviados a um novo _container_ da conta de armazenamento por meio de um upload simples.

3. **Importação dos dados das resenhas para o serviço de busca**
Para realizar a integração com o serviço de busca, foi necessário abrir o recurso de busca e importar os dados presentes na conta de armazenamento. Foram feitos um _index_ e instruções para o serviço de busca selecionar dados específicos a serem extraídos e _indexados_, como locais, opiniões, sentimentos, e outras palavras-chave que podem ser importantes no contexto de uma resenha. Ao finalizar a importação, os dados já estavam disponíveis para a busca.

### Buscas
Na página do recurso do Azure AI Search, já era possível realizar buscas por meio da aba **Explorador de pesquisa**. Lá, realizamos consultas de dados específicos (Por exemplo, `locations:'Chicago'` irá procurar todas as resenhas realizadas nas unidades da cidade de Chicago), e são retornados os dados da pesquisa em formato JSON (JavaScript Object Notation). Como exemplo prático, a seguinte consulta:
```JSON
{
  "search": "sentiment:'positive'"
  "count": true
}
```
retornará como resultado todas as resenhas que a IA classificou como "positivas" em uma análise de sentimento, como por exemplo:

```json
{
      "@search.score": 0.6931472,
      "content": "\n\nReview: I love the coffee drinks here, but my favorite part is the local art they sell. There are many kinds of paintings and watercolors they showcase each week. I love checking out the new prints that they have and buying cards for friends. Also did I mention that the wi-fi is excellent? \nDate: September 3, 2018\nLocation: Seattle, Washington  \nimage1.png\n\n\n\nimage2.png\n\n\n\n",
      "metadata_storage_path": "aHR0cHM6Ly9zdG9yYWdlYWNjb3VudGRlY29sYS5ibG9iLmNvcmUud2luZG93cy5uZXQvY29mZmVlcmV2aWV3cy9yZXZpZXdzL3Jldmlldy0xLmRvY3g1",
      "locations": [
        "Seattle",
        "Washington"
      ],
      "keyphrases": [
        "coffee drinks",
        "favorite part",
        "local art",
        "many kinds",
        "new prints",
        "Review",
        "paintings",
        "watercolors",
        "cards",
        "friends",
        "wi-fi",
        "Date",
        "September",
        "Location",
        "Seattle",
        "Washington"
      ],
      "sentiment": "[\"positive\"]",
      "merged_content": "\n\nReview: I love the coffee drinks here, but my favorite part is the local art they sell. There are many kinds of paintings and watercolors they showcase each week. I love checking out the new prints that they have and buying cards for friends. Also did I mention that the wi-fi is excellent? \nDate: September 3, 2018\nLocation: Seattle, Washington  \nimage1.png\n  \n\n\nimage2.png\n  \n\n\n",
      "text": [
        "",
        ""
      ],
      "layoutText": [
        "{\"language\":\"en\",\"text\":\"\",\"lines\":[],\"words\":[]}",
        "{\"language\":\"en\",\"text\":\"\",\"lines\":[],\"words\":[]}"
      ],
      "imageTags": [
        "person",
        "human face",
        "clothing",
        "laptop",
        "table",
        "computer",
        "sitting",
        "outdoor",
        "indoor",
        "paint",
        "painting",
        "art",
        "child art",
        "indoor",
        "wall"
      ],
      "imageCaption": [
        "{\"tags\":[\"person\",\"laptop\",\"boy\"],\"captions\":[{\"text\":\"a person sitting at a table\",\"confidence\":0.4637661278247833}]}",
        "{\"tags\":[],\"captions\":[{\"text\":\"a wall with a painting on it\",\"confidence\":0.35804319381713867}]}"
      ]
    },
```
Uma observação importante é que não apenas foram extraídos e destacados os dados do texto, como locais, datas e outras palavras-chave, como também foi feita uma análise de sentimentos para considerar uma mensagem como positiva e também uma análise dos elementos das imagens anexadas, separadas em _tags_.

### Visão Geral
O Azure AI Search prova ser uma tecnologia extremamente útil e poderosa, com potencial para ser utilizado em várias áreas. Não apenas pode ser usada para extrair informações em massa, como resenhas de locais, como também podem ser aplicada em basicamente qualquer ambiente com um banco de dados, como listas de clientes, históricos de compras, e muitos outros serviços.

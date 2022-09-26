# Questão
Crie programa em Python que recebe por parâmetro o nome de um usuário e lista o nome de todos os repos que esse usuário possui no Github.
Para listar os repositorios de um usuário a seguinte URL pode ser chamada: https://api.github.com/users/{usuario}/repos.
Crie um novo arquivo com o username e grave todos os nomes dos repositórios no arquivo.

## Código
O código abaixo importa a biblioteca "requests" e "json". Por meio da primeira, me conectei à base de dados do Github através do comando GET, e raspei todos os repositórios associados a um respectivo nome buscado.
Os valores foram retornados dentro de um dicionário, o qual é inserido em uma lista na ordem como estão dispostos no perfil.
Na sequência, criei uma variável do tipo input para que o usuário possa buscar o participante. Todos os valores retornados na função get_project são gravados, por intermédio da biblioteca "json", automaticamente, em um arquivo json assim que o nome do participante é inserido, o qual é salvo com o nome do participante que o usuário inseriu no input.

``` python
"""Import libraries."""
import json
from typing import Dict, List, Union
import requests


def get_project(url: Dict[str, str]) -> List[Dict[str, Union[int, str]]]:
    """Get and store the projects names in a dict inside a list."""
    project_list: List[Dict[Union[int, str]]] = []

    for position, value in enumerate(url):
        project_number: int = position + 1
        project_name: str = (url[position]['name'])

        project_dict: Dict[str, Union[int, str]] = {
            'Project Number': project_number,
            'Project Name': project_name}
        project_list.append(project_dict)

    return project_list


username: str = input('Digite o nome de um usuário: ')
r: str = requests.get(f'https://api.github.com/users/{username}/repos')
response: Dict[str, str] = r.json()

data: List[Dict[str, Union[int, str]]] = get_project(response)

with open(f'{username}.json', 'w', encoding='utf8') as writer:
    json.dump(
            data,
            writer,
            ensure_ascii=False,
            indent=1)
```

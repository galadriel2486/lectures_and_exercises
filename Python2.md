# Questão
Faça o download da tabela "Recorded Crime Data at the Police Force Area Level", dentro do arquivo "rec-crime-pfa.csv".
Crie um script que leia o arquivo e contenha as seguintes funcionalidades:

- O script deverá abrir o arquivo porém nunca deverá carregar mais de uma linha em memória (usar generator);
- Você deverá fazer a conversão de cada linha para um dicionário (usar map);
- Apenas linhas com o ano maior que 2012 deverão ser consideradas (usar filter);
- Fazer a somatória do número de crimes (usar reduce do pacote functools).

## Código
``` python
"""Import libraries."""
from functools import reduce
from typing import Generator, Dict, List, Iterator

FILE_NAME = 'rec-crime-pfa.csv'


def read_line(
        filename: str) -> Generator[str, str, None]:
    """Read csv line by line."""
    with open(filename, 'r', encoding='utf8') as reader:
        for line in reader:
            yield line


csv_generator: Generator[str, str, None] = read_line(FILE_NAME)


def split_line(
        values: Generator) -> Iterator[List[str]]:
    """Convert each line in a list."""
    for item in values:
        values_list = item.rstrip().split(',')
        yield values_list


values_generator: Iterator[List[str]] = split_line(csv_generator)

# Select the first row as the table header.
columns: List[str] = next(values_generator)


def convert_to_dict(
        cols: List[str],
        data: Iterator[List[str]]) -> Iterator[Dict[str, str]]:
    """Convert each line list in a dictionary."""
    for value in data:
        values_dict: Dict[str, str] = dict(zip(cols, value))
        yield values_dict


dict_generator: Iterator[Dict[str, str]] = convert_to_dict(
    columns,
    values_generator)


def date_filter(
        extracted_data: Dict[str, str]) -> bool:
    """Filter the years above 2012."""
    select_date: list[str] = extracted_data['12 months ending'].split('/')
    select_year: int = int(select_date[2])
    return select_year > 2012


dict_filtered: Iterator[Dict[str, str]] = filter(date_filter, dict_generator)

# Sum the offence values.
offence_reduced: int = reduce(
    lambda a, b: a + int(b['Rolling year total number of offences']),
    dict_filtered, 0)

print(offence_reduced)
```

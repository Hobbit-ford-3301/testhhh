# testhhh
первый тест и для какого кода он написан
-----------------------------------------------
1_для:
-----------------------------------------------

import os
from datetime import datetime


def logger(oldfunction):
    def newfunction(*args, **kwargs):
        timestamp = datetime.now().strftime('%y-%m-%d %H:%M:%S')
        argsstr = ', '.join(map(repr, args))
        kwargsstr = ', '.join(f'{k}={v!r}' for k, v in kwargs.items())
        arguments = argsstr + ', ' + kwargsstr if argsstr and kwargsstr else argsstr or kwargsstr


    result = old_function(*args, **kwargs)

    with open('main.log', 'a') as log_file:
        log_file.write(f'{timestamp}: {old_function.__name__}({arguments}) -> {result}\\n')

    return result

return new_function

def test_1():


path = 'main.log'
if os.path.exists(path):
    os.remove(path)

@logger
def hello_world():
    return 'hello world'

@logger
def summator(a, b=0):
    return a + b

@logger
def div(a, b):
    return a / b

assert 'hello world' == hello_world(), "функция возвращает 'hello world'"
result = summator(2, 2)
assert isinstance(result, int), 'должно вернуться целое число'
assert result == 4, '2 + 2 = 4'
result = div(6, 2)
assert result == 3, '6 / 2 = 3'

assert os.path.exists(path), 'файл main.log должен существовать'

summator(4.3, b=2.2)
summator(a=0, b=0)

with open(path) as log_file:
    log_file_content = log_file.read()

assert 'summator' in log_file_content, 'должно записаться имя функции'
for item in (4.3, 2.2, 6.5):
    assert str(item) in log_file_content, f'{item} должен быть записан в файл'

if name == 'main':
    test_1()

    
-----------------------------------------------------------
1_получился test:
-----------------------------------------------------------


import os
import unittest
from datetime import datetime
from unittest.mock import patch

def logger(old_function):
    def new_function(*args, **kwargs):
        timestamp = datetime.now().strftime('%y-%m-%d %h:%m:%s')
        args_str = ', '.join(map(repr, args))
        kwargs_str = ', '.join(f'{k}={v!r}' for k, v in kwargs.items())
        arguments = args_str + ', ' + kwargs_str if args_str and kwargs_str else args_str or kwargs_str

        result = old_function(*args, **kwargs)

        with open('main.log', 'a') as log_file:
            log_file.write(f'{timestamp}: {old_function.__name__}({arguments}) -> {result}\\n')

        return result

    return new_function

class TestLogger(unittest.TestCase):

    def setUp(self):
        path = 'main.log'
        if os.path.exists(path):
            os.remove(path)

    @patch('builtins.open', side_effect=open)
    def test_logging(self, mock_open):
        @logger
        def hello_world():
            return 'hello world'

        hello_world()

        with open('main.log', 'r') as log_file:
            log_content = log_file.readlines()

        self.assertIn('hello_world() -> hello world\\n', log_content)

    @patch('builtins.open', side_effect=open)
    def test_functionality(self, mock_open):
        @logger
        def summator(a, b=0):
            return a + b

        result = summator(2, 2)
        self.assertEqual(result, 4)

        result = summator(6, 2)
        self.assertEqual(result, 8)  # this test should fail to demonstrate a failing test

if __name__ == '__main':
    unittest.main()


-----------------------------------------------
2_для:
-----------------------------------------------


class FlatIterator:


    def __init__(self, list_of_list):
        self.flat_list = [item for sublist in list_of_list for item in sublist]
        self.index = 0

    def __iter__(self):
        return self

    def __next__(self):
        if self.index < len(self.flat_list):
            item = self.flat_list[self.index]
            self.index += 1
            return item
        else:
            raise StopIteration

def test_1():
    list_of_lists_1 = [
        ['a', 'b', 'c'],
        ['d', 'e', 'f', 'h', False],
        [1, 2, None]
    ]


    for flat_iterator_item, check_item in zip(
        FlatIterator(list_of_lists_1),
        ['a', 'b', 'c', 'd', 'e', 'f', 'h', False, 1, 2, None]
    ):

        assert flat_iterator_item == check_item

    assert list(FlatIterator(list_of_lists_1)) == ['a', 'b', 'c', 'd', 'e', 'f', 'h', False, 1, 2, None]

if __name__ == '__main__':
    test_1()


-----------------------------------------------------------
2_получился test:
-----------------------------------------------------------

import unittest
from FlatIterator import FlatIterator

class TestFlatIterator(unittest.TestCase):
    def test_2(self):
        # Тест на пустой список списков
        list_of_lists_2 = []
        flat_iterator = FlatIterator(list_of_lists_2)
        with self.assertRaises(StopIteration):
            next(flat_iterator)

    def test_3(self):
        # Тест на список списков с пропусками элементов
        list_of_lists_3 = [['a', 'b', 'c'], [], ['d', 'e', 'f', 'h', False], [1, 2, None]]
        expected_result = ['a', 'b', 'c', None, 'd', 'e', 'f', 'h', False, 1, 2, None]
        flat_iterator = FlatIterator(list_of_lists_3)
        actual_result = []
        for item in flat_iterator:
            actual_result.append(item)
        self.assertEqual(expected_result, actual_result)

if __name__ == '__main__':
    unittest.main()

-----------------------------------------------
3_для:
-----------------------------------------------

import requests
from bs4 import BeautifulSoup
import json
from time import sleep
import os
import sys


os.environ['PYTHONIOENCODING'] = 'utf-8'


url = 'https://spb.hh.ru/search/vacancy?text=python&area=1&area=2'
keywords = ['Django', 'Flask']
headers = {
  'User-Agent': 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) Chrome/58.0.3029.110 ',
}
response = requests.get(url, headers = headers)


if response.statuscode == 200:
    html = response.text
    soup = BeautifulSoup(html, 'html.parser')
    vacancies = soup.findall("div", class="vacancy-card--zUXteNo7bRGzxWVcL7y font-inter")
    vacancyinfo = []  
    for vacancy in vacancies:
        link = vacancy.find('a', class='bloko-link').get('href')
        company = vacancy.find("span", class="company-info-text--vgvZouLtf8jwBmaD1xgp").text
        city = vacancy.find("span", class="fake-magritte-primary-text--Hdw8FvkOzzOcoR4xXWni").text
        if city:
            city = city.strip()
        else:
            city = "Не указан"


    salary = vacancy.find("span", class_="fake-magritte-primary-text--Hdw8FvkOzzOcoR4xXWni compensation-text--kTJ0_rp54B2vNeZ3CTt2 separate-line-on-xs--mtby5gO4J0ixtqzW38wh")
    if salary:
        salary = salary.text.strip()
    else:
        salary = "Не указана"

    vacancy_info.append( {
        'link': f"{link}",
        'company': company,
        'city': city,
        'salary': salary
    } )

with open('vacancyinfo.json', 'w', encoding='utf-8') as file:
    json.dump(vacancyinfo, file, ensure_ascii=False, indent=4)

-----------------------------------------------------------
3_получился test:
-----------------------------------------------------------

import requests
from bs4 import BeautifulSoup
import json
import os

def get_vacancy_info(url, headers):
    response = requests.get(url, headers=headers)

    if response.status_code == 200:
        html = response.text
        soup = BeautifulSoup(html, 'html.parser')
        vacancies = soup.find_all("div", class_="vacancy-card--z_uxteno7brgzxwvcl7y font-inter")
        vacancy_info = []  

        for vacancy in vacancies:
            link = vacancy.find('a', class_='bloko-link').get('href')
            company = vacancy.find("span", class_="company-info-text--vgvzoultf8jwbmad1xgp").text
            city = vacancy.find("span", class_="fake-magritte-primary-text--hdw8fvkozzocor4xxwni").text.strip() if vacancy.find("span", class_="fake-magritte-primary-text--hdw8fvkozzocor4xxwni") else "не указан"
            salary = vacancy.find("span", class_="fake-magritte-primary-text--hdw8fvkozzocor4xxwni compensation-text--ktj0_rp54b2vnez3ctt2 separate-line-on-xs--mtby5go4j0ixtqzw38wh").text.strip() if vacancy.find("span", class_="fake-magritte-primary-text--hdw8fvkozzocor4xxwni compensation-text--ktj0_rp54b2vnez3ctt2 separate-line-on-xs--mtby5go4j0ixtqzw38wh") else "не указана"

            vacancy_info.append({
                'link': link,
                'company': company,
                'city': city,
                'salary': salary
            })

        return vacancy_info
    else:
        return None

url = 'https://spb.hh.ru/search/vacancy?text=python&area=1&area=2'
headers = {
  'user-agent': 'mozilla/5.0 (windows nt 10.0; win64; x64) chrome/58.0.3029.110 ',
}

vacancy_info = get_vacancy_info(url, headers)

if vacancy_info:
    with open('vacancy_info.json', 'w', encoding='utf-8') as file:
        json.dump(vacancy_info, file, ensure_ascii=False, indent=4)



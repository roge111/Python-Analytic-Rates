# Приветствие 👋
Привествую. здесь собрано решения тествого задания от компании "Достаевский". 
# Цель
Запрасить данные о ключевой ставке с сайта Центра Банка. И проанализировать зависимость продаж от ключевой ставки
# 📃 Алгоритм
  - Сбор данных о продажах, как написано в задаче
  - Парсинг ключевой ставки с сайта ЦБ
  - Группировка по максимальномой ключевой ставке на месяц
  - Сливаем таблицы ставок и продаж
  - Формируем визуальные графики для анализа зависимости
# 🧑‍💻Решение 

```
def parse_date(date):
  date = date.split('-')[::-1]
  return '.'.join(date)

date_end = (pd.to_datetime(dates[-1]) + pd.DateOffset(months=1)).strftime('%d.%m.%Y')

date_start = parse_date(dates[0])




url = f'https://www.cbr.ru/hd_base/keyrate/?UniDbQuery.Posted=True&UniDbQuery.From={date_start}&UniDbQuery.To={date_end}'

response = requests.get(url)

soup = BeautifulSoup(response.text, 'html.parser')
dates_table = soup.find('table', class_='data')

data_rates = {}

rates_date = dates_table.find_all('tr')
for index, rate_date in enumerate(rates_date):
  if index == 0:
    continue
  _, date, rate, _ = rate_date.text.split('\n')
  rate = float(rate.replace(',', '.'))
  date = date.split('.')
  if 'Day' not in data_rates:
    data_rates['Day'], data_rates['Month'], data_rates['Year'], data_rates['Rate'] = [date[0]], [date[1]], [date[2]], [rate]
  else:
    data_rates['Day'].append(date[0])
    data_rates['Month'].append(date[1])
    data_rates['Year'].append(date[2])
    data_rates['Rate'].append(rate)
df_rates = pd.DataFrame(data_rates)
```


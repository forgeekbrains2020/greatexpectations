# greatexpectations
For homework greatexpectations

==================================================================================================================================
ayashin.ods_payment
==================================================================================================================================
Table Expectation(s)
1.
batch_kwargs = {'data_asset_name': 'ayashin.ods_payment', 'datasource': 'gp_my', 'limit': 10000, 'schema': 'ayashin', 'table': 'ods_payment'}
изменен лимит до 10000, без этого не работали некоторые проверки (2)
'limit': 1000 -> 'limit': 10000

2. 
batch.expect_table_row_count_to_be_between(max_value=10001, min_value=9000)
предоложим, что нам было точно известно, что количество записей по платежам должно быть не более 10000 и не менее 9000

3. 
batch.expect_table_column_count_to_equal(value=9)
value=8 -> value=9
изменил количество столбцов, т.к.  во время формирования записей таблицы  добавляю поле load_date

4.
batch.expect_table_columns_to_match_ordered_list(column_list=['user_id', 'pay_doc_type', 'pay_doc_num', 'account', 'phone', 'billing_period', 'pay_date', 'sum', 'load_date'])
column_list=['user_id', 'pay_doc_type', 'pay_doc_num', 'account', 'phone', 'billing_period', 'pay_date', 'sum'] -> column_list=['user_id', 'pay_doc_type', 'pay_doc_num', 'account', 'phone', 'billing_period', 'pay_date', 'sum', 'load_date']
изменена проверка, добавлено поле load_date

Column Expectation(s)

5. pay_doc_type 
batch.expect_column_values_to_be_of_type(column='pay_doc_type',type_='TEXT')
выполним проверку, чтобы в поле типов платежных систем у нас содержались текстовые данные

6. pay_doc_num
batch.expect_column_values_to_be_of_type(column='pay_doc_num',type_='INTEGER')
выполним проверку, чтобы в поле были только числовые данные

7. pay_doc_num
batch.expect_column_values_to_be_unique(column='pay_doc_num')
добавим эту проверку, чтобы в случае наличия неуникальности значений выполнить, например, дополнительную обработку таких платежей

8. account
batch.expect_column_values_to_match_regex(column='account', regex='FL-\d+')
проверим, чтобы значения поля начинались с FL- а в позиции дальше 1, или более чисел.
9. phone
batch.expect_column_values_to_match_regex(column='phone', regex='\+79\d\d\d\d\d\d\d\d\d')
проверим, чтобы номера телефоно начинались с +79 и дальше еще 9 цифр

10. billing_period
batch.expect_column_values_to_be_between(column='billing_period', max_value='2021-03-01 00:00:00', min_value='2013-01-01 00:00:00')
должны попадать только данные старше 2012 года, но младше 2 марта 2021 года

11. sum
batch.expect_column_values_to_be_of_type(column='sum',type_='NUMERIC')
значения этого поля должны быть NUMERIC

12. sum
batch.expect_column_min_to_be_between(column='pay_doc_num', max_value=10, min_value=1)
Допустим известно, что минимальный платеж не может быть меньше 1 рубля, также известно что если мин платеж не входит в  диапозон от 1 до 10, это странно. Сделали соотв. проверку.

13. sum
batch.expect_column_values_to_not_be_null(column='sum')
Поле суммы не должно быть пустым.
==================================================================================================================================




==================================================================================================================================
ayashin.ods_billing
==================================================================================================================================
Table Expectation(s)
1. batch.expect_table_row_count_to_be_between(max_value=10001, min_value=9000)
предоложим, что нам было точно известно, что количество записей по биллингу должно быть не более 10000 и не менее 9000
2. batch.expect_table_columns_to_match_ordered_list(column_list=['user_id', 'billing_period', 'service', 'tariff', 'sum', 'created_at', 'load_date'])
проверяем, чтобы были все поля в нужном порядке

Column Expectation(s)

3. user_id
batch.expect_column_values_to_not_be_null(column='user_id')
в поле не должно быть пустых значений.
4. billing_period
batch.expect_column_values_to_be_between(column='billing_period', max_value='2021-03-01 00:00:00', min_value='2013-01-01 00:00:00')
должны попадать только данные старше 2012 года, но младше 2 марта 2021 года
5. service
batch.expect_column_values_to_not_be_null(column='service')
не должно быть пустых значений
6. service
batch.expect_column_value_lengths_to_equal(column='service', value=14)
лс может быть длинной 14 символов, проверяем это.
7. service
batch.expect_column_values_to_be_unique(column='service')
значения поля должны быть уникальны
8. tariff
batch.expect_column_values_to_not_be_null(column='tariff')
название тарифа дожно присутствовать в каждой строке
9. tariff
batch.expect_column_distinct_values_to_be_in_set(column='tariff', value_set=['Gigabyte', 'Maxi', 'Megabyte', 'Mini'])
У нас всего 4 тарифа, проверяем, чтобы не было лишнего
10. sum
batch.expect_column_values_to_not_be_null(column='sum')
не должно быть пустых значений
11. sum
batch.expect_column_max_to_be_between(column='sum', min_value=300 , max_value=10000 )
не может быть максимальное значение по всей таблице данных на несколько тысяч строк быть меньше 300 и больше 15000
12. load_date
batch.expect_column_values_to_be_between(column='load_date', max_value='2021-04-27 00:00:00', min_value='2021-04-19 00:00:00')
загрузка данных должна было проходить с 19.04.21 по 26.04.21. проверим это
13. created_at
batch.expect_column_values_to_be_between(column='created_at', max_value='2021-03-01 00:00:00', min_value='2013-01-01 00:00:00')
дата создания должна быть в этих границах.
==================================================================================================================================


==================================================================================================================================
ayashin.ods_issue
==================================================================================================================================
Table Expectation(s)
1. batch.expect_table_row_count_to_be_between(max_value=10001, min_value=9000)
предоложим, что нам было точно известно, что количество записей по биллингу должно быть не более 10000 и не менее 9000
2. batch.expect_table_column_count_to_equal(value=7)
проверяем, чтобы было 7 полей в таблице


Column Expectation(s)
3. user_id
batch.expect_column_unique_value_count_to_be_between(column='user_id',min_value=100, max_value=9774)
число уникальных значений не должно быть меньше 100 и больше записей в источнике
4. user_id
batch.expect_column_values_to_not_be_null(column='user_id')
в поле не должно быть пустых значений.
5. start_time
batch.expect_column_values_to_not_be_null(column='start_time')
время начала обращения обязательно должно быть представлено.
6. start_time
batch.expect_column_values_to_be_of_type(column='start_time',type_='TIMESTAMP')
значения должны быть TIMESTAMP
7. start_time
batch.expect_column_values_to_be_between(column='start_time', max_value='2021-05-01 23:59:31', min_value='2012-01-02 22:22:31', parse_strings_as_datetimes=True)
знаем что у нас обращения с 2013 года, по настоящее время. Проверим это.
8. end_time
batch.expect_column_values_to_be_of_type(column='end_time',type_='TIMESTAMP')
значения должны быть TIMESTAMP
9. end_time
batch.expect_column_min_to_be_between(column='end_time',min_value='2012-01-02 22:22:31' )
знаем что у нас обращения с 2013 года. Проверим это.
10. title
batch.expect_column_values_to_not_be_null(column='title')
не должно быть пустых значений в теме
11. description
batch.expect_column_value_lengths_to_be_between(column='description', min_value=5)
описание проблемы не должно быть менее 5 символо
12 service
batch.expect_column_values_to_not_be_null(column='service')
значения не должны быть пустыми
==================================================================================================================================

==================================================================================================================================
ayashin.ods_traffic
==================================================================================================================================
Table Expectation(s)
1. batch.expect_table_row_count_to_be_between(max_value=10001, min_value=9000)
предоложим, что нам было точно известно, что количество записей по биллингу должно быть не более 10000 и не менее 9000
2. batch.expect_table_column_count_to_equal(value=7)
проверяем, чтобы было 7 полей в таблице
3. batch.expect_table_columns_to_match_ordered_list(column_list=['user_id', 'timestamp', 'device_id', 'device_ip_addr', 'bytes_sent', 'bytes_received', 'load_date'])
проверяем, чтобы поля в таблицы были в заданном порядке

Column Expectation(s)
4. user_id
batch.expect_column_values_to_not_be_null(column='user_id')
в поле не должно быть пустых значений.
5.user_id
batch.expect_column_proportion_of_unique_values_to_be_between(column='user_id',min_value=0.0125, max_value=1)
зададим, что мин. соотношение уникальных значений  не должно быть меньше 0.0125
6. timestamp
batch.expect_column_values_to_not_be_null(column='timestamp')
не должно быть пустых значений
7. timestamp
batch.expect_column_values_to_be_between(column='timestamp',  min_value='2012-12-31 23:59:31', parse_strings_as_datetimes=True)
должны содержаться даты начиная с 2013 года
8. device_id
batch.expect_column_unique_value_count_to_be_between(column='device_id',min_value=100, max_value=9774)
в наборе из 9774 записей не должно быть менее 100 уникальных значений
9. device_id 
batch.expect_column_values_to_match_regex(column='device_id', regex='\D\d{1,10}')
предположим, что формат значений должен быть вида - сначала нечисловой символ и далее от 1-й до 10-и цифр
10. device_ip_addr
batch.expect_column_values_to_match_regex(column='device_ip_addr', regex='((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)')
проверим регулярным выражением наличие значения в формате ipv4 ip адреса
11. bytes_sent
batch.expect_column_values_to_not_be_null(column='bytes_sent')
не должно быть пустых значений
12. bytes_sent
batch.expect_column_mean_to_be_between(column='bytes_sent', max_value=29000, min_value=20000)
положим известно, границы по средним значения отправленных байт за сессию. проверим, удовлетворяют ли записи набора этому условию
13. bytes_received
batch.expect_column_median_to_be_between(column='bytes_received', max_value=27000, min_value=21000)
медиана набора должна находится в таких границах
14. load_date
batch.expect_column_values_to_be_between(column='load_date', max_value='2021-04-27 00:00:00', min_value='2021-04-19 00:00:00')
загрузка данных должна было проходить с 19.04.21 по 26.04.21. проверим это

==================================================================================================================================

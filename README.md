import json
from collections import defaultdict

# Загрузить данные из файла JSON
file_path = "C:\\Users\\vladn\\OneDrive\\Desktop\\formatted-json.json"
with open(file_path, 'r', encoding='utf-8') as file:
    orders = json.load(file)

# 1. Номер самого дорогого заказа
max_price_order_number = max(orders, key=lambda x: orders[x]['price'])

# 2. Номер заказа с самым большим количеством товаров
max_quantity_order_number = max(orders, key=lambda x: orders[x]['quantity'])

# 3. Определение дня с наибольшим числом заказов
orders_by_day = defaultdict(int)
for order in orders.values():
    order_date = order['date']
    day = order_date.split(' ')[0]  # Берем только дату без времени
    orders_by_day[day] += 1

max_orders_day = max(orders_by_day, key=orders_by_day.get)

# 4. Пользователь, сделавший наибольшее количество заказов
orders_by_user = defaultdict(int)
for order in orders.values():
    user_id = order['user_id']
    orders_by_user[user_id] += 1

most_orders_user = max(orders_by_user, key=orders_by_user.get)

# 5. Пользователь с самой большой суммарной стоимостью заказов
total_cost_by_user = defaultdict(float)
for order in orders.values():
    user_id = order['user_id']
    total_cost_by_user[user_id] += order['price']

highest_total_cost_user = max(total_cost_by_user, key=total_cost_by_user.get)

# 6. Средняя стоимость заказа
average_order_price = sum(order['price'] for order in orders.values()) / len(orders)

# 7. Средняя стоимость товаров в заказе
total_quantity = sum(order['quantity'] for order in orders.values())
average_product_price = sum(order['price'] for order in orders.values()) / total_quantity if total_quantity > 0 else 0

# Вывод результатов
print(f'Номер самого дорогого заказа: {max_price_order_number}')
print(f'Номер заказа с самым большим количеством товаров: {max_quantity_order_number}')
print(f'День с наибольшим числом заказов: {max_orders_day}')
print(f'Пользователь с наибольшим количеством заказов: {most_orders_user}')
print(f'Пользователь с самой большой суммарной стоимостью заказов: {highest_total_cost_user}')
print(f'Средняя стоимость заказа: {average_order_price:.2f}')
print(f'Средняя стоимость товаров в заказе: {average_product_price:.2f}')

# calculator-analysis--_-
# Calculator Analysis Project

Проект калькулятора с функциями:
- Сложение, вычитание, умножение, деление
- Проверка деления на ноль
- Логирование операций
- Обработка исключений

## Использование
```bash
python main.py

3. **Подключите репозиторий в Codacy:**
   - Войдите в Codacy через GitHub
   - Добавьте репозиторий для анализа
   - Дождитесь завершения анализа

## Часть 3: Сравнительный анализ

### Сравнительная таблица

| **Параметр сравнения** | **Replit** | **Codacy** | **Выводы** |
|------------------------|------------|------------|------------|
| **Основное назначение** | Облачная среда разработки | Статический анализ качества кода | Replit - для написания кода, Codacy - для контроля качества |
| **Требования к установке** | Только браузер | Только браузер | Оба инструмента не требуют установки ПО |
| **Интеграция с GitHub** | Есть (импорт/экспорт) | Прямая интеграция | Codacy имеет более глубокую интеграцию |
| **Анализ качества кода** | Базовая проверка синтаксиса | Глубокий статический анализ | Codacy предоставляет детальную аналитику |
| **Возможности для командной работы** | Совместное редактирование | Общие отчеты качества | Оба поддерживают командную работу |
| **Сильные стороны** | Простота использования, встроенные инструменты | Детальный анализ, интеграция с CI/CD | Инструменты дополняют друг друга |
| **Слабые стороны** | Ограниченные возможности отладки | Только анализ, нет редактирования | Лучше использовать вместе |

## Пример улучшенного кода после анализа Codacy

```python
"""
Модуль калькулятора с арифметическими операциями и логированием.
"""

import logging
from datetime import datetime
from typing import Union, List


class Calculator:
    """Класс калькулятора с базовыми арифметическими операциями."""
    
    def __init__(self) -> None:
        """Инициализация калькулятора с пустой историей операций."""
        self.history: List[str] = []
    
    def add(self, a: float, b: float) -> float:
        """
        Сложение двух чисел.
        
        Args:
            a: Первое число
            b: Второе число
            
        Returns:
            Результат сложения
        """
        result = a + b
        self._log_operation(f"{a} + {b} = {result}")
        return result
    
    def subtract(self, a: float, b: float) -> float:
        """
        Вычитание двух чисел.
        
        Args:
            a: Первое число
            b: Второе число
            
        Returns:
            Результат вычитания
        """
        result = a - b
        self._log_operation(f"{a} - {b} = {result}")
        return result
    
    def multiply(self, a: float, b: float) -> float:
        """
        Умножение двух чисел.
        
        Args:
            a: Первое число
            b: Второе число
            
        Returns:
            Результат умножения
        """
        result = a * b
        self._log_operation(f"{a} * {b} = {result}")
        return result
    
    def divide(self, a: float, b: float) -> float:
        """
        Деление двух чисел.
        
        Args:
            a: Делимое
            b: Делитель
            
        Returns:
            Результат деления
            
        Raises:
            ZeroDivisionError: Если делитель равен нулю
        """
        if b == 0:
            error_msg = "Деление на ноль невозможно"
            self._log_operation(f"ОШИБКА: {a} / {b} - {error_msg}")
            raise ZeroDivisionError(error_msg)
        
        result = a / b
        self._log_operation(f"{a} / {b} = {result}")
        return result
    
    def _log_operation(self, operation: str) -> None:
        """
        Логирование операции в файл и историю.
        
        Args:
            operation: Строка с описанием операции
        """
        logging.info(operation)
        timestamp = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        self.history.append(f"{timestamp} - {operation}")
    
    def show_history(self) -> None:
        """Вывод истории операций на экран."""
        print("\nИстория операций:")
        for operation in self.history:
            print(operation)


def parse_input(user_input: str) -> Union[tuple, None]:
    """
    Парсинг пользовательского ввода.
    
    Args:
        user_input: Введенная пользователем строка
        
    Returns:
        Кортеж (a, оператор, b) или None при ошибке
    """
    parts = user_input.split()
    if len(parts) != 3:
        print("Ошибка: введите операцию в формате 'число оператор число'")
        return None
    
    a_str, operator, b_str = parts
    
    try:
        a = float(a_str)
        b = float(b_str)
    except ValueError:
        print("Ошибка: введите корректные числа")
        return None
    
    return a, operator, b


def main() -> None:
    """Основная функция для работы с калькулятором."""
    # Настройка логирования
    logging.basicConfig(
        level=logging.INFO,
        format='%(asctime)s - %(levelname)s - %(message)s',
        handlers=[
            logging.FileHandler('calculator.log', encoding='utf-8'),
            logging.StreamHandler()
        ]
    )
    
    calc = Calculator()
    
    print("Калькулятор")
    print("Доступные операции: +, -, *, /")
    print("Команды: 'history' - показать историю, 'exit' - выход")
    
    while True:
        try:
            user_input = input("\nВведите операцию (например: 5 + 3): ").strip()
            
            if user_input.lower() == 'exit':
                print("Выход из программы")
                calc.show_history()
                break
            
            if user_input.lower() == 'history':
                calc.show_history()
                continue
            
            parsed = parse_input(user_input)
            if parsed is None:
                continue
            
            a, operator, b = parsed
            
            # Выполнение операции
            if operator == '+':
                result = calc.add(a, b)
            elif operator == '-':
                result = calc.subtract(a, b)
            elif operator == '*':
                result = calc.multiply(a, b)
            elif operator == '/':
                result = calc.divide(a, b)
            else:
                print("Ошибка: неизвестная операция. Используйте +, -, *, /")
                continue
            
            print(f"Результат: {result}")
            
        except KeyboardInterrupt:
            print("\nПрограмма прервана пользователем")
            calc.show_history()
            break
        except ZeroDivisionError as e:
            print(f"Ошибка: {e}")
        except Exception as e:
            print(f"Произошла непредвиденная ошибка: {e}")


if __name__ == "__main__":
    main()

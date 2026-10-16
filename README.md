# README.md
Лабораторная работа №3 — модуль **MenuModule** проекта «FoodCampus».

**User Story:** Как повар, я хочу фильтровать блюда по категориям, чтобы выдавать актуальное меню.
**Интерфейс:** `get_filtered_menu(category, only_available)`

## Файлы

| Файл | Назначение |
| --- | --- |
| `menu.py
` | Модуль `MenuModule` (итоговая версия с валидацией) |
| `test_menu.py
` | Test Driver: 6 позитивных и 14 негативных тестов |
| `menu_naive.py
` | Первая версия без валидации — только для воспроизведения багов |
| `demo_bugs.py
` | Запускает наивную версию и печатает фактические результаты для Bug Report |
| `report_lab3.md
` | Отчёт: архитектура, интерфейс, тесты, Bug Report, канбан |

## Запуск

```bash
python test_menu.py
   # ожидается: 20 PASS, 0 FAIL
python demo_bugs.py
   # воспроизведение багов на наивной версии
```

Требуется Python 3.8+, внешние библиотеки не нужны.

## Пример

```python
from menu import MenuModule

m = MenuModule()
m.get_filtered_menu("суп", only_available=True)   # [{'id': 1, 'name': 'Борщ', ...}]
m.get_filtered_menu("пицца")                      # MenuError: неизвестная категория
```
10:40
"""MenuModule — фильтрация блюд по категориям с валидацией входных данных."""

from typing import List, Dict, Any


class MenuError(Exception):
    """Доменная ошибка модуля меню."""
    pass


class MenuModule:
    """Модуль меню столовой FoodCampus.

   Хранит список блюд и умеет возвращать отфильтрованное меню
    по категории и признаку доступности.
    """

   CATEGORIES = ("суп", "горячее", "салат", "десерт", "напиток")

   def __init__(self) -> None:
        self._dishes: List[Dict[str, Any]] = [
            {"id": 1, "name": "Борщ",         "category": "суп",     "price": 120, "available": True},
            {"id": 2, "name": "Крем-суп",     "category": "суп",     "price": 150, "available": False},
            {"id": 3, "name": "Котлета",      "category": "горячее", "price": 200, "available": True},
            {"id": 4, "name": "Плов",         "category": "горячее", "price": 220, "available": True},
            {"id": 5, "name": "Цезарь",       "category": "салат",   "price": 180, "available": True},
            {"id": 6, "name": "Оливье",       "category": "салат",   "price": 160, "available": False},
            {"id": 7, "name": "Чизкейк",      "category": "десерт",  "price": 190, "available": True},
            {"id": 8, "name": "Компот",       "category": "напиток", "price":  60, "available": True},
        ]

   ---------- публичный интерфейс ----------

   def get_filtered_menu(self, category, only_available=False) -> List[Dict[str, Any]]:
        """Вернуть блюда выбранной категории.

   :param category: название категории (строка из CATEGORIES)
    :param only_available: если True — оставить только доступные блюда
        :raises MenuError: если категория невалидна или only_available не bool
        """
        self._validate_category(category)
        self._validate_only_available(only_available)

   result = [
            dish for dish in self._dishes
            if dish["category"] == category
            and (dish["available"] or not only_available)
        ]
        return result

def get_categories(self) -> List[str]:
        """Список поддерживаемых категорий."""
        return list(self.CATEGORIES)

   ---------- приватные валидаторы ----------

   @classmethod
    def _validate_category(cls, category) -> None:
        if not isinstance(category, str):
            raise MenuError("Категория должна быть строкой")
        if category.strip() == "":
            raise MenuError("Категория не может быть пустой")
        if category not in cls.CATEGORIES:
            raise MenuError(f"Неизвестная категория: {category!r}")

   @staticmethod
    def _validate_only_available(only_available) -> None:
        if not isinstance(only_available, bool):
            raise MenuError("only_available должен быть bool")




# Тестовое задание

## Формулировка

**Реализовать счетчик использования предлагаемых промптов. Реализовать метод API для отдачи значения счетчика.**

## Список изменений

### open_webui/config.py

- Добавлена переменная окружения **used_prompt_suggestions**, хранящая список использованных предложенных промптов с количеством его использования.

- Переменна окружения **used_prompt_suggestions** обернута в ранее существовавший класс PersistentConfig для удобной записи данных в базу данных.

### open_webui/main.py

- Добавлен объект конфигурации **USED_PROMPT_SUGGESTIONS** для доступа к ранее добавленной переменной окружения.

### open_webui/utils/tools.py

- Добавлен метод **find_prompt_suggestion** для поиска использованного промпта в списке использованных предложенных промптов. Метод возвращает индекс найденного элемента, иначе -1. Параметры метода:
  - **prompts: list[dict]** - список использованных предложенных промптов. Каждый элемент списка - объект класса **UsedPromptSuggestion**, представленный в виде словаря.
  - **prompt_suggestion: dict** - предложенный промпт, который необходимо найти в списке **prompts**. **prompt_suggestion** - объект класса PromptSuggestion, представленный в виде словаря.

### open_webui/routers/configs.py

- Добавлен эндпоинт **/increment_used_suggestion**, являющийся POST-методом, для инкремента счетчика использования предложенного промпта. В тело запроса необходимо передать объект класса PromptSuggestion в JSON формате. Эндпоинт возвращает объект класса **UsedPromptSuggestion** в JSON формате с обновленным счетчиком использования промпта.

  **Пример использования**

  **URI:**

    ```URI
    http://localhost:8080/api/v1/configs/increment_used_suggestion
    ```

  **Тело запроса:**

  ```JSON
  {
    "title": ["Test prompt suggestion"],
    "content": "This is content of test prompt suggestion"
  }
  ```

  **Ответ запроса:**

  ```JSON
  {
    "prompt_suggestion": {
      "title": ["Test prompt suggestion"],
      "content": "This is content of test prompt suggestion"
    },
    "count_of_uses": 1
  }
  ```

- Добавлен эндпоинт **.used_suggestions**, являющийся GET-методом, для получения списка использованных предложенных промптов с количеством их использования. Эндпоинт возвращает список объектов класса **UsedPromptSuggestion** в JSON формате.

  **Пример использования**

  **URI:**

  ```URI
  http://localhost:8080/api/v1/configs/used_suggestions
  ```

  **Ответ запроса:**

  ```JSON
  [
    {
      "prompt_suggestion": {
        "title": ["Test prompt suggestion"],
        "content": "This is content of test prompt suggestion"
      },
      "count_of_uses": 1
    }
  ]
  ```

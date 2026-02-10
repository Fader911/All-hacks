# Используем базовый образ с поддержкой GPU
FROM huggingface/transformers-pytorch-gpu:latest

# Устанавливаем рабочую директорию
WORKDIR /app

# Копируем необходимые файлы
COPY ./models ./models
COPY ./train_data.csv ./train_data.csv
COPY ./requirements.txt ./requirements.txt

# Устанавливаем зависимости
RUN pip install --no-cache-dir -r requirements.txt

# Копируем скрипт запуска
COPY run.py .

# Устанавливаем переменные окружения
ENV INPUT_PATH=/app/input
ENV OUTPUT_PATH=/app/output

# Создаем директории для входных и выходных данных
RUN mkdir -p ${INPUT_PATH} ${OUTPUT_PATH}

# Запускаем приложение
CMD ["python3", "/app/run.py", "--input-path", "${INPUT_PATH}", "--output-path", "${OUTPUT_PATH}"] 